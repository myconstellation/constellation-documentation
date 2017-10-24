---
ID: 2329
post_title: 'Contr&ocirc;ler votre Constellation en Javascript'
author: Sebastien Warin
post_date: 2016-08-18 15:48:37
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/javascript-api/controler-constellation-api-javascript/
published: true
post_modified: 2017-10-24 10:34:46
---
Le <a href="/concepts/les-diffrents-types-de-hub-et-interfaces-rest-du-serveur-constellation/">hub de contrôle</a> expose des méthodes pour contrôler la Constellation comme arrêter/démarrer des packages, s’abonner et récupérer en temps réel les logs des packages de la Constellation, propager les changements de configuration dans la Constellation, suivre les états et la consommation de ressource des packages, etc…

Les deux librairies JavaScript Constellation intègrent un client permettant de se connecter sur ce hub afin de pouvoir piloter votre Constellation. La Console Constellation est d’ailleurs basée sur ce client.

Dans cet article découvrons les fonctionnalités de ce client avec l’API JavaScript et l’API AngularJS.
<h3>Connecter une page HTML au hub de contrôle</h3>
<h4>Etape 1 : Ajouter les librairies</h4>
<h5>En utilisant le CDN</h5>
Les librairies JavaScripts sont accessibles sur : <a href="http://cdn.myconstellation.io/js/">http://cdn.myconstellation.io/js/</a> (en HTTP ou HTTPS).

Pour utiliser la librairie JavaScript :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.2.min.js"&gt;&lt;/script&gt;
</pre>
Ou si vous souhaitez l'utiliser avec le framework AngularJS :
<pre class="lang:html5 decode:true">&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.2.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.7/angular.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/ngConstellation-1.8.2.min.js"&gt;&lt;/script&gt;
</pre>
<h5>Par Nuget en utilisant Visual Studio</h5>
Dans Visual Studio et ouvrez le gestionnaire de package NuGet.

En sélectionnant la source “Constellation”, installez le package Nuget “Constellation.Javascript” pour l’API JavaScript ou “Constellation.AngularJS” pour l’API AngularJS :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-12.png" alt="image" width="354" height="122" border="0" /></a></p>

<h4>Etape 2 : Connexion au hub de contrôle</h4>
Avec l’API JavaScript (JS) vous devez créer le client avec la méthode “createConstellationController” en spécifiant l’URL de votre Constellation, une clé d’accès et un friendly name :
<pre class="lang:javascript decode:true">var controller = $.signalR.createConstellationController("http://localhost:8088", "123456789", "TestAPI");
controller.connection.stateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    console.log("Connected to the ControlHub");
  }
});
controller.connection.start();
</pre>
Avec l’API AngularJS (NG), vous devez injecter le client “constellationController” dans votre contrôleur puis l’initialiser avec la méthode “initializeClient” en spécifiant l’URL de votre Constellation, une clé d’accès et un friendly name :
<pre class="lang:javascript decode:true">var demo = angular.module('demoApp', ['ngConstellation']);
demo.controller('MyController', ['$scope',  'constellationController', function ($scope, controller) {

  controller.initializeClient("http://localhost:8088", "123456789", "TestAPI");
    
  controller.onConnectionStateChanged(function (change) {
    if (change.newState === $.signalR.connectionState.connected) {
      console.log("Connected to the ControlHub");
    }
  });
	
  controller.connect();
}]);
</pre>
La clé d’accès (<a href="/concepts/securite-accesskey-credential-authorization/">AccessKey</a>) utilisée pour se connecter au hub de contrôle doit avoir l’attribut “enableControlHub” à “true”. Vous pouvez <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">gérer les credentials dans la Console Constellation</a> ou dans <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_credentials">le fichier de configuration</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-35.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-34.png" alt="image" width="350" height="287" border="0" /></a></p>

<h3>Superviser les sentinelles</h3>
Vous disposez de deux méthodes :
<ul>
 	<li>requestSentinelsList() : pour recevoir la liste des sentinelles de votre Constellation</li>
 	<li>requestSentinelUpdates() : pour recevoir toutes les mises à jour des sentinelles de votre Constellation (ajout ou suppression de sentinelles, connexion ou déconnexion des sentinelles, mise à jour des informations, etc…).</li>
</ul>
Ces deux méthodes sont de type “void”. Les réponses sont réceptionnées par les handlers respectivement nommés ”onUpdateSentinelsList” et “onUpdateSentinel”.
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.client.onUpdateSentinelsList(function (list) {
  console.log(list);
});

controller.client.onUpdateSentinel(function (sentinel) {
  console.log(sentinel);
});

controller.connection.onConnectionStateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    controller.server.requestSentinelsList();
    controller.server.requestSentinelUpdates();
  }
}</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.onUpdateSentinelsList (function (list) {
  console.log(list);
});

controller.onUpdateSentinel  (function (se) {
  console.log(se);
});

controller.onConnectionStateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    controller.requestSentinelsList();
    controller.requestSentinelUpdates ();
  }
});
</pre>
Par exemple utilisons l’API NG pour créer une page affichant l’ensemble des sentinelles avec leur statuts en temps dans une page :

Dans le code du contrôleur, ajoutons un handler sur la mise à jour sentinelle afin de stocker les objets “sentinelle” dans une variable  de scope :
<pre class="lang:javascript decode:true">$scope.sentinels = {};
controller.onUpdateSentinel  (function (sentinel) {
  $scope.$apply(function() {
    $scope.sentinels[sentinel.Description.SentinelName.replace("-", "")] = sentinel;
  });
});</pre>
Nous sommes obligé de faire un “<em>replace("-", "")</em>” car nous utilisons le nom de la sentinelle comme nom pour la propriété de notre variable de scope, or en JavaScript une propriété ou variable ne peut pas comporter de “-“.

Lors de la connexion, abonnons-nous aux mises à jour des sentinelles afin de maintenir notre page à jour :
<pre class="lang:javascript decode:true">controller.onConnectionStateChanged(function (change) {
  if (change.newState === $.signalR.connectionState.connected) {
    console.log("Connected to the ControlHub");
    controller.requestSentinelUpdates ();
  }
});
</pre>
Enfin dans le template HTML affichons nos différentes sentinelles dans une simple liste à puce :
<pre class="lang:html5 decode:true">&lt;ul&gt;
  &lt;li ng-repeat="s in sentinels"&gt;{{s.Description.SentinelName}} [{{s.IsConnected ? "CONNECTED" : "DISCONNECTED"}}] ({{s.Description.OSVersion}})&lt;/li&gt;
&lt;/ul&gt;
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-36.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-35.png" alt="image" width="350" height="160" border="0" /></a></p>
Veuillez noter que seule les sentinelles “<a href="/concepts/sentinels-packages-virtuels/">réelles</a>” qui se sont connectées au moins une fois peuvent être récupérer car cette technique. Pour les sentinelles qui ne se sont jamais connectées comme pour les <a href="/concepts/sentinels-packages-virtuels/">sentinelles virtuelles</a>, vous devez lire le fichier de configuration en utilisant l’API REST de Management (Management API).

Voici la structure de l’objet ”sentinelle” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-37.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-36.png" alt="image" width="350" height="128" border="0" /></a></p>

<h3>Superviser les packages</h3>
Une fois la liste de vos sentinelles connue, vous pouvez récupérer la liste des packages pour chaque sentinelle en invoquant la méthode “requestPackagesList “ et en ajoutant un handler sur le “onUpdatePackageList”.
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.client.onUpdatePackageList (function (list) {
  console.log(list);
});
controller.server.requestPackagesList("MySentinel");</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.onUpdatePackageList (function (list) {
  console.log(list);
});
controller.requestPackagesList("MySentinel");</pre>
Reprenons notre page ci-dessus et ajoutons la liste des packages pour chaque sentinelle de notre Constellation.

Dans le handler “onUpdateSentinel” défini dans la section précédente, demandons au hub de contrôle la liste des packages pour chaque sentinelle :
<pre class="lang:javascript decode:true">controller.onUpdateSentinel  (function (sentinel) {
  $scope.$apply(function() {
    $scope.sentinels[sentinel.Description.SentinelName.replace("-", "")] = sentinel;
  });
  controller.requestPackagesList(sentinel.Description.SentinelName);
});
</pre>
Et ajoutons dans notre scope le handler “onUpdatePackageList” de manière à ajouter la liste des packages dans une propriété “Packages” que nous ajouterons dans nos objets “sentinelles” :
<pre class="lang:javascript decode:true">controller.onUpdatePackageList (function (p) {
  $scope.$apply(function() {
    $scope.sentinels[p.SentinelName.replace("-", "")]["Packages"] = p.List;
  });
});
</pre>
Nous pouvons maintenant enrichir notre template pour afficher les packages de chacune de nos sentinelles :
<pre class="lang:html5 decode:true">&lt;ul&gt;
  &lt;li ng-repeat="s in sentinels"&gt;{{s.Description.SentinelName}} [{{s.IsConnected ? "CONNECTED" : "DISCONNECTED"}}] ({{s.Description.OSVersion}})
    &lt;ul&gt;
      &lt;li ng-repeat="p in s.Packages"&gt;{{p.Package.Name}} version {{p.PackageVersion}} [{{p.State}}]&lt;/li&gt;
    &lt;/ul&gt;
  &lt;/li&gt;
&lt;/ul&gt;
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-38.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-37.png" alt="image" width="350" height="433" border="0" /></a></p>

<h3>Surveiller l’état et la consommation des packages</h3>
La liste de package récupérée ci-dessus n’est pas maintenue à jour. Pour recevoir les mises à jour de vos packages afin d’être notifié de chaque changement d’état vous pouvez ajouter le handler “onReportPackageState”.

De plus vous pouvez aussi vous abonner aux informations de consommation des ressources de vos packages avec le handler “onReportPackageUsage”.
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.client.onReportPackageUsage  (function (usage) {
  console.log(usage);
});

controller.client.onReportPackageState  (function (usage) {
  console.log(usage);
});
</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.onReportPackageUsage  (function (usage) {
  console.log(usage);
});

controller.onReportPackageState  (function (usage) {
  console.log(usage);
});
</pre>
Par exemple ajoutons dans notre page la consommation de CPU et de RAM pour chaque package de notre Constellation :
<pre class="lang:javascript decode:true">controller.onReportPackageUsage(function (usage) {
  $scope.$apply(function() {
    var sentinelName = usage.SentinelName.replace("-", "");
    for(var idx in $scope.sentinels[sentinelName].Packages) {
      var p = $scope.sentinels[sentinelName].Packages[idx];
      if(p.Package.Name == usage.PackageName) {
        p["CPU"] = usage.CPU;
        p["RAM"] = usage.RAM;
      }
    }
  });
});
</pre>
Dans le code ci-dessus, on ajoute dans chaque objet “Package” la valeur de la consommation du CPU et de RAM.

Ainsi on peut enrichir le template HTML avec le code :
<pre class="lang:html5 decode:true">&lt;ul&gt;
  &lt;li ng-repeat="s in sentinels"&gt;{{s.Description.SentinelName}} [{{s.IsConnected ? "CONNECTED" : "DISCONNECTED"}}] ({{s.Description.OSVersion}})
    &lt;ul&gt;
      &lt;li ng-repeat="p in s.Packages"&gt;{{p.Package.Name}} version {{p.PackageVersion}} [{{p.State}}]
        &lt;ul&gt;
          &lt;li&gt;CPU: {{p.CPU | number : 0 }}% - RAM: {{p.RAM / 1024 / 1024 | number: 0}}Mo&lt;/li&gt;
        &lt;/ul&gt;
      &lt;/li&gt;
    &lt;/ul&gt;
  &lt;/li&gt;
&lt;/ul&gt;
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-39.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-38.png" alt="image" width="350" height="452" border="0" /></a></p>

<h3>Contrôler les packages</h3>
Pour contrôler les instances de package vous disposez des méthodes suivantes :
<ul>
 	<li><em>start(sentinelName, packageName)</em> : démarre un package</li>
 	<li><em>stop(sentinelName, packageName)</em> : arrête un package</li>
 	<li><em>restart(sentinelName, packageName)</em> : redémarre un package</li>
 	<li><em>reload(sentinelName, packageName)</em> : arrête un package, force la sentinelle à télécharger la dernière version du package sur le serveur et redémarre le package</li>
 	<li><em>updatePackageSettings(sentinelName, packageName)</em> : pousse les settings d’un package au package en cours de fonctionnement (à charge au package à prendre en compte ses nouveaux paramètres).</li>
</ul>
Avec l’API JS :
<pre class="lang:javascript decode:true">controller.server.start(sentinelName, packageName);
controller.server.stop(sentinelName, packageName);
controller.server.restart(sentinelName, packageName);
controller.server.reload(sentinelName, packageName);
controller.server.updatePackageSettings(sentinelName, packageName);</pre>
Avec l’API NG :
<pre class="lang:javascript decode:true">controller.start(sentinelName, packageName);
controller.stop(sentinelName, packageName);
controller.restart(sentinelName, packageName);
controller.reload(sentinelName, packageName);
controller.updatePackageSettings(sentinelName, packageName);</pre>
Par exemple ajoutons dans notre page la possibilité de piloter les packages.

Pour ce faire exposons notre client “controller” dans le scope Angular de façon à pouvoir l’utiliser dans le template HTML :
<pre class="lang:javascript decode:true">$scope.controller = controller;</pre>
Dans notre template ajoutons ensuite des boutons en spécifiant l'action du clic grâce à l'attribut “ng-click” :
<pre class="lang:html5 decode:true">&lt;button ng-click="controller.start(s.Description.SentinelName, p.Package.Name)"&gt;Start&lt;/button&gt;
&lt;button ng-click="controller.stop(s.Description.SentinelName, p.Package.Name)"&gt;Stop&lt;/button&gt;
&lt;button ng-click="controller.reload(s.Description.SentinelName, p.Package.Name)"&gt;Reload&lt;/button&gt;
&lt;button ng-click="controller.restart(s.Description.SentinelName, p.Package.Name)"&gt;Restart&lt;/button&gt;
</pre>
Pour aller plus loin on peut également afficher/cacher les boutons en fonction de l’état du package, par exemple ne pas afficher le bouton “Start” si le package est déjà démarré :
<pre class="lang:html5 decode:true">&lt;button ng-hide="p.State == 'Started'" ng-click="controller.start(s.Description.SentinelName, p.Package.Name)"&gt;Start&lt;/button&gt;
&lt;button ng-show="p.State == 'Started'" ng-click="controller.stop(s.Description.SentinelName, p.Package.Name)"&gt;Stop&lt;/button&gt;
&lt;button ng-click="controller.reload(s.Description.SentinelName, p.Package.Name)"&gt;Reload&lt;/button&gt;
&lt;button ng-show="p.State == 'Started'" ng-click="controller.restart(s.Description.SentinelName, p.Package.Name)"&gt;Restart&lt;/button&gt;
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-40.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-39.png" alt="image" width="350" height="261" border="0" /></a></p>

<h3>S’abonner aux logs de Constellation</h3>
Pour récupérer les logs produits par les packages (virtuels ou non) en temps réel dans votre page Web, vous devez ajouter un handler sur “onReceiveLogMessage”.
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.client.onReceiveLogMessage(function (log) {
  console.log(log);
});
</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.onReceiveLogMessage(function (log) {
  console.log(log);
});
</pre>
Par exemple ajoutons à notre page la capacité d’afficher les logs des packages de notre Constellation en temps réel.

Pour cela nous allons nous abonner aux logs et les stocker dans un tableau de notre scope Angular :
<pre class="lang:javascript decode:true">$scope.logs = [];
controller.onReceiveLogMessage(function (log) {
  $scope.$apply(function() {
    $scope.logs.push(log);
  });
});</pre>
Nous pouvons donc maintenant afficher proprement nos logs dans notre template HTML :
<pre class="lang:html5 decode:true">&lt;ul&gt;
  &lt;li ng-repeat="log in logs"&gt;{{log.Date | date: 'dd/MM/yyyy HH:mm:ss' }} on {{log.SentinelName}}/{{log.PackageName}} : [{{ log.Level.toUpperCase() }}] {{log.Message}}&lt;/li&gt;
&lt;/ul&gt;
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-41.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-40.png" alt="image" width="350" height="194" border="0" /></a></p>

<h3>Récupérer les PackageDescriptors</h3>
Le <a href="/concepts/messaging-message-scope-messagecallback-saga/#Auto-description_des_MessageCallbacks">PackageDescriptor</a> est un objet publié par les packages qui permet de décrire les MessagesCallbacks que les packages exposent ainsi que les types utilisés dans leurs MC et StateObjects.

Pour récupérer un PackageDescriptor vous devez invoquer la méthode ”requestPackageDescriptor” en spécifiant le nom du package et attacher un handler “onUpdatePackageDescriptor” pour récupérer le résultat.
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.client.onUpdatePackageDescriptor(function (descriptor) {
  console.log(descriptor);
});

controller.server.requestPackageDescriptor(packageName);</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.onUpdatePackageDescriptor(function (descriptor) {
  console.log(descriptor);
});

controller.requestPackageDescriptor(packageName);</pre>
Par exemple sur notre page ci-dessus, ajoutons un bouton permettant d’afficher dans une simple boite de dialogue la liste des MessageCallbacks d’un package :

Dans le template HTML :
<pre class="lang:html5 decode:true">&lt;button ng-click="controller.requestPackageDescriptor(p.Package.Name)"&gt;Show MessageCallbacks&lt;/button&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-42.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-41.png" alt="image" width="350" height="144" border="0" /></a></p>
<p align="left">Et dans le code de notre contrôleur, de manière grossière :</p>

<pre class="lang:javascript decode:true">controller.onUpdatePackageDescriptor(function (descriptor) {
  var result = "Package "+ descriptor.PackageName + " :";
  for(var idx in descriptor.Descriptor.MessageCallbacks) {
    result += "\n - " + descriptor.Descriptor.MessageCallbacks[idx].MessageKey;
  }
  alert(result);		  
});
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-43.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-42.png" alt="image" width="350" height="195" border="0" /></a></p>
<p align="left">Une <a href="/client-api/rest-api/interface-rest-constellation/#Declarer_le_Package_Descriptor">description plus détaillée des PackageDesciptors</a> se trouve <a href="/client-api/rest-api/interface-rest-constellation/#Declarer_le_Package_Descriptor">ici</a>.</p>

<h3>Effacer des StateObjects</h3>
Vous pouvez effacer des StateObjects en invoquant la méthode “purgeStateObjects” :
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.server.purgeStateObjects(sentinelName, packageName, name, type);</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.purgeStateObjects(sentinelName, packageName, name, type);</pre>
Vous devez indiquer les filtres de sélection des StateObjects à supprimer avec la possibilité d’utiliser le wildcard ”*” seulement pour les paramètres “name” et “type”. Autrement dit vous êtes obliger de spécifier le “sentinelName” et “packageName”.
<h3>Rafraichir et déployer la configuration</h3>
Vous pouvez recharger la configuration Constellation avec la méthode “reloadServerConfiguration” :
<ul>
 	<li>Avec l’API JS :</li>
</ul>
<pre class="lang:javascript decode:true">controller.server.reloadServerConfiguration(deployConfiguration);</pre>
<ul>
 	<li>Avec l’API NG :</li>
</ul>
<pre class="lang:javascript decode:true">controller.reloadServerConfiguration(deployConfiguration);</pre>
Cette méthode attend un booléen (deployConfiguration) pour indiquer si oui ou non la configuration doit être déployée après son rechargement. Dans le cas d’un déploiement, la nouvelle configuration est envoyée à toutes les sentinelles et packages de votre Constellation.