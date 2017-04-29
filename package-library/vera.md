---
ID: 3514
post_title: 'Vera : la domotique Z-Wave dans Constellation'
author: Sebastien Warin
post_date: 2016-10-26 14:34:30
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/vera/
published: true
post_modified: 2017-01-05 16:37:27
---
<p>Le package Vera vous permet de connecter la box domotique Z-Wave Vera dans Constellation. L’état des différents périphériques est publié en tant que StateObjects et des MessageCallbacks vous permettent de déclencher des scènes ou piloter les équipements.</p> <p align="center"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-134.png" width="240" height="166"></p> <p>Le code source est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/Vera">https://github.com/myconstellation/constellation-packages/tree/master/Vera</a></p> <h3>Installation</h3> <p>Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Vera :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-135.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-124.png" width="350" height="213"></a></p> <p>Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.</p> <p>Pour finir, sur la page de Settings, vous devez obligatoirement définir l’adresse IP (ou DNS) de votre Vera :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-136.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-125.png" width="350" height="188"></a></p> <p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p><pre class="lang:html5 decode:true">&lt;package name="Vera"&gt;
  &lt;settings&gt;
    &lt;setting key="VeraHost" value=192.168.x.x /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
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
<td valign="top" width="10"><strong>VeraHost</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">L’IP ou le DNS de votre box domotique Vera</td></tr></tbody></table>
<h4>Les StateObjects</h4>
<p>Vous retrouverez autant de StateObjects que périphérique Z-Wave enregistré sur votre Vera :</p>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>Vera_&lt;&lt; SerialNumber &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.VeraDevice</td>
<td valign="top" width="446">Représente l’état de la Vera (modèle, S/N, version, etc..)</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom de la scène &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.Scene</td>
<td valign="top" width="446">Représente l’état d’une scène</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.TemperatureSensor</td>
<td valign="top" width="446">Représente l’état d’un capteur de température</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.HumiditySensor</td>
<td valign="top" width="446">Représente l’état d’un capteur d’humidité</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.WindowCovering</td>
<td valign="top" width="446">Représente l’état d’un volet</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.DimmableLight</td>
<td valign="top" width="446">Représente l’état d’un Switch Dimmable</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.Switch</td>
<td valign="top" width="446">Représente l’état d’un Switch</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.PowerMeter</td>
<td valign="top" width="446">Représente l’état d’un capteur d’énergie</td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.SecuritySensor</td>
<td valign="top" width="446">Représente l’état d’un capteur de sécurité</td></tr></tbody></table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-137.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-126.png" width="350" height="223"></a></p>
<h4 align="left">Les MessageCallbacks</h4>
<p>Le package expose 3 MessageCallbacks :</p>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>RunScene</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Déclenche une scène sur la Vera</td></tr>
<tr>
<td valign="top" width="10"><strong>SetDimmableLevel</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit le niveau (0-100%) d’un device “Dimmable”</td></tr>
<tr>
<td valign="top" width="10"><strong>SetSwitchState</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit le statuts (On/Off) d’un device “Switch”</td></tr>
<tr>
<td valign="top" width="10"><strong>SetWindowCoveringAction</strong> <a href="https://skynet.ajsinfo.net/constellation/"><i></i></a></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit l’ordre (montée, décente ou arrêt) d’un volet (WindowsCovering)</td></tr></tbody></table>
<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-138.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-127.png" width="350" height="210"></a></h3>
<h3 align="left">Quelques exemples</h3>
<ul>
<li>Afficher l’état et contrôler chaque lampe et volet sur un Dashboard HTML 
<li>Gérer les volets automatiquement en fonction de la luminosité avec un package C# 
<li>Piloter des lampes Hue en suivant un schéma depuis un interrupteur Fibaro grâce à un package C# 
<li>Piloter sa domotique Z-Wave depuis un montre Samsung Gear S2 
<li>Synchroniser la lampe du bureau avec la session Windows </li></ul>