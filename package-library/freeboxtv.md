---
ID: 3292
post_title: 'FreeboxTV : piloter votre FreeboxTV depuis Constellation'
author: Sebastien Warin
post_date: 2016-10-21 14:34:28
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/freeboxtv/
published: true
post_modified: 2016-10-25 13:36:32
---
Le package FreeboxTV vous permet de piloter votre Freebox TV / Player par Constellation.
<p align="center"><a href="http://www.freebox.fr"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-88.png" alt="image" width="350" height="197" border="0" /></a></p>
Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/FreeboxTV">https://github.com/myconstellation/constellation-packages/tree/master/FreeboxTV</a>
<h3>Installation</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package FreeboxTV :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-69.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-65.png" alt="image" width="350" height="216" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir la code de la télécommande réseau  de votre Freebox TV :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-70.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-66.png" alt="image" width="350" height="187" border="0" /></a></p>
<p align="left">Sur une Freebox révolution "Player", vous pouvez trouver le “code de la télécommande réseau” dans le menu réglage, puis télécommande, puis tout en bas à la ligne "télécommande réseau".</p>
<p align="left">Bien entendu vos  pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="FreeboxTV"&gt;
  &lt;settings&gt;
    &lt;setting key="RemoteKey" value="xxxxx" /&gt;
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
<td valign="top" width="10"><strong>RemoteKey</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Code de la télécommande réseau</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Ce package ne publie aucun StateObject.
<h4 align="left">Les MessageCallbacks</h4>
Le package expose un MessageCallback :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendCommand</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Envoi une commande à la FreeboxTV</td>
</tr>
</tbody>
</table>
<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-71.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-67.png" alt="image" width="350" height="95" border="0" /></a></h3>
<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Piloter votre FreeboxTV depuis une page Web</li>
 	<li>Piloter vitre FreeboxTV depuis un Arduino/ESP</li>
</ul>