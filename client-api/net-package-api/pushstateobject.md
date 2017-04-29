---
ID: 1304
post_title: Produire des StateObjects
author: Sebastien Warin
post_date: 2016-03-18 23:08:49
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/pushstateobject/
published: true
post_modified: 2016-08-09 13:50:41
---
<h3>Publier des StateObjects</h3> <p>Pour publier (Push) un StateObject dans Constellation vous devez invoquer la méthode “<em>PackageHost.PushStateObject</em>” en précisant obligatoirement le nom du StateObject et sa valeur.</p> <p>Par exemple, vous pouvez publier n’importe quel type de base :</p><pre class="lang:c# decode:true ">PackageHost.PushStateObject("MyString", "OK");
PackageHost.PushStateObject("MyNumber", 123);
PackageHost.PushStateObject("MyDecimal", 123.12);
PackageHost.PushStateObject("MyBoolean", true);</pre>
<p>Vous retrouverez tous les StateObjects de votre Constellation sur le “StateObject Explorer” de la Console :<a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-142.png"><img title="StateObject Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; border-top-width: 0px; margin-right: auto" border="0" alt="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-119.png" width="424" height="240"></a></p>
<p>Pouvez également publier des objets anonymes :</p><pre class="lang:c# decode:true">PackageHost.PushStateObject("AnonymousObject", new { String = "test", Number = 123 });</pre>
<p>Ou encore avec des types plus complexes :</p><pre class="lang:c# decode:true">PackageHost.PushStateObject&lt;MyCustomObject&gt;("MyObject",  new MyCustomObject() { String = "test", Number = 123 });</pre>
<p align="left">Depuis la Console, vous pouvez analyser en détail votre StateObject et même le suivre en temps réel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-143.png"><img title="StateObject Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-120.png" width="424" height="261"></a></p>
<p>Vous avez également la possibilité de définir des paramètres optionnels comme les métadonnées de votre StateObject ou sa durée de vie.</p>
<p>Par exemple, le StateObject “ShortLife” a ici une durée de vie de 20 secondes après sa publication. Au delà il sera marqué comme expiré.</p><pre class="lang:c# decode:true">PackageHost.PushStateObject("ShortLife", "Expire in 20 secs !!!", lifetime: 20);</pre>
<p>Ici, on publie un StateObject “Salon” en lui associant les metadatas “Id” et “Zone”.</p><pre class="lang:c# decode:true">PackageHost.PushStateObject("Salon", monObjetZoneSalon, metadatas: new Dictionary&lt;string, object&gt; { { "Id", 42 }, { "Zone", "Salon" } });</pre>
<p><u>Note</u> : à chaque (re)démarrage de votre package, tous les StateObjects du package sont purgés de la Constellation.</p>
<h3>Décrire les types de StateObjects</h3>
<p>Il est recommandé de décrire les types des StateObjects dans la Constellation pour activer l’auto-description (utilisé entre autre par les générateurs de code ou le MessageCallback Explorer de la Console).</p>
<p>Sur les objets personnalisés il suffit de décorer les classes de l’attribut [StateObject]. Par exemple pour reprendre l’exemple ci dessus, la classe du StateObject “MyObject” pourrait être définie ainsi :</p><pre class="lang:c# decode:true ">/// &lt;summary&gt;
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
<p>Deux points à retenir :</p>
<ul>
<li>Marquer la classe de l’attribut [StateObject] pour l’inclure dans la description du package 
<li>Ajouter des commentaires de documentation XML pour décrire le StateObject et ses propriétés.</li></ul>
<p>Autrement si vous être amenés à publier des StateObjects d’un type dont vous n’avez pas la possibilité de modifier pour ajouter l’attribut [StateObject] (par exemple un type d’un libraire tierce), vous pouvez sur votre classe IPackage, ajoutez l’attribut [StateObjectKnownTypes] en définissant la liste des types de StateObjects à décrire : </p><pre class="lang:c# decode:true ">[StateObjectKnownTypes(typeof(ThridParty.ComplexObject), typeof(AnotherThridParty.AnotherType))]
public class Program : PackageBase
{
}</pre>
<p>Vous avez également la possibilité d’ajouter à tout moment la description d’un type de StateObject via les méthodes :</p>
<ul>
<li>PackageHost.DescribeStateObjectTypes : en passant une liste de Type 
<li>PackageHost.DescribeStateObjectTypesFromAssembly : en passant une assembly (qui sera scannée à la recherche des classes marquées par l’attribut [StateObject])</li></ul>
<p>Si vous ajoutez vous même des types de StateObjects dans la description du package via les deux méthodes ci-dessus, vous devez ensuite mettre à jour la description du package par la ligne :</p><pre class="lang:c# decode:true">PackageHost.DeclarePackageDescriptor();</pre>
<h3>Supprimer des StateObjects</h3>
<p>Vous avez deux moyens de supprimer des StateObjects :</p>
<ul>
<li>PackageHost.PurgeStateObjects : pour supprimer les StateObjects de votre package 
<li>PackageHost.ControlManager.PurgeStateObjects : pour supprimer n’importe quel StateObject de votre Constellation en utilisant le ControlManager (nécessite une clé d’accès autorisant l’accès au hub de contrôle). <a href="/client-api/net-package-api/controlmanager/">Plus di’nformation ici</a></li></ul>