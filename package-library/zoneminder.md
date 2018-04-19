---
ID: 3559
post_title: 'ZoneMinder : intégrez la vidéo-surveillance dans Constellation'
author: Sebastien Warin
post_date: 2016-10-28 10:01:53
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/zoneminder/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:26:13
---
Le package ZoneMinder vous permet de connecter un serveur ZoneMinder dans Constellation.

<p align="center"><a href="https://zoneminder.com/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-148.png" alt="image" width="240" height="123" border="0" /></a></p>

<h3>Installation</h3>

<h4>Prérequis : ZoneMinder</h4>

Avant d’installer le package vous devez configurer votre serveur ZoneMinder.

Tout d’abord vous devez disposer de la version 1.29 de ZoneMinder. C’est à partir de cette version que ZM a intégré une API REST exploitée par la package Constellation. Sans cette API impossible de se connecter à votre serveur ZoneMinder.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-149.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-134.png" alt="image" width="350" height="36" border="0" /></a></p>

<p align="left">Ensuite dans les options de ZoneMinder, veuillez définir les paramètres suivants :</p>

<ul>
    <li>
<div align="left">AUTH_TYPE : builtin</div></li>
    <li>
<div align="left">AUTH_HASH_SECRET : une chaine de caractère aléatoire</div></li>
    <li>
<div align="left">AUTH_HASH_LOGINS : YES</div></li>
    <li>
<div align="left">OPT_USE_API : YES</div></li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-150.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-135.png" alt="image" width="350" height="232" border="0" /></a></p>

<p align="left">Dans votre navigateur après vous être identifier sur votre serveur ZM, aller sur l’URI : <a href="http://server/zm/api/host/getVersion.json">http://server/zm/api/host/getVersion.json</a></p>

<p align="left">Si votre serveur est opérationnel vous devriez voir le JSON suivant :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-151.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-136.png" alt="image" width="172" height="74" border="0" /></a></p>

<p align="left">Pour finir vous devez récupérer le HASH de votre mot de passe ZoneMinder. Le mot de passe est hasher avec la fonction “PASSWORD” du serveur MySQL configuré pour ZoneMinder.</p>

VOus devez donc vous connectr sur votre serveur MySQL est tapper la commande :

<pre class="lang:sql decode:true">mysql &gt; SELECT PASSWORD("xxx");</pre>

Où “xxxx” est votre mot de passe du compte ZoneMinder utilisé.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-152.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-137.png" alt="image" width="350" height="292" border="0" /></a></p>

<p align="left">Par exemple sur mon serveur MySQL, le hash du mot de passe “Password123!” est “*4F56EF3FCEF3F995F03D1E37E2D692D420111476”.</p>

<h4>Installation du package Constellation</h4>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package ZoneMinder :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-153.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-138.png" alt="image" width="350" height="214" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir sur la page de Settings vous devez obligatoirement spécifier :

<ul>
    <li>RootUri : URI “racine” de votre serveur ZoneMinder (ex: <a href="http://server/zm">http://server/zm</a>)</li>
    <li>Username : nom d’utilisateur ZoneMinder</li>
    <li>PasswordHash : le hash du password de l’utilsateur (récupéré via la fonction”PASSWORD” sur votre serveur MySQL)</li>
    <li>SecretHash : la chaine de caractère aléatoire spécifiée dans les options de ZoneMinder (paramètre AUTH_HASH_SECRET)</li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-154.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-139.png" alt="image" width="350" height="292" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="ZoneMinder"&gt;
  &lt;settings&gt;
    &lt;setting key="RootUri" value="http://server/zm/" /&gt;
    &lt;setting key="Username" value="admin" /&gt;
    &lt;setting key="PasswordHash" value="*4F56EF3FCEF3F995F03D1E37E2D692D420111476" /&gt;
    &lt;setting key="SecretHash" value="T9TyLzZ7yJBvmKx8TyLefhVVtz8AUZXf" /&gt;
    &lt;setting key="SystemRefreshInterval" value="5000" /&gt;
    &lt;setting key="EventsRefreshInterval" value="2000" /&gt;
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
<td valign="top" width="10"><strong>RootUri</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">URI “racine” de votre serveur ZoneMinder (ex: <a href="http://server/zm">http://server/zm</a>)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Username</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Nom d’utilisateur ZoneMinder utilisé pour se connecter au serveur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PasswordHash</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Le hash du mot de passe de l’utilisateur (récupéré via la fonction”PASSWORD” sur votre serveur MySQL)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SecretHash</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">La chaine de caractère aléatoire spécifiée dans les options de ZoneMinder (paramètre AUTH_HASH_SECRET)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SystemRefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut :  5000</td>
<td valign="top" width="478">L’intervalle de temps en milliseconde pour rafraichir le StateObject représentant le serveur ZoneMinder (par défaut 5 secondes)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>MonitorsRefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut :  2000</td>
<td valign="top" width="478">L’intervalle de temps en milliseconde pour rafraichir les StateObjects représentant les cameras ZoneMinder (par défaut 2 secondes)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>EventsRefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut :  2000</td>
<td valign="top" width="478">L’intervalle de temps en milliseconde pour interroger les évènements ZoneMinder (par défaut 2 secondes)</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que de caméras ZoneMinder ainsi qu’un StateObject sur le serveur ZoneMinder :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Host</strong></td>
<td valign="top" width="10">ZoneMinder.Host</td>
<td valign="top" width="446">Représente l’état du serveur ZoneMinder (URI, Version, espace disque utilisé, charge CPU, etc…)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; Nom de la camera &gt;&gt;</strong></td>
<td valign="top" width="10">ZoneMinder.Monitor</td>
<td valign="top" width="446">Représente l’état d’une caméra (ID, nom, type, fonction, taille, URI du flux vidée, état, nombre d’évènements, etc..)</td>
</tr>
</tbody>
</table>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 5 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>CancelForcedAlarm</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Annule l’alarme sur une caméra donnée.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ChangeState</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Change l’état de ZoneMinder (Start/Stop).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ForceAlarm</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Force l’alarme sur une caméra donnée.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Restart</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Redémarre le service ZoneMinder.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetMonitorFunction</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini la fonction d’une caméra (Monitor, Record, Modect, etc…)</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-155.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-140.png" alt="image" width="350" height="290" border="0" /></a></p>

<p align="left">Le package envoi également un message “Alarm” au groupe “Motion” à chaque évènement levé par les caméras.</p>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher le flux des caméras dans une page Web ou dans une application mobile multi-plateforme</li>
    <li>Activer la détection des mouvements des caméras en fonction du statuts de l’alarme</li>
    <li>Force l’enregistrement d’une camera lorsque l’on ouvre une porte ou une fenêtre</li>
    <li>Allumer la lumière en cas de mouvement détecté par une camera</li>
    <li>Envoyer une capture vidéo sur son Smartphone lorsqu’une porte est restée ouverte trop longtemps</li>
</ul>