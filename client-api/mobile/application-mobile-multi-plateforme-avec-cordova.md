---
ID: 2335
post_title: 'Cr&eacute;er une application mobile multi-plateforme connectée à Constellation avec Cordova'
author: Sebastien Warin
post_date: 2016-08-19 09:28:47
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/mobile/application-mobile-multi-plateforme-avec-cordova/
published: true
post_modified: 2017-05-04 16:37:19
---
La plateforme <a href="http://cordova.apache.org/">Apache Cordova</a> (anciennement PhoneGap) permet de créer des applications mobiles (Android, Windows Phone, iPhone, Blackberry, …) en utilisant les technologies Web (HTML, JS, CSS).
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="cordova_logo_normal_dark" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/cordova_logo_normal_dark.png" alt="cordova_logo_normal_dark" width="240" height="71" border="0" /></p>
Couplée à la <a href="/client-api/javascript-api/">libraire JavaScript Constellation</a>, il devient très facile de créer une application mobile pour votre smartphone ou tablette connectée à Constellation.
<h3>Installer Cordova</h3>
Cordova utilise nodejs, l'installation est donc très simple et rapide :
<ol>
 	<li>Installer <a href="http://nodejs.org/">NodeJS</a> si vous ne l'avez pas déjà</li>
 	<li>Depuis le terminal (ou cmd pour windows) lancez la commande :</li>
</ol>
<pre>npm install -g cordova</pre>
Et voila Cordova est installé, mais il vous faut maintenant installer les SDK spécifiques aux plateformes que vous comptez utiliser.

Pour plus d’information, quelques ressources :
<ul>
 	<li><a href="https://www.grafikart.fr/tutoriels/cordova/apache-cordova-installation-432">Installer et configurer Cordova</a></li>
 	<li><a href="http://cordova.apache.org/#getstarted">Guide de démarrage Cordova</a></li>
 	<li><a href="https://cordova.apache.org/docs/fr/latest/guide/platforms/android/">Guide pour la plate-forme Android</a></li>
</ul>
<h3>Créer un projet Cordova</h3>
Lancez une invite de commande Windows et tapez la ligne suivante pour créer votre projet :
<pre class="">cordova create Demo</pre>
Ensuite ajoutez le ou les plateformes cibles de votre application. Par exemple, développons une application pour Android :
<pre>cd Demo
cordova platform add android</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-154.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-149.png" alt="image" width="350" height="311" border="0" /></a></p>
Pour démarrer votre application sur l’émulateur Android ou sur votre terminal Android (connecté en USB) :
<pre>cordova run android</pre>
<h4>Tester votre application sur l’émulateur</h4>
En utilisant le “<em>Android Virtual Device (AVD) Manager</em>” créez un device que l’on nommera ici “SmartphoneDemo” sous Android 5.1.1 :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-165.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-160.png" alt="image" width="250" height="364" border="0" /></a></p>
<p align="left">Vous pouvez ensuite cliquer sur le bouton “Start” pour lancer le device virtuel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-160.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-155.png" alt="image" width="254" height="161" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-161.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-156.png" alt="image" width="250" height="432" border="0" /></a></p>
Enfin lancez la commande suivante pour déployer et démarrer votre application dans l’émulateur :
<pre>cordova run android</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-162.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-157.png" alt="image" width="354" height="229" border="0" /></a></p>

<h4>Debugger votre application avec l’inspecteur Chrome</h4>
Dans un nouvel onglet Chrome, rendez-vous sur <a title="chrome://inspect/#devices" href="chrome://inspect/#devices">chrome://inspect/#devices</a>. Vous pourrez alors inspecter votre application Cordova :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-163.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-158.png" alt="image" width="350" height="220" border="0" /></a></p>
<p align="left">Ainsi vous profiterez de tous les outils de debugging Web pour votre application mobile.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-164.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-159.png" alt="image" width="354" height="211" border="0" /></a></p>

<h4>Déployer votre application sur votre téléphone</h4>
En connectant votre téléphone Android en USB ( sans oublier d’activer le debugging USB dans les paramètres de votre téléphone) vous pouvez, toujours avec la commande :
<pre>cordova run android</pre>
… lancer votre application sur votre téléphone et utiliser l’inspecteur Chrome pour la debugger à distance.

Autrement vous pouvez aussi utiliser la commande :
<pre>cordova build android</pre>
… pour générer le fichier APK, l’équivalent de l’installeur de votre application, qu’il faudra copier et lancer sur votre téléphone ou tablette Android.
<h3>Connecter votre application Cordova à Constellation</h3>
La structure d’une application Cordova est la suivante :
<ul>
 	<li>Une page “index.html”, la page de démarrage de votre application</li>
 	<li>Des dossiers css, img et js pour ranger respectivement les feuilles de style CSS, les images et les scripts JS</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-166.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-161.png" alt="image" width="350" height="293" border="0" /></a></p>
<p align="left">En clair votre application mobile se développe comme une véritable application Web. Vous pouvez ensuite <a href="http://cordova.apache.org/docs/en/latest/guide/cli/#add-plugins">ajouter des plugins</a> pour accéder fonctionnalités du téléphone depuis votre code JavaScript (téléphonie, SMS, contacts, GPS, Bluetooth, Wifi, appareil photo, etc..).</p>

<h4 align="left">Ajouter les librairies Constellation</h4>
<p align="left">Comme il s’agit d’une application Web, connecter une application Cordova à Constellation revient à <a href="/getting-started/connectez-vos-pages-web-constellation/">connecter une page Web à Constellation</a>.</p>
<p align="left">Vous pouvez donc inclure les libraires en utilisant les CDN (en prenant soin de spécifier le schème explicitement, par exemple ‘https’) :</p>

<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
</pre>
<p align="left">Par contre afin d’améliorer les performances de votre application mobile, il est recommandé d’embarquer ces libraires dans votre application.</p>
<p align="left">Pour cela téléchargez (<em>clique-droit &gt; Enregistrer sous …</em>) dans le dossier “js” les librairies JavaScript suivantes :</p>

<ul>
 	<li>
<div align="left"><a href="https://code.jquery.com/jquery-2.2.4.min.js">https://code.jquery.com/jquery-2.2.4.min.js</a></div></li>
 	<li>
<div align="left"><a href="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js">https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</a></div></li>
 	<li>
<div align="left"><a href="https://cdn.myconstellation.io/js/Constellation-1.8.1.min.js">https://cdn.myconstellation.io/js/Constellation-1.8.1.min.js</a></div></li>
</ul>
Puis ajoutez-les dans votre page “index.html” de cette façon:
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="js/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="js/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;</pre>
<h4 align="left">Connecter votre application</h4>
<p align="left">Tout d’abord dans le ficher “config.xml” à la racine de votre projet Cordova, ajoutez ces quelques lignes :</p>

<pre class="lang:html5 decode:true">&lt;allow-intent href="ws://"/&gt;
&lt;allow-intent href="*" /&gt;
&lt;access origin="*" subdomains="true" /&gt;</pre>
<p align="left">Cela permet d’autoriser le container Cordova à accéder à des ressources externes comme le serveur Constellation !</p>
<p align="left">Aussi dans votre page principale,<em> index.html</em>, modifiez le “Content-Security-Policy” par :</p>

<pre class="lang:html5 decode:true">&lt;meta http-equiv="Content-Security-Policy" content="default-src *; style-src 'self' 'unsafe-inline'; script-src * 'unsafe-inline' 'unsafe-eval';"&gt;</pre>
<p align="left">On autorise ainsi notre page à exploiter des services et scripts externes.</p>
<p align="left">Dans le fichier de script principal (<em>app.js</em>), créez un client “Consumer” en spécifiant l’adresse de votre serveur Constellation, une clé d’accès et un friendly name :</p>

<pre class="lang:javascript decode:true">constellation: $.signalR.createConstellationConsumer("http://constellation.monServer.com:8088", "MaCle123!", "Demo Cordova"),</pre>
<p align="left">Dans la fonction “onDeviceReady” lancez la connexion au serveur Constellation par la ligne :</p>

<pre class="lang:javascript decode:true">app.constellation.connection.start();</pre>
<p align="left">Pour finir dans la fonction “<em>initialize</em>” attachez un handler sur le changement d’état de la connexion Constellation. L’idée étant d’afficher le texte “Connected to Constellation” à la place du message “Device is Ready” du sample Cordova.</p>

<pre class="lang:javascript decode:true">app.constellation.connection.stateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    $('.received').text('connected to constellation');
  }
});</pre>
<p align="left">En démarrant l’application dans l’émulateur, votre application se connectera au hub “Consumer” de votre Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-167.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-162.png" alt="image" width="250" height="432" border="0" /></a></p>
<p align="left">Vous pouvez maintenant envoyer des messages et invoquer des MessageCallbacks de vos différents packages, recevoir des messages pour que les autres clients de la Constellation puissent invoquer des méthodes de votre application mobile par exemple, interroger ou suivre en temps réel des StateObjects publiés par les packages de votre Constellation.</p>
<p align="left">Quelques exemples ci-après ...</p>

<h4 align="left">Afficher en temps réel des StateObjects</h4>
<p align="left">Reprenons l’exemple du StateObject “/intelcpu/0/load/0” produit par le package “HWMonitor” et correspondant à la consommation de votre CPU.</p>
<p align="left">Pour cela nous allons enregistrer <a href="/client-api/javascript-api/consommer-constellation-api-javascript/#Consommer_des_StateObjects">StateObjectLink</a> sur ce StateObject en ciblant une sentinelle en particulier. Ici la sentinelle UI de mon ordinateur se nomme “PC-SEB_UI”.</p>
<p align="left">Vous pouvons écrire :</p>

<pre class="lang:javascript decode:true">app.constellation.connection.stateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    $('.received').text('connected to constellation');
    app.constellation.client.registerStateObjectLink("PC-SEB_UI", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
      $("#cpu").text(Math.round(so.Value.Value * 100) / 100);
    });
  }
});</pre>
<p align="left">Sans oublier d’ajouter le “span” dans lequel afficher notre consommation :</p>

<pre class="lang:html5 decode:true">&lt;h2&gt;PC-SEB CPU: &lt;span id="cpu"&gt;0&lt;/span&gt; %&lt;/h2&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-168.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-163.png" alt="image" width="250" height="432" border="0" /></a></p>
<p align="left">Et voilà comment en quelques lignes afficher des StateObjects en temps réel dans une application mobile multi-plateforme.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/DemoCordova.gif"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="DemoCordova" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/DemoCordova_thumb.gif" alt="DemoCordova" width="350" height="400" border="0" /></a></p>
<p align="left">Dans cet exemple nous affichons la consommation CPU capturée par le package HWMonitor mais nous pouvons bien sûr afficher n’importe quel StateObject : des capteurs de température, état d’une zone de l’alarme, position de votre voiture, volume de l’ampli, consommation électrique, état d’une lampe, etc.…</p>

<h4 align="left">Invoquer des MessageCallbacks</h4>
<p align="left">Tout comme n’importe quel “consommateur”, votre application mobile multi-plateforme peut envoyer des messages et donc invoquer des MessageCallbacks.</p>
<p align="left">Pour découvrir les MessageCallbacks de votre Constellation, vous pouvez utiliser le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a> de la Console Constellation.</p>
<p align="left">Par exemple, après avoir déployé le package “WindowsControl” sur une sentinelle Windows, vous pouvez invoquer des MC pour arrêter votre Windows, verrouiller la session, se déconnecter, redémarrer ou encore mettre en veille la machine.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb.png" alt="image" width="350" height="185" border="0" /></a></p>
<p align="left">Ajoutons à notre application Android la capacité de verrouiller notre PC Windows. Pour cela vous pouvez cliquer sur l’icone <img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-20.png" alt="image" width="20" height="19" border="0" /> pour générer le code d’invocation de ce MC en sélectionnant le langage de votre choix, dans notre cas, “Javascript” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-2.png" alt="image" width="350" height="217" border="0" /></a></p>
<p align="left">Dans notre page “index.html”, ajoutons un simple bouton (ou lien) HTML :</p>

<pre class="lang:html5 decode:true">&lt;a id="lockPC" href="#" class="btn"&gt;LOCK MY PC&lt;/a&gt;</pre>
<p align="left">Et dans notre fichier Javascript, invoquons le MessageCallback “'LockWorkStation” lorsque l’utilisateur clique sur notre bouton :</p>

<pre class="lang:javascript decode:true">$("#lockPC").click(function () 
  constellation.server.sendMessage({ Scope: 'Package', Args: ['PC-SEB_UI/WindowsControl'] }, 'LockWorkStation', {});
});</pre>
Notez que dans le code ci-dessus, à l’inverse de l’exemple donné par le générateur de code de la Console Constellation, nous avons défini le nom de l’instance du package et non le nom du package seul. En effet, en envoyant un message au scope de type Package ”WindowsControl”, votre message sera reçu par toutes les instances du package “WindowsControl” quelque soit la sentinelle. Autrement dit, toutes les sentinelles Windows qui exécutent le package “WindowsControl” seront verrouillées. Pour cibler votre sentinelle en particulier, nous avons indiqué ici le nom de l’instance, c’est à dire le couple “Sentinelle + Package”, ici “PC-SEB_UI/WindowsControl”.

Aussi, d’un point de vue visuel, Cordova n’inclut pas de framework CSS à l’inverse <a href="/client-api/javascript-api/application-mobile-multi-plateforme-avec-ionic-et-angular-js/">d’Ionic </a>par exemple. De ce fait tout est à votre charge. Ici nous avons créer un lien (&lt;a&gt;) sur lequel nous avons appliqué la classe CSS “btn”. Vous pouvez, pour l’exemple, utiliser des générateurs tel que <a href="http://www.cssbuttongenerator.com/">celui-ci</a> ou encore <a href="http://www.bestcssbuttongenerator.com/">celui-la</a> pour générer la classe “btn”.

Et voilà, notre application Android est maintenant capable de verrouiller notre poste Windows et d’afficher la consommation CPU en temps réel :
<p align="center">(screen here)</p>

<h3>Pour aller plus loin …</h3>
Donc vous avez pu le découvrir dans cet article, réaliser des applications mobiles (ou tablettes) pour Android, iOS, Windows ou autre est relativement facile avec des plateformes comme Cordova. En utilisant la <a href="/client-api/javascript-api/">libraire JavaScript Constellation</a>, vous pouvez donc développer des applications mobile connectées à Constellation en quelques minutes.

Une fois que votre application mobile est connectée à votre Constellation, elle peut suivre en temps réel tous les StateObjects produits par les packages de votre Constellation et invoquer des méthodes de ces packages en envoyant des messages.

De ce fait vous êtes libre de créer vos propres applications de supervision ou de contrôle de vos systèmes, services, réalisations, de votre domotique, etc…

Par exemple, vous créer un objet connecté avec un Raspberry et du Python ou bien un Arduino ou ESP8266, et vous pouvez créer facilement l’enrichir avec une application mobile. Ou bien vous ajoutez des lampes Hue et un media-center Kodi et vous décidez de créer une applications mobile unique pour tout contrôler en utilisant Constellation comme centralisateur.

Pour ma part je vous recommande vivement d’utiliser Ionic plutôt que Cordova. En effet, <a href="/client-api/javascript-api/application-mobile-multi-plateforme-avec-ionic-et-angular-js/">Ionic</a> n’est ni plus ni moins qu’une surcouche à Cordova qui intègre le framework AngularJS et un framework CSS. Grace à Ionic vous pouvez développer des applications plus riche et beaucoup plus rapidement.