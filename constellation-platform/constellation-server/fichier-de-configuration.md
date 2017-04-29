---
ID: 2149
post_title: Le fichier de configuration
author: Sebastien Warin
post_date: 2016-08-09 13:52:30
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/fichier-de-configuration/
published: true
post_modified: 2017-04-27 11:01:57
---
Une Constellation est décrite en un seul fichier : le <strong>fichier de configuration Constellation</strong>.

Ce fichier se nomme “<em>Constellation.Server.exe.config</em>” et se trouve dans le répertoire d’installation du serveur (par défaut dans “<em>Program Files\Constellation Plateform\Server</em>").

Vous pouvez le modifier de différente manière :
<ul>
 	<li>En éditant ce fichier directement sur le serveur (avec un éditeur de texte type Notepad)</li>
 	<li>Via la Console Constellation (sur la page Configuration Editor)</li>
 	<li>Via Visual Studio</li>
 	<li>Via l’API de Management Constellation</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image75-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Edition de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image75_thumb.png" alt="Edition de la configuration" width="363" height="200" border="0" /></a></p>
Le fichier contient une balise globale “configuration” dans laquelle vous trouverez 4 sections :
<ul>
 	<li><u>configSections</u> : réservé au moteur .NET</li>
 	<li><u>constellation</u> : la section de configuration Constellation (anciennement nommée 'constellationSection')</li>
 	<li><u>startup</u> : réservé au moteur .NET</li>
 	<li><u>runtime</u> : réservé au moteur .NET</li>
</ul>
Vous ne devez en aucun cas modifier les sections réservées au moteur .NET sans savoir exactement ce que vous faites.

La section de “<strong>constellation</strong>” se décompose de la façon suivante :
<ul>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_listenUris">listenUris</a></u> : définit la configuration des URI d’écoute du serveur Constellation</li>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_fileServer">fileServer</a></u> : définit la configuration du serveur Web statique utilisé pour l’hébergement de la Console Constellation</li>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_recoveryOptions">recoveryOptions</a></u> : définit les options de récupération des packages</li>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_sentinels">sentinels</a></u> : définit la configuration des sentinelles (et des packages) de votre Constellation</li>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_settingsGroups">settingsGroups</a></u> : définit la configuration des groupes de “settings”</li>
 	<li><u><a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">credentials</a></u> : définit la configuration des credentials pour l’accès à votre Constellation</li>
</ul>
A chaque modification vous devez recharger la configuration soit via le hub de contrôle ou la Console Constellation (bouton “Reload Configuration”) ou soit en redémarrant le service Constellation Server.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma de configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-10.png" alt="Schéma de configuration" width="350" height="261" border="0" /></a></p>

<h3>Section “listenUris”</h3>
Cette section permet de définir les URI sur lesquels le serveur Constellation doit écouter.
<pre class="lang:xml decode:true">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
&lt;/listenUris&gt;</pre>
Ici le serveur répondra sur le port HTTP 8088 quelque soit le host (nom DNS ou adresse IP) car nous utilisons le wildcard “+” (pour plus d’information, <a href="https://msdn.microsoft.com/en-us/library/system.net.httplistener(v=vs.100).aspx#Anchor_6">lisez ceci</a>).

Vous pouvez configurer autant de “listenUri” que vous souhaitez. Par exemple :
<pre class="lang:xml decode:true">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
  &lt;uri listenUri="http://constellation.mydomain.net:8888/" /&gt;
  &lt;uri listenUri="http://+:8080/constellation/" /&gt;
  &lt;uri listenUri="https://+:8089/" /&gt;
&lt;/listenUris&gt;</pre>
Ici, le serveur Constellation répondra :
<ul>
 	<li>Aux requêtes sur le port HTTP 8088</li>
 	<li>Aux requêtes sur le port HTTP 8888 où le host est “constellation.mydomain.net” (en clair une requête <a href="http://&lt;ip&gt;:8888">http://&lt;ip&gt;:8888</a>  ne fonctionnera pas)</li>
 	<li>Aux requêtes sur le port HTTP 8080 quelque soit le host mais dont le path doit forcement démarrer par “/constellation/”</li>
 	<li>Aux requêtes sur le port HTTPS 8089 en utilisant un cryptage SSL (<a href="/constellation-platform/constellation-server/configuration-ssl/">plus d’information</a>)</li>
</ul>
Pour prendre en compte les modifications de cette section vous devez redémarrer le service Constellation.
<h3>Section “fileServer”</h3>
Cette section permet d’activer et configurer le “fileServer” de Constellation pour héberger des fichiers statiques (pages HTML, fichiers CSS, JS, images, etc..).

Cela sert principalement pour “auto-héberger”  la Console Constellation par le serveur Constellation lui-même.

En effet, la Console Constellation n’est ni plus ni moins qu’une application Web client-side, c’est à dire une série de pages HTML avec scripts JS et fichiers CSS/Image. Libre à vous de l’héberger sur vos serveurs Web mais afin de limiter les prérequis, le serveur Constellation peut lui-même héberger la console grâce au “fileServer” intégré.

La section “fileServer” ne contient que quatre attributs :
<pre class="lang:xml decode:true">&lt;fileServer enable="true" path="/WebConsole" localhostOnly="true" physicalPath="D:\App\Constellation\Console" /&gt;</pre>
<ul>
 	<li>“<u>enable</u>” : indique si le serveur de fichier statique sur le serveur Constellation doit être activé (<em>true</em> ou <em>false</em>).</li>
 	<li>“<u>path</u>” : le répertoire d’écoute (préfixé par la listenUri).</li>
 	<li>“<u>localhostOnly</u>” : indique si le fileServer répond seulement aux requêtes “locales” ou à toutes les requêtes (<em>true</em> ou <em>false</em>).</li>
 	<li>“<u>physicalPath</u>” : le répertoire physique</li>
</ul>
Dans l’exemple ci-dessus, le serveur de fichier est activé mais ne répondra qu’aux requêtes locales. Le serveur délivra les fichiers dans “D:\App\Constellation\Console” sur l’URL “&lt;listen_URI_du_serveur_Constellation&gt;/WebConsole” par exemple <a href="http://localhost:8088/WebConsole">http://localhost:8088/WebConsole</a> ou <a href="http://127.0.0.1:8088/WebConsole">http://127.0.0.1:8088/WebConsole</a> en partant du principe qu’il existe une listenUri “http://+:8088”.

Comme pour la section précédente (listenUris), il faut obligatoirement redémarrer le service Constellation Server pour prendre en compte ces modifications.
<h3>Section “recoveryOptions”</h3>
Les options de récupération (recoveryOptions) permettent de définir le comportement de la sentinelle en cas d’arrêt brutale de l’exécution d’un package (crash du package).

Chaque package peut avoir ses propres options de récupération mais cette section permet de définir les options “globales”, c’est à dire appliqué par défaut pour chaque package de la Constellation.

Les options de récupérations se résume en quatre propriétés :
<ul>
 	<li>“<u>restartAfterFailure</u>” (<em>true</em> par défaut) : définit si la sentinelle doit redémarrer un package en cas de crash de ce dernier</li>
 	<li>“<u>numberOfRetry</u>” (<em>3 </em>par défaut) : définit le nombre maximal de tentative de redémarrage d’un package</li>
 	<li>“<u>restCounterAfterMinutes</u>” (<em>15 </em>par défaut) : définit la période (en minute) au delà de laquelle le compteur d’erreur est réinitialisé</li>
 	<li>“<u>restartPackageAfterSeconds</u>” (<em>30 </em>par défaut) : définit le délai d’attente avant de redémarrer un package suite à un crash</li>
</ul>
Dans la configuration par défaut, si un package “crash” il est automatiquement redémarré 30 secondes après son crash dans la limite de 3 tentatives sachant qu’au bout de 15 minutes, le compteur de tentative est réinitialisé à 0.
<h3>Section “sentinels”</h3>
Dans cette section vous allez déclarer vos sentinelles (virtuelles ou non) dans lesquelles vous déclarerez les packages (virtuels ou non) qui eux même déclarons leurs settings et leurs groupes.

En clair, c’est cette section qui décrit votre “Constellation”.

Prenez l’exemple de la Constellation suivante :
<pre class="lang:xml decode:true">&lt;sentinels&gt;
  &lt;sentinel name="SENTINEL-NAME-DEMO" credential="Standard"&gt;
    &lt;packages&gt;    
      &lt;package name="HWMonitor" enable="true"&gt;&lt;/package&gt;          
      &lt;package name="DemoPackage" credential="ControlHubAccess" enable="true"&gt;
        &lt;settings&gt;
        &lt;import&gt;
          &lt;settingGroup groupName="GroupSettingsDemo" /&gt;
        &lt;/import&gt;
        &lt;setting key="IntSetting" value="42" /&gt;
        &lt;setting key="StringSetting" value="sample config value 2" /&gt;
        &lt;setting key="SettingXml"&gt;
          &lt;content&gt;
            &lt;demo type="test"&gt;
              &lt;list count="42"&gt;
              &lt;a&gt;123&lt;/a&gt;
              &lt;b&gt;test&lt;/b&gt;
              &lt;/list&gt;
            &lt;/demo&gt;
          &lt;/content&gt;
        &lt;/setting&gt;
        &lt;setting key="SettingJson"&gt;
          &lt;content&gt;
          &lt;![CDATA[
          {
            ListenPorts: [ 80, 443 ],
            EnableCaching : true,
            ServerProgramName: "Hypothetical WebServer 1.0",
            Websites: [
              {
                Path: "/srv/www/example/",
                Domain: "example.com",
                Contact: "admin@example.com"    
              },
              {
                Path: "/srv/www/somedomain/",
                Domain: "somedomain.com",
                Contact: "admin@somedomain.com"
              }
            ]
          }
          ]]&gt;
          &lt;/content&gt;
        &lt;/setting&gt;
        &lt;/settings&gt;
      &lt;/package&gt;
    &lt;/packages&gt;
  &lt;/sentinel&gt;
  &lt;sentinel name="ANOTHER-SENTINEL" credential="Standard"&gt;
    &lt;!-- ..... --&gt;
  &lt;/sentinel&gt;
&lt;/sentinels&gt;</pre>
Voici ce qu’elle décrit :
<ul>
 	<li>Nous avons deux sentinelles “SENTINEL-NAME-DEMO” et “ANOTHER-SENTINEL"</li>
 	<li>Les deux sentinelles utilisent le même <a href="/concepts/securite-accesskey-credential-authorization/">credential</a> “Standard” (déclaré dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">section credentials</a>).</li>
 	<li>Sur la première sentinelle est déployée deux packages :
<ul>
 	<li>“HWMonitor” : le package utilise le même credential que sa sentinelle (car non défini explicitement donc hérite du credential de sa sentinelle)</li>
 	<li>“DemoPackage” : ce package utilise le credential nommé “ControlHubAccess” et déclare les settings suivants :
<ul>
 	<li>IntSetting et StringSetting associés à des valeurs (numérique et string)</li>
 	<li>SettingXml et SettingJson associés à des contenus (XML et JSON)</li>
 	<li>Importe le groupes de settings nommé “GroupSettingsDemo” défini dans <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_settingsGroups">la section “settingsGroups”</a></li>
</ul>
</li>
</ul>
</li>
</ul>
<h4>Les sentinelles</h4>
Pour entrer dans le détail, vous allez déclarer dans la section “sentinels”, les sentinelles de votre Constellation que ce soit des sentinelles réelles ou virtuelles.

Pour cela, vous utiliser la balise “&lt;sentinel&gt;” qui comporte les attributs suivants :
<ul>
 	<li>“<u>name</u>” : le nom de la sentinelle (obligatoire et unique)</li>
 	<li>“<u>credential</u>” : le nom du credential (déclaré dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">section credentials</a>) à utiliser pour l’authentification (obligatoire)</li>
</ul>
La section “&lt;sentinel&gt;” contient une collection de “&lt;package&gt;” rangée dans la balise “&lt;packages&gt;”. Chaque package peut-être réel ou virtuel.
<h4>Les packages</h4>
L’élément “&lt;package&gt;” contient les attributs suivants :
<ul>
 	<li>“<u>name</u>”: le nom de l’instance du package (obligatoire et unique)</li>
 	<li>“<u>filename</u>” (facultatif) : le nom du fichier du package à utiliser dans le repository (<a href="/concepts/instance-package/">plus d’info</a>)</li>
 	<li>“<u>enable</u>” (facultatif, par defaut <em>true</em>) : indique si le package est activé ou désactivé</li>
 	<li>“<u>credential</u>” (facultatif, utilise par défaut du credential de sa sentinelle) : spécifie le <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">credential</a> que le package va utiliser pour s’authentifier sur le serveur Constellation</li>
 	<li>“<u>autostart</u>” (facultatif, par defaut <em>true</em>) : indique si le package doit automatiquement démarrer sur sa sentinelle ou juste être déployé (et donc démarré manuellement)</li>
</ul>
Un package peut contenir ensuite trois sous sections :
<ul>
 	<li>“<u>recoveryOptions</u>” : définit les options de récupération du package (par défaut, la sentinelle utilise les <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_recoveryOptions">options globales</a>)</li>
 	<li>“<u>settings</u>” : définit les paramètres/variables de configuration d’un package</li>
 	<li>“<u>groups</u>” : définit les groupes dans lesquels le package sera abonné</li>
</ul>
<h5>Les options de récupérations</h5>
La balise est la même que la <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_recoveryOptions">section globale</a>.

Par exemple si on ne souhaite pas que le package redémarre en cas de crash, on peut écrire :
<pre class="lang:xml decode:true">&lt;package name="MonPackage" enable="true"&gt;
  &lt;recoveryOptions restartAfterFailure="false" /&gt;
&lt;/package&gt;          
</pre>
<h5>Les paramètres de configuration</h5>
Chaque package peut définir des paramètres de configuration au niveau du serveur Constellation. Cela vous permet de changer ces paramètres directement depuis la Constellation qui se chargera de redescendre ces paramètres sur vos packages.

Il y a deux types de settings :
<ul>
 	<li>Les “Setting Value” : il s’agit d’un couple clé/value</li>
 	<li>Les “Setting Content”  : il s’agit d’un setting dont la valeur peut être un XML ou JSON</li>
</ul>
Dans l’exemple plus haut, le package “DemoPackage” contient à la fois des settings de type “value” et de type “content”.

Vous pouvez également regrouper des settings dans les groupes afin de les partager entre plusieurs package grâce aux <a href="/constellation-server/fichier-de-configuration/#Section_settingsGroups">settingsGroups</a>. Toujours dans l'exemple précédent, le package “DemoPackage” inclut le groupe “GroupSettingsDemo”, un groupe de settings décrit dans la section ”<a href="/constellation-server/fichier-de-configuration/#Section_settingsGroups">settingsGroups</a>”.

Pour plus d’information sur les settings, consultez l’article sur l’exploitation des <a href="/client-api/net-package-api/settings/">settings avec l’API .NET</a>.
<h5>Les groupes</h5>
Comme <a href="/concepts/messaging-message-scope-messagecallback-saga/#Message_Scope">vous le savez</a>, un message peut être envoyé à un groupe dans lequel des packages réels ou virtuels peuvent s’abonner.

Il y a deux manières d’abonner un package à un groupe :
<ul>
 	<li>Soit par le package lui même en invoquant la méthode “SubscribeToMessage” via les différentes API Constellation misent à disposition</li>
 	<li>Soit dans la configuration du package au niveau du serveur Constellation</li>
</ul>
Cela vous permet d’avoir la main sur l’appartenance des packages aux groupes que vous pouvez administrer sans devoir modifier vos packages (virtuels ou non).

La section “groups” sur un package permet de définir ces liens. Par exemple :
<pre class="lang:xml decode:true">&lt;package name="MonPackage" enable="true"&gt;
  &lt;groups&gt;
    &lt;group name="MonGroupeA" /&gt;
    &lt;group name="MonGroupeB" /&gt;
  &lt;/groups&gt;
&lt;/package&gt;          
</pre>
Ici le package “MonPackage” est abonné aux groupes “MonGroupeA” et “MonGroupeB”. En cas de modification des affectations aux groupes, il faudra redémarrer le package soit via vos applications en utilisant les API du hub de contrôle soit depuis la Console Constellation.
<h3>Section “settingsGroups”</h3>
Vous pouvez grouper des settings (Content ou Value) dans des groupes au niveau du serveur et importer ces groupes dans les settings de vos packages ou dans d’autres groupes.

Par exemple, créons un groupe pour “HWMonitorSettings” qui contient le setting “Interval” :
<pre class="lang:xml decode:true">&lt;settingsGroups&gt;
  &lt;group name="HWMonitorSettings"&gt;
    &lt;settings&gt;
      &lt;setting key="Interval" value="500" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
&lt;/settingsGroups&gt;</pre>
Ainsi pour chaque instance du package “HWMonitor”, vous pouvez importer le groupe “HWMonitorSettings” afin de centraliser la configuration de ce package :
<pre class="lang:xml decode:true">&lt;package name="HWMonitor"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="HWMonitorSettings" /&gt;
    &lt;/import&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
Vous pouvez importer des groupes dans des groupes sans limite.

C’est toujours la valeur du setting la plus proche du package qui gagne (surcharge).

Bien entendu, vous pouvez créer autant de groupe que vous voulez et chaque groupe peut contenir des SettingValue ou des SettingContent :
<pre class="lang:xml decode:true">&lt;settingsGroups&gt;
  &lt;group name="test"&gt;
    &lt;settings&gt;
      &lt;setting key="Demo1" value="Seb" /&gt;
      &lt;setting key="Demo2" value="123" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
  &lt;group name="test2"&gt;
    &lt;settings&gt;
      &lt;import&gt;
        &lt;settingGroup groupName="common" /&gt;
      &lt;/import&gt;
      &lt;setting key="Demo1" value="Sebastien" /&gt;
      &lt;setting key="Demo2" value="2015" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
  &lt;group name="common"&gt;
    &lt;settings&gt;
      &lt;setting key="MyStringSetting" value="This is a string" /&gt;
      &lt;setting key="MyBoolSetting" value="true" /&gt;
      &lt;setting key="MyXmlDocument"&gt;
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
    &lt;/settings&gt;
  &lt;/group&gt;
&lt;/settingsGroups&gt;
</pre>
Pour bien comprendre, imaginez le package suivant :
<pre class="lang:xml decode:true">&lt;package name="DemoPackage"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="test" /&gt;
      &lt;settingGroup groupName="test2" /&gt;
    &lt;/import&gt;
    &lt;setting key="numberTest" value="42" /&gt;
    &lt;setting key="Demo1" value="It’me" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
Ici cette instance du package “DemoPackage” définie au niveau du serveur (= sans prendre en compte les settings déclarés dans le fichier local et le manifeste) 7 settings :
<ul>
 	<li>numberTest = 42 (définit dans les settings du package)</li>
 	<li>Demo1 = “It’s me” (définit dans les settings du package, cette valeur écrase celle des groupes importés)</li>
 	<li>Demo2 = 2015 (définit par le groupe “test2”. Cette valeur écrase la valeur du groupe “test”, car le groupe “test2” est importé APRES le groupe “test”)</li>
 	<li>MyStringSetting, MyBoolSetting, MyXmlDocument et MyJsonObject définies par le groupe “common” (groupe importé dans le groupe “test2” lui même importé sur le package “DemoPackage”).</li>
</ul>
<h3>Section “credentials”</h3>
Comme évoqué dans <a href="/concepts/securite-accesskey-credential-authorization/">l’article lié à la sécurité</a>, tous les appels (http) au serveur Constellation doivent être authentifiés en utilisant une clé d’accès que l’on nomme “AccessKey”.

Ces clé d’accès sont liés à des credentials.

Dans la section “credentials” nous allons donc définir les différents credentials ainsi que les différentes autorisations et droits sur ces credentials.

Chaque credential comporte obligatoirement deux propriétés :
<ul>
 	<li>Le nom (unique) du credential</li>
 	<li>L’AccessKey</li>
</ul>
Ainsi que les propriétés suivantes :
<ul>
 	<li>“<u>enable</u>” (<em>true</em> par défaut) : indique si le credential est activé ou désactivé</li>
 	<li>“<u>enableControlHub</u>” (<em>false</em> par défaut) : indique si le credential peut se connecter sur le hub de contrôle (pour l’administration de la Constellation)</li>
 	<li>“<u>enableManagementAPI</u>” (<em>false</em> par défaut) : indique si le credential peut se connecter à l’API de Management (pour l’édition de la configuration)</li>
 	<li>“<u>enableDeveloperAccess</u>” (<em>false </em>par défaut) : indique si le credential peut utiliser la sentinelle virtuelle “Developer” pour debugger des packages (utilisé par le SDK Visual Studio)</li>
</ul>
Par exemple :
<pre class="lang:xml decode:true">&lt;credentials&gt;
  &lt;credential name="Standard" accessKey="fb1af70a46057dbf4f2ff3f43b6ec0c544b22749" /&gt;
  &lt;credential name="Administrator" accessKey="b363d6ccb86ef1418a72f9fea4b96975d8a1f2c7" enableControlHub="true" enableDeveloperAccess="true" enableManagementAPI="true" /&gt;
&lt;/credentials&gt;</pre>
Vous pouvez ensuite définir des <a href="/concepts/securite-accesskey-credential-authorization/#Les_Authorizations">autorisations</a> particulières sur un credential pour l’envoi de message, l’abonnement aux groupes de message ou encore l’interrogation de StateObject.

Pour cela il existe trois sous-sections :
<ul>
 	<li>“<u>stateObjects</u>” : autorisations spécifiques pour l’interrogation (Request ou Subscribe) des StateObjects</li>
 	<li>“<u>messages</u>” : autorisations spécifiques pour l’envoi de messages</li>
 	<li>“<u>groups</u>” : autorisations spécifiques pour l’abonnement aux groupes</li>
</ul>
<h4>Les autorisations sur l’interrogation des StateObjects</h4>
Par défaut un credential actif (<em>enable=true</em>) peut interroger (Request ou Subscribe) tous les StateObjects de votre Constellation sans restriction.

Pour modifier ce comportement vous pouvez utiliser les autorisation sur les StateObjects.

Par exemple :
<pre class="lang:xml decode:true">&lt;credential name="Standard" accessKey="fb1af70a46057dbf4f2ff3f43b6ec0c544b22749"&gt;
  &lt;authorizations&gt;
    &lt;stateObjects defaultAuthorization="Deny"&gt;
      &lt;authorization id="Paradox" authorization="Allow" packageName="Paradox" /&gt;
      &lt;authorization id="MonPC" authorization="Allow" sentinelName="PC-SEB" /&gt;
      &lt;authorization id="LesCPU" authorization="Allow" packageName="HWMonitor" name="/intelcpu/load/0" /&gt;
      &lt;authorization id="ServerRAM" authorization="Allow" sentinelName="SERVER" packageName="HWMonitor" name="/ram/load" /&gt;
    &lt;/stateObjects&gt;
  &lt;/authorizations&gt;
&lt;/credential&gt;</pre>
Ici le credential “Standard” ne pourra pas interroger les StateObjects de la Constellation (defaultAuthorization = Deny) sauf pour les exceptions suivantes :
<ul>
 	<li>Les StateObjects produits par le(s) instance(s) du package “Paradox”</li>
 	<li>Les StateObjects produits par les packages (quel qu’il soit) de la sentinelle “PC-SEB”</li>
 	<li>Les StateObjects nommés “/intelcpu/load/0” et produits par le package “HWMonitor” peu importe la sentinelle</li>
 	<li>Le StateObject nommé “/ram/load” produit par le package “HWMonitor” sur la sentinelle “SERVER”</li>
</ul>
Vous devez donc définir l’autorisation par défaut (defaultAuthorization) et optionnellement ajouter une ou plusieurs exceptions avec l’élément &lt;authorization&gt; dont les attributs sont les suivants :
<ul>
 	<li>“<u>id</u>” (obligatoire) : identifiant unique de l’autorisation</li>
 	<li>“<u>authorization</u>” (facultatif - <em>Allow</em> par défaut) : type de l’exception (<em>Allow</em> ou <em>Deny</em>)</li>
 	<li>“<u>sentinelName</u>” (facultatif – <em>*</em> par défaut) : sentinelle producteur du StateObject</li>
 	<li>“<u>packageName</u>” (facultatif – <em>*</em> par défaut) : package producteur du StateObject</li>
 	<li>“<u>name</u>” (facultatif – <em>*</em> par défaut) :  nom du StateObject</li>
</ul>
<h4>Les autorisations sur l’envoi de messages</h4>
Par défaut un credential actif (<em>enable=true</em>) peut envoyer un message à n’importe quel scope dans votre Constellation sans restriction.

Par exemple :
<pre class="lang:xml decode:true">&lt;credential name="Standard" accessKey="fb1af70a46057dbf4f2ff3f43b6ec0c544b22749"&gt;
  &lt;authorizations&gt;
    &lt;messages defaultAuthorization="Deny"&gt;
      &lt;authorization id="GroupA" authorization="Allow" scope="Group" args="A" /&gt;
      &lt;authorization id="DemoSeb" authorization="Allow" scope="Package" args="DemoPackage" /&gt;
      &lt;authorization id="TemperatureNest" authorization="Allow" scope="Package" args="Nest" messageKey="SetTargetTemperature"  /&gt;
    &lt;/messages&gt;
  &lt;/authorizations&gt;
&lt;/credential&gt;</pre>
Ici le credential “Standard” ne pourra pas envoyer de message dans la Constellation (defaultAuthorization = Deny) sauf pour les exceptions suivantes :
<ul>
 	<li>Messages envoyés au groupe “A”</li>
 	<li>Messages envoyés au package “DemoPackage”</li>
 	<li>Message “SetTargetTemperature” envoyé au package “Nest”</li>
</ul>
Vous devez donc définir l’autorisation par défaut (defaultAuthorization) et optionnellement ajouter une ou plusieurs exceptions avec l’élément &lt;authorization&gt; dont les attributs sont les suivants :
<ul>
 	<li>“<u>id</u>” (obligatoire) : identifiant unique de l’autorisation</li>
 	<li>“<u>scope</u>” (obligatoire) : type de <a href="/concepts/messaging-message-scope-messagecallback-saga/#Message_Scope">scope</a> pour le message (<em>All</em>, <em>Group</em>, <em>Package</em> ou <em>Sentinel</em>)</li>
 	<li>“<u>authorization</u>” (facultatif - <em>Allow</em> par défaut) : type de l’exception (<em>Allow</em> ou <em>Deny</em>)</li>
 	<li>“<u>args</u>” (facultatif) : les arguments du scope</li>
 	<li>“<u>messageKey</u>” (facultatif) :  la clé du message</li>
</ul>
<h4>Les autorisation sur l’abonnement aux groupes</h4>
Par défaut un credential actif (<em>enable=true</em>) peut s’abonner à n’importe quel groupe sans restriction.

Par exemple :
<pre class="lang:xml decode:true">&lt;credential name="Standard" accessKey="fb1af70a46057dbf4f2ff3f43b6ec0c544b22749"&gt;
  &lt;authorizations&gt;
    &lt;groups defaultAuthorization="Deny"&gt;
      &lt;authorization groupName="A" /&gt;
    &lt;/groups&gt;
  &lt;/authorizations&gt;
&lt;/credential&gt;</pre>
Ici le credential “Standard” ne pourra pas s’abonner à des groupes (defaultAuthorization = Deny) à l’exception du groupe “A”. Les attributs sont les suivants :
<ul>
 	<li>“<u>groupName</u>” (obligatoire) : le nom du groupe</li>
 	<li>“<u>authorization</u>” (facultatif - <em>Allow</em> par défaut) : type de l’exception (<em>Allow</em> ou <em>Deny</em>)</li>
</ul>