---
ID: 2461
post_title: >
  Envoyer des messages et invoquer des
  MessageCallbacks depuis un Arduino/ESP
author: Sebastien Warin
post_date: 2016-08-23 14:20:13
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/arduino-esp-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-depuis-arduino-esp/
published: true
post_modified: 2017-05-05 16:47:06
---
<h3>Envoyer des messages</h3>

Pour envoyer des <a href="/concepts/messaging-message-scope-messagecallback-saga/">messages et invoquer des méthodes (MessageCallbacks)</a> d’autres packages (ou consommateurs, pages Web, apps mobile, etc… ), vous pouvez utiliser la méthode “sendMessage” :

<pre class="lang:cpp decode:true">constellation.sendMessage(scope, args, messageKey, data);</pre>

C’est une fonction “variadic“ qui peut prendre plusieurs arguments qui seront utilisés pour formater le contenu (data) du message :

<pre class="lang:cpp decode:true">constellation.sendMessage(scope, args, messageKey, data, ...);</pre>

Par exemple pour envoyer un message “DemoMessage” avec en contenu un objet contenant deux propriétés (c’est à dire invoquer le MessageCallback “DemoMessage” en passant en paramètre un objet à deux propriétés) au package “MyCsharpPackage“ :

<pre class="lang:cpp decode:true">constellation.sendMessage(Package, "MyCsharpPackage", "DemoMessage", "{ A: '%s', B: %d  }",  myStringVal, myIntVal);</pre>

Autre exemple : envoyer un message “SignalReceive” au groupe “IR” (dans lequel plusieurs packages ou consommateurs peuvent s’inscrire) avec en paramètre une valeur de type ”long” formatée en hexadécimal :

<pre class="lang:cpp decode:true">unsigned long value = results.value; // the IR signal decoded
constellation.sendMessage(Group, "IR", "SignalReceive", "%#x", value);</pre>

<h4>Le scope</h4>

Le scope permet de définir les destinataires du message (<a href="/concepts/messaging-message-scope-messagecallback-saga/">plus d’information ici</a>).

Dans la libraire Arduino/SP, l’argument Scope est une énumération définie de la façon suivante :

<pre class="lang:lua decode:true">enum ScopeType : uint8_t {
  None = 0,
  Group = 1,
  Package = 2,
  Sentinel = 3,
  Other = 4,
  All = 5
};
</pre>

L’argument “args” spécifie les arguments du scope, par exemple le ou les noms des packages, des sentinelles ou des groupes où envoyer le message. Pour cibler une instance d’un package vous spécifierez le nom de l’instance complet “SENTINEL/Package” (<a href="/concepts/messaging-message-scope-messagecallback-saga/">plus d’information ici</a>).

Si vous avez plusieurs “args” vous devez les séparer par des virgules. Par exemple pour envoyer un message “HelloWorld” au groupe A et au groupe B :

<pre class="lang:cpp decode:true">constellation.sendMessage(Group, "A,B", "HelloWorld", "{}");</pre>

<h4>Le contenu (Data) du message</h4>

Dans la <a href="/concepts/messaging-message-scope-messagecallback-saga/">philosophie des MessageCallbacks</a>, le contenu du message (Data) permet de stocker les arguments d’une fonction (MessageCallback) à invoquer.

Si vous invoquez un MC sans paramètre, vous devez spécifier un objet vide “{}” :

<pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "HelloWorld", "{}");</pre>

Si vous avez plusieurs paramètres à passer, vous formaterez vos arguments dans un tableau :

<pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "[ 'Sebastien', 'Warin' ]");</pre>

Vous pouvez également envoyer en tant qu’argument un objet complexe, par exemple :

<pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "{ 'FirstName':'Sebastien', 'LastName':'Warin' }");</pre>

Ou bien même un mixte, par exemple un MC prenant en paramètre un objet complexe et un entier :

<pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "[ { 'FirstName':'Sebastien', 'LastName':'Warin' }, 42 ]");</pre>

Ici le contenu du message est formaté en JSON via une chaine de caractère mais vous pouvez également passer un JsonObject à cette méthode :

<pre class="lang:cpp decode:true">const int BUFFER_SIZE = JSON_OBJECT_SIZE(2);
StaticJsonBuffer&lt;BUFFER_SIZE&gt; jsonBuffer;
JsonObject&amp; data = jsonBuffer.createObject();
myStateObject["FirstName"] = "Sebastien";
myStateObject["LastName"] = "Warin";
constellation.sendMessage(Package, "DemoPackage", "SayHello", data);</pre>

Notez aussi que la Console Constellation liste les MessageCallbacks déclarés par l’ensemble des packages de votre Constellation et vous génère le code pour chaque API.

Par exemple, si vous souhaitez invoquer le MessageCallback “PushNote” du package “PushBullet” qui vous permettra d’envoyer une notification sur un smartphone vous pouvez cliquez sur l’icone de génération de code :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-66.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-55.png" alt="image" width="350" height="147" border="0" /></a></p>

<p align="left">Sélectionnez ensuite le langage “Arduino” pour avoir le modèle correspondant :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-67.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-56.png" alt="image" width="350" height="220" border="0" /></a></p>

Ainsi pour envoyer une notification sur mon smartphone depuis un Arduino/ESP :

<pre class="decode:true lang:cpp">constellation.sendMessage(Package, "PushBullet", "PushNote", "[ 'Demo from Arduino', 'Ceci est une demo', 'Device', '' ]");</pre>

Bien entendu, n’importe quel package ou consommateur peut recevoir des messages. Vous pouvez donc invoquer des méthodes C#, Python, JS, etc.. depuis un Arduino/ESP.

<h3>Envoyer des messages avec réponse : les Sagas</h3>

Les <a href="/concepts/messaging-message-scope-messagecallback-saga/">Sagas</a> permettent de lier des messages et donc de créer des couples de “requêtes / réponses”.

Avec la libraire Arduino/ESP, il est possible d’envoyer des messages dans une saga afin d’enregistrer un callback de traitement de la réponse.

Par exemple, le package “NetworkTools” expose entre autre un MessageCallback “Ping” permettant de pinger une machine. Si le MC est invoqué dans une saga, le package répondra dans la saga un message contenant le temps de réponse du ping.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-19.png" alt="image" width="350" height="106" border="0" /></a></p>

De ce fait, si nous souhaitons invoquer cette méthode depuis notre Arduino et traiter la réponse, nous pouvons écrire :

<pre class="lang:cpp decode:true">const char* ip = "192.168.0.10";
constellation.sendMessageWithSaga([](JsonObject&amp; json) {
    constellation.writeInfo("Ping response in %s ms", json["Data"].asString()); 
  }, Package, "NetworkTools", "Ping", "[ '%s' ]", ip);</pre>

La signature de la méthode “<em>sendMessageWithSaga</em>” est la même que la méthode “<em>sendMessage</em>” présentée ci-dessus à la seule différence qu’elle prend comme 1er argument un callback vers une fonction traitant la réponse ici passée en tant que lambda.

Dans l’exemple ci-dessus, votre Arduino envoie donc un message “Ping” avec une adresse IP en paramètre au(x) package(s) “NetworkTools” en associant un n° de saga. Ce package exécutera le ping et vous retournera un message de réponse avec le résultat du ping. Cette réponse sera traitée par la fonction lambda qui dans l’exemple affichera dans les logs Constellation le résultat du ping.

Ainsi vos Arduino/ESP peuvent très facilement invoquer des méthodes (MessageCallback) avec ou sans retour des différents packages (virtuels ou non) ou consommateurs de votre Constellation; scripts Javascripts, Powershell, Bash, programmes .NET, Python, etc…