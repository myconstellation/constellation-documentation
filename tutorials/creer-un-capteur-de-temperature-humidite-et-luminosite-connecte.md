---
ID: 4588
post_title: >
  Créer un capteur de température,
  humidité et luminosité connecté
author: Sebastien Warin
post_date: 2017-05-10 17:55:52
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:12:07
---
Dans ce tutoriel créons un capteur de température, d’humidité et de luminosité connecté avec une page Web de visualisation des données en temps réel.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-52.png"><img class="" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Capteur connecté" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-52.png" alt="Capteur connecté" width="399" height="248" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-55.png"><img class="" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-55.png" alt="image" width="271" height="248" border="0" /></a></p>

<h3>Prérequis</h3>
<ul>
 	<li>Un Arduino connecté ou un ESP8266</li>
 	<li>Des capteurs (DHT11 pour l’humidité et la température, un TSL2561 pour la luminosité par exemple)</li>
 	<li>Un serveur Constellation</li>
</ul>
<h3>Etape 1 : le montage</h3>
Ici nous utiliserons un D1 Mini Pro (ESP8266) avec un capteur DHT11 combinant la mesure de la température et de l’humidité ainsi qu'un capteur TSL2561 pour la luminosité.

Les deux capteurs seront alimentés en 3,3v par la D1 Mini.

Le TSL2561 dispose d’une interface I²C (SDA et SDL qu’on connectera sur D1 et D2) et le DHT11 n’a besoin que d’une entrée digitale (qu’on connectera sur D3).
<h3>Etape 2 : la programmation</h3>
Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d’accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée “ESP8266” et le package virtuel se nomme “MySensor”.

Dans l’Arduino IDE, nous créons un nouveau projet à partir du Starter Kit Constellation pour ESP8266 dans lequel nous allons définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d’accès  puis nous configurerons le client Constellation en spécifiant l’identité de notre package virtuel, sa clé d’accès et l’adresse/port de notre serveur Constellation :
<pre class="lang:default decode:true">// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MON SSID";
char password[] = "macléWifi!!!!";

// Constellation client
Constellation&lt;WiFiClient&gt; constellation("X.X.X.X", 8088, "ESP8266", "MySensor", "xxxxxxxxxxxxxxxxx")</pre>
Encore une fois si tout cela est nouveau pour vous, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l’API Constellation pour Arduino/ESP8266</a>.

Lancez maintenant le « gestionnaire de bibliothèque » (menu <i>Croquis &gt; Inclure une bibliothèque &gt; Gérer les bibliothèques</i>) et installez les bibliothèques suivantes :
<ul>
 	<li>Adafruit Unified Sensor (par Adafruit)</li>
 	<li>DHT Sensor library (par Adafruit)</li>
 	<li>Adafruit TSL2561 (par Adafruit)</li>
</ul>
Une fois les librairies installées, ajoutez en entête de votre code :
<pre class="lang:default decode:true">#include &lt;Adafruit_TSL2561_U.h&gt;
#include &lt;DHT_U.h&gt;
#include &lt;Wire.h&gt;
</pre>
Déclarez ensuite, juste après la déclaration du client Constellation, le capteur TSL2561 et le DHT11 par les lignes :
<pre class="lang:cpp decode:true">DHT_Unified dht(D3, DHT11);
Adafruit_TSL2561_Unified tsl(TSL2561_ADDR_FLOAT);
</pre>
Déclarez également un entier pour définir l’intervalle de temps (en ms) entre deux mesures que nous fixons à 10 secondes par défaut :
<pre class="lang:cpp decode:true">int interval = 10000; //ms</pre>
Dans la méthode « <em>setup()</em> » configurez les I/O utilisés par l’I²C (D1 et D2 dans notre cas) :
<pre class="lang:cpp decode:true">Wire.begin(D1, D2);</pre>
Initialisez ensuite le capteur DHT11 par la ligne :
<pre class="lang:cpp decode:true">dht.begin();</pre>
Puis le capteur TSL2561 :
<pre class="lang:cpp decode:true">if(!tsl.begin())
{
  Serial.print("No TSL2561 detected!");
  while(1);
}
tsl.enableAutoRange(true);
tsl.setIntegrationTime(TSL2561_INTEGRATIONTIME_13MS);</pre>
Toujours dans la méthode «<em> setup</em> », récupérons les settings de notre package sur Constellation et affectons le setting « Interval » si défini sur Constellation (autrement c’est la valeur de 10 secondes déclarée ci-dessus qui sera utilisée) :
<pre class="lang:cpp decode:true">JsonObject&amp; settings = constellation.getSettings();
if(settings.containsKey("Interval")) {
  interval = settings["Interval"].as&lt;int&gt;();
}</pre>
Pour finir dans la boucle principale, nous allons prendre les mesures de nos capteurs et publier trois StateObjects avec une durée de vie de deux fois l’intervalle de temps :
<pre class="lang:cpp decode:true">void loop(void) {
  constellation.loop();
  static int lastBeat = 0;
  if(millis() - lastBeat &gt; interval) {
    lastBeat = millis();
    sensors_event_t event;
    // Luminosité
    tsl.getEvent(&amp;event);
    if (event.light) {
      constellation.pushStateObject("Light", event.light, interval * 2);
    }  
    // Temperature
    dht.temperature().getEvent(&amp;event);
    if (!isnan(event.temperature)) {
      constellation.pushStateObject("Temperature", event.temperature, interval * 2);
    }
    // Humidité
    dht.humidity().getEvent(&amp;event);
    if (!isnan(event.relative_humidity)) {
      constellation.pushStateObject("Humidity", event.relative_humidity, interval * 2);
    }
  }
}
</pre>
<strong>Et voilà votre capteur connecté est prêt !</strong>

Il ne reste plus qu’à téléverser votre programme et ouvrir votre Console Constellation pour constater qu’il est bien opérationnel :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-53.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-53.png" alt="image" width="404" height="149" border="0" /></a></p>
Ouvrez maintenant le <a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a> et rechercher le terme “MySensor” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-54.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-54.png" alt="image" width="404" height="154" border="0" /></a></p>
On retrouve bien, en temps réel, les trois mesures réalisées par notre ESP8266. N’hésitez pas à ouvrir ces StateObjects et cliquez sur « Subscribe » pour voir les mesures en temps réel dans la Console Constellation.
<h3>Etape 3 : Une page Web pour afficher votre capteur en temps réel</h3>
Créons maintenant une page Web pour afficher en temps réel les mesures de notre capteur connecté. Pour cela créez un fichier HTML sur votre poste avec une structure HTML de base.

Dans les entêtes ajoutez les scripts suivants :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https:/cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt;</pre>
Sur la balise &lt;html&gt; ajoutez l’attribut :
<pre class="lang:html5 decode:true">ng-app="MyDemoApp"</pre>
Et sur la balise &lt;body&gt; ajoutez l’attribut :
<pre class="lang:html5 decode:true">ng-controller="MyController"</pre>
Dans le corps de votre page, placez les valeurs vos StateObjects où bon vous semble. Par exemple :
<pre class="lang:html5 decode:true">&lt;div&gt;
    Temperature: &lt;p&gt;{{Temperature}}°C&lt;/p&gt;
    Humidity: &lt;p&gt;{{Humidity}}%&lt;/p&gt;
    Light: &lt;p&gt;{{Light}} lux&lt;/p&gt;
&lt;/div&gt;
</pre>
Pour finir ajoutez le script suivant dans votre page (dans une balise &lt;script&gt;):
<pre class="lang:javascript decode:true">var myDemoApp = angular.module('MyDemoApp', ['ngConstellation']);
myDemoApp.controller('MyController', ['$scope', 'constellationConsumer', function ($scope, constellation) {

  constellation.initializeClient("http://localhost:8088", "clé d'accès constellation", "MySensor WebPage");

    constellation.onConnectionStateChanged(function (change) {
      if (change.newState === $.signalR.connectionState.connected) {

        constellation.registerStateObjectLink("*", "MySensor", "*", "*", function (so) {
          $scope.$apply(function () {
           $scope[so.Name] = so.Value;
          });
        });
      }
    });

    constellation.connect();
  }]);
</pre>
N’oubliez pas de définir l’adresse de votre Constellation ainsi que la clé d’accès pour vous connecter à votre Constellation lors de l’appel de la méthode « constellation.initializeClient »

<b>Vous obtenez en quelques minutes une page Web permettant d’afficher en temps réel les mesures de température, d’humidité et de luminosité réalisées par votre ESP8266 !</b>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-55.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-55.png" alt="image" width="404" height="369" border="0" /></a></p>

<h3>Etape 4 (optionnelle) : créer un package .NET pour exploiter les données du capteur</h3>
La page Web réalisée ci-dessus permet d’afficher en temps réel les valeurs de vos StateObjects.

Si maintenant vous souhaitez récupérer la valeur de ces StateObject dans un programme pour réagir en fonction de certaines valeurs (par exemple alerter si T° trop froide ou allumer des lumières si la luminosité est trop faible) ou bien tout simplement enregistrer ces valeurs dans un fichier, une base de données ou un service de cloud, suivez la suite.

Pour cet exemple, réalisons un programme en C#. On part du principe que vous avez installé Visual Studio et <a href="/getting-started/installer-constellation/">le SDK Constellation depuis le « Web Platform Installer »</a>.

Créez alors un nouveau projet de type « Constellation Package Console » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-56.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-56.png" alt="image" width="404" height="266" border="0" /></a></p>
Dans votre classe, ajoutez simplement des propriétés pour chaque StateObject avec l’attribut « StateObjectLink » pour lier ces propriétés .NET aux StateObjects de votre capteur (servez-vous du snippet « stateobjectlink »).

Par exemple pour inclure le StateObject « Light » dans notre classe C# :
<pre class="lang:csharp decode:true">[StateObjectLink("MySensor", "Light")]
public StateObjectNotifier Light { get; set; }</pre>
Vous pouvez maintenant afficher sa valeur à tout moment, par exemple :
<pre class="lang:csharp decode:true">PackageHost.WriteInfo($"Luminosité {this.Light.Value.LastUpdate} = {this.Light.DynamicValue} lux");</pre>
La propriété « Light » de votre classe contiendra toujours la dernière valeur du StateObject connu dans votre Constellation.

Vous pouvez donc manipuler la valeur mesurée par votre ESP8266 dans votre code C# comme une simple propriété .NET de votre code.

Vous pouvez également vous abonner à chaque mise à jour du StateObject pour déclencher une action grâce à l’évènement « ValueChanged » :
<pre class="lang:csharp decode:true">this.Light.ValueChanged += (s, e) =&gt;
{
    PackageHost.WriteInfo($"Nouvelle mesure à {this.Light.Value.LastUpdate} = {this.Light.DynamicValue} lux");
};
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-57.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-57.png" alt="image" width="404" height="206" border="0" /></a></p>