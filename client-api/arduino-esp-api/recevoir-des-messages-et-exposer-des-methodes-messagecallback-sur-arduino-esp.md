---
ID: 2459
post_title: 'MessageCallback : exposer des méthodes et recevoir des messages sur un Arduino/ESP'
author: Sebastien Warin
post_date: 2016-08-23 14:19:35
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/arduino-esp-api/recevoir-des-messages-et-exposer-des-methodes-messagecallback-sur-arduino-esp/
published: true
publish_post_category:
  - "18"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:49:08
---
Pour exposer des méthodes de votre Arduino/ESP dans la Constellation, vous pouvez enregistrer et par la même occasion déclarer auprès de Constellation des MessageCallbacks avec la méthode <em>registerMessageCallback</em>.
<h3>Enregistrer un MessageCallback</h3>
Par exemple enregistrons un MessageCallback “HelloWorld” via une expression lambda C++ à l’initialisation de votre Arduino/ESP (c’est à dire dans la méthode <em>setup()</em>):
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("HelloWorld",
  [](JsonObject&amp; json) {
    constellation.writeInfo("Hello Constellation !");
 });  
</pre>
Enfin dans la boucle principal (loop), invoquez la méthode “constellation.loop” pour traiter et dispatcher les messages reçu par votre package virtuel :
<pre class="lang:cpp decode:true">constellation.loop();</pre>
Dès lors que votre Arduino/ESP recevra un message dont la clé (<em>MessageKey</em>) est “HelloWorld”, votre méthode associée (représentée ci-dessus par une lambda) sera invoquée.
<h4>Décrire son MessageCallback</h4>
Le MessageCallback enregistré ci-dessus est dit “caché”, car il n’est pas référencé sur Constellation ! Vous pouvez bien sur envoyer un message “HelloWorld” à votre package pour invoquer votre fonction lambda mais cela suppose que vous connaissez par avance que votre package a enregistré le MessageCallback ci-dessus nommé “HelloWorld”.

Pour enregistrer<u> ET déclarer</u> un MessageCallback vous devez ajouter un “<em>MessageCallbackDescriptor</em>” dans les arguments de la méthode. Par exemple pour reprendre notre exemple “HelloWorld” :
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("HelloWorld",
  MessageCallbackDescriptor().setDescription("Hello World !"),
  [](JsonObject&amp; json) {
    constellation.writeInfo("Hello Constellation !");
 });</pre>
On a ici ajouté un “<em>MessageCallbackDescriptor</em>” en spécifiant la description de notre MC. Pour que la description du MC soit envoyée à Constellation, vous devez nécessairement publier le “Package Descriptor” à la fin de l’initialisation de votre Arduino/ESP par la ligne :
<pre class="lang:cpp decode:true">constellation.declarePackageDescriptor();</pre>
Maintenant, depuis la Console Constellation, ouvrez le “MessageCallback Explorer” et vous retrouvez un MC nommé “HelloWorld” pour votre package :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-7.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-7.png" alt="image" width="350" height="96" border="0" /></a></p>
Vous pouvez cliquer sur le bouton “Invoke” pour envoyer un message “HelloWorld” au package “MyVirtualPackage” et donc invoquer le code de la lambda.

On retrouvera donc dans la Console Log Constellation un “Hello Constellation” tel que programmé dans la lambda (via un <em>constellation.writeInfo</em>)  :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-8.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-8.png" alt="image" width="350" height="101" border="0" /></a></p>

<h4 align="left">Récupérer le contexte</h4>
<p align="left">Vous pouvez également récupérer le contexte de réception du message dans la fonction liée en ajoutant un argument "<em>MessageContext</em>" dans la signature du callback :</p>

<pre class="lang:cpp decode:true">(*)(JsonObject&amp;, MessageContext);</pre>
Le type “MessageContext” décrit de la façon suivante :
<pre class="lang:cpp decode:true">typedef struct {
	const char*	sagaId;
	bool isSaga;
	const char*	messageKey;
	MessageSender sender;
	ScopeType scope;
} MessageContext;</pre>
Avec :
<pre class="lang:cpp decode:true">enum SenderType : uint8_t {
   ConsumerHub = 0,
   ConsumerHttp = 1,
   ConstellationHub = 2,
   ConstellationHttp = 3
};

typedef struct {
	SenderType type;
	const char*	friendlyName;
	const char*	connectionId;
} MessageSender;</pre>
Ainsi vous pouvez obtenir des informations sur le contexte de réception du message à savoir :
<ul>
 	<li>Si le message est dans une saga, si oui obtenir l’identifiant de la saga</li>
 	<li>Connaitre le scope dans lequel a été envoyé le message</li>
 	<li>Connaitre l’émetteur du message (ID de connexion, friendly name, …)</li>
</ul>
<p align="left">Par exemple, pour afficher le nom de l’émetteur (son <em>FriendlyName</em>) nous pourrions écrire :</p>

<pre class="lang:cpp decode:true">constellation.registerMessageCallback("HelloWorld",
  MessageCallbackDescriptor().setDescription("Hello World !"),
  [](JsonObject&amp; json, MessageContext ctx) {
    constellation.writeInfo("Hello %s from Constellation !", ctx.sender.friendlyName);
 });</pre>
<p align="left">En invoquant le MC depuis le “MessageCallback Explorer” en étant connecté sous l’utilisateur “seb”, nous obtiendrons dans les logs :</p>

<pre>[MyVirtualSentinel/MyVirtualPackage] 17:28:41 : Hello Consumer/ControlCenter:seb from Constellation !</pre>
<h3>Décrire les paramètres de vos MessageCallbacks</h3>
Dans la <a href="/concepts/messaging-message-scope-messagecallback-saga/">philosophie des MessageCallbacks</a> Constellation, un MessageCallback sert à invoquer des méthodes où la clé du message (MessageKey) est comme le nom d’une méthode.

Pour passer des paramètres à ces méthodes, on utilisera le contenu du message (Data).
<h4>Paramètres de types simples</h4>
Imaginez vouloir exposer une méthode “SayHello” qui prend en paramètre deux chaines de paramètres (= des types simples), le nom et prénom pour les afficher dans les logs.

On déclarera le MessageCallback suivant :
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("SayHello",
  MessageCallbackDescriptor().setDescription("Say hello !").addParameter("FirstName", "System.String").addParameter("LastName", "System.String"),
  [](JsonObject&amp; json) {
    constellation.writeInfo("Hello %s %s", json["Data"][0].as&lt;char *&gt;(), json["Data"][1].as&lt;char *&gt;()); 
 });  
</pre>
On a ajouté deux paramètres de type “String” dans le <em>MessageCallbackDescriptor</em> via la méthode “addParameter”. Dans le code de notre MessageCallback, nous exploitons les valeurs des paramètres via la propriété “Data” de notre message “json”.

Le 1er paramètre (<em>json["Data"][0]</em>) étant ici le “FirstName” d’après la description et le 2ème (<em>json["Data"][1]</em>) le “LastName”.

<u>Attention</u> :
<ul>
 	<li>lorsque plusieurs paramètres sont passés comme dans l’exemple ci-dessus, la propriété “Data” est un tableau contenant la valeur de chaque argument (ici [0] est “FirstName” et [1] le “LastName”)</li>
 	<li>Si un seul paramètre est passé dans votre MC, la propriété “Data” est la valeur de l’argument directement et non un tableau.</li>
</ul>
Vous pouvez tester votre MC depuis le “MessageCallback Explorer” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-9.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-9.png" alt="image" width="350" height="105" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-10.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-10.png" alt="image" width="350" height="15" border="0" /></a></p>
<p align="left">Bien entendu vous pouvez déclarer un ou plusieurs paramètres de type différent.</p>
<p align="left">Les types de base sont :</p>

<ul>
 	<li>
<div align="left">System.String</div></li>
 	<li>
<div align="left">System.Char</div></li>
 	<li>
<div align="left">System.Boolean</div></li>
 	<li>
<div align="left">System.Int16</div></li>
 	<li>
<div align="left">System.Int32</div></li>
 	<li>
<div align="left">System.Int64</div></li>
 	<li>
<div align="left">System.Double</div></li>
 	<li>
<div align="left">System.Decimal</div></li>
 	<li>
<div align="left">System.Float</div></li>
</ul>
Pour simplifier l’enregistrement des paramètres de type de base, vous pouvez les déclarer en utilisant la forme générique.

Par exemple pour un paramètre de type “String” on écrira :
<pre class="lang:cpp decode:true">addParameter&lt;const char*&gt;("FirstName")</pre>
Autre exemple pour ajouter des paramètres de type “Boolean” et “Int32” :
<pre class="lang:cpp decode:true">addParameter&lt;bool&gt;("IsEnable")
addParameter&lt;int&gt;("MyNumber")</pre>
De ce fait notre MC “SayHello” pourrait s’écrire :
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("SayHello",
  MessageCallbackDescriptor().setDescription("Say hello !").addParameter&lt;const char*&gt;("FirstName").addParameter&lt;const char*&gt;("LastName"),
  [](JsonObject&amp; json) {
    constellation.writeInfo("Hello %s %s", json["Data"][0].as&lt;char *&gt;(), json["Data"][1].as&lt;char *&gt;()); 
 });</pre>
<h4>Paramètre de types complexes</h4>
Il est également possible de spécifier des objets complexes contenant des propriétés.

Reprenons l’exemple précédent “SayHello” sauf qu’au lieu de passer deux paramètres de type “simple”, passons qu’un seul paramètre : un objet contenant deux propriétés (LastName et FirstName).
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("SayHello",
  MessageCallbackDescriptor().setDescription("Say hello with complex object!").addParameter("User", "SampleUserData"),
  [](JsonObject&amp; json) {
    constellation.writeInfo("Hello %s %s", json["Data"]["FirstName"].as&lt;char *&gt;(), json["Data"]["LastName"].as&lt;char *&gt;());
});  
</pre>
Dans le MessageCallbackDescriptor de notre MessageCallback nous déclarons qu’un seul paramètre nommé “User” et de type “SampleUserData”.

Au niveau du code de notre MessageCallback, nous récupérons nos deux valeurs via le 1èr et unique paramètre (<em>json["Data"]</em>) en accédant à la propriété “FirstName” et “LastName”.

Bien entendu pour que cela fonctionne il faut déclarer le type “SampleUserData” via la méthode “addMessageCallbackType” :
<pre class="lang:cpp decode:true">constellation.addMessageCallbackType("SampleUserData", TypeDescriptor().setDescription("This is a smaple user data").addProperty&lt;const char*&gt;("FirstName").addProperty&lt;const char*&gt;("LastName");</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-11.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-11.png" alt="image" width="350" height="117" border="0" /></a></p>
<p align="left">Il n’y a donc plus maintenant qu’un seul paramètre, un objet complexe contenant deux propriétés de type “string”. Vous pouvez d’ailleurs cliquez sur le type “SampleUserData” pour afficher sa description :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-12.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-12.png" alt="image" width="240" height="93" border="0" /></a></p>
<p align="left">Noter que vous pouvez enregistrer et déclarer des MessagesCallbacks mixant paramètre simple et paramètre complexe :</p>

<pre class="lang:cpp decode:true">constellation.registerMessageCallback("MultipleParameters",
  MessageCallbackDescriptor().setDescription("Mixte de parametre simple &amp; complexe").addParameter("User", "SampleUserData").addParameter&lt;const char*&gt;("Password"),
  [](JsonObject&amp; json) {
	// json["Data"][0] is SampleUserData
	// json["Data"][1] is String
});  
</pre>
<h3>Exposer des MessageCallbacks avec réponse : les sagas</h3>
Pour finir un MessageCallback peut retourner un message de réponse, c’est ce que l’on appelle <a href="/concepts/messaging-message-scope-messagecallback-saga/#Les_Sagas">les sagas</a>. C’est un peu comme une méthode en programmation, elle peut être “procédure” (sans retour, un void) ou “fonction” (retourne un type simple ou complexe).

Votre Arduino/ESP peut également enregistrer des MC retournant une réponse. Pour cela deux choses sont nécessaire :
<ul>
 	<li>Appeler la méthode “sendReponse” dans votre callback pour envoyer la réponse</li>
 	<li>Appeler la méthode “setReturnType” sur le MessageCallbackDescriptor de votre MC pour déclarer le type de retour</li>
</ul>
Par exemple exposons un MessageCallback “Addition” pour réaliser des additions sur un Arduino/ESP. Ce MessageCallback prend en paramètre deux entiers et retourne le résultat de type entier (Int32) également :
<pre class="lang:cpp decode:true">constellation.registerMessageCallback("Addition",
  MessageCallbackDescriptor().setDescription("Do addition on this tiny device").addParameter&lt;int&gt;("a").addParameter&lt;int&gt;("b").setReturnType&lt;int&gt;(),
  [](JsonObject&amp; json, MessageContext ctx) {		
    int a = json["Data"][0].as&lt;int&gt;();      
    int b = json["Data"][1].as&lt;int&gt;();
    int result = a + b;
    constellation.writeInfo("Addition %d + %d = %d", a, b, result);      
    if(ctx.isSaga) {
      constellation.writeInfo("Doing additoon for %s (sagaId: %s)", ctx.sender.friendlyName, ctx.sagaId);   
	// Return the result :
      constellation.sendResponse&lt;int&gt;(ctx, result);
    }
    else {
      constellation.writeInfo("No saga, no response !");
    }
 });</pre>
Pour retourner une réponse vous devez disposer du contexte de réception du message (le “MessageContext”). Vous noterez qu’ici, nous testons que le message appartient bien à une saga afin de renvoyer la réponse car il ne sert à rien d’envoyer une réponse sans saga, il ne sera jamais reçu par l’émetteur.

En déclarant le type de retour (ici un Int32), le <em>MessageCallback Explorer</em> reconnait ce MC comme une saga. Vous pouvez donc envoyer le message dans une saga (c’est à dire avec un identifiant de Saga) ou non grâce à la case à cocher :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-13.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-13.png" alt="image" width="350" height="70" border="0" /></a></p>
<p align="left">Si le message est envoyé dans une saga, notre code Arduino ci-dessus retournera une réponse, ici à la Console Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-14.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-14.png" alt="image" width="350" height="185" border="0" /></a></p>
<p align="left">La Console Constellation vous présentera ici toutes informations quant à la réponse et au message initial.  On retrouve ici la réponse de notre addition :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-15.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-15.png" alt="image" width="350" height="239" border="0" /></a></p>
<p align="left">Dans la Console Log, on retrouvera bien les logs produits par notre Arduino :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image52.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image52_thumb.png" alt="image" width="450" height="19" border="0" /></a></p>
<p align="left">Vous pouvez également rejouer ce test mais cette fois ci en décochant la case “With Saga”, c’est à dire que nous invoquons le MC “Addition” sans n° de saga :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-17.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-17.png" alt="image" width="240" height="126" border="0" /></a></p>
<p align="left">Dans la Console Log, notre Arduino réalise bien l’addition mais ne retourne pas de message de réponse car il ne dispose pas de n° de saga pour répondre :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-18.png"><img title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-18.png" alt="image" width="350" height="20" border="0" /></a></p>

<h3>Recevoir tous les messages dans une méthode</h3>
Vous pouvez aussi “attraper” tous les messages que votre package virtuel reçoit en définissant un “callback”, c’est à dire une fonction prenant en paramètre le message sous forme d’un JsonObject et qui sera appelée à chaque message reçu.

Par exemple, écrivez la méthode suivante pour afficher le “MessageKey” de chaque message reçu (si nécessaire n’hésitez pas à lire ou relire <a href="/concepts/messaging-message-scope-messagecallback-saga/">les fondamentaux du Messaging Constellation</a>) :
<pre class="lang:cpp decode:true">void messageReceive(JsonObject&amp; json) {
  Serial.print("MSG RCV : ");
  Serial.println(json["Key"].as&lt;char *&gt;());
}</pre>
Au “setup”, attachez cette méthode à la réception des messages (<em>setMessageReceiveCallback</em>) et abonnez-vous à la réception des messages (<em>subscribeToMessage</em>) :
<pre class="lang:cpp decode:true">constellation.setMessageReceiveCallback(messageReceive); 
constellation.subscribeToMessage();</pre>
Avec les expression lambda en C++, vous auriez pu aussi écrire directement :
<pre class="lang:cpp decode:true">constellation.setMessageReceiveCallback([](JsonObject&amp; json) { 
  constellation.writeInfo("Message received ! Message key = %s", json["Key"].as&lt;char *&gt;());     
});
constellation.subscribeToMessage();
</pre>
Enfin dans la boucle principal (loop), invoquez la méthode “constellation.loop” pour traiter et dispatcher les messages reçu par votre package virtuel :
<pre class="lang:cpp decode:true">constellation.loop();</pre>
Par exemple, imaginez un ESP8266 avec 2 MessageCallbacks : “Restart” sans paramètre qui redémarre l’ESP et “SendCode” avec un objet JSON en paramètre pour envoyer un signal infrarouge.

Le code pourrait être :
<pre class="lang:cpp decode:true">constellation.setMessageReceiveCallback([](JsonObject&amp; json) { 
  Serial.print("Message receive : ");
  const char * key = json["Key"].as&lt;char *&gt;();  
  if (stricmp (key, "SendCode") == 0) {

	const char * encoder = json["Data"]["Encoding"].as&lt;char *&gt;();  
	unsigned long code = strtoul(json["Data"]["Code"].as&lt;char *&gt;(), NULL, 0);

	// TODO : send the IR signal "code"

	else {
	  constellation.writeInfo("Unable to send IR code : encoding '%s' not found !", encoder); 
	}    
	irrecv.enableIRIn();
  }
  else if (stricmp (key, "Restart") == 0) {
	ESP.restart();
  }
});  
constellation.subscribeToMessage();
</pre>
Avec cette façon de faire, votre “MessageReceiveCallback” est invoqué à chaque message reçu mais c’est à vous de “dispatcher” les messages en fonction de leur “MessageKey”.

De plus côté Constellation, il n’y a aucun “Message Callback” déclaré sur Constellation, c’est a vous de connaitre les “MessageKey” que votre package virtuel acceptent, ici “SendCode” et “Restart” !
<h4>Obtenir contexte de réception</h4>
Comme pour l'enregistrement d'un MessageCallback, vous pouvez ajouter un 2ème argument de type MessageContext pour récupérer le contexte de réception du message.

Par exemple nous pourrions écrire très simplement :
<pre class="lang:cpp decode:true">constellation.setMessageReceiveCallback([](JsonObject&amp; json, MessageContext ctx) { 
  constellation.writeInfo("Message receive from %s. Message key = %s", ctx.sender.friendlyName, json["Key"].as&lt;char *&gt;());  
});  
constellation.subscribeToMessage();</pre>
<h3>S'abonner à un groupe Constellation</h3>
Jusqu’a présent votre package virtuel recevra les messages :
<ul>
 	<li>Du Scope “All” (c’est à dire tout le monde connecté à Constellation)</li>
 	<li>Du Scope “Others” si votre package n’est pas l’émetteur du message</li>
 	<li>Du Scope de votre sentinelle</li>
 	<li>Du Scope de votre package</li>
</ul>
Si nécessaire n’hésitez pas à lire ou relire <a href="/concepts/messaging-message-scope-messagecallback-saga/">les fondamentaux du Messaging Constellation</a>.

Vous pouvez également vous abonner à des groupes pour recevoir les messages qui seraient envoyés dans ces groupes.

Pour cela il suffit d’invoquer la méthode “<em>subscribeToGroup</em>” en spécifiant le nom du groupe à joindre. Exemple :
<pre class="lang:cpp decode:true">constellation.subscribeToGroup("MonGroupe");</pre>
Noter cependant que l’appartenance d’un package, virtuel ou non, à un groupe <a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Editer_les_groupes_dun_package">peut être défini au niveau de la configuration de la Constellation éditable simplement via la Console Constellation</a> afin de pouvoir faire évoluer ces appartenances à la volée sans devoir reprogrammer vos Arduino/ESP.
<h3></h3>