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
post_modified: 2016-09-19 11:16:42
---
<h3>Envoyer des messages</h3> <p>Pour envoyer des <a href="/concepts/messaging-message-scope-messagecallback-saga/">messages et invoquer des méthodes (MessageCallbacks)</a> d’autre packages (ou consommateurs, pages Web, app mobile, etc… ), vous pouvez utiliser la méthode “sendMessage” :</p><pre class="lang:cpp decode:true">constellation.sendMessage(scope, args, messageKey, data);</pre>
<p>C’est une fonction “variadic“ qui peut prendre plusieurs argument qui seront utilisés pour formater le contenu (data) du message :</p><pre class="lang:cpp decode:true">constellation.sendMessage(scope, args, messageKey, data, ...);</pre>
<p>Par exemple pour envoyer un message “DemoMessage” avec en contenu un objet contenant deux propriétés (c’est à dire invoquer le MessageCallback “DemoMessage” en passant en paramètre un objet à deux propriétés) au package “MyCsharpPackage“ :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Package, "MyCsharpPackage", "DemoMessage", "{ A: '%s', B: %d  }",  myStringVal, myIntVal);</pre>Autre exemple : envoyer un message “SignalReceive” au groupe “IR” (dans lequel plusieurs package ou consommateur peuvent s’inscrire) avec en paramètre un valeur ”long” formatée en hexadécimal : <pre class="lang:cpp decode:true">unsigned long value = results.value; // the IR signal decoded
constellation.sendMessage(Group, "IR", "SignalReceive", "%#x", value);</pre>
<h4>Le scope</h4>
<p>Le scope permet de définir les destinataires du message (<a href="/concepts/messaging-message-scope-messagecallback-saga/">plus d’information ici</a>).</p>
<p>Dans la libraire Arduino/SP, l’argument Scope est une énumération définie de la façon suivante :</p><pre class="lang:lua decode:true">enum ScopeType : uint8_t {
  None = 0,
  Group = 1,
  Package = 2,
  Sentinel = 3,
  Other = 4,
  All = 5
};
</pre>
<p>L’argument “args” spécifie les arguments du scope, par exemple le ou les noms des packages, des sentinelles ou des groupes où envoyer le message. Pour cibler une instance d’un package vous spécifierez le nom de l’instance complet “SENTINEL/Package” (<a href="/concepts/messaging-message-scope-messagecallback-saga/">plus d’information ici</a>).</p>
<p>Si vous avez plusieurs “args” vous devez les séparer par des virgules. Par exemple pour envoyer un message “HelloWorld” au groupe A et au groupe B :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Group, "A,B", "HelloWorld", "{}");</pre>
<h4>Le contenu (Data) du message</h4>
<p>Dans la <a href="/concepts/messaging-message-scope-messagecallback-saga/">philosophie des MessageCallbacks</a>, le contenu du message (Data) permet de stocker les arguments d’une fonction (MessageCallback) à invoquer.</p>
<p>Si vous invoquez un MC sans paramètre, vous devez spécifier un objet vide “{}” :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "HelloWorld", "{}");</pre>
<p>Si vous avez plusieurs paramètres à passer, vous formaterez vos arguments dans un tableau :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "[ 'Sebastien', 'Warin' ]");</pre>
<p>Vous pouvez également envoyer en tant qu’argument un objet complexe, par exemple :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "{ 'FirstName':'Sebastien', 'LastName':'Warin' }");</pre>
<p>Ou bien même un mixte, par exemple un MC prenant en paramètre un objet complexe et un entier :</p><pre class="lang:cpp decode:true">constellation.sendMessage(Package, "DemoPackage", "SayHello", "[ { 'FirstName':'Sebastien', 'LastName':'Warin' }, 42 ]");</pre>
<p>Ici le contenu du message est formaté en JSON via une chaine de caractère mais vous pouvez également passer un JsonObject à cette méthode :</p><pre class="lang:cpp decode:true">const int BUFFER_SIZE = JSON_OBJECT_SIZE(2);
StaticJsonBuffer&lt;BUFFER_SIZE&gt; jsonBuffer;
JsonObject&amp; data = jsonBuffer.createObject();
myStateObject["FirstName"] = "Sebastien";
myStateObject["LastName"] = "Warin";
constellation.sendMessage(Package, "DemoPackage", "SayHello", data);</pre>
<p>Notez aussi que la Console Constellation liste les MessageCallbacks déclarés par l’ensemble des packages de votre Constellation et vous génère le code pour chaque API.</p>
<p>Par exemple, si vous souhaitez invoquer le MessageCallback “PushNote” du package “PushBullet” qui vous permettra d’envoyer une notification sur un smartphone vous pouvez cliquez sur l’icone de génération de code :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-66.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-55.png" width="350" height="147"></a></p>
<p align="left">Sélectionnez ensuite le langage “Arduino” pour avoir le modèle correspondant :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-67.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-56.png" width="350" height="220"></a></p>
<p>Ainsi pour envoyer une notification sur mon smartphone depuis un Arduino/ESP :</p><pre class="decode:true lang:cpp">constellation.sendMessage(Package, "PushBullet", "PushNote", "[ 'Demo from Arduino', 'Ceci est une demo', 'Device', '' ]");</pre>
<p>Bien entendu, n’importe quel package ou consommateur peut recevoir des messages. Vous pouvez donc invoquer des méthodes C#, Python, JS, etc.. depuis un Arduino/ESP.</p>
<h3>Envoyer des messages avec réponse : les Sagas</h3>
<p>Les <a href="/concepts/messaging-message-scope-messagecallback-saga/">Sagas</a> permettant de lier des messages ensemble et donc de créer des couples de “requêtes / réponses”.</p>
<p>Avec la libraire Arduino/ESP, il est possible d’envoyer des messages dans une saga et d’enregistrer un callback pour traiter la réponse de la saga.</p>
<p>Par exemple, le package “NetworkTools” expose entre autre un MessageCallback “Ping” permettant de pinger une machine. Si le MC est invoqué dans une saga, le package répondra dans la saga un message contenant le temps de réponse du ping.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-19.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-19.png" width="350" height="106"></a></p>
<p>De ce fait, si nous souhaitons invoquer cette méthode depuis notre Arduino et traiter la réponse, nous pouvons écrire :</p><pre class="lang:cpp decode:true">const char* ip = "192.168.0.10";
constellation.sendMessageWithSaga([](JsonObject&amp; json) {
    constellation.writeInfo("Ping response in %s ms", json["Data"].asString()); 
  }, Package, "NetworkTools", "Ping", "[ '%s' ]", ip);</pre>
<p>La signature de la méthode “<em>sendMessageWithSaga</em>” est la même que la méthode “<em>sendMessage</em>” présentée ci-dessus à la seule différence qu’elle prend comme 1er argument un callback vers une fonction traitant la réponse de votre saga ici passée en tant qu’expression lambda.</p>
<p>Dans l’exemple ci-dessus, votre Arduino envoie donc un message “Ping” avec une adresse IP en paramètre au(x) package(s) “NetworkTools” en associant un n° de saga. Ce package executera le ping et vous retournera un message de réponse avec le résultat du ping. Cette réponse sera traitée par la fonction lambda qui dans l’exemple affichera dans les logs Constellation le résultat du ping.</p>
<p>Ainsi vos Arduino/ESP peuvent très facilement invoquer des méthodes (MessageCallback) avec ou sans retour des différents packages (virtuels ou non) ou consommateurs de votre Constellation; scripts Javascripts, Powershell, Bash, programmes .NET, Python, etc…</p>