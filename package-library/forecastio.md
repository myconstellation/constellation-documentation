---
ID: 3279
post_title: 'ForecastIO : les prévisions météo dans Constellation'
author: Sebastien Warin
post_date: 2016-10-21 11:37:04
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/forecastio/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:35:24
---
Le package ForecastIO vous permet de connaitre les conditions météorologique actuelles et prévisions jusqu’à 8 jours.

<p align="center"><a href="http://www.forecast.io"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-84.png" alt="image" width="350" height="98" border="0" /></a></p>

<p style="text-align: left;" align="center">Le code source de ce package est disponible sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/ForecastIO">https://github.com/myconstellation/constellation-packages/tree/master/ForecastIO</a></p>

<h3>Installation</h3>

<h4>Prérequis : créer un compte développeur sur Dark Sky</h4>

Pour pouvoir utiliser le service Forecast.io vous devez créer un compte développeur Dark Sky sur : <a title="https://darksky.net/dev/" href="https://darksky.net/dev/">https://darksky.net/dev/</a>

Vous disposez de 1.000 appels au service par jour gratuitement.

<p align="center"><a href="https://darksky.net/dev/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-67.png" alt="image" width="350" height="244" border="0" /></a></p>

Une fois inscrit, copiez la clé API que vous devrez rentrer dans la configuration du package Constellation.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-68.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-64.png" alt="image" width="350" height="244" border="0" /></a></p>

<h4>Installation du package Constellation</h4>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package ForecastIO :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-63.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-60.png" alt="image" width="350" height="213" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir la configuration au format XML :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-64.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-61.png" alt="image" width="350" height="286" border="0" /></a></p>

<p align="left">Le XML de configuration du package a la structure suivante :</p>

<pre class="lang:html5 decode:true">&lt;forecastIOConfigurationSection xmlns="urn:ForecastIO" apiKey="xxxxxxxxxxxxxxxxx" refreshInterval="00:30:00" language="en"&gt;
  &lt;stations&gt;
    &lt;station name="Paris" longitude="2.35" latitude="48.853" /&gt;
  &lt;/stations&gt;
&lt;/forecastIOConfigurationSection&gt;</pre>

Sur la balise racine, vous devez obligatoirement spécifier dans l’attribut “apiKey” votre clé d’API obtenue sur le portail développeur Dark Sky. Vous pouvez également modifier l’intervalle de rafraichissement des prévisions (par défaut 30 minutes), la langue des commentaires (par défaut en anglais) ainsi que les unités (en rajoutant l'attribut "unit").

Dans la listes “&lt;stations&gt;” vous pouvez configurer autant de stations que vous souhaitez. Chaque station est définie par un nom et une position GPS (Latitude et Longitude).

Par exemple, pour maintenir à jour dans vos StateObjects Constellation, la météo (en Francais),  pour Paris, Lille et Londres toutes les 10 minutes:

<pre class="lang:html5 decode:true">&lt;forecastIOConfigurationSection xmlns="urn:ForecastIO" apiKey="xxxxxxxxxxxxxxxxx" refreshInterval="00:10:00" language="fr"&gt;
  &lt;stations&gt;
    &lt;station name="Paris" longitude="2.35" latitude="48.853" /&gt;
    &lt;station name="Lille" longitude="3.05" latitude="50.629" /&gt;
    &lt;station name="Londre" longitude="-0.14" latitude="51.557" /&gt;
  &lt;/stations&gt;
&lt;/forecastIOConfigurationSection&gt;</pre>

Bien entendu vos  pouvez également déployer ce package manuellement dans la configuration de votre Constellation :

<pre class="lang:html5 decode:true">&lt;package name="ForecastIO"&gt;
  &lt;settings&gt;
    &lt;setting key="forecastIOConfigurationSection"&gt;
        &lt;content&gt;
        &lt;forecastIOConfigurationSection xmlns="urn:ForecastIO" apiKey="xxxxxxxxxxxxxxxxx" refreshInterval="00:30:00" language="fr"&gt;
          &lt;stations&gt;
            &lt;station name="Paris" longitude="2.35" latitude="48.853" /&gt;
            &lt;station name="Lille" longitude="3.05" latitude="50.629" /&gt;
            &lt;station name="Londre" longitude="-0.14" latitude="51.557" /&gt;
          &lt;/stations&gt;
        &lt;/forecastIOConfigurationSection&gt;
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
<td valign="top" width="456"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>forecastIOConfigurationSection</strong></td>
<td valign="top" width="10">ConfigurationSection</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">XML de configuration du package</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que de stations spécifiées dans la configuration du package. Ces StateObjects sont mis à jour pour l’intervalle défini dans la configuration (toutes les 30 minutes par défaut).

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom de la station &gt;&gt;</strong></td>
<td valign="top" width="10">ForecastIO.ForecastIOResponse</td>
<td valign="top" width="446">StateObject dont le nom est le nom de la station que vous avez défini dans la configuration, et la valeur contient les conditions actuelles et prévisions météorologique pour la station.</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-65.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-62.png" alt="image" width="350" height="112" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 2 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetWeatherForecast</strong></td>
<td valign="top" width="141">ForecastIO.ForecastIOResponse</td>
<td valign="top" width="407">Récupère les conditions actuelles et prévisions météorologique pour une position GPS donnée</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-66.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-63.png" alt="image" width="350" height="115" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher la météo dans un Dashboard HTML</li>
    <li>Envoyer une notification depuis un package C# en cas de pluie dans l’heure</li>
</ul>