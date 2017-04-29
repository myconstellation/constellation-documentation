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
post_modified: 2016-08-11 15:13:17
---
<h3 align="left">Fonctionnement de base</h3> <p align="left">Comme nous l’avons vu dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">le tutoriel précèdent</a>, un package est une application !</p> <p align="left">Il faut impérativement appeler la méthode “<em>PackageHost.Start</em>” au démarrage de l’application, c’est à dire dans la méthode “Main” autrement, ce n’est pas un package Constellation mais une simple application !</p> <p align="left">Lorsque vous appelez la méthode “<em>PackageHost.Start</em>” vous devez impérativement transférer les arguments (args) et indiquer la classe de votre package (ici nommée “MonPackage”).</p><pre class="lang:c# decode:true">static void Main(string[] args)
{
    PackageHost.Start&lt;MonPackage&gt;(args);
}</pre>
<p align="left">La classe d’un package doit être une classe qui implémente l’interface “<u>IPackage</u>”. Cette interface définie trois méthodes :</p>
<ul>
<li>
<div align="left"><u>OnStart</u> qui sera invoqué lorsque le package a démarré</div>
<li>
<div align="left"><u>OnPreShutdown</u> : invoqué lorsque le package va s’arrêter (à ce stade votre package est toujours connecté à Constellation, vous pouvez encore pusher des StateObjects, envoyer des messages, écrire des logs sur le hub, etc..)</div>
<li>
<div align="left"><u>OnShutdown</u> : invoqué après le OnPreShutdown et après avoir fermé les connexions (plus aucune interaction avec la Constellation possible)</div></li></ul>
<p>Pour vous éviter de devoir implémenter ces trois méthodes, votre classe peut hériter de la classe “PackageBase”. Cette classe abstraite implémente l’interface <u>IPackage</u> dans des méthodes virtuelles vides.</p>
<p>Libre à vous d’implémenter les méthodes que vous souhaitez !</p><pre class="lang:c# decode:true">public class MonPackage : PackageBase
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
<p>Pour écrire des logs depuis un package Constellation vous disposez des méthodes :</p>
<ul>
<li>PackageHost.WriteDebug 
<li>PackageHost.WriteInfo 
<li>PackageHost.WriteWarn 
<li>PackageHost.WriteError </li></ul>
<p>Chacune de ces méthodes écrivent un message qui peut être formaté avec des arguments à la manière d’un “string.Format” :</p><pre class="lang:c# decode:true">PackageHost.WriteInfo("Je suis le package nommé {0} version {1}", PackageHost.PackageName, PackageHost.PackageVersion);</pre>
<p><u>Attention</u> : bien respecter les index dans le format de votre message sous peine d’avoir une erreur.</p>
<p><u>Note</u> : la méthode WriteDebug écrit seulement dans la console (mode debug local). Les logs de type “Debug” ne sont jamais envoyés dans la Constellation.</p>
<h3>Propriétés du PackageHost</h3>
<p>La classe centrale “PackageHost” expose différentes propriétés intéressantes .</p>
<h4>PackageHost.IsRunning </h4>
<p>Il s’agit d’un booléen qui indique si votre package est démarré. Il est affecté à “true” juste avant le OnStart et passe à “false” juste avant le OnPreShutdown.</p>
<p>Cela vous permet de connaitre l’état de votre package et sert notamment pour interrompre des boucles :</p><pre class="lang:c# decode:true">while (PackageHost.IsRunning)
{
    // Do job
}</pre>
<h4>PackageHost.IsStandAlone</h4>
<p>Cette propriété vous indique si votre package est démarré en “stand-alone”, c’est à dire lancé manuellement (en lançant le EXE) ou via Visual Studio (Debug on Constellation).</p>
<p>Si le package est démarré depuis une sentinelle Constellation, cette propriété est à “false”.</p>
<h4>PackageHost.IsConnected</h4>
<p>Vous indiques si le package est connecté ou non à la Constellation. </p>
<h4>PackageHost.ConstellationHub</h4>
<p>Retourne le proxy SignalR du hub Constellation.</p>
<h4>PackageHost.HasControlManager</h4>
<p>Indique si le package est connecté au hub de contrôle.</p>
<h4>PackageHost.ControlManager</h4>
<p>Retourne le client du hub de contrôle permettant de contrôler la Constellation. (<a href="/client-api/net-package-api/controlmanager/">plus d’information</a>)</p>
<p>Cette propriété est nulle si votre package ne demande la connexion au hub de contrôle. Vous devez donc tester si le client est disponible ou non avec la propriété ci-dessus :</p><pre class="lang:c# decode:true">if (PackageHost.HasControlManager)
{
     PackageHost.ControlManager.xxxx();
}</pre>
<h4>PackageHost.Package</h4>
<p>Retourne l’instance de votre package.</p>
<h4>PackageHost.PackageDescriptor</h4>
<p>Retourne l’instance du PackageDescriptor qui contient la description de vos MessagesCallbacks et StateObjects.</p>
<h4>PackageHost.PackageName</h4>
<p>Retourne le nom du package.</p>
<p>Le nom du package est défini dans le manifeste du package (PackageInfo.xml). SI aucune valeur n’est trouvée, le nom du package est le nom de l’assembly .NET du package.</p>
<h4>PackageHost.PackageInstanceName</h4>
<p>Retourne le nom de l’instance du package, c’est à dire le nom défini sur le serveur.</p>
<p>Un même package peut avoir plusieurs instances sur une même sentinelle mais avec des noms de packages différents.</p>
<h4>PackageHost.SentinelName</h4>
<p>Retourne le nom de la sentinelle qui a démarré le package (si IsStandAlone = false).</p>
<p>Cette propriété est nulle si le package est démarré manuellement (en lançant le EXE) ou égale à “Developer” si lancé depuis Visual Studio.</p>
<h4>PackageHost.PackageVersion</h4>
<p>Retourne le numéro de version du package tel quel défini dans le manifeste du package. Si il n’y a pas de manifeste, on retourne le “PackageAssemblyVersion”.</p>
<h4>PackageHost.PackageAssemblyVersion</h4>
<p>Retourne le numéro de version de l’assembly du package.</p>
<h4>PackageHost.ConstellationClientVersion</h4>
<p>Retourne le numéro de version de la librairie Constellation utilisée par le package.</p>
<h4>PackageHost.PackageManifest</h4>
<p>Retourne le manifeste du package. (<a href="/concepts/package-manifest/">plus d’information</a>)</p>
<h4>PackageHost.Settings</h4>
<p>Retourne les settings du packages. (<a href="/client-api/net-package-api/settings/">plus d’information</a>)</p>
<h3>Connexion à Constellation</h3>
<p>Pour savoir si votre package est connecté à Constellation, vous pouvez interroger la propriété “PackageHost.IsConnected” comme indiqué ci-dessus.</p>
<p>Vous avez également l’évènement “ConnectionStateChanged” qui se déclenche lorsque le statut de la connexion change :</p><pre class="lang:c# decode:true">PackageHost.ConnectionStateChanged += (s, e) =&gt;
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
<p>Notez que si qu’à la première connexion l’évènement n’est pas levé car Constellation n’a pas encore invoqué la méthode “OnStart” de votre package. Autrement dit le package se connecte d’abord puis “vous donnes la main”.</p>
<p>Donc si l’évènement “ConnectionStateChanged“ est déclenché et que PackageHost.IsConnected = true, c’est qu’il s’agit forcement d’une reconnexion !</p>
<p>Dans ce cas, et en fonction de votre package, il est recommandé de “mettre à jour l’état” de votre package en re-pushant vos StateObjects.</p>
<p><u>Exemple :</u></p>
<p>Vous développez un package Constellation qui fait le lien vers votre système d’alarme.</p>
<p>Chaque zone de votre alarme est publiée dans Constellation sous forme de StateObject. Vous avez donc autant de StateObject que de zone et chacun de ces StateObjects indique l’état de la zone (ouverte ou fermée).</p>
<p>Si pour une raison ou pour une autre, la connexion entre le package et la Constellation est rompue, l’activité sur les zones de l’alarme ne peut donc plus être mis à jours.</p>
<p>C’est pour cela, que lorsque votre package parvient à se reconnecter il peut être nécessaire de republier tous les StateObjects sur Constellation pour être sûr d’avoir des StateObjects à jour par rapport à votre système d’alarme (par exemple).</p>