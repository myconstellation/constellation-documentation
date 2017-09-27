---
ID: 5474
post_title: 'S-Fit : Concevez un miroir connect&eacute; orient&eacute; fitness'
author: Sebastien Warin
post_date: 2017-09-27 12:05:42
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/s-fit-concevez-un-miroir-connecte-oriente-fitness/
published: true
post_modified: 2017-09-27 12:05:42
---
<i>C’est l’été, la saison des maillots de bains, il est grand temps de se prendre en main et de se sculpter un corps de rêve. Pourquoi ne pas utiliser une des nombreuses solutions de tracker d’activités présentes sur le marché ? Ce n’est pas assez drôle pour des makers, nous avons donc décidé de créer notre propre solution fitness axée autour d’un miroir connecté !</i>
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig1.png"><img title="Résultat final du miroir connecté à Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig1_thumb.png" alt="Résultat final du miroir connecté à Constellation" width="300" height="381" border="0" /></a></p>

<h2>Introduction</h2>
Nous sommes 5 étudiants en troisième année de Cycle Informatique et Réseaux à l’ISEN et nous avons conçu un nouveau concept de solution fitness basée sur un miroir. Pour réaliser notre projet, nous avions un budget de 0€ mais nous avions surtout une grande motivation pour créer un produit innovant et agréable à utiliser.

C’est pour cela que nous avons utilisé des produits de récupérations. En effet, nous avons tous dans notre garage un ordinateur portable que nous n’utilisons plus, une ancienne webcam, et quelques planches de contreplaqué. Concernant l’aspect miroir, nous avons utilisé du film sans tain car nous en avions déjà, cependant, pour une dizaine d’euros de plus, vous pourrez utiliser une vitre sans tain. Cette dernière donnera un rendu bien meilleur à votre miroir. Voilà qui devrait suffire pour la partie matérielle de notre projet.

Pour la partie logicielle, nous avons utilisé la plateforme Constellation. Les lecteurs réguliers de ce magazine la connaissent déjà, pour les autres, il s’agit d’une plateforme technique d’orchestration et d’interconnexion des objets, des services et des applications. Elle s’appuie sur des paquets qui peuvent publier et consommer des messages ainsi que sur des fonctions partagées. Concrètement, avec Constellation, en quelques lignes, il devient très simple de connecter des objets (ou applications) entre eux. Ces derniers vont donc dialoguer via Constellation comme le feraient des micro-services. L’avantage d’utiliser cette plateforme pour un tel projet c’est la facilité avec laquelle nous avons pu connecter et déployer les différentes briques de notre miroir. Pour en savoir plus sur cette technologie, vous pouvez vous rendre sur <a href="http://www.myconstellation.io/">http://www.myconstellation.io/</a>

Pour résumer, en raison d’un coût très faible et d’un développement simplifié, S-Fit est le projet parfait pour vous occuper cet été.
<h2>Fonctionnement général</h2>
Nous avons tout d’abord pensé S-Fit comme une application dotée d’un podomètre. Cette dernière synchronise les différents profils des membres de la famille en temps réel grâce à Constellation. Vous pouvez ainsi y gérer vos propres objectifs et surveiller votre progression. Vous ne perdrez pas votre motivation grâce à notre système de trophées qui vous donnera envie de repousser vos limites chaque jour. Comme il n’existe pas de meilleure motivation que la compétition, vous pourrez vous comparer à vos proches grâces à des outils d’analyse s’appuyant sur une série de graphiques.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Les sources d’informations du miroir" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig7_thumb.png" alt="Les sources d’informations du miroir" width="240" height="240" border="0" /></a></p>
Pour vous rappeler vos objectifs chaque matin, nous avons ajouté à S-Fit un miroir compagnon. Ce dernier a demandé beaucoup de réflexion car il s’agit d’un nouvel objet avec lequel il faut interagir de manière naturelle. De plus, il fallait faire de ce dernier un bel objet que l’on puisse retrouver chez soi. Nous avons donc fait le choix d’une interface minimaliste qui affiche seulement les informations pertinentes : la météo, les évènements à venir et votre progression. De ce fait, pas besoin de toucher le miroir et d’y laisser des traces de doigts.

Pour gérer les multiples profils, nous avons également intégré un module de reconnaissance faciale qui permettra au miroir d’afficher des informations personnalisées en fonction de son utilisateur.

Comme vous vous en doutez surement, le lien entre l’application et le miroir se fait par l'intermédiaire de la plateforme Constellation. Tout est synchronisé en temps réel et cela fonctionne comme par magie.
<h3>Etape 1 : Conception du boîtier</h3>
Pour commencer, il faut démonter votre vieil ordinateur, afin d’en récupérer la dalle LCD. On utilise ensuite la référence de cette dernière pour pouvoir se procurer le contrôleur adapté.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig2.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="La dalle récupérée et son contrôleur" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig2_thumb.jpg" alt="La dalle récupérée et son contrôleur" width="236" height="244" border="0" /></a></p>
Ensuite, il faut concevoir un boîtier capable d’accueillir l’ensemble de votre appareil. Son épaisseur et ses dimensions dépendent donc de votre miroir. Nous ne fournirons donc pas de plans pour rendre votre création unique.

Attention toutefois à prévoir des espaces pour l’aération, l’alimentation et les contrôles de la dalle.

C’est la partie la plus personnelle du projet, c’est le moment de libérer votre créativité pour mettre en place votre vision de S-Fit.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig3.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Assemblage du boîtier et vernissage" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig3_thumb.jpg" alt="Assemblage du boîtier et vernissage" width="244" height="125" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig4.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Assemblage du boîtier et vernissage" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig4_thumb.jpg" alt="Assemblage du boîtier et vernissage" width="244" height="127" border="0" /></a></p>
Nous avons également prévu une trappe d’accès à l’arrière pour pouvoir modifier notre miroir plus tard.

Si vous avez fait le choix du film sans tain, il va falloir le poser. Pour cela, voici les quelques étapes à suivre :
<ul>
 	<li>Nettoyer votre dalle à l’aide d’un chiffon doux</li>
 	<li>Appliquer un peu d’eau savonneuse sur celle-ci</li>
 	<li>Poser le film sans tain petit à petit en vous aidant d’un grattoir. Attention à ne pas rayer le film avec, c’est très fragile !</li>
 	<li>Chassez, toujours avec ce grattoir, les dernières bulles d’air</li>
</ul>
Prenez bien votre temps lors de la pose, c’est une partie très délicate et elle affectera directement l’esthétique de votre miroir.

Vous avez maintenant l’ensemble des pièces qui vont constituer votre miroir. Pour terminer, il ne vous reste plus qu’à tout assembler en faisant attention à bien aligner la dalle et le boîtier.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig5.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Le miroir assemblé, prêt à être refermé" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig5_thumb.jpg" alt="Le miroir assemblé, prêt à être refermé" width="354" height="266" border="0" /></a></p>
Si vous souhaitez intégrer un module de reconnaissance faciale, il va falloir ajouter une webcam (USB).

Pour cela, il faut la démonter, récupérer le circuit et le fixer dans le boîtier.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig07.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Miroir assemblé sans la webcam" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig07_thumb.jpg" alt="Miroir assemblé sans la webcam" width="304" height="404" border="0" /></a></p>

<h3>Etape 2 : Le développement logiciel</h3>
<h4>Etape 2.1 : L’interface du miroir</h4>
Pour réaliser le miroir, nous avons choisi de concevoir une application web avec AngularJS. En effet, comme le dit le créateur de Constellation, Sébastien Warin, on peut connecter n’importe quoi avec quelques lignes de code qui vont bien.

Tout d’abord, il est important de rappeler que pour continuer ce tutoriel, il est nécessaire d’avoir une Constellation déployée chez soi. Vous trouverez la plateforme ainsi que les tutoriels de prise en main sur le portail <a href="https://developer.myconstellation.io/">https://developer.myconstellation.io/</a>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Le rôle de Constellation dans la plateforme S-Fit" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig8_thumb.png" alt="Le rôle de Constellation dans la plateforme S-Fit" width="300" height="300" border="0" /></a></p>
Nous allons donc pouvoir connecter notre application Angular à Constellation :
<pre class="lang:js decode:true">var app = angular.module('Mirror', ['ngConstellation']);

app.controller('MyController', ['$scope', 'constellationConsumer', ($scope, constellation) =&gt; {

    constellation.initializeClient("maconstellation.local", "masupercle123", "MyMirror");

    constellation.connect();

}]);
</pre>
Il ne reste plus qu’à s’abonner aux StateObjects de Constellation que l’on veut voir sur le miroir. Par exemple, ici, nous allons récupérer la météo dans la ville de Lille :
<pre class="lang:js decode:true">constellation.registerStateObjectLink("*", "ForecastIO", "Lille", "*", (so) =&gt; {
    $scope.$apply(() =&gt; {
        $scope.temperature = so.Value.currently.temperature;
    });
});
</pre>
Pour en savoir plus, vous pouvez vous rendre sur le portail dont le lien se trouve plus haut pour y trouver la documentation complète. Vous trouverez d’ailleurs un tutoriel détaillé sur l’utilisation de Constellation en JavaScript. Mais rassurez vous, ce n’est pas plus compliqué que cela. Il ne manque que quelques lignes d’HTML et de CSS pour donner vie à votre miroir.

Si on continue l’exemple de la météo, le code HTML associé pourrait-être le suivant :
<pre class="lang:html5 decode:true">&lt;p&gt;{{temperature}}&lt;/p&gt;</pre>
On obtiendrait alors une page sur laquelle la température va s’afficher dynamiquement.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="L’interface finale du miroir: le choix du noir et blanc permet un meilleur rendu sur le miroir" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig6_thumb.png" alt="L’interface finale du miroir: le choix du noir et blanc permet un meilleur rendu sur le miroir" width="450" height="336" border="0" /></a></p>

<h4>Etape 2.2 : La reconnaissance faciale</h4>
Comme nous l’expliquions plus haut, nous avons ajouté un module de reconnaissance faciale pour gérer plusieurs profils. Il s’agit d’une partie facultative et plutôt complexe.

Pour cela, nous avons utilisé une <a href="https://www.codeproject.com/Articles/261550/EMGU-Multiple-Face-Recognition-using-PCA-and-Paral">application open source existante</a> qui s’appuie sur EMGU.CV. C’est un portage en C# d’OpenCV. Malheureusement, cette application était trop ancienne et nous n’avons pas réussi à la connecter directement à Constellation. Pour résoudre ce problème, nous avons conçu un paquet Constellation qui permet de publier et de s’abonner aux StateObjects par l’intermédiaire de sockets TCP.

Lors de l’ajout d’un nouvel utilisateur S-Fit, le paquet lance la séquence d’enregistrement d’un visage automatiquement.

Nous avons effectué plusieurs essais sur la quantité d’images à enregistrer, afin d’obtenir l’équilibre idéal entre une reconnaissance optimale et un minimum d’espace utilisé. Pour vous reconnaître, l’algorithme s’appuie sur plusieurs caractéristiques faciales, comme la forme du nez, de la bouche, des yeux, de vos sourcils…

De par le peu d’espace pris par la reconnaissance, vous pouvez aisément enregistrer toute votre famille, afin que le miroir devienne un élément à part entière de votre lieu de vie, et que tout le monde participe à la compétition !

La reconnaissance faciale se déroule en deux étapes. La première consiste à détecter un visage. Pour cela nous avons utilisé le classificateur Haar car il nous fallait une reconnaissance faciale en temps réel. Le classificateur Haar est, en fait, un fichier xml contenant une quantité énorme de photos dites négatives et positives. Les photos positives contiennent un visage, tandis que les photos négatives n’en contiennent pas. Cela permet donc de savoir si un visage est présent sur une photo ne faisant pas partie du classificateur. À noter qu’il est possible de créer soi-même son propre classificateur ou même d’en améliorer un.

Ici, nous nous servons donc de ce dernier afin de vérifier si, sur la frame actuelle, un visage est présent ou non de la sorte :
<pre class="lang:csharp decode:true">gray_frame = currentFrame.Convert&lt;Gray, Byte&gt;();

MCvAvgComp[][] facesDetected = gray_frame.DetectHaarCascade(Face, 1.2, 10, Emgu.CV.CvEnum.HAAR_DETECTION_TYPE.DO_CANNY_PRUNING, new Size(50, 50));
</pre>
Ceci va donc insérer dans le tableau facesDetected[0] tous les visages ayant eu un résultat positif après comparaison avec le classificateur Haar.

Dans un second temps, on cherche à reconnaître une personne à l’aide de son visage, l’application dispose donc d’une architecture très simple divisée en trois classes C#. La classe principale va capturer le flux vidéo de la webcam puis par la suite analyser chaque image via la classe de reconnaissance de personne qui, elle, aura au préalable chargé les données des personnes déjà enregistrées. La dernière classe sert, quant à elle, à enregistrer une personne en prenant une centaine de photos du visage de celle-ci.

Pour communiquer avec notre package, nous avons également ajouté une classe TCPClient.cs qui se connecte et échange les StateObjects via le réseau.

C’est d’ailleurs les paquets reçus qui vont démarrer les fonctions d’ajout d’utilisateur.

Pour rendre la gestion des utilisateurs agréable, nous avons intégré cette reconnaissance faciale de manière totalement transparente. Lorsqu’un utilisateur s’enregistre dans l’application il doit être face au miroir. Pour vérifier cela, l’utilisateur sera invité à saisir un code à six chiffres qui s’affichera quelques secondes sur le miroir. Une fois l’ensemble des informations saisies dans l’application, le miroir va automatiquement lancer une séquence de capture de 100 clichés du nouvel utilisateur.

Pour la reconnaissance des utilisateurs enregistrés, le paquet de reconnaissance faciale va capturer une image chaque seconde pour vérifier la présence ou non d’un individu connu.

Lorsque deux utilisateurs enregistrés sont face au miroir, ce dernier va se concentrer sur celui qu’il identifie le mieux.
<h4>Etape 2.3 : L’application mobile</h4>
L’application S-Fit a été conçue avec les frameworks Ionic 3 et Apache Cordova. Ces frameworks permettent d’obtenir une application Web à l'intérieur d’une application native Android ou iOS qui embarque un serveur NodeJS sur le mobile. Comme nous l’avons vu plus haut, l’application qu’affiche le miroir est une page web, l’application mobile utilise donc les mêmes technologies.

Ainsi, la connexion s’effectuera tout aussi simplement :
<pre class="lang:javascript decode:true">var constellation = $.signalR.createConstellationConsumer("maconstellation.local", "masupercle123", "MonApp");

constellation.connection.start();
</pre>
Une application comme la nôtre peut alors consommer des StateObjects mais également envoyer des MessageCallbacks. C’est à dire exécuter des fonctions directement sur la Constellation. C’est particulièrement utile pour incrémenter le compteur de pas.

Nous avons découpé notre application en trois grandes parties : la gestion des profils, les activités et les trophées.
<h5>Gestion des profils</h5>
Comme S-Fit est pensé pour plusieurs utilisateurs, nous avons géré les profils dans notre application.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig10-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Choix d’utilisateur, nouvel utilisateur et informations" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig10_thumb-1.png" alt="Choix d’utilisateur, nouvel utilisateur et informations" width="354" height="202" border="0" /></a></p>
Lorsqu’un utilisateur lance l’application, il peut choisir son profil. Cette action va établir la connexion avec la Constellation pour récupérer les informations personnelles et l’historique d’activités. Toutefois, s’il n’a pas de profil, il peut en ajouter un s’il est en face du miroir, qui lui affichera alors un code de vérification. Lors du lancement de l’application, cette dernière synchronise instantanément les nouvelles données d’activités avec le serveur Constellation.

L’utilisation d’S-Fit est totalement transparente et ne demande pas de manipulation particulière de l’utilisateur. En effet, nous avons cherché à fournir un produit simple, accessible et entièrement automatisé. Cette synchronisation est permise par Constellation.
<h5>Les activités</h5>
Cette partie se compose d’un podomètre, d’un récapitulatif de la journée en cours et d’un historique sous forme de graphiques.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig11-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Récapitulatif de la journée en temps réel et statistiques" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig11_thumb-1.png" alt="Récapitulatif de la journée en temps réel et statistiques" width="304" height="262" border="0" /></a></p>
Pour le podomètre, nous nous sommes appuyés sur un plugin de Cordova permettant d’accéder aux données de l'accéléromètre du mobile. A l’aide des données fournies par ce plugin, nous avons pu étudier les variations sur les axes x, y et z, dans l’optique de compter les pas.

L’application récupère donc les données accélérométriques de votre mobile toutes les 0.120 secondes. C’est une valeur que nous avons retenue après plusieurs jours de tests pour affiner la précision du compteur de pas, puis, en s’appuyant sur les précédentes valeurs, on va déterminer si le mouvement effectué est un pas ou non, et donc informer Constellation si elle doit incrémenter le nombre de pas de l’utilisateur.

Le développement de cette application ne se résume pas qu’à de la programmation informatique. Nous avons également réalisé des mesures sur plusieurs dizaines de personnes afin d’obtenir un lien entre la morphologie et la longueur des pas
<pre class="lang:javascript decode:true">var slope = 0.64878048 ;
var origin = 44.6744 ;

function getDistance(size, stepCounter) {
    return (size * slope - origin) * stepCounter;
}
</pre>
Avec les informations physiologiques et l’activité de l’utilisateur, on peut donc créer un ensemble de fonctions qui permettent d’étudier son état de santé, en calculant par exemple les calories dépensées chaque jour. C’est grâce à ces données qu’il est possible de créer une application fitness entièrement maîtrisée. On est alors libre d’appliquer les algorithmes souhaités sur les données récupérées.

Pour la page des graphiques sur l’application mobile, nous avons utilisé la bibliothèque Chart.js qui permet de tracer des graphiques dynamiques avec un rendu épuré. Son principal avantage est la prise en main rapide de la bibliothèque ainsi que toutes ses options.

Voici un exemple pour tracer un graphique linéaire
<pre class="lang:javascript decode:true">var myLineChart = new Chart(ctx, {
    type: 'line',
    data: data,
    options: options
});
</pre>
Cependant, il faut tout d’abord récupérer les informations présentes dans les StateObjects pour les afficher dans les graphiques. Cela nous permet de faire des graphes qui se mettent à jour en temps réel.
<h5>Les trophées</h5>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="La liste des trophées et détails d’un trophée" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig12_thumb.png" alt="La liste des trophées et détails d’un trophée" width="354" height="303" border="0" /></a></p>
Comme nous l’expliquions plus haut, nous avons intégré un système de trophées et de récompense. Il se présente tout d’abord comme une liste de trophées que l’on débloque en réalisant des succès particuliers (pas, distance).

Les trophées montrent votre expérience sur S-Fit, et représentent donc la récompense pour vos efforts. Lorsque l’on clique sur un trophée débloqué, on affiche son détail et le nombre de points qu’il a rapporté.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma récapitulatif de la synchronisation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig11_thumb.png" alt="Schéma récapitulatif de la synchronisation" width="240" height="240" border="0" /></a></p>

<h2>Conclusion</h2>
Voilà qui conclue les grandes étapes de la réalisation de S-Fit. Comme vous avez pu le voir les possibilités de personnalisation sont très nombreuses. C’est un projet ludique et facile à réaliser. C’est également un bon point de départ pour prendre en main la plateforme Constellation. Nous espérons vraiment qu’il vous a plu et que vous allez réaliser votre propre version.

Nous tenons également à remercier Julie, Adrien et tous ceux qui se sont impliqués de près ou de loin dans la réalisation de ce projet.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig9.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="L’équipe au complet" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/fig9_thumb.jpg" alt="L’équipe au complet" width="404" height="256" border="0" /></a></p>
Auteurs : Valentin BEQUART, David BRICENO-AGUILERA, Pierre-Alexandre CHOAIN, Milan FERTIN et Hugo MROCZKOWSKI.