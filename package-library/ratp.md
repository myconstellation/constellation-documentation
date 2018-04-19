---
ID: 3476
post_title: 'Ratp : connaitre les horaires et l&rsquo;&eacute;tat du trafic RATP'
author: Sebastien Warin
post_date: 2016-10-26 08:54:50
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/ratp/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:31:13
---
Le package Rapt vous permet de connaitre l'état du trafic RAPT et les horaires.

<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/ratp.png"><img class="alignnone size-full wp-image-3478" style="background-image: none; padding-top: 0px; padding-left: 0px; padding-right: 0px; border: 0px;" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/ratp.png" alt="ratp" width="85" height="85" border="0" /></a></p>

Le code source est disponible sur : <a title="https://github.com/myconstellation/constellation-packages/tree/master/Ratp" href="https://github.com/myconstellation/constellation-packages/tree/master/Ratp">https://github.com/myconstellation/constellation-packages/tree/master/Ratp</a>

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Rapt :

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-125.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-115.png" alt="image" width="350" height="213" border="0" /></a>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Rapt" /&gt;</pre>

<h3>Détails du package</h3>

<h4>Les Settings</h4>

Ce package ne comporte aucun setting.

<h4>Les StateObjects</h4>

Ce package ne publie aucun StateObject

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
<td valign="top" width="10"><strong>GetStations</strong></td>
<td valign="top" width="141">Response&lt;StationList&gt;</td>
<td valign="top" width="407">Récupère les stations d'une ligne de Rer, Métro, Tramway, Bus ou Noctilien.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetSchedule</strong></td>
<td valign="top" width="141">Response&lt;ScheduleResult&gt;</td>
<td valign="top" width="407">Récupère les temps d'attente des prochains trains d'une ligne de Rer, Métro, Tramway, Bus ou Noctilien en fonction de la destination et de la station.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetTraffic</strong></td>
<td valign="top" width="141">Response&lt;TrafficResult&gt;</td>
<td valign="top" width="407">Récupère le trafic du réseau ferré RATP.</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-127.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-117.png" alt="image" width="350" height="219" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher les prochains trains d'une ligne sur une application mobile Ionic</li>
</ul>