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
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:54:46
---
Chaque package, virtuel ou non, peut <a href="/client-api/net-package-api/pushstateobject/">publier des StateObjects</a> dans votre Constellation. Découvrons dans cet article comment intégrer les valeurs de ces StateObjets dans votre code C#.
<h3>La base : le Request &amp; Subscribe de StateObjects</h3>
Le hub Constellation comporte deux méthodes :
<ul>
 	<li>RequestStateObjects : permettant d’interroger "à un instant T"  des StateObjects de votre Constellation</li>
 	<li>SubscribeStateObjects : permettant de s'abonner aux mises à jours des StateObjects de votre Constellation</li>
</ul>
Ces deux méthodes acceptent jusqu’à 4 paramètres, tous optionnels :
<pre class="lang:c# decode:true ">string sentinel = "*", string package = "*", string name = "*", string type = "*"</pre>
Vous devez définir le ou StateObjects que vous souhaitez récupérer (<em>Request</em>) ou suivre (<em>Subscribe</em>) en appliquant des filtres. Le wildcard “*” permet de tout sélectionner, c'est à dire de ne pas filtrer.

Par exemple, pour récupérer tous les StateObjects du package “MonPackage” (et peut importe le nombre d’instance du package) :
<pre class="lang:c# decode:true">PackageHost.RequestStateObjects(package: "MonPackage");</pre>
Si vous souhaitez tous les StateObjects du package “MonPackage” qui tourne sur une sentinelle en particulier, nommée ci-dessous "MA-SENTINELLE" :
<pre class="lang:c# decode:true">PackageHost.RequestStateObjects(sentinel:"MA-SENTINELLE", package: "MonPackage");</pre>
Pour cibler un StateObject en participer sur une instance d'un package :
<pre class="lang:c# decode:true">PackageHost.RequestStateObjects(sentinel: "MA-SENTINELLE", package: "MonPackage", name:"MonStateObject");</pre>
Bien entendu, vous pouvez définir la combinaison que vous souhaitez !

Par exemple, récupérons tous les StateObjects de type “HWMonitor.HardwareList” dans notre Constellation :
<pre class="lang:c# decode:true">PackageHost.RequestStateObjects(type: "HWMonitor.HardwareList");</pre>
Comme on sait que ce type de StateObject ne peut être publié que par le package “HWMonitor”, on pourrait écrire :
<pre class="lang:c# decode:true">PackageHost.RequestStateObjects(package: "HWMonitor", type: "HWMonitor.HardwareList");</pre>
Dans les deux cas (<em>Request</em> ou <em>Subscribe</em>) les StateObjects sont reçus un par un par l’événement .NET : “PackageHost.StateObjectUpdated”.

Par exemple, déployons le package”<a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">HWMonitor</a>” et analysons le StateObject “Hardware” via le <a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a> :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-133.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Detail d'un StateObject dans la Console" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-110.png" alt="Detail d'un StateObject dans la Console" width="424" height="466" border="0" /></a></p>
<p align="left">On retrouve un StateObject de type “HWMonitor.HardwareList” qui contient une liste du hardware de la machine.</p>
<p align="left">Dans notre package nous allons récupérer tous les StateObjects de type “HWMonitor.HardwareList” (donc autant de StateObjects que nous avons d’instance de ce package dans notre Constellation).</p>
<p align="left">On pourrait écrire :</p>

<pre class="lang:c# decode:true">PackageHost.StateObjectUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Hardware sur la machine {0}", e.StateObject.SentinelName);
    foreach (dynamic hw in e.StateObject.DynamicValue)
    {
        PackageHost.WriteInfo("{0} ({1})", hw.Name, hw.Identifier);
    }
};
PackageHost.RequestStateObjects(package: "HWMonitor", type: "HWMonitor.HardwareList");</pre>
En testant notre package dans Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-134.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-111.png" alt="Console log" width="424" height="92" border="0" /></a></p>
<p align="left">Notez qu'ici nous avons utilisé la méthode "<em>RequestStateObjects</em>" c'est à dire que nous récupérons tous les StateObjects du package HWMonitor (quelque soit la sentinelle) de type "HWMonitor.HardwareList" au moement de l'invocation de la méthode.</p>
<p align="left">Allons un peu plus long en affichant en temps réel la consommation du CPU. Cette valeur est publiée dans le StateObject “/intelcpu/0/load/0” par le package HWMonitor.</p>
<p align="left">Pour suivre en temps réel la consommation du CPU des machines (sentinelles) sur lesquelles le package HWMonitor est déployé, on commence par s'abonner à ce StateObject :</p>

<pre class="lang:c# decode:true ">PackageHost.SubscribeStateObjects(package: "HWMonitor", name: "/intelcpu/0/load/0");</pre>
Puis dans l’événement “<em>StateObjectUpdated</em>” il faudra différencier le traitement en fonction du StateObject reçu.

Le code final :
<pre class="lang:c# decode:true">PackageHost.StateObjectUpdated += (s, e) =&gt;
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
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-135.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-112.png" alt="Console log" width="424" height="116" border="0" /></a></p>
<p style="text-align: left;" align="center">Notez qu'ici nous avons utilisé la méthode "<em>SubscribeStateObjects</em>" c'est à dire que nous nous abonnons à tous les StateObjects nommés “/intelcpu/0/load/0” du package HWMonitor (quelque soit la sentinelle). C'est un abonnement, donc à chaque modification des StateObjects respectant notre filtre, nous recevrons dans notre code les nouvelles valeurs des StateObjects.</p>
<p style="text-align: left;" align="center">Cependant, si les StateObjects changent peu fréquemment, nous n'obtenons rien immédiatement. C'est pourquoi il est parfois nécessaire de faire un <em>Request</em> suivi d'un <em>Subscribe</em> pour obtenir la version actuelle du/des StateObject(s) et s'abonner aux mises à jour futures.</p>

<h3>Les StateObjectLink</h3>
Pour simplifier l’exploitation des StateObjects dans votre code, l'API Constellation introduit la notion de “StateObjectLink”.

Avec l'API.NET, il vous suffit simplement de déclarer une propriété .NET dans votre code que vous allez décorer avec l’attribut [<strong>StateObjectLink</strong>].

Votre propriété .NET peut être privée ou publique, d’instance ou statique. De plus le type de votre propriété .NET peut être :
<ul>
 	<li>Un type de base</li>
 	<li>Un type complexe</li>
 	<li>Un dynamic</li>
 	<li>Un StateObject</li>
 	<li>Un StateObjectNotifier</li>
 	<li>Un StateObjectCollectionNotifier</li>
</ul>
Par défaut seules les propriétés .NET <u>de l’instance de votre package</u> (IPackage) sont enregistrées. Si dans votre package vous instanciez des classes contenants des StateObjectsLink vous devez appeler la méthode “<em>PackageHost.RegisterStateObjectLinks</em>” en passant l’instance de votre classe pour l’enregistrement de ses StateObjectLinks (autrement les propriétés resteront nulles).
<pre class="lang:c# decode:true">PackageHost.RegisterStateObjectLinks(monObjet);</pre>
<h4>Lier la valeur d'un StateObject à une propriété NET</h4>
Prenons l’exemple du StateObject “MyNumber” que vous avons <a href="/client-api/net-package-api/pushstateobject/">publié dans cet article</a> par la ligne :
<pre class="lang:c# decode:true ">PackageHost.PushStateObject("MyNumber", 123);</pre>
Pour qu’un autre package puisse l’inclure dans son code, on peut simplement écrire :
<pre class="lang:c# decode:true">[StateObjectLink(Name = "MyNumber")]
public int MyNumber { get; set; }</pre>
Ici la propriété est de type “int” (car la valeur du StateObject est un int) et est liée à la valeur de ce StateObject.

Dès que le StateObject est mis à jour (<em>PushStateObject</em>) par le package qui la produit, votre propriété est également mise à jour de sorte que vous ayez toujours la dernière valeur du StateObject dans votre propriété.

Bien entendu la valeur du StateObject doit être compatible avec le type de la propriété liée (même type ou cast implicite possible), sinon la valeur du StateObject lié ne sera jamais affecté à votre propriété.

Vous pouvez également utiliser des types complexes. Par exemple prenons l’exemple déjà cité ci-dessus :
<pre class="lang:c# decode:true">PackageHost.PushStateObject&lt;MyCustomObject&gt;("MyObject",  new MyCustomObject() { String = "test", Number = 123 });</pre>
Pour créer le StateObjectLink depuis un autre package, on pourra écrire :
<pre class="lang:c# decode:true">[StateObjectLink(Name = "MyObject")]
public MyCustomObject MyObject { get; set; }</pre>
La propriété “MyObject” sera bien du type “MyCustomObject” et sera lié au StateObject nommé “MyObject”.

Cela sous entend que votre package ait la même définition du type “MyCustomObject” : soit en recopiant la classe ou soit en partageant une assembly commune. Notez toutefois que, comme Constellation connait la description des types utilisés par les StateObjects ou MessagesCallbacks, il est possible de générer le code (<a href="#Generer_du_code">lire ici</a>).

Maintenant si le StateObject est un type anonyme ou que vous n’avez pas la définition du type dans votre code, vous pouvez utiliser le type “dynamic”.

On pourrait alors écrire :
<pre class="lang:c# decode:true">[StateObjectLink(Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
Pour terminer reprenons notre StateObject “Hardware” publié par le package HWMonitor :
<pre class="lang:c# decode:true">[StateObjectLink(Package = "HWMonitor", Type = "HWMonitor.HardwareList")]
private dynamic Hardware { get; set; }</pre>
Et à tout moment dans votre code pour pourrez itérer sur cette liste :
<pre class="lang:c# decode:true">foreach (dynamic hw in this.Hardware)
{
    PackageHost.WriteInfo("{0} ({1})", hw.Name, hw.Identifier);
}</pre>
On pourrait reprendre également l’exemple du CPU mais nous aurions un problème : comment savoir quand le StateObject à été mis à jour pour afficher la nouvelle valeur ? C’est que nous verrons avec le <a href="#StateObjectNotifier_notification_de_mise_a_jour">StateObjectNotifier ci-dessous</a>.
<h4>L’unicité des liens</h4>
Dans les exemples ci-dessus, nous lions des propriétés .NET aux StateObjects de votre Constellation en utilisant seulement leurs noms (Name). Or comme vous le savez il peut y avoir, dans votre Constellation, plusieurs packages sur plusieurs sentinelles qui publient des StateObjects avec le même nom !

De ce fait, je peux par exemple écrire :
<pre class="lang:c# decode:true">[StateObjectLink(Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
Ici, je crée un lien entre cette propriété .NET et le<strong>s</strong> StateObjects nommés “MyObject” et publiés par le package “MonPackage”. Mais je ne sais pas combien d’instance du package “MonPackage” je trouverai dans ma Constellation (je pourrais déployer ce package sur toutes mes sentinelles).

L’unicité ne peut se faire qu'avec le triplet : Nom de la sentinelle + Nom du package + Nom du StateObject.
<pre class="lang:c# decode:true">[StateObjectLink(Sentinel = "MON-PC", Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
Dans ce dernier cas j’ai la garantie de lier cette propriété .NET à un et un seul StateObject : celui nommé “MyObject” publié par le package “MonPackage” et déployé sur la sentinelle “MON-PC”.

Dans le cas où votre lien correspond à plusieurs StateObjects, chaque mise à jours de l’un d’entre eux est affectée à la propriété.

Ainsi si vous avez un package nommé “MonPackage” qui tourne sur deux sentinelles (disons “PC1” et “PC2”), votre propriété “MyObject” définie ci-dessous sera tantôt liée à la valeur du StateObject publié par le package sur PC1 tantôt sur l’instance de PC2.
<pre class="lang:c# decode:true">[StateObjectLink(Package = "MonPackage", Name = "MyObject")]
public dynamic MyObject { get; set; }</pre>
Soyez donc vigilant aux liens que vous créez, ou sinon utilisez les StateObject<strong>Collection</strong>Notifier comme nous le verrons <a href="#StateObjectCollectionNotifier_collection_de_StateObjectNotifier">dans la suite de cet article</a>.
<h4>Lier le StateObject entier à une propriété .NET</h4>
Jusqu’à présent nous avons lier des propriétés .NET avec <u>les valeurs</u> de StateObjects. Une fois la liaison établie, vous pouvez utiliser ces propriétés pour accéder à la valeur des StateObjects mais non au StateObject lui même.

Le StateObject contient à la fois la valeur du StateObject mais également les propriétés du StateObject comme son nom, le couple sentinelle/package qui l’a publié, sa date de publication, sa durée de vie (lifetime), son type ou encore des métadonnées (dictionnaire de string/object).

Pour cela, il suffit simplement de créer une propriété de type “StateObject”.

Par exemple pour le StateObject “Hardware” du package “HWMonitor” de la sentinelle “MON-PC” (pour n’avoir qu’une seule valeur), on peut écrire :
<pre class="lang:c# decode:true">[StateObjectLink("MON-PC", "HWMonitor", "Hardware")]
private StateObject Hardware { get; set; }</pre>
Vous aurez ensuite accès aux différentes propriétés de votre StateObject :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Un StateObject" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb1_thumb.png" alt="Un StateObject" width="424" height="272" border="0" /></a></p>
<p align="left">La valeur du StateObject peut être obtenue par la propriété “Value” ou “DynamicValue”.</p>
<p align="left">La propriété “DynamicValue” retourne la “Value” en tant qu’objet dynamique. Dans le cas d’un objet complexe, la “Value” sera de type JObject ou JArray.</p>
<p align="left">Il est donc conseillé de travailler directement avec la “DynamicValue” pour résoudre dynamiquement la valeur de votre StateObject.</p>
<p align="left">Vous disposez également d’une méthode GetValue&lt;T&gt; (ou son équivalent <em>TryGetValue</em>) qui se chargera de convertir votre valeur de StateObject en T .</p>

<h4>StateObjectNotifier : être notifié des mises à jour des StateObjects liés</h4>
Jusqu’à présent nous lions des propriétés .NET avec la valeur d’un StateObject ou le StateObject lui même.

Dans les deux cas, vous pouvez accéder à tout moment à la dernière version de votre StateObject (ou sa valeur) publié dans votre Constellation car un <em>StateObjectLink</em> réalise implicitement un <em>RequestStateObjects</em> à l’initialisation de la propriété puis s’abonne aux StateObjects (<em>SubscribeStateObject</em>) pour mettre à jour en temps réel votre propriété dès que le ou les StateObjects liés sont mis à jour.

Seulement vous ne savez pas “quand” vos StateObject liés sont mis à jour, autrement dit quand est-ce que vos propriétés .NET sont mises à jour.

Pour cela il existe les <em>StateObjectNotifiers</em>. Le principe est simple, vous devez simplement créer une propriété liés de type StateObjectNotifier.

Reprenons le StateObject du CPU publié par le package HWMonitor sur “MON-PC” :
<pre class="lang:c# decode:true">[StateObjectLink("MON-PC", "HWMonitor", "/intelcpu/0/load/0")]
private StateObjectNotifier CPU { get; set; }</pre>
La classe StateObjectNotifier un container de StateObject. Elle comporte une propriété “Value” dans laquelle est contenu le StateObject.
<pre class="lang:c# decode:true">StateObject stateObjectActuel = this.CPU.Value;</pre>
On peut donc afficher la consommation à instant T :
<pre class="lang:c# decode:true">PackageHost.WriteInfo("CPU = {0}%", this.CPU.Value.DynamicValue.Value);</pre>
Pour détailler :
<ul>
 	<li>this (l’instance courante)</li>
 	<li>.CPU (le StateObjectNofitier lié au StateObject de notre CPU)</li>
 	<li>.Value (l’objet StateObject en question)</li>
 	<li>.DynamicValue (la valeur du StateObject sous forme dynamique)</li>
 	<li>.Value (la propriété de l’objet publié par le package HWMonitor)</li>
</ul>
Notez que le StateObjectNotifier comporte une propriété “DynamicValue” qui retourne la “DynamicValue” du StateObject (en clair : StateObjectNotifier.DynamicValue = StateObjectNotifier.Value.DynamicValue).

On peut donc simplifier  le code par :
<pre class="lang:c# decode:true">PackageHost.WriteInfo("CPU = {0}%", this.CPU.DynamicValue.Value);</pre>
Lorsque le (ou les) StateObjets liés sont mis à jour, l’instance du StateObjectNotifier reste inchangée, c’est seulement sa propriété Value qui est mis à jour. De ce fait il est possible de s’abonner à des événements sur un StateObjectNotifier.

A ce sujet, le StateObjectNotifier implémente l’interface bien connue en .NET et notamment dans le monde WPF : “<em>INotifyPropertyChanged</em>”.

De ce fait, un StateObjectNotifier comporte un événement “PropertyChanged” qui vous informera lorsque le StateObject change.
<pre class="lang:c# decode:true">this.CPU.PropertyChanged += (s, e) =&gt;
{
    PackageHost.WriteInfo("Le StateObject a été mis à jour");
    PackageHost.WriteInfo("CPU = {0}%", this.CPU.DynamicValue.Value);
};</pre>
Très pratique notamment pour rafraichir une interface graphique WPF (binding WPF).

De plus le <em>StateObjectNotifier </em>comporte un 2ème événement qui se déclenche lui aussi à la mise à jour du StateObject : le “<em>ValueChanged</em>”.

La différence avec le <em>PropertyChanged</em> réside dans l’EventArgs passé lorsque l’événement se déclenche :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ValueChanged d'un StateObjectNotifier" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb5_thumb.png" alt="ValueChanged d'un StateObjectNotifier" width="424" height="179" border="0" /></a></p>
<p align="left">Le “<em>StateObjectChangedEventArgs</em>” fourni trois propriétés :</p>

<ul>
 	<li>
<div align="left">NewState : la nouvelle version du StateObject</div></li>
 	<li>
<div align="left">OldState : l’ancienne version du StateObject</div></li>
 	<li>
<div align="left">IsNew : un booléen qui indique si c’est un “nouveau” StateObject (c’est à dire que OldState est null)</div></li>
</ul>
<p align="left">On peut donc comparer l’évolution du StateObject</p>

<pre class="lang:c# decode:true">this.CPU.ValueChanged += (s, e) =&gt;
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
Notez que vous pouvez créer vos propres attributs “StateObjectLink” personnalisés en héritant de la classe “StateObjectLinkAttribute”.

De la même façon vous pouvez également créer vos propres StateObjectNotifier en créant simplement une classe qui hérite de “StateObjectNotifier”.
<h4>StateObjectCollectionNotifier : collection de StateObjectNotifier</h4>
Comme nous l’avons vu plus haut, si un StateObjectLink peut lier plusieurs StateObjects dans la même propriété .NET. Chaque mise à jour d’un des StateObjects “remplace” le précèdent !

Pour pouvoir lier plusieurs StateObjects dans une seule propriété .NET, nous pouvons utiliser les <em>StateObjectCollectionNotifiers</em>. La classe <em>StateObjectCollectionNotifier</em> est une <a href="https://msdn.microsoft.com/fr-fr/library/ms668604(v=vs.110).aspx">ObservableCollection</a> de StateObjectNotifier.

Prenons cet exemple :
<pre class="lang:c# decode:true">[StateObjectLink("HWMonitor", "/intelcpu/0/load/0")]
public StateObjectCollectionNotifier CPUs { get; set; }</pre>
Nous utilisons le constructeur du StateObjectLink où le 1er argument est le nom du package et le 2ème le nom du StateObject.

Pour faciliter la compréhension, on peut également utiliser les paramètres nommés :
<pre class="lang:c# decode:true">[StateObjectLink(Package = "HWMonitor", Name = "/intelcpu/0/load/0")]
public StateObjectCollectionNotifier CPUs { get; set; }</pre>
Nous créons donc un lien vers le StateObjects “/intelcpu/0/load/0” du package HWMonitor (celui qui correspond à la consommation du CPU).

On aura donc un StateObject par sentinelle où ce package est déployé.

Par exemple, dans ma Constellation on trouve 9 StateObjects qui correspond, un par machine (sentinelle) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb7_thumb.png" alt="StateObject Explorer" width="424" height="251" border="0" /></a></p>
<p align="left">Comme il s’agit d’une collection de StateObjectNotifier, vous pouvez itérer pour chaque StateObject afin d’afficher par exemple le CPU de chaque sentinelle où le package “HWMonitor” est déployé :</p>

<pre class="lang:c# decode:true">foreach (StateObjectNotifier stateobject in this.CPUs)
{
     PackageHost.WriteInfo("CPU sur {0} = {1}%", stateobject.Value.SentinelName, stateobject.DynamicValue.Value);
}</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Console log" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb10_thumb.png" alt="Console log" width="424" height="104" border="0" /></a></p>
Tout comme le StateObjectNotifier, la classe StateObjectCollectionNotifier comporte aussi l’évènement ValueChanged :
<pre class="lang:c# decode:true">this.CPUs.ValueChanged += (s, e) =&gt;
{
     PackageHost.WriteInfo("CPU sur {0} = {1}%", e.NewState.SentinelName, e.NewState.DynamicValue.Value);
};</pre>
<h3>Générer du code</h3>
Le principe est le même que <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Generateur_de_code">la génération de code pour les MessageCallbacks</a>.

Ouvrez le générateur de code (clic-droit sur votre projet dans Visual Studio &gt; Constellation &gt; Generate Code) et sélectionnez votre Constellation puis cliquez sur “Discover”. Sélectionnez ensuite les packages que vous souhaitez ajouter dans votre code et cliquez sur “Generate”.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-144.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Générateur de code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-121.png" alt="Générateur de code" width="221" height="244" border="0" /></a></p>
<p align="left">Comme pour les MessageCallbacks, vous devez ajouter le namespace correspondant aux “StateObjects” du package que vous souhaitez manipuler.</p>
<p align="left">Par exemple pour inclure les StateObjects du package “MonPremierPackage” :</p>

<pre class="lang:c# decode:true">using ConstellationPackageConsole1.MonPremierPackage.StateObjects;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-145.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Using du code généré par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-122.png" alt="Using du code généré par package" width="424" height="36" border="0" /></a></p>
<p align="left">Ensuite créez votre propriété .NET liée à un StateObject mais en utilisant le StateObjectLink généré par package.</p>
<p align="left">Exemple, pour les StateObjects du package “MonPremierPackage”, vous avez la classe “MonPremierPackageStateObjectLink”.</p>
<p align="left">Inutile de définir le “PackageName” sur ce StateObjectLink par il est nativement fixé par cet attribut personnalisé.</p>
<p align="left">Pour le nom du StateObject, vous pouvez utiliser l’énumération MonPremierPackageStateObjectNames générée par le générateur de code :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-146.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObjectLink généré par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-123.png" alt="StateObjectLink généré par package" width="424" height="65" border="0" /></a></p>
<p align="left">Par exemple, pour lier notre propriété “MonObject” au StateObject “MyObject” publié par le “MonPremierPackage” (<a href="/client-api/net-package-api/stateobjects/#Publiez_des_StateObjects">revoir l’exemple ici</a>), on peut écrire :</p>

<pre class="lang:c# decode:true">[MonPremierPackageStateObjectLink(MonPremierPackageStateObjectNames.MyObject)]
public StateObjectNotifier MonObject { get; set; }</pre>
<p align="left">Ensuite, sur chaque StateObject ou StateObjectNotifier, vous trouverez des méthodes d’extension permettant de convertir la valeur du StateObject dans le type généré dans votre code à partir de la description du package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-147.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Méthode d'extension générées sur les StateObject &amp; StateObjectLink" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-124.png" alt="Méthode d'extension générées sur les StateObject &amp; StateObjectLink" width="424" height="214" border="0" /></a></p>
<p align="left">Par exemple, pour convertir votre StateObjectNotifier en “MyCustomObject”, le type du StateObject défini dans le package “MonPremierPackage” :</p>

<pre class="lang:c# decode:true">MyCustomObject myObject = this.MonObject.AsMonPremierPackageMyCustomObject();</pre>
<p align="left">Chaque type de StateObject décrit dans la Constellation est reproduit par le générateur de code dans votre projet.</p>
<p align="left">Vous retrouvez alors toutes les propriétés du StateObject proprement typées et documentées :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-148.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Classes générées" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-125.png" alt="Classes générées" width="424" height="86" border="0" /></a></p>
<p style="text-align: left;" align="center">N’hésitez pas <a href="/constellation-platform/constellation-sdk/generateur-de-code/">lire cet article très complet sur la génération de code C#</a> dans Constellation.</p>