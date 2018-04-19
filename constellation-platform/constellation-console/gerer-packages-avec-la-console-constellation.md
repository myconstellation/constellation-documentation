---
ID: 2404
post_title: >
  Gérer les packages avec la Console
  Constellation
author: Sebastien Warin
post_date: 2016-08-19 16:31:54
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/
published: true
publish_post_category:
  - "21"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:03:36
---
<p align="left">Dans le menu principal de gauche, cliquez sur “Packages”. Vous obtiendrez alors l’ensemble des instances de package déployées dans votre Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-45.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-34.png" alt="image" width="450" height="249" border="0" /></a></p>

<p align="left">Pour chaque package (réel) vous avez en temps réel :</p>

<ul>
    <li>
<div align="left">Le statut :</div>
<ul>
    <li>
<div align="left">Starting : package en cours de démarrage</div></li>
    <li>
<div align="left">Started : package démarré et en cours de fonctionnement</div></li>
    <li>
<div align="left">Stopping : package en cours d’arrêt</div></li>
    <li>
<div align="left">Stopped : package arrêté</div></li>
</ul>
</li>
    <li>
<div align="left">L’état de connexion qui indique si le package est bien connecté au serveur</div></li>
    <li>
<div align="left">La date de la dernière activité (il s’agit de la date de la dernière modification du statut)</div></li>
    <li>
<div align="left">La consommation en CPU et RAM sur la sentinelle sur lequel le package est démarré</div></li>
    <li>
<div align="left">Des informations quant à la version du package et de la librairie Constellation utilisée par le package</div></li>
</ul>

<p align="left">En fonction de l’état du package le bouton d’action permettra de démarrer le package (Start) et de l’arrêter (Stop).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-48.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-37.png" alt="image" width="450" height="99" border="0" /></a></p>

<p align="left">Comme sur les autres pages de la Console, vous avez un menu contextuel pour chaque package vous permettant de contrôler le package, d’éditer sa configuration ou ses settings, etc.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-47.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-36.png" alt="image" width="200" height="338" border="0" /></a></p>

<p align="left">La fenêtre d’information permet de retrouver toutes les informations sur le statut du package :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-46.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-35.png" alt="image" width="450" height="223" border="0" /></a></p>

<p align="left">Notez également que vous avez la possibilité de filtrer dans la liste des packages soit avec la zone de texte ou soit en cliquant sur le bouton “Show filters” afin de filtrer le type de package (réel ou virtuel), l'état de connexion (connecté ou non) et l'état activé/désactivé. Par défaut tous les packages sont affichés à l’exception des packages marqués “Désactivé” (<em>Enable=false</em>).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-49.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-38.png" alt="image" width="450" height="62" border="0" /></a></p>

<h3 align="left">Contrôler un package</h3>

<p align="left">Pour pouvez pour chaque package le démarrer ou l’arrêter avec le bouton d’action sur la droite ou depuis le menu contextuel.</p>

<p align="left">Vous pouvez également redémarrer le package (restart) c’est à dire demander à la sentinelle de stopper le package (si il est “Started”)  puis de le démarrer.</p>

<p align="left">Vous pouvez aussi faire un rechargement du package : “reload”. Cette action fonctionne comme un “restart” mais force la sentinelle à (re)télécharger le package du “Package Repository” du serveur. Cela permet de déployer les mises à jours de vos packages : vous envoyez la nouvelle version de votre package sur votre serveur Constellation puis vous faites un “reload” afin de reforcer les sentinelles à redémarrer le package avec sa nouvelle version.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-50.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-39.png" alt="image" width="350" height="80" border="0" /></a></p>

<p align="left">Pour finir vous pouvez également “pousser” les settings vers un package en cas de modification (sans avoir à redémarrer le package).</p>

<h3>Editer la configuration d’un package</h3>

Pour chaque package (réel ou virtuel) vous pouvez saisir le nom de l’instance (seulement lors de la création) et le credential utilisé pour la connexion du package. Vous pouvez également définir si le package est activé ou non.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-51.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-40.png" alt="image" width="350" height="219" border="0" /></a></p>

<p align="left">S'il s’agit d’un package réel, vous pouvez définir si le package doit démarré automatiquement lorsque la sentinelle se démarre et vous pouvez également redéfinir les <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_recoveryOptions">options de récupération</a> :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-52.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-41.png" alt="image" width="350" height="472" border="0" /></a></p>

<h3>Editer les settings d’un package</h3>

Il s’agit des paramètres propre au package (<a href="/constellation-platform/constellation-server/fichier-de-configuration/#Les_parametres_de_configuration">voir ici</a>).

Pour chaque package vous définir autant de variable de configuration (= une clé et une valeur) que vous désirez en cliquant sur le bouton “Add new setting” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-53.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-42.png" alt="image" width="350" height="274" border="0" /></a></p>

<p align="left">Dans le cas d’un package dit “réel” il est grandement recommandé de déclarer les settings utilisés par le package dans son manifeste : <a href="/concepts/package-manifest/#Informations_sur_les_Settings_du_package">voir ici</a>.</p>

<p align="left">Cette déclaration permet à la Console Constellation de connaitre les settings obligatoires ou non, le type de chaque setting, la description et valeur par défaut.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-54.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-43.png" alt="image" width="263" height="180" border="0" /></a> <a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-55.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-44.png" alt="image" width="241" height="180" border="0" /></a></p>

<h3>Editer les groupes d’un package</h3>

Pour <a href="/concepts/messaging-message-scope-messagecallback-saga/">rappel</a>, un message peut être envoyé à un (ou plusieurs) groupe qui regroupe des packages ou des consommateurs. Chaque package ou consommateur peut joindre un groupe depuis les API mais vous pouvez également définir ces liens d’appartenance au niveau de la <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Les_groupes">configuration du serveur</a>.

Pour cela, cliquez sur “Group membership” dans le menu contextuel d’un page et définissez les groupes dont fait partie votre package :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-56.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-45.png" alt="image" width="350" height="204" border="0" /></a></p>

<h3>Déployer un package</h3>

Pour déployer un package, vous avez plusieurs solutions :

<ul>
    <li>Cliquez sur le bouton “Deploy new package” depuis la page “Packages”</li>
    <li>Cliquez sur le bouton “Deploy new package” dans le menu contextuel d'une sentinelle sur la page "Sentinels"</li>
    <li>Utiliser la page “Package Repository”</li>
</ul>

Depuis la page des Packages, cliquez sur le bouton “Deploy new package” en haut à gauche  :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-57.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-46.png" alt="image" width="240" height="130" border="0" /></a></p>

<p align="left">Vous obtiendrez alors une pop-in vous permettant de sélectionner la sentinelle sur laquelle déployer et le package à déployer :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-58.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-47.png" alt="image" width="350" height="115" border="0" /></a></p>

<p align="left">S'il s’agit d’un page virtuel, sélectionnez “Virtual package” (voir ci-dessous) sinon sélectionner un package présent dans votre “Package repository”.</p>

<p align="left">Vous serez alors guidé dans les étapes de configuration du package.</p>

<p align="left"><u>Note</u> : par défaut le champ “Package file” est nul, c’est à dire que le fichier à utiliser pour votre instance sera déterminé dynamiquement (<a href="/concepts/instance-package-versioning-et-resolution/">lire ici</a>).</p>

<p align="left">Dans le cas où une instance est déjà présente sur la sentinelle vous serez obligé de définir un autre nom et donc de spécifier le “Package file”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-59.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-48.png" alt="image" width="350" height="89" border="0" /></a></p>

<p align="left">Par exemple, ici je pourrais nommer cette nouvelle instance “HWMonitor2” en utilisant le fichier “HWMonitor”.</p>

<p align="left">Veuillez lire ceci pour bien comprendre le <a href="/concepts/instance-package-versioning-et-resolution/">mécanisme de résolution</a>.</p>

<h3>Les packages virtuels</h3>

Comme <a href="/concepts/sentinels-packages-virtuels/">expliqué ici</a>, un package virtuel n’est pas réel dans le sens où ce n’est pas un processus lancé et supervisé par une sentinelle.

De ce fait, dans la Console, vous n’aurez aucun information remontée à son sujet.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-60.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-49.png" alt="image" width="350" height="147" border="0" /></a></p>

<p align="left">Vous pouvez seulement les déclarer ou supprimer, les associer à des credenitals, gérer l’appartenance aux groupes ou bien sûr gérer les settings.</p>