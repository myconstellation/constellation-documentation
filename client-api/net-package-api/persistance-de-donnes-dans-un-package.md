---
ID: 1536
post_title: Persistance de données dans un package
author: Sebastien Warin
post_date: 2016-03-21 16:39:53
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/persistance-de-donnes-dans-un-package/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:55:35
---
Certains de vos packages peuvent avoir besoin d’enregistrer/persister des données et vous devez savoir qu'à chaque mise à jour d'un package par sa sentinelle, l’ensemble du package est supprimé avant d’être redéployé.

Il existe deux possibilités pour persister les données :
<ul>
 	<li>Utiliser un stockage “hors Constellation”</li>
 	<li>Utiliser les StateObjects</li>
</ul>
<h3>Stocker en dehors de Constellation</h3>
Comme vous le savez, un package n’est ni plus ni moins qu’une application.

Vous pouvez donc utiliser n’importe quel système de stockage :
<ul>
 	<li>File System</li>
 	<li>Base de donnée en tout genre</li>
 	<li>Web service</li>
 	<li>Etc …</li>
</ul>
Libre à vous de stoker vos données où bon vous sembles mais attention si vous souhaitez diffuser votre package, vous ajoutez des prérequis pour l’installation de votre package.
<h4>Note sur le File System</h4>
Vous pouvez bien sûr stocker des données dans des fichiers. Par défaut le “CurrentDirectory” du package est son répertoire de déploiement. Prenez garde toutefois car à chaque déploiement du package par la sentinelle ce répertoire est vidé.

Vous devez donc utiliser un répertoire “externe” qu’on définira par un <a href="/client-api/net-package-api/settings/">setting</a>.
<h4>SerializationHelper</h4>
Notez que la librairie Constellation met à disposition la classe “<em>SerializationHelper</em>” dans le namespace “Constellation.Utils” permettant de sérialiser et dé-sérialiser facilement des objets en JSON ou en XML.
<pre class="lang:C# decode:true">Constellation.Utils.SerializationHelper.SerializeToFile&lt;MyCustomObject&gt;(new MyCustomObject() { Number = 42, String = "Demo" }, PackageHost.GetSettingValue("MyData.json"));
MyCustomObject myData = Constellation.Utils.SerializationHelper.DeserializeFromFile&lt;MyCustomObject&gt;(PackageHost.GetSettingValue("MyData.json"));</pre>
Vous disposez des méthodes :
<ul>
 	<li>SerializeToFile et DeserializeFromFile pour sérialiser/dé-sérialiser vers/depuis un fichier</li>
 	<li>SerializeToString et DeserializeFromString pour sérialiser/dé-sérialiser vers/depuis un string</li>
</ul>
Par défaut c’est le format JSON qui est utilisé mais vous pouvez définir le <em>DataContractSerialiser</em> (format XML) dans les paramètres de la méthode.
<h3>Utiliser les StateObjects comme stockage</h3>
Le principe est d’utiliser les StateObjects comme stockage, très utile pour sauvegarder/restaurer l’état d’un package.

Chaque package peut <a href="/client-api/net-package-api/stateobjects/">publier des StateObjects</a> que ce soit de simple valeur ou de véritable objet complexe. A chaque (re)démarrage de votre package, tous les StateObjects du package sont purgés de la Constellation mais cependant vous avez la possibilité de les récupérer dans votre code avant cette purge.

Pour cela vous devez ajouter dans le <a href="/concepts/package-manifest/">manifeste du package</a> (<em>PackageInfo.xml</em>) l’attribut “RequestLastStateObjectsOnStart” à true.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-150.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="RequestLastStateObjectsOnStart" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-127.png" alt="RequestLastStateObjectsOnStart" width="424" height="202" border="0" /></a></p>
<p align="left">Ensuite dans votre code, vous devez le plus tôt possible dans le “OnStart” vous abonnez à l’événement “LastStateObjectsReceived“.</p>
<p align="left">Cet événement sera levé lorsque votre package recevra ses anciens StateObjects du serveur avant d’être purgés. L’argument passé dans l’événement contient une liste des StateObjects avant purge.</p>

<pre class="lang:C# decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
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
Grace à ce mécanisme, vos packages peuvent conserver des StateObject sur “leur état”.

Par exemple, imaginez un package “Brain” qui pilote l’allumage automatique des éclairages d’un jardin.

Ce package déclare un <a href="/client-api/net-package-api/consommer-des-stateobjects/">StateObjectLink </a>vers un StateObject qui indique la luminosité extérieure. Si la valeur franchie un certain seuil, le package décide d'allumer ou éteindre les éclairages en envoyant un message pour déclencher un MessageCallback du package pilotant les éclairages.

Imaginez maintenant que suite à l’allumage automatique des éclairages par votre package vous décidez de les éteindre manuellement. Puis, quelques instants plus tard, pour diverses raisons vous devez redémarrer (ou mettre à jour) votre package “Brain”. Vos éclairages risquent de se rallumer car votre “Brain” n’a pas mémorisé qu’il a déjà réalisé cette action !

Pour ce genre de problématique, il est intéressant de stocker ces variables dans une classe d’état et de publier cette classe dans un StateObject qu’on pourra nommer “State”. Lorsque notre package redémarre, on demandera à Constellation de nous renvoyer les derniers StateObjects.

Cela nous permettra de récupérer le dernier état de notre package avant son redémarrage.

Par exemple, vous pouvez définir une classe d’état de la façon suivante :
<pre class="lang:C# decode:true">public class CurrentState
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
Sans oublier d’ajouter un handler pour la restauration de l’état précèdent :
<pre class="lang:C# decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
{
    if (e.StateObjects != null)
    {
        PackageHost.WriteInfo("Received old StateObjects (Count:{0})", e.StateObjects.Count);
        CurrentState.Load(e.StateObjects.FirstOrDefault(so =&gt; so.Name == CurrentState.STATEOBJECT_NAME));
    }
};
</pre>
De cette façon vous pouvez à tout moment accéder à votre variable via votre classe statique “State.Current” et dès que vous changez une valeur, invoquez la méthode Save(), pour re-pusher votre “State” comme StateObject de votre package :
<pre class="lang:C# decode:true">if (DateTime.Now.Date != CurrentState.Current.FemertureVoletSalon.Date &amp;&amp; ** Luminosite &lt; seuil ****)
{
   // Fermer le volet ici !!
   CurrentState.Current.FemertureVoletSalon = DateTime.Now;
   CurrentState.Current.Save();
}
</pre>