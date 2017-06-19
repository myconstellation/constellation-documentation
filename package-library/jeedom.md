---
ID: 4912
post_title: 'Jeedom dans Constellation'
author: Hydro
post_date: 2017-06-18 21:25:00
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/jeedom/
published: true
post_modified: 2017-06-18 21:25:00
---
Le package Jeedom vous permet de contrôler vos équipements et vos scénarios Jeedom.

Cette documentation a été réalisé avec la version 1.0 du package Jeedom ainsi que la version 1.0 du plugin Constellation pour Jeedom.

Le code source est disponible sur <a title="https://github.com/myconstellation/constellation-packages/tree/master/Jeedom" href="https://github.com/myconstellation/constellation-packages/tree/master/Jeedom">https://github.com/myconstellation/constellation-packages/tree/master/Jeedom</a>
<h3>Installation du package Jeedom</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Jeedom :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0024.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image002[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0024_thumb.png" alt="clip_image002[4]" width="354" height="226" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings vous devez obligatoirement spécifier l’adresse URL ainsi que la clé API de votre installation Jeedom sans le « http:// » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0044.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0044_thumb.png" alt="clip_image004[4]" width="354" height="176" border="0" /></a></p>
Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :
<pre class="lang:xhtml decode:true">&lt;package name="Jeedom"&gt;
  &lt;settings&gt;
    &lt;setting key="ServerUrl" value="192.168.1.20" /&gt;
    &lt;setting key="ApiKey" value="123456abcdef" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;
</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="10"><u>Nom</u></td>
<td valign="bottom" width="10"><u>Type</u></td>
<td valign="bottom" width="10"><u>Détail</u></td>
<td valign="bottom" width="478"><u>Description</u></td>
</tr>
<tr>
<td valign="bottom" width="10">ServerUrl</td>
<td valign="bottom" width="10">String</td>
<td valign="bottom" width="10">Obligatoire</td>
<td valign="bottom" width="478">Adresse IP de Jeedom</td>
</tr>
<tr>
<td valign="bottom" width="10">ApiKey</td>
<td valign="bottom" width="10">String</td>
<td valign="bottom" width="10">Obligatoire</td>
<td valign="bottom" width="478">Clé API de Jeedom</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Ce package ne comporte aucuns StateObjects.
<h4>Les MessageCallbacks</h4>
Le package expose 2 types de MessageCallbacks :
<ul>
 	<li>SceneControl :</li>
</ul>
Ces Message Callbacks ne produisent aucunes réponses (saga).
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="132"><u>Nom</u></td>
<td valign="bottom" width="121"><u>Champ scene_id</u></td>
<td valign="bottom" width="259"><u>Description</u></td>
</tr>
<tr>
<td width="132">Start</td>
<td width="121"><i>Id du scénario</i></td>
<td width="259">Démarre le scénario</td>
</tr>
<tr>
<td width="132">Stop</td>
<td width="121"><i>Id du scénario</i></td>
<td width="259">Arrête le scénario</td>
</tr>
<tr>
<td width="132">Activer</td>
<td width="121"><i>Id du scénario</i></td>
<td width="259">Active le scénario</td>
</tr>
<tr>
<td width="132">Desactiver</td>
<td width="121"><i>Id du scénario</i></td>
<td width="259">Désactive le scénario</td>
</tr>
</tbody>
</table>
<ul>
 	<li>SendCommand :</li>
</ul>
Ces Message Callbacks ne produisent aucunes réponses (saga).
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="132"><u>Nom</u></td>
<td valign="bottom" width="121"><u>Champ id</u></td>
<td valign="bottom" width="167"><u>Champ value</u></td>
<td valign="bottom" width="167"><u>Champ value2</u></td>
<td valign="bottom" width="259"><u>Description</u></td>
</tr>
<tr>
<td width="132">Switch</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Aucun</td>
<td width="259">Aucun</td>
</tr>
<tr>
<td width="132">Slider</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Valeur souhaitée</td>
<td width="259">Aucun</td>
</tr>
<tr>
<td width="132">Message</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Titre du message</td>
<td width="259">Corps du message</td>
</tr>
<tr>
<td width="132">Color</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Couleur souhaitée</td>
<td width="259">Aucun</td>
</tr>
</tbody>
</table>
<h3>Le plugin pour Jeedom (version 1.0)</h3>
<h4>Installation</h4>
Afin d’éviter de questionner Jeedom toutes les x secondes et pour obtenir les informations le plus rapidement possible, un plugin pour Jeedom a été développé.

Pour l’installer, il faut rajouter le répertoire <a href="http://erwann.laville.free.fr/repo.xml">http://erwann.laville.free.fr/repo.xml</a> en bas de la page plugins de votre Logitech Media Server.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0064.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image006[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0064_thumb.png" alt="clip_image006[4]" width="354" height="49" border="0" /></a></p>
Une fois validé, un nouveau plugin sera disponible à l’installation. Il faudra le coche, valider et redémarrer pour l’installer.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0084.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image008[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0084_thumb.png" alt="clip_image008[4]" width="354" height="21" border="0" /></a></p>
Une fois installé, vous pourrez indiquer les informations de Constellation dans les paramètres du plugin :
<ul>
 	<li>L’adresse IP de Constellation sans http:// et sans le port</li>
 	<li>Le port de Constellation (par défaut 8088)</li>
 	<li>La clé API correspondant au package virtuel</li>
 	<li>Le nom de la sentinel dans laquelle sera publiée les informations (par défaut Squeezebox)</li>
 	<li>Le nom du package dans lequel sera publié les informations (par défaut Info)</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image010.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image010" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image010_thumb.png" alt="clip_image010" width="354" height="173" border="0" /></a></p>
Une fois tout indiqué, celui-ci va envoyer un message dans les logs de Constellation afin de tester la connexion.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image012.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image012" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image012_thumb.png" alt="clip_image012" width="354" height="92" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0134.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image013[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0134_thumb.png" alt="clip_image013[4]" width="354" height="106" border="0" /></a></p>
Vous pouvez alors rafraichir cette page de paramètre pour voir le résultat de cette communication (cela peut prendre jusqu’à 5 secondes).

Si le résultat est Ok, vous aurez « Communication Ok » dans la partie résultat.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0154.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image015[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0154_thumb.png" alt="clip_image015[4]" width="354" height="89" border="0" /></a></p>
S’il y a eu un problème de transmission, cette erreur s’affichera.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0174.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image017[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0174_thumb.png" alt="clip_image017[4]" width="354" height="90" border="0" /></a></p>
Vous aurez également en dernier l’url qu’utilisera LMS pour envoyer des informations à Constellation.

À noter qu’à la connexion avec Constellation, LMS réalise une purge de la Sentinel / du Package indiqués dans la configuration.

Voilà la configuration du plugin est terminé. Tous les StateObjects seront envoyés vers la sentinelle et le package indiqués.

Il faut donc les rajouter dans Constellation, par exemple ici :
<pre class="lang:xhtml decode:true">&lt;sentinel name="Squeezebox" credential="Standard"&gt;
  &lt;packages&gt;
    &lt;package name="Info" /&gt;
  &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
<h4>Les StateObjects</h4>
Vous retrouverez autant de StateObject que de lecteurs connectés au LMS ainsi qu’un StateObject indiquant la liste des Squeezebox connectées :
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="200" align="center"><u>Nom du plugin</u></td>
<td valign="bottom" width="100" align="center"><u>Fonctionne</u></td>
<td valign="bottom" width="100" align="center"><u>Fonctionne pas</u></td>
</tr>
<tr>
<td valign="bottom" width="200" align="center">Agenda</td>
<td valign="bottom" width="100" align="center"></td>
<td valign="bottom" width="100" align="center">X</td>
</tr>
<tr>
<td valign="bottom" width="200" align="center">Monitoring</td>
<td valign="bottom" width="100" align="center">X</td>
<td valign="bottom" width="100" align="center"></td>
</tr>
<tr>
<td valign="bottom" width="200" align="center">Zwave</td>
<td valign="bottom" width="100" align="center">X</td>
<td valign="bottom" width="100" align="center"></td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image019.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image019" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image019_thumb.png" alt="clip_image019" width="354" height="257" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image021.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image021" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image021_thumb.png" alt="clip_image021" width="354" height="257" border="0" /></a></p>
