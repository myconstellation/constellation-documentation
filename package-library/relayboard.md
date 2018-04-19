---
ID: 3539
post_title: 'RelayBoard : pilotez vos cartes de relais USB SainSmart dans Constellation'
author: Sebastien Warin
post_date: 2016-10-27 15:08:58
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/relayboard/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:29:28
---
Le package RelayBoard vous permet de piloter des cartes de relais USB de 4 ou 8 canaux disponible chez <a href="http://www.sainsmart.com/">SainSmart</a>.

<p align="center"><a href="http://www.sainsmart.com/sainsmart-4-channel-12-v-usb-relay-board-module-controller-for-automation-robotics-1.html"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-142.png" alt="image" width="240" height="184" border="0" /></a><a href="http://www.sainsmart.com/sainsmart-4-channel-5v-usb-relay-board-module-controller-for-automation-robotics.html"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-143.png" alt="image" width="176" height="180" border="0" /></a></p>

Le code source est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/RelayBoard">https://github.com/myconstellation/constellation-packages/tree/master/RelayBoard</a>

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package RelayBoard :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-144.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-130.png" alt="image" width="350" height="214" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Vous pouvez spécifier dans les Settings, le numéro de série de votre carte autrement le package se connectera sur la première carte disponible. Vous pouvez spécifier le nombre de relais disponibles sur votre carte (4 ou 8) :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-145.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-131.png" alt="image" width="350" height="213" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="RelayBoard" /&gt;</pre>

<h3>Détails du package</h3>

<h4>Les Settings</h4>

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="10"><u>Détail</u></td>
<td valign="top" width="478"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>SerialNumber</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="478">Numéro de série de la carte SainSmart. Si non renseigné, le package se connectera sur la première carte connectée.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>RelayCount</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 8</td>
<td valign="top" width="478">Nombre de relais disponibles sur votre carte SainSmart.</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que de relais ainsi qu’un StateObject représentant votre carte SainSmart :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Relay&lt;&lt; ID &gt;&gt;</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="446">Etat du relais (ON / Off)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>RelayBoard_&lt;&lt; Serial Number &gt;&gt;</strong></td>
<td valign="top" width="10">RelayBoard</td>
<td valign="top" width="446">Informations sur la carte de relais USB (S/N, Id, port COM, version du driver, ..)</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-146.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-132.png" alt="image" width="350" height="146" border="0" /></a></h3>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 1 MessageCallback :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetSwitch</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Défini l’état d’un relais (On ou Off)</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-147.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-133.png" alt="image" width="350" height="131" border="0" /></a></p>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher et contrôler les relais depuis une page Web</li>
    <li>Piloter l’allumage des amplis de manière automatique en fonction de la source audio avec un package C#</li>
</ul>