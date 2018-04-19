---
ID: 3627
post_title: 'GoogleTraffic : l&rsquo;état du trafic routier dans Constellation'
author: Sebastien Warin
post_date: 2016-10-28 13:46:58
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/googletraffic/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:23:34
---
Le  package GoogleTraffic permet de calculer le temps de route entre deux adresses en tenant compte de l’état du trafic.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-175.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-157.png" alt="image" width="350" height="213" border="0" /></a></p>

<h3>Installation</h3>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package GoogleTraffic :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-176.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-158.png" alt="image" width="350" height="211" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings vous pouvez optionnellement remplir votre adresse de domicile et une ou plusieurs adresse de destination avec une plage horaire :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-177.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-159.png" alt="image" width="350" height="284" border="0" /></a></p>

<p align="left">Lorsque le package est dans une plage d’horaire spécifiée, il réalisera les itinéraires “de” et “vers” les destinations à l’intervalle spécifié dans l’attribut “refreshInterval” (ici 15 min). Les résultats sont publiés dans des StateObjects.</p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="GoogleTraffic" /&gt;</pre>

<p align="left">Ou avec des deux itinéraire à suivre de 7h à 10h, de 11h45 à 14h30 et de 16h à 19h30 :</p>

<pre class="lang:html5 decode:true">&lt;package name="GoogleTraffic"&gt;
  &lt;settings&gt;
    &lt;setting key="googleTrafficConfigurationSection"&gt;
        &lt;content&gt;
            &lt;googleTrafficConfigurationSection xmlns="urn:GoogleTraffic" refreshInterval="00:15:00"&gt;
                &lt;home name="Maison" address="108 rue Sebastopol" postalCode="59000" city="Lille" /&gt;
                &lt;destinations&gt;
                    &lt;address name="Adresse 1" address="48 rue de Douai" postalCode="59000" city="Lille" /&gt;
                    &lt;address name="Adresse 2" address="1 avenue victor huge" postalCode="92190 " city="Meudon" /&gt;
                &lt;/destinations&gt;
                &lt;timeRanges&gt;
                    &lt;timeRange from="07:00:00" to="10:00:00" /&gt;
                    &lt;timeRange from="11:45:00" to="14:30:00" /&gt;
                    &lt;timeRange from="16:00:00" to="19:30:00" /&gt;
                &lt;/timeRanges&gt;
            &lt;/googleTrafficConfigurationSection&gt;
       &lt;/content&gt;
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
<td valign="top" width="10"><strong>googleTrafficConfigurationSection</strong></td>
<td valign="top" width="10">ConfigurationSection</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="478">Adresse IP ou DNS du pont Hue.</td>
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
<td valign="top" width="10"><strong>RouteHomeTo&lt;&lt;  Destination &gt;&gt;</strong></td>
<td valign="top" width="10">List&lt;TrafficData&gt;</td>
<td valign="top" width="446">La liste des itinéraires possibles de l’adresse “home” à la “destination” avec le temps de route avec et sans trafic et la distance.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Route&lt;&lt;  Destination &gt;&gt;ToHome</strong></td>
<td valign="top" width="10">List&lt;TrafficData&gt;</td>
<td valign="top" width="446">La liste des itinéraires possibles de l’adresse “destination” à “home” avec le temps de route avec et sans trafic et la distance.</td>
</tr>
</tbody>
</table>

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
<td valign="top" width="10"><strong>GetRoute</strong></td>
<td valign="top" width="141">List&lt;TrafficData&gt;</td>
<td valign="top" width="407">Calcule et retourne la liste des itinéraires possibles entre deux adresses avec le temps de route avec et sans trafic et la distance.</td>
</tr>
</tbody>
</table>

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-178.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-160.png" alt="image" width="350" height="109" border="0" /></a>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Afficher le temps de route et le nom de la meilleur route sur une application Windows WPF</li>
    <li>Créer un programme pour enregistrer le temps de route aux horaires de travail et exploiter les statistiques dans Excel.</li>
    <li>Afficher l’état du trafic sur la route vers votre travail sur un anneau de LED RBG avec un Arduino/ESP</li>
</ul>