---
ID: 2865
post_title: Le Package Repository
author: Sebastien Warin
post_date: 2016-09-23 15:59:39
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-console/package-repository/
published: true
publish_post_category:
  - "21"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:05:35
---
<p align="left">La notion de Package Repository a déjà été abordé dans <a href="/concepts/instance-package-versioning-et-resolution/">cet article</a>. En quelque mot le “Package Repository” est un répertoire dans lequel on stocke les packages qui seront ensuite déployés sur les différentes sentinelles de votre Constellation. Si ces notions ne vous semblent pas claires, je vous invite vivement à lire ou relire <a href="/concepts/instance-package-versioning-et-resolution/">cet article</a>.</p>
<p align="left">La page “Package Repository” de la Console Constellation permet d’administrer votre repository : uploader des packages de votre poste local ou télécharger des packages du catalogue en ligne, les supprimer, les renommer, etc..</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-71.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-69.png" alt="image" width="350" height="192" border="0" /></a></p>
<p align="left">Comme pour les autres pages de la Console, vous disposez d’un moteur de recherche pour filtrer les packages de votre repository :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-72.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-70.png" alt="image" width="183" height="64" border="0" /></a></p>
<p align="left">Pour chaque package de votre repository vous pourrez le déployer sur une sentinelle, le renommer, le supprimer ou afficher les détails du package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-73.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-71.png" alt="image" width="190" height="189" border="0" /></a></p>
<p align="left">Par exemple affichons les détails du package Nest ici en version 2.2 installé le 20/07/2016 sur cette Constellation:</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-74.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-72.png" alt="image" width="350" height="190" border="0" /></a></p>
<p align="left">En cliquant le bouton “Deploy” vous pourrez sélectionner la sentinelle sur laquelle déployer ce package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-75.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-73.png" alt="image" width="350" height="112" border="0" /></a></p>
<p align="left">En haut de la page vous pouvez afficher le catalogue en ligne, rafraîchir la liste ou encore uploader des packages depuis votre navigateur.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-76.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-74.png" alt="image" width="354" height="106" border="0" /></a></p>
Pour uploader un ou plusieurs packages, il suffit de “drag &amp; dropper” les packages dans la fenêtre :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-77.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-75.png" alt="image" width="350" height="128" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-78.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-76.png" alt="image" width="350" height="168" border="0" /></a></p>
Depuis la version 1.8.1 de la Console Constellation, vous pouvez parcourir la liste des packages disponible sur le <a href="/plateforme/package-repository/">catalogue Constellation</a> en ligne :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-79.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-77.png" alt="image" width="350" height="213" border="0" /></a></p>
<p align="left">Vous pouvez alors cliquer sur le bouton “Deploy” pour télécharger automatiquement des packages dans le Package Repository de votre serveur Constellation et lancer l’assistant de déploiement.</p>
Pour les packages installés et à jour, vous avez l’icone <a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-84.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-80.png" alt="image" width="25" height="21" border="0" /></a> à côté du nom. Si le package est installé mais qu’une version plus récente est disponible sur le catalogue vous aurez l’icone suivante  <a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-85.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-81.png" alt="image" width="26" height="26" border="0" /></a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-81.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-79.png" alt="image" width="350" height="214" border="0" /></a></p>
<p align="left">Vous pouvez alors cliquer sur le bouton “Update” pour télécharger la nouvelle version dans votre Package Repository.</p>
<p align="left">Notez également que des notifications de mise à jour des packages (et des composants Constellation) sont affichées sous forme de notification dans la barre du haut :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-80.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-78.png" alt="image" width="350" height="144" border="0" /></a></p>
Une fois le package correctement téléchargé, vous obtiendriez une notification dans la Console Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-86.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-82.png" alt="image" width="240" height="69" border="0" /></a></p>
<p align="left">Ensuite une nouvelle fenêtre s’ouvrera automatiquement pour vous proposer de recharger directement le ou les packages sur les sentinelles de votre Constellation où ils sont déployés :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Déploiement des mises à jour des packages" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-2.png" alt="Déploiement des mises à jour des packages" width="227" height="240" border="0" /></a></p>
&nbsp;