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
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:30:16
---
Le package Vera vous permet de connecter la box domotique Z-Wave Vera dans Constellation. L’état des différents périphériques est publié en tant que StateObjects et des MessageCallbacks vous permettent de déclencher des scènes ou piloter les équipements.

<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-134.png" alt="image" width="240" height="166" border="0" /></p>

Le code source est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/Vera">https://github.com/myconstellation/constellation-packages/tree/master/Vera</a>

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Vera :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-135.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-124.png" alt="image" width="350" height="213" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir l’adresse IP (ou DNS) de votre Vera :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-136.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-125.png" alt="image" width="350" height="188" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Vera"&gt;
  &lt;settings&gt;
    &lt;setting key="VeraHost" value=192.168.x.x /&gt;
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
<td valign="top" width="478"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>VeraHost</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">L’IP ou le DNS de votre box domotique Vera</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que périphérique Z-Wave enregistré sur votre Vera :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Vera_&lt;&lt; SerialNumber &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.VeraDevice</td>
<td valign="top" width="446">Représente l’état de la Vera (modèle, S/N, version, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom de la scène &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.Scene</td>
<td valign="top" width="446">Représente l’état d’une scène</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.TemperatureSensor</td>
<td valign="top" width="446">Représente l’état d’un capteur de température</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.HumiditySensor</td>
<td valign="top" width="446">Représente l’état d’un capteur d’humidité</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.WindowCovering</td>
<td valign="top" width="446">Représente l’état d’un volet</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.DimmableLight</td>
<td valign="top" width="446">Représente l’état d’un Switch Dimmable</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.Switch</td>
<td valign="top" width="446">Représente l’état d’un Switch</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.PowerMeter</td>
<td valign="top" width="446">Représente l’état d’un capteur d’énergie</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du device &gt;&gt;</strong></td>
<td valign="top" width="10">VeraNet.SecuritySensor</td>
<td valign="top" width="446">Représente l’état d’un capteur de sécurité</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-137.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-126.png" alt="image" width="350" height="223" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 3 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>RunScene</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Déclenche une scène sur la Vera</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetDimmableLevel</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit le niveau (0-100%) d’un device “Dimmable”</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetSwitchState</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit le statuts (On/Off) d’un device “Switch”</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetWindowCoveringAction</strong> <i></i></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Définit l’ordre (montée, décente ou arrêt) d’un volet (WindowsCovering)</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-138.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-127.png" alt="image" width="350" height="210" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher l’état et contrôler chaque lampe et volet sur un Dashboard HTML</li>
    <li>Gérer les volets automatiquement en fonction de la luminosité avec un package C#</li>
    <li>Piloter des lampes Hue en suivant un schéma depuis un interrupteur Fibaro grâce à un package C#</li>
    <li>Piloter sa domotique Z-Wave depuis un montre Samsung Gear S2</li>
    <li>Synchroniser la lampe du bureau avec la session Windows</li>
</ul>