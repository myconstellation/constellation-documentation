---
ID: 1349
post_title: 'Messaging : Message, Scope, MessageCallback et Saga'
author: Sebastien Warin
post_date: 2016-03-16 15:40:29
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/messaging-message-scope-messagecallback-saga/
published: true
publish_post_category:
  - "12"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:17:43
---
Tous systèmes connectés dans Constellation (les packages réels ou virtuels et les consommateurs) peuvent tous envoyer ou recevoir des messages (sauf si des autorisations restreignent cela).
<h3>Message &amp; Scope</h3>
Les notions de base du Messaging de Constellation sont :

Un message :
<ul>
 	<li>porte obligatoirement une clé (le MessageKey), en quelque sorte l’équivalent du “Sujet” d’un mail.</li>
 	<li>peut contenir optionnellement un contenu, nommé “Data”</li>
 	<li>est envoyé à un scope.</li>
</ul>
Un scope :
<ul>
 	<li>défini les destinataires d’un message</li>
 	<li>peut être de différents types :
<ul>
 	<li>All : c’est à dire que tous les systèmes connectés dans Constellation recevront le message (y compris l’émetteur)</li>
 	<li>Other : tout le monde sauf l’émetteur</li>
 	<li>Package : cible un ou plusieurs packages</li>
 	<li>Sentinel : cible tous les packages hébergés sur les sentinelles visées par le scope</li>
 	<li>Group : cible les packages ou consommateurs appartenant aux groupes visés par le scope</li>
</ul>
</li>
</ul>
Les scopes “Package”, “Sentinel” et “Group” peuvent déclarer une liste de packages, de sentinelles et de groupes.
<ul>
 	<li>Lorsque l’on cible une sentinelle, on vise tous les packages qu’elle héberge.</li>
 	<li>Lorsque l’on cible un package, on vise toutes les instances de ce package sur les sentinelles de votre Constellation.</li>
</ul>
Si vous voulez cibler un seul package en particulier, vous devez obligatoirement créer un scope de type Package et définir le nom du package <strong>préfixé</strong> du nom de la sentinelle avec la nomenclature “SENTINEL/Package”.

Chaque package ou consommateur peut s’inscrire à un groupe ou être inscrit d’office dans la configuration du serveur (lire <a href="/constellation-platform/constellation-server/fichier-de-configuration/">l’article sur les groupes</a>).
<h3>MessageCallback</h3>
Les messages dans Constellation servent essentiellement à invoquer des méthodes. On appelle cela des “MessageCallbacks” le fait de lier un message à une fonction de votre programme.

Il s’agit tout simplement déclarer une méthode de votre code comme "MessageCallback", c’est à dire que si le package reçoit un message dont la clé du message est le nom d’une méthode déclarée comme MessageCallback, alors cette méthode sera invoquée. Le contenu du message reçu (c’est à dire les “Datas”)  contiendra les paramètres de la méthode.

De la même façon, pour invoquer une méthode d’un autre package, il suffit d’envoyer un message dont la clé est la méthode à invoquer avec comme contenu de message les paramètres à passer à cette méthode.
<h3>Les Sagas</h3>
Chaque message envoyé dans Constellation est à sens unique. Il n’y a aucun retour.

Un message est envoyé à un scope,  il peut ne jamais être reçu (si "personne" n’est visée par le scope) ou à l’inverse être reçu par tout le monde.

Cependant il y a certain cas où il est indispensable d’obtenir une réponse à un message ! Comme en programmation, il n’y a pas que des procédures, il y a aussi des fonctions.

Pour cela, Constellation propose le concept de “Saga”. Une saga est un identifiant porté par un scope. Pour faire l’analogie, “<em>La guerre des étoiles</em>”, “<em>L'Empire contre-attaque</em>" ou “<em>Le Retour du Jedi</em>” sont trois films distincts qui portent le même identifiant de Saga : “Star Wars”.

Il en va de même pour les messages Constellation. Pour “répondre”, il suffit d’envoyer un message “de réponse” à un scope qui vise l’émetteur du message original en prenant soin d’utiliser le même identifiant de saga.

Ainsi l’émetteur en recevant ce message, pourra savoir qu’il s’agit de la réponse à son message car il retrouvera le même identifiant de saga.

Bien entendu l’identifiant de Saga doit être unique et aléatoire pour chaque couple “Request / Response” .

De ce fait, lorsque l’on reçoit un message, si le scope porte un identifiant de Saga cela veut dire que l’émetteur a envoyé le message “dans une Saga”, il s’attend donc à recevoir une réponse. Le clé d’un message de réponse est toujours “__Response”.
<h3>Auto-description des MessageCallbacks</h3>
Chaque packages (virtuel ou non) doit ou devrait déclarer son “Package Descriptor”.

Il s’agit de la description du package qui contient entre autre les MessageCallbacks du package, c’est à dire les méthodes que le package expose dans la Constellation.

Le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a> de la Console Constellation exploite les “Package Descriptors” connus pour créer la liste de l'ensemble des MessageCallbacks exposés par les package d'une Constellation avec le détail des paramètres en entrée et le type de retour. Il est également possible d'invoquer ces MC avec une interface de test et de générer le code pour invoquer ces MC sur les différentes API de Constellation.

<a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-59.png"><img class="wp-image-4600 aligncenter" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-59-300x91.png" alt="MessageCallbacks Explorer" width="323" height="98" /></a>