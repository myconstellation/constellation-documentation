---
ID: 2114
post_title: 'Connectez vos pages Web &agrave; Constellation'
author: Sebastien Warin
post_date: 2016-07-06 15:22:29
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/connectez-vos-pages-web-constellation/
published: true
post_modified: 2017-11-22 14:31:36
---
<h3>Introduction</h3>
Il existe actuellement deux librairies Constellation JavaScript :
<ol>
 	<li>Constellation for Javascript</li>
 	<li>Constellation for AngularJS</li>
</ol>
La première est basée sur jQuery, la deuxième est une surcouche de la 1ère encapsulée dans un module AngularJS.

Vous retrouverez ces deux librairies depuis le gestionnaire de package Nuget intégré à Visual Studio :
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/07/image.png" alt="image" width="300" height="118" border="0" /></p>
Chacune de ces deux librairies propose deux types de client :
<ol>
 	<li>Consumer</li>
 	<li>Controller</li>
</ol>
Le client “Consumer” permet de se connecter à Constellation en tant que “consommateur” pour pouvoir interroger des StateObjects et envoyer/recevoir des messages.

Le client “Controller” permet lui de se connecter à Constellation en tant que "contrôleur" pour récupérer par exemple en temps réel les logs produits par vos packages, arrêter/démarrer les packages, surveiller la consommation de ressources de chacun d'entre eux, etc.

Dans ce guide nous allons utiliser le client "Consumer" afficher en temps réel le StateObject produit par le package <a href="/package-library/hwmonitor/">HWMonitor </a>représentant la consommation de votre CPU dans une page Web.

Pour cela vous devez avoir déployé le package “HWMonitor” sur au moins une de vos sentinelles (Windows).
<h3>Etape 1 : ajouter la librairie Constellation</h3>
<h4>En utilisant le CDN</h4>
Les librairies JavaScripts sont accessibles sur : <a href="http://cdn.myconstellation.io/js/">http://cdn.myconstellation.io/js/</a> (en HTTP ou HTTPS).

De ce fait, vous pouvez créer une simple pages HTML et ajoutez dans l’entête les balises suivantes :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.2.min.js"&gt;&lt;/script&gt;
</pre>
Si vous souhaitez connecter votre page à Constellation en utilisant le framework AngularJS :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/ngConstellation-1.8.2.min.js"&gt;&lt;/script&gt;
</pre>
<h4>Par Nuget en utilisant Visual Studio</h4>
Créez une application Web vide dans Visual Studio et ouvrez le gestionnaire de package NuGet.

En sélectionnant la source “Constellation”, installez le package Nuget “Constellation.Javascript” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-12.png" alt="image" width="354" height="122" border="0" /></a></p>
<p align="left">Une fois installée vous obtiendrez dans le dossier “Scripts” de votre projet Web les librairies jQuery, SignalR et Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-13.png" alt="image" width="244" height="224" border="0" /></a></p>
<p align="left">Créez ensuite une page Web (HTML ou ASPX) et dans l’entête de votre code HTML référencez les scripts (à adapter en fonction des n° de version des librairies contenues dans les packages Nuget) :</p>

<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="Scripts/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="Scripts/jquery.signalR-2.2.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="Scripts/Constellation-1.8.2.js"&gt;&lt;/script&gt;</pre>
<h3>Etape 2 : Initialiser le client</h3>
Tout d’abord il faut déterminer la clé d’accès qui sera utilisée par votre page. Ici, depuis la Console Constellation, nous créons un credential “DemoWeb” avec la clé d'accès “123456789” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-14.png" alt="image" width="354" height="141" border="0" /></a></p>
<p align="left">Dans notre page HTML, créons un client “Consumer” en spécifiant l’URI de notre Constellation, la clé d’accès utilisée pour se connecter et le “friendly name” de votre client :</p>

<pre class="lang:javascript decode:true">var constellation = $.signalR.createConstellationConsumer("http://localhost:8088", "123456789", "Test API JS");</pre>
<h3>Etape 3 : Etablir la connexion à Constellation</h3>
Ajoutons un handler sur le changement d’état de la connection pour afficher le message “Je suis connecté” dans la console de votre navigateur :
<pre class="lang:javascript decode:true">constellation.connection.stateChanged(function (change) {
    if (change.newState === $.signalR.connectionState.connected) {
        console.log("Je suis connecté");
    }
});
</pre>
Pour finir lançons la connexion en invoquant la méthode Start :
<pre class="lang:javascript decode:true">constellation.connection.start();</pre>
Testons la page dans Chrome par exemple :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-15.png" alt="image" width="354" height="77" border="0" /></a></p>

<h3>Etape 4 : Afficher le contenu d’un StateObject en temps réel</h3>
Dans notre cas nous voulons afficher en temps réel la consommation CPU de notre sentinelle.

Ce StateObject se nomme “/intelcpu/0/load/0” et est produit par le package “HWMonitor” que vous aurez pris le soin de déployer sur une de vos sentinelles (ici ma sentinelle se nomme “PO-SWARIN”) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-16.png" alt="image" width="244" height="176" border="0" /></a></p>
<p align="left">On peut donc enregistrer un “StateObjectLink” avec la méthode “registerStateObjectLink” en demandant un filtre sur la sentinelle nommée “MON-PC”, le package “HWMonitor” et le nom “/intelcpu/load/0” (le dernier argument de cette méthode correspond au type du StateObject, ici ”*” indique qu’il n’y a pas de filtre sur le type) et en indiquant la fonction à invoquer à chaque mise à jour du et des StateObjects liés, ici mettre à jour un &lt;span&gt; de la page.</p>
Vous devez impérativement faire cet enregistrement lorsque vous êtes connecté, c’est à dire lorsque le handler “stateChanged” est invoqué avec l’état “Connected”.

Le code final sera donc :
<pre class="lang:javascript decode:true crayon-selected">constellation.connection.stateChanged(function (change) {
    if (change.newState === $.signalR.connectionState.connected) {
        console.log("Je suis connecté");
        constellation.client.registerStateObjectLink("MON-PC", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
            console.log(so);
            $("#cpu").text(so.Value.Value);
        });
    }
});
</pre>
Dès que le StateObject est réceptionné par notre page nous affectons la propriété “Value” de la valeur du StateObject comme contenu du span “cpu” de la page. De ce fait, ajoutons un “span” nommé “cpu” dans le corps de notre page :
<pre class="lang:html5 decode:true">&lt;body&gt;
    &lt;span id="cpu"&gt;&lt;/span&gt; %
&lt;/body&gt;</pre>
Ainsi vous aurez en temps réel votre CPU dans une page HTML grâce à l’exploitation du StateObject ici produit par le package HWMonitor :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-17.png" alt="image" width="354" height="71" border="0" /></a></p>
<p align="left">Vous remarquerez que dans le handler ci-dessus nous loguons également l’objet “stateobject” réceptionné par notre page.</p>
<p align="left">Cet objet contient les différentes propriétés du StateObject : la sentinelle et le package qui ont produit ce StateObject, le nom du StateObject, son type, sa durée de vue (ici “lifetime=0” donc n’expire jamais), la date de mise à jour du StateObject, ses métadonnées et surtout sa valeur (propriété “Value”).</p>
<p align="left">Ici la valeur de ce StateObject produit par le package “HWMonitor” est un objet complexe (entouré en vert) qui contient différentes propriétés :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-18.png" alt="image" width="354" height="248" border="0" /></a></p>
<p align="left">Pour la suite je vous recommande vivement l’utilisation du framework AngularJS permettant de développer des pages plus facilement.</p>

<h3>Next step</h3>
<ul>
 	<li><a href="/client-api/javascript-api/consommer-constellation-api-javascript/">Consommer Constellation avec l’API Javascript</a></li>
 	<li><a href="/client-api/javascript-api/consommer-constellation-angular-js/">Consommer Constellation avec Angular JS</a></li>
 	<li><a href="/client-api/javascript-api/controler-constellation-api-javascript/">Contrôler votre Constellation en Javascript</a></li>
 	<li><a href="/client-api/javascript-api/application-mobile-multi-plateforme-avec-cordova-et-javascript/">Créer une application mobile multi-plateforme avec Cordova et l’API Javascript</a></li>
 	<li><a href="/client-api/javascript-api/application-mobile-multi-plateforme-avec-ionic-et-angular-js/">Créer une application mobile multi-plateforme avec Ionic et Angular JS</a></li>
 	<li><a href="/client-api/javascript-api/creer-application-montre-samsung-gear-tizen-angularjs/">Créer une application pour une montre Samsung Gear avec Tizen et AngularJS</a></li>
</ul>