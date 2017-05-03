---
ID: 4353
post_title: Installer Constellation sur Linux
author: Sebastien Warin
post_date: 2017-05-04 00:59:59
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/installer-constellation-sur-linux/
published: true
post_modified: 2017-05-04 01:19:07
---
Dans cet article nous allons découvrir comment installer Constellation sur un système Linux/Debian et ses dérivés (Raspbian, Ubuntu, etc..).

Les étapes sont conceptuellement les mêmes que pour une <a href="/getting-started/installer-constellation/">installation sur Windows</a>.
<h3>Prérequis</h3>
Le bootstrapper du <em><strong>Web Platform Installer</strong></em> pour Linux utilise l’outil APT pour installer automatiquement les dépendances de Constellation à savoir :
<ul>
 	<li>Python 2.7 et ses outils de développement (python-dev)</li>
 	<li>Mono 3.12 au minimum pour le serveur (Mono 3.10 pour la sentinelle)</li>
 	<li>Supervisor</li>
 	<li>Whiptail</li>
</ul>
Si vous êtes sur une autre distribution que Debian (ou ses dérivés) et que vous ne disposez pas de l’outil APT, vous devrez installer ces packages manuellement.

Dans cet article nous avons créé une machine virtuelle x64 et installé le système <strong>Linux Debian 8.2</strong> en “net-install” (<a href="https://www.debian.org/distrib/netinst">à télécharger ici</a>).

Notez que la procédure est exactement la même pour un Raspberry Pi (sous Raspbian). A ce sujet, une page spécifique au Raspberry Pi et Constellation est <a href="#">disponible ici</a>.

Lors de l’installation du système Debian, nous avons installé les utilitaires de base ainsi que le serveur SSH, ni plus ni moins :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation d'un serveur Linux/Debian" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-11.png" alt="Installation d'un serveur Linux/Debian" width="428" height="221" border="0" /></a></p>
<p align="left">A la fin de l’installation, nous pouvons nous connecter à cette machine Linux fraîchement installée depuis un client SSH comme Putty :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Connexion SSH" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-12.png" alt="Connexion SSH" width="428" height="273" border="0" /></a></p>
<p align="left">Pour finir, il est fortement recommandé de mettre à jour les sources APT avant de commencer l'installation de Constellation en lançant la commande suivante (en root) :</p>

<pre class="lang:bash decode:true">apt-get update
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Mise à jour des sources APT" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-13.png" alt="Mise à jour des sources APT" width="428" height="273" border="0" /></a></p>
<p align="left">N’oubliez pas, par défaut sur Debian 8, “sudo” n’est pas installé. Ainsi pour lancer des commandes en root :</p>

<ul>
 	<li>
<div align="left">soit vous vous connectez en SSH directement avec le login root</div></li>
 	<li>
<div align="left">soit vous lancez la commande “su” pour ouvrir la session root depuis votre session courante</div></li>
 	<li>
<div align="left">soit vous installez et configurez “sudo”</div></li>
</ul>
<p align="left">Voilà, notre machine Linux est prête à recevoir Constellation !</p>

<h3>Lancer le Web Platform Installer Linux</h3>
Pour télécharger et démarrer <strong><em>Web Platform Installer</em></strong>, rien de plus simple ! Lancez simplement la commande suivante :
<pre class="lang:bash decode:true">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
Le <strong><em>Web Platform Installer</em></strong> (WPI) se chargera lui même de se lancer en root en utilisant “sudo” ou “su” si la première commande n’est pas disponible.

Ainsi si le WPI est lancé depuis un autre utilisateur, vous serez amené à saisir le mot de passe du compte “root” pour l’autoriser à se lancer :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Lancement du Web Platform Installer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-14.png" alt="Lancement du Web Platform Installer" width="428" height="273" border="0" /></a></p>

<h3>Installer la plateforme</h3>
<h4>Etape 1 : installation des prérequis</h4>
Comme décrit plus haut, le WPI va installer via APT les dépendances nécessaires pour Constellation (Python-Dev, Mono et Supervisor).

Comme il s’agit d’un système fraîchement installé, nous laissons le WPI installer l’ensemble des prérequis.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation des prérequis" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-15.png" alt="Installation des prérequis" width="244" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation des prérequis" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-16.png" alt="Installation des prérequis" width="244" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation des prérequis" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-17.png" alt="Installation des prérequis" width="244" height="156" border="0" /></a></p>

<h4>Etape 2 : sélectionnez les composants Constellation à installer</h4>
Le WPI vous permet d’installer :
<ul>
 	<li>Le <strong>Serveur</strong> Constellation (avec ou sans la Console)</li>
 	<li>La <strong>Console</strong> Constellation</li>
 	<li>La <strong>Sentinel</strong> Service</li>
</ul>
Dans notre cas nous allons commencer par installer le serveur avec la console. Nous sélectionnons donc la première option :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Web Platform Installer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-18.png" alt="Web Platform Installer" width="428" height="273" border="0" /></a></p>

<h4>Etape 3 : acceptez la licence d’utilisation</h4>
Vous retrouverez le détail des licences Constellation <a href="https://developer.myconstellation.io/licensing/">sur cette page</a>. Pour résumer, Constellation est gratuit pour un usage personnel ou éducatif sans aucun but lucratif et soumis à l’acquisition d’une licence pour un usage professionnel ou entreprise.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Licence Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-19.png" alt="Licence Constellation" width="428" height="273" border="0" /></a></p>

<h4>Etape 4 : identification</h4>
Vous devez dans cette étape renseigner votre compte myConstellation.io afin de pouvoir télécharger les composants Constellation et accéder à vos licences :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-20.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Identification MyConstellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-20.png" alt="Identification MyConstellation" width="244" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Identification MyConstellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-21.png" alt="Identification MyConstellation" width="244" height="156" border="0" /></a></p>

<h4>Etape 5 : configuration du serveur Constellation</h4>
Entrons désormais dans les étapes de configuration de chacun des composants à installer en commençant par le serveur Constellation.
<h5>Etape 5.1 : répertoire d’installation</h5>
Vous devez choisir le répertoire d’installation du serveur Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Répertoire d'installation du serveur" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-22.png" alt="Répertoire d'installation du serveur" width="428" height="273" border="0" /></a></p>

<h5>Etape 5.2 : sélection de la licence</h5>
Vous devez ici sélectionner <a href="https://developer.myconstellation.io/licensing/">une licence</a> pour l’utilisation du serveur. Vous obtiendrez l’ensemble des licences associées à votre compte avec la possibilité de créer des licences gratuites pour un usage personnel (<a href="https://developer.myconstellation.io/licensing/">plus d’information ici</a>).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Selection de la licence" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-23.png" alt="Selection de la licence" width="428" height="273" border="0" /></a></p>

<h5>Etape 5.3 : choix du répertoire des packages</h5>
Dans cette étape vous devez définir le répertoire pour votre catalogue de packages de votre Constellation.

Par défaut, il s’agit du sous-dossier “Packages” de votre répertoire d’installation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Choix du répertoire des packages" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-24.png" alt="Choix du répertoire des packages" width="428" height="273" border="0" /></a></p>

<h5>Etape 5.4 : choix du port d’écoute du serveur</h5>
Le serveur Constellation utilise le protocole HTTP/s pour exposer ses différents hubs et APIs. Pour cela vous avez besoin de choisir le port d’écoute.

Par défaut, le serveur Constellation écoutera en HTTP sur le port 8088.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-25.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Choix du port d’écoute du serveur" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-25.png" alt="Choix du port d’écoute du serveur" width="428" height="273" border="0" /></a></p>
A noter que vous pouvez déclarer dans la configuration du serveur Constellation sur plusieurs ports HTTP et/ou HTTPS ou même définir un chemin HTTP personnalisé. Il est d’ailleurs conseillé d’activer le protocole HTTPS si vous souhaitez exposer votre serveur Constellation sur Internet. Vous retrouverez plusieurs articles sur la configuration avancée du serveur dans la rubrique <a href="https://developer.myconstellation.io/constellation-platform/constellation-server/">Constellation Server</a>.
<h5>Etape 5.5 : choix des clés d’accès</h5>
Pour se connecter à Constellation vous avez besoin de créer des clés d’accès (les “Access Keys”). Dans le cas d’une nouvelle installation, l’assistant vous proposera de créer deux clés :
<ul>
 	<li>Une clé “Standard” (accès de base) que vous utiliserez pour connecter vos sentinelles et packages</li>
 	<li>Une clé “Administrator” qui dispose des droits d’accès au hub de contrôle (pour le pilotage de la Constellation) et à l’API de Management (pour la configuration du serveur)</li>
</ul>
Une clé d’accès est une chaine de caractère. Il est conseillé de choisir des clés d’accès assez longues (&gt; 16 caractères) et compliquées.

Pour simplifier leurs mémorisations et générations, Constellation propose d’utiliser un couple login/password pour créer des clés d’accès. Pour cela, on utilise le hash SHA1.

Exemple : pour le login “Admin” et le mot de passe “Password”, la clé d’accès sera “d882b8721a224d38ebb54559e6b54e5df3a1bc6d” (soit SHA1(“AdminPassword”)). Notez bien que la casse est importante !

C’est pourquoi le WPI vous proposera soit de spécifier directement vos AccessKeys ou soit d’utiliser un couple login/password.

Ici sélectionnons la deuxième option :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-26.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-26.png" alt="Configuration des clés d'accès" width="428" height="273" border="0" /></a></p>
<p align="left">Pour la clé “Standard” utilisons le couple “demo/demo” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-27.png" alt="Configuration des clés d'accès" width="244" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-28.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-28.png" alt="Configuration des clés d'accès" width="244" height="156" border="0" /></a></p>
<p align="left">Et pour la clé “Administrator”, utilisons le couple “admin/password” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-29.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-29.png" alt="Configuration des clés d'accès" width="244" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-30.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-30.png" alt="Configuration des clés d'accès" width="244" height="156" border="0" /></a></p>
<p align="left">Pour finir, vous pouvez ajouter le droit de débogage sur la clé “Administrator”. Cela nous permettra de tester des packages connectés à votre Constellation depuis le SDK sous Visual Studio.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-31.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des clés d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-31.png" alt="Configuration des clés d'accès" width="428" height="273" border="0" /></a></p>

<h4>Etape 6 : installation et configuration de la Console Constellation</h4>
Une fois le serveur installé, le WPI vous proposera d’installer et configurer automatiquement la Console Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation de la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-32.png" alt="Installation de la Console" width="428" height="273" border="0" /></a></p>
Vous devez simplement spécifier le répertorie d’installation, par défaut dans /opt :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation de la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-33.png" alt="Installation de la Console" width="428" height="273" border="0" /></a></p>
Vous pouvez aussi indiquer si vous souhaitez restreindre l’accès la console à “localhost” (ce qui n’aurait pas de sens dans notre cas étant donné que notre système Linux ne dispose pas de navigateur internet localement) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-34.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Installation de la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-34.png" alt="Installation de la Console" width="428" height="273" border="0" /></a></p>

<h4>Etape 7 : Validation de l’installation</h4>
<p align="left">Et voilà, le serveur Constellation ainsi que la Console sont installés sur votre système Linux.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Fin de l'installation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-35.png" alt="Fin de l'installation" width="428" height="273" border="0" /></a></p>
Ouvrez un navigateur Internet et rendez-vous sur l’adresse IP ou DNS de votre système Linux sur le port spécifié lors de l’installation (par défaut 8088). Vous devriez atterrir sur une page du serveur Constellation vous indiquant le n° de la version du serveur, dans notre exemple 1.8.2.17118 :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Page du serveur" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-36.png" alt="Page du serveur" width="428" height="287" border="0" /></a></p>
<p align="left">Comme le serveur Constellation héberge la Console,  vous avez un lien “Open Constellation Console” sur cette page vous permettant d’accéder directement à la Console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Console Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-37.png" alt="Console Constellation" width="428" height="287" border="0" /></a></p>
<p align="left">Utilisez le couple login/password “Administrator” défini lors de l’installation pour vous connecter, dans notre exemple “admin/password” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Console Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-38.png" alt="Console Constellation" width="428" height="287" border="0" /></a></p>
<p align="left">Bravo, votre serveur Constellation est opérationnel !</p>

<h3>Next steps</h3>
<ul>
 	<li><a href="https://developer.myconstellation.io/getting-started/ajouter-des-sentinelles/">Ajoutez des sentinelles dans votre Constellation</a></li>
 	<li><a href="https://developer.myconstellation.io/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">Téléchargez et déployez des packages sur vos sentinelles</a></li>
</ul>
Prêt pour développer avec Constellation ?
<ul>
 	<li><a href="https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-csharp/">Créez votre premier package Constellation en C#</a></li>
 	<li><a href="https://developer.myconstellation.io/client-api/net-package-api/packages-ui-wpf-winform/">Créez des packages UI en Winform ou WPF</a></li>
 	<li><a href="https://developer.myconstellation.io/getting-started/connectez-vos-pages-web-constellation/">Connectez vos pages Web à Constellation</a></li>
 	<li><a href="https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-python/">Créez votre premier package Constellation en Python</a></li>
 	<li><a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">Connectez un Arduino ou un ESP8266 à Constellation</a></li>
</ul>