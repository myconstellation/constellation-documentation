---
ID: 5979
post_title: >
  Créer une serrure connectée à
  Constellation
author: Sebastien Warin
post_date: 2018-05-16 16:05:31
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-une-serrure-connectee-constellation/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "0"
update_discourse_topic:
  - "0"
post_modified: 2018-05-16 16:05:31
---
<em>Article proposé par Olivier MOUILLOT, Mohammed TAHRI JOUTEI HASSANI et Gaëtan DELABY</em>

Vous attendez quelqu’un chez vous mais vous êtes occupé à faire autre chose. Cette serrure connectée vous permettra de contrôler l’ouverture de la porte d’entrée depuis n’importe quelle pièce de votre domicile, et cela en un clic grâce à votre smartphone. Nous allons voir pas à pas comment la mettre en place simplement.

[toc]
<h2>Prérequis</h2>
Pour réaliser ce projet, il vous faut :
<ul>
 	<li>Une gâche électrique 12V / 500 A</li>
 	<li>Un ESP8266</li>
 	<li>Un régulateur de tension LM1117</li>
 	<li>Un transistor NPN TIP 110</li>
 	<li>Une résistance 5,6 KΩ</li>
 	<li>Deux condensateurs 0,1 µF et 0,33 µF</li>
 	<li>Du fil électrique</li>
 	<li>Une batterie 12V</li>
 	<li>Un serveur Constellation (Un ordinateur pourra faire l’affaire)</li>
</ul>
<h2>Etape 1 : réaliser une serrure connectée</h2>
Dans un premier temps, il nous a fallu trouver une serrure adaptée au développement du projet sur laquelle nous pouvions brancher nos fils électriques. Nous avons donc opté pour la serrure suivante que vous pourrez vous procurer via <a href="https://www.amazon.fr/Extel-90301-3-%C3%A9lectrique-passage-serrure/dp/B002LS6OTA/ref=sr_1_2?ie=UTF8&amp;qid=1511196788&amp;sr=8-2&amp;keywords=g%C3%A2che+%C3%A9lectrique&amp;dpID=31-7q-yaD0L&amp;preST=_SX300_QL70_&amp;dpSrc=srch">ce lien </a>.
<p align="center"><a href="https://www.amazon.fr/Extel-90301-3-%C3%A9lectrique-passage-serrure/dp/B002LS6OTA/ref=sr_1_2?ie=UTF8&amp;qid=1511196788&amp;sr=8-2&amp;keywords=g%C3%A2che+%C3%A9lectrique&amp;dpID=31-7q-yaD0L&amp;preST=_SX300_QL70_&amp;dpSrc=srch"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Extel Weca 90301.3 Gâche électrique sans passage de serrure" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image.png" alt="Extel Weca 90301.3 Gâche électrique sans passage de serrure" width="146" height="331" border="0" /></a></p>
Pour de plus amples informations techniques sur la gâchette, vous pouvez également regarder <a href="http://www.produktinfo.conrad.com/datenblaetter/75000-99999/094195-an-01-ml-gache_elec_de_en_fr_nl.pdf">ici</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="serrure de dessus" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb.png" alt="serrure de dessus " width="354" height="266" border="0" /></a></p>
Il faut ensuite créer le câblage nécessaire au fonctionnement de la serrure. Notre principal problème a été de convertir l’alimentation indispensable de 12V pour la serrure en une alimentation de 3,3V nécessaire à l’utilisation de notre ESP. A l’aide du régulateur de tension, nous avons réalisé le montage suivant en entrée de l’ESP.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title=" régulateur de tension" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-1.png" alt=" régulateur de tension" width="400" height="138" border="0" /></a></p>
Afin de laisser passer le courant et de faire fonctionner la gâchette, le transistor TIP110 en série avec une résistance 5,6kΩ se placent en sortie GPIO de l’ESP. Une LED peut être également placée à cette même sortie afin de visualiser l’état de la serrure.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="sortie ESP" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-2.png" alt="sortie ESP" width="350" height="232" border="0" /></a></p>
Enfin, voici le montage final reprenant tous les composants :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Schéma global du circuit" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-3.png" alt="Schéma global du circuit" width="350" height="184" border="0" /></a></p>

<h2>Etape 2 : Programmation</h2>
Vous avez fait la moitié du travail, voyons ensemble comment coder notre ESP8266.

La fonction de base de la gâche électrique est assez simple : couper le courant à travers la serrure pour la fermer ou le laisser passer le courant afin d’ouvrir la serrure. En premier lieu, nous avons développé le code Arduino servant à faire faire passer du courant ou non dans la gâche. Pour cela nous devons dans un premier temps connecter notre ESP8266 à Constellation. Pour découvrir comment connecter votre ESP à Constellation, <a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">suivez ce guide</a>.

En ce qui concerne le code Arduino permettant de contrôler la serrure, vous en trouverez ci-dessous l’intégralité.
<pre title="Code Arduino" class="lang:cpp decode:true">#include &lt;Constellation.h&gt;

// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MY-WIFI";
char password[] = "xxxxxxxx";

define LOCK_PIN D3

// Constellation client
Constellation&lt;WiFiClient&gt; constellation("172.20.10.2", 8088, "d1mini", "demoisen", "xxxxxxxx");

void setup(void) {
  Serial.begin(115200);  delay(10);

  //SET I/O 
  pinMode(LOCK_PIN, OUTPUT);
  // INITIAL mode
  digitalWrite(LOCK_PIN , LOW);

  // Set Wifi mode
  if (WiFi.getMode() != WIFI_STA) {
    WiFi.mode(WIFI_STA);
    delay(10);
  }
  
  // Connecting to Wifi  
  Serial.print("Connecting to ");
  Serial.println(ssid);  
  WiFi.begin(ssid, password);  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("WiFi connected. IP: ");
  Serial.println(WiFi.localIP());

  constellation.pushStateObject("State", false);
  
  constellation.registerMessageCallback("OpenDoor", MessageCallbackDescriptor().setDescription("Ouvres la porte"),
      [](JsonObject&amp; message) {
            digitalWrite(LOCK_PIN, HIGH);
            constellation.pushStateObject("State", true);
            delay(4000);
            digitalWrite(LOCK_PIN, LOW);   
            constellation.pushStateObject("State", false);    
      });
  
  // Declare the package descriptor
  constellation.declarePackageDescriptor();

  // WriteLog info
  constellation.writeInfo("Virtual Package on '%s' is started !", constellation.getSentinelName());  
}

void loop(void) {
  constellation.loop();
}</pre>
L’étape suivante a été d’utiliser la librairie Constellation <a href="https://developer.myconstellation.io/client-api/arduino-esp-api/recevoir-des-messages-et-exposer-des-methodes-messagecallback-sur-arduino-esp/">pour ajouter un « MessageCallback</a> » afin de contrôler le GPIO de la gâchette, <a href="https://developer.myconstellation.io/client-api/arduino-esp-api/produire-des-stateobjects-depuis-arduino-esp/">couplé à un « StateObject</a> » permettant de maintenir l’état de la serrure dans Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="StateObject" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-4.png" alt="StateObject" width="354" height="258" border="0" /></a></p>
Constellation va aller garder en permanence l’état de la serrure grâce au StateObject « State ».

Dorénavant, n’importe qui connecté au serveur peut se servir du MessageCallback « OpenDoor» exposé par l’ESP que nous avons utilisé pour faire permuter l’ouverture et la fermeture automatique de la serrure. Un délai de 4 secondes est programmé avant que la serrure ne se referme après ouverture de la gâche. Nous avons ajouté cette fonctionnalité afin d’ajouter une partie sécurité à notre dispositif.
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MessageCallback" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-6.png" alt="MessageCallback" width="350" height="73" border="0" /></p>
&nbsp;

On obtient alors simplement le pilotage de notre gâchette avec Constellation, dont l’état pourra être contrôlé grâce à une page Web qu’on vous présente dans la partie suivante.
<h2>Etape 3 : Piloter sa serrure à l’aide d’une page web</h2>
Pour cette dernière étape qui consiste à piloter notre serrure connectée grâce à une page Web. Il s’agit principalement de l’interface via laquelle vous pourrez facilement décider de l’ouverture ou la fermeture de la serrure.

Voici le code HTML que nous avons mis en place :
<pre title="Code HTML" class="lang:html5 decode:true">&lt;!DOCTYPE html&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" ng-app="MyDemoApp"&gt;
&lt;head&gt;
	&lt;link rel="stylesheet" href="bruh.css" /&gt;
    &lt;title&gt;Serrure connectée&lt;/title&gt;
	&lt;link rel="stylesheet" href="style.css" /&gt;
    &lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.2.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/ngConstellation-1.8.2.min.js"&gt;&lt;/script&gt;
    
    &lt;script&gt;
        var myDemoApp = angular.module('MyDemoApp', ['ngConstellation']);
        myDemoApp.controller('MyController', ['$scope',  'constellationConsumer', function ($scope, constellation) {
  
          constellation.initializeClient("http://172.20.10.2:8088", "xxxxxxxxxx", "WebPage");
          constellation.onConnectionStateChanged(function (change) {
            if (change.newState === $.signalR.connectionState.connected) {
                console.log("Je suis connecté !");
                constellation.requestSubscribeStateObjects("*", "DoorLock", "*", "*");
                }
            });

          constellation.onUpdateStateObject(function (stateObject) {
              console.log(stateObject);
              $scope[stateObject.Name] = stateObject.Value;
              $scope.$apply();
          });
          
          $scope.openDoor = function(){
                constellation.sendMessage({Scope: 'Package', Args: ['DoorLock']}, 'OpenDoor');
            };

            
          constellation.connect();

                }]);
    &lt;/script&gt;
 
&lt;/head&gt;

&lt;body ng-controller="MyController"&gt;
        
    &lt;button ng-click="openDoor()"&gt; OPEN &lt;/button&gt;

&lt;/body&gt;
&lt;/html&gt;

</pre>
Avec quelques lignes de CSS, le rendu final de notre page est le suivant et on ne peut plus simple d’utilisation pour le pilotage :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Page Web" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-5.png" alt="Page web" width="354" height="169" border="0" /></a></p>
Pour la suite il suffit d’héberger cette page HTML sur un serveur Web quelconque et vous pourrez ouvrir votre porte depuis un PC, un smartphone, une table ou autre.

Et comme l’ouverture de la porte est exposer comme un <a href="https://developer.myconstellation.io/concepts/messaging-message-scope-messagecallback-saga/">MessageCallback</a>, vous pourrez l’ouvrir depusi un code <a href="https://developer.myconstellation.io/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">C#,</a> <a href="https://developer.myconstellation.io/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/">Python</a>, <a href="https://developer.myconstellation.io/client-api/arduino-esp-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-depuis-arduino-esp/">Arduino</a> ou même un simple <a href="https://developer.myconstellation.io/client-api/rest-api/interface-rest-consumer/#Envoyer_des_messages">appel HTTP</a> à l’API Constellation. N’hésitez pas à utiliser le “<a href="https://developer.myconstellation.io/constellation-platform/constellation-console/messagecallbacks-explorer/">Code Generator</a>” de la console Constellation.

Au final avec une gâche électrique, un ESP8266, une Constellation et quelques lignes d’Arduino, on est capable d’ouvrir une porte depuis tous type d’application.