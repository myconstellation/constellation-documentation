---
ID: 1287
post_title: 'Créez votre premier package Constellation en C#'
author: Sebastien Warin
post_date: 2016-03-16 11:00:22
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-csharp/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:52:40
---
Dans cet article nous allons découvrir comment créer et déployer votre premier package Constellation en C# avec Visual Studio.

<p style="text-align: center;"><!--more-->
<iframe width="560" height="315" src="https://www.youtube.com/embed/wo3960Gwv6k" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>

<h3>Prérequis</h3>

<ul>
    <li>Un accès “Administrator” à une Constellation (Management API &amp; Developper access)</li>
    <li>Le SDK Constellation installé</li>
</ul>

Nous vous conseillons de suivre <a href="/getting-started/installer-constellation/">le guide de démarrage ici</a> avant de démarrer.

<h3>Créez le package dans Visual Studio</h3>

<ul>
    <li>Lancez Visual Studio</li>
    <li>Créez un nouveau projet de type “Constellation Package Console” (dans la catégorie Constellation) :</li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-98.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Création d'un package Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-75.png" alt="Création d'un package Constellation" width="424" height="294" border="0" /></a></p>

<ul>
    <li>Démarrez le package en mode debug en cliquant sur le bouton “Start” ou en pressant la touche “F5” :</li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-160.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Debugging Visual Studio" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-137.png" alt="Debugging Visual Studio" width="424" height="111" border="0" /></a></p>

<ul>
    <li>
<div align="left">Au démarrage, votre package affichera une sorte de ”Hello World” :</div></li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-100.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Hello World" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-77.png" alt="Hello World" width="424" height="209" border="0" /></a></p>

<p align="left">Vous constaterez que :</p>

<ul>
    <li>
<div align="left">La méthode “<em>OnStart</em>” est la méthode invoquée au démarrage de votre package</div></li>
    <li>
<div align="left"><em>PackageHost.WriteInfo</em> est une méthode pour écrire des logs de type “info”</div></li>
    <li>
<div align="left">PackageHost.IsRunning = true (votre package est bien en cours)</div></li>
    <li>
<div align="left">PackageHost.IsConnected = false (votre package n’est pas connecté à Constellation, car nous l’avons lancé en local)</div></li>
</ul>

<h3 align="left">Fonctionnement de base</h3>

<p align="left">Un package est une application !</p>

<p align="left">Il faut impérativement appeler la méthode “<em>PackageHost.Start</em>” au démarrage de l’application, c’est à dire dans la méthode “Main” autrement, ce n’est pas un package Constellation mais une simple application !</p>

<p align="left">Lorsque vous appelez la méthode “<em>PackageHost.Start</em>” vous devez impérativement transférer les arguments (args) et indiquer la classe de votre package qui est dans notre exemple “Program”.</p>

<pre class="lang:c# decode:true">static void Main(string[] args)
{
    PackageHost.Start&lt;Program&gt;(args);
}</pre>

<p align="left">La classe d’un package doit être une classe qui implémente l’interface “<u>IPackage</u>”. Cette interface définie trois méthodes :</p>

<ul>
    <li>
<div align="left"><u>OnStart</u> qui sera invoqué lorsque le package a démarré</div></li>
    <li>
<div align="left"><u>OnPreShutdown</u> : invoqué lorsque le package va s’arrêter (à ce stade votre package est toujours connecté à Constellation, vous pouvez encore pusher des StateObjects, envoyer des messages, écrire des logs sur le hub, etc..)</div></li>
    <li>
<div align="left">OnShutdown : invoqué après le <u>OnPreShutdown</u> et après avoir fermé les connexions.</div></li>
</ul>

Pour vous éviter de devoir implémenter ces trois méthodes, votre classe peut hériter de la classe “PackageBase”. Cette classe abstraite implémente l’interface <u>IPackage</u> dans des méthodes virtuelles vides.

Libre à vous d’implémenter les méthodes que vous souhaitez !

Dans le template de projet créé ci-dessus, la classe “Program” hérite de “PackageBase” et redéfinie la méthode “OnStart” pour écrire un message de type “info” (<em>PackageHost.WriteInfo</em>) lorsque le package a démarré.

<h3>Ecrire des logs</h3>

Pour écrire des logs depuis un package Constellation vous disposez des méthodes :

<ul>
    <li>PackageHost.WriteDebug</li>
    <li>PackageHost.WriteInfo</li>
    <li>PackageHost.WriteWarn</li>
    <li>PackageHost.WriteError</li>
</ul>

Chacune de ces méthodes écrivent un message qui peut être formaté avec des arguments à la manière d’un “string.Format” :

<pre class="lang:c# decode:true">PackageHost.WriteInfo("Je suis le package nommé {0} version {1}", PackageHost.PackageName, PackageHost.PackageVersion);</pre>

<u>Attention</u> : bien respecter les index dans le format de votre message sous peine d’avoir une erreur.

<u>Note</u> : la méthode WriteDebug écrit seulement dans la console (mode debug local). Les logs de type “Debug” ne sont jamais envoyés dans la Constellation.

<h3>Accéder aux settings</h3>

Chaque package peut définir des paramètres de configuration définis au niveau du serveur Constellation. Cela vous permet de changer ces paramètres directement depuis la Constellation qui se chargera de redescendre ces paramètres sur vos packages.

Il y a deux types de settings :

<ul>
    <li>Les “Setting Value” : très simple il s’agit d’un couple clé/value à l’instant des &lt;appSettings&gt; d’une application .NET</li>
    <li>Les “Setting Content”  : au lieu de définir la valeur d’un paramètre dans un attribut XML, on peut la définir dans un élément XML enfant permettant d’avoir des settings qui renferme du XML ou JSON</li>
</ul>

Voici par exemple des “SettingValues”  déclarés dans notre configuration :

<pre class="lang:default decode:true">&lt;setting key="MyBoolSetting" value="true" /&gt;      
&lt;setting key="MyStringSetting" value="This is a string" /&gt;
&lt;setting key="MyIntSetting" value="123" /&gt;</pre>

Ces trois settings définissent la valeur dans l’attribut “value” (= SettingValue).

Pour récupérer la valeur du paramètre “MyStringSetting”  dans votre code, utilisez la méthode “GetSettingValue” :

<pre class="lang:c# decode:true">PackageHost.WriteInfo("My String = {0}", PackageHost.GetSettingValue("MyStringSetting"));</pre>

La méthode “GetSettingValue” renvoie la valeur d’un setting de type “string” mais vous pouvez également caster la valeur depuis cette méthode en utilisant sa forme générique :

<pre class="lang:c# decode:true">int myIntSetting = PackageHost.GetSettingValue&lt;int&gt;("MyIntSetting");
bool myBoolSetting = PackageHost.GetSettingValue&lt;bool&gt;("MyBoolSetting");</pre>

Si par contre vous avez un modèle de configuration plus compliqué qu’une série de clé/valeur vous pouvez utiliser les “Setting Contents” pour définir du XML ou JSON comme valeur de setting.

Par exemple la configuration de votre package peut définir deux autres settings, contenant du XML ou JSON de cette façon :

<pre class="lang:default decode:true">&lt;setting key="MyXmlDocument"&gt;
  &lt;content&gt;
    &lt;note date="09-02-2016"&gt;
      &lt;to&gt;Tove&lt;/to&gt;
      &lt;from&gt;Jani&lt;/from&gt;
      &lt;heading&gt;Reminder&lt;/heading&gt;
      &lt;body&gt;Don't forget me this weekend!&lt;/body&gt;
    &lt;/note&gt;
  &lt;/content&gt;
&lt;/setting&gt;

&lt;setting key="MyJsonObject"&gt;
  &lt;content&gt;
    &lt;![CDATA[
    {
      "Number": 123,
      "String" : "This is a test (local)",
      "Boolean": true
    }
    ]]&gt;
  &lt;/content&gt;
&lt;/setting&gt;
</pre>

Plusieurs méthodes pour récupérer ces settings :

<ul>
    <li><u>GetSettingValue</u> : vous pouvez toujours récupérer le contenu brute de votre setting sous forme d’un string</li>
    <li><u>GetSettingAsJsonObject</u> : dé-sérialise le contenu JSON de votre setting et vous retourne un objet dynamique</li>
    <li><u>GetSettingAsJsonObject&lt;T&gt;</u> : dé-sérialise le contenu JSON de votre setting et le convertie dans un objet de votre type (T)</li>
    <li><u>GetSettingAsXmlDocument</u> : dé-sérialise le contenu XML de votre setting et vous retourne un XmlDocument</li>
    <li><u>GetSettingAsConfigurationSection&lt;TConfigurationSection&gt;</u> : dé-sérialise le contenu XML de votre setting sous forme d’un ConfigurationSection .NET</li>
</ul>

Par exemple, pour manipuler le setting XML on pourrait écrire :

<pre class="lang:c# decode:true">var xml = PackageHost.GetSettingAsXmlDocument("MyXmlDocument");
PackageHost.WriteInfo("My XmlDocument Attribute = {0} , first node value = {1}", xml.ChildNodes[0].Attributes["date"].Value, xml.ChildNodes[0].FirstChild.InnerText);</pre>

Autre exemple avec le setting JSON :

<pre class="lang:c# decode:true">dynamic json = PackageHost.GetSettingAsJsonObject("MyJsonObject");
PackageHost.WriteInfo("My JsonObject String={0}, Int={1}, Boolean={2}", json.String, json.Number, json.Boolean);
</pre>

Les settings d’un package sont déclarés au niveau du serveur dans la déclaration du package et/ou dans le fichier App.config.

Tous les settings doivent être déclarés dans le manifeste du package (fichier PackageInfo.xml).

Retrouvez plus d’information sur les settings <a href="/client-api/net-package-api/les-settings/">dans cet article</a>.

<h3>Publier des StateObjects</h3>

Pour publier (Push) un StateObject dans Constellation vous devez invoquer la méthode “<em>PackageHost.PushStateObject</em>” en précisant obligatoirement le nom du StateObject et sa valeur.

Par exemple, vous pouvez publié n’importe quel type de base :

<pre class="lang:c# decode:true">PackageHost.PushStateObject("MyString", "OK");
PackageHost.PushStateObject("MyNumber", 123);
PackageHost.PushStateObject("MyDecimal", 123.12);
PackageHost.PushStateObject("MyBoolean", true);</pre>

Pouvez également publié des objets anonymes :

<pre class="lang:c# decode:true">PackageHost.PushStateObject("AnonymousObject", new { String = "test", Number = 123 });</pre>

Ou encore avec des types plus complexes :

<pre class="lang:c# decode:true">PackageHost.PushStateObject&lt;MyCustomObject&gt;("MyObject",  new MyCustomObject() { String = "test", Number = 123 });</pre>

Vous avez également la possibilité de définir des paramètres optionnels comme les métadonnées de votre StateObject ou sa durée de vie.

Par exemple, le StateObject “ShortLife” a une durée de vie de 20 secondes après sa publication. Au delà il sera marqué comme expiré.

<pre class="lang:c# decode:true">PackageHost.PushStateObject("ShortLife", "Expire in 20 secs !!!", lifetime: 20);</pre>

Ici, on publie un StateObject “Salon” en lui associé les metadatas “Id” et “Zone”.

<pre class="lang:c# decode:true">PackageHost.PushStateObject("Salon", monObjetZoneSalon, metadatas: new Dictionary&lt;string, object&gt; { { "Id", 42 }, { "Zone", "Salon" } });</pre>

Sur les objets personnalisés il est recommande de décorer les classes de l’attribut [StateObject] si vous avez possibilité de modifier le code de la classe. Autrement, sur votre classe IPackage, ajoutez l’attribut [StateObjectKnownTypes] en définissant tous les types de StateObjects que vous serez amener à publier. Cela permet de décrire ces types dans la Constellation pour l’auto-description (utilisez entre autre par les générateurs de code).

Pour plus d’information sur <a href="/client-api/net-package-api/les-stateobjects/">les StateObjects dans l’API .NET cliquez ici</a>.

<h3>Tester son package dans sa Constellation</h3>

Pour tester nous allons créer un package qui push à intervalle régulier un StateObject.

Le code de notre Package sera :

<pre class="lang:c# decode:true">public class Program : PackageBase
{
    static void Main(string[] args)
    {
        PackageHost.Start&lt;Program&gt;(args);
    }

    public override void OnStart()
    {
        PackageHost.WriteInfo("Package starting - IsRunning: {0} - IsConnected: {1}", PackageHost.IsRunning, PackageHost.IsConnected);
        PackageHost.WriteInfo("Je suis le package nommé {0} version {1}", PackageHost.PackageName, PackageHost.PackageVersion);

        while (PackageHost.IsRunning)
        {
            PackageHost.WriteInfo("Je push un long !");
            PackageHost.PushStateObject("DemoLong", DateTime.Now.Ticks);

            Thread.Sleep(PackageHost.GetSettingValue&lt;int&gt;("Interval"));
        }
    }
}
</pre>

Notre application console démarre bien le package au démarrage (Main) où le package est notre classe Program qui hérite de PackageBase (donc c’est un IPackage).

Au démarrage on écrit deux logs de type information qui indique notamment le nom du package, sa version, si il est démarré (IsRunning) et si il est connecté (IsConnected).

Ensuite on crée une boucle qui tournera tant que le package est démarré. A chaque itération on écrit un log, publie un StateObject avant de mettre le thread en pause avant la prochaine itération.

Le temps de pause est un setting de type “Int” que l’on a nommé “Interval”.

L’utilisateur pourra le configurer dans la configuration du package sur le serveur, mais pour être sûr d’avoir une valeur par défaut nous allons déclarer ce setting ainsi que sa valeur par default dans son manifest.

Editez le fichier PackageInfo.xml pour ajouter :

<pre class="lang:default decode:true">&lt;Setting name="Interval" type="Int32" defaultValue="5000" description="Interval en millisecondes" /&gt;</pre>

Ainsi même si le setting n’est pas déclaré sur le serveur ni dans le App.config local,  cette valeur sera par défaut égale à 5 secondes.

Vous pouvez démarrer le projet en mode debug (“F5” ou bouton “Start”) mais bien entendu il ne sera pas connecté à Constellation.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-101.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug dans Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-78.png" alt="Debug dans Constellation" width="424" height="216" border="0" /></a></p>

Pour débugger dans Constellation, vous trouverez une icone dans la toolbar (en en pressant Ctrl+Alt+F8) :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-161.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Debug dans Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-138.png" alt="Debug dans Constellation" width="424" height="100" border="0" /></a></p>

Vous pouvez également cliquez droit sur votre projet et dans le menu Constellation sélectionnez “Debug On Constellation” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-102.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug dans Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-79.png" alt="Debug dans Constellation" width="424" height="307" border="0" /></a></p>

<p align="left">Si vous n’avez pas encore configuré la Constellation utilisée pour le débogage, vous devriez voir une alerte vous invitant à configurer vos accès :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-103.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Enregistrement de la Constellation dans VS" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-80.png" alt="Enregistrement de la Constellation dans VS" width="424" height="205" border="0" /></a></p>

<p align="left">Cette fois ci, votre package tourne bien en étant connecté à votre Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-104.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Hello World Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-81.png" alt="Hello World Constellation" width="424" height="216" border="0" /></a></p>

<p align="left">Lancez maintenant votre Console Constellation, vous verrez en temps réel les logs de votre package.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-105.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Hello World Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-82.png" alt="Hello World Constellation" width="424" height="232" border="0" /></a></p>

<p align="left">Sur le StateObject Explorer, vous verrez également le StateObject “DemoLong” publié par le package “MonPremierPackage”.</p>

<p align="left">Notez que la sentinelle qui héberge le package est une sentinelle virtuelle nommée “Developer”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-106.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-83.png" alt="StateObject Explorer" width="424" height="140" border="0" /></a></p>

<p align="left">Vous pouvez cliquer sur “View” pour voir toutes les informations à propos de ce StateObject.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-107.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Visualisation des StateObjects" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-84.png" alt="Visualisation des StateObjects" width="227" height="244" border="0" /></a></p>

<p align="left">Pour finir, cliquez sur “Subscribe” pour vous abonner aux mises à jour.</p>

<h3 align="left">Publier son package dans Constellation</h3>

<p align="left">Nous voulons maintenant publier ce package et le déployer sur une des sentinelles de notre Constellation.</p>

<p align="left">Toujours dans la toolbar “Constellation”, cliquez sur “Publish Constellation package” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-162.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-139.png" alt="image" width="424" height="100" border="0" /></a></p>

<p align="left">Ou depuis le menu contextuel de votre projet, sélectionnez “Publish Constellation package” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-108.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publier un package Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-85.png" alt="Publier un package Constellation" width="424" height="324" border="0" /></a></p>

<p align="left">Vous pouvez soit le publier en local puis l’uploader manuellement via la Console par exemple, soit le publier directement dans votre Constellation.</p>

<p align="left">Pour cela, sélectionnez “Upload on Constellation Server”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-109.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publier un package Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-86.png" alt="Publier un package Constellation" width="424" height="212" border="0" /></a></p>

<p align="left">Sélectionnez ensuite l’adresse de votre Constellation et cliquez sur “Publish”.</p>

<p align="left">Un message vous informera de la réussite de la publication. Dans le panneau “Ouput” de Visual Studio vous retrouvez également le détail de la publication :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-110.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publier un package Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-87.png" alt="Publier un package Constellation" width="424" height="98" border="0" /></a></p>

<p align="left">Depuis la Console, sur la page “Package Repository” vous pourrez apercevoir votre package fraichement publié :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-111.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-88.png" alt="Package Repository" width="424" height="153" border="0" /></a></p>

<p align="left">Pour le déployer, vous devez éditer la configuration de votre Constellation pour ajouter le package à une (ou plusieurs) sentinelle. Vous pouvez également redéfinir le setting “Interval” que nous exploitations dans notre package (bien que nous avons défini une valeur par défaut).</p>

<pre class="lang:default decode:true">&lt;package name="MonPremierPackage" enable="true"&gt;
    &lt;settings&gt;
        &lt;setting key="Interval" value="2000" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>

<p align="left">Vous pouvez éditer la configuration du serveur Constellation directement depuis Visual Studio en cloquant sur “Edit Constellation Server configuration” (depuis la toolbar ou via le menu contextuel).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-163.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-140.png" alt="image" width="424" height="100" border="0" /></a></p>

<p align="left">La configuration sera automatiquement téléchargée du serveur et ouverte dans Visual Studio :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-164.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Edition de la configuration depuis VS" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-141.png" alt="Edition de la configuration depuis VS" width="424" height="230" border="0" /></a></p>

<p align="left">L’avantage est de pouvoir profiter de la validation du schéma XML et de l'IntelliSense de Visual Studio :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-165.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Intellisense" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-142.png" alt="Intellisense" width="420" height="163" border="0" /></a></p>

<p align="left">Dès que vous enregistrerez votre configuration, Visual Studio détectera les modifications et vous proposera d’uploader et de recharger la nouvelle configuration sur votre serveur Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-166.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Upload de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-143.png" alt="Upload de la configuration" width="242" height="144" border="0" /></a></p>

<p align="left">Vous pouvez aussi éditer la configuration de votre Constellation depuis la Console. Après avoir ajouter votre package, cliquez sur “Save &amp; Deploy” (pour informer les sentinelles des nouveaux packages) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-115.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Edition de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-92.png" alt="Edition de la configuration" width="424" height="233" border="0" /></a></p>

<p align="left">Vous pourrez alors voir dans les logs que le package est automatiquement téléchargé et déployé sur sa sentinelle et commence à pusher des StateObject toutes les 2 secondes comme nous l’avons indiqué dans sa configuration :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-113.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Console log temps réel" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-90.png" alt="Console log temps réel" width="424" height="207" border="0" /></a></p>

<p align="left">Sur la page “Packages” vous pouvez maintenant contrôler votre package, l’arrêter, le redémarrer ou le recharger (= déploiement d’une nouvelle version).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-114.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Contrôle des packages" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-91.png" alt="Contrôle des packages" width="424" height="126" border="0" /></a></p>

<h3 align="left">Next steps</h3>

<ul>
    <li>
<div align="left">Les <a href="/client-api/net-package-api/les-bases-des-packages-net/">bases des packages .NET</a></div></li>
    <li>
<div align="left"><a href="/client-api/net-package-api/settings/">Les Settings</a> : paramètres de configuration de vos packages</div></li>
    <li>
<div align="left"><a href="/client-api/net-package-api/pushstateobject/">Publier des StateObjects</a></div></li>
    <li>
<div align="left">Exposer vos méthodes dans Constellation grâce aux <a href="/client-api/net-package-api/messagecallbacks/">MessageCallbacks</a></div></li>
    <li>
<div align="left"><a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">Envoyer des messages &amp; Invoquer des méthodes</a></div></li>
    <li>
<div align="left"><a href="/client-api/net-package-api/consommer-des-stateobjects/">Consommer des StateObjects</a></div></li>
    <li>
<div align="left">Accéder au hub de contrôle depuis un package C# avec le <a href="/client-api/net-package-api/controlmanager/">ControlManager</a></div></li>
    <li>
<div align="left">Créer des <a href="/client-api/net-package-api/packages-ui-wpf-winform/">Packages UI</a> en Winform ou WPF</div></li>
</ul>