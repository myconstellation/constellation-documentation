---
ID: 1522
post_title: Les bases des packages .NET
author: Sebastien Warin
post_date: 2016-03-21 14:21:31
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/les-bases-des-packages-net/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:53:05
---
<h3 align="left">Fonctionnement de base</h3>
<p align="left">Comme nous l’avons vu dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">le tutoriel précèdent</a>, un package est une application !</p>
<p align="left">Il faut impérativement appeler la méthode “<em>PackageHost.Start</em>” au démarrage de l’application, c’est à dire dans la méthode “Main” autrement, ce n’est pas un package Constellation mais une simple application .NET !</p>
<p align="left">Lorsque vous appelez la méthode “<em>PackageHost.Start</em>” vous devez impérativement transférer les arguments (args) et indiquer la classe de votre package (ici nommée “MonPackage”).</p>

<pre class="lang:c# decode:true">static void Main(string[] args)
{
    PackageHost.Start&lt;MonPackage&gt;(args);
}</pre>
<p align="left">La classe d’un package doit être une classe qui implémente l’interface “<em><u>IPackage</u></em>”. Cette interface définit trois méthodes :</p>

<ul>
 	<li>
<div align="left"><u>OnStart</u> qui sera invoqué lorsque le package a démarré</div></li>
 	<li>
<div align="left"><u>OnPreShutdown</u> : invoqué lorsque le package va s’arrêter (à ce stade votre package est toujours connecté à Constellation, vous pouvez encore pusher des StateObjects, envoyer des messages, écrire des logs sur le hub, etc..)</div></li>
 	<li>
<div align="left"><u>OnShutdown</u> : invoqué après le OnPreShutdown et après avoir fermé les connexions (plus aucune interaction avec la Constellation possible)</div></li>
</ul>
Pour vous éviter de devoir implémenter ces trois méthodes, votre classe peut hériter de la classe “<em>PackageBase</em>”. Cette classe abstraite implémente l’interface <u>IPackage</u> dans des méthodes virtuelles vides.

Libre à vous d’implémenter les méthodes que vous souhaitez !
<pre class="lang:c# decode:true">public class MonPackage : PackageBase
{
    //public override void OnStart()
    //{
    //}

    //public override void OnPreShutdown()
    //{
    //}

    //public override void OnShutdown()
    //{
    //}
}</pre>
<h3>Ecrire des logs</h3>
Pour écrire des logs depuis un package Constellation vous disposez des méthodes suivantes :
<ul>
 	<li>PackageHost.WriteDebug</li>
 	<li>PackageHost.WriteInfo</li>
 	<li>PackageHost.WriteWarn</li>
 	<li>PackageHost.WriteError</li>
</ul>
Chacune de ces méthodes écrivent un message qui peut être formaté avec des arguments à la manière d’un “string.Format” :
<pre class="lang:c# decode:true">PackageHost.WriteInfo("Je suis le package nommé {0} version {1}", PackageHost.PackageName, PackageHost.PackageVersion);</pre>
<u>Attention</u> : bien respecter les index dans le format de votre message sous peine d’avoir une erreur.

Pour éviter cela, vous pouvez depuis C# 6.0 (intégré nativement depuis Visual Studio 2015), utiliser les "<a href="https://msdn.microsoft.com/fr-fr/library/dn961160.aspx?f=255&amp;MSPPError=-2147217396">chaines interpolée</a>" en plaçant en début de chaine un "$" afin de pouvoir placer les arguments directement dans la chaine :
<pre class="lang:c# decode:true">PackageHost.WriteInfo($"Je suis le package nommé {PackageHost.PackageName} version {PackageHost.PackageVersion}");</pre>
<u>Note</u> : la méthode WriteDebug écrit seulement dans la console (mode debug local). Les logs de type “Debug” ne sont jamais envoyés dans la Constellation.
<h3>Propriétés du PackageHost</h3>
La classe centrale “PackageHost” expose différentes propriétés pour votre package Constellation.
<h4>PackageHost.IsRunning</h4>
Il s’agit d’un booléen qui indique si votre package est démarré. Il est affecté à “true” juste avant le OnStart et passe à “false” juste avant le OnPreShutdown.

Cela vous permet de connaitre l’état de votre package et sert notamment pour interrompre des boucles :
<pre class="lang:c# decode:true">while (PackageHost.IsRunning)
{
    // Do job
}</pre>
<h4>PackageHost.IsStandAlone</h4>
Cette propriété vous indique si votre package est démarré en “stand-alone”, c’est à dire lancé manuellement (en lançant le EXE) ou via Visual Studio (Debug on Constellation).

Si le package est démarré depuis une sentinelle Constellation, cette propriété est à “false”.
<h4>PackageHost.IsConnected</h4>
Vous indiques si le package est connecté ou non à la Constellation.
<h4>PackageHost.ConstellationHub</h4>
Retourne le proxy SignalR du hub Constellation.
<h4>PackageHost.HasControlManager</h4>
Indique si le package est connecté au hub de contrôle.
<h4>PackageHost.ControlManager</h4>
Retourne le client du hub de contrôle permettant de contrôler la Constellation. (<a href="/client-api/net-package-api/controlmanager/">plus d’information</a>)

Cette propriété est nulle si votre package ne demande pas de connexion au hub de contrôle. Vous devez donc tester si le client est disponible ou non avec la propriété ci-dessus :
<pre class="lang:c# decode:true">if (PackageHost.HasControlManager)
{
     PackageHost.ControlManager.xxxx();
}</pre>
<h4>PackageHost.Package</h4>
Retourne l’instance de votre package.
<h4>PackageHost.PackageDescriptor</h4>
Retourne l’instance du PackageDescriptor qui contient la description de vos MessagesCallbacks et StateObjects.
<h4>PackageHost.PackageName</h4>
Retourne le nom du package.

Le nom du package est défini dans le manifeste du package (PackageInfo.xml). Si aucune valeur n’est trouvée, le nom du package est le nom de l’assembly .NET du package.
<h4>PackageHost.PackageInstanceName</h4>
Retourne le nom de l’instance du package, c’est à dire le nom défini sur le serveur.

Un même package peut avoir plusieurs instances sur une même sentinelle mais avec des noms de package différents.
<h4>PackageHost.SentinelName</h4>
Retourne le nom de la sentinelle qui a démarré le package (si IsStandAlone = false).

Cette propriété est nulle si le package est démarré manuellement (en lançant le EXE) ou égale à “Developer” si lancé depuis Visual Studio.
<h4>PackageHost.PackageVersion</h4>
Retourne le numéro de version du package tel que défini dans le manifeste du package. Si il n’y a pas de manifeste, elle retourne le “PackageAssemblyVersion”.
<h4>PackageHost.PackageAssemblyVersion</h4>
Retourne le numéro de version de l’assembly du package.
<h4>PackageHost.ConstellationClientVersion</h4>
Retourne le numéro de version de la librairie Constellation utilisée par le package.
<h4>PackageHost.PackageManifest</h4>
Retourne le manifeste du package. (<a href="/concepts/package-manifest/">plus d’information</a>)
<h4>PackageHost.Settings</h4>
Retourne les settings du packages. (<a href="/client-api/net-package-api/settings/">plus d’information</a>)
<h3>Connexion à Constellation</h3>
Pour savoir si votre package est connecté à Constellation, vous pouvez interroger la propriété “PackageHost.IsConnected” comme indiqué ci-dessus.

Vous avez également l’évènement “ConnectionStateChanged” qui se déclenche lorsque le statut de la connexion change :
<pre class="lang:c# decode:true">PackageHost.ConnectionStateChanged += (s, e) =&gt;
{
    if (PackageHost.IsConnected)
    {
        // Récupération de la connexion
        // TODO : MAJ des états ?
    }
    else
    {
        // Perte de la connexion
    }
};
</pre>
Notez que lors de la première connexion cet événement n’est pas levé car Constellation n’a pas encore invoqué la méthode “OnStart” de votre package. Autrement dit le package se connecte d’abord puis “vous donnes la main”.

Donc si l’événement “ConnectionStateChanged“ est déclenché et que la propriété PackageHost.IsConnected = true, c’est qu’il s’agit forcement d’une reconnexion !

Dans ce cas, et en fonction de votre package, il est recommandé de “mettre à jour l’état” de votre package en re-pushant vos StateObjects.

<u>Exemple :</u>

Vous développez un package Constellation qui fait le lien vers votre système d’alarme.

Chaque zone de votre alarme est publiée dans Constellation sous forme de StateObject. Vous avez donc autant de StateObject que de zone et chacun de ces StateObjects indique l’état d'une zone de l'alarme (ouverte ou fermée).

Si pour une raison ou pour une autre, la connexion entre le package et la Constellation est rompue, l’activité sur les zones de l’alarme ne peut donc plus être mis à jours.

C’est pour cela que lorsque votre package parvient à se reconnecter il peut être nécessaire de republier tous les StateObjects sur Constellation pour être sûr d’avoir des StateObjects à jour par rapport à votre système d’alarme.