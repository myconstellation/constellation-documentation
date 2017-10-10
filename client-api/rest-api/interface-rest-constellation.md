---
ID: 2296
post_title: 'L&rsquo;interface REST &ldquo;Constellation&rdquo;'
author: Sebastien Warin
post_date: 2016-08-16 09:51:20
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/rest-api/interface-rest-constellation/
published: true
post_modified: 2017-05-04 14:47:03
---
L’interface REST “Constellation” est une API HTTP permettant d’exposer les fonctionnalités de base pour les <a href="/concepts/sentinels-packages-virtuels/">packages “virtuels”</a>.

Avec cette interface vous pourrez via des requêtes HTTP :

<ul>
    <li>Publier, consommer ou purger des StateObjects</li>
    <li>Envoyer et recevoir des messages</li>
    <li>Récupérer les settings du package "virtuel"</li>
    <li>Produire des logs</li>
    <li>Déclarer le “Package Descriptor” (MessageCallbacks exposés ou les types utilisés par les MC ou SO)</li>
</ul>

De ce fait un script Bash ou Powershell, une page PHP, un Arduino, ESP ou NetDuino, bref n’importe quel système, objet ou langage pouvant effectuer des appels HTTP peut être considéré comme un package (dit virtuel) de votre Constellation et produire ou consommer des StateObjects, envoyer ou recevoir des messages, écrire des logs, etc…

<h3>Généralités</h3>

<h4>Construction de l’URL</h4>

L’URL est :  &lt;Root URI&gt;/rest/constellation/&lt;action&gt;?&lt;paramètres&gt;

Partons du principe que votre Constellation est exposée en HTTP sur le port 8088 (sans path). On retrouvera dans le <a href="/constellation-server/fichier-de-configuration/">fichier de configuration</a> la section suivante :

<pre class="lang:xml decode:true">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
&lt;/listenUris&gt;</pre>

La “Root URI “ est donc “<strong>http://&lt;ip ou dns&gt;:8088/</strong>”.

Par exemple si nous sommes en local, nous pourrions écrire :

<pre>http://localhost:8088/rest/constellation/xxxxx</pre>

<h4>Authentification</h4>

Comme pour toutes les requêtes vers Constellation vous devez impérativement spécifier dans <u>les headers HTTP</u> <strong>ou</strong> dans <u>les paramètres de l’URL</u> (querystring), les paramètres “SentinelName”, “PackageName” et “AccessKey” pour l’authentification.

Ici votre “<a href="/concepts/sentinels-packages-virtuels/">package virtuel</a>” doit être déclaré dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/">configuration</a> de votre Constellation comme un véritable package, c’est à dire avec une sentinelle (dite virtuelle) et un package qui peut contenir des settings, etc…

Par exemple :

<pre class="lang:xml decode:true">&lt;sentinels&gt;
  &lt;sentinel name="MyVirtualSentinel" credential="Standard"&gt;
    &lt;packages&gt;        
      &lt;package name="MyVirtualPackage" enable="true"&gt;
        &lt;settings&gt;
          &lt;setting key="IntSetting" value="42" /&gt;
          &lt;setting key="StringSetting" value="sample config value" /&gt;
        &lt;/settings&gt;
      &lt;/package&gt;
    &lt;/packages&gt;
  &lt;/sentinel&gt;
&lt;/sentinels&gt;</pre>

On a ici déclaré une sentinelle virtuelle “MyVirtualSentinel” qui contient le package virtuelle “MonPackageVirtuel”. Ce package <a href="/concepts/securite-accesskey-credential-authorization/">utilise implicitement l’AccessKey</a> du credential “Standard” déclaré au niveau de la sentinelle.

De ce fait, pour chaque appel on passera les paramètres suivants (dans l’URL ou dans les headers) :

<ul>
    <li>SentinelName = MyVirtualSentinel</li>
    <li>PackageName = MyVirtualPackage</li>
    <li>AccessKey = MyAccessKey (en partant du principe que l’AccessKey déclaré pour le credential “Standard” est “MyAccessKey”)</li>
</ul>

Pour la suite de cet article nous passerons tous les paramètres dans l’URL, ainsi chaque appel sera sous la forme :

<pre>http://localhost:8088/rest/constellation/&lt;action&gt;?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;&lt;paramètres&gt;</pre>

<h4>Check Access</h4>

Toutes les API REST de Constellation exposent une méthode en GET “CheckAccess” qui retourne un code HTTP 200 (OK). Cela permet de tester la connexion et l’authentification au serveur Constellation.

<ul>
    <li>Action : “CheckAccess” (GET)</li>
    <li>Paramètres : aucun</li>
</ul>

Exemple :

<pre>http://localhost:8088/rest/constellation/CheckAccess?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey</pre>

<h3>Publier des StateObjects</h3>

<ul>
    <li>Action : “PushStateObject” (GET ou POST)</li>
    <li>En GET, voici les paramètres de l’URL :
<ul>
    <li><u>name</u> : le nom du StateObject</li>
    <li><u>value</u> : la valeur du StateObject</li>
    <li><u>type</u> (optionnel) : le type du StateObject</li>
    <li><u>lifetime</u> (optionnel – par défaut 0) : la durée de vie (en seconde) du StateObject avant d’être marqué “expiré”. (0 = durée de vie infinie)<!--EndFragment--></li>
</ul>
</li>
</ul>

Pour publier un StateObject vous devez obligatoirement spécifier son nom (name) et sa valeur (value). La valeur peut être un type simple (un Int, String, bool, etc..) ou un objet complexe formaté en JSON. Optionnellement vous pouvez spécifier le type et la durée de vie de votre SO (en seconde).

Par exemple publions un StateObject nommé “Temperature” dont la valeur est le nombre “21” :

<pre>http://localhost:8088/rest/constellation/PushStateObject?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;name=Temperature&amp;value=21</pre>

Autre exemple avec un objet complexe contenant deux propriété (Temperature et Humidity) dont la durée de vie est de 60 secondes et le type nommé “TemperatureHumidity” :

<pre>http://localhost:8088/rest/constellation/PushStateObject?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;name=Temperature&amp;value={ Temperature: 21, Humidity: 72 }&amp;type=TemperatureHumidity&amp;lifetime=60</pre>

Si la valeur du StateObject est trop grande pour être passée dans l’URL ou si vous souhaitez ajouter des métadonnées (metadatas) à votre StateObject vous pouvez “poster” le StateObject dans le corps de la requête HTTP en utilisant le verbe “POST” (sans oublier de définir le Content-Type à “application/json”).

Pour publier par un POST le même StateObject en lui ajoutant deux métadonnées :

<pre>POST http://localhost:8088/rest/constellation/PushStateObject?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey
Host: localhost:8088
Content-Length: 189
Content-Type: application/json

{
  "Name" : "Temperature",
  "Value" : { Temperature: 21, Humidity: 72 },
  "Type" : "TemperatureHumidity",
  "Lifetime" : 60,
  "Metadatas": { "ChipId":"ABC123", "Room":"Garden" }
}</pre>

<h3>Purger ses StateObjects</h3>

Un package peut supprimer ses StateObjects qu’il produit avec la méthode “PurgeStateObjects”.

<ul>
    <li>Action : “PurgeStateObjects” (GET)</li>
    <li>Paramètres:
<ul>
    <li><u>name</u> (optionnel – par défaut “*”) : le nom du StateObject</li>
    <li><u>type</u> (optionnel – par défaut “*”) : le type du StateObject</li>
</ul>
</li>
</ul>

Vous pouvez spécifier le nom du StateObject à supprimer et/ou son type. Par défaut ces deux paramètres sont définis à “*”. C’est à dire que si vous ne précisez rien, tous les StateObjects du package seront purgés.

<ul><!--EndFragment--></ul>

Par exemple pour purger tous les StateObjects de notre package virtuel (instance MyVirtualSentinel/MyVirtualPackage):

<pre>http://localhost:8088/rest/constellation/PurgeStateObjects?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey</pre>

Pour purger tous les StateObjects de type “Temperature” de cette même instance :

<pre>http://localhost:8088/rest/constellation/PurgeStateObjects?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;type=Temperature</pre>

Ou pour supprimer le StateObject nommé “Demo” :

<pre>http://localhost:8088/rest/constellation/PurgeStateObjects?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;name=Demo</pre>

<h3>Ecrire des logs</h3>

Les packages peuvent écrire des logs qui seront remontés dans le hub Constellation.

<ul>
    <li>Action : “WriteLog” (GET)</li>
    <li>Paramètres:
<ul>
    <li><u>message</u> : texte du log</li>
    <li><u>level</u> (optionnel – par défaut “Info”) : le niveau du log (Info, Warn ou Error)</li>
</ul>
<!--EndFragment--></li>
</ul>

Exemple d’un message “Info” :

<pre>http://localhost:8088/rest/constellation/WriteLog?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;message=Hello World</pre>

Exemple pour produire un message d’erreur (Error) :

<pre>http://localhost:8088/rest/constellation/WriteLog?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey&amp;message=Il y  a une erreur ici&amp;level=Error</pre>

<h3>Récupérer ses settings</h3>

Les settings sont récupérés en invoquant la méthode “GetSettings”.

<ul>
    <li>Action : “GetSettings” (GET)</li>
    <li>Paramètres:  aucun</li>
</ul>

Par exemple pour récupérer les settings (dictionnaire de clé / valeur) définis pour notre package virtuel :

<pre>http://localhost:8088/rest/constellation/GetSettings?SentinelName=MyVirtualSentinel&amp;PackageName=MyVirtualPackage&amp;AccessKey=MyAccessKey</pre>

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
  "Scope" : { "Type" : "&lt;type&gt;", Args: [ "&lt;arg1&gt;", "&lt;args2&gt;", .... ], "SagaId":"Identifiant de la Saga" },
  "Key" : "&lt;Key&gt;",
  "Data" : "&lt;Data&gt;"
}</pre>

La propriété “SagaId” dans le JSON ci-dessus est optionnelle.

Par exemple pour envoyer un message au package Nest en invoquant la méthode (key) “SetTargetTemperature” avec en paramètre le nombre “21” :

<pre>http://localhost:8088/rest/constellation/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=Nest&amp;key=SetTargetTemperature&amp;data=21</pre>

Ce même MessageCallback “SetTargetTemperature” du package Nest peut être invoqué dans une saga afin de recevoir un accusé de réception :

<pre>http://localhost:8088/rest/constellation/SendMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;scope=Package&amp;args=Nest&amp;key=SetTargetTemperature&amp;data=21&amp;sagaId=123456789</pre>

Il faudra ensuite “récupérer” les messages reçus (voir dessous) pour lire la réponse à votre saga que nous avons ici identifié par la clé “123456789”.

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

<pre>http://localhost:8088/rest/constellation/SubscribeToMessage?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>

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
</li>
</ul>

Il s’agit d’une requête en “long-polling” c’est à dire que la requête sera “bloquée” sur le serveur tant qu’il n’y a pas de message reçu évitant ainsi de “flooder” en continue le serveur pour savoir si de nouveaux messages sont disponibles ou non. Par défaut la requête est bloquée 60 secondes maximum mais vous pouvez personnaliser cette valeur avec le paramètre “timeout”. Si il y a des messages disponibles, le serveur vous renvoi un tableau JSON avec les messages reçus. Si il n’y a pas de réponse dans le délai spécifié par le paramètre “timeout” (60 secondes par défaut), le tableau retourné sera vide.

A chaque réponse vous devez donc relancer une requête “GetMessages” pour “écouter” les prochains messages qui vont sont destinés.

Il est également possible de limiter le nombre de message dans la réponse avec le paramètre “limit” ce qui peut être utile sur de petits “devices” ne disposant pas de beaucoup de mémoire RAM pour désérialiser de gros tableaux JSON.

Exemple simple :

<pre>http://localhost:8088/rest/constellation/GetMessages?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx</pre>

Autre exemple en limitant le nombre de message à 2 par appel et en bloquant la requête pendant 10 secondes au maximum :

<pre>http://localhost:8088/rest/constellation/GetMessages?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx&amp;timeout=10000&amp;limit=2</pre>

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

<pre>http://localhost:8088/rest/constellation/SubscribeToMessageGroup?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=xxxxx&amp;group=A</pre>

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

<pre>http://localhost:8088/rest/constellation/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123</pre>

Ou seulement ceux produits par le package “HWMonitor” (quelque soit la sentinelle) :

<pre>http://localhost:8088/rest/constellation/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;package=HWMonitor</pre>

Ou encore tous les StateObjects nommés “/intelcpu/load/0” et produits le package “HWMonitor” (quelque soit la sentinelle) :

<pre>http://localhost:8088/rest/constellation/RequestStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;package=HWMonitor&amp;name=/intelcpu/load/0</pre>

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

<pre>http://localhost:8088/rest/constellation/SubscribeToStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;sentinel=MON-PC&amp;package=HWMonitor&amp;name=/intelcpu/load/0</pre>

<strong>ATTENTION</strong> : si vous souhaitez “ajouter” des SO à votre abonnement vous devez <u>impérativement</u> préciser votre ID d’abonnement récupéré lors du 1er appel autrement vous allez créer un nouvel abonnement.

Par exemple pour “ajouter” le SO correspondant à la consommation RAM :

<pre>http://localhost:8088/rest/constellation/SubscribeToStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;sentinel=MON-PC&amp;package=HWMonitor&amp;name=/ram/load&amp;subscriptionId=&lt;subId&gt;</pre>

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
</ul>

Comme pour les messages, vous pouvez limiter le nombre de SO (limit) et le timeout de la requête (timeout).

<pre>http://localhost:8088/rest/constellation/GetStateObjects?SentinelName=Consumer&amp;PackageName=Demo&amp;AccessKey=MaCleDeTest123&amp;subscriptionId=&lt;subId&gt;</pre>

Notez que si un StateObject pour lequel vous êtes abonné change plusieurs fois entre deux appels “GetStateObjects”, vous obtiendrez la dernière valeur et non l’historique de tous les changements.

<h3>Déclarer le “Package Descriptor”</h3>

Action : “DeclarePackageDescriptor” (POST)

Le “<a href="/concepts/messaging-message-scope-messagecallback-saga/#Auto-description_des_MessageCallbacks">Package Descriptor</a>” permet décrire les MessagesCallbacks d’un package ainsi que les types utilisés par ses MC ou StateObjects.

Ainsi en publiant le PackageDescriptor, la Constellation aura connaissance des MessageCallbacks qu’expose un package ainsi que le type des StateObject qu’il publie. C’est grâce au Package Descriiptor que fonctionne le “MessageCallback Explorer” de la Console Constellation.

Le Package Descriptor est un objet JSON contenant :

<ul>
    <li>Le nom du package</li>
    <li>La liste des MessageCallbacks du package</li>
    <li>La liste des types utilisés par les MessageCallbacks du package</li>
    <li>La liste des types utilisés par les StateObjects publiés par le package</li>
</ul>

La structure de base est donc :

<pre class="lang:javascript decode:true">{
  "PackageName": "MyVirtualPackage",
  "MessageCallbacks": [],
  "MessageCallbackTypes": [],
  "StateObjectTypes": [],
}
</pre>

Chaque “MessageCallback” est un objet contenant :

<ul>
    <li><u>MessageKey</u> : la clé du message (le nom/clé du MessageCallback)</li>
    <li><u>Description</u> (optionnel) : description du MessageCallback</li>
    <li><u>ResponseType</u> (optionnel) : le type du message de retour dans le cas où le MC répond au Saga</li>
    <li><u>Parameters</u> (optionnel) : la liste des paramètres du MC</li>
</ul>

Un paramètre est décrit par un objet contenant :

<ul>
    <li><u>Name</u> : le nom du paramètre</li>
    <li><u>TypeName</u> : le type du paramètre</li>
    <li><u>Description</u> (optionnel) : la description du paramètre</li>
    <li><u>Type</u> : le type de paramètre (doit être obligatoirement défini à 2 pour un paramètre d’un MC)</li>
    <li><u>IsOptional</u> (optionnel) : booléen (true ou false) qui indique si le paramètre est optionnel ou non</li>
    <li><u>DefaultValue</u> (optionnel) : la valeur par défaut du paramètre si optionnel</li>
</ul>

Le type d’un paramètre peut être un type simple décrit par les types .NET (System.Int16, System.Int32, System.Int64, System.Double, System.Decimal, System.Float, System.Boolean, System.String) ou un type personnalisé.

Les types personnalisés utilisés dans les paramètres des MC seront décrits dans la propriété “MessageCallbackTypes” du Package Descriptor et les types personnalisés utilisés pour les StateObjects seront décrits dans la propriété “StateObjectTypes” du Package Descriptor.

Dans les deux cas il s’agit de la même structure de donnée.

La description d’un type est un objet contenant :

<ul>
    <li><u>TypeName</u> : le nom du type (l’identifiant)</li>
    <li><u>TypeFullname</u> : le nom complet du type (utilisé pour identifier le nom complet d’un type .NET par exemple)</li>
    <li><u>Description</u> (optionnel) : la description du type</li>
    <li><u>IsGeneric</u> (optionnel) : booléen (true ou false) qui indique si le type est un type générique</li>
    <li><u>IsArray</u> (optionnel) : booléen (true ou false) qui indique si le type est un tableau</li>
    <li><u>GenericParameters</u> (optionnel) : la liste des types génériques si il s’agit d’un type générique ou d’un tableau</li>
    <li><u>IsEnum</u> (optionnel) : booléen (true ou false) qui indique si le type est une énumération</li>
    <li><u>EnumValues</u> (optionnel) : la liste des valeurs de l’énumérations si le type est une énumération</li>
    <li><u>Properties</u> (optionnel) : la liste des propriétés du type</li>
</ul>

Les propriétés d’un type tout comme les valeurs d’une énumération sont décrits de la même manière qu’un paramètre d’un MessageCallback déjà détaillé ci-dessus.

Seule la propriété “Type” change :

<ul>
    <li>Type = 0 : pour décrire une propriété</li>
    <li>Type = 1 : pour décrire une valeur d’une énumération</li>
    <li>Type = 2 : pour décrire une paramètre d’un MessageCallback comme expliqué précédemment</li>
</ul>

<h4>Exemple 1 – Package “NetworkTools”</h4>

Voici le Package Descriptor du package “NetworkTools” :

<pre class="lang:javascript decode:true">{
   "PackageName":"NetworkTools",
   "MessageCallbacks":[  
      {  
         "MessageKey":"Ping",
         "Description":"Pings the specified host and return the response time.",
         "ResponseType":"System.Int64",
         "Parameters":[  
            {  
               "Name":"host",
               "TypeName":"System.String",
               "Description":"The host.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"timeout",
               "TypeName":"System.Int32",
               "Description":"The timeout (5000ms by defaut).",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"CheckPort",
         "Description":"Check a port's status by entering an address and port number above and return the response time.",
         "ResponseType":"System.Int64",
         "Parameters":[  
            {  
               "Name":"host",
               "TypeName":"System.String",
               "Description":"The host.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"port",
               "TypeName":"System.Int32",
               "Description":"The port.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"timeout",
               "TypeName":"System.Int32",
               "Description":"The timeout (5000ms by defaut).",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"CheckHttp",
         "Description":"Checks the HTTP address and return the response time.",
         "ResponseType":"System.Int64",
         "Parameters":[  
            {  
               "Name":"address",
               "TypeName":"System.String",
               "Description":"The address.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"ScanPort",
         "Description":"Scans TCP port range to discover which TCP ports are open on your target host.",
         "ResponseType":"System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.Boolean, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]",
         "Parameters":[  
            {  
               "Name":"host",
               "TypeName":"System.String",
               "Description":"The host.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"startPort",
               "TypeName":"System.Int32",
               "Description":"The start port.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"endPort",
               "TypeName":"System.Int32",
               "Description":"The end port.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"timeout",
               "TypeName":"System.Int32",
               "Description":"The timeout (5000ms by defaut).",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"WakeUp",
         "Description":"Wakes up the specified host.",
         "ResponseType":"System.Boolean",
         "Parameters":[  
            {  
               "Name":"host",
               "TypeName":"System.String",
               "Description":"The host.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"macAddress",
               "TypeName":"System.String",
               "Description":"The mac address.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"DnsLookup",
         "Description":"Resolves a host name or IP address.",
         "ResponseType":"System.String[]",
         "Parameters":[  
            {  
               "Name":"host",
               "TypeName":"System.String",
               "Description":"The host.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      }
   ],
   "MessageCallbackTypes":[  
      {  
         "Description":null,
         "IsGeneric":true,
         "IsArray":false,
         "GenericParameters":[  
            "System.Int32",
            "System.Boolean"
         ],
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"Dictionary`2",
         "TypeFullname":"System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.Boolean, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]",
         "Properties":null
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":true,
         "GenericParameters":[  
            "System.String"
         ],
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"String[]",
         "TypeFullname":"System.String[]",
         "Properties":null
      }
   ],
   "StateObjectTypes":[  
      {  
         "Description":"Monitoring type",
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"MonitoringResult",
         "TypeFullname":"NetworkTools.MonitoringResult",
         "Properties":[  
            {  
               "Name":"ResponseTime",
               "TypeName":"System.Int64",
               "Description":"Gets or sets the response time in millisecond.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"State",
               "TypeName":"System.Boolean",
               "Description":"Gets or sets the state.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      }
   ]
}</pre>

<h4>Exemple 2 – Package “Paradox”</h4>

Autre exemple plus complet, le Package Descriptor du package “Paradox” :

<pre class="lang:javascript decode:true">{  
   "PackageName":"Paradox",
   "MessageCallbacks":[  
      {  
         "MessageKey":"AreaArm",
         "Description":"Arm the area.",
         "ResponseType":"System.Boolean",
         "Parameters":[  
            {  
               "Name":"request",
               "TypeName":"ParadoxOnConstellation.ArmingRequestData",
               "Description":"The request.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"AreaDisarm",
         "Description":"Disarm the area.",
         "ResponseType":"System.Boolean",
         "Parameters":[  
            {  
               "Name":"request",
               "TypeName":"ParadoxOnConstellation.ArmingRequestData",
               "Description":"The request.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"RefreshArea",
         "Description":"Refreshes the area.",
         "ResponseType":null,
         "Parameters":[  
            {  
               "Name":"area",
               "TypeName":"Paradox.Core.Area",
               "Description":"The area.",
               "Type":2,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "MessageKey":"RefreshAll",
         "Description":"Refreshes all.",
         "ResponseType":null,
         "Parameters":[  

         ]
      }
   ],
   "MessageCallbackTypes":[  
      {  
         "Description":"Arming or disarming request",
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"ArmingRequestData",
         "TypeFullname":"ParadoxOnConstellation.ArmingRequestData",
         "Properties":[  
            {  
               "Name":"Area",
               "TypeName":"Paradox.Core.Area",
               "Description":"The number of area.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Mode",
               "TypeName":"Paradox.Core.ArmingMode",
               "Description":"The mode number.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"PinCode",
               "TypeName":"System.String",
               "Description":"The pin code.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":true,
         "EnumValues":[  
            {  
               "Name":"All",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area1",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area2",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area3",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area4",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area5",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area6",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area7",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Area8",
               "TypeName":"Paradox.Core.Area",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            }
         ],
         "TypeName":"Area",
         "TypeFullname":"Paradox.Core.Area",
         "Properties":null
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":true,
         "EnumValues":[  
            {  
               "Name":"RegularArm",
               "TypeName":"Paradox.Core.ArmingMode",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"ForceArm",
               "TypeName":"Paradox.Core.ArmingMode",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"StayArm",
               "TypeName":"Paradox.Core.ArmingMode",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"InstantArm",
               "TypeName":"Paradox.Core.ArmingMode",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            }
         ],
         "TypeName":"ArmingMode",
         "TypeFullname":"Paradox.Core.ArmingMode",
         "Properties":null
      }
   ],
   "StateObjectTypes":[  
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"UserInfo",
         "TypeFullname":"ParadoxOnConstellation.UserInfo",
         "Properties":[  
            {  
               "Name":"Id",
               "TypeName":"System.Int32",
               "Description":"The identifier.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Type",
               "TypeName":"System.String",
               "Description":"The object type.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Name",
               "TypeName":"System.String",
               "Description":"The name.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"LastActivity",
               "TypeName":"System.DateTime",
               "Description":"The last activity.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"DateTime",
         "TypeFullname":"System.DateTime",
         "Properties":[  
            {  
               "Name":"Date",
               "TypeName":"System.DateTime",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Day",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"DayOfWeek",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"DayOfYear",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Hour",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Kind",
               "TypeName":"System.DateTimeKind",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Millisecond",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Minute",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Month",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Now",
               "TypeName":"System.DateTime",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"UtcNow",
               "TypeName":"System.DateTime",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Second",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Ticks",
               "TypeName":"System.Int64",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TimeOfDay",
               "TypeName":"System.TimeSpan",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Today",
               "TypeName":"System.DateTime",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Year",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":true,
         "EnumValues":[  
            {  
               "Name":"Sunday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Monday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Tuesday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Wednesday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Thursday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Friday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Saturday",
               "TypeName":"System.DayOfWeek",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            }
         ],
         "TypeName":"DayOfWeek",
         "TypeFullname":"System.DayOfWeek",
         "Properties":null
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":true,
         "EnumValues":[  
            {  
               "Name":"Unspecified",
               "TypeName":"System.DateTimeKind",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Utc",
               "TypeName":"System.DateTimeKind",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Local",
               "TypeName":"System.DateTimeKind",
               "Description":null,
               "Type":0,
               "IsOptional":false,
               "DefaultValue":null
            }
         ],
         "TypeName":"DateTimeKind",
         "TypeFullname":"System.DateTimeKind",
         "Properties":null
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"TimeSpan",
         "TypeFullname":"System.TimeSpan",
         "Properties":[  
            {  
               "Name":"Ticks",
               "TypeName":"System.Int64",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Days",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Hours",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Milliseconds",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Minutes",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Seconds",
               "TypeName":"System.Int32",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TotalDays",
               "TypeName":"System.Double",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TotalHours",
               "TypeName":"System.Double",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TotalMilliseconds",
               "TypeName":"System.Double",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TotalMinutes",
               "TypeName":"System.Double",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"TotalSeconds",
               "TypeName":"System.Double",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "Description":"Represent Zone information",
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"ZoneInfo",
         "TypeFullname":"ParadoxOnConstellation.ZoneInfo",
         "Properties":[  
            {  
               "Name":"IsOpen",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone is open.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"IsTamper",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone is tamper.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"InAlarm",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone is in alarm.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"InFireAlarm",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone is in fire alarm].",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"SupervisionLost",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone has supervision lost.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"LowBattery",
               "TypeName":"System.Boolean",
               "Description":"A value indicating whether this zone has low battery.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Id",
               "TypeName":"System.Int32",
               "Description":"The identifier.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Type",
               "TypeName":"System.String",
               "Description":"The object type.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Name",
               "TypeName":"System.String",
               "Description":"The name.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"LastActivity",
               "TypeName":"System.DateTime",
               "Description":"The last activity.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      },
      {  
         "Description":null,
         "IsGeneric":false,
         "IsArray":false,
         "GenericParameters":null,
         "IsEnum":false,
         "EnumValues":null,
         "TypeName":"AreaInfo",
         "TypeFullname":"ParadoxOnConstellation.AreaInfo",
         "Properties":[  
            {  
               "Name":"IsFullArmed",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"IsStayArmed",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"ZoneInMemory",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"HasTrouble",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"IsReady",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"IsInProgramming",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"InAlarm",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Strobe",
               "TypeName":"System.Boolean",
               "Description":null,
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Id",
               "TypeName":"System.Int32",
               "Description":"The identifier.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Type",
               "TypeName":"System.String",
               "Description":"The object type.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"Name",
               "TypeName":"System.String",
               "Description":"The name.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            },
            {  
               "Name":"LastActivity",
               "TypeName":"System.DateTime",
               "Description":"The last activity.",
               "Type":1,
               "IsOptional":false,
               "DefaultValue":null
            }
         ]
      }
   ]
}</pre>
