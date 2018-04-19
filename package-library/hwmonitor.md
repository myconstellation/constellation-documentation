---
ID: 3592
post_title: 'HWMonitor : surveillez les capteurs hardware de vos machines'
author: Sebastien Warin
post_date: 2016-10-28 10:56:12
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/hwmonitor/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:24:16
---
Le package HWMonitor vous permet de remonter dans les StateObjects Constellation tous les capteurs hardware sur une machine Windows (consommation CPU, RAM, disques et réseau, températures du CPU, du chassis et des disques, etc…).
<p align="left">Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/HWMonitor">https://github.com/myconstellation/constellation-packages/tree/master/HWMonitor</a></p>

<h3>Installation</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package HWMonitor :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-164.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-148.png" alt="image" width="350" height="211" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :
<pre class="lang:html5 decode:true">&lt;package name="HWMonitor" /&gt;</pre>
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
<td valign="top" width="10"><strong>Interval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 1000</td>
<td valign="top" width="478">Intervalle de temps en milliseconde pour la remontée des capteurs dans Constellation (par défaut toutes les secondes).</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Vous retrouverez autant de StateObjects que de capteurs Hardwares disponibles ainsi que des StateObjects par disques dur et un StateObject contenant la liste du hardware d’une machine :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Hardware</strong></td>
<td valign="top" width="10">List&lt;HWMonitor.HardwareDevice&gt;</td>
<td valign="top" width="446">Liste des périphériques de la machine</td>
</tr>
<tr>
<td valign="top" width="10"><strong>/hdd/&lt;&lt; ID &gt;&gt;</strong></td>
<td valign="top" width="10">HWMonitor.DiskDrive</td>
<td valign="top" width="446">Information sur un disque dur (nom, S/N , modèle, fabriquant, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du capteur &gt;&gt;</strong></td>
<td valign="top" width="10">HWMonitor.SensorValue</td>
<td valign="top" width="446">Mesure d’un capteur (nom, valeur et unité de la valeur mesurée)</td>
</tr>
</tbody>
</table>
<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-165.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-149.png" alt="image" width="350" height="192" border="0" /></a>
<h4 align="left">Les MessageCallbacks</h4>
Ce package n’expose pas de MessageCallbacks.
<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Afficher la consommation du CPU de la RAM dans un dashboard HTML</li>
 	<li>Surveillez l’ensemble des capteurs hardware dans une application Windows WPF</li>
 	<li>Animer un anneau de LED RBG en fonction de la consommation CPU d’un ordinateur Windows avec un Arduino/ESP</li>
</ul>