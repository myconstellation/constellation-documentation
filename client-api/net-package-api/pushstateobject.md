---
ID: 1304
post_title: Publier des StateObjects
author: Sebastien Warin
post_date: 2016-03-18 23:08:49
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/pushstateobject/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:54:01
---
<h3>Publier des StateObjects</h3>
Pour publier (Push) un StateObject dans Constellation vous devez invoquer la méthode “<em>PackageHost.PushStateObject</em>” en précisant obligatoirement le nom du StateObject et sa valeur.

Par exemple, vous pouvez publier un StateObject avec en valeur n’importe quel type de base :
<pre class="lang:c# decode:true ">PackageHost.PushStateObject("MyString", "OK");
PackageHost.PushStateObject("MyNumber", 123);
PackageHost.PushStateObject("MyDecimal", 123.12);
PackageHost.PushStateObject("MyBoolean", true);</pre>
Vous pouvez parcourir tous les StateObjects de votre Constellation grâce au “<a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a>” de la Console Constellation :<a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-142.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-119.png" alt="StateObject Explorer" width="424" height="240" border="0" /></a>

Pouvez également publier des objets avec un type anonyme :
<pre class="lang:c# decode:true">PackageHost.PushStateObject("AnonymousObject", new { String = "test", Number = 123 });</pre>
Ou encore avec des types complexes :
<pre class="lang:c# decode:true">PackageHost.PushStateObject&lt;MyCustomObject&gt;("MyObject",  new MyCustomObject() { String = "test", Number = 123 });</pre>
<p align="left">Depuis la Console, vous pouvez analyser en détail vos StateObjects et même les suivre en temps réel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-143.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-120.png" alt="StateObject Explorer" width="424" height="261" border="0" /></a></p>
Vous avez également la possibilité de définir des paramètres optionnels sur cette méthode pour ajouter les métadonnées de votre StateObject ou encore définir sa durée de vie.

Par exemple, ici le StateObject “ShortLife” a ici une durée de vie de 20 secondes. Au delà il sera donc considéré comme expiré si il n'est pas mis à jour après 20 secondes.
<pre class="lang:c# decode:true">PackageHost.PushStateObject("ShortLife", "Expire in 20 secs !!!", lifetime: 20);</pre>
Ci-dessous, on publie un StateObject “Salon” en lui associant les metadatas “Id” et “Zone”.
<pre class="lang:c# decode:true">PackageHost.PushStateObject("Salon", monObjetZoneSalon, metadatas: new Dictionary&lt;string, object&gt; { { "Id", 42 }, { "Zone", "Salon" } });</pre>
<u>Note</u> : à chaque (re)démarrage de votre package, tous les StateObjects du package sont purgés de la Constellation.
<h3>Décrire les types de StateObjects</h3>
Il est recommandé de décrire les types complexes de vos StateObjects dans la Constellation pour activer l’auto-description (utilisé entre autre par les <a href="/constellation-platform/constellation-sdk/generateur-de-code/">générateurs de code</a> ou le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer </a>de la Console).

Sur les objets personnalisés il suffit de décorer les classes de l’attribut [StateObject]. Par exemple pour reprendre l’exemple ci dessus, la classe du StateObject “MyObject” pourrait être défini ainsi :
<pre class="lang:c# decode:true ">/// &lt;summary&gt;
/// Mon StateObject complexe
/// &lt;/summary&gt;
[StateObject]
public class MyCustomObject
{
    /// &lt;summary&gt;
    /// Gets or sets the number.
    /// &lt;/summary&gt;
    public int Number { get; set; }
    /// &lt;summary&gt;
    /// Gets or sets the string.
    /// &lt;/summary&gt;
    public string String { get; set; }
    /// &lt;summary&gt;
    /// Gets or sets the date time.
    /// &lt;/summary&gt;
    public DateTime UnTypeComplexe { get; set; }
}
</pre>
Deux points à retenir :
<ul>
 	<li>Marquer la classe de l’attribut [StateObject] pour l’inclure dans la description du package</li>
 	<li>Ajouter des commentaires de documentation XML pour décrire le StateObject et ses propriétés.</li>
</ul>
Autrement si vous être amené à publier des StateObjects d’un type dont vous n’avez pas la possibilité de modifier le code pour ajouter l’attribut [StateObject] (par exemple un type d’une libraire tierce), vous pouvez sur votre classe IPackage, ajoutez l’attribut [StateObjectKnownTypes] en définissant la liste des types de StateObjects à décrire :
<pre class="lang:c# decode:true ">[StateObjectKnownTypes(typeof(ThridParty.ComplexObject), typeof(AnotherThridParty.AnotherType))]
public class Program : PackageBase
{
}</pre>
Vous avez également la possibilité d’ajouter à tout moment la description d’un type de StateObject via les méthodes :
<ul>
 	<li>PackageHost.DescribeStateObjectTypes : en passant une liste de Type</li>
 	<li>PackageHost.DescribeStateObjectTypesFromAssembly : en passant une assembly (qui sera scannée à la recherche des classes marquées par l’attribut [StateObject])</li>
</ul>
Si vous ajoutez vous-même des types de StateObjects dans la description du package via les deux méthodes ci-dessus, vous devez ensuite mettre à jour la description du package par la ligne :
<pre class="lang:c# decode:true">PackageHost.DeclarePackageDescriptor();</pre>
<h3>Supprimer des StateObjects</h3>
Vous avez deux moyens de supprimer des StateObjects :
<ul>
 	<li>PackageHost.PurgeStateObjects : pour supprimer les StateObjects de votre package</li>
 	<li>PackageHost.ControlManager.PurgeStateObjects : pour supprimer n’importe quel StateObject de votre Constellation en utilisant le ControlManager (nécessite une clé d’accès autorisant l’accès au hub de contrôle). <a href="/client-api/net-package-api/controlmanager/">Plus di’nformation ici</a></li>
</ul>