---
ID: 3614
post_title: 'Hue : connectez vos lampes Hue dans Constellation'
author: Sebastien Warin
post_date: 2016-10-28 13:27:35
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/hue/
published: true
post_modified: 2017-04-29 09:08:28
---
Le  package Hue permet de connecter et contrôler vos différentes lampes connectées <a href="http://www2.meethue.com/fr-fr/">Philips Hue</a> dans Constellation.

<p align="center"><a href="http://www2.meethue.com/fr-fr/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-166.png" alt="image" width="350" height="175" border="0" /></a></p>

<p align="left">Vous devez disposer d’un <a href="http://www2.meethue.com/fr-fr/productdetail/philips-hue-bridge">pont Philips Hue</a> (Hue Bridge) pour pouvoir utiliser ce package.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-167.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-150.png" alt="image" width="350" height="171" border="0" /></a></p>

<p style="text-align: left;" align="center">Le code source de ce package est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/Hue">https://github.com/myconstellation/constellation-packages/tree/master/Hue</a></p>

<h3>Installation</h3>

<h4>Prérequis : créer un utilisateur sur le pont Philips Hue</h4>

Identifiez tout d’abord l’adresse IP (ou DNS) de votre pont Philips (avec un scan UPnP, en consultant les baux de votre serveur DHCP ou avec le client mobile officiel de Hue).

En entrant l’adresse de votre navigateur, vous devrez tomber sur la home page du bridge :

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-168.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-151.png" alt="image" width="350" height="308" border="0" /></a>

Rendez-vous maintenant sur l’outil intégré de l’exécution de requête HTTP à l’adresse : http://&lt;bridge ip address&gt;/debug/clip.html

<p align="center"> <a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-169.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-152.png" alt="image" width="350" height="308" border="0" /></a></p>

<p align="left">Dans le corps de la requête (Request Body) entrez le JSON suivant :</p>

<pre class="lang:javascript decode:true">{"devicetype": "constellation#connector"}</pre>

Avant d’exécuter la requête, appuyez sur le bouton “Link” sur le Bridge :

<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-170.png" alt="image" width="240" height="176" border="0" /></p>

<p align="left">Vous disposez maintenant de 30 secondes pour enregistrer votre utilisateur. Exécutez donc la requête sans tarder en cliquant sur le bouton “POST”.</p>

<p align="left">En retour (Commande Response) vous obtiendrez l’identifiant de votre utilisateur, ici nommé “83b7780291a6ceffbe0bd049104df” :</p>

<pre class="lang:javascript decode:true">[{"success":{"username": "83b7780291a6ceffbe0bd049104df"}}]</pre>

<p align="left">Retenez cet identifiant, c’est cela qu’utilisera le package Constellation pour se connecter à vos Hue.</p>

Pour plus d’informations : <a title="http://www.developers.meethue.com/documentation/configuration-api#71_create_user" href="http://www.developers.meethue.com/documentation/configuration-api#71_create_user">http://www.developers.meethue.com/documentation/configuration-api#71_create_user</a>

<h4>Installation du package Constellation</h4>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Hue :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-171.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-153.png" alt="image" width="350" height="211" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings vous devez obligatoirement spécifier l’adresse de votre Bridge Hue (IP ou DNS) ainsi que votre utilisateur créé ci-dessus.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-172.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-154.png" alt="image" width="350" height="214" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Hue"&gt;
  &lt;settings&gt;
    &lt;setting key="BridgeAddress" value="192.168.x.x" /&gt;
    &lt;setting key="BridgeUsername" value="83b7780291a6ceffbe0bd049104df" /&gt;
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
<td valign="top" width="10"><strong>BridgeAddress</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Adresse IP ou DNS du pont Hue.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>BridgeUsername</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Identifiant de l’utilisateur utilisé pour se connecter sur le pont Hue.</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que de lampes Hue appariées sur le pont ainsi qu’un StateObject pour la configuration du pont :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>BridgeCondig</strong></td>
<td valign="top" width="10">Q42.HueApi.BridgeConfig</td>
<td valign="top" width="446">Représente la configuration du pont Hue (nom, adresse MAC et IP, config réseau, versions, liste des utilisateurs, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom de la lampe &gt;&gt;</strong></td>
<td valign="top" width="10">Q42.HueApi.Light</td>
<td valign="top" width="446">Représente l’état d’une lampe Hue (ID, état On/Off, luminosité, couleur Hue, effet, alerte, modèle, version logicielle, etc…)</td>
</tr>
</tbody>
</table>

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-173.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-155.png" alt="image" width="350" height="160" border="0" /></a>

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
<td valign="top" width="10"><strong>SendCommandTo</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi une commande à une ou plusieurs lampes</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Set</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi une commande à une lampe (état, couleur, intensité)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetBrightness</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini l’intensité d’une lampe</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetColor</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini la couleur d’une lampe</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetCommandToAll</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi une commande à toutes les lampes</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetState</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini l’état (On/Off) d’une lampe</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ShowAlert</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Affiche une alerte sur une lampe</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-174.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-156.png" alt="image" width="350" height="277" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Tamiser les lumières lors du démarrage d’un film</li>
    <li>Piloter les lumières en fonction de la luminosité ambiante</li>
    <li>Piloter des configurations de lumières avec un interrupteur Legrand connecté à un module Fibaro</li>
    <li>Afficher une alerte sur une lampe lorsqu’il pleut ou que quelqu’un sonne à la porte</li>
</ul>