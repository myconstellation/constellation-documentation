---
ID: 2380
post_title: >
  Gérer les sentinelles avec la Console
  Constellation
author: Sebastien Warin
post_date: 2016-08-19 16:05:41
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-console/gerer-sentinelles-avec-la-console-constellation/
published: true
publish_post_category:
  - "21"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:03:00
---
<h3>Gérer les sentinelles</h3>

<p align="left">Dans le menu principal de gauche, cliquez sur “Sentinels” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-21.png" alt="image" width="350" height="179" border="0" /></a></p>

<p align="left">Vous retrouverez sur cette page l’ensemble des sentinelles de votre Constellation.</p>

<p align="left">Le statut peut être :</p>

<ul>
    <li>
<div align="left"><u>Unknow</u> : une sentinelle qui ne sait jamais connectée ou une sentinelle virtuelle</div></li>
    <li>
<div align="left"><u>Disconnected</u> : une sentinelle qui est déconnectée après avoir été connecté</div></li>
    <li>
<div align="left"><u>Connected</u> : la sentinelle est connectée</div></li>
</ul>

<p align="left">Dans le cadre d’une sentinelle réelle (connectée ou déconnectée) vous obtiendrez également la version de la sentinelle, du système d'exploitation et autres informations sur le moteur d'execution .NET.</p>

<p align="left">Le bouton “Détails” permet d’afficher l’ensemble des informations d’une sentinelle :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-22.png" alt="image" width="350" height="279" border="0" /></a></p>

<p align="left">Il s’agit d’un menu contextuel qui vous permettra également d’éditer les informations de connexion de la sentinelle, de la supprimer ou encore de voir ou ajouter des packages sur cette sentinelle :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-34.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-23.png" alt="image" width="202" height="240" border="0" /></a></p>

<h3>Ajouter une sentinelle</h3>

Pour déclarer une sentinelle réelle ou <a href="/concepts/sentinels-packages-virtuels/">virtuelle</a>, cliquez sur le bouton “Add sentinel” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-24.png" alt="image" width="350" height="182" border="0" /></a></p>

<p align="left">Dans la fenêtre d’ajout saisissez le nom de votre sentinelle ainsi que le credential à utiliser pour la connexion :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-25.png" alt="image" width="350" height="144" border="0" /></a></p>

<p align="left">Pour ajouter et gérer vos credentials, rendez-vous sur la page “<a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">Server Management &gt; Credentials</a>”.</p>

<p align="left"><u>Note</u> : dans le cas d’une sentinelle Service, le nom par défaut est le nom de la machine (hostname Linux ou Windows) et dans le cas d’une sentinelle UI (pour Windows seulement), il s’agit du nom de la machine suivi du suffixe “_UI”. Notez toutefois que vous pouvez <a href="/constellation-platform/constellation-sentinel/custom-sentinel/">modifier le nom de la sentinelle dans la configuration de celle-ci</a>. De plus les installeurs Windows et Linux des sentinelles utilisent l’API de Management pour enregistrer automatiquement la sentinelle au serveur, vous n’avez donc pas à l’ajouter manuellement. Cet écran servira surtout pour l’ajout de <a href="/concepts/sentinels-packages-virtuels/">sentinelle virtuelle</a>.</p>