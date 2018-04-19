---
ID: 3583
post_title: 'WindowsControl : contrôlez vos ordinateurs Windows depuis Constellation'
author: Sebastien Warin
post_date: 2016-10-28 10:42:19
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/windowscontrol/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:24:52
---
Le package WindowsControl vous permet de contrôler l’état de vos ordinateurs Windows (mise en veille, hibernation, arrêt/redémarrage, verrouillage de la session, contrôle du volume ou de la luminosité …).

Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/WindowsControl">https://github.com/myconstellation/constellation-packages/tree/master/WindowsControl</a>
<h3>Installation</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package WindowsControl :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-162.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-146.png" alt="image" width="350" height="214" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :
<pre class="lang:html5 decode:true">&lt;package name="WindowsControl" /&gt;</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
Ce package ne comporte aucun setting.
<h4>Les StateObjects</h4>
Vous retrouverez le StateObject suivant :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>SessionLocked</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="446">Indique si la session est actuellement verrouillée ou non. Le package doit être déployé sur une sentinelle “UI” afin d’être lancé au sein d’une session Windows autrement ce StateObject ne sera pas publié.</td>
</tr>
</tbody>
</table>
<h4 align="left">Les MessageCallbacks</h4>
Le package expose 6 MessageCallbacks :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Hibernate</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Mise en hibernation de la machine.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>LockWorkStation</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Verrouille la session utilisateur (uniquement si le package est déployé dans une sentinelle UI)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>LogoffSession</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Ferme la session utilisateur (uniquement si le package est déployé dans une sentinelle UI)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Reboot</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Redémarre la machine.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Shutdown</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Arrêt la machine.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Sleep</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Mise en veille de la machine.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Mute</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Active ou désactive le mode “Muet”.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>VolumeUp</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Augmente le volume.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>VolumeDown</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Diminue le volume.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetBrightness</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Définit la luminosité de l’écran (valeur entre  0 et 100).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>BrightnessUp</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Augmente la luminosité de l’écran.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>BrightnessDown</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Diminue la luminosité de l’écran.</td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-163.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-147.png" alt="image" width="350" height="268" border="0" /></a></p>

<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Synchroniser la lampe du bureau avec l’état de la session Windows</li>
 	<li>Eteindre toutes ses machines Windows avec un script Powershell</li>
 	<li>Mise en veille automatique d’une machine en cas d’absence de mouvement</li>
</ul>