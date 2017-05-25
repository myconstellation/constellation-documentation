---
ID: 4642
post_title: 'Jeu de fl&eacute;chettes connect&eacute; &agrave; Constellation'
author: Sebastien Warin
post_date: 2016-11-20 19:44:59
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/jeu-de-flechettes-connecte/
published: true
post_modified: 2017-05-25 23:19:51
---
Projet réalisé par Buguel Maxime, Cartiaux Charles, Douillard Thomas et Roeland Quentin en Novembre 2016.
<h3>Introduction</h3>
Nous sommes quatre étudiants en Master 2 à l’ISEN Lille. Nous nous sommes récemment découverts une passion pour les fléchettes, surement lié à la présence d’une cible dans le bar que nous fréquentons (avec modération) après les cours.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Cible de fléchettes Phoenix-Darts" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image18_thumb.png" alt="Cible de fléchettes Phoenix-Darts" width="156" height="240" border="0" /></a></p>
La découverte de cette nouvelle passion nous a poussé à ressortir du placard les cibles avec lesquelles nous avons pu jouer étant plus jeunes. Mais le plaisir de jouer aux fléchettes n'était pas le même. C’est vrai que l'écran LCD de 2cm par 4cm et le haut parleur 8-bit n’offre pas la même ambiance que celle que nous pouvons avoir sur une borne de bistrot.

Il existe de nombreux tutoriel pour se créer une borne d’arcade chez soi pour les fans de rétro-gaming mais nous n’avons rien trouvé pour “<i>pimper</i>” notre jeu de fléchettes, mis à part un article de <a href="https://danielfett.de/en/projects/raspberry-pidart-electronic-dartboard-superpowers/">blog</a> qui sans expliquer les détails de la transformation nous a servie de “Preuve de faisabilité”.

Mais nous avons décidé d'aller un peu plus loin que de simplement améliorer l'expérience utilisateur en optimisant l’affichage ainsi que les effets sonores. Afin d'intégrer la cible comme un objet connecté à part entière, Constellation a été utilisé
<h3>Connecter la cible</h3>
Côté matériel la première étape est de faire l'acquisition d’une cible électronique (bas de gammes afin de limiter les coûts), nous avons choisis le jeu de fléchette le moins chère que l’on ai pu trouver : 19,99 euros.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Jeu de fléchette electronique" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image27_thumb.png" alt="Jeu de fléchette electronique" width="210" height="240" border="0" /></a></p>
La détection d’une fléchette par la cible se fait à l’aide de 2 nappes en plastiques séparées par une épaisseur en mousse trouée.

On remarque que le jeu est divisé en 2 moitiés (Par l’axe qui passe entre la zone 9 et 14) l’une des 2 nappes en plastique contient 7 pistes conductrices:
<ul>
 	<li>une pour toutes les zones “Simple” de la moitié de la cible</li>
 	<li>une pour toutes les zones “Simple” de l’autre moitié de la cible</li>
 	<li>une pour toutes les zones “Double” de la moitié de la cible</li>
 	<li>une pour toutes les zones “Double” de l’autre moitié de la cible</li>
 	<li>une pour toutes les zones “Triple” de la moitié de la cible</li>
 	<li>une pour toutes les zones “Triple” de l’autre moitié de la cible</li>
 	<li>une pour le “Bull” Simple et Double</li>
</ul>
L’autre nappe contient 10 pistes, 8 des pistes correspondent chacune à une paire de zone numérotée, chacune des zones appartenant à une moitié différente ((9, 14) par exemple). Les 2 pistes restantes ont, en plus de la paire de zone, une zone du “Bull”.

On se retrouve donc avec ce tableau qui permet de déterminer quelle zone a été touchée. Il suffit d’alimenter les 7 pistes “OUT” une par une et de lire l'état des 10 pistes “IN”. Si on voit que l’une des pistes IN est alimenté alors on peut en déduire la zone touchée.
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="109">OUT \ IN</td>
<td valign="top" width="56">9

14

DB</td>
<td valign="top" width="56">12

11

SB</td>
<td valign="top" width="56">5

8

-</td>
<td valign="top" width="56">20

16

-</td>
<td valign="top" width="56">10

15

-</td>
<td valign="top" width="56">2

6

-</td>
<td valign="top" width="56">17

13

-</td>
<td valign="top" width="56">3

4

-</td>
<td valign="top" width="56">19

18

-</td>
<td valign="top" width="56">7

1

-</td>
</tr>
<tr>
<td valign="top" width="109">TRIPLE</td>
<td valign="top" width="56">T9</td>
<td valign="top" width="56">T12</td>
<td valign="top" width="56">T5</td>
<td valign="top" width="56">T20</td>
<td valign="top" width="56">T10</td>
<td valign="top" width="56">T2</td>
<td valign="top" width="56">T17</td>
<td valign="top" width="56">T3</td>
<td valign="top" width="56">T19</td>
<td valign="top" width="56">T7</td>
</tr>
<tr>
<td valign="top" width="109">DOUBLE</td>
<td valign="top" width="56">D9</td>
<td valign="top" width="56">D12</td>
<td valign="top" width="56">D5</td>
<td valign="top" width="56">D20</td>
<td valign="top" width="56">D10</td>
<td valign="top" width="56">D2</td>
<td valign="top" width="56">D17</td>
<td valign="top" width="56">D3</td>
<td valign="top" width="56">D19</td>
<td valign="top" width="56">D7</td>
</tr>
<tr>
<td valign="top" width="109">SINGLE</td>
<td valign="top" width="56">S9</td>
<td valign="top" width="56">S12</td>
<td valign="top" width="56">S5</td>
<td valign="top" width="56">S20</td>
<td valign="top" width="56">S10</td>
<td valign="top" width="56">S2</td>
<td valign="top" width="56">S17</td>
<td valign="top" width="56">S3</td>
<td valign="top" width="56">S19</td>
<td valign="top" width="56">S7</td>
</tr>
<tr>
<td valign="top" width="109">BULL</td>
<td valign="top" width="56">DB</td>
<td valign="top" width="56">SB</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
<td valign="top" width="56">-</td>
</tr>
<tr>
<td valign="top" width="109">SINGLE</td>
<td valign="top" width="56">S14</td>
<td valign="top" width="56">S11</td>
<td width="56">S8</td>
<td valign="top" width="56">S16</td>
<td valign="top" width="56">S15</td>
<td valign="top" width="56">S6</td>
<td valign="top" width="56">S13</td>
<td valign="top" width="56">S4</td>
<td valign="top" width="56">S18</td>
<td valign="top" width="56">S1</td>
</tr>
<tr>
<td valign="top" width="109">DOUBLE</td>
<td valign="top" width="56">D14</td>
<td valign="top" width="56">D11</td>
<td valign="top" width="56">D8</td>
<td valign="top" width="56">D16</td>
<td valign="top" width="56">D15</td>
<td valign="top" width="56">D6</td>
<td valign="top" width="56">D13</td>
<td valign="top" width="56">D4</td>
<td valign="top" width="56">D18</td>
<td valign="top" width="56">D1</td>
</tr>
<tr>
<td valign="top" width="109">TRIPLE</td>
<td valign="top" width="56">T14</td>
<td valign="top" width="56">T11</td>
<td valign="top" width="56">T8</td>
<td valign="top" width="56">T16</td>
<td valign="top" width="56">T15</td>
<td valign="top" width="56">T6</td>
<td valign="top" width="56">T13</td>
<td valign="top" width="56">T4</td>
<td valign="top" width="56">T18</td>
<td valign="top" width="56">T1</td>
</tr>
</tbody>
</table>
Maintenant que l’on sait comment ça marche, il ne reste plus qu'à le connecter aux GPIO du microcontrôleur de son choix.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Raspberry Pi" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image23_thumb.png" alt="Raspberry Pi" width="404" height="229" border="0" /></a></p>
Dans ce tutoriel nous avons choisi de connecter la cible à un Raspberry Pi B+ mais il est tout à fait possible de le faire avec un Arduino, un ESP8226 ou encore un autre type de Pi. La seule contrainte est qu’il faut au moins 17 GPIO, il faudra donc augmenter le nombre de GPIO à l’aide d’un <i>MCP23017-E/SP 16 I/O Expander.</i>

Il faudra aussi que soit installé sur le device une sentinelle constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-67.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma électronique pour l'acquisition depuis la cible" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-67.png" alt="Schéma électronique pour l'acquisition depuis la cible" width="402" height="338" border="0" /></a></p>
Les nappes étant en plastique on ne peut venir souder dessus, on choisit donc de venir souder les fils sur le circuit imprimé directement, ensuite on peut remonter le tout. Et pour tester on les relies au Pi à l’aide d’une plaque de test (BreadBoard).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Connexions au RPi" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image16_thumb.png" alt="Connexions au RPi" width="404" height="229" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="PCB du jeu de flechette" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image21_thumb.png" alt="PCB du jeu de flechette" width="404" height="304" border="0" /></a></p>
On peut maintenant déployer un package sur le pi pour connecter notre cible a constellation. Sur Le raspberry Pi, le plus simple est de faire un package python en utilisant la librairie GPIO.

Cette petite fonction va permettre de transformer un couple de GPIOs (entré, sortie), en un message pour constellation contenant une zone de la cible.
<pre class="lang:python decode:true crayon-selected">def dartTouch(pinin,pinout):
    outputDictionary = {}
    outputDictionary[PIN_TRIPPLE_1] = [3,1]
    outputDictionary[PIN_DOUBLE_1] = [2,1]
    outputDictionary[PIN_SINGLE_1] = [1,1]
    outputDictionary[PIN_BULL] = [0,2]
    outputDictionary[PIN_SINGLE_0] = [1,0]
    outputDictionary[PIN_DOUBLE_0] = [2,0]
    outputDictionary[PIN_TRIPPLE_0] = [3,0]

    inputDictionary = {}
    inputDictionary[PIN_9_14_B] = [9,14,50]
    inputDictionary[PIN_20_16_1] = [20,16,1]
    inputDictionary[PIN_12_11_B] = [12,11,25]
    inputDictionary[PIN_5_8_2] = [5,8,2]
    inputDictionary[PIN_6_2_3] = [6,2,3]
    inputDictionary[PIN_10_15_4] = [10,15,4]
    inputDictionary[PIN_13_17_5] = [13,17,5]
    inputDictionary[PIN_4_3_6] = [4,3,6]
    inputDictionary[PIN_18_19_7] = [18,19,7]
    inputDictionary[PIN_1_7_8] = [1,7,8]


    isButton = False
    value = inputDictionary[pinin][outputDictionary[pinout][1]]
    multiple = outputDictionary[pinout][0]
    if(multiple == 0):
        isButton = True
        multiple = 1
        if(value == 50):
            multiple = 2
            value = 25
            isButton = False
        if(value == 25):
            multiple = 1
            isButton = False
    if isButton :
       Constellation.SendMessage("DartManager", "ButtonPressed",[ value ])
    else:
       Constellation.SendMessage("DartManager", "onDartTargetTouch",[ multiple, value ])
    time.sleep(0.8)
</pre>
Il ne reste plus qu'à alimenter les pins de sorties une à une et de lire l’état des pins en entrée.
<pre class="lang:python decode:true">while True:
  for pinout in outs:
    GPIO.output(pinout,GPIO.HIGH)
    for pinin in ins:
      if(GPIO.input(pinin)):
        dartTouch(pinin,pinout)
    GPIO.output(pinout,GPIO.LOW)
</pre>
Une fois notre prototype avec la BreadBoard testé et approuvé nous avons pus réaliser un deuxième prototype sur lequel nous avons juste à venir brancher les pins de notre pi et de notre cible.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image08.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="BreadBoard d'interface avec le RPi" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image08_thumb.png" alt="BreadBoard d'interface avec le RPi" width="304" height="279" border="0" /></a></p>
Ce prototype moins encombrant, apporte également plus de robustesse à notre jeu dans l’optique d’une utilisation plus fréquente.
<h3>Constellation</h3>
Afin de pouvoir envoyer l’information qu’une fléchette a touché la cible, de pouvoir traiter l’information pour par exemple savoir si l’utilisateur en a déjà lancé trois et ainsi que cette dernière ne compte pas, puis afficher cette information sur les différentes interfaces graphiques, nous devions créer un bus de données.

Pour cela, nous avons utilisé le principe des “StateObject” et ”MessageCallBack” de Constellation afin de communiquer entre chacune des différentes parties.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Architecture avec Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image22_thumb.png" alt="Architecture avec Constellation" width="440" height="209" border="0" /></a></p>
Deux packages Constellation ont donc été créé, l’un en Python, pour la détection des fléchettes et l’autre en C# pour la gestion de parties.

Ainsi lorsque qu’une fléchette touche la cible, le package en Python va envoyer cette information au package C# en appelant la méthode “onDartTargetTouch”,
<pre class="lang:python decode:true">Constellation.SendMessage("DartManager", "onDartTargetTouch",[ multiple, value ])</pre>
qui va, après avoir effectué les modifications engendrées par ce coup sur la partie, mettre à jour le StateObject ‘’DartGame’’ afin que, l’UI qui s’y est abonné, soit informé.
<pre class="lang:csharp decode:true">[MessageCallback]
void onDartTargetTouch(short multiple,short value)
{
   PackageHost.WriteInfo(multiple + " "+ value);
   if(game.applyShoot(new Shoot(multiple, value), this)){}
   update();
}

private void update()
{
   PackageHost.PushStateObject&lt;AbstractDartGame&gt;("DartGame", game);
}
</pre>
Le StateObject “DartGame” représentant la partie actuelle (joueurs, tours, tirs, etc..) sous forme d’un objet JSON.

Le but étant, une fois les paquets installés sur chacune des machines respectives (“sentinelle” pour Constellation), de pouvoir tout contrôler depuis l’UI. Des MessageCallBack sont donc mis à la disposition de cette dernière, représentant ainsi l’ensemble des fonctions nécessaires à la gestion d’une partie.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image01.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MessageCallbacks du DartManager" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image01_thumb.png" alt="MessageCallbacks du DartManager" width="454" height="186" border="0" /></a></p>

<h3>User Experience</h3>
Le gros point noir de notre cible, comme nous l’avons décrit en introduction, était l’interaction avec les utilisateurs et comme vous pouvez vous en douter après les modifications que nous avons apportés à notre cible l’afficheur de score d’origine n’est plus fonctionnel. Pour le remplacer nous avons donc réalisé un Dashboard sous la forme d’une application web.
<h4>Le Dashboard</h4>
Le Dashboard comme nous l’avons évoqué ci-dessus est une application qui nous permet d’afficher les scores provenant de notre Constellation simplement en s’abonnant au StateObject du manager de jeu. L’interface de ce Dashboard a été réalisé avec AngularJS et Angular Material pour le design. En effet en nous abonnant à un StateObject via Constellation, le Dashboard est notifié de chaque modification apportées à la partie de fléchette en cours et de modifier l’affichage dynamiquement grâce à l’objet JSON fournis par Constellation tout cela en simplement quelques lignes de codes :
<pre class="lang:javascript decode:true">constellation.registerStateObjectLink("*", "*", "DartGame", "*", function (so) {
    $scope.$apply(function() {
       $scope.constelationGame = so.Value;
    });           
});</pre>
La connection du Dahboard à Constellation permet également d’exposer des méthodes afin de déclencher des effets sonores sur certains lancers afin de dynamiser la partie.
<pre class="lang:javascript decode:true">constellation.subscribeMessages("DartsDashboard");
constellation.registerMessageCallback("Triple", function (msg) {
    var audio = new Audio('sounds/Triple.wav');
    audio.play();
});
                
constellation.registerMessageCallback("Double", function (msg) {
    var audio = new Audio('sounds/Double.wav');
    audio.play();
});
</pre>
Pour le design, Angular Material nous a permis de développer rapidement et simplement une interface esthétique.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image26.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Interface du Dahboard" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image26_thumb.png" alt="Interface du Dahboard" width="450" height="411" border="0" /></a></p>

<h4>L’application mobile</h4>
Toujours dans le but d’améliorer les fonctionnalités offertes par notre jeu customisé, une application mobile accompagne notre nouvel objet connecté pour y ajouter une dimension sociale ! Baptisé My Dart Compagnon, elle se destine aux joueurs les plus passionnés.

L’application est réalisée avec Ionic, un Framework open source qui permet de développer des applications mobiles hybrides rapidement et facilement. Il s’appuie sur <a href="https://angularjs.org/">AngularJS</a> pour la partie application web du framework et sur <a href="http://cordova.apache.org/">Cordova</a> pour la partie construction des applications natives. On peut donc très facilement se connecter à Constellation via le SDK JavaScript. Ionic framework permet de développer une seule application et de la déployer sur tous les supports, web ou mobile, que ce soit Android, iOS ou bien Windows Mobile.

De plus, l’application intègre les services Firebase, la plateforme d’outils mobile de Google qui permet d’implémenter un espace de stockage synchronisé et personnel pour l’utilisateur en toute simplicité, en mode cloud. Cet espace peut être accessible par d’autres clients, comme par exemple le Dashboard.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Application mobile" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image17_thumb.png" alt="Application mobile" width="454" height="163" border="0" /></a></p>
En utilisant l’authentification Facebook, l’application permet de voir ses amis qui l’utilisent également et de les inviter à jouer. Au début de chaque partie, le joueur peut charger son profil sur le Dashboard présenté ci-dessus en flashant un QR Code. Cela lui permet de suivre son score personnel durant la partie sur son smartphone, de consulter l’historique de ses parties disputées et ses statistiques personnelles. Les joueurs les plus compétiteurs chercheront eux à débloquer l’intégralité des succès : des récompenses débloquées lors de la réalisation de certaines actions ou performances.