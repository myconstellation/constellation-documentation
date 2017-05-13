---
ID: 4819
post_title: 'Cr&eacute;er un capteur de luminosit&eacute; dans une prise 220v avec un ESP8266'
author: Sebastien Warin
post_date: 2017-05-13 15:35:14
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-un-capteur-de-luminosite-dans-une-prise-220v/
published: true
post_modified: 2017-05-13 15:41:17
---
Ce tutoriel est sur le principe identique à celui-ci : <a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/">Créer un capteur de température, humidité et luminosité connecté</a> sauf que nous allons utiliser un ESP-01 beaucoup plus petit et moins cher qu'un D1 Mini Pro.

Nous assemblerons l'ESP8266 et le capteur de luminosité TSL2561 dans une prise 220v avec un convertisseur 220v -&gt; 3v3.
<h3>Prérequis</h3>
<ul>
 	<li>Un serveur Constellation</li>
 	<li>Un ESP8266 comme l'ESP-01 (entre 1,5€ et 2€)</li>
 	<li>Un capteur de luminosité TSL2561 (environ 2€)</li>
 	<li>Un convertisseur 220v -&gt; 3v3 (environ 2€)</li>
 	<li>Un boitier/prise 220v (entre 5 et 15 €)</li>
</ul>
<h3>Le montage</h3>
Le capteur TSL2561 se connecte en I²C, il suffit donc de l'alimenter en 3v3 et de connecter les deux GPIO de l'ESP-01 (GPIO-0 et GPIO-2) aux pins SCL et SDA du capteur.

Le schéma de connexion est donc le suivant :
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Lux Sensor" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig-1_thumb.png" alt="Lux Sensor" width="450" height="285" border="0" /></a></p>
Sur le bornier 220v de notre prise nous allons souder deux fils (la phase et le neutre) vers l'entrée convertisseur 220v AC. En sortie du convertisseur, on a une tension de 3v3 DC que nous connecterons à l'ESP-01 ainsi qu'au TSL2561.

N'oubliez pas également de relier le port "CH_PD" de l'ESP-01 au 3v3 pour activer le Wifi.

Pour plus d’information, je vous invite <a href="https://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/" target="_blank" rel="noopener noreferrer">à lire cet article</a>.
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-84.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Lux Sensor" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-84.png" alt="Lux Sensor" width="454" height="403" border="0" /></a></p>
Disposez les différents composants dans le boitier en prenant soin d'isoler les parties 220v (avec de la colle thermofusibles ou du ruban isolant).
<h3>Le code</h3>
L'ESP-01 ne dispose pas comme le D1 Mini Pro vu dans<a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/"> ce tutoriel</a> d'une interface USB intégrée. Il vous faudra donc une interface USB/FTDI pour le programmer. Pour plus d’information, je vous invite <a href="https://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/" target="_blank" rel="noopener noreferrer">à lire cet article</a>.

A l'inverse du <a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/">tutoriel précédent</a>, ce code utilise la libraire <a href="https://github.com/adafruit/TSL2561-Arduino-Library" data-pjax="#js-repo-pjax-container">TSL2561-Arduino-Library</a> et non la "Unifed Sensor Library" afin de nous permettre de récupérer le Broadband et l'IR en plus des Lux.

Il faudra donc installer la libraire "<em>TSL2561-Arduino-Library</em>" depuis le Gestionnaire de bibliothèque. De plus on utilise ici "ArduinoThread" pour ordonner les mesures à intervalle régulier. Vous devrez également installer cette librairie.

<strong>Attention</strong> : si tout cela est nouveau pour vous, je vous recommande <a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l’API Constellation pour Arduino/ESP8266</a>.

Le code complet est :
<pre class="lang:default decode:true">#include &lt;ESP8266WiFi.h&gt;
const char* ssid     = "MON SSID";
const char* password = "lacléeWifi!!!!";

#include &lt;Constellation.h&gt;
Constellation&lt;WiFiClient&gt; constellation("IP ou DNS du serveur Constellation", 8088, "ESP-LightSensorSalon", "LightSensor", "123456789");

#include &lt;Wire.h&gt;
#include &lt;TSL2561.h&gt;
TSL2561 tsl(TSL2561_ADDR_FLOAT); 

#include &lt;Thread.h&gt;
Thread thrPushLux = Thread();

void pushLuxOnConstellation() {  
  uint32_t lum = tsl.getFullLuminosity();
  uint16_t ir, full;
  ir = lum &gt;&gt; 16;
  full = lum &amp; 0xFFFF;
  uint32_t lux = tsl.calculateLux(full, ir);
  
  constellation.pushStateObject("Lux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir), "LightSensor.Lux", 300);
}

void setup(void) {
  Serial.begin(115200);  delay(10);

  if (tsl.begin()) {
    Serial.println("Found sensor");
  } else {
    Serial.println("No sensor?");
    while (1);
  }
  
  // You can change the gain on the fly, to adapt to brighter/dimmer light situations
  //tsl.setGain(TSL2561_GAIN_0X);         // set no gain (for bright situtations)
  tsl.setGain(TSL2561_GAIN_16X);      // set 16x gain (for dim situations)  
  // Changing the integration time gives you a longer time over which to sense light
  // longer timelines are slower, but are good in very low light situtations!
  //tsl.setTiming(TSL2561_INTEGRATIONTIME_13MS);  // shortest integration time (bright light)
  //tsl.setTiming(TSL2561_INTEGRATIONTIME_101MS);  // medium integration time (medium light)
  tsl.setTiming(TSL2561_INTEGRATIONTIME_402MS);  // longest integration time (dim light)

  // Set Wifi mode
  if (WiFi.getMode() != WIFI_STA) {
    WiFi.mode(WIFI_STA);
    delay(10);
  }
  
  // Connect to Wifi  
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("WiFi connected. IP: ");
  Serial.println(WiFi.localIP());
  Serial.println(constellation.getSentinelName());
  
  JsonObject&amp; settings = constellation.getSettings();
  int interval = 10000; //ms
  if(settings.containsKey("Interval")) {
    interval = settings["Interval"].as&lt;int&gt;();
  }

  thrPushLux.onRun(pushLuxOnConstellation);
  thrPushLux.setInterval(interval);

  constellation.addStateObjectType("LightSensor.Lux", TypeDescriptor().setDescription("Lux data informations").addProperty("Broadband", "System.Int32").addProperty("IR", "System.Int32").addProperty("Lux", "System.Int32"));
  constellation.declarePackageDescriptor();
  
  constellation.writeInfo("ESP LightSensor '%s' is started (Push interval: %d sec)", constellation.getSentinelName(), interval);  
}

void loop(void) {
  if(thrPushLux.shouldRun()){
    thrPushLux.run();
  }
}
</pre>
Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d’accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée “ESP-LightSensorSalon” et le package virtuel se nomme “LightSensor”.

Ensuite il faut définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d’accès puis nous configurerons le client Constellation en spécifiant l’identité de notre package virtuel, sa clé d’accès et l’adresse/port de notre serveur Constellation.

Dans la méthode "<em>setup()</em>", on initialise le capteur TSL puis on se connecte au Wifi et enfin on déclare l'intervalle de temps entre chaque mesure, par défaut de 10 secondes qu'on peut modifier en ajoutant un setting "Interval" sur notre package virtuel.

On configure ensuite un "thread" qui exécutera à l'intervalle de temps spécifié la méthode "<em>pushLuxOnConstellation</em>". Pour finir on déclare la description du StateObject par la méthode "<em>constellation.addStateObjectType</em>" et on affiche un log. Dans la <em>loop()</em>, on déclenche le thread.

La méthode "<em>pushLuxOnConstellation</em>" récupère les données du capteur et publie le StateObject "Lux" avec les trois propriétés :
<pre class="lang:default decode:true">constellation.pushStateObject("Lux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir), "LightSensor.Lux", 300);</pre>
Une fois le programme téléversé, votre prise publiera le StateObject à l'intervalle régulier la luminosité ambiante.
<h3>Exploiter le capteur</h3>
Comme vous le savez, une fois les données mesurées publiées dans un StateObject n'importe quel système connecté dans Constellation peut récupérer ou s'abonner en temps réel au StateObject.

On peut donc écrire des pages Web, des scripts, des packages .NET ou Python, Arduino, etc… qui exploiteront ces mesures en temps réel.

Je vous renvoie sur <a href="/tutorials/un-capteur-de-luminosite-exterieur-pilote-par-raspberry/#Exploitez_les_donnees">cette page</a> ou<a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/#Etape_3_Une_page_Web_pour_afficher_votre_capteur_en_temps_reel"> cette autre page</a> pour avoir quelques idées.

Pour ma part ce capteur installé dans le salon est affiché en temps réel sur <a href="https://sebastien.warin.fr/2015/07/15/3033-s-panel-une-interface-domotique-et-iot-multi-plateforme-avec-cordova-angularjs-et-constellation-ou-comment-crer-son-dashboard-domotique-mural/">un dashboard Web</a>,  stocké dans ElasticSearch et Cacti et exploité dans un package .NET nommé "MyBrain" car cette information sur la luminosité ambiante sert dans beaucoup de scénario : gestion des éclairages ou des volets.