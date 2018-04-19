---
ID: 3256
post_title: 'DayInfo : heures de lever et coucher du soleil et fête du jour'
author: Sebastien Warin
post_date: 2016-10-21 10:33:42
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/dayinfo/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:36:09
---
Le package DayInfo vous permet de connaitre la fête du jour ainsi que les heures du lever et coucher du soleil pour une position GPS donnée.

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package DayInfo :

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-57.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-54.png" alt="image" width="350" height="213" border="0" /></a>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir votre position GPS (latitude et longitude) ainsi que votre “timezone”.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-58.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-55.png" alt="image" width="350" height="225" border="0" /></a></p>

Par exemple pour Paris:

<ul>
    <li>Timezone : 1</li>
    <li>Latitude : 48.866667</li>
    <li>Longitude : 2.333333</li>
</ul>

Pour avoir un calcul des horaires du soleil plus précis, entrez votre position GPS exacte. Pour connaitre la position GPS précise pour une adresse donnée : <a title="http://www.coordonnees-gps.fr/" href="http://www.coordonnees-gps.fr/">http://www.coordonnees-gps.fr/</a>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="DayInfo"&gt;
  &lt;settings&gt;
    &lt;setting key="TimeZone" value="1" /&gt;
    &lt;setting key="Latitude" value="48.866667" /&gt;
    &lt;setting key="Longitude" value="2.333333" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>

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
<td valign="top" width="10"><strong>TimeZone</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Timezone de votre position (ex: 1 pour la France)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Latitude</strong></td>
<td valign="top" width="10">Double</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Latitude GPS de votre position</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Longitude</strong></td>
<td valign="top" width="10">Double</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Longitude GPS de votre position</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez 2 StateObjects publiés chaque jour par le package :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>NameDay</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="446">Fête du jour</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SunInfo</strong></td>
<td valign="top" width="10">DayInfo.SunInfo</td>
<td valign="top" width="446">Information sur les horaires du soleil pour votre position GPS</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-59.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-56.png" alt="image" width="350" height="108" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 2 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetNameDay</strong></td>
<td valign="top" width="141">String</td>
<td valign="top" width="407">Récupère la fête du jour pour une date donnée</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetSunInfo</strong> <i></i></td>
<td valign="top" width="141">DayInfo.SunInfo</td>
<td valign="top" width="407">Récupère les informations sur les horaires du soleil pour une position et une date données</td>
</tr>
</tbody>
</table>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Exploiter les horaires du soleil dans vos algorithmes C#</li>
    <li>Afficher la fête jour dans vos Dashboards HTML ou WPF</li>
</ul>