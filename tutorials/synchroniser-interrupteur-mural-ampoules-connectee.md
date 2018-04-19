---
ID: 4853
post_title: 'Synchroniser un interrupteur mural avec des ampoules connectées et contrôler l&rsquo;ambiance lumineuse de votre pièce'
author: Sebastien Warin
post_date: 2017-05-16 01:12:13
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/synchroniser-interrupteur-mural-ampoules-connectee/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:06:39
---
Dans cet article nous allons découvrir comment contrôler l'ambiance lumineuse d'une pièce équipée d'ampoules connectées avec un simple interrupteur mural.

L'idée est à la fois de pouvoir allumer ou éteindre toutes les ampoules de la pièce à partir de l’interrupteur mais également de pouvoir sélectionner une ambiance lumineuse en fonction des circonstances, par exemple pour un salon : ambiance "réception", ambiance "soirée", ambiance "Ciné", ambiance "Feu de cheminée", etc...
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Hue1.gif"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Hue1" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Hue1_thumb.gif" alt="Hue1" width="150" height="266" border="0" /></a></p>

<h3>Prérequis</h3>
<ul>
 	<li>Un serveur Constellation</li>
 	<li>Des lampes ou ampoules connectées, ici des Philips Hue</li>
 	<li>Un interrupteur mural connecté, ici un interrupteur Legrand relié à un module Z-Wave Fibaro FGS-211</li>
 	<li>Le SDK Constellation pour Visual Studio</li>
</ul>
<h3>Etape 1 : connecter un interrupteur mural</h3>
La première étape consiste donc à intégrer un interrupteur dans Constellation. Avec la polyvalence de la plateforme Constellation, il y a mille et une manière d'y parvenir.

Pour ma part, j'ai conservé les interrupteurs existants de la gamme Mosaic Legrand. Il s'agit pour être exacte d'un double bouton poussoir.

Pour le rendre connecté, j'ai ajouté un micro-module de la marque Fibaro spécialement conçu pour être intégré dans la boite d'encastrement juste derrière l’interrupteur.

Ce module Fibaro est un FGS-211 connecté en Z-Wave et disposant de deux relais en sortie pilotant des charges jusqu'à 1500W et de deux entrées. Dans mon cas, il n'y a aucune charge connectée sur ce module, il me sert juste de"capteur".

Je rappelle aussi que les ampoules Philips Hue doivent être constamment alimentées en 220v de façon à être pilotable même après avoir fermé les lumières !

Le module Fibaro est simplement connecté sur le réseau électrique 220v pour son alimentation et aux deux boutons poussoir Legrand de la façon suivante :
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-87.png"><img class="alignnone" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Connexion du FGS-211" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-86.png" alt="Connexion du FGS-211" width="450" height="321" border="0" /></a></p>
Cela permettant d'avoir deux boutons connectés sur le même interrupteur : l'un servira pour allumer ou éteindre toutes les lumières du salon et l'autre permettra de changer la configuration lumineuse de la pièce.
<p style="text-align: center;"><img class="aligncenter" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-86.png" alt="image" width="240" height="232" border="0" /></p>
<p style="text-align: left;">Ce module Fibaro a été appairé sur mon contrôleur Z-Wave qui est une Vera Lite. Après avoir déployé le package <a href="/package-library/vera/">Vera </a>sur une des sentinelles de ma Constellation, on obtient différents StateObjects représentant l'état en temps réel de chaque device Z-Wave et des MessageCallbacks permettant de les piloter.</p>
<p style="text-align: left;">Les deux relais du FGS-221 sont considérés comme deux devices distincts de type "Switchs" (On ou Off). On a donc deux StateObjects dans Constellation pour représenter l'état de ces deux relais. Chacun de ces StateObjects contient la propriété "Status", un booléen indiquant si le relais est ouvert ou fermé.</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-89.png"><img class="aligncenter" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject du FGS-211" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-88.png" alt="StateObject du FGS-211" width="354" height="293" border="0" /></a></p>
<p style="text-align: left;">On peut permuter l'état du relais informatiquement parlant par la Vera (ou par Constellation via le MessageCallback "<em>SetSwitchState</em>" du package Vera) ou directement en appuyant sur les boutons poussoirs.</p>
Ainsi dès lors que l'utilisateur va appuyer sur l'un des boutons poussoirs, l'état du relais changera et donc le StateObject le représentant sera également mis à jour dans votre Constellation. On pourra donc s'abonner à ces deux StateObjects via l'API.NET, Python, Arduino ou autre pour réagir à ces changements d'état, par exemple pour allumer ou éteindre les ampoules Hue.
<h3>Etape 2 : connecter des lampes</h3>
Deuxième étape pour mener à bien notre projet : connecter des lampes dans Constellation. Il y a différentes solutions comme nous avons pu le découvrir dans <a href="/tutorials/synchroniser-lampe-bureau-avec-session-windows/#Etape_1_piloter_une_lampe_par_Constellation">cet autre tutoriel</a> : solutions DIY, prises connectées en Wifi, RF, en Z-Wave, lampes ou ampoules connectées en Wifi, en ZeeBee, etc.. etc..

Dans mon cas, j'ai équipé les huit appliques de mon salon avec des ampoules Philips Hue.
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/hue.jpg"><img class="aligncenter" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="hue" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/hue_thumb.jpg" alt="hue" width="400" height="210" border="0" /></a></p>
<p style="text-align: left;">Toutes les ampoules communiquent en ZeeBee à un pont Ethernet, nommé le bridge (à gauche de la photo ci-dessus). Grace au package <a href="/package-library/hue/">Hue</a>, vous disposez de différents MessageCallbacks pour contrôler chaque lampes Hue (état, intensité lumineuse, couleur, effet, etc..) et d'un StateObject par luminaire représentant son état en temps réel.</p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-90.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject d'une ampoule Hue" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-89.png" alt="StateObject d'une ampoule Hue" width="354" height="258" border="0" /></a></p>
<p style="text-align: center;" align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-91.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks du package Hue" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-90.png" alt="MessageCallbacks du package Hue" width="354" height="358" border="0" /></a></p>

<h3>Etape 3 : synchroniser l’interrupteur avec les lampes</h3>
Passons au chose sérieuse, nous avons d’un côté deux StateObjects nous indiquant l'état des deux relais qu'on contrôle par les boutons poussoirs et de l'autre des MessageCallbacks nous permettant de contrôler les ampoules Hue du salon, reste juste à faire le lien !

Pour cela nous allons <a href="https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-csharp/">créer un package .NET en C#</a> depuis Visual Studio.

Dans la classe principale (<em>Program</em>), nous allons ajouter deux <a href="https://developer.myconstellation.io/client-api/net-package-api/consommer-des-stateobjects/">StateObjectLinks</a>, c’est à dire des propriétés dans notre code liées aux StateObjects représentant les états des interrupteurs :
<pre class="lang:c# decode:true ">[StateObjectLink("Vera", "Interrupteur Hue 1")]
public StateObjectNotifier InterrupteurHue1 { get; set; }

[StateObjectLink("Vera", "Interrupteur Hue 2")]
public StateObjectNotifier InterrupteurHue2 { get; set; }</pre>
Nous allons commencer par le bouton de gauche, c'est à dire l'interrupteur n°1 qui se chargera d'allumer ou d’éteindre toutes les ampoules du salon.

Pour cela nous allons attacher un "handler" qui réagira à la mise à jour du StateObject "Interrupteur Hue 1". Le handler commencera par vérifier que l'état à bien changé, c'est à dire que la propriété "Status" du "OldState" versus le "NewState" est bien différente (le StateObject peut être mis à jour par exemple dans le cas d'un "poll" Z-Wave à intervalle régulier sans pour autant que son Status ait changé !).

Ensuite on invoque simplement le MessageCallback "<em>SetState</em>" du package Hue. Le 1er argument est le n° de la lampe, 0 indiquant "toutes les lampes" et le 2ème argument est l'état (On ou Off) de la lampe. Ici on passera l'état du relais (propriété du Status).
<pre class="lang:c# decode:true">this.InterrupteurHue1.ValueChanged += (s, e) =&gt;
{
    if ((bool)e.OldState.DynamicValue.Status != (bool)e.NewState.DynamicValue.Status)
    {
        PackageHost.CreateMessageProxy("Hue").SetState(0, (bool)e.NewState.DynamicValue.Status);
    }
};</pre>
Ainsi dès que vous appuyez sur le bouton poussoir de gauche, le relais change d'état donc le code ci-dessus est invoqué ce qui allumera ou éteindra toutes vos lampes Hue !

Et voilà comment en quelques lignes on peut synchroniser des choses ensembles ! Au final, on obtient un interrupteur connecté pilotant plusieurs ampoules connectées.

Une fois votre package testé et validé, vous pouvez <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">le publier</a> dans votre Constellation et le déployer sur une de vos sentinelles (<a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Publier_son_package_dans_Constellation">voir le guide</a>). Simple et efficace !
<h3>Etape 4 : contrôler les ambiances lumineuses</h3>
Ce que nous avons fait jusqu'à présent est relativement simple : on synchronise simplement l'état du relais Fibaro (piloté par l’interrupteur) à l'état (state On/Off) des lampes Hue.

Le MessageCallback "<em>SetState</em>" allume ou éteint des lampes Hue. Dans le cas d'un allumage, les lampes reprendront leurs dernières configurations en terme de couleur et d'intensité.
<p style="text-align: left;">Il existe d'autre MC sur ce package comme le <em>SetColor</em>, <em>SetBrightness</em>, ou le <em>Set</em> qui combine les 3 en un en prenant en paramètre à la fois l'état, la couleur et l'intensité.</p>
<p style="text-align: left;">Nous allons donc perfectionner notre package en utilisant <a href="/client-api/net-package-api/settings/">un setting au format JSON</a> pour décrire nos différentes ambiances lumineuses. L’interrupteur de gauche ne se contentera plus de faire un simple <em>SetState</em> sur chaque lampe mais appliquera la configuration dite par défaut que nous pourrons à tout moment mettre à jour dans les settings depuis la Console.</p>
<p style="text-align: left;">L'interrupteur n°2 quant à lui, celui de droite, servira pour appliquer la configuration suivante de façon cyclique (arrivé à la dernière, on revient au début).</p>
<p align="center"><img title="Hue2" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Hue2_thumb.gif" alt="Hue2" width="150" height="266" border="0" /></p>
<p style="text-align: left;" align="center">Commençons d'abord par définir nos configurations lumineuses dans un setting JSON.</p>

<h4>Créer des "configurations lumineuses"</h4>
Pour cela je vous propose de créer une page Web pour générer le setting de configuration sur base de l'état actuelle de vos lampes.

En clair, avec l'application officielle Hue, réglez vos lampes pour créer l'ambiance lumineuse souhaitée et en temps réel notre page Web générera l'objet JSON représentant votre configuration.

Nous allons simplement créer un simple page HTML en y ajoutant le module Constellation / AngularJS <a href="/client-api/javascript-api/consommer-constellation-angular-js/">comme vu ici</a>.

Lorsque nous sommes connecté, on enregistre un StateObjectLink sur tous les StateObjects du type "<em>Q42.HueApi.Light</em>" du package Hue. Pour chaque lampe Hue, on stocke la valeur du StateObject dans une variable de scope nommée "lights" :
<pre class="lang:js decode:true">constellation.registerStateObjectLink("*", "Hue", "*", "Q42.HueApi.Light", function (stateObject) { 
    $scope.$apply(function () {
        $scope.lights[stateObject.Name] = stateObject.Value;
    });
});</pre>
Ainsi l'état de chacune de nos lampes sera synchronisée en temps réel dans cette variable Javascript.

Maintenant dans le code HTML, générons une liste pour afficher le nom de chaque lampe, son ID, son état (on/off), sa couleur (Hue et Saturation) et son intensité (bri) avec un "ng-repeat" Angular :
<pre class="lang:xhtml decode:true">&lt;ul&gt;
    &lt;li ng-repeat="(name, value) in lights" title="{{value}}"&gt;{{name}} = #{{value.id}} On:{{value.state.on}} Hue:{{value.state.hue}} Sat:{{value.state.sat}} Bri:{{value.state.bri}}&lt;/li&gt;
&lt;/ul&gt;</pre>
C'est aussi simple que cela !

Maintenant ajoutons une méthode "<em>ExportCurrentConfig</em>" qu'on appellera dans notre StateObjectLink dès qu'un StateObject est mis à jour :
<pre class="lang:js decode:true">$scope.ExportCurrentConfig = function() {
    var config = { ConfigName: "MyConfig", Lights: []};
    for(var name in $scope.lights) {
        var value = $scope.lights[name];
        config.Lights.push({ id: value.id, state: value.state.on, hue: value.state.hue, sat:value.state.sat, bri:value.state.bri });
    }
    $scope.strConfigs = JSON.stringify([ config ]).replace(/"/g, "'");
};</pre>
On crée un objet avec une propriété "<em>ConfigName</em>" pour nommer notre configuration et un tableau "<em>Lights</em>" qu'on remplit avec l'état connu de chaque lampe. Pour finir on "<em>stringify</em>" notre objet en JSON qu'on affichera ensuite dans un "<em>textarea</em>" sur la page :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-88.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-87.png" alt="image" width="454" height="274" border="0" /></a></p>
<p style="text-align: left;" align="center">Il ne reste plus qu'à générer différentes configurations notre "extracteur" pour la suite de notre tutoriel.</p>
<p style="text-align: left;" align="center">Le code complet de la page :</p>

<pre class="lang:xhtml decode:true">&lt;!DOCTYPE html&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" ng-app="hue"&gt;
&lt;head&gt;
    &lt;title&gt;Hue Configuration&lt;/title&gt;
        
    &lt;script type="text/javascript" src="/Scripts/jquery-2.1.3.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="/Scripts/angular.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="/Scripts/jquery.signalR-2.2.0.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="/Scripts/Constellation-1.8.1.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript" src="/Scripts/ngConstellation-1.8.1.js"&gt;&lt;/script&gt; 
    
    &lt;script&gt;
        var hue = angular.module('hue',  ['ngConstellation'])
            .controller('HueController', ['$scope', 'constellationConsumer', function ($scope, constellation) {
                $scope.lights = {};
                $scope.strConfigs = "";
            
                constellation.initializeClient("http://xxxxxxx:8088/", "demo123", "Hue Configurator");
                
                $scope.ExportCurrentConfig = function() {
                    var config = { ConfigName: "MyConfig", Lights: []};
                    for(var name in $scope.lights) {
                        var value = $scope.lights[name];
                        config.Lights.push({ id: value.id, state: value.state.on, hue: value.state.hue, sat:value.state.sat, bri:value.state.bri });
                    }
                    $scope.strConfigs = JSON.stringify([ config ]).replace(/"/g, "'");
                };

                constellation.onConnectionStateChanged(function (change) {
                    if (change.newState === $.signalR.connectionState.connected) {
                        console.log("Connected");
                        constellation.registerStateObjectLink("*", "Hue", "*", "Q42.HueApi.Light", function (stateObject) {  
                            $scope.$apply(function () {
                                $scope.lights[stateObject.Name] = stateObject.Value;    
                                $scope.ExportCurrentConfig();
                            });
                        });                        
                    }
                    else if (change.newState === $.signalR.connectionState.disconnected) {    
                        constellation.connect();
                    }
                });
                
                constellation.connect();
            }]);
    &lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;ul&gt;
        &lt;li ng-repeat="(name, value) in lights" title="{{value}}"&gt;{{name}} = #{{value.id}} On:{{value.state.on}} Hue:{{value.state.hue}} Sat:{{value.state.sat}} Bri:{{value.state.bri}}&lt;/li&gt;
    &lt;/ul&gt;
    &lt;hr/&gt;
    &lt;p&gt;Current configuration :&lt;/p&gt;
    &lt;textarea rows="40" cols="50"&gt;{{strConfigs}}&lt;/textarea&gt;
    
&lt;/body&gt;
&lt;/html&gt;</pre>
<h4>Activer les configurations depuis interrupteur</h4>
Premièrement nous allons déclarer dans le <a href="/concepts/package-manifest/">manifeste de notre package</a> (<em>PackageInfo.xml</em>) le setting "<em>Hue.Configuration</em>" par la ligne :
<pre class="lang:xhtml decode:true ">&lt;Setting name="Hue.Configurations" type="JsonObject" isRequired="true" description="Configurations lumineuses" /&gt;</pre>
Le contenu de ce setting sera un tableau des différentes configurations générées par notre extracteur ci-dessus.

Pour ma part voici mes configurations à titre d'exemple :
<pre class="lang:js decode:true ">[
   {
      'ConfigName':'Blanc chaud',
      'DefaultConfig':true,
      'Lights':[
         {
            'id':'1',
            'state':true,
            'hue':6479,
            'sat':252,
            'bri':254
         },
         {
            'id':'2',
            'state':true,
            'hue':13907,
            'sat':187,
            'bri':179
         },
         {
            'id':'3',
            'state':true,
            'hue':13907,
            'sat':187,
            'bri':254
         },
         {
            'id':'4',
            'state':true,
            'hue':14101,
            'sat':180,
            'bri':141
         },
         {
            'id':'5',
            'state':true,
            'hue':14101,
            'sat':180,
            'bri':254
         },
         {
            'id':'6',
            'state':true,
            'hue':14101,
            'sat':180,
            'bri':161
         },
         {
            'id':'7',
            'state':true,
            'hue':12778,
            'sat':219,
            'bri':120
         },
         {
            'id':'8',
            'state':true,
            'hue':12778,
            'sat':219,
            'bri':254
         }
      ]
   },
   {
      'ConfigName':'Film du soir',
      'Lights':[
         {
            'id':'1',
            'state':true,
            'hue':7157,
            'sat':243,
            'bri':104
         },
         {
            'id':'2',
            'state':false,
            'hue':13907,
            'sat':187,
            'bri':179
         },
         {
            'id':'3',
            'state':true,
            'hue':13122,
            'sat':211,
            'bri':80
         },
         {
            'id':'4',
            'state':true,
            'hue':12778,
            'sat':219,
            'bri':142
         },
         {
            'id':'5',
            'state':true,
            'hue':13122,
            'sat':211,
            'bri':80
         },
         {
            'id':'6',
            'state':false,
            'hue':14101,
            'sat':180,
            'bri':161
         },
         {
            'id':'7',
            'state':false,
            'hue':13122,
            'sat':211,
            'bri':120
         },
         {
            'id':'8',
            'state':false,
            'hue':4712,
            'sat':241,
            'bri':254
         }
      ]
   },
   {
      'ConfigName':'Cheminée',
      'Lights':[
         {
            'id':'2',
            'state':false,
            'hue':7980,
            'sat':252,
            'bri':79
         },
         {
            'id':'8',
            'state':true,
            'hue':4712,
            'sat':241,
            'bri':254
         },
         {
            'id':'5',
            'state':true,
            'hue':13122,
            'sat':211,
            'bri':91
         },
         {
            'id':'4',
            'state':false,
            'hue':7862,
            'sat':252,
            'bri':53
         },
         {
            'id':'3',
            'state':true,
            'hue':13122,
            'sat':211,
            'bri':91
         },
         {
            'id':'6',
            'state':true,
            'hue':8774,
            'sat':252,
            'bri':56
         },
         {
            'id':'7',
            'state':true,
            'hue':13122,
            'sat':211,
            'bri':120
         },
         {
            'id':'1',
            'state':true,
            'hue':6196,
            'sat':247,
            'bri':104
         }
      ]
   },
   {
      'ConfigName':'Tout éteind',
      'OffConfig':true,
      'Lights':[
         {
            'id':'1',
            'state':false,
            'hue':7157,
            'sat':243,
            'bri':104
         },
         {
            'id':'2',
            'state':false,
            'hue':13907,
            'sat':187,
            'bri':179
         },
         {
            'id':'3',
            'state':false,
            'hue':3584,
            'sat':252,
            'bri':254
         },
         {
            'id':'4',
            'state':false,
            'hue':12778,
            'sat':219,
            'bri':142
         },
         {
            'id':'5',
            'state':false,
            'hue':13122,
            'sat':211,
            'bri':80
         },
         {
            'id':'6',
            'state':false,
            'hue':14101,
            'sat':180,
            'bri':161
         },
         {
            'id':'7',
            'state':false,
            'hue':12778,
            'sat':219,
            'bri':120
         },
         {
            'id':'8',
            'state':false,
            'hue':12778,
            'sat':219,
            'bri':254
         }
      ]
   }
]</pre>
Vous remarquerez que j'ai ajouté sur la première configuration la propriété "<em>DefaultConfig = true</em>" et "<em>OffConfig =true</em>" sur la dernière.  On se servira de ces deux propriétés pour savoir quelles sont les configurations à appliquer en cas de "On" et de "Off" sur notre interrupteur n°1.

Le setting est de type "JsonObject", on aura donc un bel éditeur dans la Console Constellation pour le déclarer ou le modifier. Pour vos développements vous pouvez aussi utiliser le <a href="/client-api/net-package-api/settings/">fichier App.config</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-92.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration du setting" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-91.png" alt="Configuration du setting" width="454" height="278" border="0" /></a></p>
Pour revenir à notre code C#, nous allons tout d'abord déclarer les variables privées suivantes :
<pre class="lang:c# decode:true">private int currentConfigurationId = 0, defaultConfigurationId = 0, offConfigurationId = 0;
private List&lt;dynamic&gt; hueConfiguration = new List&lt;dynamic&gt;();</pre>
Dans la méthode démarrage (<em>OnStart</em>) on charge notre setting JSON dans la variable "<em>hueConfiguration</em>" avec la méthode "<em>GetSettingAsJson</em>" puis on récupère dans ce tableau l'index de la configuration "Default" et "Off" par la méthode .NET "<em>FindIndex</em>" :
<pre class="lang:c# decode:true ">hueConfiguration = new List&lt;dynamic&gt;(PackageHost.GetSettingAsJsonObject("Hue.Configurations"));
defaultConfigurationId = this.hueConfiguration.FindIndex(c =&gt; c.DefaultConfig == true);
offConfigurationId = this.hueConfiguration.FindIndex(c =&gt; c.OffConfig == true);</pre>
Créons maintenant la méthode "<em>ApplyHueConfiguration</em>" qui se chargera d'appliquer la configuration sur nos lampes Hue.

En quelques mots elle itère sur chaque "<em>Light</em>" de la configuration spécifiée par son index puis invoque le MessageCallback "<em>Set</em>" du package Hue en passant l'ID de la lampe avec son état (state, hue/sat et brightness). La configuration courante est sauvegardée dans la variable "<em>currentConfigurationId</em>".

Il est aussi possible de spécifier optionnellement l'index de la configuration à exclure ce qui aura pour action d'appliquer automatique la configuration suivante.
<pre class="lang:c# decode:true">private int ApplyHueConfigration(List&lt;dynamic&gt; configurationList, int configIdx, int configToExclude = -1)
{
    // Si la config selectionnée est à exclure ...
    if (configToExclude &gt;= 0 &amp;&amp; configIdx == configToExclude)
    {
        // On passe à la configuration suivante :
        return this.ApplyHueConfigration(configurationList, (configIdx + 1) % configurationList.Count, configToExclude);
    }
    else
    {
        // On récupere la configuration à partir de son Index
        var config = configurationList[configIdx];
        PackageHost.WriteInfo("Applying Hue configuration '{0}'", config.ConfigName);
        // On applique la configuration en invoquant le MC "Set" à chaque lampe
        this.currentConfigurationId = configIdx;
        foreach (dynamic hue in config.Lights)
        {
            PackageHost.CreateMessageProxy("Hue").Set(hue.id, hue.state, hue.hue, hue.sat, hue.bri);
            Thread.Sleep(100);
        }
        // On retourne l'Index de la configuration appliquée
        return configIdx;
    }
}</pre>
Maintenant modifions notre handler sur le StateObjectLink "InterrupteurHue1" que nous avons créé précédemment.

Dès que cet interrupteur change d'état on applique la configuration "Default" ou "Off" en fonction de l'état du relais FGS-211 correspondant. On prendra garde de ne rien faire si l'interrupteur change à "On" alors que l'état des lampes n'est pas éteint et inversement, on ne fera rien si l'interrupteur change à "Off" alors que les lampes sont déjà éteintes.
<pre class="lang:c# decode:true">this.InterrupteurHue1.ValueChanged += (s, e) =&gt;
{
    // Si InterrupteurHue1 change d'état
    if ((bool)e.OldState.DynamicValue.Status != (bool)e.NewState.DynamicValue.Status)
    {
        if ((bool)e.NewState.DynamicValue.Status &amp;&amp; this.currentConfigurationId != offConfigurationId)
        {
            // Ne rien faire si InterrupteurHue1 = ON et que la configuration actuelle n'est pas "tout eteint" !
            return;
        }
        else if ((bool)e.NewState.DynamicValue.Status == false &amp;&amp; this.currentConfigurationId == offConfigurationId)
        {
            // Ne rien faire si InterrupteurHue1 = OFF et que la configuration actuelle est déjà "tout eteint" !
            return;
        }
        else
        {
            // Autrement on applique la configuration "default" si InterrupteurHue1 = ON ou "off" si InterrupteurHue1 = Off
            this.ApplyHueConfigration(hueConfiguration, (bool)e.NewState.DynamicValue.Status ? defaultConfigurationId : offConfigurationId);
        }
    }
};</pre>
Maintenant pour le deuxième interrupteur, dès qu'il changera d'état, deux cas :
<ul>
 	<li>Si l'interrupteur n°1 n'est pas allumé, on invoque le MC "<em>SetSwitchState</em>" du package Vera pour l'allumer. Ainsi son SO sera mis à jour et donc le code ci-dessus sera invoqué pour appliquer la configuration "Default" (synchronisation parfaite entre l'état du relais Fibaro et nos lampes)</li>
 	<li>Si l'interrupteur n°1 est déjà allumé, on appelle notre méthode "<em>ApplyHueConfiguration</em>" en spécifiant l'index de la prochaine configuration et en excluant la configuration "tout éteint" (Off)</li>
</ul>
<pre class="lang:default decode:true">// On permute les configurations avec InterrupteurHue2
this.InterrupteurHue2.ValueChanged += (s, e) =&gt;
{
    // Si InterrupteurHue2 change d'état
    if ((bool)e.OldState.DynamicValue.Status != (bool)e.NewState.DynamicValue.Status)
    {
        // Si InterrupteurHue1 = OFF, on l'allume pour garder InterrupteurHue1 syncrhonisé !
        if ((bool)this.InterrupteurHue1.DynamicValue.Status == false)
        {
            // Cela déclenchera le handler ci-dessus ce qui appliquera la configuration "default".
            PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.InterrupteurHue1.DynamicValue.Id, State = true });
        }
        else // Si InterrupteurHue1 est déjà ON
        {
            // ON applique la configuration suivante de facon cyclique en excluant la configuration " tout éteint"
            this.ApplyHueConfigration(hueConfiguration, (this.currentConfigurationId + 1) % hueConfiguration.Count, configToExclude: offConfigurationId);
        }
    }
};</pre>
Ainsi on a bien l'interrupteur n°1 qui contrôle le relais n° 1 du Fibaro, lui même parfaitement synchronisé avec nos lampes. Si le relais est éteint, on applique la configuration "OffConfig" de notre setting et si le relais est allumé, on applique la configuration "DefaultConfig".

Quand on appuie sur l'interrupteur n°2, on permute l'état du relais n° 2 du Fibaro ce qui applique donc, de manière cyclique, les différentes configurations de notre setting sur nos lampes Hue.

On peut modifier comme bon nous semble le setting depuis la Console Constellation pour changer la configuration par défaut, ajouter ou supprimer de nouvelles ambiances ou modifier celles existantes ! De même, si ajoutez des lampes dans votre pièce vous n'aurez qu'à les déclarer dans votre JSON;

De plus, avec n'importe quelle application ou contrôleur Z-Wave comme un Minimote par exemple, vous pouvez contrôler l'interrupteur n°1 qui se synchronisera parfaitement avec vos lampes Hue !
<h4>Pour aller plus loin</h4>
Ajoutons une dernière méthode nommée "<em>ApplyHueConfiguration</em>" qui accepte en paramètre le nom de la configuration à appliquer. On l'invoque avec le nom de la configuration (et non l'index) et elle se chargera d'appliquer cette configuration si elle existe bien dans votre setting.

Une fois n'est pas coutume, on enverra des messages au package Vera pour synchroniser l'état du relais n°1 si nécessaire.
<pre class="lang:default decode:true">[MessageCallback]
public int ApplyHueConfigration(string configName)
{
    // On récupere l'ID de la configuration à partir de son nom
    int configId = this.hueConfiguration.FindIndex(c =&gt; c.ConfigName == configName);
    if (configId &gt;= 0)
    {
        // On synchronise l'interrupteur Fibaro avec les lampes (sens Hue -&gt; Fibaro)
        if (configId != offConfigurationId &amp;&amp; (bool)this.InterrupteurHue1.DynamicValue.State == false)
        {
            PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.InterrupteurHue1.DynamicValue.Id, State = true });
        }
        else if (configId == offConfigurationId &amp;&amp; (bool)this.InterrupteurHue1.DynamicValue.State == true)
        {
            PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.InterrupteurHue1.DynamicValue.Id, State = false });
        }

        // On applique la configuration
        return this.ApplyHueConfigration(hueConfiguration, configId);
    }
    else
    {
        return configId;
    }
}</pre>
Vous remarquerez que nous avons ajouté l'attribut <em>[MessageCallback]</em> sur cette méthode ce qui veut dire que votre package exposera cette méthode, que l'on nomme <em>MessageCallback</em>, pour toute votre Constellation.

C'est à dire que n'importe quel autre package C#, Python, un script Powershell ou Bash, un Arduino ou ESP8266, une page Web, etc... Tous pourront envoyer un message à votre package pour appliquer des configurations lumineuses sur vos lampes Hue en fonction des configurations que nous avez configuré dans votre setting !