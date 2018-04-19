---
ID: 4428
post_title: Constellation sur un Raspberry Pi
author: Sebastien Warin
post_date: 2017-05-04 12:26:38
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/constellation-raspberry-pi/
published: true
publish_post_category:
  - "19"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:12:32
---
Le <b>Raspberry Pi</b> est un nano-ordinateur monocarte à <a href="https://fr.wikipedia.org/wiki/Architecture_ARM">processeur ARM</a> conçu par le créateur de jeux vidéo <a href="https://fr.wikipedia.org/wiki/David_Braben">David Braben</a>. La première version est sortie en 2012 et depuis il y a eu beaucoup d’évolutions.

Il permet l'exécution de plusieurs variantes du système d'exploitation libre Linux et même de Windows 10 IoT.
<h3>Les différents modèles et versions du Raspberry Pi</h3>
A ce jour, on distingue trois types de modèle :
<ul>
 	<li><u>Le modèle A</u> : Raspberry “light” sans interface réseau</li>
 	<li><u>Le modèle B</u> : la version la plus répandue du Raspberry (avec interface réseau)</li>
 	<li><u>Le modèle Zero</u> : version minimaliste et low-cost  du Raspberry</li>
</ul>
Les Raspberry Pi A et B sont animés par un ARM11 (ARMv6) à 700Mhz avec 256 Mo de mémoire vive pour le modèle d'origine (512 Mo sur les dernières versions du modèle B/B+).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/raspberry-pi-model-a-vs-mod_opt.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Raspberry Pi A/A+" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/raspberry-pi-model-a-vs-mod_opt_thumb.jpg" alt="Raspberry Pi A/A+" width="240" height="127" border="0" /></a></p>
En 2015 le Raspberry Pi B version 2 (B2) est équipé d'un processeur Broadcom BCM2836, quatre cœurs ARM Cortex-A7 (ARMv7) à 900 MHz, accompagné de 1 Go de RAM.

Puis en début 2016, pour le quatrième anniversaire de la commercialisation du premier modèle, la fondation Raspberry Pi annonce la sortie du Raspberry Pi B version 3 (B3). Comparé au B2, il dispose d'un processeur Broadcom BCM2837 64 bit à quatre cœurs ARM Cortex-A53 (ARMv8) à 1,2 GHz et d'une puce Wifi 802.11n et Bluetooth 4.1 intégrée.

Le modèle A n’a qu’en a lui pas évolué.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/raspberry_pi_b.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Raspberry Pi B+" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/raspberry_pi_b_thumb.jpg" alt="Raspberry Pi B+" width="240" height="188" border="0" /></a></p>
De son côté le Raspberry Zero sorti fin 2015 reprend les spécifications du modèle A/B version 1 avec un processeur ARM11 (ARMv6) cadencé à 1 GHz au lieu de 700 MHz. Il est par contre plus petit, disposant d'une connectique minimale. Début 2017, pour le cinquième anniversaire du Raspberry Pi, le Raspberry Pi Zero est maintenant doté de Wi-fi et de Bluetooth en conservant les mêmes spécifications.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/RASP_PI_ZERO.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Raspberry Pi Zero" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/RASP_PI_ZERO_thumb.png" alt="Raspberry Pi Zero" width="240" height="142" border="0" /></a></p>
Retrouvez ici toutes <a href="https://fr.wikipedia.org/wiki/Raspberry_Pi#Sp.C3.A9cifications_mat.C3.A9rielles_et_architectures">les spécifications du Rapsberry sur cette page</a> ainsi qu’un <a href="https://fr.wikipedia.org/wiki/Raspberry_Pi#Tableau_comparatif">tableau comparatif</a>.

Ce qui est important de retenir ici :
<ul>
 	<li>Tous les modèles B intègrent nativement une interface Ethernet et les version B3 et Zero-W intègrent une interface Wifi. Sur les autres versions, en particulier les modèles A, il faudra connecter une clé USB Wifi pour disposer d’une interface réseau.</li>
 	<li>Les modèles A/A+, B/B+ (v1) et Zero/ZeroW sont tous animés par des processeurs ARMv6 à la différence des RPi B2 et B3, respectivement équipés d’un ARMv7 et ARMv8.</li>
</ul>
<h3>Raspberry Pi et Constellation</h3>
Comme le <b>Raspberry Pi</b> est un véritable (nano) ordinateur basé sur une architecture ARM et sur lequel on peut installer un système Linux, il est à la fois possible  :
<ul>
 	<li>de l’utiliser pour héberger une Constellation, c’est à dire déployer le <strong>serveur Constellation</strong></li>
 	<li>de l’utiliser comme <strong>Sentinelle</strong> pour pouvoir déployer des packages</li>
</ul>
Par exemple, la <a href="http://sebastien.warin.fr/2015/03/24/2478-senergy-la-solution-de-monitoring-des-ressources-energetiques-de-la-maison-geek-is-in-da-house-2015/">solution de monitoring des ressources énergétiques de la maison S-Energy</a> ou encore <a href="http://sebastien.warin.fr/2015/08/20/2833-s-opener-connectez-et-scurisez-votre-porte-de-garage-avec-constellation-et-un-raspberry-pi-la-porte-de-garage-intelligente/">S-Opener, la porte de garage intelligente</a> sont deux solutions basées sur des Raspberry Pi exploités comme sentinelles d’une Constellation.

Ceci dit il est important de se rappeler des prérequis pour ces deux composants Constellation :
<ul>
 	<li>Le serveur Constellation nécessite au minimum Mono 3.12</li>
 	<li>La sentinelle Constellation nécessite au minimum Mono 3.10</li>
</ul>
Or Mono 3.12 et + n’est pas stable sur une architecture ARMv6. Le <strong><em>Web Platform Installer</em></strong> Linux installera automatiquement la dernière version de Mono <u>à l’exception</u> des architectures ARMv6 pour lequel il installera la version 3.10, la seule stable (à ce jour du moins) sur ce type d’architecture.

<u>Autrement dit :</u>
<ul>
 	<li>La <strong>sentinelle Constellation</strong> peut être installée sur <strong>tous les Raspberry Pi</strong></li>
 	<li>Le <strong>serveur Constellation</strong> ne peut être installé que sur un <strong>Raspberry Pi B version 2 ou version 3</strong></li>
</ul>
Autre information très importante, le fait d’être sur un ARMv6 (modèles A/A+, B/B+ et Zero/ZeroW) restreint Mono à la version 3.10 ce qui restreint l’usage de package utilisant le framework .NET 4.0 !

Autrement dit, les <u>packages Constellation développés pour .NET 4.5 (et plus) peuvent être instables sur un ARMv6.</u>

De ce fait si vous développez des <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">packages C#</a> ou des <a href="/getting-started/creez-votre-premier-package-constellation-en-python/">packages Python</a> que vous souhaitez déployer sur une sentinelle Raspberry A/A+, B/B+ ou Zero/ZeroW, n’oubliez pas de définir le “Target” de votre projet à “.NET Framework 4” et pas au delà lors de la création de votre projet ou dans les propriétés de celui-ci.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-46.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Target des packages" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-46.png" alt="Target des packages" width="244" height="171" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-47.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Target des packages" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-47.png" alt="Target des packages" width="234" height="171" border="0" /></a></p>
<p align="left"><strong><u>En résumé :</u></strong></p>

<ul>
 	<li>
<div align="left">Les Raspberry Pi A/A+, B/B+ et Zero/ZeroW sont équipés d’un ARMv6, donc Mono 3.10 au maximum, donc “Sentinelle Constellation” seulement pour déployer des packages .NET 4.0 au maximum.</div></li>
 	<li>
<div align="left">Les Raspberry Pi B2 ou B3 sont équipés d’ARM V7 ou V8 permettant l’installation des dernières versions de Mono, permettant donc l’installation du serveur Constellation et/ou de la sentinelle pour déployer des packages .NET 4.0, 4.5 ou plus.</div></li>
</ul>
<h3>Installer Constellation sur un Raspberry</h3>
<h4>Etape 1 : installer Raspbian</h4>
A l’aide d’un outil comme “<a href="https://sourceforge.net/projects/win32diskimager/">Win32 Disk Imager</a>” sous Windows, copiez la dernière version en date de l’image du <a href="https://www.raspberrypi.org/downloads/raspbian/">système Raspbian</a> sur une carte MicroSD.

Je vous recommande l’image “<a href="https://www.raspberrypi.org/downloads/raspbian/">Lite</a>” du système, c’est à dire sans interface graphique car inutile pour Constellation, nous exploitons le Raspberry à travers la plateforme Constellation et SSH pour l’installation initiale.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Copie de Raspbian sur la carte SD" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-39.png" alt="Copie de Raspbian sur la carte SD" width="304" height="156" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Copie de Raspbian sur la carte SD" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-40.png" alt="Copie de Raspbian sur la carte SD" width="304" height="156" border="0" /></a></p>
<p align="left">Une fois l’image de Raspbian copiée sur votre carte MicroSD, créez un fichier nommé “ssh” (sans extension et sans contenu) à la racine de votre carte SD de façon à activer le service SSH lors du premier boot.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Activation du SSH" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-41.png" alt="Activation du SSH" width="304" height="259" border="0" /></a></p>
<p align="left">Vous pouvez maintenant insérer votre carte MicroSD dans votre Raspberry puis l’alimenter sans oublier de le connecter au réseau via l’interface Ethernet.</p>
<p align="left">Après quelques secondes, connectez-vous dessus à l’aide d’un client SSH comme <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">Putty</a>. Le nom d’utilisateur est “pi” et le mot de passe par défaut est “raspberry” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-42.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Connexion SSH au Raspberrry" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-42.png" alt="Connexion SSH au Raspberrry" width="428" height="273" border="0" /></a></p>
<p align="left">Voilà votre Raspberry Pi est prêt et connecté !</p>

<h4>Etape 2 : configurer son Raspberry Pi</h4>
Avant de démarrer l’installation de Constellation, je vous recommande vivement de configurer certain élément de base sur ce système fraichement installé.

Pour cela lancez l’utilitaire “raspi-config” par la commande :
<pre class="lang:bash decode:true">sudo rapsi-config</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-43.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Raspi-config" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-43.png" alt="Raspi-config" width="428" height="273" border="0" /></a></p>
Les actions de base à réaliser :
<ol>
 	<li><u>Change User Password</u> : changez le mot de passe par défaut de l’utilisateur “pi”</li>
 	<li><u>Hostname</u> : changez le nom du système (par défaut “raspberry”) pour l’identifier plus facilement. N’oubliez pas que le nom du système sert pour identifier une sentinelle dans une Constellation</li>
 	<li><u>Localisation Options &gt; Change Timezone</u> : spécifiez votre fuseau horaire pour définir correctement l’heure locale du système</li>
 	<li><u>Interfacing Options &gt; SSH</u> : activez le service SSH</li>
 	<li><u>Advanced Options &gt; Expand Filesystem</u> : si nécessaire redimensionne la partition pour exploiter pleinement la capacité de votre carte SD</li>
</ol>
Optionnellement si vous avez une interface Wifi (native sur les B3 et Zero-W ou externe) configurez le réseau à joindre.

Pour finir, n’hésitez pas à mettre à jour les sources APT avant de lancer le WPI surtout si vous utilisez une ancienne image.
<pre class="lang:bash decode:true">sudo apt-get update</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-44.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Mise à jour des sources APT" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-44.png" alt="Mise à jour des sources APT" width="354" height="268" border="0" /></a></p>

<h4>Etape 3 : lancer le Web Platform Installer</h4>
Pour télécharger et démarrer <strong><em>Web Platform Installer</em></strong>, rien de plus simple ! Lancez simplement la commande suivante :
<pre class="lang:bash decode:true">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
Le WPI se chargera de vérifier et installer l’ensemble des prérequis puis lancera le programme d’installation de Constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-45.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Démarrage du Web Platform Installer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-45.png" alt="Démarrage du Web Platform Installer" width="354" height="277" border="0" /></a></p>
<p align="left">Vous pourrez alors installer une sentinelle sur votre Raspberry ou le serveur Constellation si vous êtes sur un RPi B2 ou B3.</p>

<h3>Next steps</h3>
Que souhaitez-vous souhaitez installer maintenant sur votre Raspberry ?
<ul>
 	<li><a href="/constellation-platform/constellation-server/installer-constellation-sur-linux/#Installer_la_plateforme">le <strong>serveur</strong> Constellation</a></li>
 	<li><a href="/getting-started/ajouter-des-sentinelles/#Installation_dune_sentinelle_sur_un_systeme_Linux">la <strong>sentinelle</strong> Constellation</a></li>
</ul>