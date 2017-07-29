---
ID: 2284
post_title: 'L&rsquo;interface REST &ldquo;Consumer&rdquo;'
author: Sebastien Warin
post_date: 2016-08-12 15:44:31
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/rest-api/interface-rest-consumer/
published: true
post_modified: 2017-07-29 08:43:19
---
L’interface REST “Consumer” permet à tout système, script, objet ou programme de “consommer” des éléments dans une Constellation sans devoir être déclaré comme un package.

Un consommateur peut :
<ul>
 	<li>Consommer des StateObjects (Request ou Subscribe)</li>
 	<li>Envoyer ou recevoir des messages</li>
</ul>
<h3>Généralités</h3>
<h4>Construction de l’URL</h4>
L’URL est :  &lt;Root URI&gt;/rest/consumer/&lt;action&gt;?&lt;paramètres&gt;

Partons du principe que votre Constellation est exposée en HTTP sur le port 8088 (sans path). On retrouvera dans le <a href="https://developer.myconstellation.io/constellation-server/fichier-de-configuration/">fichier de configuration</a> la section suivante :
<pre class="lang:xml decode:true">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
&lt;/listenUris&gt;</pre>
La “Root URI “ est donc “<strong>http://&lt;ip ou dns&gt;:8088/</strong>”.

Par exemple si nous sommes en local, nous pourrions écrire :
<pre>http://localhost:8088/rest/consumer/xxxxx</pre>
<h4>Authentification</h4>
Comme pour toutes les requêtes vers Constellation vous devez impérativement spécifier dans <u>les headers HTTP</u> <strong>ou</strong> dans <u>les paramètres de l’URL</u> (querystring), les paramètres “SentinelName”, “PackageName” et “AccessKey” pour l’authentification.

Dans le cas de l’API “Consumer”, la “SentinelName” est “Consumer” et le “PackageName” est le nom de votre choix que l’on considère comme un “FriendlyName”. Typiquement le “FriendlyName” est par exemple le nom de l’application/page qui se connecte.

L’AccessKey doit bien sur être déclarée et activée sur le serveur.

Ainsi chaque appel sera sous la forme :
<pre>http://localhost:8088/rest/consumer/&lt;action&gt;?SentinelName=Consumer&amp;PackageName=&lt;Friendly name&gt;&amp;AccessKey=&lt;access key&gt;&amp;&lt;paramètres&gt;</pre>
<h4>Check Access</h4>
Toutes les API REST de Constellation exposent une méthode en GET “CheckAccess” qui retourne un code HTTP 200 (OK). Cela permet de tester la connexion et l’authentification au serveur Constellation.
<ul>
 	<li>Action : “CheckAccess” (GET)</li>
 	<li>Paramètres : aucun</li>
</ul>
Exemple :
<pre>http://localhost:8088/rest/consumer/CheckAccess?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
<h3>Envoyer des messages</h3>
<ul>
 	<li>Action : “SendMessage” (GET ou POST)</li>
 	<li>En GET, voici les paramètres de l’URL :
<ul>
 	<li><u>scope</u> : le type de scope (All, Other, Group, Sentinel ou Package)</li>
 	<li><u>args</u> : les arguments du scope (par exemple le nom du groupe, de la sentinelle, du package ou de l’instance du package – plusieurs valeurs possibles séparées par des virgules)</li>
 	<li><u>key</u> : la clé du message (= la méthode à invoquer)</li>
 	<li><u>data</u> : le contenu du message (= les arguments de la méthode)</li>
 	<li><u>sagaId</u> (optionnel) : l’identification de la saga si le message est envoyé dans une saga</li>
</ul>
</li>
</ul>
Vous pouvez également invoquer cette action en POST. Le corps de votre requête contiendra l’objet JSON suivant :
<pre class="lang:javascript decode:true">{
  "Scope" : { "Scope" : "&lt;type&gt;", Args: [ "&lt;arg1&gt;", "&lt;args2&gt;", .... ], "SagaId":"Identifiant de la Saga" },
  "Key" : "&lt;Key&gt;",
  "Data" : "&lt;Data&gt;"
}</pre>
La propriété “SagaId” dans le JSON ci-dessus est optionnelle.

Par exemple pour envoyer un message au package Nest en invoquant la méthode (key) “SetTargetTemperature” avec en paramètre le nombre “21” :
<pre class="">http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=Nest&amp;key=SetTargetTemperature&amp;data=21</pre>
Par exemple pour invoquer ce MessageCallback depuis cURL :
<pre class="lang:default decode:true">curl "http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=Nest&amp;key=SetTargetTemperature&amp;data=21"</pre>
Ce même MessageCallback “SetTargetTemperature” du package Nest peut être invoqué dans une saga afin de recevoir un un accusé de réception :
<pre class="">http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=Nest&amp;key=SetTargetTemperature&amp;data=21&amp;sagaId=123456789</pre>
Il faudra ensuite “récupérer” les messages reçus (voir dessous) pour lire la réponse à votre saga que nous avons ici identifié par la clé “123456789”.

Autre exemple avec un MessageCallback avec plusieurs paramètres :
<pre class="lang:default decode:true">http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=MyPackage&amp;key=MethodeAvec3Params&amp;data=[ 'param 1', 123, true ]</pre>
Attention le contenu du paramètre "args" doit être encodé. De ce fait, avec cURL :
<pre class="lang:default decode:true">curl "http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=MyPackage&amp;key=MethodeAvec3Params&amp;data=%5B+%27param+1%27%2C+123%2C+true+%5D"</pre>
Pour invoquer ce même MC avec un "POST" depuis cURL :
<pre class="lang:default decode:true ">curl -H "Content-Type: application/json" -X POST -i -d '{ "Scope" : { "Scope" : "Package", Args: [ "MyPackage"] }, "Key" : "MethodeAvec3Params", "Data" : [ "param 1", 123, true ] }' "http://localhost:8088/rest/consumer/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123"</pre>
<h3>Recevoir des messages</h3>
<h4>Créer un abonnement de réception</h4>
Tout d’abord pour recevoir des messages il faut s’abonner aux messages.
<ul>
 	<li>Action : “SubscribeToMessage” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>subscriptionId</u> (optionnel) : identifiant de l’abonnement si déjà connu</li>
</ul>
</li>
</ul>
<pre>http://localhost:8088/rest/consumer/SubscribeToMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
Vous obtiendrez en réponse l’ID de votre abonnement, le “Subscription Id”.
<h4>Relever les messages</h4>
<ul>
 	<li>Action : “GetMessages” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>subscriptionId</u> : identifiant de l’abonnement</li>
 	<li><u>timeout</u> (optionnel – par défaut 60000) : temps maximal en milliseconde de la mise en attente de la requête (entre 1000ms et 120000ms)</li>
 	<li><u>limit</u> (optionnel – par défaut 0) : nombre maximum de message à retourner pour l’appel</li>
</ul>
<!--EndFragment--></li>
</ul>
Il s’agit d’une requête en “long-polling” c’est à dire que la requête sera “bloquée” sur le serveur tant qu’il n’y a pas de message reçu évitant ainsi de “flooder” en continue le serveur pour savoir si de nouveaux messages sont disponibles ou non. Par défaut la requête est bloquée 60 secondes maximum mais vous pouvez personnaliser cette valeur avec le paramètre “timeout”. Si il y a des messages disponibles, le serveur vous renvoi un tableau JSON avec les messages reçus. Si il n’y a pas de réponse dans le délai spécifié par le paramètre “timeout” (60 secondes par défaut), le tableau retourné sera vide.

A chaque réponse vous devez donc relancer une requête “GetMessages” pour “écouter” les prochains messages qui vont sont destinés.

Il est également possible de limiter le nombre de message dans la réponse avec le paramètre “limit” ce qui peut être utile sur de petits “devices” ne disposant pas de beaucoup de mémoire RAM pour désérialiser de gros tableaux JSON.

Exemple simple :
<pre>http://localhost:8088/rest/consumer/GetMessages?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx</pre>
Autre exemple en limitant le nombre de message à 2 par appel et en bloquant la requête pendant 10 secondes au maximum :
<pre>http://localhost:8088/rest/consumer/GetMessages?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx&amp;timeout=10000&amp;limit=2</pre>
<h4>S’abonner à un groupe</h4>
Vous pouvez vous abonner à des groupes pour recevoir les messages envoyés dans ces groupes par l’action “SubscribeToMessageGroup” en précisant le nom du groupe et votre ID d’abonnement.
<ul>
 	<li>Action : “SubscribeToMessageGroup” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>group</u> : le nom du groupe à joindre</li>
 	<li><u>subscriptionId</u> (optionnel) : identifiant de l’abonnement si déjà connu</li>
</ul>
</li>
</ul>
<pre>http://localhost:8088/rest/consumer/SubscribeToMessageGroup?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx&amp;group=A</pre>
Comme l’action “SubscribeToMessage”, cette action vous retourne l’ID d’abonnement à utiliser pour l’action “GetMessages”.
<h3>Consommation de StateObjects</h3>
<h4>Request</h4>
Cette méthode permet de récupérer la valeur actuelle d’un ou de plusieurs StateObjects.
<ul>
 	<li>Action : “RequestStateObjects” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>sentinel</u> (optionnel – par défaut “*”): nom de la sentinelle</li>
 	<li><u>package</u> (optionnel – par défaut “*”) : nom du package</li>
 	<li><u>name</u> (optionnel – par défaut “*”) : nom du StateObject</li>
 	<li><u>type</u> (optionnel – par défaut “*”) : type du StateObject</li>
</ul>
</li>
</ul>
Par exemple pour récupérer tous les StateObject de votre Constellation :
<pre>http://localhost:8088/rest/consumer/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>
Ou seulement ceux produits par le package “HWMonitor” (quelque soit la sentinelle) :
<pre>http://localhost:8088/rest/consumer/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;package=HWMonitor</pre>
Ou encore tous les StateObjects nommés “/intelcpu/load/0” et produits le package “HWMonitor” (quelque soit la sentinelle) :
<pre>http://localhost:8088/rest/consumer/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;package=HWMonitor&amp;name=/intelcpu/load/0</pre>
<h4>Subscribe</h4>
Vous pouvez également vous abonner aux mises à jour des StateObjects.

Le principe est le même qu’avec les messages : il faut récupérer un ID d’abonnement et invoquer une méthode en long-polling pour recevoir les mises à jour.
<h5>S’abonner à des StateObjects</h5>
<ul>
 	<li>Action : “SubscribeToStateObjects” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>subscriptionId</u> (optionnel) : identifiant de l’abonnement si déjà connu</li>
 	<li><u>sentinel</u> (optionnel – par défaut “*”): nom de la sentinelle</li>
 	<li><u>package</u> (optionnel – par défaut “*”) : nom du package</li>
 	<li><u>name</u> (optionnel – par défaut “*”) : nom du StateObject</li>
 	<li><u>type</u> (optionnel – par défaut “*”) : type du StateObject</li>
</ul>
</li>
</ul>
En retour vous obtiendrez l’ID d’abonnement (un GUID).

Par exemple pour s’abonner au SO “/intelcpu/load/0”, produit le package “HWMonitor” sur la sentinelle “MON-PC” :
<pre>http://localhost:8088/rest/consumer/SubscribeToStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;sentinel=MON-PC&amp;package=HWMonitor&amp;name=/intelcpu/load/0</pre>
<strong>ATTENTION</strong> : si vous souhaitez “ajouter” des SO à votre abonnement vous devez <u>impérativement</u> préciser votre ID d’abonnement récupéré lors du 1er appel autrement vous allez créer un nouvel abonnement.

Par exemple pour “ajouter” le SO correspondant à la consommation RAM :
<pre>http://localhost:8088/rest/consumer/SubscribeToStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;sentinel=MON-PC&amp;package=HWMonitor&amp;name=/ram/load&amp;subscriptionId=&lt;subId&gt;</pre>
<h5>Relever les StateObjects mis à jour</h5>
Pour récupérer les SO de votre abonnement qui ont changés entre deux appels vous devez invoquer l’action “GetStateObjects” en spécifiant l’ID de votre abonnement :
<ul>
 	<li>Action : “GetStateObjects” (GET)</li>
 	<li>Paramètres :
<ul>
 	<li><u>subscriptionId</u> : identifiant de l’abonnement</li>
 	<li><u>timeout</u> (optionnel – par défaut 60000) : temps maximal en milliseconde de la mise en attente de la requête (entre 1000ms et 120000ms)</li>
 	<li><u>limit</u> (optionnel – par défaut 0) : nombre maximum de message à retourner pour l’appel</li>
</ul>
</li>
 	<li><!--EndFragment--></li>
</ul>
Comme pour les message, vous pouvez limiter le nombre de SO (limit) et le timeout de la requête (timeout).
<pre>http://localhost:8088/rest/consumer/GetStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=&lt;subId&gt;</pre>
Notez que si un StateObject pour lequel vous êtes abonné change plusieurs fois entre deux appels “GetStateObjects”, vous obtiendrez la dernière valeur et non l’historique de tous les changements.