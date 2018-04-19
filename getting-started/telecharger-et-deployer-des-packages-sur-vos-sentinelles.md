---
ID: 1149
post_title: >
  Télécharger et déployer des packages
  sur vos sentinelles
author: Sebastien Warin
post_date: 2016-03-15 11:58:25
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/
published: true
publish_post_category:
  - "21"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:28:50
---
<p style="text-align: center;"><iframe width="560" height="315" src="https://www.youtube.com/embed/cJ6oV9x-grc" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>

<h3>Télécharger et déployer des packages depuis la Console Constellation</h3>

Sur la Console Constellation, rendez-vous sur la page “Package Repository”, le catalogue des packages de votre Constellation. C’est là que vous pourrez gérer votre catalogue de package.

Pour ajouter des packages dans votre catalogue, vous pouvez soit <a href="#Uploader_des_packages_manuellement">les uploader manuellement</a> ou soit les télécharger directement depuis la catalogue de package gratuit Constellation en cliquant sur le bouton “Online Package Repository” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-142.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-137.png" alt="image" width="350" height="198" border="0" /></a></p>

<p align="left">Vous pourrez alors choisir sur le catalogue en ligne les packages à télécharger dans votre catalogue local.</p>

<p align="left">Par exemple, ajoutons le package “NetworkTools” en cliquant sur le bouton “Deploy” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-143.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-138.png" alt="image" width="350" height="214" border="0" /></a></p>

Le téléchargement débutera alors :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-144.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-139.png" alt="image" width="240" height="69" border="0" /></a></p>

Une fois le package télécharger un assistant de déploiement s’affichera vous invitant à sélectionner la sentinelle sur laquelle déployer votre package :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-145.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-140.png" alt="image" width="350" height="114" border="0" /></a></p>

Dans mon cas je choisis de déployer ce package sur la sentinelle “PO-SWARIN”.

Vous pouvez ensuite sélectionner les options de déploiement, pour démarrer laissez les paramètres par défaut :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-146.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-141.png" alt="image" width="350" height="312" border="0" /></a></p>

<p align="left">Ensuite chaque package peut déclarer des settings. Ici le package “NetworkTools” ne déclare qu’un setting pour le monitoring. Vous pouvez éditer le JSON pour définir les ressources à monitorer :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-147.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-142.png" alt="image" width="350" height="287" border="0" /></a></p>

<p align="left">En cliquant sur le bouton “Deploy”, Constellation va automatiquement déployer ce package sur la sentinelle de votre choix. Sur la page “Packages” vous retrouverez vos packages démarrés. Vous pourrez suivre l’état et l’uptime de chaque package, sa consommation CPU et RAM, etc… Vous pouvez également arrêter, redémarrer ou mettre à jour vos packages sur n’importe quelle sentinelles grâce à cette page :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-148.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-143.png" alt="image" width="350" height="226" border="0" /></a></p>

<p align="left">Sur la page “Console log” vous pourrez suivre en temps réel les logs produit par l’ensemble des packages de votre Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-149.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-144.png" alt="image" width="350" height="223" border="0" /></a></p>

<p align="left">Sur le StateObject Explorer vous pourrez consulter les StateObjects publiés par chaque package. Ici le package “NetworkTools” publie un StateObject pour chaque ressource qu’il monitore (décrit dans ses settings) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-150.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-145.png" alt="image" width="350" height="118" border="0" /></a></p>

<p align="left">Vous pouvez cliquer sur le bouton “View” pour afficher le détail de chaque StateObject et le suivre en temps réel :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-151.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-146.png" alt="image" width="350" height="277" border="0" /></a></p>

<p align="left">Sur le MessageCallback Explorer, vous pouvez consulter toutes les méthodes que vos packages exposent. Un formulaire permet même de tester les MC depuis la Console :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-152.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-147.png" alt="image" width="350" height="220" border="0" /></a></p>

Notez qu’à tout moment vous pouvez vous rendre dans votre Package Repository et cliquez sur le bouton “Deploy package” dans le menu “Action” du package que vous souhaitez à nouveau déployer :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-153.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-148.png" alt="image" width="350" height="257" border="0" /></a></p>

<h3 align="left">Quelques packages pour démarrer</h3>

<ul>
    <li>
<div align="left"><a href="/package-library/BatteryChecker"><u>BatteryChecker</u> </a>: permet de publier un StateObject sur l’état de votre batterie (pour les PC portables ou onduleurs)</div></li>
    <li>
<div align="left"><a href="/package-library/DayInfo/"><u>DayInfo</u></a> : information sur l’heure du levé et couché du soleil et la fete du jour</div></li>
    <li>
<div align="left"><a href="/package-library/HWMonitor/"><u>HWMonitor</u> </a>: permet de publier des StateObjects sur les compteurs “Hardware” de votre machines (CPU, RAM, réseau, disques durs, etc…)</div></li>
    <li>
<div align="left"><a href="/package-library/WindowsControl"><u>WindowsControl</u> </a>: exposer des MessageCallbacks pour contrôler l’état d’une machine Windows (Reboot, Shutdown, Sleep, Lock, …)</div></li>
    <li>
<div align="left"><a href="/package-library/ForecastIO"><u>ForecastIO</u></a> : service de météo</div></li>
    <li>
<div align="left"><a href="/package-library/FreeboxTV"><u>FreeboxTV</u></a> : pour les possesseurs de Freebox</div></li>
    <li>
<div align="left"><a href="/package-library/hue/"><u>Hue</u> </a>: pilotage des lampes Hue</div></li>
    <li>
<div align="left"><a href="/package-library/Nest"><u>Nest</u> </a>: pilotage des thermostats et détecteur de fumée Nest</div></li>
    <li>
<div align="left"><a href="/package-library/NetAtmo"><u>NetAtmo</u></a> : intégration des capteurs NetAtmo dans Constellation</div></li>
    <li>
<div align="left"><a href="https://developer.myconstellation.io/plateforme/package-repository/"><u>PushBullet</u> </a>: envoi de notification sur smartphone, tablette et PC</div></li>
    <li>
<div align="left"><a href="/package-library/rfxcom"><u>Rfxcom</u> </a>: intégration des capteur RF</div></li>
    <li>
<div align="left"><a href="/package-library/vera"><u>Vera</u> </a>: intégration de la box domotique Vera</div></li>
    <li>
<div align="left"><a href="/package-library/Wemo"><u>Wemo</u></a> : intégration des prises connectée Wemo</div></li>
    <li>
<div align="left"><a href="/package-library/xml"><u>Xbmc</u> </a>: intégration des media-centers Kodi</div>
<!--EndFragment--></li>
</ul>

Rendez-vous dans la <a href="/package-library/">rubrique packages</a>.

<h3>Autres moyens</h3>

<h4>Uploader des packages manuellement</h4>

Vous pouvez par exemple télécharger des packages manuellement depuis <a href="/plateforme/package-repository/">le catalogue officiel</a> ou tout autre source.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-187.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Catalogue de packages" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-164.png" alt="Catalogue de packages" width="244" height="469" border="0" /></a></p>

<p align="left">Depuis la Console, rendez-vous sur la page “Package Repository” et cliquez sur le bouton “Upload” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-45.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Upload des packages" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-44.png" alt="Upload des packages" width="424" height="275" border="0" /></a></p>

<p align="left">Vous pouvez alors glisser-déplacer les package Constellation (fichiers ZIP) dans le rectangle ou cliquer dessus pour ouvrir la fenêtre de sélection du fichier :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-46.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Upload des packages" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-45.png" alt="Upload des packages" width="424" height="221" border="0" /></a></p>

Déposons les différents packages téléchargés dans le rectangle afin de les uploader dans votre catalogue :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb65.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Upload des packages" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb65_thumb.png" alt="Upload des packages" width="420" height="355" border="0" /></a></p>

<p align="left">Et voilà les packages sont disponibles dans le Package Repository de votre Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb66.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb66_thumb.png" alt="Package Repository" width="420" height="169" border="0" /></a></p>

<h4 align="left">Uploader des packages depuis Visual Studio</h4>

Vous pouvez publier un package Constellation directement depuis Visual Studio :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-91.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-86.png" alt="image" width="350" height="144" border="0" /></a></p>

Le package publié est celui marqué comme “Projet de démarrage” dans le cas où vous avez plusieurs projet dans votre solution. Autrement cliquez-droit sur le projet que vous souhaitez publier et cliquer sur “Publish Constellation package” dans le menu “Constellation” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-92.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-87.png" alt="image" width="350" height="318" border="0" /></a></p>

Vous pourrez ensuite publier votre package en local ou directement sur un serveur Constellation.

<p align="left">Pour plus d’information, <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">consultez cet article</a>.</p>

<h4 align="left">Déployer manuellement des packages sur vos sentinelles</h4>

<p align="left">La définition des déploiements, c’est à dire “quel package sur quelle sentinelle” es t décrite dans <a href="/constellation-platform/constellation-server/fichier-de-configuration/">le fichier de configuration de la Constellation</a>.</p>

<p align="left">Vous pouvez éditer ce fichier manuellement sur votre serveur Constellation avec notre éditeur de texte préféré, <a href="/constellation-platform/constellation-sdk/editer-configuration-constellation-depuis-visual-studio/">depuis Visual Studio</a> ou directement depuis la <a href="/constellation-platform/constellation-console/configuration-editor/">Console Constellation</a> :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-188.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Edition de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-165.png" alt="Edition de la configuration" width="424" height="134" border="0" /></a></p>

<p align="left">Par exemple déployons les 5 packages ci-dessous sur la sentinelle nommée “PO-SEB” :</p>

<pre class="lang:xml decode:true">&lt;sentinel name="PO-SEB" credential="StandardAccess"&gt;
    &lt;packages&gt;        
        &lt;package name="BatteryChecker"&gt;&lt;/package&gt; 
        &lt;package name="GoogleTraffic"&gt;&lt;/package&gt; 
        &lt;package name="HWMonitor"&gt;&lt;/package&gt; 
        &lt;package name="WakeOnLan"&gt;&lt;/package&gt; 
        &lt;package name="WindowsControl"&gt;&lt;/package&gt; 
    &lt;/packages&gt;        
&lt;/sentinel&gt;</pre>

<p align="left">En cliquant sur le bouton “Save &amp; Deploy”, vous allez enregistrer la configuration sur le serveur et déployer cette configuration dans votre Constellation.</p>

<p align="left">Pour plus d’information sur le schéma XML de <a href="/constellation-platform/constellation-server/fichier-de-configuration/">la configuration Constellation</a> consultez <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_sentinels">cet article</a>.</p>