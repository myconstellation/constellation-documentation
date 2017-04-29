---
ID: 2327
post_title: 'Consommer Constellation avec l&rsquo;API Javascript'
author: Sebastien Warin
post_date: 2016-08-18 15:41:46
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/javascript-api/consommer-constellation-api-javascript/
published: true
post_modified: 2016-09-23 08:40:57
---
<h3>Connecter une page HTML avec l’API Javascript</h3> <p>Vous pouvez soit utiliser le gestionnaire de package Nuget depuis Visual Studio pour installer la dernière version du package “Constellation.Javascript” et ses dépendances :</p> <p align="center"><img alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/07/image.png"></p> <p>Ou bien utiliser (ou copier) le(s) CDN :</p><pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="//code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="//cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
</pre>
<p>Une fois les scripts ajoutés dans votre page vous devez initialiser le client Constellation en spécifiant l’URL de votre serveur Constellation, la clé d’accès et le “Friendly name” de votre page :</p><pre class="lang:javascript decode:true">var constellation = $.signalR.createConstellationConsumer("http://localhost:8088", "123456789", "Test API JS");</pre>
<p>Vous pouvez <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">créer une clé d’accès depuis la Console Constellation</a> ou en <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">éditant le fichier de configuration du serveur</a>.</p>
<p>Enfin ajoutons un handler sur le changement d’état de la connexion pour afficher le message “Je suis connecté” dans la console de votre navigateur par exemple lorsque la connexion à Constellation est établie :</p><pre class="lang:javascript decode:true">constellation.connection.stateChanged(function (change) {
    if (change.newState === $.signalR.connectionState.connected) {
        console.log("Je suis connecté");
    }
});
</pre>
<p>Il ne reste plus qu’à lancer la connexion en invoquant la méthode Start :</p><pre class="lang:javascript decode:true">constellation.connection.start();</pre>
<p>Et voilà, votre page est connectée à votre Constellation.</p>
<h3>Envoyer des messages et invoquer des MessageCallbacks</h3>
<h4>Envoyer des messages</h4>
<p>Pour envoyer des messages et donc invoquer des MessageCallbacks vous devez utiliser la méthode “<em>constellation.server.sendMessage</em>” en spécifiant :</p>
<ul>
<li>Le scope 
<li>La clé du message 
<li>Le contenu du message (= les paramètres du MessageCallback)</li></ul>
<p>Par exemple, avec le package Nest déployé dans votre Constellation, on retrouvera un MessageCallback&nbsp; “SetTargetTemperature” pour piloter la température de consigne d’un thermostat Nest. 
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-21.png"><img title="image" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-20.png" width="350" height="99"></a></p>
<p align="left">N’hésitez pas à utiliser le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a> pour découvrir l’ensemble des MessageCallbacks exposés par les packages de votre Constellation.</p>
<p>Pour invoquer le MessageCallback&nbsp; “SetTargetTemperature” depuis votre page Web :<pre class="lang:javascript decode:true">constellation.server.sendMessage({ Scope: 'Package', Args: ['Nest'] }, 'SetTargetTemperature', [ "thermostatId", 19 ]);</pre>
<p>Vous pouvez également passer plusieurs arguments à votre MC combinant type simple et type complexe. Si il y a plusieurs paramètres vous devez les stocker dans un tableau.</p>
<p>Par exemple le MC “ShowNotification” du package Xbmc permettant d’afficher une notification sur une interface Kodi/XBMC prend deux paramètres : le nom d l’hôte XBMC (un type string) et le détail de la notification à afficher (un type complexe) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-23.png"><img title="image" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-22.png" width="350" height="143"></a></p><pre class="lang:javascript decode:true">constellation.server.sendMessage({ Scope: 'Package', Args: ['Xbmc'] }, 'ShowNotification', [ xbmcName, { "Title":"Hello", "Message":"Hello from JS" } ]);</pre>
<p>Bien entendu vous pouvez invoquer des MessageCallbacks de n’importe quel package, réel (programme Linux/Windows) ou virtuel (Arduino/ESP) ou même sur d’autres consommateurs (pages Web par exemple).</p>
<p>Ainsi vos pages Web peuvent, en envoyant des messages, invoquer des méthodes sur n'importe quel système connecté dans votre Constellation.</p>
<p>Par exemple dans l’article sur les ESP/Arduinos, notre ESP8266 exposait un MC “Reboot” pour redémarrer la puce. Si l’on souhaite redémarrer notre Arduino/ESP depuis une page Web, il suffirai d’envoyer un message sans paramètre au bon scope. Par exemple :</p><pre class="lang:javascript decode:true">constellation.server.sendMessage({ Scope: 'Sentinel', Args: ['MyArduino'] }, 'Reboot', {});</pre>
<p>Ici le scope est “Sentinel” avec l’argument “MyArduino”, c’est à dire que le message “Reboot” sera envoyé à tous les packages de la sentinelle “MyArduino”.</p>
<p>Les <a href="/concepts/messaging-message-scope-messagecallback-saga/">scopes</a> peuvent être :</p>
<ul>
<li>Sentinel 
<li>Package 
<li>Group 
<li>Others 
<li>All</li></ul>
<p>La propriété “Args” de l’objet scope est un tableau contenant le nom des groupes, des sentinelles ou packages en fonction du type de scope sélectionné. Seuls les scopes “All” et “Others” n’ont pas besoin d’argument.</p>
<h4>Envoyer des messages avec réponse : les sagas</h4>
<p>Les <a href="https://developer.myconstellation.io/concepts/messaging-message-scope-messagecallback-saga/">Sagas</a> permettant de lier des messages ensemble et donc de créer des couples de “requêtes / réponses”. 
<p>Avec la libraire JavaScript, il est possible d’envoyer des messages dans une saga et d’enregistrer un callback pour traiter la réponse de la saga. 
<p>Par exemple, le package “NetworkTools” expose un MC “Ping” permettant de réaliser un ping réseau. Le résultat du ping est retourné dans un message de réponse (un Int64 représentant le temps du réponse du ping) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-25.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-24.png" width="350" height="155"></a></p>
<p align="left">De ce fait pour réaliser un ping depuis une page Web :</p><pre class="lang:javascript decode:true">constellation.server.sendMessageWithSaga({ Scope: 'Package', Args: ['NetworkTools'] }, 'Ping', "www.google.fr",
	function(response) {
		console.log("Ping %s ms", response.Data);
	});
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-26.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-25.png" width="350" height="160"></a></p>
<p align="left"><a href="/client-api/arduino-esp-api/recevoir-des-messages-et-exposer-des-methodes-messagecallback-sur-arduino-esp/#Exposer_des_MessageCallbacks_avec_reponse_les_sagas">Souvenez-vous de notre package virtuel de notre ESP8266 / Arduino</a>, on exposait un MessageCallback “Addition” capable de réaliser une addition locale (sur le CPU de l’Arduino ou ESP) et le résultat était retourné dans la saga. Pour réaliser cet aller-retour depuis notre page Arduino, on aurait pu écrire :</p><pre class="lang:javascript decode:true">constellation.server.sendMessageWithSaga({ Scope: 'Sentinel', Args: ['MyArduino'] }, 'Addition', [ 40, 2],
	function(response) {
		console.log("Le resultat de 40+2 par l'Arduino est %s", response.Data);
	});
</pre>
<p>Ainsi vos pages Web peuvent invoquer des méthodes et exploiter la réponse de tous les systèmes connectés sur Constellation exposant des MessageCallbacks.</p>
<h3>Recevoir des messages</h3>
<p>Une page Web connectée sur Constellation peut elle même recevoir des messages et donc exposer des MessageCallbacks que d’autres consommateurs (autres pages Web) ou packages (réels et virtuels) pourront invoquer.</p>
<p>Ceci dit un consommateur tel qu’une page Web n’a pas réellement d’existence. Autrement dit elle n’a pas d’identité et donc ne peut recevoir que des messages adressés à un groupe qu’elle devra joindre.</p>
<p>Pour joindre un groupe vous devez utiliser la méthode “subscribeMessages“ (et “unSubscribeMessages” pour en sortir). Vous pouvez joindre autant de groupe que vous souhaitez.</p>
<p>Par exemple, pour ajouter votre page au groupe “Demo” :</p><pre class="lang:javascript decode:true">constellation.server.subscribeMessages("Demo");</pre>
<p>Vous pourrez ensuite recevoir les messages envoyés dans ce groupe en ajoutant un handler sur le “onReceiveMessage” :</p><pre class="lang:javascript decode:true">constellation.client.onReceiveMessage(function (message) {
    console.log("Message recu !", message);
});
</pre>
<p>Dans ce handler vous recevrez TOUS les messages (quelque soit la clé du message).</p>
<p>Vous avez aussi un moyen plus simple consistant à définir un handler pour une clé de message spécifiquement via la méthode “registerMessageCallback”.</p>
<p>Par exemple :</p><pre class="lang:javascript decode:true">constellation.client.registerMessageCallback("ChangeTitle", function (msg) {
	document.title = msg.Data;
});
constellation.client.registerMessageCallback("HelloWorld", function (msg) {
	alert("Hello World");
});

</pre>
<p>Ici on enregistre deux MessageCallbacks.</p>
<p>Si votre page reçoit un MessageCallback “HelloWorld”, une alerte sera ouverte, si elle reçoit un MessageCallback “ChangeTitle”, le titre de votre page sera modifiée avec l’argument passé dans le MC.</p>
<p>Imaginez par exemple que vous ouvrez une page Web en plein écran sur différents devices dont vous n’avez pas de contrôle (par exemple des écrans Dashboard). Sur cette page vous affichez différentes données en temps réel, par exemple basées sur les StateObjects Constellation.</p>
<p>Vous modifier votre page où vous souhaiterez rafraichir sur tous les navigateurs qui ont ouvert votre page pour recharger la nouvelle version.</p>
<p>Pour ce faire, vous pouvez facilement exposer un MessageCallback “ReloadMe” qui recharge la page avec le code :</p><pre class="lang:javascript decode:true">constellation.client.registerMessageCallback("ReloadMe", function (msg) {
	location.reload();
});
</pre>
<p>Puis vous ajoutez votre page dans le groupe “ControlPage” :</p><pre class="lang:javascript decode:true">constellation.server.subscribeMessages("ControlPage");</pre>
<p>Dans ce fait, vous pouvez maintenant créer une page “cachée” avec un bouton pour invoquer le MessageCallback “ReloadMe” sur le groupe “ControlPage” afin de recharger toutes les navigateurs qui ont ouvert votre page par la ligne :</p><pre class="lang:javascript decode:true">constellation.server.sendMessage({ Scope:'Group', Args:['ControlPage'] }, "ReloadMe", {});</pre>
<p>Bien entendu, comme vos pages HTML peuvent enregistrer autant de MessageCallbacks qu’elles souhaitent et que ces MessageCallbacks sont invocables depuis n’importe quel système connecté à Constellation, d’autres pages Web, des objets à base d’Arduino, ESP ou autre, des programmes Python, .NET, etc… vous pouvez imaginer tout type d’usage. </p>
<h3>Consommer des StateObjects</h3>
<p>Pour consommer des StateObjects produits par des packages (virtuels ou réels) de votre Constellation, vous avez deux méthodes : l’évènement “onUpdateStateObject” ou les StateObjectLinks.</p>
<p>La première méthode consiste à enregistre un ou plusieurs handlers sur l’évènement “onUpdateStateObject” :</p><pre class="lang:javascript decode:true">constellation.client.onUpdateStateObject(function (stateobject) {
    console.log(stateobject);
});
</pre>
<p>Les handlers seront déclenchés à chaque mise à jour ou interrogation des StateObjects de votre page quelque soit le StateObject.</p>
<p>Pour interroger un ou plusieurs StateObjects à un instant T vous disposez de la méthode “requestStateObjects”.</p>
<p>Pour vous abonner aux mises à jour d’un ou plusieurs StateObjects, vous disposez de la méthode “subscribeStateObjects” (et “unSubscribeStateObjects” pour vous désabonner).</p>
<p>Enfin la méthode “requestSubscribeStateObjects” réalise un “requestStateObjects” et un “subscribeStateObjects” c’est à dire que la méthode demande la valeur actuelle du ou des StateObjects et s’abonne auprès du serveur Constellation pour être notifiée des mises à jour du et de ces StateObjects.</p>
<p>Toutes ces méthodes prennent en paramètre : le nom de la sentinelle, le nom du package, le nom du StateObject et le type du StateObject. Vous pouvez utiliser le wildcard “*” pour ne pas appliquer de filtre (<a href="/concepts/stateobjects/">plus d’information ici</a>).</p>
<p>Par exemple, pour récupérer tous les StateObjects du package “Demo” quelque soit la sentinelle et pour s’abonner aux mises à jour de ces StateObjects :</p><pre class="lang:javascript decode:true">constellation.server.requestSubscribeStateObjects("*", "Demo", "*", "*");</pre>
<p>L’autre méthode consiste à enregistrer un (ou plusieurs) StateObjectLink. Cela permet de lier une fonction à un abonnement.</p>
<p>Par exemple :</p><pre class="lang:javascript decode:true">constellation.client.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", "*", function (so) {
    // Code A
});

constellation.client.registerStateObjectLink("*", "Demo", "*", "*", function (so) {
    // Code B
});</pre>
<p>Ci-dessus le code A sera invoqué dès qu’un SO nommé "/intelcpu/0/load/0” et produit par le package “HWMonitor” est modifié alors que le code B sera déclenché dès qu’un StateObject du package “Demo” est modifié.</p>