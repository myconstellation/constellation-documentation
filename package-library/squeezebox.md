---
ID: 4924
post_title: 'Squeezebox : le multiroom connect&eacute;e dans Constellation'
author: Sebastien Warin
post_date: 2017-05-21 10:36:44
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/squeezebox/
published: true
post_modified: 2017-05-22 17:58:35
---
Le package Squeezebox vous permet de contrôler vos Squeezebox via le Logitech Media Server (lecture, pause, suivant, contrôle du volume …).

Cette documentation a été réalisé avec la version 1.1 du package Squeezebox ainsi que la version 2.0 du plugin Constellation pour Logitech Media Server.

Le code source est disponible sur <a title="https://github.com/myconstellation/constellation-packages/tree/master/Squeezebox" href="https://github.com/myconstellation/constellation-packages/tree/master/Squeezebox">https://github.com/myconstellation/constellation-packages/tree/master/Squeezebox</a>
<h3>Installation du package Squeezebox</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Squeezebox :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0024.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image002[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0024_thumb.png" alt="clip_image002[4]" width="354" height="226" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings vous devez obligatoirement spécifier l’adresse URL ainsi que le port du Logitech Media Server sans le « http:// » :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0044.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image004[4]" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image0044_thumb.png" alt="clip_image004[4]" width="354" height="176" border="0" /></a></p>
Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :
<pre class="lang:xhtml decode:true">&lt;package name="Squeezebox"&gt;
  &lt;settings&gt;
    &lt;setting key="ServerUrl" value="192.168.1.10:9000" /&gt;
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
<td valign="bottom" width="478">Adresse IP du LMS avec le port</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Ce package ne comporte aucuns StateObjects.
<h4>Les MessageCallbacks</h4>
Le package expose 2 types de MessageCallbacks :
<ul>
 	<li>SendToServer :</li>
</ul>
Ces Message Callbacks ne produisent aucunes réponses (saga).
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="144"><u>Nom</u></td>
<td valign="bottom" width="335"><u>Description</u></td>
</tr>
<tr>
<td width="144"><b>Scan_Cancel</b></td>
<td width="335">Annule le scan de la librairie</td>
</tr>
<tr>
<td width="144"><b>Scan_Fast</b></td>
<td width="335">Lance un scan rapide de la librairie</td>
</tr>
<tr>
<td width="144"><b>Scan_Full</b></td>
<td width="335">Lance un scan complet de la librairie</td>
</tr>
</tbody>
</table>
<ul>
 	<li>SendToSqueezebox :</li>
</ul>
Le champs Squeezebox correspond au nom ou à l’adresse MAC de la squeezebox cible.

Si ce champ est laissé vide, la commande sera lancée sur toutes les squeezebox.

Vous pouvez également indiquer plusieurs squeezebox en les séparant par une virgule, exemple : « SdB,Salon » ou « e4:f4:c6:47:5e:2a,00:15:5d:01:6a:04 »

Ces Message Callbacks ne produisent aucunes réponses (saga).
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="bottom" width="132"><u>Nom</u></td>
<td valign="bottom" width="121"><u>Champ value</u></td>
<td valign="bottom" width="167"><u>Champ squeezebox</u></td>
<td valign="bottom" width="259"><u>Description</u></td>
</tr>
<tr>
<td width="132">Add_Album_Id</td>
<td width="121"><i>Id de l’album</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Ajoute un album par son id à la fin de la playlist</td>
</tr>
<tr>
<td width="132">Add_Artist_Id</td>
<td width="121"><i>Id de l’artiste</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Ajoute un artiste par son id à la fin de la playlist</td>
</tr>
<tr>
<td width="132">Add_Title_Id</td>
<td width="121"><i>Id du titre</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Ajoute un titre par son id à la fin de la playlist</td>
</tr>
<tr>
<td width="132">Delete_Album_Id</td>
<td width="121"><i>Id de l’album</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Supprime les titres de l’album de la playlist</td>
</tr>
<tr>
<td width="132">Delete_Artist_Id</td>
<td width="121"><i>Id de l’artiste</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Supprime les titres de l’artiste de la playlist</td>
</tr>
<tr>
<td width="132">Delete_Title_Id</td>
<td width="121"><i>Id du titre</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Supprime le titre de la playlist</td>
</tr>
<tr>
<td width="132">Connect</td>
<td width="121"><i>Adresse IP de l’autre serveur LMS</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox sur l’autre serveur LMS</i></td>
<td width="259">Connecte la squeezebox d’un autre serveur à ce serveur</td>
</tr>
<tr>
<td width="132">Connect_To</td>
<td width="121"><i>Adresse IP de l’autre serveur LMS</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Connecte une squeezebox de ce serveur à un autre serveur</td>
</tr>
<tr>
<td valign="top" width="132">Mute_Off</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Désactive l’option muet</td>
</tr>
<tr>
<td valign="top" width="132">Mute_On</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active l’option muet</td>
</tr>
<tr>
<td valign="top" width="132">Mute_Toggle</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active/désactive l’option muet</td>
</tr>
<tr>
<td valign="top" width="132">Next</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance la prochaine musique dans la playlist</td>
</tr>
<tr>
<td valign="top" width="132">Pause</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Met sur pause la musique</td>
</tr>
<tr>
<td valign="top" width="132">Play</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Met en lecture la musique</td>
</tr>
<tr>
<td valign="top" width="132">Play_Album</td>
<td width="121"><i>Nom de l’album</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un album basé sur son nom

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Album_Id</td>
<td width="121"><i>Id de l’album</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un album basé sur son id

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Artist</td>
<td width="121"><i>Nom de l’artiste</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un artiste basé sur son nom

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Artist_Id</td>
<td width="121"><i>Id de l’artiste</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un artiste basé sur son id

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Index</td>
<td width="121"><i>Index du titre dans la playlist</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance une musique par sa position dans la playlist</td>
</tr>
<tr>
<td valign="top" width="132">Play_Playlist</td>
<td width="121"><i>Nom de la playlist</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance une playlist basée sur son nom

(nom de la playlist requis – remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Playlist_Id</td>
<td width="121"><i>Id de la playlist</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance une playlist basée sur son id

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Title</td>
<td width="121"><i>Nom du titre</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un titre basé sur son nom

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Title_Id</td>
<td width="121"><i>Id du titre</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un titre basé sur son id

(remplace la playlist en cours)</td>
</tr>
<tr>
<td valign="top" width="132">Play_Title_Id_Next</td>
<td width="121"><i>Id du titre</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Ajoute un titre à la prochaine position de la playlist basé sur son id</td>
</tr>
<tr>
<td valign="top" width="132">Play_Toggle</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Alterne pause et lecture de la musique</td>
</tr>
<tr>
<td valign="top" width="132">Playlist_Clear</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Vide la playlist en cours</td>
</tr>
<tr>
<td valign="top" width="132">Power_Off</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Éteint virtuellement la squeezebox</td>
</tr>
<tr>
<td valign="top" width="132">Power_On</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Allume virtuellement la squeezebox</td>
</tr>
<tr>
<td valign="top" width="132">Power_Toggle</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Alterne éteindre/allumer virtuellement la squeezebox</td>
</tr>
<tr>
<td valign="top" width="132">Previous</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance la musique précédente dans la playlist</td>
</tr>
<tr>
<td valign="top" width="132">Random_Album</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un mix aléatoire par album</td>
</tr>
<tr>
<td valign="top" width="132">Random_Artist</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un mix aléatoire par artiste</td>
</tr>
<tr>
<td valign="top" width="132">Random_Title</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un mix aléatoire</td>
</tr>
<tr>
<td valign="top" width="132">Random_Year</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Lance un mix aléatoire par année</td>
</tr>
<tr>
<td valign="top" width="132">Repeat_Off</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Désactive la répétition</td>
</tr>
<tr>
<td valign="top" width="132">Repeat_Playlist</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active la répétition de la playlist</td>
</tr>
<tr>
<td valign="top" width="132">Repeat_Title</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active la répétition du titre en cours</td>
</tr>
<tr>
<td valign="top" width="132">Repeat_Toggle</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Alterne les modes de répétition</td>
</tr>
<tr>
<td valign="top" width="132">Shuffle_Album</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active le mélange par album</td>
</tr>
<tr>
<td valign="top" width="132">Shuffle_Off</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Désactive le mélange</td>
</tr>
<tr>
<td valign="top" width="132">Shuffle_Title</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Active le mélange par titre</td>
</tr>
<tr>
<td valign="top" width="132">Shuffle_Toggle</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Alterne les modes de mélange</td>
</tr>
<tr>
<td valign="top" width="132">Stop</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Stop la musique en cours de lecture</td>
</tr>
<tr>
<td valign="top" width="132">Sync</td>
<td width="121"><i>Nom ou adresse MAC de la squeezebox qui sera synchronisée</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Synchronise une autre squeezebox avec celle-ci</td>
</tr>
<tr>
<td valign="top" width="132">Sync_Off</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Désynchronise la squeezebox</td>
</tr>
<tr>
<td valign="top" width="132">Sync_To</td>
<td width="121"><i>Nom ou adresse MAC de la squeezebox « maître »</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Synchronise cette squeezebox à une autre</td>
</tr>
<tr>
<td valign="top" width="132">Volume</td>
<td width="121"><i>Niveau du volume</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Définie le niveau du volume</td>
</tr>
<tr>
<td valign="top" width="132">Volume_Down</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Baisse le volume de 2%</td>
</tr>
<tr>
<td valign="top" width="132">Volume_Up</td>
<td width="121"><i>Aucun</i></td>
<td width="167"><i>Nom ou adresse MAC de la squeezebox cible</i></td>
<td width="259">Augmente le volume de 2%</td>
</tr>
</tbody>
</table>
<h3>Le plugin Logitech Media Server (version 2.0)</h3>
<h4>Installation</h4>
Afin d’éviter de questionner le Logitech Media Server toutes les x secondes et pour obtenir les informations le plus rapidement possible, un plugin pour le Logitech Media Server a été développé.

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
<td valign="bottom" width="71"><u>Nom</u></td>
<td valign="bottom" width="56"><u>Type</u></td>
<td valign="bottom" width="476"><u>Description</u></td>
</tr>
<tr>
<td valign="bottom" width="71">Players</td>
<td valign="bottom" width="56">N/A</td>
<td valign="bottom" width="476">Liste des lecteurs avec pour chacun le nom et l’adresse MAC.</td>
</tr>
<tr>
<td valign="bottom" width="71">&lt;&lt; Nom du lecteur &gt;&gt;</td>
<td valign="bottom" width="56">N/A</td>
<td valign="bottom" width="476">Informations sur le lecteur (nom, adresse MAC, adresse IP…) ainsi que sur la lecture en cours (titre, album, artiste, volume, pochette…)</td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image019.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image019" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image019_thumb.png" alt="clip_image019" width="354" height="257" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image021.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image021" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/clip_image021_thumb.png" alt="clip_image021" width="354" height="257" border="0" /></a></p>