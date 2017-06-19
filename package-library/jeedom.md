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

<p align="center"><a href="http://i.imgur.com/dftrTld.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image002[4]" src="http://i.imgur.com/dftrTld.png" alt="clip_image002[4]" width="354" height="226" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings vous devez obligatoirement spécifier l’adresse URL ainsi que la clé API de votre installation Jeedom sans le « http:// » :

<p align="center"><a href="http://i.imgur.com/D5VLHMX.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/D5VLHMX.png" alt="clip_image004[4]" width="354" height="176" border="0" /></a></p>

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
<td valign="bottom" width="200"><u>Champ scene_id</u></td>
<td valign="bottom" width="259"><u>Description</u></td>
</tr>
<tr>
<td width="132">Start</td>
<td width="200"><i>Id du scénario</i></td>
<td width="259">Démarre le scénario</td>
</tr>
<tr>
<td width="132">Stop</td>
<td width="200"><i>Id du scénario</i></td>
<td width="259">Arrête le scénario</td>
</tr>
<tr>
<td width="132">Activer</td>
<td width="200"><i>Id du scénario</i></td>
<td width="259">Active le scénario</td>
</tr>
<tr>
<td width="132">Desactiver</td>
<td width="200"><i>Id du scénario</i></td>
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
<td width="167">Aucun</td>
<td width="259">Pour un équipement de type switch</td>
</tr>
<tr> 
<td width="132">Slider</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Valeur souhaitée</td>
<td width="167">Aucun</td>
<td width="259">Pour un équipement de type slider</td>
</tr>
<tr>
<td width="132">Message</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Titre du message</td>
<td width="167">Corps du message</td>
<td width="259">Pour un équipement de type message</td>
</tr>
<tr>
<td width="132">Color</td>
<td width="121"><i>Id de l'équipement</i></td>
<td width="167">Couleur souhaitée</td>
<td width="167">Aucun</td>
<td width="259">Pour un équipement de type color</td>
</tr>
</tbody>
</table>
<h3>Le plugin pour Jeedom (version 1.0)</h3>
<h4>Installation</h4>

Afin d’éviter de questionner Jeedom toutes les x secondes et pour obtenir les informations le plus rapidement possible, un plugin pour Jeedom a été développé.

Celui-ci vous permet d'envoyer toutes les informations d'un équipement quand une ou plusieurs informations de cet équipement se mettent à jour.

Le plugin peut être téléchargé à cette adresse : http://erwann.laville.free.fr/Jeedom/constellation.zip

Il vous suffit alors de l'extraire dans le dossier plugin de Jeedom. Une fois installé, vous pourrez l'activer dans la liste des plugins sur Jeedom :

<p align="center"><a href="http://i.imgur.com/JAqfqUX.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/JAqfqUX.png" width="354" height="176" border="0" /></a></p>

Une fois activé, vous aurez accès à la configuration générale du plugin. Il vous faudra indiquer l'url de Constellation (sans le http), le nom de la sentinelle et la clé créditential associée au plugin.

<p align="center"><a href="http://i.imgur.com/4RLHco7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/4RLHco7.png" width="354" height="176" border="0" /></a></p>

Par la suite vous pouvez ajouter autant d'équipement que souhaités. Chaque équipement créé correspondra à un package différent sur Constellation. Le nom de cet équipement correspondra au nom du package.

<p align="center"><a href="http://i.imgur.com/jytCq2H.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/jytCq2H.png" width="354" height="176" border="0" /></a></p>

<p align="center"><a href="http://i.imgur.com/eHOQm3f.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/eHOQm3f.png" width="354" height="176" border="0" /></a></p>

<h4>Les StateObjects</h4>

Le plugin Constellation pour Jeedom envoi un SO par équipement ajouté dans le "package". 

Si vous ajoutez plusieurs commandes d'un même équipement dans le package, les informations de l'équipement seront envoyés au même package dans Constellation à chaque mise à jour de chaque commandes indiquées.

Par exemple dans mon équipement Zwave, je rajoute #[Salle de Bains][Wall Plug][Etat]# et #[Salle de Bains][Wall Plug][Puissance]#. Les informations de mon Wall Plug seront envoyés à Constellation si l'état ou si la puissance changent.

<p align="center"><a href="http://i.imgur.com/Vn2H4I4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="http://i.imgur.com/Vn2H4I4.png" width="354" height="176" border="0" /></a></p>

Pour le moment, les SO ont comme nom  le chemin de l'équipement dans Jeedom, par exemple : [Salle de Bains][Wall Plug]

N'oubliez pas de rajouter dans Constellation la sentinel et le package, par exemple ici :
<pre class="lang:xhtml decode:true">&lt;sentinel name="Squeezebox" credential="Standard"&gt;
  &lt;packages&gt;
    &lt;package name="Zwave" /&gt;
  &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
<h4>Les Plugins compatiblent</h4>
Du fait du système utilisé (fonction listener) tous les plugins de Jeedom ne sont pas compatibles. Voici une liste non exhaustive des plugins Jeedom essayés  :
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="200" align="center"><u>Nom du plugin</u></td>
<td valign="bottom" width="100" align="center"><u>Compatible</u></td>
<td valign="bottom" width="100" align="center"><u>Non compatible</u></td>
</tr>
<tr>
<td valign="bottom" width="200" align="center">BLEA</td>
<td valign="bottom" width="100" align="center">X</td>
<td valign="bottom" width="100" align="center"></td>
</tr>
<tr>
<td valign="bottom" width="200" align="center">Espeasy</td>
<td valign="bottom" width="100" align="center">X</td>
<td valign="bottom" width="100" align="center"></td>
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
<td valign="bottom" width="200" align="center">RFXCom</td>
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
