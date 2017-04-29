---
ID: 2279
post_title: 'Les diff&eacute;rents hubs et interfaces REST du serveur Constellation'
author: Sebastien Warin
post_date: 2016-08-12 15:19:15
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/les-differents-hubs-et-interfaces-rest-du-serveur-constellation/
published: true
post_modified: 2016-12-18 15:22:58
---
Le serveur Constellation est accessible en HTTP ou <a href="/constellation-platform/constellation-server/configuration-ssl/">HTTPS</a> sur <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_listenUris">une ou plusieurs URI</a>.

Il héberge des hubs et des interfaces HTTP/REST. De plus il peut également héberger des fichiers statiques en configurant le “<a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_fileServer">fileServer</a>” ce qui permet par exemple l’auto-hébergement de la console Constellation (<a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_fileServer">plus d’info ici</a>).

Les hubs Constellation sont basés sur la technologie SignalR et exposent des canaux de communication bi-directionnels et temps réel en HTTP long-polling, server-event ou WebSocket en fonction des capacités du client.

Les interfaces HTTP/REST sont en fait des contrôleurs WebAPI exposant des méthodes en HTTP/REST. Les interfaces HTTP/REST permettent d’exposer des fonctionnalités de Constellation de manière très simple pour les devices ou frameworks ne pouvant supporter SignalR.

Il existe 4 hubs :
<ul>
 	<li>Constellation</li>
 	<li>Consumer</li>
 	<li>Controller</li>
 	<li>Sentinel</li>
</ul>
Ainsi que 3 interfaces HTTP/REST :
<ul>
 	<li>Constellation</li>
 	<li>Consumer</li>
 	<li>Management</li>
</ul>
Le hub et l’interface HTTP “Constellation” sont utilisés par les packages Constellation (réels ou virtuels). Ils exposent des méthodes pour écrire des logs, pour récupérer les settings, pour publier ou interroger des StateObjects, envoyer ou recevoir des messages, etc…

Le hub et l’interface HTTP “Consumer” sont utilisés par les “consommateurs”. Par exemple une page Web est un consommateur, c’est à dire qu’elle se connecte à Constellation pour interroger des StateObjects ou envoyer/recevoir des messages mais elle n’est pas un package (son cycle de vie est lié au navigateur du client). Un “consommateur” ne peut pas produire des logs, avoir de settings ou encore publier des StateObjects.

Le hub “Controller” expose des méthodes pour contrôler la Constellation comme arrêter/démarrer des packages, s’abonner et récupérer en temps réel les logs des packages de la Constellation, propager les changements de configuration dans la Constellation, suivre les états et consommation des ressources des packages, etc… Ce hub est notamment utilisé par la Console Constellation pour vous fournir une interface de pilotage de votre Constellation.

Le hub “Sentinel” est un hub réservé aux sentinelles de la Constellation. C’est sur ce hub que les sentinelles s’enregistrent, remontent les états des packages qu’elles supervisent, etc…

Enfin l’interface HTTP “Management” aussi nommée “Management API” est une API REST de management du serveur permettant la gestion des sentinelles, des packages, des settings, des credentials, etc… Cette API est notamment utilisée par la Console Constellation pour vous fournir une interface d'administration de votre Constellation.