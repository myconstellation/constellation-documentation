---
ID: 5668
post_title: 'Cr&eacute;er une prise connect&eacute;e avec un ESP8266'
author: Lucas
post_date: 2017-10-31 12:29:46
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/creer-une-prise-connectee-avec-un-esp8266/
published: true
post_modified: 2017-10-31 12:29:46
---
La prise connectée est un élément phare de la domotique de la maison. Il permet d'allumer ou d'éteindre un équipement branché dessus ou encore de connaitre sa consommation en énergie. Dans mon cas, j'avais besoin d'allumer ou d'éteindre les enceintes de mon média center automatiquement lorsque ce dernier était démarré.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-08-16.32.30.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Prise connectée" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-08-16.32.30_thumb.jpg" alt="Prise connectée" width="404" height="304" border="0" /></a></p>
<p align="left">Découvrons ensemble comment créer sa prise connectée avec un ESP8266.</p>
<!--more-->
<h2>Prérequis</h2>
Pour ce tutoriel, il vous faut :
<ul>
 	<li>Un bloc prise avec un interrupteur</li>
 	<li>Un transformateur AC/DC 5v</li>
 	<li>Un ESP-01 (ESP8266)</li>
 	<li>Un régulateur de tension 3.3v</li>
 	<li>Un relais 220V pilotable en 5v</li>
 	<li>Un transistor, des résistances, des leds, une diode, des condensateurs</li>
 	<li>Du fil électrique</li>
 	<li>Un pistolet à colle et une drémel</li>
 	<li>Un serveur Constellation</li>
</ul>
<h2>Etape 1 : Construire la prise</h2>
Dans un premier temps, il nous faut un boitier abordable que nous pourrons ouvrir pour insérer notre ESP à l'intérieur. Après quelques recherches, j'ai opté pour le <a href="http://www.conrad.fr/ce/fr/product/778994/Prise-intermdiaire-commutable-Renkforce-778994-1-ple-argent">boitier Renkforce disponible chez Conrad</a> pour 3€ environ.

On commence donc par l’ouvrir pour la vider littéralement :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.38.32.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Prise Renkforce" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.38.20_thumb.jpg" alt="Prise Renkforce" width="204" height="271" border="0" /><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Prise Renkforce" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.38.32_thumb.jpg" alt="Prise Renkforce" width="204" height="271" border="0" /></a></p>
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Démontage" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.41.28_thumb.jpg" alt="Démontage" width="244" height="184" border="0" /><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.41.28.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Prise démontée" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-11-10-22.44.15_thumb.jpg" alt="Prise démontée" width="244" height="184" border="0" /></a></p>
Puis tous les supports plastique à l'intérieur doivent être cassés pour libérer un maximum de place. Je les ai cassés avec une pince coupante et j'ai fini de retirer le maximum de plastique avec un dremel. Sur cette photo j'avais retiré une partie du fond de la prise pour un autre projet.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-12-18-11.25.15.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Usinage" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-12-18-11.25.15_thumb.jpg" alt="Usinage" width="354" height="266" border="0" /></a></p>
Il faut ensuite préparer un câblage avec le relais pour qu'il s'intercale entre l'arrivée de la phase (mur) et la phase distribuée à l'élément branché sur la prise. Mais il faut également garder en tête que l'alimentation de l'ESP doit être permanente :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-10-22.52.19.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-10-22.52.19_thumb.jpg" alt="Schéma" width="354" height="266" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/Schma-relais.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Schéma" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/Schma-relais_thumb.png" alt="Schéma" width="350" height="321" border="0" /></a></p>
Mon principal problème dans ce tutoriel a été de tout faire rentrer dans la prise. En effet, l'alimentation + le relais prennent beaucoup de place et tout est rentré au chausse-pied, avec le câblage noyé dans la colle chaude afin d'assurer l'isolation.

Le transformateur alimente donc en 5v un régulateur de tension LM1117 3.3V avec deux condensateurs pour lisser du 3.3v pour l'ESP01.

Il alimente également directement la bobine du relais dont le circuit est interrompu par un transistor NPN BC547 dont la base sera pilotée en saturation par un GPIO de l'ESP.

L'ESP pilote deux leds de statut : une rouge et une verte et possède également son dernier GPIO en input pour un bouton physique placé sur le dessus du boitier. Si vous avez suivi jusque-là et que vous connaissez l'ESP01, vous aurez compris qu'il est impossible de le programmer directement dans la prise, deux des 4 GPIO devant normalement être utilisés pour la communication série.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-07-16.00.46.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="2016-08-07 16.00.46" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-07-16.00.46_thumb.jpg" alt="2016-08-07 16.00.46" width="454" height="342" border="0" /></a></p>
Pour combler le "trou" du bouton physique original, j'ai choisi de coller par l'intérieur du boitier un petit bout de plexiglas translucide. Je l'ai ensuite percé pour faire passer les deux leds rouge et verte.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-12-18-18.27.32.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="2016-12-18 18.27.32" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-12-18-18.27.32_thumb.jpg" alt="2016-12-18 18.27.32" width="244" height="184" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-22-23.08.43.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="2016-08-22 23.08.43" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/2016-08-22-23.08.43_thumb.jpg" alt="2016-08-22 23.08.43" width="244" height="184" border="0" /></a></p>
<p align="left">Et voilà, on obtient une prise connectée par Wifi avec un ESP8266 avec un bouton poussoir et deux LEDs, reste plus qu’à le programmer !</p>

<h2>Etape 2 : la programmation</h2>
La fonction de base de la prise est assez simple : couper le courant ou le laisser passer. Dans un premier temps, j'ai uploadé un sketch de base Constellation avec Arduino sur l'ESP01 à l'extérieur de la prise. Je l'ai ensuite branché dans la prise que j'ai enfiché dans le mur. Bazinga, le régulateur 3.3v fait son job, l'ESP boote, se connecte à mon réseau wifi et envoie un "hello world" dans la console Constellation. Pour découvrir comment connecter un ESP8266 à Constellation, <a href="https://developer.myconstellation.io/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">suivez ce guide</a>.

Ensuite, j'ai utilisé la librairie Constellation <a href="https://developer.myconstellation.io/client-api/arduino-esp-api/recevoir-des-messages-et-exposer-des-methodes-messagecallback-sur-arduino-esp/">pour ajouter un MessageCallback</a> pour activer ou désactiver le GPIO de la prise, <a href="https://developer.myconstellation.io/client-api/arduino-esp-api/produire-des-stateobjects-depuis-arduino-esp/">couplé à un StateObject</a> pour maintenir l’état de la prise dans Constellation :
<pre title="Liaison entre la prise et l'état de Kodi" class="lang:csharp decode:true">constellation.registerMessageCallback("Switch", MessageCallbackDescriptor().setDescription("Switch le statut du relais."),
  [](JsonObject &amp; json) {
    statutRelais = !statutRelais;
    digitalWrite(gpioRelais, statutRelais);
    constellation.pushStateObject("Status", stringFormat("{ 'IsActivated':%s }", statutRelais ? "true" : "false" ));
  });
</pre>
<p align="left">Ainsi Constellation a toujours connaissance de l’état de la prise via le StateObject nommé “Status” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/StateObject.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject de l'état de la prise" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/StateObject_thumb.png" alt="StateObject de l'état de la prise" width="354" height="259" border="0" /></a></p>
<p align="left">Et tout le monde peut maintenant découvrir et utiliser le MessageCallback “Switch” exposé par notre ESP pour permuter l’état de notre prise :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/10/MessageCallback2-002.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MessageCallback" src="https://developer.myconstellation.io/wp-content/uploads/2017/10/MessageCallback2-002_thumb.png" alt="MessageCallback" width="354" height="115" border="0" /></a></p>
<p align="left">Bingo, on a donc une prise 220V connectée à Constellation qu’on pourra piloter depuis une page Web, un programme Python ou autre.</p>
<p align="left">Pour vous donnez quelques idées, n’hésitez pas à relire ce tutoriel : <a href="https://developer.myconstellation.io/tutorials/creer-un-relais-connecte/">Créer un relais connecté</a>.</p>

<h2 align="left">Etape 3 : Lier sa prise connectée à l’état de son média-center</h2>
<p align="left">Dans ma Constellation, je dispose d'un package "brain" développé en C# avec Visual Studio qui contient l’ensemble des règles de la maison (gestion du chauffages, lumières, volets, etc..).</p>
<p align="left">Je l’ai enrichi pour faire en sorte que si Kodi est en train de lire un média (audio ou vidéo) et que la prise n’est pas allumée, alors il invoque le MessageCallback pour allumer la prise. Et inversement pour l'éteindre !</p>
<p align="left">J’ai donc dans une classe C#, ajouté <a href="https://developer.myconstellation.io/client-api/net-package-api/consommer-des-stateobjects/#Les_StateObjectLink">deux StateObjectLinks</a>, c’est à dire que j’ai deux propriétés de mon code C# qui sont liées à mes StateObjets représentant l’état de mon media-center de l’état de ma prise !</p>
<p align="left">Il me reste plus qu’à ajouter un handler sur le changement d’état du State Object de Kodi, afin d’ajouter deux conditions “if” :</p>

<ul>
 	<li>
<div align="left">Si la prise est éteinte alors que Kodi joue quelque chose (PlayerState différent de null) alors on allume la prise</div></li>
 	<li>
<div align="left">Si la prise est allumée alors que Kodi joue rien (PlayerState null) alors on éteint la prise</div></li>
</ul>
<p align="left">Pour allumer ou éteindre la prise, il suffit d’invoquer le MessageCallback “Switch” exposé par notre code Arduino <a href="https://developer.myconstellation.io/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">en créant un proxy vers notre package.</a></p>

<pre title="Liaison entre la prise et l'état de Kodi" class="lang:csharp decode:true">public class KodiDemo
{
    /// &lt;summary&gt;
    /// StateObject XBMC. Permet de connaitre les infos de lecture.
    /// &lt;/summary&gt;
    [StateObjectLink(Package = "Xbmc", Name = "Kodi Salon NUC")]
    public StateObjectNotifier KodiNotifier { get; set; }

    /// &lt;summary&gt;
    /// StateObject de l'ESP controlant le relais d'activation. Permet de synchroniser les infos de lecture avec la valeur du relais.
    /// &lt;/summary&gt;
    [StateObjectLink(Sentinel = "ESP8266-01-001", Package = "ESP_Relay_Button", Name = "Status")]
    public StateObjectNotifier PriseKodi { get; set; }

    public void Start()
    {
        this.KodiNotifier.ValueChanged += (s, e) =&gt;
        {
            if (this.PriseKodi.DynamicValue.Status == false
                &amp;&amp; e.IsNew == false
                &amp;&amp; e.OldState.DynamicValue.PlayerState == null
                &amp;&amp; e.NewState.DynamicValue.PlayerState != null)
            {
                // démarrage.
                PackageHost.WriteInfo("Activation de la prise.");
                PackageHost.CreateMessageProxy("ESP8266_01_002/ESP_Relay_Button").Switch();
            }

            if (this.PriseKodi.DynamicValue.Status == true
                &amp;&amp; e.IsNew == false
                &amp;&amp; e.OldState.DynamicValue.PlayerState != null
                &amp;&amp; e.NewState.DynamicValue.PlayerState == null)
            {
                PackageHost.WriteInfo($"Arret de la prise.");
                PackageHost.CreateMessageProxy("ESP8266_01_002/ESP_Relay_Button").Switch();
            }
        };
    }
}</pre>
Et voilà comment en quelques lignes de C# et grâce à Constellation, mes enceintes seront automatiquement allumées ou éteintes selon que mon media-center diffuse ou non un média vidéo ou audio !
<h2>Pour aller plus loin</h2>
Pour aller plus loin, j'ai ajouté quelques fonctionnalités intéressantes :
<ul>
 	<li>J'ai pluggé le bouton poussoir ajouté sur le dessus de la prise pour qu'il change l'état du relais et mette à jour le state objet en conséquence.</li>
 	<li>J'ai ajouté la possibilité d'associer les leds de façade au fonctionnement de la prise en m'inspirant de ce qui existe sur les prises connectées du marché. La led rouge indique le statut de fonctionnement (power on / connexion au wifi en clignotant), la led verte indique l'état du relais.</li>
 	<li>J'ai ajouté également un mode "blind", je trouve que c'est une fonctionnalité intéressante mais qui est absente des prises sur le marché : Quand il fait noir dans une pièce et que la prise se reconnecte au wifi, cela peut être gênant de la voir clignoter. Un package de "brain" peut alors gérer les leds directement en <a href="https://developer.myconstellation.io/showcases/connecter-volets-constellation-arduino-raspberry/">fonction de mes volets</a> :)</li>
 	<li>Ensuite, en cas de déconnection du wifi ou de coupure de courant, j'ai prévu un bout de code permettant, au démarrage de l'ESP, de requêter son propre StateObject. Cela permet à la prise de revenir à l'état dans lequel elle était avant la coupure.</li>
 	<li>J'ai également fait intervenir <a href="https://developer.myconstellation.io/tutorials/connecter-un-video-projecteur-dans-constellation/">l'activation de mon projecteur</a>. Ce dernier push un StateObject. Si le média center est éteint, il envoie un paquet WOL via <a href="https://developer.myconstellation.io/package-library/networktools/">le package networktools</a> pour l'allumer et envoie une notification de fermeture des volets du salon. Le démarrage de la lecture du média sur kodi pilote la prise d'allumage des enceintes sans action manuelle. Ainsi, le démarrage du projecteur et la lecture sur kodi lancent l'ambiance parfaite pour profiter de mes séries en un seul geste.</li>
</ul>