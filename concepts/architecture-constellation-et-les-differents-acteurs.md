---
ID: 2139
post_title: 'L&#8217;architecture Constellation et les différents acteurs : sentinelle, package, contrôleur et consommateur'
author: Sebastien Warin
post_date: 2016-08-09 13:28:55
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/architecture-constellation-et-les-differents-acteurs/
published: true
post_modified: 2017-05-11 16:18:01
---
Une Constellation est constituée des éléments suivants :

<ul>
    <li>Un serveur Constellation</li>
    <li>Des sentinelles Constellation</li>
    <li>Des packages déployés sur ces sentinelles</li>
    <li>Des contrôleurs et consommateurs qui peuvent se connecter au serveur</li>
</ul>

<h3>Les Sentinelles</h3>

Une sentinelle est un agent de déploiement pour Constellation. Concrètement il s’agit d’une application (ou service) installée sur un système Windows ou Linux et qui tourne en tache de fond.

Le rôle d'une sentinelle est d’exécuter les ordres provenant du serveur Constellation pour :

<ul>
    <li>Déployer des packages, c’est à dire télécharger des packages (= des programmes) depuis le serveur et les démarrer localement</li>
    <li>Contrôler les packages déployés sur la sentinelle (démarrer, arrêter ou redémarrer un package)</li>
    <li>Remonter les informations au serveur Constellation sur chaque package (état, consommation des ressources CPU et RAM, etc..)</li>
</ul>

Une fois une sentinelle installée et enregistrée dans une Constellation, il est possible depuis l’API ou la Console de suivre son état.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-6.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Sentinelles Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-6.png" alt="Sentinelles Constellation" width="244" height="126" border="0" /></a></p>

<h3>Les packages</h3>

Les packages sont des applications graphiques ou non graphiques (services) stockées dans le “Package Repository” du serveur Constellation et qui seront déployés sur des sentinelles de la Constellation.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-7.png" alt="image" width="244" height="78" border="0" /></a></p>

Vous pouvez déployer autant de package que vous souhaitez sur chaque sentinelle. Une fois le package démarré, vous retrouverez son état et sa consommation de ressource dans la Console ou via l’API. Vous pourrez également l’arrêter, le redémarrer ou forcer la mise à jour du package.

Le package lui-même remonte ses logs dans Constellation et récupère ses paramètres de configuration (les Settings) depuis Constellation.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-8.png" alt="image" width="244" height="116" border="0" /></a></p>

Ainsi si vous souhaitez changer une variable de configuration de l’un de vos packages, vous pouvez le faire directement depuis l’API ou la Console et propager cette mise à jour au package en question.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-9.png" alt="image" width="244" height="184" border="0" /></a></p>

<p align="left">Chaque package peut produire des données publiées dans ce que l'on nomme des "<a href="/concepts/stateobjects/">StateObjects</a>" qui seront accessibles par les autres packages et les consommateurs (comme une page Web) de sa Constellation. De même un package peut consommer les StateObjects produits par les autres packages de la Constellation.</p>

<p align="left">Enfin chaque package peut échanger des messages avec les autres packages ou consommateurs connectés sur les hubs Constellation ce qui permet par exemple d’invoquer des méthodes sur d’autre système (ce que l'on nomme les <a href="/concepts/messaging-message-scope-messagecallback-saga/">MessageCallbacks</a>).</p>

<p align="left">A noter qu'il existe aussi une notion de "package virtuel" expliquée à <a href="/concepts/sentinels-packages-virtuels/">la page suivante</a>.</p>

<h3 align="left">Les consommateurs</h3>

<p align="left">Le serveur Constellation expose une interface REST ainsi qu'un hub nommé "<a href="/concepts/les-differents-hubs-et-interfaces-rest-du-serveur-constellation/">Consumer</a>" à destination des "consommateurs".</p>

<p align="left">Une page Web par exemple est un consommateur, c’est à dire qu’elle se connecte à Constellation pour interroger des StateObjects ou envoyer/recevoir des messages mais elle n’est pas considéré comme un package dans le sens où une page Web n'est pas un programme déployé et démarré par une sentinelle (son cycle de vie est lié au navigateur du client). Un “consommateur” ne peut pas produire des logs ni avoir de settings ou encore publier des StateObjects. Il consomme ni plus ni moins !</p>

<h3 align="left">Les contrôleurs</h3>

<p align="left">Le serveur Constellation expose un hub nommé “<a href="/concepts/les-differents-hubs-et-interfaces-rest-du-serveur-constellation/">Controller</a>” permettant contrôler des éléments de la Constellation comme arrêter/démarrer des packages, s’abonner et récupérer en temps réel les logs des packages de la Constellation, propager les changements de configuration dans la Constellation, suivre l' état et la consommation des ressources de chaque packages, etc…</p>

<p align="left">Ce hub est notamment utilisé par la Console Constellation pour vous fournir une interface de pilotage de votre Constellation.</p>

<p align="left">Pour se connecter sur ce hub, le credential utilisé pour l’authentification doit avoir la clé "<a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">EnableControlHub</a>" activée.</p>