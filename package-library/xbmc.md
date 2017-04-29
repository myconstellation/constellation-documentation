---
ID: 3574
post_title: 'Xbmc : int&eacute;grez vos media-centers XBMC/Kodi dans Constellation'
author: Sebastien Warin
post_date: 2016-10-28 10:28:12
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/xbmc/
published: true
post_modified: 2016-10-28 15:37:17
---
Le package XBMC/Kodi vous permet de connecter des hôtes XBMC/Kodi dans Constellation.
<p align="center"><a href="https://kodi.tv"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-156.png" alt="image" width="354" height="186" border="0" /></a></p>
<p style="text-align: left;" align="center">Le code source de ce package est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/xbmc">https://github.com/myconstellation/constellation-packages/tree/master/xbmc</a></p>

<h3>Installation</h3>
<h4>Prérequis : configuration de Kodi</h4>
Avant d’installer le package vous devez configurer votre media-center Kodi. Pour cela, rendez-vous dans les paramètres (System &gt; Settings) puis entrer dans l’onglet “Services”.

Sur la page “Serveur Web” vous devez activer le contrôle de Kodi par HTTP et définir un port (ex: 80) et optionnellement un nom d’utilisateur et un mot de passe.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-157.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-141.png" alt="image" width="350" height="192" border="0" /></a></p>
<p align="left">Sur la page “Contrôle à distance”, cocher la case permettant d’autoriser une application externe à contrôler votre media-center :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-158.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-142.png" alt="image" width="354" height="196" border="0" /></a></p>

<h4>Installation du package Constellation</h4>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Xbmc:
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-159.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-143.png" alt="image" width="350" height="214" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir sur la page de Settings vous devez obligatoirement spécifier la liste des hôtes XBMC/Kodi à connecter dans Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-160.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-144.png" alt="image" width="350" height="283" border="0" /></a></p>
<p align="left">Le schéma XML de ce paramètre est relativement simple. Vous devez simplement ajouter des balises “<em>&lt;xbmcHost&gt;</em>” dans la section ”&lt;hosts&gt;” pour chaque hôte Kodi que vous voulez connecter dans Constellation.</p>
<p align="left">Pour chaque “<em>&lt;xbmcHost&gt;</em>” vous devez obligatoirement définir les attributs “Name” (le nom unique de votre media-center qui servira d’identifiant dans Constellation), “host” l’IP ou DNS de votre Kodi et “port”, le n° du prot TCP du serveur Web de Kodi (configuré ci-dessus). Vous pouvez également spécifier un “login” et “password”.</p>
<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Xbmc"&gt;
  &lt;settings&gt;
    &lt;setting key="xbmcConfigurationSection"&gt;
      &lt;content&gt;
        &lt;xbmcConfigurationSection xmlns="urn:Xbmc"&gt;
          &lt;hosts&gt;
            &lt;xbmcHost name="XBMC Salon" host="pc-salon" port="80" login="xbmc" password="xbmc" /&gt;
            &lt;xbmcHost name="XBMC Chambre" host="pc-chambre" port="80" login="xbmc" password="xbmc" /&gt;
          &lt;/hosts&gt;
        &lt;/xbmcConfigurationSection&gt;
      &lt;/content&gt;
    &lt;/setting&gt;
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
<td valign="top" width="10"><strong>xbmcConfigurationSection</strong></td>
<td valign="top" width="10">ConfigurationSection</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Schéma XML de configuration des hôtes XBMC/Kodi</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Vous retrouverez autant de StateObjects que d’hôtes XBMC/Kodi enregistrés :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom du Media-Center &gt;&gt;</strong></td>
<td valign="top" width="10">Xbmc.XbmcState</td>
<td valign="top" width="446">Représente l’état du media-center. Le StateObject contient notamment une propriété “IsConnected” permettant de savoir si le media-center est connecté ainsi que les propriétés “PlayerState” (informations sur l’état du lecteur) et “PlayerItem” (informations sur le média en lecture)</td>
</tr>
</tbody>
</table>
<h4 align="left">Les MessageCallbacks</h4>
Le package expose 10 MessageCallbacks :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>OpenEpisode</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Lance un épisode d’une série de la médiathèque.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>OpenMovie</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Lance un film de la médiathèque.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PlayPause</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Play/Pause sur le média en cours.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ScanAudioLibrary</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Lance un scan de la médiathèque audio.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ScanVideoLibrary</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Lance un scan de la médiathèque vidéo.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendInputKey</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi une touche.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetMute</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Active ou désactive le mode muet.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetVolume</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini le volume.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ShowNotification</strong> <i></i></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Affiche une notification sur le media-center</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Stop</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Arrête la lecture du média en cours</td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-161.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-145.png" alt="image" width="350" height="497" border="0" /></a></p>

<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Afficher et contrôler le média en cours de lecture sur une page Web ou une application mobile multi-plateforme</li>
 	<li>Contrôler le media-center Kodi avec un Arduino-ESP et une simple télécommande infrarouge</li>
 	<li>Contrôler les lumières Hue automatiquement d’un film</li>
 	<li>Afficher des notifications sur le média-center  plutôt que sur le smartphone lorsque l’on regarde un film</li>
 	<li>Contrôler le “Play/Pause” à partir d’un bouton poussoir piloté par un .NET Gadgeteer</li>
</ul>