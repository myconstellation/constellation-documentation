---
ID: 5519
post_title: 'Cr&eacute;er une bo&icirc;te aux lettres connect&eacute;e avec Constellation'
author: Sebastien Warin
post_date: 2017-09-27 12:29:50
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/creer-une-boite-aux-lettres-connectee-constellation/
published: true
post_modified: 2017-09-27 15:03:54
---
<em>Projet réalisé par Judith Caroff, Jeanne Leclercq, Luc Fermaut, Pierre Hourdé, Jean-Baptiste Lavoine et Victorien Renault.</em>
<h2>Introduction</h2>
Étudiants en 3<sup>ème</sup> année à l’ISEN-Lille, nous avons eu l’idée de développer une boîte aux lettres connectée en utilisant la plateforme Constellation.

En réalisant ce projet, nous voulions proposer une solution de boîte aux lettres connectée à un prix raisonnable et possédant une interface fluide pour améliorer l’expérience de l’utilisateur. Pour cela, nous avons acheté des composants peu coûteux à placer sur notre boîte aux lettres, et développé une application Ionic.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Schma-de-prsentation.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Schéma de fonctionnement global de notre boîte aux lettres" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Schma-de-prsentation_thumb.png" alt="Schéma de fonctionnement global de notre boîte aux lettres" width="450" height="225" border="0" /></a></p>
Notre boîte aux lettres est connectée à la plateforme Constellation par le biais d’un ESP8266, vous pouvez le connecter facilement à votre Constellation en suivant <a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">cette documentation sur le site Constellation</a>.

Nous détectons ensuite la présence de courriers à l’aide de capteurs à ultrasons.

Avec un lecteur de cartes NFC, nous contrôlons l’accès à notre boîte aux lettres. Celui-ci nous permet de savoir qui a ouvert la boîte aux lettres et donc d’envoyer les bonnes informations à notre Constellation. Le lecteur NFC nous permet également de vérifier qu’une carte ou qu’un badge a les autorisations nécessaires afin de commander l’ouverture via un servo-moteur.

L’application, développée avec le framework ionic nous permet d’offrir une interface utilisateur ergonomique. En la connectant à Constellation, l’utilisateur a accès à toutes les informations nécessaires très facilement.
<h2>La boîte aux lettres connectée</h2>
<i>Prérequis : Avoir connecté l’ESP8266 à Constellation.</i>
<h3>Composants utilisés</h3>
Pour connecter notre boîte aux lettres à Constellation, nous avons utilisé :
<ul>
 	<li>Un ESP8266 D1 Mini (8€ sur Amazon)</li>
 	<li>Des capteurs à ultrasons HC SR04 (3€ sur Amazon)</li>
 	<li>Un lecteur de cartes NFC (8€ sur Amazon)</li>
 	<li>Un servo-moteur (6€ sur eBay)</li>
</ul>
Voici la correspondance pour définir les différents pins avec cet ESP :
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">D0</td>
<td valign="top">D1</td>
<td valign="top">D2</td>
<td valign="top">D3</td>
<td valign="top">D4</td>
<td valign="top">D5</td>
<td valign="top">D6</td>
<td valign="top">D7</td>
<td valign="top">D8</td>
<td valign="top">RX</td>
<td valign="top">TX</td>
</tr>
<tr>
<td valign="top">16</td>
<td valign="top">5</td>
<td valign="top">4</td>
<td valign="top">0</td>
<td valign="top">2</td>
<td valign="top">14</td>
<td valign="top">12</td>
<td valign="top">13</td>
<td valign="top">15</td>
<td valign="top">3</td>
<td valign="top">1</td>
</tr>
</tbody>
</table>
<h3>Etape 1 : Détecter la présence d’une lettre grâce à un capteur ultrason</h3>
<i>Nos capteurs fonctionnent avec une alimentation de 5V</i><i></i>

Les capteurs à ultrasons nous permettent de mesurer une distance en créant une impulsion sur une des broches. Grâce à cette mesure, nous pouvons détecter une variation de la distance lorsqu’un courrier et inséré dans la boîte aux lettres. Nous envoyons alors l’information à Constellation grâce à un MessageCallback.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/capteur-ultrason.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Capteur ultrason" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/capteur-ultrason_thumb.jpg" alt="Capteur ultrason" width="300" height="136" border="0" /></a></p>
Relier le capteur à notre ESP est assez simple, notre capteur à ultrasons possède 4 branches, chacune branchée sur un pin de notre ESP.

Ci-dessous, les correspondances entre les branches du capteur à ultrasons et les pins de l’ESP8266 :
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="63%">
<p style="text-align: left;">Branche du capteur à ultrasons</p>
</td>
<td valign="top" width="36%">Pin de l’ESP8266</td>
</tr>
<tr>
<td valign="top" width="63%">VCC</td>
<td valign="top" width="36%">5V</td>
</tr>
<tr>
<td valign="top" width="63%">GND</td>
<td valign="top" width="36%">GND</td>
</tr>
<tr>
<td valign="top" width="63%">TRIG</td>
<td valign="top" width="36%">D4</td>
</tr>
<tr>
<td valign="top" width="63%">ECHO</td>
<td valign="top" width="36%">D3</td>
</tr>
</tbody>
</table>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Module-ultrasons.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Module à ultrasons connecté à notre ESP8266" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Module-ultrasons_thumb.png" alt="Module à ultrasons connecté à notre ESP8266" width="354" height="205" border="0" /></a></p>
<p style="text-align: left;">Dans notre code, nous configurons les pins dans le setup afin de préparer l’impulsion.</p>

<pre class="lang:arduino decode:true"> void setup() {
   pinMode(TRIGGER, OUTPUT);
   digitalWrite(TRIGGER, LOW); //La broche TRIGGER doit être à LOW au repos
   pinMode(ECHO, INPUT);
   Serial.begin(9600);
 }
</pre>
<p style="text-align: left;">Nous envoyons ensuite de façon continue des impulsions pour savoir si un obstacle est présent puis, s’il y en a un, nous envoyons un Message Callback.</p>

<pre class="lang:cpp decode:true"> void loop() {    
    /* 1. Lance une mesure de distance en envoyant une impulsion HIGH de 10 µs sur la broche TRIGGER */
   digitalWrite(TRIGGER, HIGH);
   delayMicroseconds(10);
   digitalWrite(TRIGGER, LOW);
   
   /* 2. Mesure le temps entre l'envoi de l'impulsion ultrasonore et son écho (s'il existe) */
   long measure = pulseIn(ECHO, HIGH);                
                                                                                 
   /* 3. Calcul la distance à partir du temps mesuré */
   long distanceMeasured = measure *vitesse ;
  
   /* 4. Envoie d'un MessageCallback à Constellation si on rentre dans le if */ 
   if(distanceMeasured &lt; defaultValue){
     constellation.sendMessage(Package, "Brain", "message");
   }
}
</pre>
<p style="text-align: center;"><img class="size-full wp-image-5527 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/video1-2.gif" alt="" width="196" height="350" /></p>

<h3 style="text-align: left;">Etape 2 : Gérer les accès à la boîte aux lettres grâce à un lecteur de carte NFC</h3>
<p style="text-align: left;">Dans cette partie, nous allons envoyer un Message Callback à Constellation grâce à l’ID du badge NFC détecté par notre lecteur de cartes.</p>
<p style="text-align: left;">Cela nous permettra par la suite de gérer les autorisations d’accès à la boîte aux lettres, de contrôler l’ouverture par notre servo-moteur, et d’avertir l’utilisateur qu’un colis a été livré ou que le courrier a été récupéré. Nous devrons donc enregistrer les différents utilisateurs en renseignant leur ID de badge et leur statut (facteur ou utilisateur).</p>
<p style="text-align: left;">Voici le câblage réalisé pour cette partie entre le NFC et l’ESP :</p>

<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="49%">Branche du NFC</td>
<td valign="top" width="50%">Pin de l’ESP8266</td>
</tr>
<tr>
<td valign="top" width="49%">RST</td>
<td valign="top" width="50%">D1</td>
</tr>
<tr>
<td valign="top" width="49%">SDA(SS)</td>
<td valign="top" width="50%">D2</td>
</tr>
<tr>
<td valign="top" width="49%">MOSI</td>
<td valign="top" width="50%">D7</td>
</tr>
<tr>
<td valign="top" width="49%">MISO</td>
<td valign="top" width="50%">D6</td>
</tr>
<tr>
<td valign="top" width="49%">SCK</td>
<td valign="top" width="50%">D5</td>
</tr>
<tr>
<td valign="top" width="49%">GND</td>
<td valign="top" width="50%">GND</td>
</tr>
<tr>
<td valign="top" width="49%">3.3 V</td>
<td valign="top" width="50%">3.3 V</td>
</tr>
</tbody>
</table>
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Lecteur-NFC.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Lecteur NFC connecté à notre ESP8266" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Lecteur-NFC_thumb.png" alt="Lecteur NFC connecté à notre ESP8266" width="354" height="183" border="0" /></a></p>
<p style="text-align: left;">Il faut commencer par télécharger la bibliothèque suivante :</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/bibliothque-arduino-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="bibliothèque arduino 1" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/bibliothque-arduino-1_thumb.png" alt="bibliothèque arduino 1" width="354" height="71" border="0" /></a></p>
<p style="text-align: left;">Voici ensuite la fonction nécessaire à l’envoi d’un Message Callback lorsque nous captons un nouveau badge.</p>

<pre class="lang:cpp decode:true">void NFC() {
	// Regarde s'il y a une nouvelle carte, s'il n'y en a pas on quitte la fonction
	if ( ! mfrc522.PICC_IsNewCardPresent()) {
		return;
	}
	// S'il n'arrive pas à lire la carte on quitte la fonction
	if ( ! mfrc522.PICC_ReadCardSerial()) {
		return;
	}
	char sochar[256]; // 256 correspond à un octet
	String UID= dump_byte_array(mfrc522.uid.uidByte, mfrc522.uid.size); 
	/* Permet de transformer un tableau de bit en String */
	UID.toCharArray(sochar, 256);
	constellation.sendMessage(Package, "Brain", "Authorisation", sochar); /* envoie un message Callback du package Brain appelé Authorisation*/
}</pre>
<h3 style="text-align: left;">Etape 3 : Ouvrir et fermer la porte grâce à un servo-moteur</h3>
<p style="text-align: left;">Le servomoteur va nous servir à commander notre verrou. Ce dernier va donc s’abonner à un State Object nous donnant l’état de la porte (verrouillée ou déverrouillée). Ainsi, lorsque notre lecteur NFC captera un ID autorisé, notre State Object changera, ainsi que l’état de verrouillage de la porte.</p>
<p style="text-align: left;">Voici les branchements qui relient notre moteur à l’ESP :</p>

<table class=" aligncenter" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="59%">PIN DU MOTEUR</td>
<td valign="top" width="40%">Pin de l’ESP8266</td>
</tr>
<tr>
<td valign="top" width="59%">5 V (câble orange)</td>
<td valign="top" width="40%">5 V</td>
</tr>
<tr>
<td valign="top" width="59%">GND (câble marron)</td>
<td valign="top" width="40%">GND</td>
</tr>
<tr>
<td valign="top" width="59%">Commande (câble jaune)</td>
<td valign="top" width="40%">D8</td>
</tr>
</tbody>
</table>
<p style="text-align: left;">Avant d’utiliser le code ci-dessous, il faut vérifier que vous avez bien installé les bibliothèques suivantes :</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/bibliothque-arduino-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="bibliothèque arduino 2" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/bibliothque-arduino-2_thumb.png" alt="bibliothèque arduino 2" width="354" height="128" border="0" /></a></p>
<p style="text-align: left;">Dans le Setup ci-dessous, nous nous abonnons à un StateObject qui nous permet d’ouvrir et de fermer la porte lorsqu’il change d’état.</p>

<pre class="lang:cpp decode:true">void setup() {
    monServo.attach(15); // Définit le moteur au D8
    monServo.write(0); // ServoMoteur à sa position initiale
    constellation.registerStateObjectLink("*", "Brain", "Porte_ouverte", [](JsonObject&amp; so) {
     delay(100); // délai afin d'attendre la connexion à  Constellation
     if (so["Value"]== true){   
       monServo.write(90);  // Position de la porte ouverte
       }
 
    if (so["Value"]== false){
       monServo.write(0);   // Position de la porte fermée
       }
       delay(3000); // attendre au minimum 3 secs avant que la porte ne change d'état.
   });  
 }
</pre>
<p style="text-align: center;"><img class="alignnone size-full wp-image-5528 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/video2-2.gif" alt="" width="196" height="350" /></p>

<h3 style="text-align: left;">Etape 4 : Synchroniser toutes nos parties avec un package Constellation</h3>
<p style="text-align: left;">Vous pouvez retrouver notre code complet pour la boîte aux lettres sur GitHub pour plus de précision (<a href="https://l.facebook.com/l.php?u=https%3A%2F%2Fgithub.com%2FSqyluck%2FBoite-aux-lettres-connectee&amp;h=ATO-A7kid-AyGmrtD0hCDSOEWcwWMsyF8aAuEcrDY3b1Q9b-_NEMOnsJ9DOWADshjfo8FwRAmqfoT5MoHe3RwBJ1U_2af1vgeNd_XHbk_4LNVdA4-AYBgC-v6mcC5YakbL12cgMSWJFFBA">https://github.com/Sqyluck/Boite-aux-lettres-connectee</a>).</p>
<p style="text-align: left;">Il est ensuite nécessaire de créer un package Constellation, qui va nous permettre de traiter toutes les données de nos différents éléments.</p>
<p style="text-align: left;">→ Voici les différents Message Callbacks créent dans le package afin de répondre à nos besoins.</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Message-Callbacks.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Message Callbacks" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Message-Callbacks_thumb.png" alt="Message Callbacks" width="354" height="357" border="0" /></a></p>
<p style="text-align: left;">→ Voici la liste des State Objects générés par le package pour permettre l’affichage de nos données dans l’application et l’envoi d’ordre au servomoteur via l’ESP8266.</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/State-Objects.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="State Objects" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/State-Objects_thumb.png" alt="State Objects" width="354" height="166" border="0" /></a></p>

<h3 style="text-align: left;">Etape 5 : Fabrication de la boîte aux lettres</h3>
<p style="text-align: left;">Nous avons utilisé des planches en bois afin de réaliser notre boîte aux lettres. Nous avons vissé les planches entre elles, puis nous avons fixé nos éléments sous le toit de la boîte.</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-extrieur.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Visuel boite au lettre extérieur" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-extrieur_thumb.jpg" alt="Visuel boite au lettre extérieur" width="254" height="310" border="0" /></a></p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-composants.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Visuel boite au lettre composants" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-composants_thumb.jpg" alt="Visuel boite au lettre composants" width="223" height="244" border="0" /></a> <a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-intrieur.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Visuel boite au lettre intérieur" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Visuel-boite-au-lettre-intrieur_thumb.jpg" alt="Visuel boite au lettre intérieur" width="207" height="244" border="0" /></a></p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/Schma-composants.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Schéma composants" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/Schma-composants_thumb.png" alt="Schéma composants" width="450" height="232" border="0" /></a></p>

<h2 style="text-align: left;">L’interface utilisateur</h2>
<p style="text-align: left;"><i>Prérequis : disposer de Node.js et de npm</i></p>

<h3 style="text-align: left;">Etape 1 : Créer une application avec ionic</h3>
<p style="text-align: left;">Afin de réaliser notre interface utilisateur, nous avons décidé dans un premier temps de développer notre application avec Ionic 2 dans Visual Studio. Mais avec cette méthode nous étions obligé de développer notre application en langage typescript ce qui était plus compliqué pour connecter l’application à la plateforme Constellation. Nous avons donc décidé d’utiliser Ionic 1 afin de développer notre application en javascript et de la connecter plus facilement.</p>
<p style="text-align: left;">Avant de commencer, il faut lancer l’installation de Ionic 1 depuis l’invite de commande :</p>

<pre class="lang:default decode:true">npm install – g cordova ionic</pre>
<p style="text-align: left;"><i>(Attention : cette commande ne fonctionnera pas si vous ne disposez pas auparavant de node.js et de npm)</i><i></i></p>
<p style="text-align: left;">Une fois que nous nous sommes placé dans le dossier dans lequel nous souhaitons développer notre application, nous pouvons utiliser la commande suivante pour lancer la création d’un nouveau projet.</p>

<pre class="lang:default decode:true">ionic start myApp tabs --type ionic1</pre>
<p style="text-align: left;"><i>(Nous venons ici de créer un nouveau projet ionic 1 de type tabs intitulé myApp)</i></p>
<p style="text-align: left;">Dans l’invite de commande, placez-vous dans le dossier de l’application (en utilisant la commande cd).</p>
<p style="text-align: left;">La commande ionic serve permet d’exécuter l’application dans un navigateur.</p>
<p style="text-align: left;">Lorsque le projet est créé, vous devez éditer vos différentes pages (par ex. sur Notepad++). Les fichiers à modifier se trouvent dans le dossier « www » :</p>

<ul style="text-align: left;">
 	<li>Le fichier index.html</li>
 	<li>Les fichiers .html appartenant au dossier “templates” → C’est ici que vous pouvez modifier le contenu de chaque page de votre application.</li>
 	<li>Le fichier app.js dans le dossier “js” → Vous devez définir votre constellation à cet endroit.</li>
 	<li>Le fichier controllers.js dans le dossier “js” → Chaque controller est associé à une page html. Vous pouvez utiliser les variables et les fonctions définies dans ces pages grâce aux “Scopes”.</li>
</ul>
<h3 style="text-align: left;">Etape 2 : Visualiser notre application sur un smartphone</h3>
<p style="text-align: left;">Si vous souhaitez utiliser votre application sur votre smartphone, vous pouvez utiliser l’application “ionic view” disponible sur l’App Store et Google Play. Cette application vous permettra de visualiser directement l’application que vous êtes en train de développer sur votre smartphone.</p>
<p style="text-align: left;">Une fois l’application installée, vous devez ensuite vous rendre <a href="https://apps.ionic.io/apps/">sur ce site</a>, créer un compte, puis cliquez sur “New App”. Il ne vous reste plus qu’à faire le lien avec votre application et l’envoyer sur votre appareil. Pour cela, dans le dossier de votre application, utilisez les deux commandes suivantes dans votre invite de commande</p>

<pre class="lang:default decode:true">ionic link 
ionic upload</pre>
<p style="text-align: left;">Ouvrez ensuite ionic view et visualisez votre application.</p>
<p style="text-align: left;">Vous pouvez également, après avoir branché votre téléphone et avoir activé le mode debug, utiliser la commande cordova run android afin de simuler directement votre application sur votre téléphone android.</p>

<h3 style="text-align: left;">Etape 3 : Connecter notre application à constellation</h3>
<p style="text-align: left;">Afin de connecter notre application à la plateforme constellation, nous allons modifier le fichier app.js ainsi que le index.html.</p>
<p style="text-align: left;">Pour cela, nous allons importer les bibliothèques suivantes dans le fichier index.html :</p>

<ul style="text-align: left;">
 	<li>jquery-2.2.4.min.js</li>
 	<li>jquery.signalr-2.2.1.min.js</li>
 	<li>Constellation-1.8.1.min.js</li>
 	<li>ngConstellation-1.8.1.min.js</li>
</ul>
<p style="text-align: left;">Les librairies sont disponibles sur cette <a href="http://cdn.myconstellation.io/js/">page</a>. Nous vous conseillons de créer un dossier intitulé constellation dans le dossier « lib » et d’y enregistrer ces différentes bibliothèques.</p>
<p style="text-align: left;">Il ne vous restera plus qu’à ajouter les lignes de code suivantes dans le fichier index.html :</p>

<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="lib/constellation/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="lib/constellation/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="lib/constellation/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="lib/constellation/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt; 
</pre>
<p style="text-align: left;">Puis nous allons ajouter le code suivant à notre fichier app.js :</p>
<p style="text-align: left;">1. Modifier la ligne angular.module, y ajouter ‘ngConstellation’ comme ci-dessous :</p>

<pre class="lang:javascript decode:true">angular.module('starter', ['ionic', 'starter.controllers', 'ngConstellation'])</pre>
<p style="text-align: left;">2. Modifier la fonction .run, y ajouter $rootScope et constellationConsumer :</p>

<pre class="lang:javascript decode:true">.run(function($ionicPlatform, $rootScope, constellationConsumer)</pre>
<p style="text-align: left;">3. Ajouter le code suivant dans la fonction .run :</p>

<pre class="lang:javascript decode:true">constellationConsumer.initializeClient("http://192.168.43.171:8088", "123456789", "Application Ionic");
	constellationConsumer.onConnectionStateChanged(function (change) {
		if (change.newState === $.signalR.connectionState.connected) {
			console.log("Connecté à constellation");
			$rootScope.isConnected = true;
		}
	});
	constellationConsumer.connect();
	$rootScope.constellation = constellationConsumer;
</pre>
<p style="text-align: left;">Vous pouvez maintenant vérifier que vous êtes bien connecté à votre constellation en regardant dans votre console (f12 dans votre navigateur).</p>

<h3 style="text-align: left;">Etape 4 : Connecter notre application à la boîte aux lettres</h3>
<p style="text-align: left;">Un certain nombre de State Objects et de Message Callbacks ont été réalisés dans notre package constellation afin de traiter nos différentes informations (ce package est disponible sur gitHub).</p>
<p style="text-align: left;">Le rôle de notre application ici est simplement de s’abonner aux State Objects et d’envoyer des messages Callbacks à notre constellation.</p>
<p style="text-align: left;">→ <b>Exemple de récupération d’un State Object</b> :</p>
<p style="text-align: left;">Les StateObjects se récupèrent dans le fichier app.js, à l’endroit où nous avons défini notre constellation. Voici un exemple de code correspondant à notre State Object « Users » :</p>

<pre class="lang:javascript decode:true">//State Object Utilisateurs
constellationConsumer.registerStateObjectLink("*", "Brain", "Users", "*", function(so) {
	$rootScope.Users = so.Value;
	$rootScope.$apply();
});</pre>
<p style="text-align: left;">Pour afficher nos utlisateurs, il nous suffit maintenant, dans notre page html d’utiliser le code suivant :</p>

<pre class="lang:html5 decode:true">&lt;ion-item ng-repeat="user in Users"&gt;
  &lt;h2&gt;{{user.firstName}} {{user.name}}&lt;/h2&gt;
  &lt;p&gt;{{user.client ? "client":"facteur"}}&lt;/p&gt;
&lt;/ion-item&gt;
</pre>
<p style="text-align: left;">→ <b>Exemple d’envoi d’un message Callback</b> :</p>
<p style="text-align: left;">Les messages Callbacks s’envoient dans le fichier controllers.js. Voici l’exemple de code pour le Message Callback « DeleteUser » :</p>

<pre class="lang:javascript decode:true">//Message CallBack DeleteUser
$scope.name = $stateParams.name
$scope.firstName = $stateParams.firstName;
$scope.deleteUser = function() {
    var deleteUser = $ionicPopup.confirm({
      title: 'Supprimer cet utilisateur',
      template: 'Etes vous sur de vouloir supprimer cet utilisateur ?'
    });
    deleteUser.then(function(res) {
      if(res) {
          $rootScope.constellation.sendMessage({ Scope: 'Package', Args: ['Brain'] }, 'DeleteUser', [$scope.firstName, $scope.name ]);
          $state.go('tab.utilisateurs');
          console.log('deleted');
      } 
      else {
          console.log('do nothing');
      }
    });
};
</pre>
<p style="text-align: left;">Dans notre page html, on appelle maintenant la fonction créée dans notre controller.js dans un bouton par exemple :</p>

<pre class="lang:html5 decode:true">&lt;button ng-click="deleteUser()"&gt;Supprimer cet utilisateur&lt;/button&gt;
</pre>
<p style="text-align: left;">En vous servant de cette base, vous pouvez réaliser une application vous permettant de recevoir un message sur votre application lorsque vous recevez un courrier ou un colis mais aussi d’ajouter ou de supprimer des accès à la boite aux lettres.</p>
<p style="text-align: center;"><img class="alignnone size-full wp-image-5530 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/video3-2.gif" alt="" width="196" height="350" /></p>
<p style="text-align: left;">Il est également possible de réutiliser des packages existants, par exemple, nous nous sommes servis du package PushBullet afin de recevoir des notifications sur notre smartphone.</p>
<p style="text-align: left;">Voici un aperçu de notre application ionic :</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/notifications.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="notifications" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/notifications_thumb.png" alt="notifications" width="150" height="295" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/paramtres.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="paramètres" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/paramtres_thumb.png" alt="paramètres" width="150" height="295" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/utilisateurs.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="utilisateurs" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/utilisateurs_thumb.png" alt="utilisateurs" width="150" height="295" border="0" /></a></p>