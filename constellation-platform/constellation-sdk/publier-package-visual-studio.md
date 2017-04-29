---
ID: 2155
post_title: Publier un package depuis Visual Studio
author: Sebastien Warin
post_date: 2016-08-09 13:54:13
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-sdk/publier-package-visual-studio/
published: true
post_modified: 2016-11-03 17:37:24
---
Pour publier un package Constellation depuis Visual Studio vous pouvez cliquez sur l’icone <img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-90.png" alt="image" width="26" height="31" border="0" /> dans la barre Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-91.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-86.png" alt="image" width="350" height="144" border="0" /></a></p>
<p align="left">Le package à publier est celui marqué comme “Projet de démarrage” dans le cas où vous avez plusieurs projet dans votre solution Visual Studio.</p>
Autrement cliquez-droit sur le projet que vous souhaitez publier et cliquez sur “Publish Constellation package” dans le menu “Constellation” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-92.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-87.png" alt="image" width="350" height="318" border="0" /></a></p>
Vous pourrez ensuite publier votre package en local ou directement sur un serveur Constellation.
<h3>Publication locale</h3>
Il suffit d’indiquer un répertoire de sortie et un nom de fichier :

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-93.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-88.png" alt="image" width="350" height="174" border="0" /></a>
<h3>Publication sur un serveur Constellation</h3>
Pour publier votre package sur un serveur Constellation sélectionnez la méthode “Upload on Constellation Server” et indiquez le serveur à utiliser :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-94.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-89.png" alt="image" width="350" height="174" border="0" /></a></p>
Pour gérer les serveurs Constellation depuis Visual Studio, <a href="/constellation-platform/constellation-sdk/gerer-connexions-constellation/">consultez cet article</a>.

Une confirmation vous indiquera le bon déroulé de la publication :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-95.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-90.png" alt="image" width="350" height="121" border="0" /></a></p>
<p align="left">Notez que vous pouvez suivre le détail du processus de publication dans la fenêtre “Output” de Visual Studio :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-96.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-91.png" alt="image" width="350" height="89" border="0" /></a></p>
Une fois publié, votre package sera disponible dans le Package Repository de votre Constellation et sera prêt à être déployé sur une de vos sentinelles :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-97.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-92.png" alt="image" width="350" height="86" border="0" /></a></p>