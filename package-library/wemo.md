---
ID: 3524
post_title: 'Wemo : int&eacute;grez vos prises Belkin Wemo dans Constellation'
author: Sebastien Warin
post_date: 2016-10-26 15:20:42
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/wemo/
published: true
post_modified: 2016-10-28 13:55:12
---
<p>Le package Wemo vous permet de connecter des équipements Belkin/Wemo dans Constellation. </p> <p align="center"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-139.png" width="350" height="166"></p> <p align="left">Actuellement, dans la version 1.0 du package, seuls les prises connectés Wemo Switch et Wemo Insight sont supportées.</p> <p>Le code source est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/Wemo">https://github.com/myconstellation/constellation-packages/tree/master/Wemo</a>&nbsp; </p> <h3>Installation</h3> <p>Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Wemo:</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-140.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-128.png" width="350" height="214"></a></p> <p>Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.</p> <p align="center"></p> <p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p><pre class="lang:html5 decode:true">&lt;package name="Wemo" /&gt;</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="10"><u>Détail</u></td>
<td valign="top" width="478"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>DeviceQueryInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel<br>Par défaut : 1</td>
<td valign="top" width="478">Intervalle de temps en seconde pour interroger et rafraichir le statuts du périphérique Wemo dans les StateObjects Constellation (par défaut, toutes les secondes).</td></tr>
<tr>
<td valign="top" width="10"><strong>DiscoveryInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel<br>Par défaut : 600</td>
<td valign="top" width="478">Intervalle de temps en seconde pour lancer une découverte des nouveaux périphérique Wemo sur le réseau (par défaut, toutes les 600 secondes, soit 10 minutes).</td></tr></tbody></table>
<h4>Les StateObjects</h4>
<p>Vous retrouverez autant de StateObjects que périphérique Wemo découvert sur le réseau :</p>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; SerialNumber &gt;&gt;</strong></td>
<td valign="top" width="10">Wemo.WemoSwitch</td>
<td valign="top" width="446">Représente l’état d’une prise connectée Wemo</td></tr></tbody></table>
<h4 align="left">Les MessageCallbacks</h4>
<p>Le package expose 3 MessageCallbacks :</p>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>Discover</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Lance une découverte des nouveaux périphérique Wemo sur le réseau</td></tr>
<tr>
<td valign="top" width="10"><strong>SetSwitchState</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Défini le statuts (On/Off) d’une prise Wemo</td></tr></tbody></table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-141.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-129.png" width="350" height="102"></a></p>
<h3 align="left">Quelques exemples</h3>
<ul>
<li>Afficher l’état et contrôler chaque des prises Wemo depuis une application mobile multi-plateforme</li></ul>