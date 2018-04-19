---
ID: 2134
post_title: 'PushBullet : envoyer des notifications, images, SMS sur vos différents devices'
author: Sebastien Warin
post_date: 2016-08-09 13:03:41
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/pushbullet/
published: true
wpcr3_format:
  - product
wpcr3_product_name:
  - PushBullet
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:36:32
---
PushBullet est un service très complet permettant de relier vos ordinateurs et appareils mobiles ensemble pour partager entre eux, globalement ou de manière ciblée, des liens, des notifications (par exemple en cas de réception de SMS), des messages, des copier-coller et même des fichiers.

<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-21.png" alt="image" width="308" height="74" border="0" /></p>

Vous pouvez installer les clients PushBullet sur Android, iOS, Windows et directement dans les navigateurs (sous forme d’extension) Chrome, Firefox, Safari ou Opéra.

<p align="center"><a href="https://www.pushbullet.com/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-22.png" alt="image" width="354" height="250" border="0" /></a></p>

<p align="left">Le package PushBullet pour Constellation vous permettra :</p>

<ul>
    <li>
<div align="left">d’envoyer des notifications texte</div></li>
    <li>
<div align="left">d’envoyer des fichiers</div></li>
    <li>
<div align="left">d’envoyer des liens</div></li>
    <li>
<div align="left">d’envoyer des SMS de vos smartphones Android</div></li>
    <li>
<div align="left">de copier un texte dans le presse-papier (avec un abonnement PushBullet Prenium)</div></li>
    <li>
<div align="left">de récupérer des informations sur l’état de vos devices et utilisateurs connectés au service</div></li>
</ul>

<p align="left">Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/PushBullet">https://github.com/myconstellation/constellation-packages/tree/master/PushBullet</a></p>

<h3 align="left">Installation</h3>

<h4 align="left">Prérequis : PushBullet</h4>

<p align="left">Pour démarrer vous devez créer un compte sur <a title="https://www.pushbullet.com/" href="https://www.pushbullet.com/">https://www.pushbullet.com/</a>.</p>

<p align="left">Une fois votre compte activé, installer des clients PushBullet sur vos différents devices, par exemple votre smartphone (iOS ou Android) et dans votre navigateur.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-20.png" alt="image" width="350" height="232" border="0" /></a></p>

<p align="left">Dès à présent, les notifications de votre smartphone seront partagées avec votre navigateur. Vous pourrez également depuis votre navigateur lire/écrire des SMS, etc…</p>

<p align="left">Pour finir, dans les options de votre compte PushBullet, créer un “Access Token” qui servira pour connecter le package Constellation à votre comte PushBullet :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-21.png" alt="image" width="350" height="232" border="0" /></a></p>

<h4 align="left">Installation du package</h4>

<p align="left">Depuis le “Online Package Repository” de votre Console Constellation, déployez le package PushBullet :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-25.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-22.png" alt="image" width="350" height="195" border="0" /></a></p>

<p align="left">Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-26.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-23.png" alt="image" width="350" height="115" border="0" /></a></p>

<p align="left">Pour finir entrez le token dans la page de Settings et vous pouvez déployer votre package :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-27.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-24.png" alt="image" width="350" height="284" border="0" /></a></p>

<p align="left">Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p>

<pre class="lang:html5 decode:true">&lt;package name="PushBullet"&gt;
  &lt;settings&gt;
    &lt;setting key="token" value="&lt;&lt; access token &gt;&gt;" /&gt;
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
<td valign="top" width="10"><strong>token</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Le token d’accès au compte PushBullet</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushDevicesAsStateObjects</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="10">Optionnel
Défaut : ”true”</td>
<td valign="top" width="456">Indique si les devices de votre compte PushBullet doivent être synchronisés dans les StateObjects</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushChatsAsStateObjects</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="10">Optionnel
Défaut : ”true”</td>
<td valign="top" width="456">Indique si les Chats associés à votre compte PushBullet doivent être synchronisés dans les StateObjects</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushCurrentUserAsStateObject</strong></td>
<td valign="top" width="10">Boolean</td>
<td valign="top" width="10">Optionnel
Défaut : ”true”</td>
<td valign="top" width="456">Indique si les détails votre compte PushBullet doit être synchronisé dans les StateObjects</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendPushesReceivedToGroup</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Optionnel
Défaut : ”PushBullet”</td>
<td valign="top" width="456">Spécifie le nom du groupe dans lequel le package Pushbullet va envoyer les notifications qu’il reçoit (par défaut:  'PushBullet'). Laissez le champ vide pour désactiver cette fonctionnalité</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendEphemeralsReceivedToGroup</strong></td>
<td valign="top" width="10">String</td>
<td valign="top" width="10">Optionnel
Défaut : ”PushBullet”</td>
<td valign="top" width="456">Spécifie le nom du groupe dans lequel le package Pushbullet va envoyer les “ephemerals” qu’il reçoit (par défaut:  'PushBullet'). Laissez le champ vide pour désactiver cette fonctionnalité.</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez 4 StateObjects publiés par le package :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>CurrentUser</strong></td>
<td valign="top" width="10">PushBullet.Models.User</td>
<td valign="top" width="446">Information sur l’utilisateur connecté</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Chats</strong></td>
<td valign="top" width="10">PushBullet.Models.ChatsList</td>
<td valign="top" width="446">Liste des chats de l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Devices</strong></td>
<td valign="top" width="10">PushBullet.Models.DevicesList</td>
<td valign="top" width="446">Liste des devices connectés à PushBullet de l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>RateLimit</strong></td>
<td valign="top" width="10">PushBullet.RateLimits</td>
<td valign="top" width="446">Etat des limites du service pour l’utilisateur</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-28.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-25.png" alt="image" width="350" height="131" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>

Le package expose 9 MessageCallbacks :

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetDevices</strong></td>
<td valign="top" width="141">PushBullet.Models.DevicesList</td>
<td valign="top" width="407">Récupère la liste des devices de l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetChats</strong></td>
<td valign="top" width="141">PushBullet.Models.ChatsList</td>
<td valign="top" width="407">Récupère la liste des chats de l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetPushes</strong></td>
<td valign="top" width="141">PushBullet.Models.PushesList</td>
<td valign="top" width="407">Récupère la liste des notification reçu par l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetCurrentUser</strong></td>
<td valign="top" width="141">PushBullet.Models.User</td>
<td valign="top" width="407">Récupère le détail de l’utilisateur</td>
</tr>
<tr>
<td valign="top" width="10"><strong>SendSMS</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Envoi un SMS depuis le smartphone (compatible avec le client Android seulement)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>CopyToClipboard</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Copie un texte dans le presse-papier d’un device (compatible avec un compte Prenium uniquement)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushNote</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Push une notification (message texte)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushLink</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Push une notification avec un lien</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushFile</strong></td>
<td valign="top" width="141">Aucune</td>
<td valign="top" width="407">Push une notification avec un fichier</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-29.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-26.png" alt="image" width="350" height="454" border="0" /></a></h3>

<h3 align="left">Quelques exemples</h3>

<h4 align="left">Tester l’envoi d’une notification depuis la Console Constellation</h4>

<p align="left">Le MC pour envoyer une simple notification se nomme “PushNote” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-30.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-27.png" alt="image" width="350" height="144" border="0" /></a></p>

<p align="left">Il prend en paramètre :</p>

<ul>
    <li>
<div align="left">Le titre de la notification</div></li>
    <li>
<div align="left">Le texte de la notification</div></li>
    <li>
<div align="left">Le type de la cible pour la notification</div></li>
    <li>
<div align="left">L’argument de la cible</div></li>
</ul>

<p align="left">La cible (target) est une énumération :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-31.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-28.png" alt="image" width="350" height="203" border="0" /></a></p>

<p align="left">Vous pouvez envoyer votre notification à un de vos devices, à un autre utilisateur (identifié par son mail), à un canal ou un client Web.</p>

<p align="left">Dans notre cas, sélectionnons la cible par default “Device” et laissons le champs “targetArgument” afin de recevoir la notification sur tous nos devices enregistrés sur PushBullet et cliquons sur “Invoke” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-32.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-29.png" alt="image" width="350" height="235" border="0" /></a></p>

<p align="left">Votre notification sera instantanément reçue sur votre Smartphone :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-32.png" alt="image" width="350" height="316" border="0" /></a></p>

<p align="left">Ici la notification reçue par Chrome sur un poste Windows :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-33.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-30.png" alt="image" width="350" height="165" border="0" /></a></p>

<p align="left">Avec une smartwatch associée à votre téléphone, vous recevrez également vos notifications à votre poignet:</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-34.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-31.png" alt="image" width="354" height="296" border="0" /></a></p>

<h4 align="left">Envoyer des notification texte ou image depuis un package C#</h4>

<p align="left">N’hésitez pas dans le MessageCallbacks Explorer, à cliquer sur le bouton  <a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-33.png" alt="image" width="23" height="18" border="0" /></a> afin d’ouvrir le générateur de code pour invoquer le MC sélectionné avec les différentes API Constellation.</p>

<p align="left">Ici en sélectionnant le langage C# :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-34.png" alt="image" width="354" height="223" border="0" /></a></p>

<p align="left">Pour envoyer notre notification de puis un package C# on écrira :</p>

<pre class="lang:csharp decode:true">PackageHost.CreateMessageProxy("PushBullet").PushNote("Constellation", "Ceci est un test");</pre>

<p align="left">Les deux derniers paramètres étant optionnels (target et targetArguments) vous pouvez les omettre !</p>

<p align="left">Autre solution, générer du code avec <a href="/constellation-platform/constellation-sdk/generateur-de-code/">le générateur inclus dans le SDK</a> Constellation. Sélectionnez simplement le package “PushBullet” dans le générateur de code :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-35.png" alt="image" width="350" height="387" border="0" /></a></p>

<p align="left">Vous pouvez ensuite invoquer vos MC dans votre code C# à partir du code générer profitant ainsi de l’auto-complétion et de la documentation directement dans votre IDE :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-36.png" alt="image" width="450" height="112" border="0" /></a></p>

<p align="left">Par exemple pour envoyer une image vous pouvez invoquer le MC “PushFile” en spécifiant l’URI (file:// ou http(s)://) vers le fichier à envoyer. Si vous souhaitez envoyer une notification avec l’image de votre caméra par exemple :</p>

<pre class="lang:csharp decode:true">MyConstellation.Packages.PushBullet.CreatePushBulletScope().PushFile(urlImageCamera, "Porte de garage ouverte");</pre>

<p align="left">L’image sera alors envoyée sur tous vos devices :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-37.png" alt="image" width="204" height="99" border="0" /></a></p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-38.png" alt="image" width="204" height="240" border="0" /></a></p>

<h3 align="left">Tutoriels</h3>

<p align="left">[tutoriels package="pushbullet"]</p>

<h4 align="left">Lister ses devices et envoyer des notifications depuis une page Web</h4>

<p align="left">Prenons une page HTML que nous allons connecter à Constellation avec AngularJS (<a href="/client-api/javascript-api/consommer-constellation-angular-js/">documentation ici</a>).</p>

<p align="left">Lorsque la page sera connectée à Constellation, enregistrons un lien vers le StateObject nommé “Devices” pour le package “PushBullet” afin d’enregistrer la liste des devices (propriété “Devices”) de la valeur du StateObject dans une variable de scope Angular sans oublier de faire un “$apply” pour rafraichir le scope AngularJS :</p>

<pre class="lang:javascript decode:true">constellation.registerStateObjectLink("*", "PushBullet", "Devices", "*", function(so) { 
    $scope.myDevices = so.Value.Devices;
    $scope.$apply();
});
</pre>

<p align="left">Enfin dans le corps de votre page HTML, affcher une liste à point pour chaque device en affichant différentes informations comme le nom, fabriquant et modele de vos device :</p>

<pre class="lang:html5 decode:true">&lt;body ng-controller="DemoController"&gt;
  My devices :
  &lt;ul&gt;
    &lt;li ng-repeat="device in myDevices"&gt;{{device.Nickname}} ({{device.Manufacturer}} {{device.Model}}
  &lt;/ul&gt;
&lt;/body&gt;</pre>

Allons un peu plus loin en ajoutant un bouton sur chaque device pour envoyer une notification (PushNote) au device spécifiquement sélectionné. Dans le &lt;li&gt; ajoutons :

<pre class="lang:html5 decode:true">&lt;a href="#" ng-click="sendPush(device)"&gt;Send Push&lt;/a&gt;</pre>

Et bien sûr déclarons la fonction “sendPush” dans notre scope pour invoquer le MC “PushNote” de notre package PushBullet en spécifiant l’ID du device destinataire de ce push et non tous les devices :

<pre class="lang:javascript decode:true">$scope.sendPush = function(device) {
  constellation.sendMessage({ Scope: 'Package', Args: ['PushBullet'] }, 'PushNote', [ "DemoJS", "Hello " + device.Nickname + " from JS", 'Device',
};
</pre>

Et voilà, aussi simple que cela !

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-50.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-47.png" alt="image" width="354" height="176" border="0" /></a></p>

Le code complet de la page :

<pre class="lang:html5 decode:true">&lt;!DOCTYPE html&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" ng-app="demoApp"&gt;
&lt;head&gt;
    &lt;title&gt;Test API AngularJS&lt;/title&gt;
    &lt;script type="text/javascript" src="//code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="//cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="//cdn.myconstellation.io/js/ngConstellation-1.8.1.min.js"&gt;&lt;/script&gt;
    
    &lt;script&gt;

    var demo = angular.module('demoApp', ['ngConstellation']);
    demo.controller('DemoController', ['$scope', 'constellationConsumer', function ($scope, constellation) {

        constellation.initializeClient("https://skynet.ajsinfo.net/constellation/", "9eBsRK5FFS5h36cM3OeE74qaDhlxCE2", "TestAPI");
            
        constellation.onConnectionStateChanged(function (change) {
            if (change.newState === $.signalR.connectionState.connected) {
                console.log("Connected to the Constellation");
                constellation.registerStateObjectLink("*", "PushBullet", "Devices", "*", function(so) {                    
                    $scope.myDevices = so.Value.Devices;
                    $scope.$apply();
                });
            }
        });
        
        $scope.sendPush = function(device) {
            constellation.sendMessage({ Scope: 'Package', Args: ['PushBullet'] }, 'PushNote', [ "DemoJS", "Hello " + device.Nickname + " from JS", 'Device', device.Id ]);
        };
        
        constellation.connect();
    }]);

    &lt;/script&gt;

&lt;/head&gt;
&lt;body ng-controller="DemoController"&gt;

    My devices :
    &lt;ul&gt;
        &lt;li ng-repeat="device in myDevices"&gt;{{device.Nickname}} ({{device.Manufacturer}} {{device.Model}}) &lt;a href="#" ng-click="sendPush(device)"&gt;Send Push&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
    
&lt;/body&gt;
&lt;/html&gt;</pre>

<h4>Recevoir des SMS depuis un Arduino/ESP</h4>

Si le paramètre “SendEphemeralsReceivedToGroup” n’est pas vide, le package enverra les “Ephemerals” comme les SMS reçus au groupe que vous avez spécifié par ce paramètre. Par défaut, il est défini à “PushBullet”, c’est à dire que si vous vous abonnez au groupe “PushBullet” vous recevrez les SMS et autre de notification de votre smartphone.

Prenons par exemple la page Web AngularJS créée ci-dessus pour bien comprendre. Lorsque votre page est connectée à Constellation, ajoutez le code suivant :

<pre class="lang:javascript decode:true">constellation.subscribeMessages("PushBullet");</pre>

Puis ajoutez un MessageCallback de votre page pour réceptionner le message nommé “ReceiveEphemeral”. On affichera alors le message reçu dans la console de notre navigateur pour comprendre ce que le package envoie :

<pre class="lang:javascript decode:true">constellation.registerMessageCallback("ReceiveEphemeral", function(msg) {
  console.log(msg);
});</pre>

Lancez votre page et envoyez-vous un SMS :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-51.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-48.png" alt="image" width="350" height="280" border="0" /></a></p>

Le message est bien reçu du package PushBullet (propriété Sender) avec la clé (identifiant du MessageCallback) “ReceiveEphemeral”. Le contenu du message (Data) contient :

<ul>
    <li>L’identifiant du device qui a reçu la notification</li>
    <li>Le type de notification. Pour une notification liée à la réception d’un SMS cette valeur est à “sms_changed”</li>
    <li>Le tableau contenant le ou les notifications</li>
</ul>

Si la notification de l’“Ephemeral” est de type “sms_changed”, le tableau des notifications contiendra les SMS reçus avec le nom du contact émetteur du SMS (title), le texte du SMS (body) et la date de réception (timestamp).

<p align="left">Enfin, lorsque vous ouvrez le SMS reçu sur votre Smartphone vous allez faire disparaitre la notification ce qui enverra un autre “Ephemeral” de type “dismissal” pour le package “sms” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-49.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-46.png" alt="image" width="350" height="147" border="0" /></a></p>

<p align="left">Il devient alors facile de réaliser un petit objet connecté qui serait capable de nous prévenir en faisant clignoter une LED par exemple, lorsque nous recevons un SMS et d’arrêter cette LED lorsque vous nous faisons disparaitre la notification sur notre Smartphone.</p>

<p align="left">Dans la méthode “setup()” nous allons abonnez notre ESP au groupe “PushBullet” :</p>

<pre class="lang:cpp decode:true">constellation.subscribeToGroup("PushBullet");</pre>

<p align="left">Notez que vous pouvez également abonner votre package depuis la Console Constellation.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-52.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-49.png" alt="image" width="350" height="207" border="0" /></a></p>

<p align="left">Ajoutons ensuite un MessageCallback “ReceiveEphemeral” pour définir si oui ou non nous devons faire clignoter une LED :</p>

<pre class="lang:cpp decode:true">constellation.registerMessageCallback("ReceiveEphemeral",
  [](JsonObject&amp; json) {
    if(json["Data"]["type"] == "sms_changed") {
      Serial.println("New SMS received");
      blinkLed = true;
    }
    else if(json["Data"]["type"] == "dismissal") {
      Serial.println("SMS readed");
      blinkLed = false;
    }
});
</pre>

De ce fait déclarons cette variable ainsi que d’autre :

<pre class="lang:cpp decode:true">bool blinkLed = false;
int ledState = LOW;
unsigned long previousMillis = 0;
const int ledPin = LED_BUILTIN;
const long interval = 1000;</pre>

Pour faire clignoter la LED sans bloquer le programme avec des delay(), ajoutons la méthode suivante :

<pre class="lang:cpp decode:true">void driveLed() {
  if(blinkLed) {
    unsigned long currentMillis = millis();
    if(currentMillis - previousMillis &gt;= interval) {
      previousMillis = currentMillis;
      ledState = ledState == LOW ? HIGH: LOW; // Switches led state
    }
  }
  else ledState = LOW; 
  digitalWrite(ledPin, ledState);
}</pre>

Pour finir dans la boucle principale, appellons notre méthode “driveLed” poru faire clignoter ou éteindre la LED en fonction :

<pre class="lang:cpp decode:true">void loop(void) {
  constellation.loop();
  driveLed();
}</pre>

Et voilà comment notifier la réception de SMS avec un LED drivée par un ESP8266 connecté à Constellation !

<h4>Envoyer un SMS depuis un simple appel HTTP</h4>

Si vous avez installé un client PushBullet sur un smartphone Android, vous pouvez l’utiliser en tant que passerelle SMS avec le MessageCallback “SendSMS” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-53.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-50.png" alt="image" width="350" height="150" border="0" /></a></p>

Ce MC prend en paramètre :

<ul>
    <li>L’ID de l’utilisateur</li>
    <li>L’ID du device qui sera utilisé comme passerelle SMS (ce device doit être un téléphone sous Android)</li>
    <li>Le n° du destinataire du SMS</li>
    <li>Le texte du SMS</li>
</ul>

Pour récupérer votre ID d’utilisateur, affichez les détails du StateObject “CurrentUser” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-54.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-51.png" alt="image" width="348" height="253" border="0" /></a></p>

Faites de même pour récupérer l’ID de votre device Android en visualisant le StateObject “Devices” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-55.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-52.png" alt="image" width="350" height="251" border="0" /></a></p>

Cliquez ensuite sur l’icone <a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-45.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-42.png" alt="image" width="23" height="18" border="0" /></a> dans le MessageCallback Explorer pour afficher le code snippet du MC “SendSMS” en sélectionnant en tant que langage “HTTP Call” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-56.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-53.png" alt="image" width="350" height="220" border="0" /></a></p>

<p align="left">L’exemple est donné pour <a href="/client-api/rest-api/interface-rest-constellation/">l’API Rest Constellation</a> mais vous pouvez aussi utiliser <a href="/client-api/rest-api/interface-rest-consumer/">l’API Rest Consumer</a> en remplaçant “/rest/constellation” par “/rest/consumer”.</p>

De ce fait, pour envoyer un SMS depuis un appel HTTP :

<pre class="lang:csharp decode:true">https://constellation.monserver.com/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=DemoConstellation&amp;AccessKey=xxxx&amp;scope=Package&amp;args=PushBullet&amp;key=SendSMS&amp;data=[ "mon_user_id_ici", "mon_device_id_android_ici", "0600000000", "Hello, ceci est un sms de test envoyé depuis l'API REST de Constellation" ]</pre>

Sans oublier bien sûr de remplacer le “xxxx” par une <a href="/concepts/securite-accesskey-credential-authorization/">AccessKey</a> valide de votre Constellation.