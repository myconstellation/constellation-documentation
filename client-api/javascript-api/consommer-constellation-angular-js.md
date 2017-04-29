---
ID: 2331
post_title: Consommer Constellation avec Angular JS
author: Sebastien Warin
post_date: 2016-08-19 09:27:12
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/javascript-api/consommer-constellation-angular-js/
published: true
post_modified: 2016-09-23 10:37:28
---
<h3>Préparer la page AngularJS</h3> <p>Vous pouvez soit utiliser le gestionnaire de package Nuget depuis Visual Studio pour installer la dernière version du package “Constellation.Angular” et ses dépendances :</p> <p align="center"><img alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/07/image.png"></p> <p>Ou bien utiliser (ou copier) le(s) CDN :</p><pre class="lang:javascript decode:true">&lt;script type="text/javascript" src="//code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="//cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="//cdn.myconstellation.io/js/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt;
</pre>
<p>Vous devez ensuite créer un module Angular pour votre page que nous appelleront “MyDemoApp” et dans lequel nous injecterons le module “ngConstellation” :</p><pre class="lang:javascript decode:true">var myDemoApp = angular.module('MyDemoApp', ['ngConstellation']);</pre>
<p>Vous devez également ajouter l’attribut “ng-app” sur la balise &lt;html&gt; pour indiquer que votre page fait partie du module “MyDemoApp” :</p><pre class="lang:html5 decode:true">&lt;html xmlns="http://www.w3.org/1999/xhtml" ng-app="MyDemoApp"&gt;</pre>
<p>Enfin vous devez créer un contrôleur AngularJS dans lequel nous injecterons la factory “constellationConsumer” que nous appellerons dans notre code “constellation” :</p><pre class="lang:javascript decode:true">myDemoApp.controller('MyController', ['$scope',  'constellationConsumer', function ($scope, constellation) {

}]);
</pre>
<p>Sans oublier de lier votre corps de page (&lt;body&gt;) à ce contrôleur :</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;</pre>
<p>Et voilà votre squelette est prêt !</p>
<p>Pour plus d’information sur AngularJS, je vous recommande la lecture de ce guide : <a title="https://docs.angularjs.org/misc/started" href="https://docs.angularjs.org/misc/started">https://docs.angularjs.org/misc/started</a></p>
<p>Pour résumer notre squelette page est donc :</p><pre class="lang:html5 decode:true">&lt;!DOCTYPE html&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" ng-app="MyDemoApp"&gt;
&lt;head&gt;
    &lt;title&gt;Test API AngularJS&lt;/title&gt;
	&lt;script type="text/javascript" src="//code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
	&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
	&lt;script type="text/javascript" src="//cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
	&lt;script type="text/javascript" src="//cdn.myconstellation.io/js/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt;
    
    &lt;script&gt;
		var myDemoApp = angular.module('MyDemoApp', ['ngConstellation']);
		myDemoApp.controller('MyController', ['$scope',  'constellationConsumer', function ($scope, constellation) {

		}]);
    &lt;/script&gt;

&lt;/head&gt;
&lt;body ng-controller="MyController"&gt;

&lt;/body&gt;
&lt;/html&gt;

</pre>
<h3>Connecter une page HTML à Constellation avec AngularJS</h3>
<p>Dans notre contrôleur AngularJS, nous allons initialiser le client Consumer en spécifiant l’URL de votre serveur Constellation, la clé d’accès et le “Friendly name” de votre page :</p><pre class="lang:javascript decode:true">constellation.initializeClient("http://localhost:8088", "123456789", "TestAPI");</pre>
<p>Vous pouvez <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">créer une clé d’accès depuis la Console Constellation</a> ou en <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">éditant le fichier de configuration du serveur</a>.</p>
<p>Enfin ajoutons un handler sur le changement d’état de la connexion pour afficher le message “Je suis connecté” dans la console de votre navigateur par exemple lorsque la connexion à Constellation est établie :</p><pre class="lang:javascript decode:true">constellation.onConnectionStateChanged(function (change) {
    if (change.newState === $.signalR.connectionState.connected) {
        console.log("Je suis connecté !");
    }
});
</pre>
<p>Il ne reste plus qu’à lancer la connexion en invoquant la méthode “connect” :</p><pre class="lang:javascript decode:true">constellation.connect();</pre>
<p>Et voilà, votre page AngularJS est connectée à votre Constellation.</p>
<h3>Envoyer des messages et invoquer des MessageCallbacks</h3>
<h4>Envoyer des messages</h4>
<p>Pour envoyer des messages et donc invoquer des MessageCallbacks vous devez utiliser la méthode “<em>constellation.sendMessage</em>” en spécifiant :</p>
<ul>
<li>Le scope 
<li>La clé du message 
<li>Le contenu du message (= les paramètres du MessageCallback)</li></ul>
<p>Par exemple, avec le package Nest déployé dans votre Constellation, on retrouvera un MessageCallback&nbsp; “SetTargetTemperature” pour piloter la température de consigne d’un thermostat Nest. 
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-21.png"><img title="image" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-20.png" width="350" height="99"></a></p>
<p align="left">N’hésitez pas à utiliser le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a> pour découvrir l’ensemble des MessageCallbacks exposés par les packages de votre Constellation.</p>
<p>Pour invoquer le MessageCallback&nbsp; “SetTargetTemperature” depuis votre page Web :<pre class="lang:javascript decode:true">constellation.sendMessage({ Scope: 'Package', Args: ['Nest'] }, 'SetTargetTemperature', [ "thermostatId", 19 ]);</pre>
<p>En AngularJS, pour lier l’invocation de ce code à un bouton, vous devez simplement ajouter l’attribut “ng-click” sur votre bouton.</p>
<p>Par exemple, dans votre code HTML :</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;
    &lt;button ng-click="SetNestTemperature()"&gt;Set Nest to 19°C&lt;/button&gt;
&lt;/body&gt;</pre>
<p>Et dans votre contrôleur :</p><pre class="lang:javascript decode:true">$scope.SetNestTemperature = function () {
   constellation.sendMessage({ Scope: 'Package', Args: ['Nest'] }, 'SetTargetTemperature', [ "thermostatId", 19 ]);
};
</pre>
<p>Autre solution, on pourrait également exposer le client Consumer dans le scope AngularJS :</p><pre class="lang:javascript decode:true">$scope.constellation = constellation;</pre>
<p>Et donc se servir directement de votre client dans le template HTML :</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;
    &lt;button ng-click="constellation.sendMessage({ Scope: 'Package', Args: ['Nest'] }, 'SetTargetTemperature', [ "thermostatId", 19 ])"&gt;Set Nest to 19°C&lt;/button&gt;
&lt;/body&gt;</pre>
<p>Allons un peu plus loin en ajoutant un champ permettant à l’utilisateur de définir la température de consigne.</p>
<p>Dans le template HTML :</p><pre class="lang:html5 decode:true">Temperature : &lt;input type="number" ng-model="targetTemperature" /&gt;
&lt;button ng-click="SetNestTemperature()"&gt;Set target temperature&lt;/button&gt;
</pre>
<p>Le champ “input” est de type “number” et est lié à la variable de scope (le modèle) que nous appellerons “targetTemperature”.</p>
<p>De ce&nbsp; fait dans notre fonction “SetNestTemperature” nous pouvons récupérer la valeur définie par l’utilisateur :</p><pre class="lang:javascript decode:true">$scope.SetNestTemperature = function () {
   constellation.sendMessage({ Scope: 'Package', Args: ['Nest'] }, 'SetTargetTemperature', [ "thermostatId", $scope.targetTemperature ]);
};
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-29.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-28.png" width="350" height="60"></a></p>
<p>Comme pour l’API Javaqcript, vous pouvez également passer plusieurs arguments à votre MC combinant type simple et type complexe. Si il y a plusieurs paramètres vous devez les stocker dans un tableau.</p>
<p>Par exemple le MC “ShowNotification” du package Xbmc permettant d’afficher une notification sur une interface Kodi/XBMC prend deux paramètres : le nom d l’hôte XBMC (un type string) et le détail de la notification à afficher (un type complexe) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-23.png"><img title="image" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-22.png" width="350" height="143"></a></p><pre class="lang:javascript decode:true">constellation.sendMessage({ Scope: 'Package', Args: ['Xbmc'] }, 'ShowNotification', [ xbmcName, { "Title":"Hello", "Message":"Hello from JS" } ]);</pre>
<p>Bien entendu vous pouvez invoquer des MessageCallbacks de n’importe quel package, réel (programme Linux/Windows) ou virtuel (Arduino/ESP) ou même sur d’autres consommateurs (pages Web par exemple).</p>
<p>Ainsi vos pages Web peuvent, en envoyant des messages, invoquer des méthodes sur n'importe quel système connecté dans votre Constellation.</p>
<p>Par exemple dans l’article sur les ESP/Arduinos, notre ESP8266 exposait un MC “Reboot” pour redémarrer la puce. Si l’on souhaite redémarrer notre Arduino/ESP depuis une page Web, il suffirai d’envoyer un message sans paramètre au bon scope. Par exemple :</p><pre class="lang:javascript decode:true">constellation.sendMessage({ Scope: 'Sentinel', Args: ['MyArduino'] }, 'Reboot', {});</pre>
<p>Ici le scope est “Sentinel” avec l’argument “MyArduino”, c’est à dire que le message “Reboot” sera envoyé à tous les packages de la sentinelle “MyArduino”.</p>
<p>Les <a href="/concepts/messaging-message-scope-messagecallback-saga/">scopes</a> peuvent être :</p>
<ul>
<li>Sentinel 
<li>Package 
<li>Group 
<li>Others 
<li>All</li></ul>
<p>La propriété “Args” de l’objet scope est un tableau contenant le nom des groupes, des sentinelles ou packages en fonction du type de scope sélectionné. Seuls les scopes “All” et “Others” n’ont pas besoin d’argument.</p>
<h4>Envoyer des messages avec réponse : les sagas</h4>
<p>Les <a href="https://developer.myconstellation.io/concepts/messaging-message-scope-messagecallback-saga/">Sagas</a> permettant de lier des messages ensemble et donc de créer des couples de “requêtes / réponses”. 
<p>Avec la libraire JavaScript, il est possible d’envoyer des messages dans une saga et d’enregistrer un callback pour traiter la réponse de la saga. 
<p>Par exemple, le package “NetworkTools” expose un MC “Ping” permettant de réaliser un ping réseau. Le résultat du ping est retourné dans un message de réponse (un Int64 représentant le temps du réponse du ping) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb2.png"><img title="image_thumb2" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image_thumb2" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb2_thumb.png" width="244" height="110"></a></p>
<p align="left">De ce fait pour réaliser un ping depuis une page Web, on pourrait proposer à l’utilisateur un champ pour saisir l’adresse à pinger, un bouton et un afficher le résultat dans un paragraphe.</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;
   Host/IP : &lt;input ng-model="host" /&gt;
   &lt;button ng-click="Ping()"&gt;Ping&lt;/button&gt;
   &lt;p ng-show="pingResult"&gt;Result : {{pingResult}}ms&lt;/p&gt;
&lt;/body&gt;</pre>
<p>Vous remarquez que le paragraphe du n’est afficher (ng-show) que si la valeur de scope “pingResult” est définie, ce qui n’est pas le cas à l’ouverture de la page.</p>
<p>Déclarons enfin la méthode de scope “Ping” pour envoyer un message au package “NetworkTools” afin d’invoquer dans une saga&nbsp; le MC “Ping” avec le contenu du champs input en paramètre du message, c’est à dire la variable “$scope.host”.</p>
<p>Comme il s’agit d’une saga, nous utilisons la méthode “sendMessageWithSaga” dans laquelle nous passons la fonction callback qui sera invoquée lors de la réponse. A la réception de la réponse, nous allons tous simplement stocker le résultat de la réponse (propriété Data) dans la variable de scope “pingResult”.</p>
<p>L’affectation de la réponse dans la variable de scope “pingResult” est réalisée dans un “$scope.$apply”, une fonction AngularJS qui permet d’indiquer au contrôleur AngularJS que des variables de scope ont été modifiées et donc qu’il faut refaire le binding sur la vue.</p>
<p>Dans notre cas, une fois la variable de scope “pingResult” affectée avec le résultat du ping, le paragraphe sera afficher avec le résultat :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-30.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-29.png" width="350" height="88"></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-31.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-30.png" width="350" height="90"></a></p>
<p align="left">&nbsp;</p><pre class="lang:html5 decode:true">scope.Ping = function () {
  constellation.sendMessageWithSaga({ Scope: 'Package', Args: ['NetworkTools'] }, 'Ping', $scope.host,
    function(response) {
      $scope.$apply(function() {
        $scope.pingResult = response.Data;
      });
    });
};
</pre>
<p>Ainsi vos pages Web peuvent invoquer des méthodes et exploiter la réponse de tous les systèmes connectés sur Constellation exposant des MessageCallbacks.</p>
<h3>Recevoir des messages</h3>
<p>Une page Web connectée sur Constellation peut elle même recevoir des messages et donc exposer des MessageCallbacks que d’autres consommateurs (autres pages Web) ou packages (réels et virtuels) pourront invoquer.</p>
<p>Ceci dit un consommateur tel qu’une page Web n’a pas réellement d’existence. Autrement dit elle n’a pas d’identité et donc ne peut recevoir que des messages adressés à un groupe qu’elle devra joindre.</p>
<p>Pour joindre un groupe vous devez utiliser la méthode “subscribeMessages“ (et “unSubscribeMessages” pour en sortir). Vous pouvez joindre autant de groupe que vous souhaitez.</p>
<p>Par exemple, pour ajouter votre page au groupe “Demo” :</p><pre class="lang:javascript decode:true">constellation.subscribeMessages("Demo");</pre>
<p>Vous pourrez ensuite recevoir les messages envoyés dans ce groupe en ajoutant un handler sur le “onReceiveMessage” :</p><pre class="lang:javascript decode:true">constellation.onReceiveMessage(function (message) {
    console.log("Message recu !", message);
});
</pre>
<p>Dans ce handler vous recevrez TOUS les messages (quelque soit la clé du message).</p>
<p>Vous avez aussi un moyen plus simple consistant à définir un handler pour une clé de message spécifiquement via la méthode “registerMessageCallback”.</p>
<p>Par exemple :</p><pre class="lang:javascript decode:true">constellation.registerMessageCallback("ChangeTitle", function (msg) {
	document.title = msg.Data;
});
constellation.registerMessageCallback("HelloWorld", function (msg) {
	alert("Hello World");
});
</pre>
<p>Ici on enregistre deux MessageCallbacks.</p>
<p>Si votre page reçoit un MessageCallback “HelloWorld”, une alerte sera ouverte, si elle reçoit un MessageCallback “ChangeTitle”, le titre de votre page sera modifiée avec l’argument passé dans le MC.</p>
<p>Bien entendu, comme vos pages HTML peuvent enregistrer autant de MessageCallbacks qu’elles souhaitent et que ces MessageCallbacks sont invocables depuis n’importe quel système connecté à Constellation, d’autres pages Web, des objets à base d’Arduino, ESP ou autre, des programmes Python, .NET, etc… vous pouvez imaginer tout type d’usage. </p>
<h3>Consommer des StateObjects</h3>
<p>Pour consommer des StateObjects produits par des packages (virtuels ou réels) de votre Constellation, vous avez deux méthodes : l’évènement “onUpdateStateObject” ou les StateObjectLinks.</p>
<p>La première méthode consiste à enregistre un ou plusieurs handlers sur l’évènement “onUpdateStateObject” :</p><pre class="lang:javascript decode:true">constellation.onUpdateStateObject(function (stateobject) {
    console.log(stateobject);
});
</pre>
<p>Les handlers seront déclenchés à chaque mise à jour ou interrogation des StateObjects de votre page quelque soit le StateObject.</p>
<p>Pour interroger un ou plusieurs StateObjects à un instant T vous disposez de la méthode “requestStateObjects”.</p>
<p>Pour vous abonner aux mises à jour d’un ou plusieurs StateObjects, vous disposez de la méthode “subscribeStateObjects” (et “unSubscribeStateObjects” pour vous désabonner).</p>
<p>Enfin la méthode “requestSubscribeStateObjects” réalise un “requestStateObjects” et un “subscribeStateObjects” c’est à dire que la méthode demande la valeur actuelle du ou des StateObjects et s’abonne auprès du serveur Constellation pour être notifiée des mises à jour du et de ces StateObjects.</p>
<p>Toutes ces méthodes prennent en paramètre : le nom de la sentinelle, le nom du package, le nom du StateObject et le type du StateObject. Vous pouvez utiliser le wildcard “*” pour ne pas appliquer de filtre (<a href="/concepts/stateobjects/">plus d’information ici</a>).</p>
<p>Par exemple, pour récupérer tous les StateObjects du package “Demo” quelque soit la sentinelle et pour s’abonner aux mises à jour de ces StateObjects :</p><pre class="lang:javascript decode:true">constellation.requestSubscribeStateObjects("*", "Demo", "*", "*");</pre>
<p>L’autre méthode consiste à enregistrer un (ou plusieurs) StateObjectLink. Cela permet de lier une fonction à un abonnement.</p>
<p>Par exemple :</p><pre class="lang:javascript decode:true">constellation.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
    // Code A
});

constellation.registerStateObjectLink("*", "Demo", "*", "*", function (so) {
    // Code B
});</pre>
<p>Ci-dessus le code A sera invoqué dès qu’un SO nommé "/intelcpu/0/load/0” et produit par le package “HWMonitor” est modifié alors que le code B sera déclenché dès qu’un StateObject du package “Demo” est modifié.</p>
<p>Prenons un cas pratique, nous voulons afficher en temps réel la consommation de notre CPU. Comme expliqué ci-dessus, la consommation peut être connue avec le StateObject nommé "/intelcpu/0/load/0” et produit par le package “HWMonitor”.</p>
<p>Nous allons donc ajouter un StateObjectLink et affecter la variable de scope “cpu” à chaque mise à jour de ce SO. Comme pour réception de message, nous devons utiliser la fonction Angular “$scope.$apply” pour informer le contrôleur Angular que nous avons changé une variable de scope :</p>
<p>Aussi vous devez enregistrer vos SOLink une fois connecté :</p><pre class="lang:javascript decode:true">constellation.onConnectionStateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    constellation.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
      $scope.$apply(function() {
        $scope.cpu = so.Value.Value;
      });
    });
  }
});
</pre>
<p>Et dans notre template :</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;
	&lt;p&gt;CPU : {{cpu}}&lt;/p&gt;
&lt;/body&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-33.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-32.png" width="350" height="64"></a></p>
<p>Plutôt que stocker dans notre variable de scope la propriété “Value” de la valeur de notre StateObject, nous pouvons aussi stocker le StateObject lui même :</p><pre class="lang:html5 decode:true">$scope.cpu = so;</pre>
<p>De ce fait dans le template nous pouvons afficher différentes informations contenu dans notre StateObject&nbsp; :</p><pre class="lang:html5 decode:true">&lt;body ng-controller="MyController"&gt;
	&lt;p&gt;CPU on {{cpu.SentinelName }} at {{cpu.LastUpdate}} is &lt;strong&gt;{{cpu.Value.Value}} {{cpu.Value.Unit}}&lt;/strong&gt;&lt;/p&gt;
&lt;/body&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-32.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-31.png" width="350" height="50"></a></p>
<p>Alors bien sûr le format de la date ou encore la précision de la valeur mesurée par le package HWMonitor n’est pas très lisible. Nous pouvons alors ajouter des <a href="https://docs.angularjs.org/api/ng/filter/filter">filtres AngularJS</a> directement dans notre template pour rendre notre page plus “user-friendly”.</p>
<p>Utilisons le filtre “date” pour formater la date et “number” pour limite le notre de chiffre par la virgule :</p><pre class="lang:html5 decode:true">&lt;p&gt;CPU on {{cpu.SentinelName }} at {{cpu.LastUpdate | date : 'dd//MM/yyyy @ hh:mm:ss' }} is &lt;strong&gt;{{cpu.Value.Value | number:2}} {{cpu.Value.Unit}}&lt;/strong&gt;&lt;/p&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-34.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-33.png" width="350" height="69"></a></p>
<p>C’est tout la magie d’AngularJS <img class="wlEmoticon wlEmoticon-winkingsmile" style="border-top-style: none; border-bottom-style: none; border-right-style: none; border-left-style: none" alt="Clignement d'&oelig;il" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/wlEmoticon-winkingsmile-1.png"></p>
<p>Allons encore un peu plus loin !</p>
<p>Ici nous avons lié notre variable de scope “cpu” au StateObject nommé "/intelcpu/0/load/0” et produit par le package “HWMonitor”. Mais n’oubliez pas que <a href="/concepts/stateobjects/">l’unicité d’un StateObject</a> est obtenu avec le triplet : Sentinelle + Package + Nom.</p>
<p>Autrement dit il peut y avoir plusieurs SO qui correspondent à ce filtre dans le cas où vous avez déployé le package HWMonitor sur plusieurs sentinelles.</p>
<p>Dans ce cas, la valeur du SO est sans cesse écrasée et notre page est illisible :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/cpus.gif"><img title="cpus" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="cpus" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/cpus_thumb.gif" width="350" height="58"></a></p>
<p>Nous allons gérer ce cas en créant une variable de scope qui contiendra les différents StateObjects de nos CPUs.</p>
<p>Dans votre scope, commençons par déclarer cette variable de scope, nommée “cpus” en l’initialisant avec un objet vide :</p><pre class="lang:javascript decode:true">$scope.cpus = {}</pre>
<p>Modifions notre StateObjectLink pour stocker le StateObject dans notre objet “cpus” sous la propriété du nom de la sentinelle de notre StateObject.</p><pre class="lang:javascript decode:true">constellation.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
  $scope.$apply(function() {
    $scope.cpus[so.SentinelName.replace("-", "")] = so;
  });
});
</pre>
<p>C’est à dire que notre variable de scope “cpus” est un objet avec des propriétés du nom de la sentinelle et qui contient le StateObject de son CPU associé.</p>
<p>Attention le nom de la sentinelle est utilisée comme nom de propriété de notre objet JavaScript et vous n’êtes pas sans savoir que les “-“ (entre autre) ne sont pas autorisés en JS ce qui explique que nous les effaçons avec le “replace”.</p>
<p>Ainsi si je veux afficher dans mon template le CPU de ma machine nommée ”PC-SEB_UI” (= propriété “PCSEB_UI”), je devrais écrire :</p><pre class="lang:javascript decode:true">&lt;p&gt;CPU on {{cpus.PCSEB_UI.SentinelName }} at {{cpus.PCSEB_UI.LastUpdate | date : 'dd//MM/yyyy @ hh:mm:ss' }} is &lt;strong&gt;{{cpus.PCSEB_UI.Value.Value | number:2}} {{cpus.PCSEB_UI.Value.Unit}}&lt;/strong&gt;&lt;/p&gt;</pre>
<p>Et ainsi je n’aurai plus l’effet indésirable de voir les StateObjects chevaucher constamment.</p>
<p>Mais alors on peut aller encore plus loin car notre variable “cpus” contient tous les SO de la consommation de CPU produits par l’ensemble des packages HWMonitor de notre Constellation.</p>
<p>Il suffit d’itérer sur cette variable pour affcher le SO de chaque propriété de notre objet. Avec AngularJS vous pouvez utiliser la directive “<a href="https://docs.angularjs.org/api/ng/directive/ngRepeat">ng-repeat</a>” :</p><pre class="lang:html5 decode:true">&lt;p ng-repeat="cpu in cpus"&gt;CPU on {{cpu.SentinelName }} at {{cpu.LastUpdate | date : 'dd//MM/yyyy @ hh:mm:ss' }} is &lt;strong&gt;{{cpu.Value.Value | number:2}} {{cpu.Value.Unit}}&lt;/strong&gt;&lt;/p&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/cpus2.gif"><img title="cpus2" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="cpus2" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/cpus2_thumb.gif" width="350" height="256"></a></p>