---
ID: 4680
post_title: >
  Connecter ses volets dans la
  Constellation avec des Arduino et un
  Raspberry
author: Sebastien Warin
post_date: 2015-06-08 11:21:39
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/connecter-volets-constellation-arduino-raspberry/
published: true
post_modified: 2017-05-11 11:23:34
---
<em>Ecrit par Lucas Dupuis en Juin 2015.</em>

Lorsque nous avons acheté notre maison, nous avons entamé de lourds travaux de rénovation ce qui m’a permis de passer du câble multifonction blindé à chaque point de contrôle (interrupteurs) dans l’optique de pouvoir facilement ajouter des fonctionnalités domotique sans avoir recours à des solutions sans fil comme le Z-Wave assez coûteux.

Après l’installation de pas moins d’une dizaine volets électrique dans la maison, il me fallait une solution de pilotage à la fois simple, fiable et surtout économique. De plus cette solution devra être ouverte me permettant de concevoir ma propre interface de contrôle sur tout type de support et de pouvoir facilement ajouter de l’intelligence.

Grace à la plateforme Constellation, un Raspberry Pi et quelques Arduinos, j’ai pu concevoir mon propre système de gestion des volets.
<h3>La conception</h3>
Concrètement chaque volet dispose de deux phases : une pour la montée et une autre pour la descente.

L’idée est donc de piloter ces deux phases avec des relais. Pour garder le contrôle manuel des volets (c’est-à-dire à partir des interrupteurs encastrés dans le mur), j’ai connecté chaque interrupteur bistable au système pour déclencher les volets (obligatoire pour une certification WAF J).

Pour cela derrière chaque interrupteur des volets, j’ai placé une carte de contrôle avec deux relais 220V et deux entrées. Chaque carte de contrôle est connectée sur un câble multifonction qui arrive à chaque volet. On utilise donc 6 fils : deux pour les entrées des interrupteurs (montée et descente), deux pour la commande des deux relais (montée et descente) et deux pour l’alimentation (5V ou 12V en fonction de vos relais).

Côté budget, comptez moins de 3€ par volet.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-68.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Carte de relais par volet" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-68.png" alt="Carte de relais par volet" width="304" height="404" border="0" /></a></p>
L’ensemble des câbles se rejoignent tous jusque dans ma baie réseau au RDC. Chaque volet a donc besoin de deux sorties pour les relais et deux entrées pour l’interrupteur bistable. Avec 10 volets, ça nous donne donc 20 sorties et 20 entrées, beaucoup trop pour un Raspberry !

Toujours dans l’optique de baisser les coûts, j’utilise un réseau d’Arduino Mini Pro acheté moins de 2€ pièce.

Chaque Arduino peut gérer jusqu’à 3 volets. Ils sont tous installés sur une carte et connectés en série sur un bus I2C sur lequel est également connecté un Raspberry qui sera le cerveau des volets, lui-même connecté dans la Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-69.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Centrale Arduino et Raspberry" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-69.png" alt="Centrale Arduino et Raspberry" width="454" height="342" border="0" /></a></p>
N’importe quel objet, page Web ou programme connecté dans la Constellation pourra envoyer un ordre à chaque volet. Chaque ordre sera traité par le package Constellation qui tourne sur le Raspberry qui, commandera l’Arduino associé via le bus I2C qui lui-même activera le relais correspondant à l’ordre de montée ou descente du volet cible !
<h3>Conception logicielle</h3>
Une fois nos volets connectés sur les cartes relais reliés aux Arduinos, il suffit d’activer les sorties pour basculer l’état des relais et donc piloter la montée ou la descente de chacun de nos volets.

Par exemple pour une montée :
<pre class="lang:cpp decode:true">digitalWrite(volets[i].relaisMontee, HIGH);
digitalWrite(volets[i].relaisDescente, LOW);
</pre>
Pour capter un changement d’état sur les interrupteurs bistables, il suffit de lire l’entrée correspondante configurée en pull-up. Si l’entrée change d’état à l’état LOW c’est qu’on vient d’appuyer sur le bouton de montée :
<pre class="lang:default decode:true">if (digitalRead(volets[i].buttonMontee) == LOW &amp;&amp; volets[i].previousSens != 1)
{
  // Demande de montée
}
</pre>
Pour la communication des Arduino avec la base Raspberry, j’utilise le protocole I2C. Chaque Arduino à son adresse 0x11 pour le 1<sup>er</sup>, 0x12 pour le 2<sup>ème</sup>, etc.. en esclave sur le bus. Le Raspberry est maître.

Lors du setup, j’attribue l’adresse de mon Arduino et configure des callbacks pour la réception et l’émission :
<pre class="lang:cpp decode:true">Wire.begin(SLAVE_ADDRESS);
Wire.onReceive(receiveData);
Wire.onRequest(sendData);   
</pre>
Pour la lecture, dès qu’il y a quelque chose sur le bus, je récupère un Int dont les deux 1<sup>ers</sup> bits m’indiquent le n° du volet, suivi d’un bit pour savoir si je dois allumer ou eteindre le volet. En cas d’allumage, le dernier bit permet de connaitre le sens (montée ou descente) :
<pre class="lang:cpp decode:true">while(Wire.available()) 
{
    int dataReceived = 0;
    dataReceived = Wire.read();
    rawVal += dataReceived;
}
int rawInt = rawVal.toInt();
int numVolet = bitRead(rawInt,2) &amp; bitRead(rawInt,3)*2;
int allumageVolet = bitRead(rawInt,1);
int sensVolet = bitRead(rawInt,0);
// Envoyer l'ordre sur les sorties du volet correspondant
</pre>
Pour connaitre l’état des interrupteurs depuis le Raspberry, chaque Arduino écrit sur le bus un Int avec le même format pour indiquer pour chaque volet son état :
<pre class="lang:cpp decode:true">Wire.write(/* int représentant l’état du volet */);</pre>
Maintenant que je peux piloter l’ensemble de mes volets depuis mon Raspberry, j’utilise la plateforme Constellation pour tout connecter facilement.

Grace au SDK Constellation dans Visual Studio et aux Python Tools for Visual Studio, j’ai créé un package Constellation en Python.

Le package expose des méthodes dans la Constellation pour le contrôle et publie des « StateObjects » sur l’état de chaque volet.
<pre class="lang:python decode:true">def ActionVoletMessageCallback(data):
    # Action à envoyer à un volet.
    try:
        volet = GetShutter(data["shutterName"].lower())
        actionStr = data["actionShutter"].lower()
	 if actionStr == "montee":
            volet.Open()
        elif actionStr == "descente":
            volet.Close()
        else:
            volet.Stop()
    Constellation.WriteInfo("ActionVolet: %s =&gt; %s ", (volet.nom, actionStr))
</pre>
Les methodes Open/Close/Stop écrivent sur le bus I2C avec la commande I2CSet à destination de l’adresse de l’arduino cible, l’entier représentant l’ordre à exécuter :
<pre class="lang:python decode:true">commande = 'i2cset -y 1 %s 0x%x' % (self.adresse, i)
os.system(commande)
</pre>
Pour la lecture sur le bus, j’utilise la commande « i2cget ».
<pre class="lang:python decode:true">f = os.popen('i2cget -y 1 %s' % self.adresse, 'r')
retour = f.read()
</pre>
Une fois l’entier représentant l’état de chaque volet décrypté, je publie dans la Constellation un StateObject avec toutes les infos :
<pre class="lang:python decode:true"># Changement, on met à jour le stateobject
 Constellation.PushStateObject(volet.nom, volet.etat, "Shutter",
   {
       "Adresse": volet.adresse,
       "Numero": volet.numero,
       "Statut inversé" : volet.inverseStatut,
       "Commande inversée" : volet.inverseCommande
   })
</pre>
Une fois mon package développé, je peux directement depuis Visual Studio le publier sur mon serveur et le déployer sur une sentinelle de ma Constellation.

Au préalable j’ai installé une Sentinelle Constellation sur mon Raspberry. Il me suffit ensuite de lancer mon package depuis l’interface Web de contrôle de ma Constellation.

Cette interface me permet également de suivre en temps réel les logs de mes packages :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-70.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Console Constellation 1.7" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-70.png" alt="Console Constellation 1.7" width="354" height="100" border="0" /></a></p>

<h3>Aller plus loin avec Constellation</h3>
A ce stade l’état de chaque volet de ma maison est publié dans la Constellation et je suis capable de piloter chacun d’entre eux en envoyer un message dans la Constellation à destination de mon package Python.

La suite logique est de créer une interface de pilotage. Pour cela je vais utiliser l’API Constellation pour AngularJS me permettant de créer une application HTML/JS connectée dans la Constellation avec le framework Angular de Google.

Grace à la Constellation, le code Javascript se résume à cela :
<pre class="lang:javascript decode:true">app.controller('ShutterAngularController', ['$scope', 'constellation', function ($scope, constellation) {

    constellation.intializeClient("http://constellation.myhome.lan:8088/", "MonAcessKey", "HomeControl");

    var MesVolets = {};
    constellation.onUpdateStateObject(function (message) {
        $scope.$apply(function () {
            $scope.MesVolets[message.Name] = message;
        });
    });

    constellation.onConnectionStateChanged(function (change) {
        if (change.newState === $.signalR.connectionState.connected) {
            constellation.requestSubscribeStateObjects("*", "ShutterController", "*", "*");
        }
    });

    $scope.actionShutter = function (shutter, action) {
        constellation.sendMessage({ Scope: 'Package', Args: ['ShutterController'] }, 'ActionVoletMessageCallback', { 'shutterName': shutter.Name, 'actionShutter': action });
    };

    constellation.connect();
}]);
</pre>
Dans mon contrôleur, je me connecte à ma Constellation et je m’abonne au StateObject du package « ShutterController ». A chaque réception d’un state object, je l’ajoute dans mon scope Angular. De plus j’ai ajouté une méthode pour envoyer un message de contrôle à mon package Python.

Il ne reste plus qu’à faire une vue HTML en faisant une boucle sur notre variable de scope « MesVolets » :
<pre class="lang:html5 decode:true">&lt;div class="list-group-item" ng-repeat="shutter in ShutterController"&gt;
    &lt;h3&gt;                        
        {{shutter.Name}}
        &lt;em class="pull-right btn-group" role="group" aria-label="..."&gt;
            &lt;button type="button" 
                class="btn btn-default glyphicon glyphicon-chevron-up" 
                ng-class="{ active: shutter.Value==='Montee' }" 
                ng-click='actionShutter(shutter, "montee")'&gt;
                &lt;/button&gt;
            &lt;button type="button" 
                class="btn btn-default glyphicon glyphicon-stop" 
                ng-class="{ active: shutter.Value==='Arret' }" 
                ng-click='actionShutter(shutter, "stop")'&gt;
                &lt;/button&gt;
            &lt;button type="button" 
                class="btn btn-default glyphicon glyphicon-chevron-down" 
                ng-class="{ active: shutter.Value==='Descente' }" 
                ng-click='actionShutter(shutter, "descente")'&gt;
                &lt;/button&gt;
        &lt;/em&gt;
    &lt;/h3&gt;
&lt;/div&gt;</pre>
Et voilà comment créer une première interface Web de supervision et de contrôle de l’ensemble des volets à la maison.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-71.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Interface de contrôle des volets" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-71.png" alt="Interface de contrôle des volets" width="404" height="387" border="0" /></a></p>
Les propriétés des volets étant exposées dans la Constellation, il devient facile d’ajouter de l’intelligence dans la maison. Par exemple, j’ai également un autre Raspberry avec un capteur de luminosité TSL2561 dans ma Constellation ce qui me permet d’ouvrir et fermer les volets en fonction du seuil de luminosité ou encore de fermer automatiquement les volets lorsque je lance un film sur mon XBMC (XBMC/Kodi étant lui-même connecté dans ma Constellation).

Grace à la plateforme Constellation, l’interconnexion des objets devient un jeu d’enfant comme l’orchestration et le déploiement de mes programmes à travers les différents devices de la maison.

Ainsi, moyennant un budget inférieur à 100€, j’ai pu connecter et rendre intelligents mes dix volets.

<b><i>Lucas Dupuis</i></b>