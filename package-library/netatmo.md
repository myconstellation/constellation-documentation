---
ID: 3355
post_title: 'NetAtmo : vos capteurs NetAtmo dans votre Constellation'
author: Sebastien Warin
post_date: 2016-10-21 16:23:47
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/netatmo/
published: true
post_modified: 2016-10-25 13:35:07
---
Le package NetAtmo permet de connecter votre station météo NetAtmo dans Constellation.

La version actuelle du package ne gère que les stations météo et modules associées et non les autres objets NetAtmo tel que le thermostat ou la caméra Welcome.
<p align="center"><a href="http://www.netatmo.com"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-89.png" alt="image" width="240" height="167" border="0" /></a></p>
<p align="left">Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/NetAtmo">https://github.com/myconstellation/constellation-packages/tree/master/NetAtmo</a></p>

<h3>Installation</h3>
<h4>Prérequis : créer une application NetAtmo</h4>
Pour pouvoir utiliser le service NetAtmo vous devez créer un compte développeur sur : <a title="https://dev.netatmo.com/" href="https://dev.netatmo.com/">https://dev.netatmo.com/</a>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-90.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-82.png" alt="image" width="350" height="270" border="0" /></a></p>
Une fois inscrit, créer une application en cliquant sur “Create an app” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-91.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-83.png" alt="image" width="350" height="270" border="0" /></a></p>
<p align="left">Entrez les informations pour votre connecteur :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-92.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-84.png" alt="image" width="350" height="270" border="0" /></a></p>
<p align="left">Puis en enregistrant, vous obtiendrez un peu plus bas sur cette page, le “Client ID” et “Client Secret” de votre application NetAtmo qui servira pour la configuration du package Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-93.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-85.png" alt="image" width="350" height="270" border="0" /></a></p>

<h4>Installation du package Constellation</h4>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package NetAtmo :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-94.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-86.png" alt="image" width="350" height="216" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez définir différents paramètres :
<ul>
 	<li><u>Netatmo.ClientId</u> : le “Client ID” de votre application NetAtmo créée précédemment</li>
 	<li><u>Netatmo.ClientSecret</u> : le “Client Secret” de votre application NetAtmo créée précédemment</li>
 	<li><u>Netatmo.Username</u> : votre utilisateur NetAtmo</li>
 	<li><u>Netatmo.Password</u> : votre mot de passe NetAtmo pour l’utilisateur utilisé</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-95.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-87.png" alt="image" width="350" height="264" border="0" /></a></p>
<p align="left">Vous pouvez aussi redéfinir le setting “RefreshInterval” qui représente l’intervalle de temps en seconde de rafraichissement des données de votre station météo sur le service NetAtmo. Par défaut ce paramètre est fixé à 300 secondes soit 5 minutes. Notez par ailleurs que votre station météo NetAtmo remonte ses mesures toutes les 5 minutes.</p>
Bien entendu vos  pouvez également déployer ce package manuellement dans la configuration de votre Constellation :
<pre class="lang:html5 decode:true">&lt;package name="NetAtmo"&gt;
  &lt;settings&gt;
    &lt;setting key="Netatmo.ClientId" value="xxxxxx" /&gt;
    &lt;setting key="Netatmo.ClientSecret" value="xxxxx" /&gt;
    &lt;setting key="Netatmo.Username" value="xxx@xxx.com" /&gt;
    &lt;setting key="Netatmo.Password" value="xxxxxx" /&gt;
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
<td valign="top" width="10"><strong>Netatmo.ClientId</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Client ID de l’application NetAtmo associée</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Netatmo.ClientSecret</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Client Secret de l’application NetAtmo associée</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Netatmo.Username</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Nom d’utilisateur du compte NetAtmo</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Netatmo.Password</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Mot de passe de l’utilisateur NetAtmo</td>
</tr>
<tr>
<td valign="top" width="10"><strong>RefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 300</td>
<td valign="top" width="456">Intervalle de temps en seconde d’interrogation du service NetAtmo</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Vous retrouverez autant de StateObjects que vous avez de mesures pour chaque modules connectés à votre station. Ces StateObjects sont mis à jour pour l’intervalle défini dans la configuration (toutes les 5 minutes par défaut).

Par exemple
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom de la station &gt;&gt;.Info</strong></td>
<td valign="top" width="10">NetAtmo.Device</td>
<td valign="top" width="446">Description de la station NetAtmo (IP, firmware, modules attachés, qualité du Wifi, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module&gt;&gt;.Info</strong></td>
<td valign="top" width="10">NetAtmo.Module</td>
<td valign="top" width="446">Description du module NetAtmo (ID, firmware, état RF, niveau de batterie, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.Temperature</strong></td>
<td valign="top" width="10">NetAtmo.TemperatureMeasurement</td>
<td valign="top" width="446">Mesure de la température</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.Humidity</strong></td>
<td valign="top" width="10">NetAtmo.HumidityMeasurement</td>
<td valign="top" width="446">Mesure de l’humidité relative</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.Rain</strong></td>
<td valign="top" width="10">NetAtmo.RainMeasurement</td>
<td valign="top" width="446">Mesure de la pluie</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.CarbonDioxide</strong></td>
<td valign="top" width="10">NetAtmo.CarbonDioxideMeasurement</td>
<td valign="top" width="446">Mesure du CO²</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.Noise</strong></td>
<td valign="top" width="10">NetAtmo.NoiseMeasurement</td>
<td valign="top" width="446">Mesure du niveau sonore</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom du module/station&gt;&gt;.Pressure</strong></td>
<td valign="top" width="10">NetAtmo.PressureMeasurement</td>
<td valign="top" width="446">Mesure de la pression atmosphérique</td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-96.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-88.png" alt="image" width="350" height="220" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>
Ce package n’expose pas de MessageCallback.
<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Notifier l’utilisateur si il pleut alors qu’une fenetre est restée ouverte</li>
 	<li>Afficher les mesures des modules NetAtmo dans une application graphique pour Windows (WPF)</li>
 	<li>Afficher la qualité de l’air sur une matrice de LED pilotée par un Arduino/ESP</li>
 	<li>Piloter le thermostat Nest depuis une autre pièce en fonction de la température d’un module NetAtmo</li>
</ul>