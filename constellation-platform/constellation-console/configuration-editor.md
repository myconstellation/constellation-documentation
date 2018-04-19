---
ID: 2838
post_title: Le Configuration Editor
author: Sebastien Warin
post_date: 2016-09-23 15:51:06
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-console/configuration-editor/
published: true
publish_post_category:
  - "21"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:05:59
---
L’éditeur de configuration intégré à la Console Constellation permet d’éditer le fichier de configuration de votre serveur Constellation directement depuis votre navigateur Web.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-67.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-65.png" alt="image" width="350" height="191" border="0" /></a></p>

Pour plus d’information sur le <a href="/constellation-platform/constellation-server/fichier-de-configuration/">fichier de configuration</a>, n'hésitez pas à lire ou relire <a href="/constellation-platform/constellation-server/fichier-de-configuration/">cet article</a>.

Vous pouvez mettre l’éditeur en plein écran avec le bouton F11.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-68.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-66.png" alt="image" width="350" height="198" border="0" /></a></p>

<p align="left">L’éditeur dispose d’un système d’auto-complétion assez simple sur base du schéma du fichier de configuration :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-69.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-67.png" alt="image" width="350" height="149" border="0" /></a></p>

<p align="left">Vous trouverez sur l’éditeur trois boutons :</p>

<ul>
    <li>
<div align="left">Get Config : permettant d’ouvrir la dernière version du serveur</div></li>
    <li>
<div align="left">Save Config : permettant d’enregistrer la configuration en cours d’édition sur le serveur (raccourci : Ctrl+S)</div></li>
    <li>
<div align="left">Save &amp; Deploy : permettant d’enregistrer la configuration en cours d’édition sur le serveur puis d’appliquer (déployer) la configuration dans la Constellation</div></li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-70.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-68.png" alt="image" width="350" height="122" border="0" /></a></p>

Le fait de déployer la configuration informe tous les packages et les sentinelles de votre Constellation de la nouvelle configuration. C’est en déployant la configuration que les sentinelles vont déployer les nouveaux packages ajoutés ou au contraire fermer les packages supprimés, etc.