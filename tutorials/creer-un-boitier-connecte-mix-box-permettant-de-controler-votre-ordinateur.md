---
ID: 5991
post_title: >
  Créer un boitier connecté « Mix
  box » permettant de contrôler votre
  ordinateur
author: Sebastien Warin
post_date: 2018-05-16 16:24:43
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-un-boitier-connecte-mix-box-permettant-de-controler-votre-ordinateur/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-05-16 16:25:10
---
<em>Tutoriel proposé par Marc-Antoine DUVAL, Joël GUILLEM et Quentin LEVERT.</em>

[toc]

Dans ce tutoriel, nous allons créer un boitier qui est relié à un ou plusieurs ordinateurs de la maison grâce à un ESP8266 connecté à constellation. Des LEDs permettent à l’utilisateur de suivre le résultat des commandes en temps réel. Le boitier permet de commander à distance le lecteur de vidéo VLC media player à travers différentes fonctionnalités. Il gère :
<ul>
 	<li>Le volume</li>
 	<li>La marche avant / marche arrière au cours d’un visionnage</li>
 	<li>La mise en lecture / pause d’une vidéo</li>
 	<li>La fermeture de VLC</li>
</ul>
<h2>Prérequis</h2>
<ul>
 	<li>Un ESP8266, nous utilisons un D1-mini</li>
 	<li>6 boutons poussoirs tactiles blancs</li>
 	<li>2 LEDs RGB</li>
 	<li>Une plaque de plexiglas noire</li>
 	<li>Une imprimante laser</li>
 	<li>Un serveur constellation</li>
 	<li>Le package « Windows Control » déployé sur une ou plusieurs sentinelles</li>
</ul>
<h2>Etape 1 : Réaliser le montage des boutons et LEDs</h2>
Dans un premier temps, nous allons réaliser le montage électronique des composants. Tout d’abord il est nécessaire de brancher l’ESP8266 à la breadboard et de l’alimenter en le connectant en mini-USB à une source d'alimentation. Il faut ensuite brancher la masse de l’ESP à la colonne de masse de la breadboard.

Pour les besoins de l’explication du montage, tous les autres composants seront connectés à la breadboard, cependant il est tout à fait possible de les connecter une fois insérés dans le boitier (voir Etape 2). Le schéma global est présenté ci-après.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/Cablage.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Cablage" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/Cablage_thumb.png" alt="Cablage" width="354" height="357" border="0" /></a></p>
&nbsp;

Ici nous avons choisi 2 LEDs RGB afin d’améliorer l’effet visuel pour obtenir une LED de couleur jaune. La première LED verte pourra être connectée directement au pin D8 de l’ESP. Quant à la seconde LED RGB, il suffit de connecter les pattes correspondant aux LEDs rouge et verte à la même ligne, puis de lier cette ligne au pin D7 de l’ESP.

Les boutons poussoirs sont à connecter avec les broches 1 et 3 d’un côté et les broches 2 et 4 de l’autre. Le branchement est classique, à savoir les pin D1 à D6 connectés directement sur la patte 1 de chaque bouton, puis retour à la masse sur la patte 3 des boutons. Lorsque l’on appuie sur le bouton, le pin correspondant passera donc en état bas (LOW) et est en état haut (HIGH) tant que l’on n’appuie pas.

Une fois le câblage terminé, nous allons passer à la réalisation du boitier qui contiendra à l’intérieur l’ESP et la breadboard.
<h2>Etape 2 : Réaliser le boitier</h2>
Dans un second temps, il nous faut un boitier que nous pourrons ouvrir pour mettre notre ESP. Le design de la face de devant doit aussi correspondre au nombre de boutons et LED que l’on souhaite mettre en place.

Afin de résoudre cette problématique, nous avons décidé d’imprimer le boitier grâce à une imprimante laser. Nous avons utilisé une plaque de plexiglas noir d’une épaisseur de 3mm.

Tout d’abord, nous avons créé le fond de la boite sans le couvercle. Nous avons utilisé le site : <a href="http://carrefour-numerique.cite-sciences.fr/fablab/wiki/doku.php?id=projets:generateur_de_boites">http://carrefour-numerique.cite-sciences.fr/fablab/wiki/doku.php?id=projets:generateur_de_boites</a> qui permet de générer automatique le fichier .svg qui sera reconnu et utilisé pour l’impression laser. Il suffit d’entrer les dimensions voulues.

Ensuite nous avons utilisé SolidWorks afin de créer notre face avant personnalisée. Nous avons choisi percer 2 trous pour les LEDs et 6 pour les boutons carrés. Après la réalisation de la pièce, plusieurs étapes sont alors nécessaires pour exporter le fichier SolidWorks en fichier .svg.
<ul>
 	<li>Sélectionner la vue correspondant à la pièce à réaliser</li>
 	<li>Enregistrer le fichier SolidWorks au format .DXF.</li>
 	<li>Sélectionner la vue à exporter. Ici, on sélectionne la vue en cours.</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/face_avant.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Choix de la face à sauvegarder" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/face_avant_thumb.png" alt="Choix de la face à sauvegarder" width="354" height="194" border="0" /></a></p>

<ul>
 	<li>Installer Inkscape, ce logiciel permettra de créer le fichier pour l’imprimante laser.</li>
 	<li>Importer le fichier dans Inkscape. Il faut désactiver mise à l’échelle automatique.</li>
 	<li>Sauvegarder le fichier qui sera utilisé pour l’impression laser.</li>
</ul>
Enfin, nous avons assemblé toutes les pièces du boitier :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/Boitier.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Boitier" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/Boitier_thumb.jpg" alt="Boitier" width="254" height="316" border="0" /></a></p>

<h2>Etape 3 : Programmation</h2>
Dans un premier temps, il faut vous assurer que votre ESP8266 soit connecté à votre constellation, si vous ne savez pas comment vous y prendre, je vous invite donc à suivre le tutoriel réalisé par Sébastien Warin : <a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/</a> .

Pour commencer, il faut déclarer le matériel dans la méthode setup de notre ESP8266, conformément à <b>l'étape 1, </b>nous avons choisi les pin 1 à 6 en mode PULLUP pour les boutons et les pins 7 &amp; 8 pour les LEDs :
<pre title="Initialisation des inputs" class="lang:cpp decode:true">  pinMode(D1,INPUT_PULLUP);
  pinMode(D2,INPUT_PULLUP);
  pinMode(D3,INPUT_PULLUP);
  pinMode(D4,INPUT_PULLUP);
  pinMode(D5,INPUT_PULLUP);
  pinMode(D6,INPUT_PULLUP);
  pinMode(D7,OUTPUT);
  pinMode(D8,OUTPUT);
  digitalWrite(D7,LOW);
  digitalWrite(D8,LOW);
</pre>
Nous avons associé à chaque bouton un messageCallback avec les paramètres permettant d'effectuer l'action voulu. Pour faire cela, nous avons réalisé une fonction "checkButtonState" qui permet de gérer les évènements d'appuie sur les boutons et d'appeler le messageCallback correspondant au bouton. Juste avant l'appel du messageCallback, la led cablée au pin D7 est allumée pour une période de 2 secondes afin de montrer à l'utilisateur que l'ESP8266 a bien réalisé l'action. Cela permettra en cas de disfonctionnement, que l'utilisateur puisse savoir si c'est le hardware ou la transmission Wifi/Constellation qui est défectueux.

Le tableau ci-dessous récapitule les actions, messageCallback et pins qui sont associés ensemble :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/05/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/05/image_thumb-6.png" alt="image" width="450" height="109" border="0" /></a></p>
Ci-dessous le code de la fonction checkButtonState :
<pre title="Gestion des boutons" class="lang:cpp decode:true">void checkButtonState(){
  
  static int lastButtonStateD1 = HIGH;
  static int lastButtonStateD2 = HIGH;
  static int lastButtonStateD3 = HIGH;
  static int lastButtonStateD4 = HIGH;
  static int lastButtonStateD5 = HIGH;
  static int lastButtonStateD6 = HIGH;
  static long led1TimeLitUP = 0;
  // If the led is lit up for 2 secondes, we shut off the led
  if(millis() - led1TimeLitUP &gt;2000){
      digitalWrite(D7, LOW);
    }
  // We retrieve the pin value
  int reading = digitalRead(D1);
// If pin value has changed
  if (reading != lastButtonStateD1){
   // And if pin value is high which mean button is pushed, then we make the associated action
    if (reading == HIGH){
      // We lit up the led in order to show to the user that the messageCallback is sending
       digitalWrite(D7,HIGH);
       // "led1TimeLitUp" take the value of the moment when it has been litted up
       led1TimeLitUP = millis();
       // we use the messageCallback to do the wanted action
       constellation.sendMessage(Package, "WindowsControl", "VolumeUp","{}");
     
      }
      lastButtonStateD1 = reading ;
     
      return;
  }
  reading = digitalRead(D2);
  if (reading != lastButtonStateD2){
     if (reading == HIGH){
        digitalWrite(D7,HIGH); 
        led1TimeLitUP = millis();
        constellation.sendMessage(Package, "WindowsControl", "VolumeDown", "{}"); 
        
      }
      lastButtonStateD2 = reading;
      return;
    }
  reading = digitalRead(D3);
  if (reading != lastButtonStateD3){
     if (reading == HIGH){
        digitalWrite(D7,HIGH); 
        led1TimeLitUP = millis();
        constellation.sendMessage(Package, "WindowsControl", "SendKey", "SpaceBar");
       
      }
      lastButtonStateD3 = reading;
      return;
    }
    
  reading = digitalRead(D4);
  if (reading != lastButtonStateD4){
     if (reading == HIGH){
        digitalWrite(D7,HIGH); 
        led1TimeLitUP = millis();
        constellation.sendMessage(Package, "WindowsControl", "SendCombinationOfKey", "['ALT', 'Left']");
        
      }
      lastButtonStateD4 = reading;
      return;
    }
   reading = digitalRead(D5);
    if (reading != lastButtonStateD5){
     if (reading == HIGH){
        digitalWrite(D7,HIGH); 
        led1TimeLitUP = millis();
        constellation.sendMessage(Package, "WindowsControl", "SendCombinationOfKey", "['ALT', 'Right']");
      
      }
      lastButtonStateD5 = reading;
      return;
    }
    reading = digitalRead(D6);
    if (reading != lastButtonStateD6){
     if (reading == HIGH){
        digitalWrite(D7,HIGH); 
        led1TimeLitUP = millis();
        constellation.sendMessage(Package, "WindowsControl", "SendCombinationOfKey", "['CTRL', 'Q']");
        
      }
      lastButtonStateD6 = reading;
      return;
    }  
  }
</pre>
Et voilà notre boitier de contrôle est opérationnel. Il ne reste plus qu’à l’alimenter et vous avons à notre disposition des boutons physiques pour piloter notre Windows facilement.