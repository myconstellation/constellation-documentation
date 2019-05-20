---
ID: 1727
post_title: >
  Ajouter des sentinelles dans votre
  Constellation
author: Sebastien Warin
post_date: 2016-03-30 16:26:53
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/ajouter-des-sentinelles/
published: true
publish_post_category:
  - "20"
publish_to_discourse:
  - "0"
update_discourse_topic:
  - "0"
discourse_post_id:
  - "1417"
discourse_topic_id:
  - "935"
discourse_permalink:
  - >
    https://forum.myconstellation.io/t/ajouter-des-sentinelles-dans-votre-constellation/935
wpdc_xmlrpc_failure_sent:
  - "1"
post_modified: 2019-05-20 17:11:07
---
Les sentinelles sont des agents exécutés sur des systèmes Windows ou Linux qui, connectés à votre Constellation, permettent de déployer des packages Constellation.

En suivant <a href="/getting-started/installer-constellation/">le guide d’installation</a> vous avez pu installer une sentinelle Service ou UI sur la même machine que le serveur Constellation.

Voyons maintenant comment installer des sentinelles sur vos autres machines (laptops, desktop, serveurs, Raspberry, etc..).
<p style="text-align: center;"><iframe width="560" height="315" src="https://www.youtube.com/embed/OIPI6VYK5Jw" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>

<h3>Prérequis</h3>
Il existe deux types de sentinelle :
<ul>
 	<li>La “<strong>Sentinel Service</strong>” : il s’agit d’un service compatible Windows et Linux qui tourne en arrière plan et permet de déployer des packages Constellation sans interface graphique.</li>
 	<li>La “<strong>Sentinel UI</strong>” : il s’agit d’une application Windows s’exécutant au sein d’une session Windows et permettant de déployer des packages Constellation avec interface graphique (dit <a href="/client-api/net-package-api/packages-ui-wpf-winform/">Package UI</a>) ou encore des packages ayant besoin d’interagir avec la session de l’utilisateur.</li>
</ul>
Les prérequis pour installer une sentinelle sont :
<ul>
 	<li>Un système Windows avec le .NET Framework 4.0 installé (soit au minimum un Windows XP SP3 ou un Windows 2003 SP2)</li>
 	<li>Un système Linux avec Mono 3.10 au minimum</li>
</ul>
<u>Note</u> : la sentinelle UI n’est disponible que pour Windows.
<h3>Installation d’une sentinelle sur un système Windows</h3>
Les installeurs pour la sentinelle Service et UI sont similaires, la procédure d’installation est donc identique.

<u>Note</u> : vous pouvez installer sur la même machine une sentinelle UI et une sentinelle Service.

Pour cela, vous pouvez soit <a href="/download/">télécharger</a> le programme d’installation spécifique à la Sentinelle UI et/ou Service ou, pour faire plus simple, utiliser le “<em>Web Platform Installer</em>” pour installer et mettre à jour les dernières versions des composants Constellation.
<h4>Etape 1 : lancez le “Web Platform Installer</h4>
<p align="center">[wpfilebase tag=file id=42 /]</p>

<h4>Etape 2 : sélection des composants</h4>
Commencez par vous identifier avec votre compte Constellation et acceptez la licence d’utilisation avant de pouvoir sélectionner les composants à installer :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-30.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Selection des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-30.png" alt="Selection des composants" width="424" height="330" border="0" /></a></p>
<p align="left">Dans le cas ci-dessus, nous avons déjà installé la dernière version du SDK Constellation comme vous pouvez le constater !</p>
<p align="left">Dans ce guide nous allons installer les deux sentinelles (UI et Service) que nous connecterons à notre Constellation <a href="/getting-started/installer-constellation/">précédemment</a> installée sur une autre machine.</p>

<h4 align="left">Etape 3 : type d’installation des sentinelles</h4>
<p align="left">Vous avez ensuite le choix entre :</p>

<ol>
 	<li>
<div align="left">Installer la sentinelle <strong>et l’enregistrer</strong> dans votre Constellation en utilisant l’API de Management (il vous faudra connaitre une AccessKey qui a les droits de management pour procéder à l’enregistrement).</div></li>
 	<li>
<div align="left">Installer la sentinelle seulement</div></li>
</ol>
<p align="left">Bien entendu pour automatiser le processus, choisissons la première option :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-31.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Choix d'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-31.png" alt="Choix d'installation" width="424" height="330" border="0" /></a></p>
<p align="left">Notez que si vous souhaitez ajouter une sentinelle sur un système sur lequel un serveur Constellation est installé, l’assistant vous proposera d’installer et d’enregistrer automatiquement votre sentinelle sur le serveur local.</p>

<h4 align="left">Etape 4 : sélection du serveur Constellation à joindre</h4>
<p align="left">Vous devez indiquer l’URI de votre serveur Constellation ainsi que la clé d’accès avec les droits d’administration pour enregistrer vos sentinelles.</p>
<p align="left">Dans cet exemple le serveur Constellation est accessible sur l’URL “http://pc-seb.ajsinfo.loc:8088/” avec le couple “admin/password” (cliquez sur le bouton “Use Password” pour générer l’AccessKey à partir de ce couple) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Enregistrement de la sentinelle" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-32.png" alt="Enregistrement de la sentinelle" width="424" height="330" border="0" /></a></p>

<h4 align="left">Etape 5 : choix de la clé d’accès pour la sentinelle</h4>
<p align="left">Vous aurez ensuite à choisir parmi les clé d’accès configurées sur votre serveur, laquelle doit être utilisée pour la connexion de vos sentinelles :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Choix de la clé d'accès pour la sentinelle" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-33.png" alt="Choix de la clé d'accès pour la sentinelle" width="424" height="332" border="0" /></a></p>

<h4 align="left">Etape 6 : installation</h4>
<p align="left">Il ne vous reste plus qu’à confirmer l’installation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-34.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Confirmation de l'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-34.png" alt="Confirmation de l'installation" width="424" height="330" border="0" /></a></p>
<p align="left">L’installeur téléchargera et installera les composants puis réalisera l’enregistrement vos deux nouvelles sentinelles sur votre serveur Constellation avant de lancer le service (pour la Sentinel Service) ou l’application (pour la Sentinel UI).</p>
<p align="left">Depuis la Console Constellation, nous pouvons observer nos deux nouvelles sentinelles (PO-SWARIN et PO-SWARIN_UI) fraichement connectées :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-46.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinelles connectées" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-46.png" alt="Sentinelles connectées" width="428" height="140" border="0" /></a></p>

<h3>Installation d’une sentinelle sur un système Linux</h3>
Sur un système Linux utilisez le “Web Platform Installer” pour automatiser le processus d’installation et de configuration.

Le WPI se chargera d’installer tous les prérequis (Mono, Python, PIP, etc..) et les dernières versions des composants Constellation.
<h4>Etape 1 : lancez le “Web Platform Installer</h4>
Pour lancer le WPI entrez la commande suivante :
<pre class="lang:default decode:true ">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
Le programme d’installation doit être lancé en “root” pour cela, le script se relancera automatiquement en “sudo” si la commande est présente. Autrement il tentera de se relancer automatiquement en “su root”. Si cette commande n’existe pas non plus, il affichera un message d’erreur. Vous aurez alors besoin de relancer manuellement le script “install.sh” en root.

Avec “sudo” ou “su root”, vous aurez dans certain cas besoin de fournir votre mot de passe “root” pour pouvoir autoriser le script d’installation à se lancer  :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Web Platform Installer" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-35.png" alt="Web Platform Installer" width="428" height="271" border="0" /></a></p>
<p align="left">Le script va vérifier les prérequis à savoir :</p>

<ul>
 	<li>
<div align="left">Python (2.7 ou 3.x)</div></li>
 	<li>
<div align="left">Python-dev</div></li>
 	<li>
<div align="left">Mono 3.10 sur un ARMv6 ou autrement Mono 3.12 minimum</div></li>
 	<li>
<div align="left">Supervisor</div></li>
</ul>
<p align="left">Sur un système Debian ou dérivé, le programme d’installation vous proposera d’installer automatiquement les prérequis si besoin.</p>

<h4 align="left">Etape 2 : sélection des composants</h4>
<p align="left">Une fois les prérequis validés, vous pourrez choisir les composants Constellation à installer :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Selection des composants" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-36.png" alt="Selection des composants" width="428" height="271" border="0" /></a></p>
Dans ce guide, sélectionnons le composant “Sentinel”. Vous commencerez par accepter la licence d’utilisation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Licence d'utilisation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-37.png" alt="Licence d'utilisation" width="428" height="271" border="0" /></a></p>
Puis vous devrez renseigner vos identifiants de votre compte myConstellation.io :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Identification" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-38.png" alt="Identification" width="428" height="271" border="0" /></a></p>
<p align="left">Définissez ensuite le répertoire d’installation de la sentinelle sur votre système :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Répertoire d'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-39.png" alt="Répertoire d'installation" width="428" height="271" border="0" /></a></p>

<h4 align="left">Etape 3 : sélection du serveur Constellation à rejoindre</h4>
<p align="left">Renseignez maintenant l’URI de votre serveur Constellation sur laquelle votre sentinelle doit se connecter :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="URI du serveur Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-40.png" alt="URI du serveur Constellation" width="428" height="271" border="0" /></a></p>
<p align="left">Notez que si vous souhaitez ajouter une sentinelle sur un système sur lequel un serveur Constellation est installé, l’assistant vous proposera d’installer et d’enregistrer automatiquement votre sentinelle sur le serveur local.</p>

<h4 align="left">Etape 4 : type d’installation de la sentinelle</h4>
<p align="left">Comme pour l’installation d’une sentinelle sous Windows, indiquez si vous souhaitez également procéder à son enregistrement :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Choix d'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-41.png" alt="Choix d'installation" width="428" height="271" border="0" /></a></p>
<p align="left">Au quel cas il faudra spécifier un compte avec les droits de management pour pouvoir joindre la sentinelle dans votre Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-42.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Identification sur le serveur Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-42.png" alt="Identification sur le serveur Constellation" width="244" height="155" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-43.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Identification sur le serveur Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-43.png" alt="Identification sur le serveur Constellation" width="244" height="155" border="0" /></a></p>

<h4 align="left">Etape 5 : choix de la clé d’accès pour la sentinelle</h4>
<p align="left">Vous pourrez ensuite choisir parmi les clé d’accès configurées sur votre serveur, laquelle doit être utilisée par votre sentinelle :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-44.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Choix de la clé d'accès" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-44.png" alt="Choix de la clé d'accès" width="428" height="271" border="0" /></a></p>

<h4 align="left">Etape 6 : prérequis pour les packages Python</h4>
<p align="left">Enfin si vous comptez déployer des packages Constellation Python vous devez installer le runtime Python ainsi que différentes librairies (<a href="/getting-started/creez-votre-premier-package-constellation-en-python/">plus d'informations</a>).</p>
<p align="left">Le WPI vous proposera d’installer ces prérequis pour vous de manière automatique :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des prérequis Python" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-10.png" alt="Installation des prérequis Python" width="432" height="273" border="0" /></a></p>

<h4 align="left">Etape 7 : installation et démarrage</h4>
<p align="left">Le WPI procèdera à l’installation et à la configuration de la sentinelle et des prérequis.</p>
<p align="left">A la fin de ce processus, la sentinelle sera automatiquement démarrée :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-45.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Fin de l'installation" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-45.png" alt="Fin de l'installation" width="428" height="271" border="0" /></a></p>
<p align="left">En retournant sur la Console de notre Constellation, on pourra constater qu’une nouvelle sentinelle ici nommée “rpi2” s’est bien connectée dans notre Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/05/image-47.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinelles connectées" src="https://developer.myconstellation.io/wp-content/uploads/2016/05/image_thumb-47.png" alt="Sentinelles connectées" width="428" height="170" border="0" /></a></p>
<p align="left">Votre sentinelle Linux est prête, vous pouvez maintenant y <a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">déployer des packages</a> ou développer vos propres packages en <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">C#</a> ou en <a href="/getting-started/creez-votre-premier-package-constellation-en-python/">Python</a>.</p>