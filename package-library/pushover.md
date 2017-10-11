---
ID: 3426
post_title: 'Pushover : push de notifications sur Android, iOS et navigateurs'
author: Sebastien Warin
post_date: 2016-10-25 16:21:47
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/pushover/
published: true
post_modified: 2017-10-11 15:40:45
---
Le package Puhover permet d’envoyer des notifications sur vos appareils Android, iOS (iPhone/iPad) ou Deskop (Chrome, Safari et Firefox).

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/pushover.png"><img class="size-full wp-image-3431 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/pushover.png" alt="pushover" width="190" height="190" /></a>

Le code source du package est disponible sur : <a title="https://github.com/myconstellation/constellation-packages/tree/master/Pushover" href="https://github.com/myconstellation/constellation-packages/tree/master/Pushover">https://github.com/myconstellation/constellation-packages/tree/master/Pushover</a>

<h3>Installation</h3>

<h4>Prérequis : création du compte Pushover</h4>

Tout d’abord vous devez vous inscrire sur <a title="https://pushover.net/" href="https://pushover.net/">https://pushover.net/</a>

<p align="center"><a href="https://pushover.net/"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-108.png" alt="image" width="350" height="296" border="0" /></a></p>

<p align="left">Ensuite installez les clients sur vos appareils (Desktop, Android et/ou iOS).</p>

<p align="center"><a href="https://pushover.net/clients"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-109.png" alt="image" width="350" height="214" border="0" /></a></p>

<p align="left">Vous pourrez alors enregistrer vos différents appareils. Le client coute 4,99$ (en une fois, pas d’abonnement) par type de plateforme que vous pouvez installer sur tous vos appareils.</p>

<p align="left">Sur le Dashboard, notez votre ID d’utilisateur en haut à droite :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-110.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-100.png" alt="image" width="350" height="273" border="0" /></a></p>

<p align="left">Pour finir, vous devez déclarer une application dans Pushover. Pour cela sur le dashboard, cliquez sur “<a href="https://pushover.net/apps/build">Register an Application/Create an API Token</a>” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-111.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-101.png" alt="image" width="350" height="273" border="0" /></a></p>

<p align="left">Vous pourrez alors spécifier le nom de votre application, le logo (qui sera utilisé sur vos appareils), etc..</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-112.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-102.png" alt="image" width="350" height="273" border="0" /></a></p>

<p align="left">Sur la page de detail vous pourrez suivre la consommation/stat de votre application et surtout récupérer l’API Token indispensable pour la configuration du package Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-113.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-103.png" alt="image" width="350" height="273" border="0" /></a></p>

<h4>Installation du package dans Constellation</h4>

Depuis le “Online Package Repository” de votre Console Constellation, déployez le package Puhover :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-104.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-96.png" alt="image" width="350" height="214" border="0" /></a></p>

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous devez obligatoirement définir le votre l’API Token délivré par Pushover et l’ID de votre utilisateur Pushover qui sera le destinataire par défaut des notifications envoyés par le package.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-105.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-97.png" alt="image" width="350" height="246" border="0" /></a></p>

Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :

<pre class="lang:html5 decode:true">&lt;package name="Pushover"&gt;
  &lt;settings&gt;
    &lt;setting key="Token" value="&lt;&lt; Pushover API token &gt;&gt;" /&gt;
    &lt;setting key="UserId" value="&lt;&lt; Pushover User Id &gt;&gt;" /&gt;
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
<td valign="top" width="10"><strong>Token</strong></td>
<td valign="top" width="10">ConfigurationSection(XML)</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">API Token délivré par Pushover</td>
</tr>
<tr>
<td valign="top" width="10"><strong>UserId</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">ID de votre utilisateur Pushover destinataire par défaut des notifications envoyés par le package</td>
</tr>
<tr>
<td valign="top" width="10"><strong>DefaultEmergencyRetry</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 60</td>
<td valign="top" width="456">Intervalle de temps en seconde où Pushover vous renverra les notifications “urgentes” tant qu’une confirmation de lecture (un acquittement) n’est pas envoyée. Par défaut les notifications “urgentes” sont renvoyées toutes les minutes.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>DefaultEmergencyExpiration</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel
Par défaut : 3600</td>
<td valign="top" width="456">Intervalle de temps où Pushover considéra la notification “urgente” expirée si aucune confirmation de lecture (un acquittement) n’est envoyée. Par défaut, au bout d’une heure, Pushover arrêtera de vous notifier.</td>
</tr>
</tbody>
</table>

<h4>Les StateObjects</h4>

Vous retrouverez autant de StateObject que de compteur de performance enregistrés dans la configuration de votre package.

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>RateLimits</strong></td>
<td valign="top" width="10">Pushover.RateLimit</td>
<td valign="top" width="446">Objet indiquant les limites liées à votre abonnement. Par défaut, vous disposez de 7500 notifications gratuitement par mois.</td>
</tr>
</tbody>
</table>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-106.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-98.png" alt="image" width="350" height="109" border="0" /></a></p>

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
<td valign="top" width="10"><strong>CheckUserOrGroup</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Vérifie l’ID d’un utilisateur ou d’un groupe Pushover.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>GetNotificationStatus</strong> <i></i></td>
<td valign="top" width="141">Pushover.PushoverReceipt</td>
<td valign="top" width="407">Vérifie le statuts d’une notification “urgente”. L’objet “PushoverReceipt” vous indiquera entre autre la date d’acquittement de la notification, l’utilisateur qui l’a acquitté et sur quel appareil, etc..</td>
</tr>
<tr>
<td valign="top" width="10"><strong>PushNotification</strong></td>
<td valign="top" width="141">Pushover.PushoverResponse</td>
<td valign="top" width="407">Permet d’envoyer une notification Pushover. Vous pourrez spécifier le titre, le son de la notification, sa priorité, l’utilisateur ou l’appareil ciblé, ou encore la date de la notification.</td>
</tr>
</tbody>
</table>

<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-107.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-99.png" alt="image" width="350" height="257" border="0" /></a></h3>

<h3 align="left">Quelques tests depuis la Console Constellation</h3>

<h4 align="left">Envoyer une notification</h4>

<p align="left">Pour tester son bon fonctionnement, depuis le MessageCallbacks Explorer de la Console Console, recherchez le MC “PushNotification” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-114.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-104.png" alt="image" width="350" height="290" border="0" /></a></p>

<p align="left">Ce MessageCallback prend plusieurs paramètres dont beaucoup optionnels. Le seul obligatoire étant le “message”.</p>

<p align="left">Par exemple écrivons comme message “Hello world” et cliquons sur “Invoke” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-115.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-105.png" alt="image" width="350" height="69" border="0" /></a></p>

<p align="left">Comme le message est envoyé dans une saga, la Console vous affichera la réponse du package dans la barre de notification du haut :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-116.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-106.png" alt="image" width="350" height="150" border="0" /></a></p>

<p align="left">Le message de retour de cette saga vous indiquera le statuts de la notification. Vous avez donc la possibilité dans vos applications/objets de savoir si la notification a bien été envoyée :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-117.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-107.png" alt="image" width="350" height="238" border="0" /></a></p>

<p align="left">Ici sur mon smartphone Android, la notification est bien réceptionnée :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-118.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-108.png" alt="image" width="350" height="219" border="0" /></a></p>

<h4 align="left">Envoyer une notification “urgente” avec acquittement</h4>

<p align="left">Testons maintenant une notification “urgente”, c’est à dire que la notification sera retransmise à intervalle régulier tant qu’un l’utilisateur ne l’acquitte pas.</p>

<p align="left">Pour cela sélectionnez “Emergency” pour le paramètre “priority”. Vous pouvez également définir les champs retry/expire autrement ceux sont les valeurs de la configuration du package qui seront utilisées.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-119.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-109.png" alt="image" width="350" height="192" border="0" /></a></p>

<p align="left">Dans le message de retour de la saga vous obtiendrez l’ID du reçu.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-120.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-110.png" alt="image" width="350" height="242" border="0" /></a></p>

<p align="left">Sur un Android, la notification est accompagnée d’un bouton pour acquitter la notification.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-121.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-111.png" alt="image" width="350" height="198" border="0" /></a></p>

<p align="left">Toujours dans le MessageCallbacks Explorer de la Console Constellation, vous pouvez invoquer le MC “GetNotificationStatus” en spécifiant l’ID du reçu :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-122.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-112.png" alt="image" width="350" height="85" border="0" /></a></p>

<p align="left">Le message de retour nous indique que la notification a bien été délivrée (Status = 1) mais n’est pas encore acquittée.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-123.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-113.png" alt="image" width="350" height="238" border="0" /></a></p>

<p align="left">Acquittez ensuite la notificaiton sur votre appareil Android ou iOS et invoquez de nouveau le MC “GetNotificationStatus”  :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-124.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-114.png" alt="image" width="350" height="242" border="0" /></a></p>

<p align="left">Cette fois ci on constate bien que la notification a bien été acquitté (a telle heure, par tel utilisateur et sur tel appareil, ici le XperiaZ5).</p>

<h3 align="left">Quelques exemples</h3>

<ul>
    <li>
<div align="left">Envoyer une notification urgente si votre site Web est offline avec un package C#</div></li>
</ul>