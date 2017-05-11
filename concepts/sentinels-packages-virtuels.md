---
ID: 2141
post_title: 'Sentinelles et Packages &quot;virtuels&quot;'
author: Sebastien Warin
post_date: 2016-08-09 13:29:33
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/sentinels-packages-virtuels/
published: true
post_modified: 2017-05-11 16:21:07
---
Constellation met en œuvre des sentinelles (agent de déploiement) permettant de déployer des packages assurant ainsi les taches de supervision et d’administration, c'est ce que l'on appelle l'Orchestration.

Tout cela est possible sur de véritables systèmes d’exploitation, tel que Windows ou Linux, car en effet un package n’est ni plus ni moins qu’une application ou un programme (graphique ou non) déployé sur une sentinelle. La vie d’un package est ainsi contrôlée par Constellation.

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/constellation-proto-board.png"><img class=" wp-image-819 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/constellation-proto-board-300x102.png" alt="" width="450" height="153" /></a>

Seulement ce mécanisme n’est pas toujours possible. Si vous prenez un Arduino/ESP, NetDuino/Gadgeteer ou équivalent, il n’y a pas de notion de “processus”. Le programme est directement "téléversé" lors de l’opération de programmation de la puce. Une fois débranché, le microprogramme fonctionne de manière autonome mais il n’est pas possible de le modifier.

Autre exemple : un script Powershell. Le “programme” est dans le script lui-même. Il n’a pas de capacité de s’auto-programmer.

C’est pour cela que Constellation apporte cette notion de “sentinelles et packages <strong>virtuels</strong>”. En effet, une sentinelle virtuelle n'existe pas dans le sens il n'y a pas de "sentinelle" capable de déployer un package. Cette notion existe seulement décrire l'identité du package "virtuel". Le package existe dans le sens où il fait partie de l'objet ou du script, mais il n'est ni stocké et ni déployé par la Constellation, il est "virtuel".

Prenez l’exemple d’une sonde de température réalisée avec un Arduino/ESP8266 et connectée à Constellation.

Vous allez déclarer dans votre Constellation une sentinelle “virtuelle” nommée “CapteurSalon”. Cette sentinelle "contiendra" (virtuellement) un package virtuel nommé “Temperature”. Ce package est virtuel car il n’y a aucun package de ce nom dans le repository de votre serveur.

Dans votre microprogramme Arduino/ESP, vous allez identifier votre Arduino comme étant la sentinelle “CapteurSalon” exécutant le package “Temperature”.

Vous serez alors connecté à Constellation et votre package “virtuel” pourra exploiter les différentes fonctionnalités de la Constellation :

<ul>
    <li>Récupération des settings définis sur le serveur</li>
    <li>Envoi des logs</li>
    <li>Production de StateObjects</li>
    <li>Exploitation des StateObjects de la Constellation</li>
    <li>Echange de messages pour l’invocation de méthode (les MessageCallbacks)</li>
</ul>

D'un point vue Constellation, il s'agit d'un véritable package. Le package se nomme "Temperature" et est exécuté par la sentinelle "CapteurSalon". Il s'agit en tout point d'un véritable package à la différence qu'il n'y pas la partie “orchestration”, c’est à dire qu’on ne pourra pas l’arrêter/démarrer à distance ce package ou encore déployer de nouvelles versions du package depuis Constellation.