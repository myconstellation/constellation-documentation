---
ID: 1082
post_title: >
  Migrer Constellation 1.7 vers
  Constellation 1.8
author: Sebastien Warin
post_date: 2016-03-14 10:37:21
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/migrer-constellation-1-7-vers-constellation-1-8/
published: true
publish_post_category:
  - "19"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:16:29
---
Si vous avez une Constellation 1.7 déployée vous avez deux manière de migrer vers la 1.8 :

<ol>
    <li>Tout désinstaller et ré-installer la nouvelle installation</li>
    <li>Migrer l’existant</li>
</ol>

Pour la première solution, il suffit simplement de tout désinstaller depuis l’ajout et suppression de programmes Windows  sans oublier de supprimer le répertoire du serveur. Puis en suivant le <a href="/getting-started/installer-constellation/">guide de démarrage</a>, réinstaller la nouvelle version 1.8 en partant de zéro.

Bien entendu cette solution n’est valable que si votre Constellation n’est pas ou peu utilisée. Autrement si souhaitez migrer l’existant vers la nouvelle version 1.8 suivez ce guide.

<h3>Plan de migration</h3>

Pour mettre à jour une Constellation 1.7 vers la 1.8, voici le plan des étapes à réaliser :

<ol>
    <li>Mise à jour du serveur</li>
    <li>Mise à jour de la Console</li>
    <li>Mise à jour des pages Web (ou assimilés) exploitants l’API JavaScript</li>
    <li>Mise à jour du SDK</li>
    <li>Mise à jour des sentinelles</li>
    <li>Mise à jour des packages</li>
</ol>

<h3>Mise à jour du serveur</h3>

La première étape consiste à mettre à jour le serveur de façon à migrer votre Constellation 1.7 en 1.8.

L’installeur tout-en-un “Platform” ou l’installeur propre au serveur réalise la migration automatiquement (en tenant compte de votre configuration actuelle).

Prenons l’exemple d’une installation avec le serveur Constellation 1.7.6 déployé, la Console (nommée “Control Center” dans la version 1.7) et une sentinelle 1.7 sur laquelle est déployé le package HWMonitor également en version 1.7 :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-22.png" alt="image" width="354" height="249" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-23.png" alt="image" width="354" height="249" border="0" /></a></p>

Pour mettre à jour le serveur, la console et la sentinelle en une seule fois, je vous recommande de lancer l’installeur tout-en-un “Platform”.

L’installeur vous informera alors que ces trois composants seront mis à jour vers la version 1.8 :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-25.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-24.png" alt="image" width="354" height="275" border="0" /></a> <a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-26.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-25.png" alt="image" width="354" height="275" border="0" /></a></p>

<p align="left">A la fin de l’installation, dans les services Windows vous pourrez constater que les services du serveur et de la sentinelle ont bien été mis à jour :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-26.png" alt="image" width="424" height="64" border="0" /></a></p>

Vous pouvez également vous rendre sur l’URI de votre Constellation pour vérifier que le serveur Constellation 1.8 est bien démarré.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-28.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-27.png" alt="image" width="424" height="219" border="0" /></a></p>

<p align="left">Notez que toute votre configuration est restée échangé (clé d’accès, sentinelle, configuration des  packages, etc…).</p>

<h3>Mise à jour de la Console</h3>

Avec l’installeur “Platform” ci-dessus, la nouvelle Console (anciennement “Control Center”) est bien déployée.

Il vous suffit de réactualiser la page et vous retrouverez votre sentinelle (elle aussi migrée en 1.8 via l’installeur Plateform) et votre package HWMonitor (toujours fonctionnel dans votre nouvelle Constellation) :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-29.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-28.png" alt="image" width="354" height="249" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-30.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-29.png" alt="image" width="354" height="249" border="0" /></a></p>

<h4 align="left">Activer la page de login et l’accès à l’API de Management</h4>

Comme pour le serveur, l’installeur garde votre configuration actuelle lors de la mise à jour vers Constellation 1.8.

De ce fait, la configuration de la Console se trouve toujours dans le fichier “config.js” à la racine du répertoire d’installation de la Console.

Jusqu’à la version 1.7, ce fichier devait contenir une clé d’accès avec l’autorisation d’accès au hub de contrôle. Depuis la version 1.8, vous n’êtes plus obligé de définir en dur la clé d’accès dans ce fichier. Si la variable est vide, la console proposera alors une page de login.

L’idée est donc d’utiliser un couple login/password qu’on “hashera” pour l’utiliser en tant que clé d’accès (il est plus simple de mémoriser un login/password qu’une longue clé d’accès). Nous allons donc changer la clé d’accès (ou en créer une nouvelle) en utilisant ce procédé.

De plus dans Constellation 1.8, une nouvelle API est apparue pour l’administration du serveur : l’API de Management. Il vous faudra ajouter l’autorisation d’y accéder sur votre Access Key :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-31.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-30.png" alt="image" width="354" height="249" border="0" /></a></p>

<h5 align="left">Etape 1 : créez une Access Key à partir d’un login/password</h5>

<p align="left">Rendez-vous sur un outil de création de hash SHA1 tel que <a title="http://www.sha1.fr/" href="http://www.sha1.fr/">http://www.sha1.fr/</a> et créez un hash SHA1 du login concaténé avec votre mot de page.</p>

<p align="left">Par exemple, si mon login est “seb” et mon mot de passe “Password”, je crée le hash de “sebPassword” ce qui me retourne “567841343531a170817ac02941d644df12d7d3ae”.</p>

<h5 align="left">Etape 2 : déclarez votre nouveau credential</h5>

<p align="left">Editez le fichier de configuration du serveur (“Constellation.Server.exe.config” dans le répertoire d’installation du serveur) et ajoutez dans la section <em>&lt;credentials&gt;</em>:</p>

<pre class="lang:xml decode:true">&lt;credential name="MyAdminAccess" accessKey="567841343531a170817ac02941d644df12d7d3ae" enableControlHub="true" enableManagementAPI="true" enableDeveloperAccess="true"/&gt;</pre>

<p align="left">Le nom du credential n’a peu d’importance (il ne sert que pour votre organisation), l’AccessKey est le hash SHA1 généré à partir de notre login &amp; password, et n’oubliez pas d’autoriser l’accès au Control Hub (enableControlHub) et également à l’API de management (enableManagementAPI).</p>

<p align="left">SI vous souhaitez également utiliser cette clé d’accès pour débugger vos packages depuis le SDK Visual Studio, activez l’accès développeur (enableDeveloperAccess).</p>

<p align="left">Depuis la Console, cliquez sur le bouton “Reload Config” pour recharger la configuration avec votre nouveau credential :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-31.png" alt="image" width="354" height="160" border="0" /></a></p>

<h5 align="left">Etape 3 : mettez à jour le fichier de configuration de la Console</h5>

<p align="left">Editez maintenant le fichier de configuration de la Console (fichier “config.js” dans le répertoire d’installation de la config).</p>

<p align="left">Commencez par <strong>supprimer</strong> l’ajout de l’ancien menu en retirant les lignes :</p>

<pre class="lang:javascript decode:true">$(document).one('pagebeforecreate', function() {
    $.get("menu.html", function(data) {
        $.mobile.pageContainer.prepend(data);
        $("#nav-panel").panel();
        $("#menu-list").listview();
    });
});
</pre>

<p align="left">Puis <strong>effacer</strong> le contenu de la variable “constellationAccessKey” pour forcer l’affichage de la page de login.</p>

<p align="left">Au final, le contenu de votre fichier “config.js” doit être (en reprenant soin de spécifier l’URI de votre serveur Constellation en fonction de votre installation) :</p>

<pre lang="JavaScript">// Constellation Server URI
var constellationUri = "http://localhost:8088";
// Constellation AccessKey (w/ enableControlHub)
var constellationAccessKey = ""; // leave empty to enable the login page</pre>

<p align="left">Vous pouvez ensuite rafraichir votre page (Ctrl+F5 de préférence), la page de login s’affichera car aucune clé d’accès n’est définie dans le fichier “config.js”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-32.png" alt="image" width="354" height="249" border="0" /></a></p>

<p align="left">Pour vous connecter, utilisez votre login/password afin d’accéder à la console.</p>

<p align="left">SI vous avez activé l’accès à l’API de Management (enableManagementAPI), vous aurez un nouveau menu “Server Management” ajoutant des fonctionnalités pour la gestion du serveur, comme l’édition du fichier de configuration de serveur directement depuis la Console :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-34.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-33.png" alt="image" width="354" height="230" border="0" /></a></p>

<h4 align="left">Optionnel : auto-hébergement de la Console par le serveur Constellation</h4>

<p align="left">Depuis la version 1.8, le serveur Constellation est capable d’héberger des pages Web ce qui permet d’héberger la Console Constellation sans avoir besoin d’installer un serveur Web type IIS, Apache ou autre.</p>

<p align="left">Pour cela, vous devez simplement éditer la configuration du serveur pour activer le “File Server” :</p>

<pre class="lang:xml decode:true">&lt;fileServer enable="true" physicalPath="..\Console\" localhostOnly="true" path="/WebConsole"/&gt;</pre>

<p align="left">Passez l’attribut “enable” à true, indiquez le chemin physique du répertoire d’installation de la Console (par exemple : c:\inetpub\wwwroot), si vous voulez limiter ou non la console en local et l’URL relatif de la console (par default /webconsole/).</p>

<p align="left">Pour prendre en compte ces modifications vous devez redémarrer le serveur Constellation (dans la MMC des Services Windows).</p>

<p align="left">Aussi dans le fichier “config.js” de la Console, la variable “constellationUri” doit être vide pour indiquer que l’URI du serveur et la même que la console.</p>

<h3>Mise à jour des pages Web (ou assimilés) exploitants l’API JavaScript</h3>

Depuis la version 1.8, plusieurs modifications ont été apporté au hub de contrôle et la 1.8 se voit doter d’un nouveau hub : le hub de consommation (Consumer Hub).

Historiquement le hub de contrôle (Control Hub) permettait à la fois de contrôler la Constellation mais également d’interroger les StateObjects, envoyer et recevoir des messages, etc… car c’était le seul hub disponible pour le Web (Javascript).

Avec Constellation 1.8, le hub de contrôle est exclusivement dédié au contrôle de la Constellation (gestion des packages, récupération des statuts, des logs, reload de la configuration, etc…). Toutes les méthodes liées aux messages et aux StateObjects ont été déplacé dans un nouveau hub : le hub de consommation.

Une fois votre serveur migré en 1.8, toutes les pages Web (ou assimilés type applications Tizen ou Cordova qui utilisent l’API Javascript) ne fonctionneront plus tant que vous n’aurez pas migrés les API Javascript 1.8

<h4>Etape 1 : supprimez les anciennes librairies</h4>

Tout d’abord, commencez par désinstaller les packages Nuget “Constellation.CommonJS” (API Constellation JS) et “Constellation.CommonNG” (API Constellation AngularJS) car il s’agit des anciens packages pour Constellation 1.7.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-34.png" alt="image" width="424" height="144" border="0" /></a></p>

<p align="left">Si vous n’avez pas utilisé Nuget pour installer ces librairies, supprimez manuellement les fichier “Constellation.js” et “ngConstellation.js”.</p>

<h4 align="left">Etape 2 :  installez les nouvelles librairies</h4>

<p align="left">Depuis Nuget, installez le ou les nouveaux packages :</p>

<ul>
    <li>
<div align="left">“Constellation.Javascript” pour la libraire Constellation pour Javascript</div></li>
    <li>
<div align="left">“Constellation.Angular” pour la librairie Constellation pour AngularJS (sur couche)</div></li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-35.png" alt="image" width="424" height="128" border="0" /></a></p>

<p align="left">Dans vos pages HTML, changez les liens vers les nouveaux fichiers Javascript portant désormais le numéro de version de l’API.</p>

Remplacez :

<pre class="lang:javascript decode:true">&lt;script type="text/javascript" src="/constellation/Scripts/Constellation.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="/constellation/Scripts/ngConstellation.js"&gt;&lt;/script&gt;</pre>

par :

<pre class="lang:javascript decode:true">&lt;script type="text/javascript" src="/constellation/Scripts/Constellation-1.8.0.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="/constellation/Scripts/ngConstellation-1.8.0.js"&gt;&lt;/script&gt;</pre>

<h4>Etape 3 : modification dans l’API</h4>

Pour l’API Javascript (Constellaiton.Javascript), la méthode “<em>createConstellationClient</em>” est remplacée par “<em>createConstellationConsumer</em>” pour l’accès au hub de consommation ou “<em>createConstellationController</em>” pour l’accès au hub de contrôle.

Pour l’API AngularJS (Constellation.Angular), le module “<em>constellation</em>” à injecter dans votre contrôleur est remplacé par “<em>constellationConsumer</em>” et “<em>constellationController</em>” pour les deux hubs. La méthode “<em>intializeClient</em>” reste inchangée et est disponible sur les deux modules.

<h3>Mise à jour du SDK</h3>

Lancez tout simplement l’installeur tout en un “Platform” ou l’installeur du SDK.

Il procèdera à l’installation du VSIX pour Visual Studio 2012, 2013 et 2015.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-36.png" alt="image" width="424" height="330" border="0" /></a></p>

<h3>Mise à jour des sentinelles</h3>

Les sentinelles 1.7 peuvent fonctionner sans aucun soucis dans une Constellation 1.8 (à l’inverse, une sentinelle 1.8 ne peut pas se connecter à une Constellation 1.7).

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-37.png" alt="image" width="424" height="244" border="0" /></a></p>

Cependant, afin de bénéficier des nouveautés de la 1.8, il est conseillé de mettre à jour ses sentinelles.

Pour cela, vous devez soit lancer l’installeur “Plateform” si vous être sur la même machine que votre serveur Constellation, ou lancer l’installeur dédié à la sentinelle Service ou UI :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-38.png" alt="image" width="424" height="331" border="0" /></a></p>

<h3>Mise à jour des packages</h3>

Comme nous l’avons vu avec les API Javascript, les requêtes (Request) et abonnements (Subscribe) sur les StateObjects et l’envoi et la réception de messages ne sont plus disponibles sur le hub de contrôle.

Ainsi les packages .NET ou Python qui utilisaient ces fonctionnalités comme les [StateObjectLink] ne pourront plus fonctionner une fois votre serveur Constellation migré en 1.8. Vous devez donc impérativement mettre à jour la librairie Constellation 1.8 pour ces packages.

Autrement, pour tous les packages Constellation 1.7.x qui n’exploitent pas les StateObjects (hormis le Push) et n’utilisent pas les fonctionnalités du hub de contrôle, pourront fonctionner sans problème sur une Constellation 1.8.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-39.png" alt="image" width="354" height="132" border="0" /></a></p>

<p align="left">Au démarrage vous aurez simplement un warning (et seulement si la sentinelle est en version 1.8.x) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-40.png" alt="image" width="354" height="86" border="0" /></a></p>

<p align="left">Pensez toutefois à mettre à jour la libraire cliente Constellation pour bénéficier des nouveautés de la 1.8.</p>

<h4>Mise à jour du package Constellation 1.8</h4>

Comme pour les API Javascript, le package NuGet de l’API .NET a changé d’identifiant. Vous devez donc commencer par désinstaller le package Constellation 1.7 nommé “Constellation.Common” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-42.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-41.png" alt="image" width="424" height="196" border="0" /></a></p>

Puis, toujours depuis NuGet, installez le package “Constellation” 1.8 :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-43.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-42.png" alt="image" width="424" height="140" border="0" /></a></p>

<h4>Les breaking changes</h4>

Une fois vos projets migrés sur la nouvelle librairie 1.8, il y a plusieurs “breaking changes” que vous devrez corriger pour pouvoir compiler votre package. Retrouvez ci-dessous la liste des changements bloquants (et non la liste des nouveautés de l’API 1.8).

<h5>Le Namespace</h5>

Premièrement l’espace de nom “<em>Constellation.Host</em>” devient “<em>Constellation.Package</em>”. Vous devez donc changer les différents “using” de votre code.

<pre class="lang:csharp">using Constellation.Package;</pre>

<h5>Le PackageManifest</h5>

L’attribut "<em>RestoreStateObjects</em>" est renommé en "<em>RequestLastStateObjectsOnStart</em>" dans le manifest (PackageInfo.xml) de votre package. Il permet d’indiquer au serveur Constellation de vous envoyer les derniers StateObjects du package quand il démarre (avant la purge).

De même, l’évènement "<em>RestoreStateObjects</em>" est lui renommé en "<em>LastStateObjectsReceived</em>" sur la classe statique “<em>PackageHost</em>” :

<pre class="lang:csharp decode:true">PackageHost.LastStateObjectsReceived += (s, e) =&gt;
{
     PackageHost.WriteInfo("Recu {0} StateObject(s)", e.StateObjects.Count);
};</pre>

<h5>Les Settings</h5>

La version 1.7 de Constellation a introduit les “SettingContents” permettant de mettre du XML dans la déclaration d’un setting. Il fallait alors utiliser la méthode "<em>GetSettingContent&lt;TConfigurationSection&gt;</em>" où TConfigurationSection devait être une ConfigurationSection.

En 1.8, cette méthode a été renommé en "<em>GetSettingAsConfigurationSection&lt;TConfigurationSection&gt;</em>".

De plus de nouvelles méthodes sont apparues :

<pre class="lang:csharp decode:true">MaSection section = PackageHost.GetSettingAsConfigurationSection&lt;MaSection&gt;("MaSection");
dynamic configJson = PackageHost.GetSettingAsJsonObject("MonObjectJson");
MonObject objetJson = PackageHost.GetSettingAsJsonObject&lt;MonObject&gt;("MonObjectJson");
XmlDocument xml = PackageHost.GetSettingAsXmlDocument("MonXml");</pre>

Lorsque le package est démarré en mode “debug”, depuis Visual Studio, les SettingValues sont lues depuis les &lt;appSettings&gt; de votre App.config. Avec la 1.8, les &lt;appSettings&gt; sont abandonnés. Vous devez définir une section “constellationSettings” qui prend la même structure que les settings sur le serveur avec le support des &lt;content&gt;.

Pour plus d’information, consultez la page dédiée à la <a href="#">configuration des packages</a>.

<h5>Les StateObjects</h5>

<ul>
    <li>Désormais la propriété “Metadatas” d’un StateObject est dictionnaire de String (clé) / Object (valeur) (et non plus String/String).</li>
    <li>La classe  StateObject est maintenant dans le namespace racine “Constellation”</li>
    <li>Les classes StateObjectNotifier et StateObjectCollectionNotifier ainsi que l’attribut StateObjectLink et les EventsArgs associés appartiennent au namespace “Constellation.Package”</li>
    <li>Les notions  des StateObjectsLink &amp; Notifier (méthodes et events) sont déplacés dans le PackageHost et non plus dans le ControlManager.</li>
</ul>

Il n’y a donc plus besoin de définir l’attribut “EnableControlHub” à true pour ajouter des StateObjectsLinks dans vos packages.

<h5>Les messages</h5>

La méthode <em>PackageHost.AttachMessageCallbacks</em> est renommée en <em>PackageHost.RegisterMessageCallback</em>. A noter qu’elle est (toujours) implicitement appelée au démarrage du package sur votre package “PackageBase”.

Les méthodes <em>PackageHost.CreateScope</em> &amp; <em>PackageHost.CreateSaga</em> ont été supprimées. Pour créer un scope vous devez directement utiliser la classe MessageScope.

Par exemple :

<pre class="lang:csharp decode:true">MessageScope scope = MessageScope.Create("MonPackageCible");
MessageScope scope2 = MessageScope.Create(MessageScope.ScopeType.Group, "GroupA", "GroupB");</pre>

<u>Breaking change</u> : ScopeType.Groups est devenu ScopeType.Group (au singulier)

Pour récupérer le proxy dynamic, vous devez utiliser la méthode d’extension “GetProxy” et non plus la propriété “Proxy” :

<pre class="lang:csharp decode:true">dynamic proxy = scope.GetProxy();</pre>

Par exemple, pour envoyer le message “Demo” au package “DemoPackage” :

<pre class="lang:csharp decode:true">MessageScope.Create("DemoPackage").GetProxy().Demo("Un argument ici !");</pre>

Pour envoyer ce message dans une saga, vous devez attacher à un callback de retour avec la méthode d’extension “OnSagaResponse” :

<pre class="lang:csharp decode:true">MessageScope.Create("DemoPackage").OnSagaResponse(reponse =&gt;
{
     PackageHost.WriteInfo("Recu reponse de {0}", MessageContext.Current.Sender.FriendlyName);
}).GetProxy().Demo("Un argument ici !");</pre>

Par défaut, la variable de retour est un dynamic, mais vous pouvez aussi spécifier le type de retour :

<pre class="lang:csharp decode:true">MessageScope.Create("DemoPackage").OnSagaResponse&lt;MonObject&gt;(reponse =&gt;
{
     // Reponse est de type MonObject
     PackageHost.WriteInfo("Recu reponse de {0}", MessageContext.Current.Sender.FriendlyName);
}).GetProxy().Demo("Un argument ici !");</pre>

Pour finir, il ne s’agit pas d’un breaking change mais d’une nouveauté : vous pouvez également envoyer un message dans une saga de façon “awaitable” :

<pre class="lang:csharp decode:true">Task&lt;dynamic&gt; reponse = MessageScope.Create("DemoPackage").GetProxy().Demo&lt;MonObject&gt;("Un argument ici !");</pre>

Retrouvez plus d’information sur <a href="#">l’envoi de message et les sagas sur cette page</a>.