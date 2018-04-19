---
ID: 1491
post_title: >
  Accéder au hub de contrôle avec le
  ControlManager
author: Sebastien Warin
post_date: 2016-03-21 11:24:49
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/controlmanager/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:56:05
---
Le <a href="/concepts/les-differents-hubs-et-interfaces-rest-du-serveur-constellation/">hub de contrôle</a> permet d’accéder à différentes fonctions de pilotage de votre Constellation.

Voyons en détail comment contrôler votre Constellation depuis un package C#/.NET.
<h3>Accéder au hub de contrôle</h3>
Tout d’abord il faut déclarer que votre package souhaite un accès au hub de contrôle. Pour cela vous devez ajouter l’attribut “EnableControlHub” à <em>true</em> dans le <a href="/concepts/package-manifest/">manifeste </a>de votre package (fichier <em>PackageInfo.xml</em>).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-151.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-128.png" alt="image" width="424" height="258" border="0" /></a></p>
Ensuite, il faut que votre package se connecte à votre Constellation avec une “Access Key” qui possède l’autorisation d’accès au hub de contrôle.

Il faut ajouter l’attribut “enableControlHub” à true sur un de vos credentials et affecter ce credential à votre package :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-152.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-129.png" alt="image" width="424" height="233" border="0" /></a></p>
Ainsi votre package pourra accéder au hub de contrôle de contrôle. Vous devriez d’ailleurs toujours tester la propriété “<em>HasControlManager</em>” pour gérer les erreurs où l’accès au hub de contrôle n'est possible.
<pre class="lang:C# decode:true">if (PackageHost.HasControlManager)
{
    PackageHost.WriteInfo("ControlHub OK");
}
else
{
    PackageHost.WriteError("ControlHub access denied !");
}</pre>
<h3>Suivre les sentinelles et leurs statuts</h3>
Il y a deux manières d’obtenir la liste des sentinelles enregistrées dans votre Constellation :
<ul>
 	<li><u>RequestSentinelsList</u> : vous retourne la liste des sentinelles en une seule fois par l’événement “<em>SentinelsListUpdated</em>”</li>
 	<li><u>RequestSentinelUpdates</u> : vous retourne l’état de chaque sentinelle dans l’événement “<em>SentinelUpdated</em>” (cet événement est levé pour chaque sentinelle)</li>
</ul>
En principe pour récupérer la liste des sentinelles de votre Constellation, vous utiliserez toujours la première méthode.

Par exemple :
<pre class="lang:C# decode:true">PackageHost.ControlManager.SentinelsListUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Il y a {0} sentinelle(s)", e.Sentinels.Count);
    foreach (SentinelInfo sentinel in e.Sentinels)
    {
        PackageHost.WriteInfo("Sentinelle '{0}' (Plateforme: {1}) =&gt; IsConnected = {2}",
            sentinel.Description.SentinelName,
            sentinel.Description.Platform,
            sentinel.IsConnected);
    }
};
PackageHost.ControlManager.RequestSentinelsList();</pre>
Vous pouvez également vous abonner en temps réel aux mises à jour des sentinelles de votre Constellation.

Pour cela il faut activer la réception des mises à jour :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReceiveSentinelUpdates = true;</pre>
Puis attacher un handler sur l’événement “SentinelUpdated” qui se produit à chaque mise à jour d’une sentinelle :
<pre class="lang:C# decode:true">PackageHost.ControlManager.SentinelUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Sentinelle '{0}' (Plateforme: {1}) =&gt; IsConnected = {2}",
        e.Sentinel.Description.SentinelName,
        e.Sentinel.Description.Platform,
        e.Sentinel.IsConnected);
};</pre>
<h3>Superviser les packages</h3>
<h4>Récupérer les packages d’une sentinelle</h4>
Pour récupérer la liste des packages qui sont déployés sur une sentinelle vous devez attacher un handler sur l’événement “<em>PackagesListUpdated</em>” et appeler la méthode “<em>RequestPackagesList</em>” en spécifiant le nom de la sentinelle.

Pour exemple :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PackagesListUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Il y a {0} package(s) sur la sentinelle {1}", e.Packages.Count, e.SentinelName);
};
PackageHost.ControlManager.RequestPackagesList("MA-SENTINELLE");</pre>
Chaque “PackageInfo” contient différentes informations sur l’instance du package (description du package, état du package, état de la connexion, version du package, etc…) :
<pre class="lang:C# decode:true">foreach (PackageInfo package in e.Packages)
{
    PackageHost.WriteInfo("Package '{0}' - Etat = {1}", package.Package.Name, package.State);
}
</pre>
<h4>Suivre l’état des packages</h4>
Vous pouvez aussi vous abonnez aux mises à jour des états des packages :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReceivePackageState = true;</pre>
Vous pourrez ensuite être notifier de tout changement d’état de vos packages dans votre Constellation :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PackageStateUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Le package '{0}' est maintenant dans l'état {1}", e.PackageName, e.State);
};</pre>
<h4>Suivre la consommation des packages</h4>
Vous pouvez aussi récupérer la consommation des ressources (CPU et RAM) de vos packages :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReceivePackageUsage = true;</pre>
Par exemple, on pourrait écrire un warning si un package consomme plus de 50% du CPU :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PackageUsageUpdated += (s, e) =&gt;
{
    if (e.CPU &gt; 50) // Package consommant plus de 50% du CPU à un instant T
    {
        PackageHost.WriteWarn("Le package {0} consomme maintenant plus de 50% !!! CPU={1}% RAM={2}ko",
            e.PackageName, e.CPU, e.RAM / 1024);
    }
};</pre>
<h4>Contrôler les packages</h4>
Vous pouvez démarrer un package sur une sentinelle avec la méthode “<em>StartPackage</em>” :
<pre class="lang:C# decode:true">// Démrrage du package "MonPremierPackage" sur la sentinelle "MON-PC"
PackageHost.ControlManager.StartPackage("MON-PC", "MonPremierPackage");</pre>
Ou encore l’arrêter avec la méthode “<em>StopPackage</em>” :
<pre class="lang:C# decode:true">// Arret du package "MonPremierPackage" sur la sentinelle "MON-PC"
PackageHost.ControlManager.StopPackage("MON-PC", "MonPremierPackage");</pre>
Pour redémarrer (<em>Stop</em> puis <em>Start</em>) un package sur une sentinelle donnée, utilisez la méthode “<em>RestartPackage</em>” :
<pre class="lang:C# decode:true">// Redémarrage du package "MonPremierPackage" sur la sentinelle "MON-PC"
PackageHost.ControlManager.RestartPackage("MON-PC", "MonPremierPackage");</pre>
Enfin pour mettre à jour un package, utilisez la méthode “<em>ReloadPackage</em>”.

Un “reload” stoppe le package, ordonne à la sentinelle de re-télécharger le package sur le serveur, le déploie en local avant de le démarrer.
<pre class="lang:C# decode:true">// Mise à jour du package "MonPremierPackage" sur la sentinelle "MON-PC"
PackageHost.ControlManager.ReloadPackage("MON-PC", "MonPremierPackage");</pre>
<h3>Accéder aux logs en temps réel</h3>
Pour récupérer les logs produits par vos sentinelles et packages dans votre Constellation, activez la propriété suivante :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReceivePackageLog = true;</pre>
Il suffit ensuite d’attacher handler sur l’événement “<em>LogEntryReceived</em> “.

Vous recevrez en paramètre un objet “<em>LogEntry</em>” qui contient toutes les informations sur un log (le message, la date, le couple Sentinelle/Package à l’origine du log et la sévérité du message).

Par exemple :
<pre class="lang:C# decode:true">PackageHost.ControlManager.LogEntryReceived += (s, e) =&gt;
{
    if (e.LogEntry.PackageName != PackageHost.PackageInstanceName) // Ne pas prendre les logs de ce package pour éviter la boucle ;)
    {
        // On log des logs, juste pour l'exemple ;)
        PackageHost.WriteInfo("Je reçois un log daté du {0} du package {1} sur la sentinelle {2} de type {3}." +
            "Message = {4}",
            e.LogEntry.Date.ToString(),
            e.LogEntry.SentinelName,
            e.LogEntry.PackageName,
            e.LogEntry.Level,
            e.LogEntry.Message);
    }
};</pre>
Notez qu’ici on logue dans Constellation (“WriteInfo”) les logs des autres packages (d’où le &lt;if&gt;). Ca n’a pas de sens, c’est juste pour l’exemple !
<h3>Récupérer la description des packages</h3>
Comme vous le savez, tous les <a href="/client-api/net-package-api/messagecallbacks/#Decrire_ses_MessageCallbacks">MessageCallbacks ainsi que les types personnalisés</a> utilisés en entrée ou en sortie sont décrits dans un <a href="/concepts/messaging-message-scope-messagecallback-saga/">PackageDescriptor</a> tout comme les <a href="/client-api/net-package-api/stateobjects/#Decrire_les_types_de_StateObjects">types personnalisés des StateObjects</a>.

En vous connectant au hub de contrôle vous pouvez récupérer le <em>PackageDescriptor</em> de chaque package. Par exemple :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PackageDescriptorUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Pour le package {0}, il y a {1} MessageCallback(s) associés à " +
                         "{2} type(s) ainsi que {3} type(s) de StateObject",
        e.PackageName,
        e.Descriptor.MessageCallbacks.Count,
        e.Descriptor.MessageCallbackTypes.Count,
        e.Descriptor.StateObjectTypes.Count);
};
PackageHost.ControlManager.RequestPackageDescriptor("MonPremierPackage");</pre>
<h3>Purger les StateObjects</h3>
Pour supprimer tous les StateObjects d’un package déployé une sentinelle :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PurgeStateObjects("MON-PC", "MonPremierPackage");</pre>
Vous pouvez aussi spécifier le nom du StateObject, pour supprimer un StateObject en particulier (identifié par son nom) :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PurgeStateObjects("MON-PC", "MonPremierPackage", "Demo");</pre>
Ou encore, tous les StateObjects d’un type particulier d’une instance de package :
<pre class="lang:C# decode:true">PackageHost.ControlManager.PurgeStateObjects("MON-PC", "MonPremierPackage", type:"TypeDemo");</pre>
<h3>Recharger la configuration</h3>
Vous pouvez recharger et déployer la configuration Constellation en invoquant la méthode :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReloadServerConfiguration();</pre>
Vous pouvez également passer un booléen pour indiquer si il faut déployer la configuration (c’est à dire pousser la configuration sur chaque sentinelle et package). Par défaut, la valeur est à “true” (la configuration est déployée).

Autrement :
<pre class="lang:C# decode:true">PackageHost.ControlManager.ReloadServerConfiguration(false);</pre>