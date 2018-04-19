---
ID: 3499
post_title: 'Rfxcom : captez vos sondes RF et pilotez des &eacute;quipements Somfy'
author: Sebastien Warin
post_date: 2016-10-26 13:56:21
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/rfxcom/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:30:37
---
Le package Rfxcom vous permet d’interagir avec l’émetteur/récepteur RFXCOM USB.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-128.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-118.png" alt="image" width="150" height="247" border="0" /></a></p>

<p align="left">Actuellement, le package en version 1.0 supporte :</p>

<ul>
    <li>Les sondes de température (Oregon THR128/138, THC138, THC238/268, THN132, THWR288, THRN122, THN122, AW129/131, THWR800, RTHN318, La Crosse TX2, TX3, TX4, TX17, WS2300, TS15C, Viking 02811, RUBiCSON, TFA 30.3133)</li>
    <li>Les sondes de température et d’humidité  (Oregon THGN122/123, THGN132, THGR122/228/238/268, THGR810, THGN800, THGR810, RTGR328, THGR328, WTGR800, THGR918/928, THGRN228, THGN500, TFA TS34C, Cresta, WT260,WT260H,WT440H,WT450,WT450H, Viking 02035,02038, Rubicson, EW109)</li>
    <li>L’envoi de commande RFY (pour le pilotage des périphérique Somfy)</li>
</ul>

Le code source est disponible sur : <a title="https://github.com/myconstellation/constellation-packages/tree/master/Rfxcom" href="https://github.com/myconstellation/constellation-packages/tree/master/Rfxcom">https://github.com/myconstellation/constellation-packages/tree/master/Rfxcom</a>

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Rfxcom :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-129.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-119.png" alt="image" width="354" height="217" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir le port COM sur lequel est connecté votre RFXcom :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-130.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-120.png" alt="image" width="350" height="281" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Rfxcom"&gt;
  &lt;settings&gt;
    &lt;setting key="PortName" value="COM3" /&gt;
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
<td valign="top" width="10"><strong>PortName</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="478">Le nom du port COM du RFXcom (ex: COM1)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Verbose</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="10">Optionnel
Par défaut : false</td>
<td valign="top" width="478">Indique si le package écrit tous les messages qu’il reçoit dans les logs Constellation</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ProtocolsEnabled</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="478">Liste des protocoles RF à activer (sépéré par des virgules). Exemple : “Oregon,X10”. Voir la liste <a href="http://www.rfxcom.com/WebRoot/StoreNL2/Shops/78165469/MediaGallery/Downloads/RFXtrx_User_Guide_-_FR.pdf">ici</a> à la page 8.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ForwardRawMessageToGroup</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="478">Indique le nom du groupe dans lequel transférer les messages reçu.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SensorStateObjectLifetime</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 300</td>
<td valign="top" width="478">Durée de vie en seconde pour les StateObjects publiés.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>StateObjectCustomNames</strong></td>
<td valign="top" width="10">JsonObject</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="478">Par défaut chaque StateObject est nommé par le type et numéro de série du capteur. Vous pouvez personnaliser le nom avec ce setting.</td>
</tr>
</tbody>
</table>

<h5>Personnaliser les noms des StateObjects</h5>

Le package publie un StateObject pour chaque capteur qu’il recoit. Chaque StateObject est composé du type de capteur (par exemple : “Temperature”) et de son identifiant.

Par exemple pour un capteur de température Oregon THR128 avec l’ID “21762”, le StateObject le représentant se nommera “TemperatureHumiditySensor_21762”.

Pour personnaliser les noms des StatObjects publiés vous pouvez définir un objet JSON dans le setting “<strong>StateObjectCustomNames</strong>” en indiquant le nom du capteur et son nom de StateObject :

<pre class="lang:javascript decode:true">{
  "SensorName": "CustomName",
  "SensorName2": "CustomName2"
}</pre>

Voici par exemple la personnalisation de plusieurs sondes Oregon :

<pre class="lang:javascript decode:true">{
    "TemperatureHumiditySensor_21762": "Capteur Bureau",
    "TemperatureHumiditySensor_64001": "Capteur Salle de bain",
    "TemperatureHumiditySensor_62468": "Capteur Buanderie",
    "TemperatureSensor_22529": "Temperature Chambre 3",
    "TemperatureSensor_36866": "Temperature Entrée",
    "TemperatureSensor_19716": "Temperature Cellier"
}</pre>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-131.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-121.png" alt="image" width="350" height="213" border="0" /></a></p>

Ainsi le capteur “TemperatureHumiditySensor_21762” est représenté dans la Constellation par le StateObject nommé “Capteur Bureau”.

Soyez particulièrement vigilant sur le fait que l’identifiant peut changer si le capteur n’a plus d’energie pendant plusieurs secondes (ce qui peut arriver lors du changement de pile). Il faudra alors mettre à jour la table de mapping pour que votre capteur conserve son nom de StateObject.

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que de trame de capteur RF  :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>RFXCOM</strong></td>
<td valign="top" width="10">RfxCom.Device</td>
<td valign="top" width="446">Représente l’état du périphérique RFXcom (nom, version, protocole activée, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du capteur ou ID &gt;&gt;</strong></td>
<td valign="top" width="10">RfxCom.TemperatureSensor</td>
<td valign="top" width="446">Représente l’état d’un capteur 0x50 (température)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du capteur ou ID &gt;&gt;</strong></td>
<td valign="top" width="10">RfxCom.TemperatureHumiditySensor</td>
<td valign="top" width="446">Représente l’état d’un capteur 0x52(température et humidité)</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-132.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-122.png" alt="image" width="350" height="152" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 4 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>RefreshStatus</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Rafraichit le statuts du RFXcom.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendMessage</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi un message en RF.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendRFYCommand</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Envoi une commande RFY (pour les équipements Somfy).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetProtocols</strong></td>
<td valign="top" width="141"><em>Aucune</em></td>
<td valign="top" width="407">Défini la liste des protocoles RF à activer (voir la liste <a href="http://www.rfxcom.com/WebRoot/StoreNL2/Shops/78165469/MediaGallery/Downloads/RFXtrx_User_Guide_-_FR.pdf">ici</a> à la page 8).</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-133.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-123.png" alt="image" width="350" height="206" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher les données de vos capteurs de température sur une page Web</li>
    <li>Envoyer une notification d’urgence en cas de de température trop haute depuis un package C#</li>
    <li>Piloter une prise Somfy depuis une page Web ou une application mobile multi-plateforme</li>
</ul>