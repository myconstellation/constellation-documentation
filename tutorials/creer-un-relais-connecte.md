---
ID: 4604
post_title: 'Cr&eacute;er un relais connect&eacute;'
author: Sebastien Warin
post_date: 2017-05-10 17:46:17
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-un-relais-connecte/
published: true
post_modified: 2017-05-10 17:51:19
---
Dans ce tutoriel nous allons créer un relais connecté qu’on pourra contrôler depuis une page Web, une montre ou même un script Powershell pour piloter une lumière, une porte de garage, une chaudière ou n’importe quelle charge électrique (10 ampères max) ou contact sec.
<h3>Prérequis</h3>
<ul>
 	<li>Un Arduino connecté ou un ESP8266</li>
 	<li>Un shield relais (ou un relais seul avec une diode, une résistance et un transistor NPN)</li>
 	<li>Un serveur Constellation</li>
</ul>
<h3>Etape 1 : le montage</h3>
Ici on utilisera un D1 Mini Pro (ESP8266) avec un "Shield" relais tout fait qu'il n'y a plus qu'à déposer sur le "D1 Mini". Si vous n'avez de shield, vous pouvez utiliser un relais seul que vous piloterez avec un transistor NPN via une sortie de l'ESP8266.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-58.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Relais connecté" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-58.png" alt="Relais connecté" width="454" height="257" border="0" /></a></p>
Vous retrouverez sur ce shield un bornier à 3 pôles :
<ul>
 	<li>Le pôle de gauche est « NO » (normalement ouvert)</li>
 	<li>Le pole du milieu est le commun</li>
 	<li>Le pôle de droite est « NC » (normalement fermé)</li>
</ul>
Autrement dit, au repos il y a contact entre le 2<sup>ème</sup> et 3<sup>ème</sup> pole (normalement fermé). Lorsque que vous activez le relais, vous permutez le contact entre la 1<sup>ère</sup> et le 2<sup>ème</sup> pole.

Si vous voulez piloter une lampe, connectez un des fils entre la source d’énergie et votre lampe sur le pole « commun » et le pole « NO ». Ainsi au repos, votre lampe ne sera pas allumée mais si vous activez votre relais elle le sera.

Attention tout de même si vous manipulez des sources AC 220V et n’oubliez pas que votre relais ne supporte que des charges inférieures à 10A (soit environ 2200W).
<h3>Etape 3 : la programmation</h3>
Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d’accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée “ESP8266” et le package virtuel se nomme “MyRelay”.

Dans l’Arduino IDE, nous créons un nouveau projet à partir du Starter Kit Constellation pour ESP8266 dans lequel nous allons définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d’accès  puis nous configurerons le client Constellation en spécifiant l’identité de notre package virtuel, sa clé d’accès et l’adresse/port de notre serveur Constellation :
<pre class="lang:default decode:true crayon-selected">// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MON SSID";
char password[] = "macléWifi!!!!";

// Constellation client
Constellation&lt;WiFiClient&gt; constellation("X.X.X.X", 8088, "ESP8266", "MyRelay", "xxxxxxxxxxxxxxxxx")</pre>
Encore une fois si tout cela est nouveau pour vous, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l’API Constellation pour Arduino/ESP8266</a>.

Notre relais se pilote par la sortie D1. Commencez donc par configurer cette sortie en ajoutant la ligne suivante dans la méthode «<em> setup()</em> » :
<pre class="lang:cpp decode:true">pinMode(D1, OUTPUT);</pre>
Ensuite, toujours dans la méthode de démarrage « <em>setup()</em> » et une fois connecté au Wifi, déclarez le MessageCallback suivant :
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("SwitchState",
  MessageCallbackDescriptor().setDescription("Set the state of the relay").addParameter&lt;bool&gt;("state", "The state of the relay"),
  [](JsonObject&amp; json) {
      bool state = json["Data"].as&lt;bool&gt;();
      // Changer l'état du relais
      digitalWrite(D1, state ? HIGH : LOW);
      // Mise à jour du StateObject de l'état du relais
      constellation.pushStateObject("State", state);
 });
</pre>
Nous exposons ici un MessageCallback nommé « <em>SwitchState</em> »  qui prend en paramètre un booléen pour définir l’état du relais (ON ou OFF).

Le code du MC pilotera alors la sortie D1 sur lequel est connecté notre relais et publiera un StateObject « <em>State</em> » avec l’état actuel du relais.

Pour finir, toujours au démarrage, ajoutez cette ligne après la déclaration de votre MC :
<pre class="lang:cpp decode:true">constellation.pushStateObject("State", false);</pre>
Ainsi au démarrage de l’ESP8266, le relais est forcément sur Off, donc nous actualisation le StateObject « State » sur Constellation avec la valeur « false ».

<b>Et voilà, votre relais connecté est opérationnel !</b>

Vous pouvez connecter sur les pôles NO et commun une charge électrique comme une lampe ou une simple LED, en courant continue ou alternatif (220V par exemple) sans dépasser 10 ampères.

Si vous ne connectez rien sur votre relais, vous pouvez tout de même tester son bon fonctionnement :
<ul>
 	<li>Le relais « claque » lorsqu’il change d’état</li>
 	<li>Une petite LED rouge à côté de l’inscription « NC » s’allumera quand le relais est sur « ON »</li>
</ul>
Lancez le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a> de la Console Constellation pour tester votre MessageCallback :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-59.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-59.png" alt="MessageCallbacks Explorer" width="454" height="141" border="0" /></a></p>

<h3>Etape 3 : créer une page Web de pilotage du relais</h3>
Créons maintenant une page Web pour piloter notre relais. Pour cela créez un fichier HTML sur votre poste avec une structure HTML de base.

Dans les entêtes ajoutez les scripts suivants :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https:/cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt;
</pre>
Sur la balise &lt;html&gt; ajoutez l’attribut :
<pre class="lang:html5 decode:true">ng-app="MyDemoApp"</pre>
Et sur la balise &lt;body&gt; ajoutez l’attribut :
<pre class="lang:html5 decode:true">ng-controller="MyController"</pre>
Ajoutez ensuite le script suivant dans votre page (dans une balise &lt;script&gt;) :
<pre class="lang:javascript decode:true">var myDemoApp = angular.module('MyDemoApp', ['ngConstellation']);
myDemoApp.controller('MyController', ['$scope', 'constellationConsumer', function ($scope, constellation) {

      constellation.initializeClient("http://localhost:8088", "clé d'accès ici", "MyRelay Controller");
      constellation.onConnectionStateChanged(function (change) {
          if (change.newState === $.signalR.connectionState.connected) {
              console.log("Je suis connecté !");

              constellation.registerStateObjectLink("*", "MyRelay", "*", "*", function (so) {
                  $scope.$apply(function () {
                      $scope.RelayState = so.Value;
                  });
              });

          }
      });
      constellation.connect();
      $scope.constellation = constellation;
  }]);
</pre>
N’oubliez pas de définir l’adresse de votre Constellation ainsi que la clé d’accès pour vous connecter à votre Constellation lors de l’appel de la méthode « <em>constellation.initializeClient</em> ».

Enfin, ajoutez sur votre page la valeur du StateObject « State » et un bouton pour envoyer un message au package « MyRelay » afin d’invoquer le MC « SwitchState ».

Par exemple avec quelques classes CSS issues de Bootstrap :
<pre class="lang:html5 decode:true">&lt;div class="panel" ng-class="RelayState ? 'panel-danger' : 'panel-default'"&gt;
    &lt;div class="panel-heading"&gt;
        &lt;h3 class="panel-title"&gt;My Relay&lt;/h3&gt;
    &lt;/div&gt;
    &lt;div class="panel-body"&gt;
        &lt;p&gt;Current state : {{ RelayState ? "ON" : "OFF" }}&lt;/p&gt;
        &lt;a href="#" class="btn btn-primary" ng-click="constellation.sendMessage({ Scope: 'Package', Args: ['MyRelay'] }, 'SwitchState', !RelayState)"&gt;Switch {{ RelayState ? "OFF" : "ON" }}&lt;/a&gt;
    &lt;/div&gt;
&lt;/div&gt;
</pre>
<b>Vous obtenez en quelques minutes une page Web permettant de piloter en temps réel votre relais avec un retour sur son état courant.</b>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-66.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Page Web de contrôle" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-66.png" alt="Page Web de contrôle" width="382" height="246" border="0" /></a></p>
Vous pouvez d’ailleurs ouvrir la page sur plusieurs navigateurs ou sur plusieurs machines et constater que toutes les pages se mettent à jour en temps réel dès que l’état du relais change.
<h3>Etape 4 (optionnelle) : Piloter le relais avec un script Powershell</h3>
Pour invoquer votre MessageCallback depuis un script Powershell nous allons utilisez l’interface http.

Vous pouvez d’ailleurs afficher le code snippet pour obtenir l’URL depuis le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a>.

Dans notre cas l’URL sera : <a href="http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=DemoPowershel&amp;AccessKey=xxxx&amp;scope=Package&amp;args=MyRelay&amp;key=SwitchState&amp;data=true">http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=DemoPowershel&amp;AccessKey=xxxx&amp;scope=Package&amp;args=MyRelay&amp;key=SwitchState&amp;data=true</a>

Comme vous le constatez nous utilisons l’interface « /consumer ». L'argument "sentinelName" doit donc être « Consumer » et le nom du package est en fait un « friendly name » que nous appellerons ici « DemoPowershell ». Vous devez remplacer le « xxxx » par une clé d’accès valide, par exemple la même que celle utilisée par les ESP.

Notre Powershell est donc un « consommateur » et non un « package virtuel ». Il n’y a donc pas besoin de le déclarer dans la configuration de notre Constellation. Un consommateur peut envoyer ou recevoir des messages et consommer des StateObjects mais il ne peut pas écrire de log ni même publier des StateObjects.

Si vous souhaitez utiliser ce script sur une autre machine sur le serveur Constellation, remplacez dans l’URL « localhost » par l’adresse IP ou DNS de votre serveur Constellation.

Pour invoquer une URL en Powershell vous pouvez utiliser la command-let « Invoke-WebRequest ».

Créons alors un script Powershell que nous nommerons « SwitchRelay.ps1 » avec le code suivant :
<pre class="lang:powershell decode:true">param (
    [Parameter(Mandatory=$true)][bool]$state
 )
$res = Invoke-WebRequest ("http://localhost:8088//rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=DemoPowershell&amp;AccessKey=5863d9e4cbdf522eaa62e0747fceb1c5b249ba13&amp;scope=Package&amp;args=MyRelay&amp;key=SwitchState&amp;data=" + $state.ToString().ToLower())
Write-Host "SwitchState to" $state
</pre>
<b>Et voilà votre commande Powershell est capable d’invoquer un MessageCallback dans votre Constellation et donc de piloter le relais de votre ESP8266 !</b>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-60.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Script Powershell" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-60.png" alt="Script Powershell" width="454" height="92" border="0" /></a></p>