---
ID: 2143
post_title: 'La sécurité : AccessKey, Credential et Authorization'
author: Sebastien Warin
post_date: 2016-08-09 13:30:20
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/securite-accesskey-credential-authorization/
published: true
publish_post_category:
  - "12"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:19:23
---
Tous les appels (HTTP) au serveur Constellation doivent être authentifiés. Pour cela, nous utilisons une clé d’accès que l’on nomme “AccessKey”.
<h3>L’AccessKey</h3>
Chaque requête au serveur Constellation comporte soit dans les paramètres de l’URL (querystring) soit dans les entêtes (headers) HTTP trois informations :
<ul>
 	<li><u>SentinelName</u> : le nom de la sentinelle</li>
 	<li><u>PackageName</u> : le nom du package</li>
 	<li><u>AccessKey</u> : la clé d’accès</li>
</ul>
Les paramètres “SentinelName” et “PackageName” permettent d’identifier le package qui réalise la requête car un package (virtuel ou non) est toujours associé à une sentinelle (virtuelle ou non).

En fait le couple “SentinelName/PackageName” forme l’identifiant unique du package (nommé <a href="/concepts/instance-package-versioning-et-resolution/">instance de package</a>) dans la Constellation et l’AccessKey peut être comparé au “mot de passe”.

Une sentinelle (virtuelle ou non) déclarée dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/">configuration de votre Constellation</a> doit obligatoirement être associée à une clé d’accès et les packages (virtuels ou non) déclarés sur une sentinelle utilisent implicitement la même clé d’accès ou bien peuvent redéfinir une clé spécifique.

Il y a quelques cas particuliers :
<ul>
 	<li>Lorsqu'une sentinelle produit des logs, son “PackageName” est “Sentinel”</li>
 	<li>Pour les consommateurs ou contrôleurs, le “SentinelName” est soit “Consumer” ou soit “Controller” et le “PackageName” sert de “FriendlyName” pour les logs</li>
 	<li>Dans le cas de l’utilisation de la fonction de “Debug on Constellation” depuis le SDK VisualStudio, le “SentinelName” est “Developer”</li>
</ul>
<h3>Les Credentials</h3>
Dans le <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">fichier de configuration de votre Constellation</a>, on ne déclare pas l’AccessKey directement sur une sentinelle ou un package.

En effet une sentinelle est associée à un credential et un package utilise implicitement le même credential que sa sentinelle ou bien redéfini son propre credential.

Un credential comporte obligatoirement deux propriétés :
<ul>
 	<li>Le nom (unique) du credential</li>
 	<li>L’AccessKey</li>
</ul>
Pour authentifier une requête, le serveur Constellation regardera alors si l’AccessKey de la requête correspond bien à celle déclarée pour le credential utilisé par le couple Sentinel/Package.

Un credential peut définir les propriétés suivantes :
<ul>
 	<li><u>Enable</u> : indique si le credential est activé ou désactivé</li>
 	<li><u>EnableControlHub</u> : indique si le credential peut se connecter sur le hub de contrôle (pour l’administration de la Constellation)</li>
 	<li><u>EnableManagementAPI</u> : indique si le credential peut se connecter à l’API de Management (pour l’édition de la configuration)</li>
 	<li><u>EnableDeveloperAccess</u> : indique si le credential peut utiliser la sentinelle virtuelle “Developer” pour debugger des packages (utilisé par le SDK Visual Studio)</li>
</ul>
Vous pouvez gérer les credentials depuis <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">la Console Constellation</a> ou depuis <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">le fichier de configuration</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Manage Credential" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-11.png" alt="Manage Credential" width="354" height="141" border="0" /></a></p>

<h3>Login et AccessKey</h3>
L’AccessKey doit être une chaîne de caractère assez longue et sera utilisée un peu comme un mot de passe pour l’authentification.

Cependant il est assez compliqué de retenir un AccessKey surtout comparé à un couple “login / password” que nous utilisons au quotidien.

C’est pourquoi la Console Constellation, les installeurs Constellation ou encore le SDK Constellation propose d’utiliser un login/password pour générer un AccessKey.

La formule est très simple : <strong>AccessKey = sha1(login + password)</strong>. En clair une clé d’accès peut être obtenue par le hash SHA1 de la concaténation du login avec un mot de passe.

Par exemple, si mon login est “<u>admin</u>” et que mon mot de passe est “<u>P@</u><u>ssw0rd</u>” alors ma clé d’accès (AccessKey) sera <u>a5761adf00e4060f05fab7017b32905cc9e5d540</u> (= le hash SHA1 de “adminP@ssw0rd”).
<h3>Les Authorizations</h3>
Par défaut un credential actif (<em>enable=true</em>) à le droit d’interroger (<em>Request</em> ou <em>Subscribe</em>) tous les StateObjects de votre Constellation sans restriction ou d'envoyer des messages à n’importe quel scope ou encore de s’abonner n’importe quel groupe.

Imaginez maintenant que vous réalisez une page Web en <a href="/client-api/javascript-api/">Javascript</a> connectée à votre Constellation pour afficher les valeurs de quelques StateObjects en temps réel ou encore pour envoyer des messages pour contrôler certains dispositifs.

Dans le cas présent, si il s’agit d’une page JavaScript, le code est “client-side”, donc n’importe quelle personne ayant accès à cette page pourra visualiser le code source et récupérer l’AccessKey utilisée.

En clair si vous développez un petit morceau de Javscript pour afficher en temps réel la consommation de votre CPU (StateObject produit par le package HWMonitor par exemple) sur votre site Web, n’importe qui pourrait récupérer votre clé d’accès et suivre en temps réel le statut de votre alarme, votre courbe de poids, la température de votre chambre, le volume de votre ampli, etc… Ou pire encore, envoyer différents messages dans votre Constellation pour éteindre des machines, allumer des lumières, ouvrir la porte de garage, augmenter le chauffage, etc…

Un concept "d'autorisation” a été introduite dans la version 1.8 de Constellation pour restreindre les droits sur une clé d’accès (AccessKey) sur les éléments suivants :
<ul>
 	<li>L’interrogation des StateObjects</li>
 	<li>L’envoi de message (invocation des MessageCallback)</li>
 	<li>L’abonnement à des groupes de message</li>
</ul>
De ce fait, si vous souhaitez afficher par exemple la consommation de votre CPU de telle machine, vous allez créer un credential qui n’aura que le droit (autorisation) de visualiser ce StateObject ni plus ni moins.

Plus d’information, consultez l'article dédié à <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">la configuration des autorisations</a>.