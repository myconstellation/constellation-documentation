---
ID: 2368
post_title: 'G&eacute;rer les credentials avec la Console Constellation'
author: Sebastien Warin
post_date: 2016-08-19 15:56:33
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/
published: true
post_modified: 2016-11-03 17:54:59
---
Dans le menu principal de gauche, sous la section “Server Management”, rendez-vous sur la page “Manage Credentials” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-27.png" alt="image" width="350" height="120" border="0" /></a></p>
<p align="left">Vous obtiendrez sur la page la liste des credentials déclarés sur votre Constellation avec leurs clés d’accès associées, utilisées pour authentifier toutes requêtes vers votre Constellation.</p>
<p align="left">N’hésitez pas à lire ou relire la <a href="/concepts/securite-accesskey-credential-authorization/">page dédiée aux concepts de Credential / AccessKey et Authorization</a> avant de poursuivre !</p>
<p align="left">Pour chaque credential vous avez la possibilité d’activer ou désactiver la clé d’accès en cliquant sur le bouton “Disable” ou “Enable” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-28.png" alt="image" width="124" height="50" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-29.png" alt="image" width="124" height="48" border="0" /></a></p>
<p align="left">Attention ces actions sont immédiates. Si vous désactivez une clé d’accès actuellement utilisée par un ou plusieurs packages, leurs accès seront immédiatement révoqués.</p>
<p align="left">Vous pouvez également accéder au menu contextuel pour éditer le crédential, ses autorisation ou le supprimer.</p>
<p align="left"></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-30.png" alt="image" width="190" height="174" border="0" /></a></p>

<h3>Ajouter ou éditer un credential</h3>
Si vous cliquez sur le bouton “Add credential” ou “Edit” du menu contextuel d’une sentinelle, vous obtiendrez la fenêtre suivante :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-42.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-31.png" alt="image" width="350" height="273" border="0" /></a></p>
<p align="left"><u>Note</u> : dans le cas d’une édition vous ne pourrez pas éditer le nom du credential.</p>
<p align="left">Vous pouvez cependant éditer en toute circonstance la clé d’accès et même utiliser un générateur login/password comme <a href="/concepts/securite-accesskey-credential-authorization/#Login_et_AccessKey">expliqué ici</a>.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-43.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-32.png" alt="image" width="350" height="353" border="0" /></a></p>
<p align="left">Pour rappel, chaque credential porte les propriétés suivantes :</p>

<ul>
 	<li><u>Enable</u> : indique si le credential est activé ou désactivé</li>
 	<li><u>EnableControlHub</u> : indique si le credential peut se connecter sur le hub de contrôle (pour l’administration de la Constellation)</li>
 	<li><u>EnableManagementAPI</u> : indique si le credential peut se connecter à l’API de Management (pour l’édition de la configuration)</li>
 	<li><u>EnableDeveloperAccess</u> : indique si le credential peut utiliser la sentinelle virtuelle “Developer” pour debugger des packages (utilisé par le SDK Visual Studio)</li>
</ul>
<h3>Gérer les autorisations</h3>
En cliquant sur “Edit Authorizations” vous pourrez gérer les autorisations d’un credential pour limiter l’envoi de message, les StateObjects autorisés à consommer et les groupes.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-44.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-33.png" alt="image" width="350" height="238" border="0" /></a></p>