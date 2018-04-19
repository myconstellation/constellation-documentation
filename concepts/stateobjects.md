---
ID: 2145
post_title: Les StateObjects
author: Sebastien Warin
post_date: 2016-08-09 13:31:32
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/stateobjects/
published: true
publish_post_category:
  - "12"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:18:44
---
Les StateObjects sont des objets de données produits et publiés par des packages qu’ils soient “réels” ou “virtuels” d'une Constellation.

Ils représentent des variables de type simple (numérique, chaîne de caractère, booléen, etc..) ou de type complexe (objet formaté en JSON).

Les StateObjects sont tous stockés sur le serveur Constellation et peuvent être interrogés par n’importe quels packages ou consommateurs de la Constellation (sauf si des autorisations restreignent l’accès).
<h3>Le StateObject</h3>
Chaque StateObject comporte :
<ul>
 	<li>Le nom de la sentinelle et du package qui a produit le StateObject</li>
 	<li>Un nom (la clé d'un StateObject)</li>
 	<li>Une valeur</li>
 	<li>Une date de mise à jour</li>
 	<li>Une durée de vie en seconde (“0” si infinie)</li>
 	<li>Un type</li>
 	<li>Des métadatas (dictionnaire de clé / valeur)</li>
</ul>
Dans la philosophie Constellation, chaque package (<a href="/concepts/sentinels-packages-virtuels/">virtuel ou non</a>) publie des StateObjects représentant des "variables publiques".

Par exemple un capteur de température Arduino publiera des StateObjects sur la température ou humidité qu’il relève, un package comme le “HWMonitor” publiera des StateObjects sur les compteurs hardware qu’il mesure (consommation CPU, RAM, etc..), ou encore un package comme celui “Paradox” publiera des StateObjects pour chaque zone d’un système d’alarme où chaque StateObject contiendra un objet indiquant l’état de la zone (ouverte/fermée, en alarme ou non, en défaut ou non, etc..).

Le "<a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a>" de la Console Constellation permet d’explorer l’ensemble des StateObjects de la Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-27.png"><img class="alignnone" title="Le StateObjects Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-26.png" alt="Le StateObjects Explorer" width="350" height="176" border="0" /></a></p>
<p align="left">Avec la possibilité de visualiser tous les détails et de vous abonner aux mises à jour en temps réel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-28.png"><img class="alignnone" title="Détail d'un StateObject" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-27.png" alt="Détail d'un StateObject" width="350" height="251" border="0" /></a></p>
<p style="text-align: left;" align="center">La capture ci-dessus montre un StateObject produit par le package “NetAtmo” qui représente le détail d’un capteur NetAtmo (Id, version du firmware, nom du capteur, statuts de la communication, niveau de la batterie, etc..).</p>
<p style="text-align: left;" align="center">Comme chaque package peut publier des StateObjects dans Constellation, il est possible en interrogeant les StateObjects de votre Constellation de connaitre en temps réel l’état de tous vos systèmes (ou du moins “le dernier état connu”).</p>
<p style="text-align: left;" align="center">A noter que l’unicité d’un StateObject est obtenu par le triplet : “Sentinel + Package + Nom” car un nom de StateObject est unique pour une <a href="/concepts/instance-package-versioning-et-resolution/#Sentinel_Package_Instance_de_package">instance de package</a> (couple Sentinel / Package), de même qu’un package est un unique pour une sentinelle et qu’une sentinelle est unique dans une Constellation.</p>

<h3>Interrogation, abonnement et filtre</h3>
Dans les librairies Constellation (<a href="/client-api/javascript-api/">JavaScript</a>, <a href="/client-api/net-package-api/">.NET</a>, <a href="/client-api/python-api/">Python</a>, <a href="/client-api/arduino-esp-api/">Arduino</a>, etc..) et d’un point de vue plus générique avec les <a href="/client-api/rest-api/">interfaces REST</a> Constellation vous allez retrouver les mêmes fonctionnalités :
<ul>
 	<li>Le “Request” de StateObject</li>
 	<li>Le “Subscribe” de StateObject</li>
</ul>
Le “Request” permet de récupérer la dernière valeur connue d’un ou de plusieurs StateObjects.

Le ”Subscribe” permet de créer un abonnement sur un ou plusieurs StateObjects. Une fois abonné, vous serez notifié en temps réel des qu’un StateObject de votre abonnement est mis à jour.

Dans ces deux méthodes vous serez amené à définir le ou les StateObjects que vous souhaitez récupérer ou suivre. Pour cela vous devez définir un filtre en indiquant :
<ul>
 	<li>Le nom de la sentinelle du StateObject</li>
 	<li>Le nom du package du StateObject</li>
 	<li>Le nom du StateObject</li>
 	<li>Le type du StateObject</li>
</ul>
Pour chaque filtre vous pourrez appliquer le wildcard “*” pour accepter toutes les valeurs possibles.

Ainsi appliquer le filtre */*/*/* sélectionnera tous les StateObjects de votre Constellation. N’oubliez pas que l’unicité d’un StateObject est obtenue par le triplet : “Sentinel + Package + Nom”.

Quelques exemples :
<ul>
 	<li>*/Demo/*/* : sélectionne tous les StateObjects du package “Demo” quelque soit la sentinelle</li>
 	<li>MonPC/Demo/*/* : sélectionne tous les StateObjects du package “Demo” de la sentinelle “MonPC”</li>
 	<li>MonPC/*/*/* : tous les StateObjects produits sur la sentinelle MonPC</li>
 	<li>MonPC/*/*/MyData : tous les StateObjects de type “MyData” et produits sur la sentinelle MonPC</li>
 	<li>MonPC/Demo/ABC/* : le StateObject ABC du package Demo sur la sentinelle MonPC</li>
</ul>