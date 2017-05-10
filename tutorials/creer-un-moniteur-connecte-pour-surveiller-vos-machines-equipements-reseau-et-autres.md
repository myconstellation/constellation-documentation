---
ID: 4616
post_title: 'Cr&eacute;er un moniteur connect&eacute; pour surveiller vos machines, &eacute;quipements r&eacute;seau et autres capteurs'
author: Sebastien Warin
post_date: 2017-05-10 17:33:57
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-un-moniteur-connecte-pour-surveiller-vos-machines-equipements-reseau-et-autres/
published: true
post_modified: 2017-05-10 17:33:57
---
Créons dans ce tutoriel un petit moniteur pour afficher en temps réel des StateObjects de votre Constellation sur un écran OLED comme par exemple la consommation CPU d’une de vos machines Windows récupérée grâce au package <a href="/package-library/hwmonitor/">HWMonitor</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-61.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Monitor connecté" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-61.png" alt="Monitor connecté" width="454" height="295" border="0" /></a></p>

<h3>Prérequis</h3>
<ul>
 	<li>Un ESP8266 ou un Arduino avec interface réseau</li>
 	<li>Un petit écran OLED</li>
 	<li>Un serveur Constellation</li>
</ul>
<h3>Etape 1 : le montage</h3>
Nous utilisons ici un ESP8266 de type D1 Mini Pro. Nous pouvons alors utiliser un écran OLED en Shield qu'on a juste à "déposer" sur le D1 Mini :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-62.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Shield OLED" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-62.png" alt="Shield OLED" width="454" height="362" border="0" /></a></p>
<p style="text-align: left;" align="center">Autre solution, utilisez un écran OLED classique :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-63.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Ecran OLED I2C" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-63.png" alt="Ecran OLED I2C" width="454" height="295" border="0" /></a></p>
Ces deux écrans OLED sont pilotables en I²C. Pour les connexions du 2ème écran, il suffit de connecter les masses (GND) ensemble, la pin « Vcc » de l’écran sur la pin « 3V3 » de la D1 Mini et le « SLK » sur « D2 » et « SDA » sur « D1 ».
<h3>Etape 2 : la programmation</h3>
Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d’accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée “ESP8266” et le package virtuel se nomme “MyMonitor”.

Dans l’Arduino IDE, nous créons un nouveau projet à partir du Starter Kit Constellation pour ESP8266 dans lequel nous allons définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d’accès  puis nous configurerons le client Constellation en spécifiant l’identité de notre package virtuel, sa clé d’accès et l’adresse/port de notre serveur Constellation :
<pre class="lang:default decode:true">// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MON SSID";
char password[] = "macléWifi!!!!";
 
// Constellation client
Constellation&lt;WiFiClient&gt; constellation("X.X.X.X", 8088, "ESP8266", "MyMonitor", "xxxxxxxxxxxxxxxxx")</pre>
Encore une fois si tout cela est nouveau pour vous, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l’API Constellation pour Arduino/ESP8266</a>.

Maintenant que la coquille de notre package virtuel sur ESP8266 est prête, installons la librairie « ESP8266 Oled Driver for SSD1306 » pour pouvoir piloter l'écran OLED depuis le gestionnaire de bibliothèque :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-64.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation de la librarie" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-64.png" alt="Installation de la librarie" width="404" height="229" border="0" /></a></p>
Il vous suffit ensuite d’importer cette librairie et déclarer l’objet « display » de cette façon :
<pre class="lang:default decode:true">#include &lt;Wire.h&gt;
#include "SSD1306.h"
SSD1306  display(0x3c, D1, D2);
</pre>
Au démarrage de votre programme, dans la méthode « <em>setup()</em> », initialisez votre écran :
<pre class="lang:cpp decode:true">display.init();
display.flipScreenVertically();
display.setFont(ArialMT_Plain_10);
</pre>
Toujours la méthode « <em>setup()</em> » et une fois connecté au Wifi, récupérez le setting « Machine » en prenant soin de recopier la valeur par les lignes :
<pre class="lang:cpp decode:true">JsonObject&amp; settings = constellation.getSettings();
static String machine = String(settings["Machine"].asString());
</pre>
Il ne nous reste plus qu’à enregistrer un ou plusieurs « <em>StateObjectLink</em> » pour récuperer les informations que vous souhaitez suivre.

Dans notre exemple nous allons enregistrer un « <em>StateObjectLink » </em>vers le StateObject « /intelcpu/0/load/0 » du package « HWMonitor » pour la sentinelle définie dans le setting « Machine ».

A chaque mise à jour du StateObject, nous afficherons sa valeur textuellement avec la méthode « <em>drawString</em> » ainsi que dans une barre de progression avec la méthode « <em>drawProgressBar</em> » :
<pre class="lang:cpp decode:true">constellation.registerStateObjectLink(machine.c_str(), "HWMonitor", "/intelcpu/0/load/0", [](JsonObject&amp; so) {
    int cpuUsage = so["Value"]["Value"].as&lt;int&gt;();
    // Vider l'écran
    display.clear();
    // Afficher une progressbar
    display.drawProgressBar(0, 32, 120, 10, cpuUsage);
    // Afficher la consommation du CPU
    display.setTextAlignment(TEXT_ALIGN_CENTER);
    display.drawString(64, 15, "CPU = " + String(cpuUsage) + "%");
    // Dessiner sur l'écran OLED
    display.display();
  )});
</pre>
<h3>Demos</h3>
Avant de lancer votre code, assurez-vous d’avoir bien déclaré le setting « Machine » avec le nom de la sentinelle sur laquelle est déployé le package « HWMonitor » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-65.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Setting du package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-65.png" alt="Setting du package" width="404" height="194" border="0" /></a></p>
Il ne vous reste plus qu’à téléverser votre programme sur la D1 Mini et observer le résultat.
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-61.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MyMonitor en action" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-61.png" alt="MyMonitor en action" width="454" height="295" border="0" /></a></p>
<b>Et voilà comment en quelque ligne de code vous avez un moniteur temps réel de votre consommation CPU Windows sur un écran OLED piloté par un ESP8266.</b>
<h3>Pour aller plus loin</h3>
Vous pouvez bien entendu adapter le ou les StateObjects à afficher. Nous avons dans ce tutoriel affiché la consommation CPU mesurée par le package « <a href="/package-library/hwmonitor/">HWMonitor </a>» mais ce même package met à disposition beaucoup d’autre StateObject : consommation RAM, des disques dur, réseau, température du châssis, consommation énergétique, etc. N’hésitez pas à utiliser le <a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a> pour explorer les StateObjects de ce package.

Il existe aussi plein d’autres packages qui produisent des StateObjects que vous pourrez afficher sur votre écran OLED. Quelques exemples : le package « NetAtmo » pour les mesures sur la T°, humidité, taux de CO², ambiance sonore, pluviomètre et autre ; le package « Nest » sur l’état de votre chauffage, T° ambiante et de consigne ; package « Tesla » pour récupérer en temps réel toutes les données sur votre voiture ; package « DayInfo » pour connaitre les heures du soleil ou la fête du jour ; le package « NetworkTools » où vous pourrez superviser des services TCP ou les temps de réponse de vos sites Web ou encore le package « SNMP » pour pouvoir afficher sur votre écran OLED des informations issues du SNMP, comme par exemple la bande passante de votre routeur.

Bref vous l’aurez compris, la seule limite sera votre imagination !