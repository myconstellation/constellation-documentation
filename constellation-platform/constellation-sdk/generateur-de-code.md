---
ID: 2153
post_title: 'Le générateur de code C#'
author: Sebastien Warin
post_date: 2016-08-09 13:53:45
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-sdk/generateur-de-code/
published: true
publish_post_category:
  - "22"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:09:36
---
Etant donné que chaque package peut (et devrait) déclarer la liste des MessageCallbacks détaillée (description, liste des paramètres, type de réponse, …) qu’il expose ainsi que la liste des types personnalisées qu’il utilise dans la signature de ses MessageCallbacks ou des StateObjects qu’il publie, il est donc possible de générer du code de manière automatique.

C’est grâce à cette description, que l’on nomme le “PackageDescriptor” que fonctionne le “<a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a>” de la Console Constellation par exemple.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-116.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-111.png" alt="image" width="350" height="200" border="0" /></a></p>

<p align="left">L’interface est capable de lister chaque MessageCallbacks  de chaque packages de votre Constellation avec un formulaire pour la saisie des paramètres (simples ou complexes) afin de tester simplement vos MC.</p>

<p align="left">En cliquant sur le bouton <a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-117.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-112.png" alt="image" width="25" height="22" border="0" /></a>, le “<a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallback Explorer</a>” de la Console Constellation vous propose des “code snippets” pour différents langages (C#, Python, Arduino, JS, etc.).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-118.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-113.png" alt="image" width="354" height="226" border="0" /></a></p>

<h3 align="left">Pourquoi générer du code ?</h3>

<p align="left">Comme vous le savez, en C# pour envoyer un message et donc invoquer un MC d’un autre package, vous devez créer un scope et invoquer le MC en utilisant un proxy dynamique.</p>

<p align="left">Par exemple pour invoquer le MC “AreaArm” du package Paradox, on pourrait écrire :</p>

<pre class="lang:csharp decode:true">PackageHost.CreateMessageProxy("Paradox").AreaArm(new { Area = Paradox.Core.Area, Mode = Paradox.Core.ArmingMode, PinCode = System.String });</pre>

<p align="left">Il est important de noter ici que la méthode “<em>CreateMessageProxy</em>” retourne un “proxy dynamique”, c’est à dire que la méthode invoquée, ici “AreaArm” sera la clé du message.</p>

<p align="left">Autrement dit, comme tout est dynamique, il y a aucune aide ou auto-complétion. On pourrait très bien écrire ceci :</p>

<pre class="lang:csharp decode:true">PackageHost.CreateMessageProxy("Paradox").CeciEstUnExemple();</pre>

Votre package enverra un message “CeciEstUnExemple” au(x) package(s) Paradox de votre Constellation. Vous devez donc être vigilent sur le nom des MC invoqués car une erreur de frappe passera inaperçu !

Pour plus d’information sur l’envoi de messages &amp; invocation de MessageCallbacks en C#, <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">veuillez lire ceci</a>.

Le “problème” est le même avec la consommation des StateObjects (<a href="/client-api/net-package-api/consommer-des-stateobjects/">lire ceci</a>). Par exemple pour injecter dans votre code, le StateObject “/intelcpu/0/load/0” produit par le package “HWMonitor” sur la sentinelle “MON-PC”, on peut écrire :

<pre class="lang:csharp decode:true">[StateObjectLink("MON-PC", "HWMonitor", "/intelcpu/0/load/0")]
private StateObjectNotifier CPU { get; set; }</pre>

Je peux donc ensuite exploiter la valeur de mon StateObject :

<pre class="lang:csharp decode:true">dynamic value = this.CPU.DynamicValue;</pre>

La propriété “Value” du StateObjectNotifier  me donne la valeur du StateObject et la propriété “Value” de ce StateObject me donne la valeur du StateObject. Vous pouvez utiliser la propriété “DynamicValue” directement sur le StateObjectNotifier  pour récupérer la valeur du SO sous forme d’un ”dynamic”.

Comme chaque StateObject est différent, je ne sais pas ne que je trouverai dedans (une valeur simple, un objet complexe, …). D’où l’intérêt d’utiliser le <a href="/constellation-platform/constellation-console/stateobjects-explorer/">StateObject Explorer</a> de la Console Constellation pour explorer les SO de votre Constellation.

Par exemple, le StateObject “/intelcpu/0/load/0” produit par le package “HWMonitor” est un objet complexe contenant 4 propriétés :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-119.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-114.png" alt="image" width="354" height="252" border="0" /></a></p>

<p align="left">Alors que le StateObject de type “CarbonDioxideMeasurement” produit par le package “NetAtmo” est un entier :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-120.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-115.png" alt="image" width="354" height="280" border="0" /></a></p>

C’est pour cela que dans votre code, la valeur d’un StateObject est “dynamique” : tout dépend du StateObject que vous consommez !

Que ce soit pour l’invocation de MessageCallback ou la consommation de StateObject, la forme “dynamique” permet de s’adapter à toutes les situations. En revanche vous perdez l’auto complétion ce qui peut vous ralentir mais aussi être une source d’erreur. D’où l’intérêt de générer du code dans votre package!

<h3>Générer du code depuis Visual Studio.</h3>

Le générateur de code inclut dans le SDK Constellation pour Visual Studio ne fonctionne que pour un projet C#.

Commencez tout d’abord par cliquer le bouton <a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-110.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-105.png" alt="image" width="30" height="24" border="0" /></a> “Generate Code” dans la barre de menu :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-109.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-104.png" alt="image" width="240" height="92" border="0" /></a></p>

Le code sera généré pour le projet marqué comme projet de démarrage dans le cas où votre solution contient plusieurs projets.

Autrement, en cliquant-droit sur le projet de votre choix, sélectionnez “Generate Code” dans le sous-menu “Constellation” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-111.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-106.png" alt="image" width="350" height="327" border="0" /></a></p>

Vous serez amené à sélectionner dans la liste déroulante la Constellation à cibler. Pour configurer des connexions vers vos Constellations, <a href="/constellation-platform/constellation-sdk/gerer-connexions-constellation/">lisez ceci</a>.

Une fois le serveur Constellation sélectionné, cliquez sur “Connect and Discover”. Vous obtiendrez la liste de toutes vos sentinelles et packages de votre Constellation :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-122.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-117.png" alt="image" width="350" height="387" border="0" /></a></p>

<p align="left">Sélectionnez tout simplement la liste des packages que vous souhaitez inclure dans votre code. Par exemple, ici nous allons générer du code pour les packages “DoorBell”, “LightSensor”, “IRRemote”,”Paradox”, “Pionner” et “Vera”.</p>

<p align="left">Après avoir cliqué sur le bouton “Generate”, un message de confirmation vous indiquera le bon déroulé de l’opération :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-113.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-108.png" alt="image" width="168" height="165" border="0" /></a></p>

<p align="left">Le SDK génère le code dans le fichier “<em>MyConstellation.generated.cs</em>” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-114.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-109.png" alt="image" width="291" height="203" border="0" /></a></p>

<p align="left">Attention, vous ne devez pas modifier ce code directement car ce fichier est écrasé à chaque fois que vous relancer une génération.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-115.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-110.png" alt="image" width="350" height="308" border="0" /></a></p>

<h3 align="left">Utiliser le code généré</h3>

<h4 align="left">Organisation du code généré</h4>

<p align="left">Le code généré dans le fichier “<em>MyConstellation.generated.cs</em>” s’organise dans plusieurs espaces de nom (namespaces) :</p>

<ul>
    <li>
<div align="left">Dans le namespace de votre assembly vous trouverez :</div>
<ul>
    <li>
<div align="left">La classe statique “<em>MyConstellation</em>” représentant votre Constellation</div></li>
    <li>
<div align="left">Les classes utilitaires <em>RealNameAttribute</em> et <em>RealNameExtension</em> indispensable au fonctionnement du code généré</div></li>
</ul>
</li>
    <li>
<div align="left">Des namespaces par package</div>
<ul>
    <li>
<div align="left"><em>VotreNamespace.NomDuPackage.StateObjects</em> : code généré pour les StateObjects (si des StateObjects sont déclarés pour le package)</div></li>
    <li>
<div align="left"><em>VotreNamespace.NomDuPackage.MessageCallbacks</em> : code généré pour les MessageCallbacks (si des MessageCallbacks sont déclarés pour le package)</div></li>
</ul>
</li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-123.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-118.png" alt="image" width="350" height="406" border="0" /></a></p>

<h4 align="left">Des énumérations pour les sentinelles, packages et instances de votre Constellation</h4>

<p align="left">La classe statique “<em>MyConstellation</em>” contient trois énumérations :</p>

<ul>
    <li>
<div align="left"><u>Sentinels</u> : contenant la liste des sentinelles de votre Constellation</div></li>
    <li>
<div align="left"><u>Packages</u> : contenant le liste des packages de votre Constellation</div></li>
    <li>
<div align="left"><u>PackageInstances</u> : contenant la liste des instances des packages de votre Constellation</div></li>
</ul>

<p align="left">Comme les noms de sentinelles ou packages peuvent contenir des caractères interdits en C# (comme par exemple les tirets), les valeurs des énumérations sont “nettoyées” et la valeur réelle se trouve dans l’attribut “RealName” que vous pouvez récupérer avec la méthode d’extension “<em>GetRealName()</em>”.</p>

<p align="left">Dans notre exemple l’énumération “Sentinels” générée est la suivante :</p>

<pre class="lang:csharp decode:true">/// &lt;summary&gt;
/// Specifies the sentinels in your Constellation
/// &lt;/summary&gt;
public enum Sentinels
{
    /// &lt;summary&gt;
    /// Sentinel 'CEREBRUM'
    /// &lt;/summary&gt;
    [RealName("CEREBRUM")]
    CEREBRUM,
    /// &lt;summary&gt;
    /// Sentinel 'ESP8266'
    /// &lt;/summary&gt;
    [RealName("ESP8266")]
    ESP8266,
    /// &lt;summary&gt;
    /// Sentinel 'ESP-DoorBell'
    /// &lt;/summary&gt;
    [RealName("ESP-DoorBell")]
    ESP_DoorBell,
    /// &lt;summary&gt;
    /// Sentinel 'ESP-LightSensorSalon'
    /// &lt;/summary&gt;
    [RealName("ESP-LightSensorSalon")]
    ESP_LightSensorSalon,
    /// &lt;summary&gt;
    /// Sentinel 'esp-senergy'
    /// &lt;/summary&gt;
    [RealName("esp-senergy")]
    esp_senergy,
    /// &lt;summary&gt;
    /// Sentinel 'SKYNET-SERVER'
    /// &lt;/summary&gt;
    [RealName("SKYNET-SERVER")]
    SKYNET_SERVER,
}</pre>

<p align="left">De ce fait on peut manipuler les sentinelles et récupérer le nom réel avec la méthode “GetRealName()” :</p>

<pre class="lang:csharp decode:true">MyConstellation.Sentinels sentinel = MyConstellation.Sentinels.ESP_DoorBell;
string realName = sentinel.GetRealName();
</pre>

<p align="left">De plus, dans cette classe vous trouverez des méthodes d’extension pour <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Creer_un_scope">créer un “MessageScope”</a> vers une de vos sentinelles, packages ou instances de package.</p>

<p align="left">Par exemple pour créer un scope vers les packages “Hue” :</p>

<pre class="lang:csharp decode:true">MessageScope scope = MyConstellation.Packages.Hue.CreateScope();</pre>

Ce qui est équivalent à :

<pre class="lang:csharp decode:true">MessageScope scope = MessageScope.Create("Hue");</pre>

Sauf qu’avec le code généré plus besoin de chercher le nom exact ni même de risquer de faire une erreur de frappe, car tout est énumération !

On peut également cibler une instance d’un package en particulier. Par exemple pour cibler précisément le package “Hue” déployé sur la sentinelle “SKYNET-SERVER” :

<pre class="lang:csharp decode:true">MessageScope scope = MyConstellation.PackageInstances.SKYNET_SERVER_Hue.CreateScope();</pre>

<h4 align="left">Code généré pour les StateObjects</h4>

<p align="left">Prenons un exemple simple :  dans le code généré ci-dessus j’ai sélectionné le package “Paradox”, un package permettant de connecter les système d’alarme Paradox dans Constellation.</p>

<p align="left">Ce package publie plusieurs StateObjects :</p>

<ul>
    <li>
<div align="left">Des StateObjects de type “AreaInfo” par secteur qui représente l’état d’un secteur (système armé ou non par exemple)</div></li>
    <li>
<div align="left">Des StateObjects de type “ZoneInfo” par zone qui représente l’état d’une zone (zone ouverte ou non par exemple)</div></li>
    <li>
<div align="left">Des StateObjects de type “UserInfo” par utilisateur qui représente l’état d’un utilisateur (nom de l’utilisateur, dernière activité, etc..)</div></li>
</ul>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-124.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-119.png" alt="image" width="350" height="206" border="0" /></a></p>

<p align="left">Prenons par exemple le StateObject “ZoneInfo1” de ce package :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-125.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-120.png" alt="image" width="350" height="268" border="0" /></a></p>

<p align="left">On y trouvera plusieurs informations sur l’état de cette zone.</p>

<p align="left">Le code généré pour les StateObjects de ce package sera donc rangé dans le namespace : <em>VotreNamespace.Paradox.StateObjects </em>et contiendra :</p>

<ul>
    <li>
<div align="left">Une énumération “<em>ParadoxStateObjectNames</em>” référençant le nom des StateObjects actuellement connus sur le serveur</div></li>
    <li>
<div align="left">Une classe “<em>ParadoxStateObjectLinkAttribute</em>” (spécialisation de la classe <em>StateObjectLinkAttribute</em>)</div></li>
    <li>
<div align="left">Des classes pour chaque types personnalisés du package, ici le générateur aura généré les classes “AreaInfo”, “ZoneInfo” et “UserInfo”</div></li>
    <li>
<div align="left">Une classe “<em>ParadoxExtensions</em>” contenant des méthodes d’extension pour convertir des StateObjects en type personnalisé</div></li>
</ul>

<p align="left">Voyons par exemple comment inclure notre StateObject de la zone “1” dans votre code C# avec le code généré.</p>

<p align="left">Tout d’abord, il faut inclure le namespace :</p>

<pre class="lang:csharp decode:true">using Paradox.StateObjects;</pre>

<p align="left">Ensuite ajoutons un “StateObjectLink” de type “ParadoxStateObjectLink” où nous préciserons le nom du StateObject avec l’énumération :</p>

<pre class="lang:csharp decode:true">[ParadoxStateObjectLink(ParadoxStateObjectNames.ZoneInfo1)]
public StateObjectNotifier Zone1 { get; set; }
</pre>

<p align="left">Sans le code généré nous aurions écrit :</p>

<pre class="lang:csharp decode:true">[StateObjectLink(Package="Paradox", Name="ZoneInfo1")]
public StateObjectNotifier Zone1 { get; set; }
</pre>

<p align="left">Je peux ensuite utiliser la méthode d’extension générée “<em>AsZoneInfo()</em>” pour convertir la valeur du StateObject en “<em>ZoneInfo</em>” (“<em>Zon</em>eInfo” étant un type personnalisé décrit par le package Paradox) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-126.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-121.png" alt="image" width="400" height="125" border="0" /></a></p>

<p align="left">Le générateur a “reproduit” ce type dans votre code généré avec les commentaires tel que spécifiés dans le PackageDescriptor du package Paradox.</p>

<p align="left">Sans le code généré, vous devez travailler avec un objet dynamique, donc sans auto-complétion :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-127.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-122.png" alt="image" width="230" height="32" border="0" /></a></p>

<p align="left">Bien entendu tous les Packages (virtuels ou non) peuvent déclarer des Packages Descriptor.</p>

<p align="left">Prenez l’exemple d’un capteur d’électricité basé sur un ESP8266. En utilisant la <a href="/client-api/arduino-esp-api/">librairie Constellation pour Arduino</a>, le code C++ de ce package virtuel commence par déclarer le type “<em>SEnergy.Electricity</em>” :</p>

<pre class="lang:cpp decode:true">constellation.addStateObjectType("SEnergy.Electricity", TypeDescriptor().setDescription("S-Energy Electricity data").addProperty("Counter", "System.Int64", "Number of revolution").addProperty("Timestamp", "System.Int64", "Internal timestamp of the last revolution").addProperty("RevolutionTime", "System.Double", "The time (in ms) of the last revolution").addProperty("WattPerHour", "System.Int32", "Energy consumed").addProperty("Cumul", "System.Int64", "Total of KWh consumed")); 
constellation.declarePackageDescriptor();</pre>

Puis à chaque fois que le capteur détecte une consommation électrique il publie un StateObject de la façon suivante :

<pre class="lang:cpp decode:true">StaticJsonBuffer&lt;JSON_OBJECT_SIZE(5)&gt; jsonBuffer;
JsonObject&amp; myStateObject = jsonBuffer.createObject();
myStateObject["Counter"] = counter;
myStateObject["Timestamp"] = ts;
myStateObject["RevolutionTime"] = timePerRevolution;
myStateObject["WattPerHour"] = wattPerHour;
myStateObject["Cumul"] = cumul;
constellation.pushStateObject("Electricity", myStateObject, "SEnergy.Electricity", 600);</pre>

<p align="left">On retrouve bien ce StateObject sur la Console Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-129.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-124.png" alt="image" width="350" height="249" border="0" /></a></p>

<p align="left">Après avoir sélectionné le package “<em>SElectricity</em>” dans le générateur de code, je peux très facilement exploiter ce StateObject avec auto-complétion, description, etc.. :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-130.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-125.png" alt="image" width="450" height="176" border="0" /></a></p>

<p align="left">Par exemple pour suivre en temps réel la consommation électrique dans mon package C# avec le code généré à partir du capteur ESP8266 écrit en C++/Arduino :</p>

<pre class="lang:csharp decode:true">namespace ConstellationPackageConsole2
{
    using Constellation.Package;
    using ConstellationPackageConsole2.SElectricity.StateObjects;

    public class Program : PackageBase
    {
        [SElectricityStateObjectLink(SElectricityStateObjectNames.Electricity)]
        public StateObjectNotifier Electricity { get; set; }

        static void Main(string[] args)
        {
            PackageHost.Start&lt;Program&gt;(args);
        }

        public override void OnStart()
        {
            this.Electricity.ValueChanged += (s, e) =&gt;
            {
                PackageHost.WriteInfo($"Current Energy Consumption : {e.NewState.AsSElectricitySEnergy_Electricity().WattPerHour}W @ {e.NewState.LastUpdate}");
            };
        }
    }
}</pre>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-131.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-126.png" alt="image" width="350" height="176" border="0" /></a></p>

<h4 align="left">Code généré pour les MessageCallbacks</h4>

<p align="left">Le principe est le même avec les MessagesCallbacks, le générateur de code va créer le code dans le namespace “<em>VotreNamespace.NomDuPackage.MessagesCallbacks</em>” :</p>

<ul>
    <li>
<div align="left">Des classes pour chaque types personnalisés utilisés dans les MC du package</div></li>
    <li>
<div align="left">Une classe “<em>(NomDuPackage)Scope</em>” permettant de référencer les MC sous forme de méthodes .NET</div></li>
    <li>
<div align="left">Une classe “<em>(NomDuPackage)Extensions</em>” : classe d’extension pour créer un scope du package à partir d’un MessageScope ou des énumérations Sentinels, Packages, PackagesInstances générées par le générateur</div></li>
</ul>

<h5 align="left">MessageCallbacks avec ou sans paramètre</h5>

<p align="left">Prenons par exemple le package virtuel “IRremote”, un récepteur/émetteur d’infrarouge développé en Arduino/C++ sur un ESP8266. Ce package virtuel expose deux MessageCallbacks : “Restart” pour rebooter l’ESP et “SendCode” pour envoyer un signal IR.</p>

<p align="left">En utilisant la <a href="/client-api/arduino-esp-api/">librairie Constellation pour Arduino</a> de ces deux MC se résume par ces quelques lignes de C++ :</p>

<pre class="lang:cpp decode:true">// SendCode MessageCallback
constellation.registerMessageCallback("SendCode",
  MessageCallbackDescriptor().setDescription("Send the IR code").addParameter("encoding", "System.String").addParameter("code", "System.Int64"),
  [](JsonObject&amp; json) {
    const char * encoder = json["Data"][0].asString();  
    unsigned long code = strtoul(json["Data"][1].asString(), NULL, 0);
    // send the "code" here !
 });

// Restart MessageCallback
constellation.registerMessageCallback("Restart",
  MessageCallbackDescriptor().setDescription("Restart the ESP"),
  [](JsonObject&amp; json) {
    ESP.restart();
 });</pre>

Une fois l’ESP démarré, nos deux MessageCallbacks sont correctement référencés sur Console Constellation avec les listes des paramètres, types et description :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-132.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-127.png" alt="image" width="350" height="150" border="0" /></a></p>

Dans Visual Studio, générons maintenant le code pour notre package “IRRemote” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-133.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-128.png" alt="image" width="350" height="195" border="0" /></a></p>

Ajoutons ensuite le namespace correspondant aux MessageCallbacks de notre package, ici “IRRemote” :

<pre class="lang:csharp decode:true">using IRremote.MessageCallbacks;</pre>

Vous pouvez ensuite utiliser l’énumération “Packages” (ou “PackagesIntances”) et accéder à la méthode d’extension “<em>CreateIRRemoteScope</em>” pour créer un scope spécifiquement pour notre package :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-134.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-129.png" alt="image" width="450" height="109" border="0" /></a></p>

Vous avez également une méthode d’extension nommée “ToXXXXSCope()” sur un MessageScope. En clair vous avez plusieurs moyen de créer un scope spécifiquement pour votre package “IRremote” avec le code généré :

<pre class="lang:csharp decode:true">IRremoteScope scope =  MyConstellation.Packages.IRremote.CreateIRremoteScope();

IRremoteScope scope = new IRremoteScope(MessageScope.Create("IRRemote"));

IRremoteScope scope = MessageScope.Create("IRRemote").ToIRremoteScope();

IRremoteScope scope = MessageScope.Create(MyConstellation.Packages.IRremote.GetRealName()).ToIRremoteScope();
</pre>

Ensuite sur la classe Scope généré pour votre package, ici “<em>IRRemoteScope</em>”, vous <strong>retrouverez vos MessageCallbacks sous forme de méthode .NET avec les paramètres et descriptions</strong> !

De ce fait vous disposez de l’auto complétion sans risque de faire des erreurs de frappe :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-135.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-130.png" alt="image" width="450" height="152" border="0" /></a></p>

Ainsi pour envoyer un code IR sur l’Arduino/ESP depuis mon code C#, on pourrait écrire :

<pre class="lang:csharp decode:true">// Send Power ON/OFF to Samsung TV
MyConstellation.Packages.IRremote.CreateIRremoteScope().SendCode("Samsung", 0xE0E040BF);</pre>

<h5>MessageCallbacks avec des paramètres complexes</h5>

Ici le package “<em>IRRemote</em>” expose un MC sans paramètre et un autre avec deux paramètres simples (string et long).

Mais le générateur est également capable de gérer les types complexes. Par exemple prenez le package “Xbmc” permettant de piloter des média-centers Xbmc/Kodi.

Le package expose différents MessageCallbacks pour lancer un média, mettre pause, piloter le volume et également pour afficher un message à l’écran via le MessageCallback nommé “ShowNotification”.

Ce MessageCallback prend deux paramètres : le nom de l’hôte Xbmc (un string) et la notification à afficher. Cette notification est un objet de type “<em>Xbmc.NotificationRequest</em>” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-136.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-131.png" alt="image" width="350" height="155" border="0" /></a></p>

<p align="left">Sur la Console Constellation, vous pouvez cliquer sur les types personnalisés pour afficher les détails du type, ici un objet avec quatre propriétés :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-137.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-132.png" alt="image" width="350" height="172" border="0" /></a></p>

<p align="left">Lorsque vous générez le code C# pour ce package vous retrouverez bien le MC “<em>ShowNotification</em>” avec le type personnalisé en paramètre :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-138.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-133.png" alt="image" width="450" height="92" border="0" /></a></p>

Le générateur a en effet reproduit le type personnalisé dans votre code C# :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-139.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-134.png" alt="image" width="450" height="92" border="0" /></a></p>

Ainsi pour afficher une notification sur un hôte Kodi, je pourrais écrire très simplement :

<pre class="lang:csharp decode:true">MyConstellation.Packages.Xbmc.CreateXbmcScope().ShowNotification("Kodi", new NotificationRequest() { Title = "Constellation", Message = "Hello World !" });</pre>

<h5>MessageCallbacks avec réponse : les sagas</h5>

Une invocation d’un MessageCallback peut donner lieu à une réponse, on appelle cela les “<a href="/concepts/messaging-message-scope-messagecallback-saga/#Les_Sagas">Sagas</a>”. Avec l’API.NET, n’hésitez pas à relire les articles dédiés : <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Invoquer_un_MessageCallback_avec_reponse_Utilisation_des_Sagas">Invoquer un MessageCallback avec réponse</a> et <a href="/client-api/net-package-api/messagecallbacks/#Repondre_a_une_saga">Répondre à une saga</a>.

Prenons un exemple, le package “Vera” (interface pour les box domotique) expose des MC pour envoyer des ordres à des périphériques Z-Wave :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-140.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-135.png" alt="image" width="350" height="213" border="0" /></a></p>

<p align="left">Vous remarquerez d’ailleurs que les MC “<em>SetDimmableLevel</em>” et “<em>SetSwitchState</em>” prennent en argument un type complexe comme expliqué dans le chapitre précédent.</p>

<p align="left">Vous remarquerez également que ces trois MC retournent un message de réponse, ici de type “Boolean”. En effet le résultat de l’exécution de l’ordre Z-Wave par la Vera est retourné à l’appellent si celui-ci à attaché un numéro de Saga.</p>

<p align="left">Ainsi dans le code généré, les méthodes générées pour ces MessageCallbacks retournent une <em>Task&lt;T&gt; </em>où T est le type de retour, ici un booléen :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-141.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-136.png" alt="image" width="350" height="70" border="0" /></a></p>

Ainsi dans le C# on peut très facilement invoquer le MessageCallback et récupérer la réponse :

<pre class="lang:csharp decode:true"> bool result = await MyConstellation.Packages.Vera.CreateVeraScope().SetSwitchState(new DeviceRequest() { DeviceID = 42, State = false });
 if (result)
 {
     PackageHost.WriteInfo("Device #42 turn off !");
 }
 else
 {
     PackageHost.WriteWarn("Unable to turn off the device #42 !");
 }
</pre>

Pour plus d’information à ce sujet, je vous recommande la lecture de l’article : <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Invoquer_un_MessageCallback_avec_reponse_Utilisation_des_Sagas">Invoquer un MessageCallback avec réponse</a>.