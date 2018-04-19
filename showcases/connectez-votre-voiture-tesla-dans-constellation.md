---
ID: 4769
post_title: >
  Connectez votre voiture Tesla dans
  Constellation
author: Sebastien Warin
post_date: 2016-07-12 10:49:54
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/connectez-votre-voiture-tesla-dans-constellation/
published: true
publish_post_category:
  - "11"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:00:20
---
La plateforme Constellation permet l’intégration d’une multitude de périphériques ou de services de façon aisée et ce, peu importe la technologie et les langages utilisés.

Dans cet article, nous allons nous intéresser à l’ajout d’un périphérique ultime puisqu’il s’agit de l’interaction avec la voiture électrique de la marque Tesla, la Model S et d’un ensemble de différents capteurs déjà présents dans la constellation.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Vue d’ensemble de la solution" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig1_thumb.jpg" alt="Vue d’ensemble de la solution" width="450" height="159" border="0" /></a></p>

<h3>Créer un package Tesla pour Constellation</h3>
<h4>Découverte de l’API non-officielle de Tesla</h4>
La Tesla Model S est en permanence connectée via une connexion 3G intégrée qui communique avec les serveurs de Tesla leur permettant de récupérer des informations de diagnostic sur le véhicule mais aussi de délivrer des mises à jour « over the air » et d’apporter des fonctionnalités d’interaction avec le véhicule depuis l’application officielle disponible pour iOS et Android.

Certains développeurs ont donc cherché à comprendre comment ces applications interagissent avec l’infrastructure de Tesla pour piloter un ensemble de fonctionnalités de la voiture ou pour récupérer les informations sur la voiture et ils sont arrivés à fournir une spécification relativement exhaustive qui a permis le développement d’applications tierces (mobiles ou statistiques).

Il est à noter que cette interface peut tout à fait changer en fonction des choix faits par Tesla néanmoins nous allons explorer quelques fonctionnalités et préciser comment il est possible d’interagir avec le véhicule et le reste des équipements connectés à la Constellation.

Pour la sécurité, elle repose sur la mise en œuvre d’un protocole OAuth 2.0 classique dont il faut ensuite transporter le jeton en entête des requêtes ensuite transmises.

L’API gère plusieurs véhicules associés au même utilisateur, il est donc tout à fait possible de gérer plusieurs Tesla et pour les lister nous utiliserons la méthode dédiée (GET) :
<pre>https://owner-api.teslamotors.com/api/1/vehicles</pre>
L’API propose des fonctionnalités pour récupérer des informations sur le véhicule mais également pour exécuter des commandes qui peuvent agir sur la voiture. Voici un schéma présentant les fonctionnalités proposées :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig3.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="API Tesla" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig3_thumb.jpg" alt="API Tesla" width="450" height="227" border="0" /></a></p>

<h4>Intégration dans un package Constellation</h4>
En utilisant le <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">SDK de Constellation directement intégré à Visual Studio</a>, la création d’un package est simplifié et la réalisation du package dédié à Tesla se fait directement en utilisant le Framework .Net et en utilisant le langage C#. On commence par créer une application console en prenant soin d’intégrer l’API Tesla, nous créons une tache en background pour venir régulièrement interroger l’état de la voiture :
<pre class="lang:default decode:true">Task.Factory.StartNew(
    async () =&gt;
    {
        while (PackageHost.IsRunning)
        {
            // Get every information about the car
            this.GetDrivingInformation();
            this.GetChargeInformation();
            this.GetClimateInformation();
            this.GetVehicleInformation();

            await Task.Delay(PackageHost.GetSettingValue&lt;int&gt;("RefreshInterval"));
        }
    });
</pre>
Par exemple, la méthode « <em>GetClimateInformation</em> » invoque la méthode REST suivante :
<pre class="">https://owner-api.teslamotors.com/api/1/vehicles/{vehicle_id}/data_request/climate_state</pre>
Les données remontées nous permettent de récupérer l’état de la climatisation ainsi que les températures à l’intérieur du véhicule :
<pre class="lang:javascript decode:true">{
  "response": {
    "inside_temp": 17.0,          	// Température en degrés Celsius (°C) intérieur
    "outside_temp": 9.5,          	// Peut être null quand pas initialisé
    "driver_temp_setting": 22.6,  	// Consigne côté chauffeur (°C)
    "passenger_temp_setting": 22.6, 	// Consigne côté passager (°C)
    "is_auto_conditioning_on": false, 	// Fonctionnalité en Beta pour chauffage auto
    "is_front_defroster_on": null, 	// Dégivrage avant
    "is_rear_defroster_on": false,	// Dégivrage arrière
    "fan_status": 0               	// Vitesse de la ventilation
  }
}
</pre>
Une fois les données désérialisées dans un objet .NET, il suffit simplement de le publier dans la Constellation en tant que StateObject grâce à la méthode « <a href="/client-api/net-package-api/pushstateobject/">PushStateObject </a>».

Par exemple, lorsque nous récupérons l’état de la climatisation (classe ClimateInformation), nous remontons cet objet au niveau Constellation avec la ligne :
<pre class="lang:csharp decode:true">PackageHost.PushStateObject&lt;ClimateInformation&gt;("Tesla.Climate", informations,
       new Dictionary&lt;string, object&gt;
           {
                { "TimeStamp", DateTime.UtcNow.ToUnixTimestamp() },
                { "VehicleId", vehicleId }
           });
</pre>
Ainsi notre package remonte actuellement quatre StateObjects dans Constellation à intervalle régulier :
<ul>
 	<li>Tesla.Charge : informations quant à la batterie (méthode REST « charge_state »)</li>
 	<li>Tesla.Climate : informations quant à la climatisation (méthode REST « climate_state»)</li>
 	<li>Tesla.Drive : informations quant à la position et le déplacement de la voiture (méthode REST « drive_state»)</li>
 	<li>Tesla.State : informations quant à l’état de la voiture (méthode REST « vehicle_state»)</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="StateObject Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig5_thumb.png" alt="StateObject Explorer" width="454" height="175" border="0" /></a></p>
Vous pouvez explorer et <a href="/constellation-platform/constellation-console/stateobjects-explorer/">visualiser vos StateObjects depuis la console Console</a>. Par exemple, si nous ouvrons le StateObject «Tesla.Climate », nous retrouvons toutes les informations fournies par l’API Tesla :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="StateObject sur la climatisation" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig6_thumb.png" alt="StateObject sur la climatisation" width="404" height="475" border="0" /></a></p>
Dès lors, n’importe quel système connecté à Constellation, que ce soit le type d’application, peut connaitre en temps réel l’état de ma voiture, de la climatisation, la batterie, etc… Le package réalisé a donc la responsabilité d’interroger régulièrement l’API de Tesla pour mettre à jour les StateObjects dans la Constellation et donc agir tel un « connecteur » faisant le lien entre l’API Tesla et le monde Constellation.

Au-delà des StateObjects, un package Constellation peut également exposer des actions, c’est ce qu’on appelle des « MessageCallbacks ». En .NET, il suffit d’ajouter l’attribut « MessageCallback » pour exposer une méthode dans Constellation comme suit :
<pre class="lang:csharp decode:true">[MessageCallback]
public bool CommandFlashLight(string vehicleId)
{
    bool result = false;

    if (this.VehicleConfigurations.FirstOrDefault(c =&gt; c.Id == vehicleId) != null)
    {
        result = new VehicleCommandConnector(this.AuthenticationInformation, vehicleId).Flash();

        PackageHost.WriteInfo("Tesla Package - Command - Flash lights invoked");
    }

    return result;
}
</pre>
Concrètement, la méthode « Flash() » invoquera la méthode http/REST « flash_lights » avec l’URL suivante :
<pre>https://owner-api.teslamotors.com/api/1/vehicles/{vehicle_id}/command/flash_lights</pre>
Tout comme les StateObjects, il est possible d’<a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">explorer les MessageCallbacks</a> de chaque package de la Constellation depuis la Console. Ici notre package Tesla expose plusieurs méthodes invocables depuis tous les autres packages selon les paramètres de sécurité établis. Ces méthodes permettent de récupérer des informations sur le véhicules (charge, position, climatisation) ou de contrôler la voiture comme faire des appels de phare, klaxonner, ouvrir le toit ouvrant, régler la climatisation…
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig7_thumb.png" alt="MessageCallbacks Explorer" width="404" height="374" border="0" /></a></p>

<h3>Scénario 1 – Pilotage en fonction de la météo</h3>
De manière logique et simple, lorsque les conditions météos ne sont pas optimales et que le véhicule est resté ouvert, un module de supervision et d’intelligence ambiante provoque la fermeture du toit ouvrant ou signale à son utilisateur qu’une porte est restée ouverte.

Pour cela nous utilisons la station météo Netatmo installée chez moi et qui est composée de plusieurs modules ayant des rôles distincts utilisés en intérieur ou en extérieur (module central, module d’intérieur, module d’extérieur, pluviomètre et anémomètre) et fournissant ensuite des données accessibles à travers une API dédiée.

<img class="alignnone wp-image-3388 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/netatmo-logo-300x211.png" alt="" width="164" height="115" />

Au sein du catalogue de package mis à disposition sur le site de Constellation et produits par la communauté, on retrouve un package <a href="/package-library/netatmo/">NetAtmo </a>permettant de remonter l’état de toutes les sondes sous forme de StateObjects et il nous suffit alors de créer un package .NET pour gérer cette « intelligence » souhaitée.

Dans ce scénario, notre package a besoin de connaitre l’état de la voiture et du pluviomètre, pour cela, nous pouvons utiliser l’attribut « StateObjectLink » sur les propriétés souhaitées afin de lier notre package d’intelligence aux données des packages NetAtmo et Tesla :
<pre class="lang:csharp decode:true">[StateObjectLink(Package = "NetAtmo", Name = "Pluviomètre.Rain")]
public StateObjectNotifier Pluviometre { get; set; }

[StateObjectLink(Package = "Tesla", Name = "Tesla.State")]
public StateObjectNotifier Tesla { get; set; }
</pre>
Puis au démarrage du package, il suffit de s’attacher au changement d’état du pluviomètre pour être notifier en cas de pluie afin de vérifier si le toit ouvrant est bien fermé sinon, le fermer de manière automatique en invoquant le MessageCallback « CommandRoof » du package Tesla :
<pre class="lang:csharp decode:true">this.Pluviometre.ValueChanged += (s, e) =&gt; // lorsque l'état du pluviomètre change
{
    if ((int)e.NewState.DynamicValue &gt; 0) // si de la pluie est mesurée
    {
        PackageHost.WriteInfo("Il pleut !");
        if (this.Tesla.DynamicValue.RoofOpeningPercent.AssociateValue != 0) // si le toit ouvrant n'est pas complément fermé
        {
            PackageHost.WriteInfo("Toit ouvrant ouvert ! Fermeture automatique");
            PackageHost.CreateMessageProxy("Tesla").CommandRoof(vehiculeId, 0);
        }
    }
};
</pre>
Il est également possible de connaitre via le StateObject « Tesla.Drive » si le véhicule est en mouvement et sa position géographique pour vérifier qu’il est bien à proximité de la station météo utilisée ou sinon et au besoin, utiliser un service tiers de météo comme source d’information (ex : le package Forecast.io du catalogue disponible).

Contrairement au Model X, le Model S ne propose pas des portes qui soient pilotables à distance, cependant cet exemple, utilisant le toit ouvrant, illustre de manière simple la mise en œuvre d’un traitement externe ayant une possibilité d’interaction avec le véhicule via la Constellation.
<h3>Scénario 2 – Préchauffage/Pré climatisation le matin</h3>
En se basant sur les déplacements au sein de la maison ou du bureau, le module d’intelligence ambiante historise les déplacements dans les pièces et donc le parcours réalisé avant de quitter le lieu et anticipe ainsi le départ afin de préparer le véhicule en préchauffant ou pré-climatisant le véhicule en fonction des conditions météos.
<h4>Présentation des capteurs Estimote</h4>
Les capteurs Estimote utilisés ici sont des stickers résistants à l’eau et permettant simplement de rendre « connecté » n’importe quel objet sur lequel cet élément est collé.

Ces stickers intègrent des capteurs de déplacement, de température et d’orientation et peuvent renvoyer ces informations aux périphériques à proximité en plus de leur état logique (batterie, firmware…).

Ils peuvent communiquer selon deux protocoles de communication : iBeacon ou Nearable qui reposent tout deux sur une utilisation de Bluetooth Low Energy (BLE 4.0) ce qui permet de communiquer avec de multiples périphériques.
<h4>Analyse des parcours usuels et scénario retenu</h4>
Dans une application mobile déployée sur le téléphone, il est possible de remonter les informations broadcastées de manière régulière au server Constellation. Il est possible, pour cela, d’utiliser le SDK fourni par Estimote ou pour les plus courageux, implémenter les communications Bluetooth de manière à gérer les trames reçues, dans le cas présent une application PhoneGap permet de gérer ce cas avec notamment l’emploi d’un plugin spécifique.

En pratique, le suivi permet de tracer les modifications des observations des stickers et des signaux liés qui pourront être historisées à travers la Constellation en communiquant les StateObjects utiles via l’API REST disponible.

Si l’on traduit cela à des emplacements de stickers spécifiques et en filtrant quelque peu les données observées, on récupère des statistiques qui, une fois retranscrits sur un plan et dans une séquence organisée dans le temps, permettent d’identifier le scénario à retenir dans un créneau horaire visée :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig12.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Plans de maison" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig12_thumb.jpg" alt="Plans de maison" width="450" height="262" border="0" /></a></p>
Il est également possible d’analyser dynamiquement les déplacements et en extraire un « pattern » habituel pour une journée type : jour de semaine, weekend, jour férié… pour déclencher des scénarios sur des habitudes de déplacements identifiés dans la maison.

Plusieurs limites apparaissent dans cette première implémentation mais ces dernières ne limitent pas pour autant la possibilité d’appliquer le traitement souhaité. Parmi celles-ci on peut indiquer qu’il n’y a pas de différenciation des utilisateurs activant les capteurs ou que l’on n’a pas d’état direct d’une porte par exemple notamment si elle est déjà ouverte.

Néanmoins, à travers l’implémentation actuelle, les captures permettent de faire fonctionner le scénario et devront faire l’objet d’amélioration dans le traitement des messages émis par l’application ou reçus dans la Constellation.
<h4>Vérification des paramètres météorologiques depuis le package Netatmo</h4>
A travers le package NetAtmo, on récupère les informations météos de la station et on récupère également la température observée.

Aussi, la climatisation est un instrument à utiliser avec parcimonie et il est recommandé de ne pas excéder 5°C d’écart entre l’habitacle et l’extérieur. C’est la raison pour laquelle la température extérieure est récupérée et utilisée au sein de la règle mise en œuvre que voici :
<pre>SI la [Température Extérieure] &gt; 25°C ALORS
  Mettre la température à [Température Extérieure] – 5
SINON
  Mettre la température à 20°C
</pre>
Dès lors, ce paramètre évidemment modifiable dans la configuration du module garantit un confort optimal d’usage du véhicule.
<h4>Commandes et contrôle du véhicule</h4>
La récupération de l’état du système de climatisation permet d’éviter de modifier les paramètres qui pourraient avoir déjà été spécifiés par l’utilisateur dans le véhicule ou depuis l’application, ceci afin de ne pas écraser les valeurs spécifiées. Les données remontées nous permettent de récupérer l’état de la climatisation ainsi que les températures à l’intérieur du véhicule.

Pour récupérer l’état de la climatisation de la voiture et du capteur de température extérieure, on créée des propriétés « StateObjectLink »  :
<pre class="lang:csharp decode:true">[StateObjectLink(Package = "NetAtmo", Name = "Jardin.Temperature")]
public StateObjectNotifier TemperatureExt { get; set; }

[StateObjectLink(Package = "Tesla", Name = "Tesla.Climate")]
public StateObjectNotifier ClimTesla { get; set; }
</pre>
Lorsqu’il est l’heure de partir, on vient définir la température de consigne pour le conducteur et le passager grâce au MessageCallback « CommandTemperature » du package Tesla en fonction de la température extérieure mesurée par notre sonde NetAtmo :
<pre class="lang:csharp decode:true">if ((int) this.TemperatureExt.DynamicValue &gt; 25)
{
   int targetTemperature = (int) this.TemperatureExt.DynamicValue - 5; 
   PackageHost.WriteInfo("Il fait trop chaud, réglage de la climatisation de la Tesla à {0}°C !" , targetTemperature);
   PackageHost.CreateMessageProxy("Tesla").CommandTemperature(vehiculeId, targetTemperature, targetTemperature);
}
else
{
    PackageHost.WriteInfo("Réglage de la climatisation de la Tesla à 20°C !");
    PackageHost.CreateMessageProxy("Tesla").CommandTemperature(vehiculeId, 20, 20);
}
</pre>
<h3>Scénario 3 – Gestion de la charge liée au planning</h3>
<h4>Précisions sur la charge et la recharge</h4>
Même si la Model S propose une autonomie suffisante pour effectuer plusieurs centaines de kilomètres (420 km réels pour le modèle concerné 85D), il arrive parfois que le véhicule ne soit pas assez chargé pour le parcours du lendemain. Là où avec un modèle thermique, il suffit de se rendre dans une station-service et repartir en quelques minutes, la recharge à domicile prend davantage de temps (Pour information, chez moi de 0 à 90% en environ 13h, en pratique quelques heures suffisent car la charge n’est jamais si peu élevée).

Avec l’ensemble des modèles électriques du marché on a l’obligation d’attendre plus ou moins selon le véhicule, l’équipement à domicile ou les bornes sur lesquelles l’on est connecté. Dans l’optique de parcourir plusieurs centaines de kilomètres sans problèmes, Tesla est arrivé avec une approche disruptive et novatrice et propose un réseau, toujours en expansion, de bornes de recharge ultra-rapides appelées « superchargeurs » qui facilitent le déplacement lors des longs parcours sans effectuer des arrêts prolongés.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig13.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Super chargeurs Tesla" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig13_thumb.jpg" alt="Super chargeurs Tesla" width="454" height="246" border="0" /></a></p>

<h4>Synopsis</h4>
Dans le scénario retenu, on souhaite inspecter l’agenda de l’utilisateur et le calendrier professionnel pour vérifier les déplacements prévus, leur emplacement et donc vérifier que la charge du véhicule sera suffisante pour se rendre au rendez-vous ou s’il faut charger le véhicule en le connectant à domicile à la veille ou en partant légèrement plus tôt en passant via un superchargeur.
<h4>Vérification du planning de la journée et récupération des détails des événements</h4>
La plateforme Office 365 est accessible à travers l’API Microsoft Graph qui permet également de se connecter à de multiples autres plateformes, dans le cas présent nous souhaitons récupérer le calendrier et les événements contenus.

Afin de récupérer les événements à venir du calendrier, il est possible d’utiliser l’API dédiée et exécuter la méthode suivante :
<pre>https://graph.microsoft.com/beta/me/events?$top=5</pre>
A noter que cette méthode supporte les compléments de requête en supportant le protocole OData ce qui permet de filtrer davantage sur des propriétés des événements. Ces derniers ainsi retournés permettent de récupérer entre autres l’emplacement du rendez-vous, la date et l’heure précise ce qui seront utiles pour les traitements qui suivent.

Encore une fois, il existe un package Constellation permettant d’injecter un événement issu du calendrier dans un StateObject sur la Constellation
<h4>Calcul de l’itinéraire et récupération des informations de parcours</h4>
Pour réaliser le calcul de l’itinéraire, on retrouve plusieurs plateformes dédiées et qui proposent des fonctionnalités géographiques. Afin d’anticiper sur d’autres scénarios qui seront implémentés par la suite, nous choisissons d’utiliser la plateforme proposée par Here.

Voici la requête (GET) permettant de calculer l’itinéraire avec les alternatives utiles et le profil d’élévation :
<pre>https://route.cit.api.here.com/routing/7.2/calculateroute.json
?app_id={APP_ID}&amp;app_code={APP_CODE}
&amp;waypoint0=geo!51.325947,0.704923
&amp;waypoint1=geo!51.093373,1.119516
&amp;mode=fastest;car;traffic:disabled;
&amp;alternatives=3
&amp;routeAttributes=shape
&amp;returnelevation=true
</pre>
Il est ensuite possible de régénérer un itinéraire incluant l’arrêt au superchargeur en tenant compte de nombreux paramètres (trafic prédictif, heure de départ/arrivée, temps de charge…) ce qui peut être fait en utilisant le package Here disponible dans le catalogue et exposant les méthodes sous forme de MessageCallbacks permettant à tout autre package de tirer profit des fonctionnalités (géocodage, calcul d’itinéraires avancés, génération d’isochrone...).
<h4>Récupération de l’état de charge et stratégie de recharge</h4>
La récupération du niveau de charge du véhicule s’effectue par la récupération du dernier StateObject concerné dans la Constellation et les données renvoyées permettent de récupérer l’état de la batterie ainsi que des informations sur l’autonomie estimée du véhicule.

Voici par exemple la valeur de mon StateObject « Tesla.Charge » actuel :
<pre class="lang:javascript decode:true">{
  "BatteryCurrent": { "AssociateValue": 0, "Unit": "Ampere" },
  "BatteryLevel": { "AssociateValue": 69, "Unit": "Percentage" },
  "ChargeActive": false,
  "ChargePortOpen": false,
  "ChargerActualCurrent": { "AssociateValue": 0, "Unit": "Ampere" },
  "ChargeRate": { "AssociateValue": 0, "Unit": "KilometerPerHour" },
  "ChargerMaxCurrent": { "AssociateValue": 0, "Unit": "Ampere" },
  "ChargerPower": { "AssociateValue": 0, "Unit": "KilowattHour" },
  "ChargerVoltage": { "AssociateValue": 0, "Unit": "Volt" },
  "RangeBatteryEstimated": { "AssociateValue": 192.21, "Unit": "Mile" },
  "RangeBatteryNominal": { "AssociateValue": 180.09, "Unit": "Mile" },
  "RangeBatteryRated": { "AssociateValue": 225.11, "Unit": "Mile" },
  "RangeStart": null,
  "Supercharging": false,
  "TimeToCharge": { "AssociateValue": 0, "Unit": "Minute" }
}
</pre>
Un phénomène lié au caractéristique physique et chimique des batteries occasionne une légère déperdition d’énergie même au cours d’une période d’inactivité du véhicule, on parle alors de vampirisation de la batterie à l’arrêt, pouvant être négligeable dans la majorité des cas, on peut estimer ce phénomène à 1 km de perte / heure d’inactivité.

La charge s’effectue à une vitesse dépendante de l’équipement de recharge, mesurable via l’API et donc le package. A domicile, je charge à une vitesse de 32 kilomètres par heure de recharge et on peut alors calculer le temps nécessaire pour la recharge et on pourrait même envisager de piloter la recharge automatiquement (début et arrêt de charge).

Pour calculer le temps, l’énergie consommée et le coût estimé :
<pre>{Charge nécessaire} = ({distance du parcours} - {distance restant} + [marge confort/arrivée]) 
* {consommation estimée sur le parcours} / [efficacité de recharge] 
{Temps} = {Charge nécessaire} / {Taux de chargement}
{Coût} = {Charge nécessaire} * [taux du kwh]
</pre>
En pratique, dans un cas appliqué, la charge nécessaire :
<pre>{Charge nécessaire} = (150 kilomètres - 60 kilomètres restants + 40 kilomètres de marge) 
* (220 kwh/km moyen sur le parcours) 
/ (0.82 efficacité de recharge moyenne sur équipement 220V/32A)
{Charge nécessaire} = Environ 35 kwh (34.9 kwh)
</pre>
Le temps nécessaire :
<pre>{Temps} = 35 / 7.4 kwh en recharge
{Temps} = 280 min environ soit 4h40
</pre>
Le coût associé :
<pre>{Coût} = 35 kwh * [0.17£]		// 0.22 €, oui l'électricité est chère en UK
{Coût} = 5,95 £			// 7,70 €
</pre>
A noter, l’API de Tesla et donc notre package Tesla dans la Constellation retourne également le temps estimé de charge nécessaire pour atteindre la limite qui serait éventuellement configurée sur le véhicule. Il est alors aisé de prendre la décision et de ne pas « oublier » de charger en prévision du rendez-vous et donc d’arriver à l’heure pour ce dernier.
<h4>Alternative via superchargeurs sur le parcours</h4>
Une autre option consiste alors à étudier la possibilité d’utiliser un superchargeur sur la route et calculer le temps nécessaire pour s’y rendre au cours du parcours, charger et continuer le chemin.

Pour cela, nous avons créé une Web API dédiée, consommée depuis le package de gestion de la recharge et permettant de trouver un superchargeur à proximité d’une route possible pour se rendre à l’emplacement prévu, calculée par l’API Here décrite plus tôt, et donc ensuite de calculer les temps nécessaires de parcours et de charge.

Le traitement réalisé peut être illustré comme suit :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig16.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Processus" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Fig16_thumb.jpg" alt="Processus" width="300" height="321" border="0" /></a></p>
Il est alors possible enfin d’ajouter un déplacement dans l’agenda piloté via le package vers le Microsoft Graph pour intégrer cet arrêt de recharge au planning de la journée qui sera présenté en arrivant dans le véhicule.
<h4>Comparaison des deux alternatives</h4>
Ces alternatives reposent sur des estimations dans la recharge et dans le parcours et de fait, plusieurs limitations apparaissent notamment la prise en compte avancée des éléments du calcul d’itinéraire (pas de profil d’élévation, de conditions météo et trafic) mais également sur les informations du superchargeur (puissance de la borne, disponibilité en temps réel, prise en compte du l’état de la batterie notamment le besoin de préchauffage, fréquentation/utilisation prévue…) ce qui peut limiter quelque peu la justesse de la proposition, ces traitements pourront être améliorées dans la continuité de l’implémentation.

D’une manière simple, l’utilisateur peut alors observer un résumé de l’information sur son application mobile sur son téléphone ou n’importe quel périphérique connecté et peut alors prendre la décision en ayant connaissance du temps nécessaire et du coût pour chacune des options qui s’offrent à lui, apportant un réel gain de temps et un confort au quotidien.
<h3>Pour aller plus loin</h3>
La plateforme Constellation permet d’apporter le squelette facilitant les échanges entre les plateformes externes, les APIs personnalisées ou les objets connectés et ces usages dépassent dans le cas présent, la domotique habituellement présentée.

Les objets connectés sont en effet de plus en plus présents dans notre quotidien et le véhicule n’est pas en reste avec une nouvelle fois davantage d’échanges d’informations directement rendus possibles et nous avons pu le voir ici, dans la Constellation, l’ensemble des objets connectés peuvent interagir avec le véhicule ou fournir une information utile pour faire un traitement encore plus riche.

La Tesla, en véhicule avant-gardiste, propose des fonctionnalités très originales, souvent utiles au quotidien, parfois plus triviales mais toujours dans le sens de l’innovation et des avancées permettant de faciliter l’intégration de ce véhicule dans un contexte d’intelligence ambiante.

<b>Nicolas Boonaert, </b>MVP - Bing Maps for Enterprise, CEO @ <a href="http://www.ketto.fr" target="_blank" rel="noopener noreferrer">Ketto</a>