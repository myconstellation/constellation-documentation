---
ID: 3265
post_title: 'BatteryChecker : vos batteries et onduleurs dans Constellation'
author: Sebastien Warin
post_date: 2016-10-21 10:47:02
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/batterychecker/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:35:44
---
Le package BatteryChecker synchronise l’état de vos batterie (ou onduleurs) dans les StateObjects Constellation afin de pouvoir exploiter cette donnée dans vos pages Web, programmes, objets connectés ou autre.

Attention ce package exploite la couche WMI (<em>Windows Management Instrumentation</em>) pour récupérer l’état des batteries et onduleurs. De ce fait, ce package ne fonctionnera que sur des sentinelles Windows.

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package BatteryChecker :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-60.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-57.png" alt="image" width="350" height="214" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous pouvez redéfinir l’intervalle de rafraichissement des StateObjects de vos batteries/onduleurs. Par défaut la valeur est configurée à 5000 ms, c’est à dire que les StateObjects de vos batteries seront rafraichit toutes les 5 secondes.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-61.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-58.png" alt="image" width="350" height="188" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="BatteryChecker" /&gt;</pre>

<h3>Détails du package</h3>

<h4>Les Settings</h4>

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="10"><u>Détail</u></td>
<td valign="top" width="456"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>RefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 5000</td>
<td valign="top" width="456">Intervalle de temps en milliseconde pour la mise à jour de l’état des batteries dans Constellation.</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que vous avez de batteries ou onduleurs :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;Serial Number&gt;</strong></td>
<td valign="top" width="10">BatteryChecker.BatteryState</td>
<td valign="top" width="446">StateObject dont le nom est le n° de série de votre batterie/onduleur, et la valeur contient différentes informations sur son état, temps de charge estimé en %, temps de charge restant en minutes, etc ..</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-62.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-59.png" alt="image" width="350" height="104" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package n’expose aucun MessageCallback.

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Envoyer une notification en cas de  batterie trop faible</li>
    <li>Afficher la charge de la batterie sur un anneau de LED avec un Arduino/ESP</li>
    <li>Afficher l’état de la batterie sur un Dashboard HTML</li>
</ul>