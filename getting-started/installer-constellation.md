---
ID: 1016
post_title: Installer Constellation 1.8
author: Sebastien Warin
post_date: 2016-03-13 19:22:53
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/installer-constellation/
published: true
post_modified: 2016-12-21 10:50:22
---
Après avoir introduit Constellation, découvrons comment installer la plateforme Constellation.
<p style="text-align: center;"><iframe width="560" height="315" src="https://www.youtube.com/embed/hD8Upyb2jXo" frameborder="0" allowfullscreen></iframe></p>

<h3>Prérequis</h3>
Une plateforme Constellation se compose :
<ul>
 	<li>D’un serveur qui héberge votre Constellation (Un serveur = Une Constellation)</li>
 	<li>Des sentinelles (UI ou Service) à installer sur vos différents devices</li>
 	<li>D’une console, application Web de pilotage de la Constellation</li>
</ul>
Vous pouvez installer le serveur Constellation sur une machine Windows ou Linux, que ce soit un laptop, un desktop, un serveur local ou dans le Cloud.

Dans la rubrique consacrée au <a href="/constellation-platform/constellation-server/">Constellation Server</a> vous retrouverez plusieurs articles sur les différentes installations (un serveur Linux, sur un cloud Azure, etc…).

Pour démarrer nous allons déployer une Constellation complète (serveur, sentinelle, console et SDK) sur une machine Windows.

Vous avez donc besoin:
<ul>
 	<li>Un ordinateur sous Windows 7, 8.x ou 10</li>
 	<li>Pour installer le SDK, vous devez au préalable avoir installé Visual Studio 2012, 2013 ou 2015</li>
</ul>
Si vous ne disposez pas de licence de Visual Studio, vous pouvez installer la version  “Community”, une version gratuite de Visual Studio compatible avec le SDK Constellation : <a title="https://www.visualstudio.com/fr-fr/products/visual-studio-community-vs.aspx" href="https://www.visualstudio.com/fr-fr/products/visual-studio-community-vs.aspx">https://www.visualstudio.com/fr-fr/products/visual-studio-community-vs.aspx</a>
<h3>Télécharger la plateforme Constellation</h3>
Sur la <a href="/download/">page de téléchargement</a> vous trouverez plusieurs programmes d’installation pour chaque composant Constellation  :
<ul>
 	<li>Le serveur</li>
 	<li>Les sentinelles (UI ou Service)</li>
 	<li>La console</li>
 	<li>Le SDK Visual Studio</li>
</ul>
Pour gagner en productivité, vous avez également à votre disposition le “Constellation Web Platform Installer”, un programme d’installation qui télécharge automatiquement les composants que vous souhaitez installer.

Commencez donc par télécharger le programme  “<strong>Constellation Web Platform Installer</strong>” pour Windows :
<p align="center">[wpfilebase tag=file id=42 /]</p>

<h3>Installer la plateforme</h3>
<h4>Etape 1 : lancement de l’installation</h4>
Lancez le programme “Constellation Web Platform Installer.exe”
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Web Platform Installer" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-8.png" alt="Web Platform Installer" width="428" height="333" border="0" /></a></p>

<h4>Etape 2 : identification</h4>
Vous devez dans cette étape renseigner votre compte MyConstellation.io :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Identification" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-9.png" alt="Identification" width="428" height="333" border="0" /></a></p>

<h4>Etape 3 : acceptez la licence d’utilisation</h4>
Vous retrouverez le détail des licences Constellation <a href="/licensing/">sur cette page</a>. Pour résumer, Constellation est gratuit pour un usage personnel ou éducatif sans aucun but lucratif et soumis à l’acquisition d’une licence pour un usage professionnel ou entreprise.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Licence d'utilisation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image11_thumb.png" alt="Licence d'utilisation" width="424" height="347" border="0" /></a></p>

<h4 align="left">Etape 4 : répertoire d’installation</h4>
<p align="left">Vous devez choisir le répertoire d’installation racine de la plateforme Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image47.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Repertoire d'installation de la plateforme" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image47_thumb.png" alt="Repertoire d'installation de la plateforme" width="424" height="347" border="0" /></a></p>

<h4 align="left">Etape 5 : sélectionnez les composants Constellation à installer</h4>
<p align="left">Vous pouvez sélectionner les composants à installer ou utiliser les profiles prédéfinies :</p>

<ul>
 	<li>
<div align="left"><u>Full server installation</u> : installation typique pour un serveur (le serveur, la sentinelle en version service et la console Constellation)</div></li>
 	<li>
<div align="left"><u>Full developer installation</u> : installation typique pour un poste de développement (le serveur, la sentinelle UI, la console et le SDK Visual Studio)</div></li>
 	<li>
<div align="left"><u>Custom installation</u> : sélectionnez les composants que vous souhaitez installer</div></li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-10.png" alt="Selection des composants" width="428" height="333" border="0" /></a></p>
Notez que le programme vous indiques la dernière version de chaque composant à installer et est capable de gérer les mises à jour de chacun de ces composants.

Si Visual Studio n’est pas installé, le composant SDK sera grisé avec la mention “Not applicable”.

Dans ce guide, installons le profile “Développeur” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-11.png" alt="Selection des composants" width="428" height="333" border="0" /></a></p>

<h4 align="left">Etape 6 : configuration du serveur Constellation</h4>
<p align="left">Rentrons désormais les étapes de configuration de chacun des composants à installer, à commencer par le serveur Constellation.</p>

<h5 align="left">Etape 6.1 : sélection de la licence</h5>
<p align="left">Premièrement, vous devez sélectionner <a href="/licensing/">une licence</a> pour l’utilisation du serveur. Vous pouvez utiliser une licence que vous aurez au préalable télécharger ou directement vous connecter sur le service de licence de Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection de la licence" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-12.png" alt="Selection de la licence" width="428" height="333" border="0" /></a></p>
<p align="left">Vous obtiendrez l’ensemble des licences associées à votre compte avec la possibilité de créer des licences gratuitement pour un usage personnel (<a href="/licensing/">plus d’information ici</a>).</p>
<p align="left">Dans notre cas créons une licence personnelle :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection/Création de la licence" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-13.png" alt="Selection/Création de la licence" width="428" height="333" border="0" /></a></p>

<h5 align="left">Etape 6.2 : choix du répertoire des packages</h5>
<p align="left">Dans cette étape vous devez définir le répertoire pour votre catalogue de packages de votre Constellation.</p>
<p align="left">Par défaut, il s’agit du sous-dossier “Packages” de votre répertoire d’installation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration du Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-14.png" alt="Configuration du Package Repository" width="428" height="333" border="0" /></a></p>

<h5 align="left">Etape 6.3 : choix du port d’écoute du serveur</h5>
<p align="left">Le serveur Constellation utilise le protocole HTTP pour exposer ses différents hubs et API. Pour cela vous avez besoin de choisir le port d’écoute et de l’ouvrir au niveau de votre firewall.</p>
<p align="left">L’assistant peut déclarer le port que vous avez choisi dans le pare-feu de Windows. Si vous souhaitez ouvrir Constellation à l’extérieur de votre réseau local, à vous d’ouvrir le port sur votre routeur.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration réseau" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-15.png" alt="Configuration réseau" width="428" height="333" border="0" /></a></p>
<p align="left">A noter que vous pouvez déclarer dans la configuration du serveur Constellation sur plusieurs port HTTP et/ou HTTPS et même définir un chemin HTTP personnalisé. Il est d’ailleurs conseiller d’activer le HTTPS pour ouvrir votre serveur Constellation sur Internet. Vous retrouverez plusieurs articles sur la configuration avancée du serveur dans la rubrique consacrée au <a href="/constellation-platform/constellation-server/">Constellation Server</a>.</p>

<h5 align="left">Etape 6.4 : choix des clés d’accès</h5>
<p align="left">Pour se connecter à Constellation vous avez besoin de créer des clés d’accès (les “Access Keys”). Dans le cas d’une nouvelle installation, l’assistant vous proposera de créer deux clés :</p>

<ul>
 	<li>
<div align="left">Une clé “Standard” (accès de base) que vous utiliserez pour connecter vos sentinelles et packages</div></li>
 	<li>
<div align="left">Une clé “Administrator” qui dispose des droits d’accès au hub de contrôle (pour le pilotage de la Constellation) et à l’API de Management (pour la configuration du serveur) :</div></li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des Access Keys" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-16.png" alt="Configuration des Access Keys" width="428" height="333" border="0" /></a></p>
<p align="left">Une clé d’accès est une chaine de caractère. Il est conseillé de choisir des clés d’accès assez longues (&gt; 16 caractères) et compliquées.</p>
<p align="left">Pour simplifier leurs mémorisations et générations, Constellation propose d’utiliser un couple login/password pour créer des clés d’accès. Pour cela, on utilise le hash SHA1.</p>
<p align="left">Exemple : pour le login “Admin” et le mot de passe “Password”, la clé d’accès sera “d882b8721a224d38ebb54559e6b54e5df3a1bc6d" (soit SHA1(“AdminPassword”)). Notez bien que la casse est importante !</p>
<p align="left">Dans l’assistant d’installation, cliquez sur le bouton “Use Password” pour renseigner un couple login/password afin de générer les Access Keys :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Création des Access Keys" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-17.png" alt="Création des Access Keys" width="354" height="275" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Création des Access Keys" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-18.png" alt="Création des Access Keys" width="354" height="275" border="0" /></a></p>
<p align="left">Pour finir, vous ajoutez le droits de débogage sur la clé “Administrator” en cochant la case correspondante. Cela nous permettra de tester des packages connectés à votre Constellation depuis Visual Studio.</p>

<h4 align="left">Etape 7 : configuration de la Console Constellation</h4>
<p align="left">Comme la Console Constellation est déployée sur la même machine que le serveur Constellation, l’assistant vous propose d’auto-hoster la console par le serveur Constellation.</p>
<p align="left">Si vous désirez utiliser votre propre service Web pour exposer la Console (Apache, IIS, etc..), sélectionnez la deuxième option mais dans notre guide laissons le serveur Constellation héberger la console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Hosting de la console" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-19.png" alt="Hosting de la console" width="428" height="333" border="0" /></a></p>
<p align="left">Vous pouvez ensuite fixer une clé d’accès en dur ou utiliser une page de login. De plus vous pouvez également restreindre l’accès à la console en local seulement.</p>
<p align="left">Laissons les options par défaut (c’est à dire Console ouverte à tous avec une page de login) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-20.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration de la console" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-20.png" alt="Configuration de la console" width="428" height="333" border="0" /></a></p>

<h4 align="left">Etape 8 : configuration de la Sentinelle Constellation</h4>
<p align="left">Dans la sélection des composants nous avons indiqué vouloir installer la sentinelle UI. Vous arrivez donc à son écran de configuration.</p>
<p align="left">Comme la sentinelle est installée sur la même machine sur le serveur, vous pouvez l’inclure automatiquement dans votre Constellation ou bien l’inclure dans une autre Constellation en sélectionnant la deuxième option.</p>
<p align="left">Dans notre cas, laissons la 1er option sélectionnée pour ajouter notre sentinelle UI à notre Constellation en cours d”installation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Ajout de la sentinelle" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-21.png" alt="Ajout de la sentinelle" width="428" height="333" border="0" /></a></p>

<h4>Etape 8 : Installation</h4>
La configuration des composants est désormais terminée. Vous retrouverez tout le détail sur l’écran de confirmation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Confirmation d'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-22.png" alt="Confirmation d'installation" width="428" height="333" border="0" /></a></p>
<p align="left">En cliquant sur le bouton “Install”, le programme d’installation va télécharger les dernières versions de composants Constellation à installer :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Téléchargement des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-23.png" alt="Téléchargement des composants" width="428" height="333" border="0" /></a></p>
<p align="left">Puis lancera l’installation et la configuration de chacun de ces composants :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-24.png" alt="Installation des composants" width="428" height="333" border="0" /></a></p>
<p align="left">A la fin de l’installation, l’assistant vous proposera de lancer la Console et la Sentinelle UI (car ces deux composants ont été installés) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-25.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Fin de l'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-25.png" alt="Fin de l'installation" width="428" height="333" border="0" /></a></p>

<h4 align="left">Etape 9 : Validation de l’installation</h4>
<p align="left">En fermant le programme d’installation, la sentinelle UI va être lancée :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image73.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Démarrage de la sentinelle UI" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image73_thumb.png" alt="Démarrage de la sentinelle UI" width="424" height="140" border="0" /></a></p>
<p align="left">En double-cliquant sur l’icone vous voir les logs de cette sentinelle qui doit être correctement connectée au serveur :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image77.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinel UI" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image77_thumb.png" alt="Sentinel UI" width="424" height="207" border="0" /></a></p>
<p align="left">Vous avez également la Console qui se lance à la fin de l’installation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image81.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Page de connexion à la Console" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image81_thumb.png" alt="Page de connexion à la Console" width="424" height="332" border="0" /></a></p>
Utilisez le Login/Password de la clé d’accès “Administrator” pour vous connecter. Sur la page “Sentinels” vous devriez voir votre sentinelle UI locale connectée :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image85.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinelle connectée" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image85_thumb.png" alt="Sentinelle connectée" width="424" height="332" border="0" /></a></p>
<p align="left">Bravo vous avez correctement déployé votre Constellation !</p>

<h4>Optionnellement : ajouter un composant</h4>
Vous pouvez à tout moment relancer le “Web Platform Installer” pour ajouter ou mettre à jour les composants déjà installés.

Par exemple ajoutons la “Sentinel Service” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-26.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Ajout de composant" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-26.png" alt="Ajout de composant" width="428" height="333" border="0" /></a></p>
Vous entrez alors dans la configuration de la sentinelle avec la possibilité d’ajouter cette nouvelle sentinelle à votre Constellation locale (car précédemment installée) ou de l’ajouter à une autre Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image95.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Ajout d'une sentinelle" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image95_thumb.png" alt="Ajout d'une sentinelle" width="428" height="333" border="0" /></a></p>
<p align="left">Sélectionnons la première option pour l’ajouter à notre Constellation précédemment installée.</p>
<p align="left">Vous devez ensuite sélectionner la clé d’accès à utiliser pour cette sentinelle, par exemple la clé “Standard” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection de la clé d'accès pour la sentinelle" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-27.png" alt="Selection de la clé d'accès pour la sentinelle" width="428" height="333" border="0" /></a></p>
<p align="left">Il ne vous reste plus qu’à valider l’installation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-28.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Confirmation d'installation de la sentinelle service" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-28.png" alt="Confirmation d'installation de la sentinelle service" width="428" height="333" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-29.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Fin de l'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-29.png" alt="Fin de l'installation" width="428" height="333" border="0" /></a></p>
<p align="left">En retournant sur la Console, votre nouvelle sentinelle est bien connectée :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinelles connectées" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-21.png" alt="Sentinelles connectées" width="424" height="331" border="0" /></a></p>