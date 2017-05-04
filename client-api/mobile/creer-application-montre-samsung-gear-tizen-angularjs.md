---
ID: 2746
post_title: 'Cr&eacute;er une application pour une montre Samsung Gear avec Tizen et AngularJS'
author: Sebastien Warin
post_date: 2016-09-23 10:42:43
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/mobile/creer-application-montre-samsung-gear-tizen-angularjs/
published: true
post_modified: 2017-05-04 16:22:09
---
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-3.png" alt="image" width="117" height="145" border="0" /></a></p>

<h3>Découverte de Tizen</h3>

La Samsung Gear S2 tourne sous Tizen. Cet OS a été initié par Samsung sous le nom LiMo (Linux Mobile) avant d’être renommé en 2011 par Tizen lorsque qu’Intel a rejoint le projet en abandonnant MeeGo. Depuis 2012, Tizen est développé par la “Tizen Association” qui regroupe entre autres Samsung et Intel mais aussi Huawei, Fujitsu, NEC, Panasonic, Orange, NTT DoCoMo ou encore Vodafone.

Dans ses premières versions toutes les applications Tizen étaient des applications HTML/JS permettant de garantir une portabilité entre les différents équipements. Depuis la version 2.0 sortie en 2013, il est également possible de développer des applications natives. C’est également à cette date que Samsung a annoncé l’abandon de son précédent système Bada pour se consacrer exclusivement à Tizen.

On retrouve Tizen dans plusieurs équipements de la marque : des smartphones (série Samsung Z), tablettes ou PC aux wearables (série Gear) en passant par les TV comme la dernière UN65H8000AF ou les appareils photos (comme le NX1). Cet OS a pour objectif d’équiper une multitude équipement dont des produits électroménagers (micro-onde, lave-linge, réfrigérateur), des voitures, des imprimantes, etc…

<h3>Hello World Tizen</h3>

Pour commencer, rendez-vous sur le site développeur Samsung pour télécharger le SDK Tizen.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-4.png" alt="image" width="350" height="115" border="0" /></a></p>

Une fois la base installée, il faudra lancer l’<em>Update Manager</em> afin de sélectionner les composants dont vous avez besoin.

Dans notre cas, nous allons télécharger le Framework “Wearable” 2.3.1 pour développer des applications pour la Gear S2 ainsi que les outils du SDK Tizen (dont l’Emulateur Manager) mais surtout, dans la catégorie “Extras”, les extensions “Tizen Wearable” et le Certification Manager.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-5.png" alt="image" width="350" height="222" border="0" /></a></p>

Pour développer une application Tizen Wearable, on peut soit faire une application “Web” en utilisant HTML5/CSS/JavaScript, soit une application native C/C++ ou soit une application hydride (Web + native).

Ces applications Wearables peuvent être rangées dans deux catégories :

<ul>
    <li>“Standalone” : fonctionne sur votre montre de manière autonome</li>
    <li>“Companion” : est liée à une application “host” qui tourne sur un mobile (Tizen ou Android)</li>
</ul>

Lançons donc l’IDE Tizen, un IDE basé sur Eclipse. Nous pouvons alors créer un nouveau projet à partir de template ou d’exemple prêt à l’emploi.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-6.png" alt="image" width="350" height="316" border="0" /></a></p>

Ici la page HTML est un simple Hello World avec quelques lignes de JS pour changer le contenu d’une div au clic et gérer le bouton « back ».

Pour tester votre application, vous disposez du « Emulator Manager » vous permettant de créer des VM pour émuler votre Gear S2 et tester vos applications en local :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-7.png" alt="image" width="350" height="367" border="0" /></a></p>

Comme pour le développement d’une application Cordova/Ionic sur Android, vous disposez de l’inspecteur Chrome intégré :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-8.png" alt="image" width="350" height="168" border="0" /></a></p>

Bien entendu, vous pouvez également déployer et debugger directement sur votre montre. La première chose à faire est d’activer le débogage dans les paramètres de votre Gear S2 et de la connecter sur votre réseau Wifi.

Depuis le “Connection Explorer” de l’IDE Tizen, vous pourrez alors ajouter un nouveau “Remote Device” en spécifiant l’IP de votre Gear :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-9.png" alt="image" width="350" height="258" border="0" /></a></p>

Vous devrez alors approuver la connexion sur votre montre :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-10.png" alt="image" width="350" height="197" border="0" /></a></p>

Après quoi elle sera connectée dans l’IDE Tizen de la même manière que l’émulateur :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-11.png" alt="image" width="350" height="185" border="0" /></a></p>

Nous pouvons alors relancer l’application en mode “Run” ou “Debug” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-12.png" alt="image" width="350" height="197" border="0" /></a></p>

Avec toujours avec la possibilité de faire du “Remote Debug” avec l’inspecteur Chrome directement sur la montre !

<h3>Créer une application Tizen connectée à Constellation</h3>

Comme il s’agit d’une application Web/Javascript, nous pouvons simplement utiliser la librairie JavaScript de Constellation. Et pour simplifier d’avantage le code, utilisons AngularJS et son module Constellation.

Créons donc un nouveau projet Tizen Wearable :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-13.png" alt="image" width="350" height="319" border="0" /></a></p>

Puis ajoutons les libraires Constellation dans le dossier « js » :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-14.png" alt="image" width="232" height="203" border="0" /></a></p>

Après avoir référencé ces libraires dans la page « index.html », éditons le fichier « app.js » à la racine pour créer un contrôleur AngularJS en y injectant le module Constellation.

Nous initialiserons le client Constellation avec l’URI de votre Constellation et une clé d’accès :

<pre class="lang:javascript decode:true">var swatch = angular.module('swatch', ['ngConstellation'])  .controller('MainController', ['$scope', 'constellationConsumer',
     function ($scope, constellation) { 
      
     constellation.initializeClient("https://monServerConstellation.com/constellation", "MonAccessKey", "SWatch");
        
     constellation.connect();  
}]);
</pre>

Pour chaque StateObject reçu de la Constellation, stockons-le dans une variable de notre scope Angular ce qui nous permettra de les exploiter dans le template HTML :

<pre class="lang:javascript decode:true">constellation.onUpdateStateObject(function (message) {
  $scope.$apply(function () {                  
    if ($scope[message.PackageName] == undefined) {
      $scope[message.PackageName] = {}; 
    }
    $scope[message.PackageName][message.Name] = message;      
    });
});
</pre>

Enfin, lorsque que nous sommes connecté à Constellation abonnons-nous aux StateObjects des différents packages de notre Constellation que nous souhaitons intégrer dans notre application Tizen.

Pour ma part je demande les StateObjects du thermostat Nest, des caméras de sécurité connectés à ZoneMinder, de mon système home-made de monitoring des ressources énergétiques S-Energy (basé sur un Raspberry) et S-Opener (gestion de la porte de garage), des capteurs NetAtmo et Oregon via un RFXCOM, de la domotique Z-Wave via une Vera, des media-centers Xbmc/Kodi, de mon ampli Pioneer, des onduleurs, de l’alarme (Paradox), des capteurs de luminosité home-made (à base de Raspberry et ESP8266), des capteurs hardware via WMI et ceux du routeur DD-WRT et également les StateObjects du package Tesla présentés dans l’article précédent.

<pre class="lang:javascript decode:true">constellation.onConnectionStateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    constellation.requestSubscribeStateObjects("*", "tesla", "*", "*");
    constellation.requestSubscribeStateObjects("*", "ZoneMinder", "*", "ZoneMinder.Monitor");
    constellation.requestSubscribeStateObjects("*", "SEnergy", "*", "*");
    constellation.requestSubscribeStateObjects("*", "SOpener", "*", "*");       
    constellation.requestSubscribeStateObjects("*", "NetAtmo", "*", "*");       
    constellation.requestSubscribeStateObjects("*", "rfxcom", "*", "*");
    constellation.requestSubscribeStateObjects("*", "Vera", "*", "*");
    constellation.requestSubscribeStateObjects("*", "Nest", "*", "*");
    constellation.requestSubscribeStateObjects("*", "Xbmc", "*", "*");
    constellation.requestSubscribeStateObjects("*", "Pioneer", "*", "*");
    constellation.requestSubscribeStateObjects("*", "BatteryChecker", "*", "*");
    constellation.requestSubscribeStateObjects("*", "Paradox", "*", "*");       
    constellation.requestSubscribeStateObjects("*", "LightSensor", "Lux", "*");     
    constellation.requestSubscribeStateObjects("CEREBRUM", "HWMonitor", "*", "*");
    constellation.requestSubscribeStateObjects("*", "ddwrt", "RouterAJS.WAN", "*");
    constellation.requestSubscribeStateObjects("*", "ddwrt", "RouterAJS.WAN.Stats", "*");
  }
});</pre>

Pas besoin de plus de code, on peut dès maintenant éditer la page HTML pour créer notre interface.

Par exemple pour afficher la température et l’humidité captées par les sondes NetAtmo, il me suffira dans le template HTML d’ajouter le code suivant :

<pre class="lang:html5 decode:true">&lt;p&gt;Salon : {{NetAtmo['Salon.Temperature'].Value}}°C | {{NetAtmo['Salon.Humidity'].Value}}%&lt;/p&gt;
&lt;p&gt;Jardin : {{NetAtmo['Jardin.Temperature'].Value}}°C | {{NetAtmo['Jardin.Humidity'].Value}}%&lt;/p&gt;
&lt;p&gt;Chambre1 : {{NetAtmo['Chambre1.Temperature'].Value}}°C | {{NetAtmo['Chambre1.Humidity'].Value}}%&lt;/p&gt;
&lt;p&gt;Chambre2 : {{NetAtmo['Chambre2.Temperature'].Value}}°C | {{NetAtmo['Chambre2.Humidity'].Value}}%&lt;/p&gt;</pre>

Tizen propose des classes CSS prêtes à l’emploi que nous pouvons directement appliquer sur nos balises HTML pour rendre le visuel plus agréable :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-15.png" alt="image" width="193" height="240" border="0" /></a></p>

De plus grâce à la notion de StateObjects de Constellation, nous pouvons sans trop d’effort afficher tout type de donnée peu importe la source. Par exemple, ici nous affichons la température de 3 pièces différentes captées par trois technologies différentes (NetAtmo, RFXCOM et Z-WAve)

<pre class="lang:html5 decode:true">&lt;p&gt;Chambre (Netatmo) : {{NetAtmo['Chambre1.Temperature'].Value}}°C&lt;/p&gt;
&lt;p&gt;Bureau (RFXCOM/Oregon) : {{rfxcom['Capteur Bureau'].Value.Temperature}}°C&lt;/p&gt;
&lt;p&gt;Cave (Vera/Z-WAve) : {{Vera['Temperature Cave'].Value.Temperature}}°C&lt;/p&gt;
</pre>

La librairie Tizen fournit également quelques contrôles Javascript. Par exemple utilisons le « tau.widget.CircleProgressBar » qui nous attachons sur un élément « progressbar » lié au StateObject « Nest » :

<pre class="lang:html5 decode:true">&lt;div class="ui-content"&gt;
  &lt;div&gt;Consigne: &lt;span id="nestTarget"&gt;{{ Nest['Living Room'].Value.target_temperature_c }}&lt;/span&gt;°C&lt;/div&gt;
  &lt;div&gt;Actuelle: {{Nest['Living Room'].Value.ambient_temperature_c}}°C&lt;/div&gt;
&lt;/div&gt;
&lt;progress class="ui-circle-progress" id="nestProgress" value="{{(Nest['Living Room'].Value.target_temperature_c}}" min="9" max="32"&gt;&lt;/progress&gt;
</pre>

Ainsi il devient alors possible de contrôler son thermostat en tournant le cadran rotatif de la montre :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-16.png" alt="image" width="193" height="231" border="0" /></a></p>

Pour envoyer la température de consigne, il me suffit d’invoquer le MessageCallback « SetTargetTemperature » au package Nest par la ligne :

<pre class="lang:javascript decode:true">constellation.sendMessage({ Scope: 'Package', Args: [ 'Nest' ] }, 'SetTargetTemperature', [ deviceid, temperature ])</pre>

Pour reprendre l’exemple de la sonnette connectée à Constellation par un ESP8266, on peut facilement ajouter le contrôle du mode « muet » sur notre montre. Il suffit d’ajouter une checkbox dont l’état est coché si notre StateObject « IsMute » est vrai :

<pre class="lang:html5 decode:true">&lt;div class="ui-toggleswitch ui-toggleswitch-large"&gt;
  &lt;input type="checkbox" class="ui-switch-input" ng-checked="DoorBell.IsMute" ng-click="SetMute(!DoorBell.IsMute)"&gt;
  &lt;div class="ui-switch-button"&gt;&lt;/div&gt;
&lt;/div&gt;
</pre>

Et lorsque l’on clique sur la checkbox, on invoque le MessageCallback « SetMute » en passant l’état inverse de notre StateObject :

<pre class="lang:javascript decode:true">$scope.SetMute = function(isMute) {
  constellation.sendMessage({ Scope: 'Package', Args: [ 'DoorBell' ] }, 'SetMute', isMute);                 
};
</pre>

De quoi contrôler notre sonnette à n’importe quel moment depuis son poignet :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-17.png" alt="image" width="186" height="232" border="0" /></a></p>

On peut aussi très facilement contrôler toute notre domotique Z-Wave. Pour ce faire, créons une listview (&lt;ul&gt;) pour tous nos StateObjects du package « Vera » afin d’afficher une checkbox liée à l’état de notre module Z-Wave en utilisant un « ng-repeat ».

Et comme pour la sonnette, lorsque l’on clique dessus, on envoie un message au package Vera pour définir le nouvel état :

<pre class="lang:html5 decode:true">&lt;ul class="ui-listview"&gt;
  &lt;li class="li-has-multiline li-has-toggle" ng-repeat="device in Vera"&gt;
   &lt;label&gt;{{device.Name}}
      &lt;span class="li-text-sub ui-li-sub-text"&gt;{{device.Metadatas.Room}}&lt;/span&gt;
      &lt;div class="ui-toggleswitch"&gt;
         &lt;input type="checkbox" class="ui-switch-input" ng-click="SwitchVeraDevice(device)" ng-checked="device.Value.Status"&gt;
         &lt;div class="ui-switch-button"&gt;&lt;/div&gt;
      &lt;/div&gt;
   &lt;/label&gt;
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

Et là encore, juste avec ces quelques lignes de code HTML on obtient une interface de contrôle de tous nos équipements Z-Wave (lumières, appliances, volets) sur notre poignet :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-18.png" alt="image" width="179" height="240" border="0" /></a></p>

De ce fait, chaque objet, service ou programme connecté dans Constellation peut être facilement intégré dans vos applications Javascript, que ce soit une page Web, une application mobile Cordova/Ionic ou bien comme ici une application Tizen pour votre montre !

Au final avec à peine 150 lignes de Javascript et 350 de code HTML, cette application me permet au quotidien de garder un œil sur mes serveurs, ma consommation énergétique et les différents capteurs météo de la maison, voir en temps réel mes caméras de sécurité, piloter mon système d’alarme, les lampes et volets de la maison, le thermostat, l’ampli ou encore porte ma porte de garage.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-19.png" alt="image" width="186" height="240" border="0" /></a></p>