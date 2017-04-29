---
ID: 2939
post_title: 'L&rsquo;interface REST &ldquo;Management&rdquo;'
author: Sebastien Warin
post_date: 2016-09-23 22:19:38
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/rest-api/interface-rest-management/
published: true
post_modified: 2016-09-29 15:21:29
---
<p>L’interface REST “Management” (ou Management API) permet de l'édition du fichier de configuration :</p> <ul> <li>Gestion des sentinelles  <li>Gestion des packages  <li>Gestion des groupes  <li>Gestion des settings  <li>Gestion des RecoveryOptions  <li>Gestion des credentials et autorisations  <li>Gestion du Package Repository et upload de package  <li>Gestion de la licence  <li>Lire / Ecriture du fichier de configuration  <li>Récupération du numéro de version du serveur  <li>Rechargement et déploiement de la configuration </li></ul> <h3>Généralités</h3> <h4>Construction de l’URL</h4> <p>L’URL est :&nbsp; &lt;Root URI&gt;/rest/management/&lt;action&gt;?&lt;paramètres&gt;</p> <p>Partons du principal que votre Constellation est exposée en HTTP sur le port 8088 (sans path) :</p><pre class="lang:xml decode:true">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
&lt;/listenUris&gt;</pre>
<p>La “Root URI “ est donc “<strong>http://&lt;ip ou dns&gt;:8088/</strong>”.</p>
<p>Par exemple si nous sommes en local, nous pourrions écrire :</p><pre>http://localhost:8088/rest/management/xxxxx</pre>
<h4>Authentification</h4>
<p>Comme pour toutes les requêtes vers Constellation vous devez impérativement spécifier dans <u>les headers HTTP</u> <strong>ou</strong> dans <u>les paramètres de l’URL</u> (querystring) les paramètres “SentinelName”, “PackageName” et “AccessKey” pour l’authentification.</p>
<p>Dans le cas de l’API “Management”, la “SentinelName” est “Management” et le “PackageName” est le nom de votre choix que l’on considère comme un “FriendlyName”. Typiquement le “FriendlyName” est par exemple le nom de l’application/page qui se connecte.</p>
<p>L’AccessKey doit bien sûr être déclaré et activé sur le serveur. De plus elle doit avoir le droit&nbsp; d’utiliser l’API de Management, soit depuis la <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">Console Constellation</a> ou soit directement dans le <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">fichier de configuration</a> en ajoutant l’attribut “<a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">enableManagementAPI</a>”.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-121.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-116.png" width="240" height="192"></a></p>
<p>Ainsi chaque appel sera sous la forme :</p><pre>http://localhost:8088/rest/management/&lt;action&gt;?SentinelName=Management&amp;PackageName=&lt;Friendly name&gt;&amp;AccessKey=&lt;access key&gt;&amp;&lt;paramètres&gt;</pre>
<h4>Check Access</h4>
<p>Toutes les API REST de Constellation exposent une méthode en GET “CheckAccess” qui retourne un code HTTP 200 (OK). Cela permet de tester la connexion et l’authentification au serveur Constellation.</p>
<ul>
<li>Action : “CheckAccess” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/CheckAccess?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Récupérer la version du serveur</h4>
<p>Pour connaitre la version du serveur Constellation, vous pouvez invoquer l’action “/server/version”.</p>
<ul>
<li>Action : “/server/version” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/server/version?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h3>Edition du fichier de configuration</h3>
<h4>Lire le fichier de configuration</h4>
<p>Vous pouvez récupérer le contenu entier du fichier de configuration de votre Constellation en invoquant l’action ”/configuration”</p>
<ul>
<li>Action : “/configuration” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/configuration?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Ecrire le fichier de configuration</h4>
<p>Vous pouvez également réécrire le fichier de configuration en postant le nouveau contenu sur la même URL.</p>
<ul>
<li>Action : “/configuration” (POST) 
<li>Paramètres : aucun </li>
<li>Corps de la requête : le fichier de configuration XML</li><!--EndFragment--></ul>
<p>Le serveur vérifiera le bon format du fichier XML avant d’écraser le fichier de configuration existant par votre nouvelle version. Vous devrez ensuite recharger la configuration pour appliquer les modifications.</p>
<h3>Recharger et déployer la configuration</h3>
<p>Pour recharger le fichier de configuration vous devez invoquer l’action “/configuration/reload” :</p>
<ul>
<li>Action : “/configuration/reload ” (GET) 
<li>Paramètres : </li>
<ul>
<li>deployConfiguration (optionnel – par défaut “false”) : indique si l’ordre de mise à jour doit être envoyé aux sentinelles et packages de la Constellation</li></ul></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/configuration/reload?deployConfiguration=true&amp;SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h3>La License Constellation</h3>
<p>Vous pouvez récupérer les caractéristiques de la licence affectée à votre serveur en invoquant l’action&nbsp; “/license”.</p>
<ul>
<li>Action : “/license” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/license?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<p>Vous pouvez également charger une nouvelle licence en utilisant le verbe “POST” sur cette action et en plaçant le contenu de la nouvelle licence dans le corps de la requête.</p>
<h3>Gestion du Package Repository</h3>
<h4>Lister le Package Repository</h4>
<p>Pour récupérer la liste des packages contenus dans le Package Repository de votre Constellation :</p>
<ul>
<li>Action : “/packages” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/packages?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Récupérer un package</h4>
<p>Pour récupérer un package en particulier de votre Package Repository :</p>
<ul>
<li>Action : “/packages/{packageName}” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/packages/{packageName}?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Renommer un package</h4>
<p>Pour renommer un package de votre Package Repository vous devez poster un objet JSON contenant son nouveau nom :</p>
<ul>
<li>Action : “/packages/{packageName}” (POST) 
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : { "newName": newName }</li></ul>
<h4>Supprimer un package</h4>
<p>Pour supprimer package de votre Package Repository :</p>
<ul>
<li>Action : “/packages/{packageName}” (DELETE) 
<li>Paramètres : aucun </li></ul>
<h4>Récupérer l’icone d’un package</h4>
<p>Pour récupérer l’icone d’un package de votre Package Repository :</p>
<ul>
<li>Action : “/packages/{packageName}/icon” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Vous obtiendrez l’icone au format “PNG”.</p>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/packages/{packageName}/icon?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Récupérer le schéma XSD d’un paramètre de configuration d’un package</h4>
<p>Chaque package peut avoir des settings. Ces settings peuvent être de plusieurs types (types simples, objet JSON ou XML). Chaque package doit (ou devrait) <a href="/concepts/package-manifest/#Informations_sur_les_Settings_du_package">déclarer la liste des settings</a> qu’il utilise dans son “<a href="/concepts/package-manifest/">Package Manifest</a>”.</p>
<p>Si le setting est de type XML (“<em>ConfigurationSection</em>” ou “<em>XmlDocument</em>”) il peut être lié à un schéma XSD pour décrire le format XML du setting en question.</p>
<p>L’action ci-dessous permet justement de récupérer ce schema XSD pour un setting d’un package de votre Package Repository.</p>
<ul>
<li>Action : ”/packages/{package}/settings/{settingName}/xsd ” (GET) </li>
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/packages/{packageName}}/settings/{settingName}/xsd?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Télécharger un package vers votre Package Repository</h4>
<p>Vous pouvez ordonner à votre serveur Constellation de télécharger un fichier vers le Package Repository.</p>
<p>Pour cela vous utiliserez l’action “/download-package” en spécifiant l’URL du fichier à télécharger.</p>
<ul>
<li>Action : ”/download-package” (POST) </li>
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : { "url": url}</li><!--EndFragment--></ul>
<h4>Téléverser un package vers votre Package Repository</h4>
<p>Vous pouvez également envoyer un fichier vers votre Package Repository en utilisant l’action “/upload-package”.</p>
<ul>
<li>Action : ”/upload-package” (POST) </li>
<li>Paramètres : aucun </li>
<li>Corps de la requête (multipart/form-data) : les fichiers à enregistrer dans votre Package Repository.</li></ul>
<h3>Les RecoveryOptions</h3>
<p>Les <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_recoveryOptions">Recovery Options</a> spécifient les options de récupération en cas de crash d’un package sur vos sentinelles.</p>
<p>Chaque package peut avoir ses propres options mais par défaut il hérite des options “globales”.</p>
<p>Pour récupérer ces options “globales” :</p>
<ul>
<li>Action : ”/configuration/recoveryoptions” (GET) </li>
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/configuration/recoveryoptions?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<p>Vous pouvez également redéfinir ces options en postant sur cette même URI votre nouvel objet JSON :</p>
<ul>
<li>Action : ”/configuration/recoveryoptions” (POST) </li>
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : l’objet “RecoveryOptions” avec les nouvelles valeurs</li></ul>
<h3>Gestion des sentinelles</h3>
<h4>Lister les sentinelles</h4>
<p>Pour récupérer la liste des sentinelles de votre Constellation :</p>
<ul>
<li>Action : “/sentinels” (GET) 
<li>Paramètres : aucun </li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Ajouter ou modifier une sentinelle</h4>
<p>Pour ajouter ou modifier une sentinelle de votre Constellation :</p>
<ul>
<li>Action : “/sentinels” (POST) 
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : un objet JSON contenant deux propriétés :</li>
<ul>
<li>“Name”&nbsp; : le nom de la sentinelle</li>
<li>“Credential” : le nom du credential associé à cette sentinelle</li></ul></ul>
<h4>Supprimer une sentinelle</h4>
<p>Pour supprimer une sentinelle de votre Constellation :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}” (DELETE) 
<li>Paramètres : aucun</li></ul>
<h3>Gestion des packages et des settings</h3>
<h4>Lister les packages d’une sentinelles</h4>
<p>Pour récupérer la liste des packages (virtuels ou non) d’une sentinelle (virtuelle ou non) de votre Constellation :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels/{sentinelName}/packages?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Récupérer une instance d’un package</h4>
<p>Pour récupérer le détail d’une instance de package sur une sentinelle donnée :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels/{sentinelName}/packages/{packageName}?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Ajouter ou modifier une instance d’un package</h4>
<p>Pour ajouter ou modifier un package sur une sentinelle de votre Constellation :</p>
<ul>
<li>Action : “/sentinels/{sentinel}/packages” (POST) 
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : un objet JSON contenant cinq propriétés :</li>
<ul>
<li>“Name”: le nom de l’instance du package (obligatoire et unique) 
<li>“PackageFile” (facultatif) : le nom du fichier du package à utiliser dans le repository (<a href="https://developer.myconstellation.io/concepts/instance-package/">plus d’info</a>) 
<li>“Enable” (facultatif, par defaut <em>true</em>) : indique si le package est activé ou désactivé 
<li>“Credential” (facultatif, utilise par défaut du credential de sa sentinelle) : spécifie le <a href="https://developer.myconstellation.io/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">credential</a> que le package va utiliser pour s’authentifier sur le serveur Constellation 
<li>“AutoStart” (facultatif, par defaut <em>true</em>) : indique si le package doit automatiquement démarrer sur sa sentinelle ou juste être déployé (et donc démarré manuellement)</li></ul></ul>
<h4>Supprimer une instance d’un package</h4>
<p>Pour supprimer un package sur une sentinelle de votre Constellation :</p>
<ul>
<li>Action : “/sentinels/{sentinel}/packages/{packageName}” (DELETE) 
<li>Paramètres : aucun</li></ul>
<h4>Options de récupération d’une instance d’un package</h4>
<p>Pour récupérer les options de récupération d’une instance de package :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/recoveryoptions” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels/{sentinelName}/packages/{packageName}/recoveryoptions?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<p>Pour les redéfinir vous devez poster la nouvelle valeur de votre objet “RecoveryOptions” sur cette même URI.</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/recoveryoptions” (POST) 
<li>Paramètres : aucun</li>
<li>Corps de la requête (application/json) : l’objet “RecoveryOptions” avec les nouvelles valeurs</li></ul>
<h4>Groupes d’une instance d’un package</h4>
<p>Pour récupérer les <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Les_groupes">groupes d’une instance d’un package</a> :</p>
<ul><!--EndFragment--></ul>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/groups” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels/{sentinelName}/packages/{packageName}/groups?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<p>Utilisez cette même URI pour poster la nouvelle liste des groupes de votre instance :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/groups” (POST) 
<li>Paramètres : aucun</li>
<li>Corps de la requête (application/json) : la liste des groupes de votre instance</li><!--EndFragment--></ul>
<h4>Settings d’une instance d’un package</h4>
<p>Pour récupérer les settings d’une instance d’un package :</p>
<ul></ul>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/settings” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/sentinels/{sentinelName}/packages/{packageName}/settings?SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<p>Utilisez cette même URI pour poster le nouveau dictionnaire (clé/valeur) des settings :</p>
<ul>
<li>Action : “/sentinels/{sentinelName}/packages/{packageName}/settings” (POST) 
<li>Paramètres : aucun</li>
<li>Corps de la requête (application/json) : le dictionnaire (clé/valeur) des settings de votre instance</li></ul>
<h3>Gestion des credentials et autorisations</h3>
<h4>Lister les credentials</h4>
<p>Pour récupérer la liste des credentials de votre Constellation :</p>
<ul>
<li>Action : “/credentials” (GET) 
<li>Paramètres : </li>
<ul>
<li>includeAccessKeys (optionnel – par défaut “false”) : indique si vous souhaitez inclure les AccessKeys de chaque credentials</li></ul></ul>
<p>Exemple :</p><pre>http://localhost:8088/rest/management/credentials?includeAccessKeys=true&amp;SentinelName=Management&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h4>Ajouter ou modifier un credential </h4>
<p>Pour ajouter ou modifier un credential de votre Constellation :</p>
<ul>
<li>Action : “/credentials” (POST) 
<li>Paramètres : aucun </li>
<li>Corps de la requête (application/json) : un objet JSON contenant six propriétés :</li>
<ul>
<li>“Name” : le nom du credential</li>
<li>“AccessKey” : la clé d’accès du credential</li>
<li>“Enable” (<em>true</em> par défaut) : indique si le credential est activé ou désactivé </li>
<li>“EnableControlHub” (<em>false</em> par défaut) : indique si le credential peut se connecter sur le hub de contrôle (pour l’administration de la Constellation) 
<li>“EnableManagementAPI” (<em>false</em> par défaut) : indique si le credential peut se connecter à l’API de Management (pour l’édition de la configuration) 
<li>“EnableDeveloperAccess” (<em>false </em>par défaut) : indique si le credential peut utiliser la sentinelle virtuelle “Developer” pour debugger des packages (utilisé par le SDK Visual Studio)</li></ul></ul>
<h4>Supprimer un credential </h4>
<p>Pour supprimer un credential de votre Constellation :</p>
<ul>
<li>Action : “/credentials/{credentialName}” (DELETE) 
<li>Paramètres : aucun</li></ul>
<h4>Récupérer les autorisations d’un credential </h4>
<ul>
<li>Action : “/credentials/{credentialName}/authorizations” (GET) 
<li>Paramètres : aucun</li></ul>
<p>Vous récupèrerez un objet de type “<em>CredentialAuthorizations</em>“ sérialisé en JSON</p>
<h4>Définir les autorisations d’un credential </h4>
<ul>
<li>Action : “/credentials/{credentialName}/authorizations” (POST) 
<li>Paramètres : aucun</li>
<li>Corps de la requête (application/json) : l’objet <em>CredentialAuthorizations</em> sérialisé en JSON</li></ul>