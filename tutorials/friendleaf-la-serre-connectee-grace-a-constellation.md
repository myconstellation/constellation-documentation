---
ID: 6084
post_title: 'FriendLeaf : la serre connectée grâce à Constellation'
author: Sebastien Warin
post_date: 2018-07-17 11:40:50
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/friendleaf-la-serre-connectee-grace-a-constellation/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-07-17 11:40:50
---
<i>Plus besoin de disposer d'un espace extérieur pour faire pousser vos propres herbes aromatiques, salades et fleurs. Grâce à la serre connectée FriendLeaf, vous pouvez faire pousser plusieurs plantes et vous en occuper facilement.</i>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title=" FriendLeaf : la serre connectée grâce à Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure1_thumb.png" alt="FriendLeaf : la serre connectée grâce à Constellation" width="354" height="266" border="0" /></a></p>
Projet réalisé par Théo DELOOSE,Clara BOMY,Clément NOUGET,Mathieu GABRIEL,Marine DAEL etThaï-Son DANG.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="L'équipe FriendLeaf" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure2_thumb.png" alt="L'équipe FriendLeaf" width="254" height="208" border="0" /></a></p>
<!--more-->

[toc]
<h3>Introduction</h3>
Nous sommes six étudiants en troisième année à l’ISEN Lille. Dans le cadre de notre projet de fin d’année, nous avons conçu une serre connectée dédiée à un usage en intérieur. Celle-ci est équipée d’un système d’éclairage intelligent, d’une pompe d’arrosage automatique et d’un brumisateur intégré afin de garantir la bonne croissance des plantes, rassemblant les conditions nécessaires à leur développement. Pour une plus grande facilité d'utilisation, notre serre est associée à une application mobile simple et ludique permettant de suivre en temps réel les données de l’environnement de la serre et de contrôler celle-ci à distance.

En réalisant ce projet, notre but était de proposer une solution de serre connectée à un prix raisonnable et possédant une interface attrayante pour améliorer l’expérience de l’utilisateur.

De plus nous voulions que la serre puisse être intégrée dans différents systèmes facilement pour que l’utilisateur puisse utiliser les données à sa guise.
<h3>Fonctionnement général</h3>
Nous avons pensé<i> FriendLeaf </i>comme un système de monitoring et de pilotage. Il propose en effet de gérer automatiquement l’arrosage, l’humidité et la luminosité de la serre, ou bien de les activer manuellement à notre guise. Il synchronise les données récupérées par les différents capteurs grâce à la plateforme Constellation et active, par le biais d’un relais, les actionneurs. Enfin, <i>FriendLeaf </i>alerte l’utilisateur lorsque le réservoir d’eau est vide. Tout est synchronisé en temps réel comme par magie grâce à Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Communication" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure3_thumb.png" alt="Communication" width="400" height="150" border="0" /></a></p>
Nous avons donc ajouté dans la serre les capteurs permettant de relever les informations sur l’humidité de l’air et du sol, sur la température et sur la luminosité.  L’équipement installé comporte aussi une guirlande lumineuse ayant pour but d’éclairer et d’afficher les alertes, une pompe pour le système d’arrosage et un brumisateur permettant d’humidifier l’air.
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma général de la serre " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure4_thumb.png" alt="Schéma général de la serre " width="450" height="244" border="0" /></a></p>

<h3>Matériel utilisé</h3>
Pour réaliser notre projet, nous nous sommes servis des composants suivants :

Pour la Serre :
<ul>
 	<li><u><a href="https://www.ikea.com/fr/fr/catalog/products/70186603/">Serre SOCKER Ikea</a></u> (12,99€), Ikea</li>
 	<li>Bac pour le terreau (1,50€), Jardinerie</li>
 	<li><u><a href="https://www.amazon.fr/Raspberry-Pi-3-Mod%C3%A8le-B-Carte-m%C3%A8re/dp/B07BDR5PDW/ref=sr_1_cc_3?s=aps&amp;ie=UTF8&amp;qid=1528895423&amp;sr=1-3-catcorr&amp;keywords=raspberry+pi+3B">Carte Raspberry Pi 3B </a></u>(40,80€), Amazon</li>
 	<li><u><a href="https://www.amazon.fr/Elegoo-Optocoupleur-Continu-Arduino-Raspberry/dp/B06XKST8XC/ref=sr_1_1?s=computers&amp;ie=UTF8&amp;qid=1528792288&amp;sr=1-1&amp;keywords=4%2Bcanaux%2B250V&amp;th=1">Relais 4 canaux</a></u> supportant jusqu’à 5A et 250V (AC) et 30V (DC) (9,99€), Amazon</li>
 	<li>Câbles de connexion</li>
 	<li>Boîte de dérivation</li>
</ul>
Pour les capteurs :
<ul>
 	<li>Humidité et température air : <u><a href="https://www.amazon.fr/SODIAL-Humidite-Numerique-Temperature-Arduino/dp/B00K67XRFC/ref=sr_1_1?ie=UTF8&amp;qid=1529502640&amp;sr=8-1&amp;keywords=dht+11">DHT11</a></u> (1,31€), Amazon</li>
 	<li>Luminosité : <u><a href="https://www.gotronic.fr/art-capteur-de-luminosite-tsl2561-19569.htm">TSL2561</a></u> (6,95€), Gotronic</li>
 	<li>Humidité sol : <u><a href="https://www.gotronic.fr/art-capteur-d-humidite-gt110-26091.htm#complte_desc">GT110</a></u> (2,40€), Gotronic</li>
 	<li>Niveau d’eau : <u><a href="https://www.gotronic.fr/art-detecteur-de-niveau-gravity-sen0205-25296.htm">Gravity SEN0205</a></u> (10,50€), Gotronic</li>
</ul>
Pour le brumisateur :
<ul>
 	<li>Un <u><a href="https://www.amazon.fr/Ultrasons-Shineus-Humidificateur-Fontaine-Atomisation/dp/B077HSKYN8/ref=sr_1_4?ie=UTF8&amp;qid=1528895226&amp;sr=8-4&amp;keywords=mist+maker">émetteur à ultrasons</a></u> (9,89€), Amazon</li>
 	<li>Une alimentation pour le brumisateur (24V DC, 1A) (25€), Derotronic, Lille</li>
 	<li>Un bol</li>
</ul>
Pour le système d’arrosage :
<ul>
 	<li>Une <u><a href="https://www.amazon.fr/Gugutogo-submersible-%C3%A9lectrique-silencieux-m%C3%A9canique/dp/B07BXJTND4/ref=sr_1_3?s=electronics&amp;ie=UTF8&amp;qid=1529927406&amp;sr=1-3&amp;keywords=pompe+%C3%A0+eau+12v">pompe à eau</a></u> (5,48€), Amazon</li>
 	<li>Une alimentation 12V 600mA (12€), Derotronic, Lille</li>
 	<li>Un jerrican (environ 10€)</li>
 	<li>Un tuyau (ø7mm -ø10mm), Diall</li>
</ul>
Pour l’éclairage :
<ul>
 	<li>2m de <u><a href="http://www.blachere-illumination-store.com/fr/nos-creations/326-fil-lumiere-blanc-8m-3587880017658.html">guirlandes lumineuses</a></u> (1m LED et 1m incandescent), Blachère</li>
 	<li>Une alimentation pour fil lumière, Blachère</li>
 	<li>Colliers de serrage</li>
</ul>
<h3>Conception de la serre</h3>
<h4>Étape 1 : Préparation de la serre</h4>
Dans la serre, nous avons disposé les capteurs, le pot et un bol pour le brumisateur. Nous avons également percé des trous pour pouvoir faire passer tous les capteurs et le tuyau d’arrosage.

Pour ce qui est de la guirlande lumineuse, il nous a fallu percer quelques trous supplémentaires dans les montants de la serre pour y glisser les colliers de serrage.
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="La serre &quot;nue&quot; " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure5_thumb.png" alt="La serre &quot;nue&quot; " width="354" height="266" border="0" /></a></p>

<h4>Étape 2 : Branchement du relais</h4>
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Les relais" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure6_thumb.png" alt="Les relais" width="240" height="240" border="0" /></a></p>
Afin de pouvoir contrôler les actionneurs, nous avons branché le relais quatre canaux à une carte Raspberry Pi.

Celui-ci est analogue à un interrupteur. Pour chaque canal, une alimentation est nécessaire et chacun d’entre eux peut être piloté individuellement grâces aux différents pins dont dispose le relais. Il faut également prévoir une masse que l’on branche sur le pin “Gnd” du relais.

Par défaut, lorsque les pins d’entrée ne sont pas alimentés, les canaux du relais délivrent la tension d’alimentation correspondante sur la sortie NC (“Normally Closed”) ou sur la sortie NO (“Normally Open”) dans le cas contraire.
<h4>Etape 3 : Éclairage de la serre</h4>
Pour l’éclairage de notre serre, nous avons percé des trous dans la structure et fixé la guirlande avec des colliers de serrage. Celle-ci doit être alimentée par une tension de 230V (<i>tension secteur</i>) et est composée d’un mètre de LEDs rouge et d’un mètre incandescent jaune.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Les LEDs" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure7_thumb.png" alt="Les LEDs" width="244" height="160" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure7bis.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Les LEDs" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure7bis_thumb.png" alt="Les LEDs" width="244" height="184" border="0" /></a></p>
Elle est connectée au relais par la phase et le neutre, chacun branchés sur un canal différent, afin d’isoler totalement la guirlande du secteur et ainsi éviter tout problème avec le reste des composants.
<h4>Etape 4 : Installation du brumisateur</h4>
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Le relais avec les LEDs et le brumisateur " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure8_thumb.png" alt="Le relais avec les LEDs et le brumisateur " width="244" height="324" border="0" /></a></p>
Pour augmenter l’humidité de l’air dans la serre, nous avons installé un brumisateur fonctionnant par émissions d’ultrasons. Il nécessite une alimentation de 24V/1A maximum. On le relie ensuite au relais. La masse est directement connectée à l’adaptateur secteur par le biais d’un domino.
<h4>Etape 5 : Installation du système d’arrosage</h4>
<h4 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation du système d’arrosage" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure9_thumb.png" alt="Installation du système d’arrosage" width="184" height="244" border="0" /></a></h4>
En ce qui concerne l’arrosage, nous disposons d’une pompe submersible qui nécessite une alimentation de 12V et 400mA. Elle est branchée en sortie à un tuyau percé pour un arrosage homogène. Nous la connectons ensuite à un des canaux du relais.
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Le reservoir" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure10_thumb.png" alt="Le reservoir" width="184" height="244" border="0" /></a></p>
Pour le réservoir, nous avons récupéré un bidon de vinaigre de 5L sur lequel nous avons fait un trou pour le capteur de niveau d’eau et une ouverture pour le remplir et y faire passer la pompe.
<h4>Pour résumer...</h4>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma du relais et de la Raspberry" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure11_thumb.png" alt="Schéma du relais et de la Raspberry" width="404" height="301" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Rendu dans la boite " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure12_thumb.png" alt="Rendu dans la boite " width="185" height="244" border="0" /></a></p>

<h4>Étape 6 : Mise en place des capteurs et paramétrage de la carte Arduino</h4>
Nous installons à présent les capteurs d’humidité/température de l’air, d’humidité du sol, de de luminosité et de niveau d’eau. Nous branchons tous ces capteurs aux pins de la Raspberry ou de l’Arduino en faisant attention au fait que :
<ul>
 	<li>Les capteurs d’humidité du sol et de niveau d’eau se branchent sur du 5V.</li>
 	<li>Les capteurs d’humidité/température de l’air et de luminosité sont alimentés en 3,3V.</li>
</ul>
<h5>Étape 6.1 : Capteur d’humidité et de température de l’air</h5>
Le capteur d’humidité et de température de l’air fournissant des données numériques, nous le relions directement à notre Raspberry Pi comme montré sur le schéma ci-dessous :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Capteur d'humidité/température " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure13_thumb.png" alt="Capteur d'humidité/température " width="350" height="134" border="0" /></a></p>

<h5>Étape 6.2 : Capteurs de luminosité, d’humidité du sol et de niveau d’eau</h5>
<a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure14.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title=" Capteurs et Arduino sur la Raspberry " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure14_thumb.png" alt=" Capteurs et Arduino sur la Raspberry " width="350" height="274" border="0" /></a>

Ces capteurs fournissant des données analogiques, nous les avons branchés à une carte Arduino nano qui permet de lire ces données, contrairement à la Raspberry. Elle est reliée directement par USB à la Raspberry Pi comme montré ci-après :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Arduino et ses capteurs" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure15_thumb.png" alt="Arduino et ses capteurs" width="244" height="184" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Arduino et ses capteurs" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure16_thumb.png" alt="Arduino et ses capteurs" width="244" height="185" border="0" /></a></p>

<h5>Étape 6.3 : Programmation de la carte Arduino</h5>
Dans l’IDE Arduino, nous avons donc commencé par programmer la carte.

Nous ajoutons tout d’abord les librairies d’Adafruit pour pouvoir initialiser le capteur de luminosité et régler le temps d’intégration des données. Dans le gestionnaire de bibliothèque (menu<i> Croquis &gt; Inclure une bibliothèque</i>), nous avons installé les librairies suivantes :
<ul>
 	<li>Adafruit Unified Sensor</li>
 	<li>Adafruit TSL2561</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Les librairies Arduino " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure17_thumb.png" alt="Les librairies Arduino " width="350" height="76" border="0" /></a></p>
Au démarrage, dans la méthode “setup()”, nous configurons les pins utilisés. Nous décidons d’utiliser les pins analogiques A0 et A1, respectivement pour le capteur d’humidité du sol et du niveau d’eau et les pins A4 et A5 pour le capteur de luminosité :
<pre title="Code de démarrage" class="lang:cpp decode:true">void setup(void) 
{
  Serial.begin(9600);
 
  /*Réglage du capteur de luminosité*/
  if(!tsl.begin()) //Initialisation du capteur
  {
    //S'il y a un problème pour détecter le capteur, vérifier votre connexion
    Serial.print("Ooops, pas de capteur détecté... Vérifier votre connexion!");
    while(1);
  }
  
    //Configuration du gain du capteur et du temps d'intégration
    tsl.enableAutoRange(true);          
    tsl.setIntegrationTime(TSL2561_INTEGRATIONTIME_13MS);    
 
}
</pre>
Dans la méthode “loop()”, nous recueillons ensuite les valeurs des capteurs et les faisons afficher sur le Monitor Série en utilisant les commandes analogRead() et Serial.print()  :
<pre title="Boucle principale" class="lang:cpp decode:true">void loop(void) 
{  
  /*Pour recevoir une nouvelle donnée par le capteur de luminosité*/ 
  sensors_event_t event;
  tsl.getEvent(&amp;event);
 
  //Lecture des pins analogiques pour les données des capteurs d'humidité du sol et de niveau d'eau
  Hum_value = analogRead(A0);
  Liquid_value=analogRead(A1);
 
  //Affichage des valeurs sur le Moniteur série
  Serial.print(event.light,0);
  Serial.print(',');
  Serial.print(map(Hum_value,0,1024,0,100)); //Le capteur d'humidité renvoie une valeur en pourcentage 
  Serial.print(',');
  Serial.println(map(Liquid_value,0,512,0,1)); //Le capteur de niveau d'eau retourne la valeur 1 quand il est en contact avec l'eau, 0 sinon
  delay(5000); //Retard de 5s pour éviter de surcharger le buffer de la Raspberry 
}
</pre>
Nous récupèrerons ensuite les données des différents capteurs à l’aide de la carte Raspberry Pi.
<h3>La programmation vers Constellation</h3>
La programmation vers Constellation est divisée en deux packages : le premier correspond à la récupération des données des capteurs et à l’activation des actionneurs, le second a été créé pour gérer le stockage des données en vue de créer des graphiques sur notre application.
<h4>Etape 1 : Package relatif aux capteurs et actionneurs</h4>
<h5>Etape 1.1 : Acquisition des données des capteurs</h5>
Pour récupérer les données des différents capteurs cités précédemment, nous avons utilisé la librairie “Adafruit_DHT” pour Raspberry Pi et la librairie “serial” qui permet de faire le lien entre l’Arduino-Nano et la Raspberry Pi. Ces valeurs vont nous permettre de décider s’il faut activer ou non les actionneurs tels que la pompe, le brumisateur ou les LED.

La fonction ci-après permet de récupérer les valeurs des différents capteurs.
<pre class="lang:python decode:true" title="Récupération des données">def getCapteur():
    humiditeAir, temperature = Adafruit_DHT.read_retry(11,4)
    lux, humiditeSol, eau = ser.readline()[:-2].split(",")
</pre>
Nous avons ensuite créé un State Object rassemblant les valeurs d’humidité du sol, d’humidité de l’air, de température et de luminosité ambiante pour pouvoir les publier sur notre Constellation.
<pre title="Publication du StateObject" class="lang:python decode:true">Constellation.PushStateObject("Capteurs", {"HumiditeSol": int(self.humiditeSol), "HumiditeAir": int(self.humiditeAir), "Temperature": int(self.temperature), "Luminosite": int(self.lux)}, "CapteursInfos")</pre>
<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title=" Le State Object des capteurs " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure18_thumb.png" alt=" Le State Object des capteurs " width="350" height="169" border="0" /></a></p>

<h5>Etape 1.2 : Activation et désactivation des différents actionneurs (brumisateur, guirlande lumineuse et pompe à eau)</h5>
Comme mentionné précédemment, les actionneurs sont reliés à la Raspberry Pi via un relais quatre canaux. Cela simplifie grandement la programmation : il nous suffit simplement de gérer l’ouverture et la fermeture des relais.

Nous avons donc fait en sorte que chaque actionneur soit associé à son relais grâce à la librairie GPIO de la Raspberry Pi. Ainsi, le code diffère seulement au niveau des numéros des pins utilisés par les actionneurs.

Voici un exemple du code de l’activation et de la désactivation de notre pompe :
<pre title="Controle de la pompe" class="lang:python decode:true">GPIO.setmode(GPIO.BOARD)
GPIO.setup(12,GPIO.OUT)
GPIO.output(12,0)
</pre>
Le pin 12 de la Raspberry est celui relié au relais contrôlant la pompe.
<h5>Etape 1.3 : Automatisation des actionneurs</h5>
Pour réaliser la fonction d’automatisation, nous nous sommes tout d’abord renseignés sur les conditions propices à la bonne croissance des plantes au niveau de l’humidité du sol et de l’air, de la luminosité et de la température de l’air.

Pour le brumisateur, nous avons choisi de l’activer si l’humidité est inférieure à 35% et de le désactiver si l’humidité remonte au dessus de 37%.
<pre title="Automatisation" class="lang:python decode:true">if humiditeAir &gt; 37 and brumisateur:
	DesactiverBrumisateur()
elif humiditeAir &lt; 35 and not brumisateur:
	ActiverBrumisateur()
    time.sleep(1)
</pre>
Pour la pompe, le fonctionnement est différent. Puisque la diffusion de l’eau dans la terre est plus lente, nous activons la pompe pendant 5s avant de la désactiver pendant 30s pour laisser le temps au capteur de ressentir les changements.
<pre title="Automatisation" class="lang:python decode:true">if humiditeSol &lt; 30 and not pompe:
	if self.ticks == 0:
        	if self.next_water == 0:
               		ActiverPompe()
			self.next_water = 15
		else:
               		self.next_water -= 1
		elif humiditeSol &gt; 30:
               		self.next_water = 0
	elif pompe:
        	self.ticks += 1
               if self.ticks &gt;= 3:
               		DesactiverPompe()
                       self.ticks = 0
time.sleep(1)
</pre>
Ces différentes fonctions sont exécutées dans des threads séparés pour ne pas que le code soit bloquant.

Maintenant, il ne reste plus qu'à sécuriser notre système d’arrosage via le capteur de niveau d’eau. Connecté à la carte Arduino nano, ce capteur va nous renvoyer 1 s’il y a de l’eau dans le réservoir, 0 sinon.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package PushBullet" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure19_thumb.png" alt="Package PushBullet" width="354" height="163" border="0" /></a></p>
Les données du capteur de niveau d’eau nous permettent d’arrêter le système d’arrosage lorsque le réservoir est presque vide et d’informer l’utilisateur via un PushBullet et l’allumage des LEDs qu’il faut remplir le réservoir.
<pre title="Notification via PushBullet" class="lang:python decode:true">Constellation.SendMessage("PushBullet", "PushNote", [ "FriendLead", "Le reservoir d'eau est vide"], Constellation.MessageScope.package)</pre>
<div align="center">
<pre><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure20.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Notification sur smartphone" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure20_thumb.png" alt="Notification sur smartphone" width="184" height="364" border="0" /></a></pre>
</div>
<h4>Étape 2 : Package relatif au stockage des données</h4>
Afin d’historiser les valeurs des capteurs stockées dans un des State Objects du premier package, nous en avons créé un autre que l’on a déployé sur le même serveur que Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Enregistrement CSV" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure21_thumb.png" alt="Enregistrement CSV" width="177" height="244" border="0" /></a></p>
Ce package génère une base de données dans un fichier .csv qui est mis à jour toutes les minutes avec les nouvelles données.

Chaque ligne contient la date, l’heure, l’humidité du sol, l’humidité de l’air, la température et la luminosité.
<pre title="Abonnement au StateObject" class="lang:python decode:true">@Constellation.StateObjectLink(package = "FriendLeafCapteurs", name = "Capteurs")
def RecupValue(stateObject):
    humiditeSol = stateObject.Value.HumiditeSol
    humiditeAir = stateObject.Value.HumiditeAir
    temperature = stateObject.Value.Temperature
</pre>
Nous avons ensuite créé un message callback qui permet de visualiser le fichier CSV dans notre Constellation.
<pre title="Lecture du fichier" class="lang:python decode:true">@Constellation.MessageCallback()</pre>
<pre title="Lecture du fichier" class="lang:python decode:true">def LireBDD():
'''
: return string : La base de données 
'''
    file = open('BDD.csv','r')
    lines = file.readlines()
    return lines
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="La saga du Message Callback " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure22_thumb.png" alt="La saga du Message Callback " width="350" height="203" border="0" /></a></p>

<h3>L’interface utilisateur : l’application mobile de monitoring et de pilotage de la serre</h3>
<h4>Étape 1 : Le noyau de l’application</h4>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="L'application Cordova" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure23_thumb.png" alt="L'application Cordova" width="184" height="364" border="0" /></a></p>
Pour développer l’application mobile nous nous sommes aidés de Cordova, un Framework permettant de créer des application Android et iOS avec des technologies web.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Cordova" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure24_thumb.png" alt="Cordova" width="240" height="98" border="0" /></a></p>
Nous avons également utilisé jQuery et AngularJS pour faciliter l’interaction entre Javascript et HTML.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="AngularJS" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure27_thumb.png" alt="AngularJS" width="240" height="143" border="0" /></a></p>
De plus, pour gérer certaines fonctionnalités comme le traitement du CSV ou l’affichage des graphiques, nous avons utilisé des librairies externes comme PapaParser et Chartist. Finalement, pour embellir le CSS, nous avons utilisé le Framework SemanticUI qui ressemble en certain points à Bootstrap.
<h4>Étape 2 : Connexion à Constellation, State Object et Messages Callback</h4>
Pour nous connecter à Constellation avec AngularJS, nous avons utilisé ces lignes de codes :
<pre title="Connexion à Constellation" class="lang:javascript decode:true">Constellation.initializeClient(url_port, sha, "FriendLeaf");
</pre>
<pre title="Connexion à Constellation" class="lang:javascript decode:true">Constellation.onConnectionStateChanged(function (change) {
   //suite
});
</pre>
et une ligne de ce type pour chaque State Object :
<pre title="Abonnement à un StateObject" class="lang:javascript decode:true">Constellation.registerStateObjectLink("*", "FriendLeafCapteurs", "Actionneurs", "*", function (so) {
    //suite
});
</pre>
Quant aux messages Callback, ils sont envoyés comme suit :
<pre title="Invocation d'un MessageCallback" class="lang:javascript decode:true">Constellation.sendMessage({ Scope: 'Package', Args: ['FriendLeafCapteurs'] }, 'DesactiverBrumisateur');</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure28.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Page de configuration" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure28_thumb.png" alt="Page de configuration" width="184" height="364" border="0" /></a></p>
Pour que l’application soit utilisable sur n’importe quelle Constellation, nous avons créé un page de configuration où l’utilisateur rentre l’adresse de sa constellation, le port, et des identifiants.

Une fois tout cela rempli, le nom d’utilisateur et le mot de passe sont hachés en SHA1 grâce à une librairie externe et la connexion à Constellation commence.

Lorsque l’application se lance pour la première fois, l'utilisateur est directement redirigé vers cette page pour entrer les informations nécessaires.
<h4>Étape 3 : Affichage des données en temps réel</h4>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure29.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Le tableau de bord " src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure29_thumb.png" alt="Le tableau de bord" width="184" height="364" border="0" /></a></p>
Pour l’affichage des données en temps réel nous avons utilisé des images SVG car elles sont très flexibles et nous voulions réaliser des barres de progression en arc de cercle. Pour les valeurs numériques on affiche un scope AngularJS dans lequel sont stockés les valeurs des capteurs.

Le code JS :
<pre title="Consommation des SO" class="lang:javascript decode:true">//Stockage des valeurs des capteurs dans la variable capteur
scope.capteurs["humiditeAir"] = so.Value.HumiditeAir;
scope.$apply(); //Applications des modifications du scope

//Pour changer le remplissage de la jauge 
$("#h_air_gauge").css("stroke-dasharray",(so.Value.HumiditeAir*18)/100 + " 18 0"); 
</pre>
SVG en HTML :
<pre title="La ViewBox SVG en HTML" class="lang:html5 decode:true">&lt;svg viewbox="0 0.5 10 8"&gt;
	&lt;defs&gt;
	&lt;linearGradient id="linear" x1="0%" y1="0%" x2="100%" y2="0%"&gt;
		&lt;stop offset="0%" stop-color="#00ee4f"/&gt;
		&lt;stop offset="66%" stop-color="#eeae00"/&gt;
		&lt;stop offset="100%" stop-color="#ee0000"/&gt;
	&lt;/linearGradient&gt;
	&lt;/defs&gt;
	&lt;text x="50%" y="50%" id="h_air_value" text-anchor="middle"  alignment-baseline="middle" fill="#00ee4f"&gt;
		{{Math.round(capteurs["humiditeAir"])}}%
	&lt;/text&gt;
	&lt;path d="M2 8 A 4 4 0 1 1 8 8" fill="none" stroke-width="0.78" stroke="#E8F6FD" /&gt;
	&lt;path class="loader" id="h_air_gauge" d="M2 8 A 4 4 0 1 1 8 8" fill="none" stroke-width="0.8" 
stroke="url(#linear)" /&gt;
&lt;/svg&gt;
</pre>
<h4>Étape 4 : Contrôler sa serre</h4>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Page de contrôle" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure32_thumb.png" alt="Page de contrôle" width="124" height="244" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure30.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Page de contrôle" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure30_thumb.png" alt="Page de contrôle" width="124" height="244" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure31.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Page de contrôle" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure31_thumb.png" alt="Page de contrôle" width="124" height="244" border="0" /></a></p>
Nous avons également prévu dans l’application de pouvoir gérer les différents actionneurs de la serre.

Pour gérer le côté automatique du package responsable des capteurs nous avons utilisés des sliders qui lorsqu’activés, vont envoyer un Message Callback comme décrit ci-dessus. On va également suivre l’évolution du State Object indiquant si l’automatisation est activée pour tel ou tel actionneur et ainsi bloqué ou non le bouton d’activation manuel. Car oui, il est également possible d’activer manuellement chaque actionneur grâce à un bouton.
<h4>Étape 5 : Les graphiques</h4>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Graphiques" src="https://developer.myconstellation.io/wp-content/uploads/2018/07/figure33_thumb.png" alt="Graphiques" width="184" height="361" border="0" /></a></p>
Pour ce qui est des graphiques nous avons utilisé une librairie externe que nous avons modifié pour la rendre compatible sur mobile. Cette librairie s’appelle Chartist. Grâce à elle nous avons pu faire de superbes graphiques.
<h3>Conclusion</h3>
Voilà qui conclut les grandes étapes de la réalisation de FriendLeaf. Comme vous avez pu le voir, la serre remplit complètement son rôle. C’est un projet ludique, simple à réaliser et facilement transposable sur d’autres installations grâce aux State Objects et aux messages Callback. C’est également un bon point de départ pour prendre en main la plateforme Constellation. Nous espérons vraiment qu’il vous a plu et que vous allez réaliser votre propre serre.

Nous tenons également à remercier Léa, le jardin de Théo et les parents de Marine qui nous ont fourni quelques accessoires nécessaires à la serre.