---
ID: 1474
post_title: Consommer des StateObjects
author: Sebastien Warin
post_date: 2016-03-20 15:18:48
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/consommer-des-stateobjects/
published: true
post_modified: 2016-03-28 17:49:46
---
<h3>Request &amp; Subscribe de StateObjects</h3> <p>Le hub Constellation comporte deux méthodes :</p> <ul> <li>RequestStateObjects : permet d’interroger à un instant T les StateObjects de votre Constellation  <li>SubscribeStateObjects : permet de vous abonnez aux mises à jours des StateObjects de votre Constellation </li></ul> <p>Ces deux méthodes acceptent jusqu’à 4 paramètres, tous optionnels :</p><pre class="lang:c# decode:true ">string sentinel = "*", string package = "*", string name = "*", string type = "*"</pre>
<p>Vous devez définir le ou StateObjects que vous souhaitez récupérer (Request) ou suivre (Subscribe). Le wildcard “*” permet de tout sélectionner !</p>
<p>Par exemple, pour récupérer tous les StateObjects du package “MonPackage” (et peut importe le nombre d’instance du package) :</p><pre class="lang:c# decode:true">PackageHost.RequestStateObjects(package: "MonPackage");</pre>
<p>SI vous souhaitez tous les StateObjects du package “MonPackage” qui tourne sur une sentinelle en particulier :</p><pre class="lang:c# decode:true">PackageHost.RequestStateObjects(sentinel:"MA-SENTINELLE", package: "MonPackage");</pre>
<p>Pour cibler un StateObject en participer sur une instance :</p><pre class="lang:c# decode:true">PackageHost.RequestStateObjects(sentinel: "MA-SENTINELLE", package: "MonPackage", name:"MonStateObject");</pre>
<p>Bien entendu, vous pouvez définir la combinaison que vous souhaitez !</p>
<p>Par exemple, récupérons tous les StateObjects de type “HWMonitor.HardwareList” dans notre Constellation :</p><pre class="lang:c# decode:true">PackageHost.RequestStateObjects(type: "HWMonitor.HardwareList");</pre>
<p>Comme on sait que ce type de StateObject ne peut être pusher que par le package “HWMonitor”, on pourrait écrire :</p><pre class="lang:c# decode:true">PackageHost.RequestStateObjects(package: "HWMonitor", type: "HWMonitor.HardwareList");</pre>
<p>Dans les deux cas (Request ou Subscribe) les StateObjects sont reçus un par un dans l’évènement : “PackageHost.StateObjectUpdated”.</p>
<p>Par exemple, déployons le package”<a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">HWMonitor</a>” et analyons le StateObject “Hardware” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-133.png"><img title="Detail d'un StateObject dans la Console" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Detail d'un StateObject dans la Console" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-110.png" width="424" height="466"></a></p>
<p align="left">On retrouve un StateObject de type “HWMonitor.HardwareList” qui contient une liste du hardware de la machine avec entre autre un identifier et un nom.</p>
<p align="left">Dans notre package nous allons récupérer tous les StateObjects de type “HWMonitor.HardwareList” (donc autant de StateObjects que nous avons d’instance de ce package dans notre Constellation).</p>
<p align="left">On pourrait écrire :</p><pre class="lang:c# decode:true">PackageHost.StateObjectUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Hardware sur la machine {0}", e.StateObject.SentinelName);
    foreach (dynamic hw in e.StateObject.DynamicValue)
    {
        PackageHost.WriteInfo("{0} ({1})", hw.Name, hw.Identifier);
    }
};
PackageHost.RequestStateObjects(package: "HWMonitor", type: "HWMonitor.HardwareList");</pre>
<p>En testant notre package dans Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-134.png"><img title="Console log" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-111.png" width="424" height="92"></a></p>
<p align="left">Allons un peu plus long en affichant en temps réel la consommation du CPU. Cette valeur est publiée dans le StateObject “/intelcpu/0/load/0” par le package HWMonitor.</p>
<p align="left">Pour suivre en temps réel la consommation du CPU des machines (sentinelles) sur lesquelles le package HWMonitor est déployé :</p><pre class="lang:c# decode:true ">PackageHost.SubscribeStateObjects(package: "HWMonitor", name: "/intelcpu/0/load/0");</pre>
<p>Par contre dans l’évènement “StateObjectUpdated” il faudra différencier le traitement en fonction du StateObject reçu.</p>
<p>Le code final :</p><pre class="lang:c# decode:true">PackageHost.StateObjectUpdated += (s, e) =&gt;
{
    if (e.StateObject.Name == "Hardware")
    {
        PackageHost.WriteInfo("Hardware sur la machine {0}", e.StateObject.SentinelName);
        foreach (dynamic hw in e.StateObject.DynamicValue)
        {
            PackageHost.WriteInfo("{0} ({1})", hw.Name, hw.Identifier);
        }
    }
    else if (e.StateObject.Name == "/intelcpu/0/load/0")
    {
        PackageHost.WriteInfo("CPU de {0} = {1}%", e.StateObject.SentinelName, e.StateObject.DynamicValue.Value);
    }
};
PackageHost.RequestStateObjects(package: "HWMonitor", type: "HWMonitor.HardwareList");
PackageHost.SubscribeStateObjects(package: "HWMonitor", name: "/intelcpu/0/load/0");</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-135.png"><img title="Console log" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-112.png" width="424" height="116"></a></p>
<h3>Les StateObjectLink</h3>
<p>Pour simplifier l’exploitation des StateObjects dans votre code, l’API .NET introduit la notion de “StateObjectLink”.</p>
<p>Il vous suffit simplement de déclarer une propriété .NET dans votre code que vous allez décorer avec l’attribut [<strong>StateObjectLink</strong>].</p>
<p>Cette propriété peut être privée ou publique, d’instance ou statique et le type de la propriété peut être :</p>
<ul>
<li>Un type de base 
<li>Un type complexe 
<li>Un dynamic 
<li>Un StateObject 
<li>Un StateObjectNotifier 
<li>Un StateObjectCollectionNotifier </li></ul>
<p>Par défaut seules les propriétés .NET <u>de l’instance de votre package</u> (IPackage) sont enregistrées. Si dans votre package vous instanciez des classes contenants des StateObjectsLink vous devez appeler la méthode “<em>PackageHost.RegisterStateObjectLinks</em>” en passant l’instance de votre classe pour l’enregistrement de ses StateObjectLinks (autrement les propriétés resteront nulles).</p><pre class="lang:c# decode:true">PackageHost.RegisterStateObjectLinks(monObjet);</pre>
<h4>Lier la valeur d’un StateObject</h4>
<p>Prenons l’exemple du StateObject “MyNumber” que vous avons publié au début de cet article par la ligne :</p><pre class="lang:c# decode:true ">PackageHost.PushStateObject("MyNumber", 123);</pre>
<p>Pour qu’un autre package puisse l’inclure dans son code, on pourrait écrire :</p><pre class="lang:c# decode:true">[StateObjectLink(Name = "MyNumber")]
public int MyNumber { get; set; }</pre>
<p>Ici la propriété est un “int” (car la valeur du StateObject est un int) et est liée à la valeur de ce StateObject.</p>
<p>Dès que le StateObject est mis à jour (<em>PushStateObject</em>), votre propriété est également mise à jour de sorte que vous ayez toujours la dernière valeur du StateObject dans votre propriété.</p>
<p>Bien entendu la valeur du StateObject doit être compatible avec le type de la propriété liée (même type ou cast implicite possible), sinon votre propriété ne sera jamais affectée.</p>
<p>Vous pouvez également utiliser des types complexes. Par exemple prenons l’exemple déjà cité ci-dessus :</p><pre class="lang:c# decode:true">PackageHost.PushStateObject&lt;MyCustomObject&gt;("MyObject",  new MyCustomObject() { String = "test", Number = 123 });</pre>
<p>Pour créer le StateObjectLink depuis un autre package, on pourra écrire :</p><pre class="lang:c# decode:true">[StateObjectLink(Name = "MyObject")]
public MyCustomObject MyObject { get; set; }</pre>
<p>La propriété “MyObject” sera bien du type “MyCustomObject” et sera lié au StateObject nommé “MyObject”.</p>
<p>Cela sous entend que votre package ait la même définition du type “MyCustomObject” : soit en recopiant la classe ou soit en partageant une assembly commune. Notez toutefois que, comme Constellation connait la description des types utilisés par les StateObjects ou MessagesCallbacks, il est possible de générer le code (<a href="#Generer_du_code">lire ici</a>).</p>
<p>Maintenant si le StateObject est un type anonyme ou que vous n’avez pas la définition du type dans votre code, vous pouvez utiliser le type “dynamic”.</p>
<p>On pourrait alors écrire :</p><pre class="lang:c# decode:true">[StateObjectLink(Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
<p>Pour terminer reprenons notre StateObject “Hardware” publié par le package HWMonitor :</p><pre class="lang:c# decode:true">[StateObjectLink(Package = "HWMonitor", Type = "HWMonitor.HardwareList")]
private dynamic Hardware { get; set; }</pre>
<p>Et à tout moment dans votre code pour pourrez itérer sur cette liste :</p><pre class="lang:c# decode:true">foreach (dynamic hw in this.Hardware)
{
    PackageHost.WriteInfo("{0} ({1})", hw.Name, hw.Identifier);
}</pre>
<p>On pourrait reprendre également l’exemple du CPU mais nous irons un problème : comment savoir quand le StateObject à été mis à jour pour afficher la nouvelle valeur ? C’est que nous verrons avec le <a href="#StateObjectNotifier_notification_de_mise_a_jour">StateObjectNotifier ci-dessous</a>.</p>
<h4>L’unicité des liens</h4>
<p>dans les exemples ci-dessus, nous lions des propriétés .NET aux StateObjects Constellation en utilisant seulement leurs noms (Name). Or comme vous le savez il peut y avoir, dans votre Constellation, plusieurs packages sur plusieurs sentinelles qui publient des StateObjects avec le même nom !</p>
<p>De la même manière j’écrire la propriété suivante :</p><pre class="lang:c# decode:true">[StateObjectLink(Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
<p>Je crée un lien entre cette propriété .NET et le<strong>s</strong> StateObjects nommés “MyObject” et publiés par le package “MonPackage”. Mais je ne sais pas combien d’instance du package “MonPackage” je trouverai dans ma Constellation (je pourrais déployer ce package sur toutes mes sentinelles).</p>
<p>L’unicité ne peut se faire qu'avec le triplet : Nom de la sentinelle + Nom du package + Nom du StateObject.</p><pre class="lang:c# decode:true">[StateObjectLink(Sentinel = "MON-PC", Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
<p>Dans ce dernier cas j’ai la garantie de lier cette propriété .NET à un et un seul StateObject : celui nommé “MyObject” publié par le package “MonPackage” qui tourne sur la sentinelle “MON-PC”.</p>
<p>Dans le cas où votre lien correspond à plusieurs StateObjects, chaque mise à jours de l’un d’entre eux est affecté à la propriété.</p>
<p>Ainsi si vous avez votre package “MonPackage” qui tourne sur deux sentinelles (disons “PC1” et “PC2”), votre propriété “MyObject” définie ci-dessous sera tantôt liée à la valeur du StateObject publié par le package sur PC1 tantôt sur l’instance de PC2.</p><pre class="lang:c# decode:true">[StateObjectLink(Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
<p>Soyez donc vigilant aux liens que vous créez, ou sinon utilisez les StateObject<strong>Collection</strong>Notifier comme nous le verrons <a href="#StateObjectCollectionNotifier_collection_de_StateObjectNotifier">dans la suite de cet article</a>.</p>
<h4>Lier un StateObject</h4>
<p>Jusqu’à présent nous avons lier des propriétés .NET avec <u>les valeurs</u> de StateObjects. Une fois la liaison établie, vous pouvez utiliser ces propriétés pour accéder à la valeur des StateObjects mais non au StateObject lui même.</p>
<p>Le StateObject est une coquille qui certes contient la valeur mais également des propriétés comme : son nom, le couple sentinelle/package qui l’a publié, sa date de publication, sa durée de vie (lifetime), son type ou encore ses métadonnées (dictionnaire de string/object).</p>
<p>Pour cela, il suffit simplement de créer une propriété de type “StateObject”.</p>
<p>Par exemple pour le StateObject “Hardware” du package “HWMonitor” de la sentinelle “MON-PC” (pour n’avoir qu’une seule valeur), on peut écrire :</p><pre class="lang:c# decode:true">[StateObjectLink("MON-PC", "HWMonitor", "Hardware")]
private StateObject Hardware { get; set; }</pre>
<p>Vous aurez ensuite accès aux différentes propriétés de votre StateObject :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb1.png"><img title="Un StateObject" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Un StateObject" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb1_thumb.png" width="424" height="272"></a></p>
<p align="left">La valeur du StateObject peut être obtenue par la propriété “Value” ou “DynamicValue”.</p>
<p align="left">La propriété “DynamicValue” retourne la “Value” en tant qu’objet dynamique. Dans le cas d’un objet complexe, la “Value” sera de type JObject ou JArray.</p>
<p align="left">Il est donc conseillé de travailler directement avec la “DynamicValue” pour résoudre dynamiquement la valeur de votre StateObject.</p>
<p align="left">Vous disposez également d’une méthode GetValue&lt;T&gt; qui se chargera de convertir votre valeur de StateObject en T et son équivalent TryGetValue.</p>
<h4>StateObjectNotifier : notification de mise à jour</h4>
<p>Jusqu’à présent nous lions des propriétés .NET avec la valeur d’un StateObject ou le StateObject lui même.</p>
<p>Dans les deux cas, vous pouvez accéder à tout moment à la dernière version de votre StateObject (ou sa valeur) car un StateObjectLink réalise un RequestStateObjects à l’initialisation de la propriété puis s’abonne aux StateObjects (SubscribeStateObject) pour mettre à jour en temps reel votre propriété dès que le/les StateObjects liés sont mis à jour.</p>
<p>Seulement vous ne savez pas “quand” est-ce que les StateObjects sont mis à jour, autrement dit quand est-ce que vos propriétés .NET sont affectées à de nouvelles versions de vos StateObjects et de leurs valeurs.</p>
<p>Pour cela il existe les StateObjectNotifier. Le principe est simple, vous devez simplement créer une propriété de type StateObjectNotifier et la lier toujours avec le StateObjectObjectLink.</p>
<p>Reprenons le StateObject du CPU publié par le package HWMonitor sur “MON-PC” :</p><pre class="lang:c# decode:true">[StateObjectLink("MON-PC", "HWMonitor", "/intelcpu/0/load/0")]
private StateObjectNotifier CPU { get; set; }</pre>
<p>La classe StateObjectNotifier un container de StateObject. Elle comporte une propriété “Value” dans laquelle est contenu le StateObject.</p><pre class="lang:c# decode:true">StateObject stateObjectActuel = this.CPU.Value;</pre>
<p>On peut donc afficher la consommation à instant T :</p><pre class="lang:c# decode:true">PackageHost.WriteInfo("CPU = {0}%", this.CPU.Value.DynamicValue.Value);</pre>
<p>Pour détailler :</p>
<ul>
<li>this (l’instance courante) 
<li>.CPU (le StateObjectNofitier lié au StateObject de notre CPU) 
<li>.Value (l’objet StateObject en question) 
<li>.DynamicValue (la valeur du StateObject sous forme dynamique) 
<li>.Value (la propriété de l’objet publié par le package HWMonitor) </li></ul>
<p>Notez que le StateObjectNotifier comporte une propriété “DynamicValue” qui retourne la “DynamicValue” du StateObject (en clair : StateObjectNotifier.DynamicValue = StateObjectNotifier.Value.DynamicValue).</p>
<p>On peut donc simplifier&nbsp; le code par :</p><pre class="lang:c# decode:true">PackageHost.WriteInfo("CPU = {0}%", this.CPU.DynamicValue.Value);</pre>
<p>Lorsque le (ou les) StateObjets liés sont mis à jour, l’instance du StateObjectNotifier reste inchangée, c’est seulement sa propriété Value qui est mis à jour. De ce fait il est possible de s’abonner à des évènements sur un StateObjectNotifier.</p>
<p>A ce sujet, le StateObjectNotifier implémente l’interface bien connue en .NET et notamment dans le monde WPF : “INotifyPropertyChanged”.</p>
<p>De ce fait, un StateObjectNotifier comporte un évènement “PropertyChanged” qui vous informera lorsque le StateObject et donc la propriété “Value” change.</p><pre class="lang:c# decode:true">this.CPU.PropertyChanged += (s, e) =&gt;
{
    PackageHost.WriteInfo("Le StateObject a été mis à jour");
    PackageHost.WriteInfo("CPU = {0}%", this.CPU.DynamicValue.Value);
};</pre>
<p>Très pratique notamment pour rafraichir une interface graphique WPF (binding WPF).</p>
<p>Autre le StateObjectNotifier&nbsp; comporte un 2ème évènement qui se déclenche lui aussi à la mise à jour de la valeur : le “ValueChanged”.</p>
<p>La différence avec le PropertyChanged réside dans l’EventArgs passé lorsque l’évènement se déclenche :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb5.png"><img title="ValueChanged d'un StateObjectNotifier" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="ValueChanged d'un StateObjectNotifier" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb5_thumb.png" width="424" height="179"></a></p>
<p align="left">Le “StateObjectChangedEventArgs” fourni trois propriétés fort utiles :</p>
<ul>
<li>
<div align="left">NewState : la nouvelle version du StateObject</div>
<li>
<div align="left">OldState : l’ancienne version du StateObject</div>
<li>
<div align="left">IsNew : un booléen qui indique si c’est un “nouveau” StateObject (c’est à dire que OldState est null)</div></li></ul>
<p align="left">On peut donc comparer l’évolution du StateObject</p><pre class="lang:c# decode:true">this.CPU.ValueChanged += (s, e) =&gt;
{
    if (e.IsNew)
    {
         PackageHost.WriteInfo("CPU = {0}%", e.NewState.DynamicValue.Value);
    }
    else
    {
         double difference = (double)e.NewState.DynamicValue.Value - (double)e.OldState.DynamicValue.Value;
         PackageHost.WriteInfo("CPU = {0}% - TENDANDE: ", e.NewState.DynamicValue.Value, difference &gt; 0 ? "A LA HAUSSE" : "A LA BAISSE");
    }
};</pre>
<h4>StateObjectLink et Notifier personnalisés</h4>
<p>Notez que vous pouvez créer vos propres attributs “StateObjectLink” personnalisés en héritant de la classe “StateObjectLinkAttribute”.</p>
<p>De la même façon vous pouvez également créer vos propres StateObjectNotifier en créant simplement une classe qui hérite de “StateObjectNotifier”.</p>
<h4>StateObjectCollectionNotifier : collection de StateObjectNotifier</h4>
<p>Comme nous l’avons vu plus haut, si un StateObjectLink peut lier plusieurs StateObjects dans la même propriété .NET. Chaque mise à jour d’un des StateObjects “remplace” le précèdent !</p>
<p>Pour pouvoir lier plusieurs StateObjects dans une seule propriété, nous pouvons utiliser les StateObjectCollectionNotifier. La classe StateObjectCollectionNotifier est une <a href="https://msdn.microsoft.com/fr-fr/library/ms668604(v=vs.110).aspx">ObservableCollection</a> de StateObjectNotifier.</p>
<p>Prenons cet exemple :</p><pre class="lang:c# decode:true">[StateObjectLink("HWMonitor", "/intelcpu/0/load/0")]
public StateObjectCollectionNotifier CPUs { get; set; }</pre>
<p>Nous utilisons le constructeur du StateObjectLink où le 1er argument est le nom du package et le 2ème le nom du StateObject.</p>
<p>Pour faciliter la compréhension, on peut également utiliser les paramètres nommés :</p><pre class="lang:c# decode:true">[StateObjectLink(Package = "HWMonitor", Name = "/intelcpu/0/load/0")]
public StateObjectCollectionNotifier CPUs { get; set; }</pre>
<p>Nous créons un lien vers le StateObjects “/intelcpu/0/load/0” du package HWMonitor (celui qui correspond à la consommation du CPU).</p>
<p>On aura donc un StateObject par sentinelle où ce package est déployé.</p>
<p>Par exemple, dans ma Constellation on trouve 9 StateObjects qui correspond, un par machine (sentinelle) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb7.png"><img title="StateObject Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb7_thumb.png" width="424" height="251"></a></p>
<p align="left">Comme il s’agit d’une collection de StateObjectNotifier, vous pouvez itérer pour chaque StateObject afin d’afficher par exemple le CPU de chaque sentinelle où le package “HWMonitor” est déployé :</p><pre class="lang:c# decode:true">foreach (StateObjectNotifier stateobject in this.CPUs)
{
     PackageHost.WriteInfo("CPU sur {0} = {1}%", stateobject.Value.SentinelName, stateobject.DynamicValue.Value);
}</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb10.png"><img title="Console log" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb10_thumb.png" width="424" height="104"></a></p>
<p>Tout comme le StateObjectNotifier, la classe StateObjectCollectionNotifier comporte aussi l’évènement ValueChanged :</p><pre class="lang:c# decode:true">this.CPUs.ValueChanged += (s, e) =&gt;
{
     PackageHost.WriteInfo("CPU sur {0} = {1}%", e.NewState.SentinelName, e.NewState.DynamicValue.Value);
};</pre>
<h3>Générer du code</h3>
<p>Le principe est le même que <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Generateur_de_code">la génération de code pour les MessageCallbacks</a>.</p>
<p>Ouvrez le générateur de code (clic-droit sur votre projet dans Visual Studio &gt; Constellation &gt; Generate Code) et sélectionnez votre Constellation puis cliquez sur “Discover”. Sélectionnez ensuite les packages que vous souhaitez ajouter dans votre code et cliquez sur “Generate”.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-144.png"><img title="G&eacute;n&eacute;rateur de code" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="G&eacute;n&eacute;rateur de code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-121.png" width="221" height="244"></a></p>
<p align="left">Comme pour les MessageCallbacks, vous devez ajouter le namespace correspondant aux “StateObjects” du package que vous souhaitez manipuler.</p>
<p align="left">Par exemple pour inclure les StateObjects du package “MonPremierPackage” :</p><pre class="lang:c# decode:true">using ConstellationPackageConsole1.MonPremierPackage.StateObjects;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-145.png"><img title="Using du code g&eacute;n&eacute;r&eacute; par package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Using du code g&eacute;n&eacute;r&eacute; par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-122.png" width="424" height="36"></a></p>
<p align="left">Ensuite créer votre propriété .NET lié à un StateObject mais en utilisant le StateObjectLink généré par package.</p>
<p align="left">Exemple, pour les StateObjects du package “MonPremierPackage”, vous avez la classe “MonPremierPackageStateObjectLink”.</p>
<p align="left">Inutile de définir le “PackageName” sur ce StateObjectLink par il est nativement fixé au package “MonPremierPackage”.</p>
<p align="left">Pour le nom du StateObject, vous pouvez utiliser l’énumération MonPremierPackageStateObjectNames fraichement générée :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-146.png"><img title="StateObjectLink g&eacute;n&eacute;r&eacute; par package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="StateObjectLink g&eacute;n&eacute;r&eacute; par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-123.png" width="424" height="65"></a></p>
<p align="left">Par exemple, pour lier notre propriété “MonObject” au StateObject “MyObject” publié par le “MonPremierPackage” (<a href="/client-api/net-package-api/stateobjects/#Publiez_des_StateObjects">revoir l’exemple</a>), on peut écrire :</p><pre class="lang:c# decode:true">[MonPremierPackageStateObjectLink(MonPremierPackageStateObjectNames.MyObject)]
public StateObjectNotifier MonObject { get; set; }</pre>
<p align="left">Ensuite, sur chaque StateObject ou StateObjectNotifier, vous trouverez des méthodes d’extension permettant de convertir la valeur du StateObject dans le type proprement généré dans votre code à partir de la description du package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-147.png"><img title="M&eacute;thode d'extension g&eacute;n&eacute;r&eacute;es sur les StateObject &amp; StateObjectLink" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="M&eacute;thode d'extension g&eacute;n&eacute;r&eacute;es sur les StateObject &amp; StateObjectLink" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-124.png" width="424" height="214"></a></p>
<p align="left">Par exemple, pour convertir votre StateObjectNotifier en “MyCustomObject”, le type du StateObject défini dans le package “MonPremierPackage” :</p><pre class="lang:c# decode:true">MyCustomObject myObject = this.MonObject.AsMonPremierPackageMyCustomObject();</pre>
<p align="left">Chaque type de StateObject décrit dans la Constellation est reproduit par le générateur de code dans votre projet.</p>
<p align="left">Vous retrouvez alors toutes les propriétés du StateObject proprement typées et documentées :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-148.png"><img title="Classes g&eacute;n&eacute;r&eacute;es" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Classes g&eacute;n&eacute;r&eacute;es" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-125.png" width="424" height="86"></a></p>