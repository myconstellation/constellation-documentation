---
ID: 1536
post_title: 'Persistance de donn&eacute;es dans un package'
author: Sebastien Warin
post_date: 2016-03-21 16:39:53
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/persistance-de-donnes-dans-un-package/
published: true
post_modified: 2017-04-20 09:56:50
---
<p>Certain de vos packages peuvent avoir besoin d’enregistrer/persister des données.</p> <p>Vous devez savoir à chaque mise qu’à jour du package par sa sentinelle, l’ensemble du package est supprimé avant d’être redéployer.</p> <p>Il existe deux possibilités pour persister les données :</p> <ul> <li>Utiliser un stockage “hors Constellation”  <li>Utiliser les StateObjects</li></ul> <h3>Stocker en dehors de Constellation</h3> <p>Comme vous le savez, un package n’est ni plus ni moins qu’une application.</p> <p>Vous pouvez donc utiliser n’importe quel système de stockage :</p> <ul> <li>File System  <li>Base de donnée en tout genre  <li>Web service  <li>Etc …</li></ul> <p>Libre à vous de stoker vos données où bon vous sembles mais attention si vous souhaitez diffuser votre package, vous ajoutez des prérequis pour l’installation de votre package.</p> <h4>Note sur le File System</h4> <p>Vous pouvez bien sûr stoker des données dans des fichiers sur la sentinelle.</p> <p>Par défaut le “CurrentDirectory” du package est son répertoire de déploiement. Prennez garde, car à chaque déploiement du package par la sentinelle ce répertoire est vidé.</p> <p>Vous devez donc utiliser un répertoire “externe” qu’on spécifiera par un <a href="/client-api/net-package-api/settings/">setting</a>.</p> <h4>SerializationHelper</h4> <p>Notez que la librairie Constellation met à disposition la classe “SerializationHelper” dans le namespace “Constellation.Utils” permettant de sérialiser et dé-sérialiser facilement des objets en JSON ou XML.</p><pre class="lang:C# decode:true">Constellation.Utils.SerializationHelper.SerializeToFile&lt;MyCustomObject&gt;(new MyCustomObject() { Number = 42, String = "Demo" }, PackageHost.GetSettingValue("MyData.json"));
MyCustomObject myData = Constellation.Utils.SerializationHelper.DeserializeFromFile&lt;MyCustomObject&gt;(PackageHost.GetSettingValue("MyData.json"));</pre>
<p>Vous disposez des méthodes :</p>
<ul>
<li>SerializeToFile et DeserializeFromFile pour sérialiser/dé-sérialiser vers/depuis un fichier 
<li>SerializeToString et DeserializeFromString pour sérialiser/dé-sérialiser vers/depuis un string</li></ul>
<p>Par défaut c’est le format JSON qui est utilisé mais vous pouvez définir le <em>DataContractSerialiser</em> (format XML) dans les paramètres de la méthode.</p>
<h3>Utiliser les StateObjects comme stockage</h3>
<p>Le principe est d’utiliser les StateObjects comme stockage, très utilise pour sauvegarder/restaurer l’état d’un package.</p>
<p>Chaque package peut <a href="/client-api/net-package-api/stateobjects/">publier des StateObjects</a> que ce soit de simples valeurs ou des véritables objets complexes. A chaque (re)démarrage de votre package, tous les StateObjects du package sont purgés de la Constellation.</p>
<p>Cependant vous avez la possibilité de les récupérer dans votre code avant cette purge.</p>
<p>Pour cela vous devez ajouter dans le manifeste du package (PackageInfo.xml) l’attribut “RequestLastStateObjectsOnStart” à true.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-150.png"><img title="RequestLastStateObjectsOnStart" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="RequestLastStateObjectsOnStart" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-127.png" width="424" height="202"></a></p>
<p align="left">Ensuite dans votre code, vous devez le plus tôt possible dans le “OnStart” vous abonnez à l’évènement “LastStateObjectsReceived“.</p>
<p align="left">Cet évènement sera levé lorsque votre package recevra ses anciens StateObjects sur le serveur avant d’être purgés. L’argument passé dans l’évènement contient une liste de StateObject.</p><pre class="lang:C# decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
{
    PackageHost.WriteInfo("Reception de mes anciens StateObjects au nombre de {0}", e.StateObjects.Count);
};</pre>
Vous pouvez “bêtement” re-pusher l’ensemble des StateObjects sans aucune modification :
<pre class="lang:C# decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
{
    foreach (StateObject so in e.StateObjects)
    {
        // Re-Pusher “bêtement” les StateObjects
        PackageHost.PushStateObject(so.Name, so.DynamicValue, so.Type, so.Metadatas, so.Lifetime);
    }
};</pre>
<p>Grace à ce mécanisme, vos packages peuvent pusher des StateObject sur “leur état”. A chaque (re) démarrage (si à une mise à jour ou simple rédémarrage) retrouver son ancien état.</p>
<p>Cela est fortement utile dans le package type “Brain”. Par exemple, imaginez un package “Brain” qui pilote l’allumage automatique des éclairages d’un jardin.</p>
<p>Ce package déclare un StateObjectLink vers un StateObject qui indique la luminosité extérieure. Si la valeur franchie un certain seuil, le package envoie un message pour déclencher à un MessageCallback d’un package pilotant les éclairages.</p>
<p>Imaginez maintenant que suite à l’allumage automatique des éclairages par votre package vous décidez de les éteindre manuellement. Puis, quelques instants plus tard, pour diverses raisons vous devez redémarrer (ou mettre à jour) votre package “Brain”. Vos éclairages risquent de se rallumer car votre “Brain” n’a pas mémoriser qu’il a déjà réalisé cette action !</p>
<p>Pour ce genre de problématique, il est intéressant de stocker ces variables dans une classe d’état et de publier cette classe dans un StateObject qu’on pourra nommer “State”. Lorsque notre package redémarre, on demandera à Constellation de nous renvoyer les derniers StateObjects.</p>
<p>Cela nous permettra de récupérer le dernier état de notre package avant son redémarrage.</p>
<p>Par exemple, vous pouvez définir une classe d’état de la façon suivante :</p><pre class="lang:C# decode:true">public class CurrentState
{
    public DateTime OuvertureVolet { get; set; }

    public DateTime FemertureVolet { get; set; }

    public DateTime OuvertureLumiereJardin { get; set; } 

    // etc....

    public const string STATEOBJECT_NAME = "CurrentState";

    private static CurrentState current;

    public static CurrentState Current
    {
        get { return current; }
    }

    public static void Load(StateObject stateObject = null)
    {
        if (stateObject != null)
        {
            PackageHost.WriteInfo("Restoring state from {0}", stateObject.LastUpdate);
            current = stateObject.GetValue&lt;CurrentState&gt;();
        }
        else
        {
            PackageHost.WriteInfo("Creating new state");
            current = new CurrentState();
        }
        current.Save();
    }

    public void Save()
    {
        PackageHost.WriteInfo("Saving current state");
        PackageHost.PushStateObject(STATEOBJECT_NAME, current);
    }
}</pre>
<p>Sans oublier d’ajouter un handler pour la restauration de l’état précèdent :</p><pre class="lang:C# decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
{
    if (e.StateObjects != null)
    {
        PackageHost.WriteInfo("Received old StateObjects (Count:{0})", e.StateObjects.Count);
        CurrentState.Load(e.StateObjects.FirstOrDefault(so =&gt; so.Name == CurrentState.STATEOBJECT_NAME));
    }
};
</pre>
<p>De cette façon vous pouvez à tout moment accéder à votre variable via votre classe statique “State.Current” et dès que vous changez une valeur, invoquez la méthode Save(), pour re-pusher votre “State” comme StateObject de votre package :</p><pre class="lang:C# decode:true">if (DateTime.Now.Date != CurrentState.Current.FemertureVoletSalon.Date &amp;&amp; ** Luminosite &lt; seuil ****)
{
   // Fermer le volet ici !!
   CurrentState.Current.FemertureVoletSalon = DateTime.Now;
   CurrentState.Current.Save();
}
</pre>