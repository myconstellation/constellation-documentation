---
ID: 2342
post_title: 'Connecter un Arduino ou un ESP8266 &agrave; Constellation'
author: Sebastien Warin
post_date: 2016-08-19 10:22:50
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/
published: true
post_modified: 2017-10-12 15:08:59
---
Vous pouvez connecter tout type d’objet ou système dans Constellation à partir du moment où vous disposez d’une connectivité IP pour réaliser des appels HTTP.
<h3>Introduction</h3>
Par exemple si vous disposez d’un Arduino il vous faudra un shield Ethernet ou Wifi bien que ces derniers ont été retirés du support Arduino.

Arduino propose en alternative des modèles déjà équipés d’une connectivité Ethernet et/ou Wifi comme le <a href="https://www.arduino.cc/en/Main/ArduinoMKR1000">MKR1000</a>, le <a href="https://www.arduino.cc/en/Main/ArduinoYunShield">Yun Shield</a> ou le <a href="https://www.arduino.cc/en/Main/ArduinoWiFiShield101">Wifi Shield 101</a>.
<p align="center"><a href="https://www.arduino.cc/en/Main/ArduinoMKR1000"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-25.png" alt="image" width="240" height="145" border="0" /></a><a href="https://www.arduino.cc/en/Main/ArduinoYunShield"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-26.png" alt="image" width="225" height="145" border="0" /></a><a href="https://www.arduino.cc/en/Main/ArduinoWiFiShield101"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-27.png" alt="image" width="176" height="145" border="0" /></a></p>
<p align="left">En alternative vous avez les ESP8266 que j’ai <a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/">pu vous présenter sur mon blog personnel</a>.</p>

<blockquote>
<p align="left">Ce microcontrôleur est cadencé à 80Mhz par un processeur 32bits RISC avec 96K de RAM et une mémoire flash de 512Ko à 4Mo selon les modèles.</p>
<p align="left">Il dispose d’une connectivité Wifi 802.11 b/g/n supportant le WEP, WPA/2/WPS et réseau ouvert et 16 GPIO dont le support du SPI, I²C, UART et un port ADC (pour les I/O analogiques).</p>
</blockquote>
<p align="center"><a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-22.png" alt="image" width="204" height="132" border="0" /></a></p>
<p align="left">Vous trouverez différents modèles sur internet, comme ceux produit par AI-Thinker mais il faudra réaliser vous même la carte de programmation comme <a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/#prettyPhoto">je l’explique sur mon blog</a> ou bien des modules “tout en un” comme ceux d’<a href="https://www.adafruit.com/product/2821">Adafruit</a> ou <a href="https://www.sparkfun.com/products/13231">Sparkfun</a> disposant d’un port USB intégré pour une programmation facile :</p>
<p align="center"><a href="https://www.adafruit.com/product/2821"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-28.png" alt="image" width="240" height="185" border="0" /></a> <a href="https://www.sparkfun.com/products/13231"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-29.png" alt="image" width="231" height="185" border="0" /></a></p>
<p align="left">Si vous débutez je vous recommande la Wemos D1 Mini qui intègre un ESP8266 avec 4M de mémoire Flash, 11 I/O digitales et 1 analogique (ADC) ainsi qu’une interface USB le tout pour moins de 5€ :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/mini_v2.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="mini_v2" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/mini_v2_thumb.jpg" alt="mini_v2" width="305" height="211" border="0" /></a></p>
<p align="left">Pour bien comprendre les ESP8266, je vous recommande une nouvelle fois mon article <a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/#prettyPhoto"><strong>Découverte des ESP8266 : le microcontrôleur connecté par Wifi pour 2€ au potentiel phénoménal avec Constellation</strong></a></p>
<p align="left">Le plus important étant que les ESP8266 peuvent être programmés comme les Arduino avec l’IDE Arduino.</p>

<h3 align="left">Arduino/ESP8266 = package virtuel</h3>
Ce <a href="/concepts/sentinels-packages-virtuels/">concept a déjà été abordé ici</a>. En clair le micro-programme d’un Arduino ou d’un ESP est stocké dans la puce elle même et il n’y a pas de concept de “processus” sur ce genre de device. Il n’est donc pas possible d’avoir une véritable sentinelle qui tourne ce genre d’objet pour déployer et superviser des packages Constellation à la volée comme sur un système Windows ou Linux.

C’est pour cela qu’on parle de sentinelle et de package “<strong>virtuel</strong>” : on déclare bien dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/">configuration de notre Constellation</a> une sentinelle (par exemple nommée “Arduino” et associée à un credential) et contenant un package (par exemple nommé  “TemperatureSensor”). Ce package peut être joint à des groupes et avoir des settings qui lui sont propres.

Dans le microprogramme téléversé sur votre Arduino/ESP vous allez vous connecter en spécifiant le nom de la sentinelle et du package virtuel (ici Arduino/TemperatureSensor) et la clé d’accès associée. Votre device sera alors connecté à Constellation comme étant le package “TemperatureSensor” sur la sentinelle “Arduino”. Comme n'importe quel package il pourra produire des logs, publier des StateObjects, récupérer ses settings, envoyer ou recevoir des messages ou encore consommer les StateObjects d’autres packages.

Techniquement parlant l’Arduino/ESP utilise <a href="/client-api/rest-api/interface-rest-constellation/">l’API REST “Constellation”</a>.
<h3>Installer la librairie Constellation dans l’IDE Arduino</h3>
Ouvrez le « Gestionnaire de bibliothèque » dans le menu « <i>Croquis &gt; Inclure une bibliothèque &gt; Gérer les bibliothèques</i> » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/12/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/12/image_thumb-2.png" alt="image" width="450" height="274" border="0" /></a></p>
Avec le moteur de recherche, recherchez le terme « <i>ArduinoJson</i> » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/12/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/12/image_thumb-3.png" alt="image" width="450" height="253" border="0" /></a></p>
Vous trouverez lors la bibliothèque « ArduinoJson » de Benoit Blanchon (dépendance de la librairie Constellation) ainsi que la librairie Constellation.

Pour ces deux bibliothèques, cliquez sur le bouton « Installer » (ou « Mise à jour » si la librairie est déjà installée).
<h3>Hello World, Hello Constellation</h3>
Commencez par créer un nouveau sketch et ajoutez la librairie Constellation :
<pre class="lang:cpp decode:true">#include &lt;Constellation.h&gt;</pre>
Vous pouvez maintenant déclarer le client “Constellation” en l’initialisant avec l’adresse (IP ou DNS) de votre serveur Constellation, le port et les informations de connexion de Constellation, c’est à dire le nom de votre sentinelle et package ainsi que la clé d’accès que vous aurez déclarés sur votre serveur :
<pre class="lang:cpp decode:true">Constellation&lt;WiFiClient&gt; constellation("IP or DNS du serveur constellation", 8088, "MyVirtualSentinel", "MyVirtualPackage", "123456789");</pre>
Pour ajouter <a href="/constellation-platform/constellation-console/gerer-sentinelles-avec-la-console-constellation/#Ajouter_une_sentinelle">une sentinelle virtuelle</a> cliquez sur le bouton “Add sentinel” sur la page “Sentinels” de la Console Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-61.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-50.png" alt="image" width="350" height="111" border="0" /></a></p>
<p align="left">Puis ajoutons un <a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Les_packages_virtuels">package virtuel</a> en cliquant sur le bouton contextuel de notre sentinelle “Deploy package” ou directement depuis la package “Packages” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-62.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-51.png" alt="image" width="350" height="124" border="0" /></a></p>
<p align="left">Déclarez ensuite le nom de votre package, ici “MyVirtualPackage” utilisant la même clé que celle définie au niveau de la sentinelle :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-63.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-52.png" alt="image" width="350" height="143" border="0" /></a></p>
Vous remarquerez que dans la déclaration du client Constellation nous avons spécifié dans le template de la classe Constellation le type de notre client réseau, qui dans le cas d’un ESP8266 ou d’un Arduino MKR1000 est “WifiClient”.

N’oubliez pas d’ajouter les librairies de votre client réseau, quelques exemples :
<pre class="lang:cpp decode:true">/* Arduino Wifi (ex: Wifi Shield) */
#include &lt;SPI.h&gt;
#include &lt;WiFi.h&gt;

/* Arduino Wifi101 (for WiFi Shield 101 and MKR1000 board) */
#include &lt;SPI.h&gt;
#include &lt;WiFi101.h&gt;

/* Arduino Ethernet */
#include &lt;SPI.h&gt;
#include &lt;Ethernet.h&gt;

/* ESP8266 Wifi */
#include &lt;ESP8266WiFi.h&gt;</pre>
Au démarrage du programme, nous allons d’abord connecter votre client réseau sur le réseau. Dans le cas d’un ESP8266, on se connecte au réseau Wifi de cette façon :
<pre class="lang:cpp decode:true">void setup(void) {
  Serial.begin(115200);

  // Connect to Wifi  
  WiFi.begin("MY-SSID", "MY-WIFI_KEY");  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // For WiFi101, wait 10 seconds for connection!
  // delay(10000);

  Serial.println("WiFi connected. IP: ");
  Serial.println(WiFi.localIP());
}
</pre>
Une fois connecté au réseau, on peut utiliser les méthodes de la librairie Constellation.

Par exemple, produisons un log avec la méthode “WriteInfo” :
<pre class="lang:cpp decode:true">constellation.writeInfo("Hello Constellation");</pre>
Cette méthode gère nativement le formatage “à la printf”, par exemple :
<pre class="lang:cpp decode:true">constellation.writeInfo("Hello Constellation, I'm '%s !", constellation.getSentinelName());</pre>
Notez que vous pouvez produire de la même façon des messages de type “Info”, “Warning” ou “Error” :
<pre class="lang:cpp decode:true">constellation.writeInfo("This is an information"); 
constellation.writeWarn("This is a warning"); 
constellation.writeError("This is an error");</pre>
Bien entendu, comme n’importe quel package, les logs sont remontés en temps réel dans la Constellation ce qui permet à n’importe quel “contrôleur” de voir les logs de votre Arduino/ESP en temps réel.

Par exemple, en ouvrant la Console Constellation on pourra suivre votre Arduino/ESP en temps réel :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-26.png" alt="image" width="425" height="118" border="0" /></a></p>

<h3>Accès aux settings</h3>
Tous les settings des packages de votre Constellation sont déclarés depuis <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Les_parametres_de_configuration">le fichier de configuration</a> de votre Constellation. Vous pouvez aussi utiliser la <a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Editer_les_settings_dun_package">Console Constellation pour les administrer</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-68.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-57.png" alt="image" width="230" height="118" border="0" /></a> <a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-69.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-58.png" alt="image" width="240" height="118" border="0" /></a></p>
<p align="left">Dans votre code Arduino, vous devez utiliser la méthode “getSettings” pour récupérer un objet JSON contenant les settings (clé/valeur).</p>

<pre class="lang:cpp decode:true">JsonObject&amp; settings = constellation.getSettings();</pre>
<p align="left">Par exemple pour récupérer la valeur du setting “MonChiffre” déclarée sur le serveur à 42 :</p>

<pre class="lang:cpp decode:true">JsonObject&amp; settings = constellation.getSettings();
static int monChiffre= settings["MonChiffre"].as&lt;int&gt;()</pre>
<p align="left">La variable “monChiffre” sera donc affectée à la valeur “42”. Dans l’exemple ci-dessus, la variable est statique pour pouvoir être utilisée n’importe où dans votre code.</p>
<p align="left">Pour les chaines de caractères vous pouvez utiliser la classe “String” :</p>

<pre class="lang:cpp decode:true">JsonObject&amp; settings = constellation.getSettings();
static String strMaChaine = String(settings["MaChaine"].as&lt;char *&gt;());</pre>
Encore une fois la variable est statique pour pouvoir l’utiliser n’importe où dans votre code.

Si vous devez convertir votre chaîne de caractères en <em>const char *</em>, vous pouvez utiliser la fonction <em>c_str()</em> :
<pre class="lang:cpp decode:true">const char* maChaine = strMaChaine.c_str();</pre>
Vous pouvez également tester la présence d'un setting avec la méthode "containsKey" :
<pre class="lang:c++ decode:true ">if(settings.containsKey("MySetting")) {  
  // Do something with settings["MySetting"]  
}</pre>
Par exemple vous pouvez gérer les valeurs par défaut. Imaginez un code où la valeur d'un timeout est de 5 secondes par défaut mais vous vous laissez la possibilité de surcharger cette variable depuis les settings Constellation :
<pre class="lang:default decode:true">// Default value
int timeout = 5; // sec

setup() {
  // .....
  JsonObject&amp; settings = constellation.getSettings();
  if(settings.containsKey("Timeout")) {  
    timeout = settings["Timeout"].as&lt;int&gt;();
  }
}

void loop() {
   // DO something with 'timeout'
}</pre>
Si le setting "Timeout" est défini dans les settings de votre package sur Constellation, la variable "timeout" sera affectée à votre valeur, autrement, si le setting n'existe pas, la valeur de cette variable sera de 5.
<h3>Publier des StateObjects</h3>
Pour produire et publier un StateObject dans votre Constellation vous devez invoquer la méthode “pushStateObject” :
<pre class="lang:cpp decode:true">constellation.pushStateObject(name, value);</pre>
Vous pouvez aussi passer le “type” de votre StateObject :
<pre class="lang:cpp decode:true">constellation.pushStateObject(name, value, type);</pre>
Notez que pour les types simples (int,bool, long, float et double) le type est spécifié implicitement.

Par exemple, pour publier un StateObject dont la valeur est un chiffre :
<pre class="lang:cpp decode:true">constellation.pushStateObject("Temperature", 21);</pre>
Vous pouvez aussi publier des StateObject avec un objet complexe en utilisant la fonction “stringFormat” pour formater votre valeur en JSON.

Par exemple pour publier un StateObject nommé “Lux” étant un objet contenant 3 propriétés :
<pre class="lang:cpp decode:true">constellation.pushStateObject("Lux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir));</pre>
Vous pouvez également faire la même chose en construisant un “JsonObject” :
<pre class="lang:cpp decode:true">StaticJsonBuffer&lt;200&gt; jsonBuffer;
JsonObject&amp; root = jsonBuffer.createObject();

root["Lux"] = lux;
root["Broadband"] = full;
root["IR"] = ir;
constellation.pushStateObject("Lux", &amp;root);</pre>
Une fois vos StateObjects publiés, ils sont accessibles en temps réel aux autres packages et consommateurs de votre Constellation. Vous pouvez également utiliser le “StateObject Explorer” de la Console Constellation pour explorer tous les StateObjects de votre Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-70.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-59.png" alt="image" width="240" height="181" border="0" /></a></p>

<h3>Next steps</h3>
<ul>
 	<li><a href="/client-api/arduino-esp-api/produire-des-stateobjects-depuis-arduino-esp/">Publier des StateObjects</a></li>
 	<li><a href="/client-api/arduino-esp-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-depuis-arduino-esp/">Envoyer des messages</a></li>
 	<li><a href="/client-api/arduino-esp-api/recevoir-des-messages-et-exposer-des-methodes-messagecallback-sur-arduino-esp/">Recevoir des messages</a></li>
 	<li><a href="/client-api/arduino-esp-api/consommer-des-stateobjects-depuis-arduino-esp/">Consommer des StateObjects</a></li>
 	<li><a href="/client-api/arduino-esp-api/utiliser-lua-sur-nodemcu-pour-connecter-des-esp8266/">Connectez vos ESP8266 à Constellation en Lua avec NodeMCU</a></li>
</ul>