---
ID: 3319
post_title: 'Nest : pilotez et surveillez vos objets Nest dans votre Constellation'
author: Sebastien Warin
post_date: 2016-10-21 15:26:00
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/nest/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:33:34
---
Le package Nest vous permet de piloter votre thermostat Nest et de suivre en temps réel vos objets connectés Nest (Thermostats, détecteur de fumée ou caméras).

<p align="center"><a href="http://www.nest.com"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-87.png" alt="image" width="400" height="202" border="0" /></a></p>

Le code source de ce package est en ligne sur : <a title="https://github.com/myconstellation/constellation-packages/tree/master/Nest" href="https://github.com/myconstellation/constellation-packages/tree/master/Nest">https://github.com/myconstellation/constellation-packages/tree/master/Nest</a>

<h3>Installation</h3>

<h4>Prérequis : créer un jeton de sécurité Nest</h4>

Pour installer le package Nest dans votre Constellation vous allez avoir besoin d’un jeton de sécurité pour autoriser le package à se connecter au service Nest.

<h5>Etape 1 : Créer un compte développeur Nest</h5>

Pour commencez donc par créer un compte sur <a title="https://developers.nest.com/" href="https://developers.nest.com">https://developers.nest.com</a> :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-72.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-68.png" alt="image" width="350" height="238" border="0" /></a></p>

<h5 align="left">Etape 2 : Créer un produit Nest</h5>

<p align="left">Créez ensuite un “Product” en cliquant sur le bouton “Create New Product” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-73.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-69.png" alt="image" width="350" height="238" border="0" /></a></p>

Vous devrez alors fournir les informations sur votre “Produit”. Par exemple nommez-le “Constellation”.

Vous devrez également définir les autorisations accordées à ce produit : activer les différents services (Thermostat, Away, etc..) en “Read / Write” si vous souhaitez contrôler vos équipements Nest.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-74.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-70.png" alt="image" width="350" height="238" border="0" /></a></p>

<p align="left">Une fois votre produit Nest créé, vous obtiendrez votre “Product ID” et “Product Secret” que vous gardez sous la main :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-75.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-71.png" alt="image" width="350" height="238" border="0" /></a></p>

<h5>Etape 3 : Générer le jeton de sécurité pour le package Constellation</h5>

Vous pouvez maintenant générer un jeton de sécurité qui sera utilisé par le package Constellation.

Vous avez deux méthodes :

<ol>
    <li>Via un script Powershell (à partir d’un poste Windows)</li>
    <li>Manuellement</li>
</ol>

Pour la première méthode, téléchargez <a href="https://github.com/myconstellation/constellation-packages/raw/master/Nest/GetNestAccessToken.ps1">ce fichier ici</a>, il s’agit d’un script Powershell à exécuter sur un poste Windows (voir <a href="https://github.com/myconstellation/constellation-packages/blob/master/Nest/GetNestAccessToken.ps1">le code source ici</a>).

Une fois téléchargé sur votre poste Windows, ouvrez une invite de commande Powershell et lancez le script en passant en paramètre votre “Product ID” et “Product Secret” :

<pre class="lang:powershell decode:true">GetNestAccessToken.ps1 &lt;&lt;product ID&gt;&gt; &lt;&lt;product Secret&gt;&gt;</pre>

Vous devez valider le fait d’exécuter un script téléchargé sur Internet en entrant la lettre “O” en majuscule.

Le script ouvrira votre navigateur Internet sur une page de Nest vous demandant votre autorisation pour activer le produit que vous avez créé ci-dessus :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-76.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-72.png" alt="image" width="350" height="226" border="0" /></a></p>

Il suffit de cliquer sur le bouton “Accepter” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-77.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-73.png" alt="image" width="350" height="337" border="0" /></a></p>

<p align="left">Vous obtiendrez en retour un code PIN temporaire :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-78.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-74.png" alt="image" width="350" height="251" border="0" /></a></p>

<p align="left">Copiez alors ce code PIN dans le script PowerShell et celui ci vous retournera l’AccessToken (le jeton de sécurité) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-79.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-75.png" alt="image" width="350" height="226" border="0" /></a></p>

<p align="left">Ce jeton est également copié automatiquement dans votre presse-papier ! Vous pouvez maintenant le “coller” où vous souhaitez ! Gardez-le pour l’installation du package Constellation ci-dessous.</p>

<h4>Installation du package Constellation</h4>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Nest :

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-80.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-76.png" alt="image" width="350" height="213" border="0" /></a>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir le jeton de sécurité obtenu précédemment pour accéder à votre compte Nest.

Si vous avez utiliser le script Powershell, l’AccessToken est déjà dans votre presse-papier, il suffit simplement de le coller dans la page de settings :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-81.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-77.png" alt="image" width="350" height="198" border="0" /></a></p>

<p align="left">Bien entendu vos  pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="Nest"&gt;
  &lt;settings&gt;
    &lt;setting key="AccessToken" value="xxxxx" /&gt;
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
<td valign="top" width="10"><strong>AccessToken</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Jeton de sécurité pour accès à votre compte Nest</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObjects que d’objet et de structure Nest rattachés à votre compte :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom de la structure &gt;&gt;</strong></td>
<td valign="top" width="10">Nest.Structure</td>
<td valign="top" width="446">Information sur la structure (maison) Nest (localisation, mode absence, objets rattachés)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; nom de l’objet Nest &gt;&gt;</strong></td>
<td valign="top" width="10">Nest.thermostat,
Nest.SmokeDetector,
Nest.Camera</td>
<td valign="top" width="446">Information sur l’objet Nest. Par exemple pour un thermostat : T° actuelle et de consigne, mode de chauffe, humidité, etc..</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-82.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-78.png" alt="image" width="350" height="159" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose un MessageCallback :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetAwayMode</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Spécifie le mode absence d’une structure</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetTargetTemperature</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Spécifie la température de consigne d’un thermostat</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SetProperty</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Définie la valeur d’un propriété sur un objet Nest</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-83.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-79.png" alt="image" width="350" height="300" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>Piloter et réguler la température avec un package C#</li>
    <li>Piloter le thermostat depuis un Dashboard HTML</li>
    <li>Synchroniser le mode absence du thermostat Nest avec l’alarme</li>
</ul>