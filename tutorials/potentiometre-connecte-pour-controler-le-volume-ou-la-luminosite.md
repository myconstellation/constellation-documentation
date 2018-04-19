---
ID: 4549
post_title: 'Un potentiomètre connecté pour contrôler le volume de votre PC, la luminosité de l&rsquo;écran et bien plus encore'
author: Sebastien Warin
post_date: 2017-05-10 11:15:44
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/potentiometre-connecte-pour-controler-le-volume-ou-la-luminosite/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:11:15
---
Dans ce tutoriel nous allons créer un encodeur rotatif connecté pour piloter tout type de chose comme le volume ou la luminosité de vos ordinateurs Windows par exemple grâce au package <a href="/package-library/windowscontrol/">WindowsControl</a>.
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Volume2" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Volume2.gif" alt="Volume2" width="400" height="225" border="0" /></p>
Pour cela nous allons utiliser un encodeur rotatif aussi nommé potentiomètre angulaire comme le KY-040 (comptez entre 50 cts et 2 euros pièce) avec un ESP8266 connecté en Wifi à votre Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/ky040.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="KY-040" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/ky040_thumb.jpg" alt="KY-040" width="240" height="184" border="0" /></a></p>

<h3>Prérequis</h3>
Pour réaliser ce tutoriel, il nous faut :
<ul>
 	<li>Un ESP8266, ici nous utilisons un D1 Mini Pro</li>
 	<li>Un encodeur rotatif KY-040</li>
 	<li>Un <a href="/getting-started/installer-constellation/">serveur Constellation</a></li>
 	<li>Le package <a href="/package-library/windowscontrol/">WindowsControl </a>déployé sur au moins une <a href="/getting-started/ajouter-des-sentinelles/">sentinelle Windows</a></li>
</ul>
<h3>Etape 1 : créer un encodeur rotatif connecté</h3>
Le KY-040 a 5 pins : deux pour l'alimentation (5V et Gnd), deux pour l'encodeur rotatif (CLK et DT) et un dernier pour le bouton (lorsque l'on pousse sur le potentiomètre).

Nous allons donc tout simplement connecter les pin 5V et Gnd sur le port 5V et Gnd de notre D1 Mini Pro. Les pins CLK et DT respectivement sur les ports D1 et D2 et le switch (pin SW) sur le port D3.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/DSC_0019.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESP RotaryController" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/DSC_0019_thumb.jpg" alt="ESP RotaryController" width="304" height="173" border="0" /></a></p>
Vous pouvez optionnellement habiller le potentiomètre avec un bouchon, ci-dessous récupéré sur une veille chaine Hifi :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/DSC_0014.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESP RotaryController avec un bouchon" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/DSC_0014_thumb.jpg" alt="ESP RotaryController avec un bouchon" width="304" height="173" border="0" /></a></p>
<p style="text-align: left;" align="center">Votre encodeur est prêt, il ne reste plus qu'à le programmer.</p>

<h3>Etape 2 : programmer l’ESP8266</h3>
Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d’accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée “ESP8266” et le package virtuel se nomme “RotaryController”.

Dans l’Arduino IDE, nous créons un nouveau projet à partir du Starter Kit Constellation pour ESP8266 dans lequel nous allons définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d’accès  puis nous configurerons le client Constellation en spécifiant l’identité de notre package virtuel, sa clé d’accès et l’adresse/port de notre serveur Constellation :
<pre class="lang:default decode:true">// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MON SSID";
char password[] = "macléWifi!!!!";

// Constellation client
Constellation&lt;WiFiClient&gt; constellation("X.X.X.X", 8088, "ESP8266", "RotaryController", "xxxxxxxxxxxxxxxxx");</pre>
Encore une fois si tout cela est nouveau pour vous, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l’API Constellation pour Arduino/ESP8266</a>.

Maintenant que la coquille de notre package virtuel sur ESP8266 est prête, ajoutons une librairie pour interpréter les signaux du KY-040 (inputs CLK et DT) pour connaitre et mesurer le mouvement du potentiomètre.

Dans le gestionnaire de bibliothèque (menu<em> Croquis &gt; Inclure une bibliothèque</em>), installez la librairie “<a href="https://www.pjrc.com/teensy/td_libs_Encoder.html">Encoder</a>” de Paul Stoffregen et ajoutez-la dans votre code avant de déclarer une variable de type “Encoder” que nous nommerons “encoder”.

Pour rappel nous avons connecté le KY-040 sur les ports D1 (CLK) et D2 (DT) et le bouton sur D3 (SW) :
<pre class="lang:cpp decode:true">#include &lt;Encoder.h&gt;

Encoder encoder(D1, D2);
int pinSW = D3;</pre>
Le bouton poussoir du potentiomètre est un simple bouton poussoir connecté en "pull-up" (à la masse). Il n'est donc pas traité par la librairie "Encoder" et sera interprété avec un simple "<em>digitalRead</em>".

Toujours dans l’entête de votre code, configurons les valeurs par défaut suivantes :
<pre class="lang:cpp decode:true">// Default values
static String target = "WindowsControl";
static String mcbUp = "VolumeUp";
static String mcbDown = "VolumeDown";
static String mcbPush = "Mute";</pre>
Ici on configure où on va envoyer le message, par défaut au scope du package "WindowsControl".

A ce package "WindowsControl" on enverra :
<ul>
 	<li>Un message "VolumeUp" si le potentiomètre "monte"</li>
 	<li>Un message "VolumeDown" si le potentiomètre "descend"</li>
 	<li>Un message "Mute" si on pousse le potentiomètre "monte"</li>
</ul>
C’est à dire que par défaut, notre potentiomètre connecté contrôlera le volume des PC Windows où le package WindowsControl est déployé. On pourra par la suite personnaliser ces actions dans les settings comme nous le verrons ci-dessous.

Entrons maintenant dans le code de la méthode "<em>setup()</em>". Ajoutons la ligne suivante pour initialiser l'entrée D3 où est connecté le bouton poussoir :
<pre class="lang:cpp decode:true">// Init I/O
pinMode(pinSW, INPUT_PULLUP);</pre>
Enregistrons ensuite un MessageCallback "ReloadSettings" pour permettre de recharger la configuration "à la volée" sans devoir redémarrer notre EPS. Ce MessageCallback se charge d'invoquer la fonction "<em>loadSettings()</em>".

De plus, à la fin de notre méthode "setup()" invoquons cette méthode "<em>loadSettings()</em>" pour charger la configuration au démarrage.
<pre class="lang:cpp decode:true">// ReloadSettings MessageCallback
constellation.registerMessageCallback("ReloadSettings", MessageCallbackDescriptor().setDescription("Reload the settings"),
  [](JsonObject&amp; json) {
    loadSettings();
    constellation.writeInfo("Settings reloaded");
 });
constellation.declarePackageDescriptor();

// Load settings on start
loadSettings();</pre>
Cette méthode "<em>loadSettings()</em>" récupère les settings sur le serveur Constellation et écrase la valeur par défaut si le setting est défini sur le serveur.
<pre class="lang:cpp decode:true">void loadSettings() {
  JsonObject&amp; settings = constellation.getSettings();
  if(settings.containsKey("Target")) {
    target = String(settings["Target"].asString());
  }
  if(settings.containsKey("OnUp")) {
    mcbUp = String(settings["OnUp"].asString());
  }
  if(settings.containsKey("OnDown")) {
    mcbDown = String(settings["OnDown"].asString());
  }
  if(settings.containsKey("OnPush")) {
    mcbPush = String(settings["OnPush"].asString());
  }
}</pre>
De ce fait par défaut, notre potentiomètre connecté invoque le MessageCallback "VolumeUp" sur le package "WindowsControl" lorsque l'on tourne à droite, "VolumeDown" à gauche ou "Mute" lorsqu'on appuie dessus. Il suffira d'ajouter/modifier les settings de notre package virtuel depuis la Console Constellation pour modifier ces actions.

Il ne reste plus qu'à définir le code de la boucle principale, la fonction "<em>loop()</em>".

Celle-ci lit la valeur de l'encodeur rotatif et envoi les messages dans Constellation en cas de montée ou de descente. La boucle lit également la valeur de l'entrée D3 pour envoyer le message en cas d'appui sur le potentiomètre.
<pre class="lang:cpp decode:true">void loop(void) {
  constellation.loop();

  static long lastValue = -1;
  long value = encoder.read();
  if (value != lastValue &amp;&amp; abs(value - lastValue) &gt;= 4) {
    if(lastValue &gt; value) {
      Serial.println("Up");
      constellation.sendMessage(Package, target.c_str(), mcbUp.c_str(), "{}");
    }
    else {
      Serial.println("Down");
      constellation.sendMessage(Package, target.c_str(), mcbDown.c_str(), "{}");
    }    
    lastValue = value;
  }
  
  static int lastButtonState = LOW; 
  int buttonState = digitalRead(pinSW);
  if (buttonState != lastButtonState &amp;&amp; buttonState == LOW) {
     Serial.println("Push");
     constellation.sendMessage(Package, target.c_str(), mcbPush.c_str(), "{}");
  }
  lastButtonState = buttonState;  
}</pre>
Et voilà votre potentiomètre connecté est opérationnel !
<h3>Démos</h3>
<h4>Piloter le volume de vos PC</h4>
Une fois le programme téléversé sur votre ESP8266, vous pouvez tourner le potentiomètre à gauche ou à droite pour diminuer ou augmenter le volume. Si vous appuyez sur le bouton, vous alternez entre les modes "mute/un-mute".
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MultiSentinels" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/MultiSentinels.gif" alt="MultiSentinels" width="400" height="225" border="0" /></p>
<p style="text-align: left;" align="center">Par contre si vous avez déployé le package <a href="/package-library/windowscontrol/">WindowsControl </a>sur plusieurs de vos sentinelles, vous <strong>contrôlerez simultanément le volume sur TOUTES vos sentinelles</strong> comme le montre l'animation ci-dessus.</p>
<p style="text-align: left;" align="center">Souvenez-vous du <a href="/concepts/instance-package-versioning-et-resolution/">concept d'instance</a>, ici notre ESP8266 envoie un message au scope "WindowsControl", c'est donc TOUTES les instances du package "WindowsControl" qui recevrons le message.</p>

<h4>Piloter le volume d’un PC en particulier</h4>
Pour contrôler le volume d'un PC en particulier, il faut envoyer les messages au scope d'une instance du "WindowsControl" en particulier.

Pour cela nous devez spécifier l'instance selon la nomenclature suivante : "SENTINEL/Package".

Par exemple, le nom de la sentinelle sur laquelle est déployée le package WindowsControl est "PC-SEB_UI". Il faudra donc que j'envoie les messages au package "PC-SEB_UI/WindowsControl" pour contrôler le volume de mon ordinateur "PC-SEB" et non de tous.

Comme nous avons créé un système personnalisable en se servant des settings Constellation, il me suffit, depuis la Console Constellation, d'ajouter le setting que nous avons nommé "Target" pour définir le nom du scope où envoyer les messages :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-50.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Personnaliser les settings" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-50.png" alt="Personnaliser les settings" width="404" height="194" border="0" /></a></p>
<p style="text-align: left;" align="center">Et voilà, désormais je ne pilote que le volume de cette machine en particulier et non de toutes mes machines.</p>

<h4>Piloter la luminosité d’un PC</h4>
Si on observe les MessageCallbacks du package <a href="/package-library/windowscontrol/">WindowsControl</a>, on constate qu'il expose d'autre MCs dont "BrightnessUp" et "BrightnessDown" pour augmenter ou diminuer la luminosité.

Avec notre code se base sur les settings, nous pouvons modifier le nom des MC à invoquer en ajoutant/modifiant les settings du package.

Ajoutons donc deux settings nommés "OnUp" et "OnDown" respectivement définis avec les valeurs "BrightnessUp" et "BrightnessDown". Invoquons ensuite le MC "ReloadSettings" sur notre package virtuel ou bien appuyez sur le bouton "Reset" de votre ESP8266 pour le redémarrer et donc recharger les settings :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-51.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-51.png" alt="image" width="404" height="214" border="0" /></a></p>
<p style="text-align: left;" align="center">Désormais notre potentiomètre connecté pilotera la luminosité :</p>
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Brightness" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Brightness.gif" alt="Brightness" width="400" height="225" border="0" /></p>

<h4>Pour aller plus loin …</h4>
Comme vous l'avez vu, notre potentiomètre est entièrement personnalisable car le nom des MC à invoquer en cas de montée, descente ou d'appui sur le potentiomètre est personnalisable dans les settings.

Ici on pilote le volume ou la luminosité d'un Windows grâce au package WindowsControl, mais on pourrait également piloter tout type de chose : l'intensité d'une lampe, le thermostat, le toit ouvrant d'une voiture ou bien des valeurs de vos programmes ou pages Web, par exemple le slider d'une page HTML !

Libre à votre imagination ;)