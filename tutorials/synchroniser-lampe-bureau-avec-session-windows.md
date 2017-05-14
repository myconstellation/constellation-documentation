---
ID: 4829
post_title: >
  Synchroniser la lampe du bureau avec sa
  session Windows
author: Sebastien Warin
post_date: 2017-05-14 14:35:00
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/synchroniser-lampe-bureau-avec-session-windows/
published: true
post_modified: 2017-05-14 14:37:57
---
L'un des avantages de Constellation est qu'il est très facile de faire "parler" des objets/systèmes entre eux. Dans ce tutoriel, nous allons lier notre lampe du bureau à notre session Windows.

L'idée est très simple : lorsque vous déverrouillez (ou ouvrez) votre session Windows on allumera automatiquement la lampe du bureau et lorsque vous verrouillez (ou fermez) votre session, la lampe s'éteindra automatiquement.
<p align="center"><img class="aligncenter wp-image-4833 size-full" title="Demo" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/WindowsControl.gif" alt="Demo" width="450" height="253" /></p>
On peut même aller plus loin si vous avez un capteur de luminosité connecté pour n'allumer la lampe que lorsqu'il fait trop sombre.
<h3>Prérequis</h3>
<ul>
 	<li>Un serveur Constellation</li>
 	<li>Une lampe pilotable par Constellation</li>
 	<li>Le package <a href="/package-library/windowscontrol/">WindowsControl</a> déployé sur la sentinelle de l'ordinateur Windows à synchroniser avec la lampe</li>
 	<li>Optionnellement un capteur de luminosité connecté dans Constellation</li>
 	<li>Le SDK Constellation pour Visual Studio</li>
</ul>
<h3>Etape 1 : piloter une lampe par Constellation</h3>
Bien entendu il faut d'abord pouvoir piloter une lampe par Constellation pour réaliser ce tutoriel !

Vous pouvez par exemple piloter un relais depuis un Raspberry en créant un package Python ou bien depuis un Arduino ou un ESP8266 comme vu <a href="/tutorials/creer-un-relais-connecte/">dans ce tutoriel</a>. Il suffit de créer une fonction pilotant un relais (simple manipulation d'une sortie digitale) et d'exposer cette fonction comme <a href="/concepts/messaging-message-scope-messagecallback-saga/">MessageCallback</a>. On pourra ainsi invoquer votre méthode pour ouvrir et fermer le relais et donc piloter une lampe depuis n'importe quel système connecté dans votre Constellation.
<p align="center"><img class="alignnone size-full wp-image-4599" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-58.png" alt="" width="350" height="198" /></p>
Autre solution utiliser une carte de relais connectée par USB qu'on pilotera avec le package <a href="/package-library/relayboard/">RelayBoard</a>. Ce package expose des MC permettant d'ouvrir ou fermer les relais et publie l'état de chacun d'entre eux comme StateObject. Il ne reste plus qu'à connecter une lampe sur l'un des relais.
<p align="center"><img class="alignnone size-full wp-image-3530" title="RelayBoard" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-143.png" alt="RelayBoard" width="176" height="180" /></p>
On peut aussi utiliser des prises Wifi Belkin Wemo qu'on connectera à Constellation avec le package <a href="/package-library/wemo/">Wemo</a>. Encore une fois ce package expose des MC permettant d'allumer ou d’éteindre la charge connectée dessus et publie son état en tant que StateObject. Il suffit d'y brancher une lampe !
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Prise Wemo Insight" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/wemo.jpg" alt="Prise Wemo Insight" width="200" height="200" border="0" /></p>
Dans la même idée, on peut utiliser des prises Somfy qu'on connectera à Constellation avec le package <a href="/package-library/rfxcom/">RFXCOM </a>nécessitant une passerelle RFXcom connectée en USB. Le package expose un MessageCallback permettant d'envoyer des ordres par radiofréquences aux équipements RTS (protocole Somfy) pour piloter par exemple des prises. Il n'y a pas de retour d'état sur ce protocole, donc il ne sera pas possible de récupérer son état mais on pourra allumer ou éteindre la prise et donc une lampe !
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Prise Somfy" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/prise-telecommandee-interieure-variateur-100-w-rts-2401092-somfy.jpg" alt="Prise Somfy" width="200" height="200" border="0" /></p>
Encore une autre possibilité, utiliser des prises ou des modules Z-Wave. Il faut pour cela un contrôleur Z-Wave qu'on connectera à Constellation. Vous pouvez soit utiliser le package <a href="/package-library/jeedom/">Jeedom </a>ou soit le package <a href="/package-library/vera/">Vera</a>.
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Prise Z-Wave AN158" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/an158.jpg" alt="Prise Z-Wave AN158" width="200" height="200" border="0" /></p>
On pourrait également utiliser des lampes ou des ampoules connectées de la gamme Phillips Hue qu'on connectera à Constellation grâce au package <a href="/package-library/hue/">Hue</a>.
<p style="text-align: center;"><img class="alignnone size-full wp-image-3598" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-166.png" alt="" width="350" height="175" /></p>
Pour ma part la lampe de mon bureau est contrôlée par une prise Z-Wave AN158 appairée sur une Vera Lite. Ce contrôleur Z-Wave est connecté à Constellation grâce au package <a href="/package-library/vera/">Vera</a>.

J'ai donc dans ma Constellation des StateObjets pour chaque périphérique Z-Wave et des MessageCallbacks pour envoyer des ordres Z-Wave. Par exemple depuis le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallbacks Explorer</a>, on retrouve le MC "<em>SetSwitchState</em>" permettant de définir l'état d'un switch (On/Off) Z-Wave.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-85.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-85.png" alt="MessageCallbacks Explorer" width="454" height="169" border="0" /></a></p>
<p style="text-align: left;" align="center">Depuis l'API.net il me suffit d'invoquer le code suivant pour allumer le device #42 :</p>

<pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = 42, State = true });</pre>
Vous pouvez appuyer sur le bouton <img class="alignnone size-full wp-image-2961" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-112.png" alt="" width="25" height="22" /> pour obtenir les exemples de code pour chaque MessageCallback. Par exemple toujours pour allumer le device Z-Wave #42 depuis un Arduino :
<pre class="lang:default decode:true ">constellation.sendMessage(Package, "Vera", "SetSwitchState", "{ 'DeviceID':42, 'State':true }");</pre>
Bref il existe différente méthode de piloter une lampe (ou n'importe quelle charge) depuis Constellation, soit en mode DIY en créant vous-même votre package réel (C#, Python, ...) ou virtuel (Arduino, ESP8266, Gadgeteer, ...) ou bien en utilisant des technologies telles RTS de Somfy, le Z-Wave, les lampes et ampoules Philips, les prises Wifi de Belkin, etc... avec les packages déjà disponibles dans le <a href="/plateforme/package-repository/">catalogue en ligne</a>.

Et quand bien même il n'existe pas de connecteur Constellation pour piloter votre lampe, libre à vous de créer votre propre package créant ainsi la passerelle, le connecteur entre Constellation et votre lampe.
<h3>Etape 2 : connaitre l'état de la session avec le package WindowsControl</h3>
Maintenant que nous avons un MessageCallback pour piloter la lampe du bureau, faut-il encore savoir si la session est ouverte ou non.

Pour cela vous avez dans le catalogue en ligne, le package <a href="/package-library/windowscontrol/">WindowsControl</a> qui expose différent MessageCallbacks pour verrouiller ou fermer une session, pour arrêter ou redémarrer l'ordinateur, pour contrôler le volume ou la luminosité (comme vu <a href="/tutorials/potentiometre-connecte-pour-controler-le-volume-ou-la-luminosite/">dans ce tutoriel</a>).

Ce package produit également un StateObject nommé "SessionLocked" de type booléen qui indique si la session est verrouillée (ou fermée) ou non.

Il suffit donc de <a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">déployer ce package</a> depuis la <a href="/constellation-platform/constellation-console/package-repository/">Console Constellation</a> sur la sentinelle UI du PC à synchroniser avec votre lampe. On suivra ainsi ce StateObject pour savoir quel est l'état de votre session et réagir tout changement d'état.
<h3>Etape 3 : synchroniser la lampe avec la session en C#</h3>
Passons au chose sérieuse, nous avons d'un côté un StateObject nous indiquant si la session est ouverte ou non et un MessageCallback nous permettant d'allumer ou d'éteindre la lampe du bureau, reste juste à créer le lien.

Pour cela nous allons <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">créer un package .NET en C#</a> depuis Visual Studio.

Dans la classe principale (<em>Program</em>), nous allons ajouter un <a href="/client-api/net-package-api/consommer-des-stateobjects/">StateObjectLink</a>, c'est à dire une propriété dans notre code liée au StateObject représentant l'état de la session en ajoutant les lignes :
<pre class="lang:default decode:true">[StateObjectLink("WindowsControl", "SessionLocked")]
public StateObjectNotifier SessionLocked { get; set; }</pre>
Prenez garde toutefois, si vous avez déployé le package WindowsControl sur plusieurs sentinelles de votre Constellation, le StateObjectLink ci-dessus sera lié à plusieurs StateObjects et non au StateObject de votre ordinateur à scruter. L’unicité d’un StateObject est obtenu par le triplet : “Sentinel + Package + Nom” car un nom de StateObject est unique pour une <a href="https://developer.myconstellation.io/concepts/instance-package-versioning-et-resolution/#Sentinel_Package_Instance_de_package">instance de package</a> (couple Sentinel / Package), de même qu’un package est unique pour une sentinelle et qu’une sentinelle est unique dans une Constellation.

De ce fait pour lier la propriété "<em>SessionLocked</em>" au StateObject "<em>SessionLocked</em>" de votre ordinateur on écrira :
<pre class="lang:default decode:true">[StateObjectLink("PC-SEB_UI", "WindowsControl", "SessionLocked")]
public StateObjectNotifier Session { get; set; }</pre>
Dans l'exemple ci-dessus, la machine est nommée "PC-SEB" et donc le nom de sa sentinelle "PC-SEB_UI" étant donné que le package WindowsControl est déployé sur la Sentinelle UI d'où le suffixe "_UI" dans le nom de la sentinelle. Vous pouvez bien entendu vérifier le nom exacte de votre sentinelle sur la Console Constellation.

Maintenant que l'état de la session de notre PC est synchronisé dans cette propriété .NET de notre code C#, nous allons ajouter un "handler" pour réagir au changement d'état :
<pre class="lang:c# decode:true">this.SessionLocked.ValueChanged += (s, e) =&gt;
{
    if ((bool)this.SessionLocked.DynamicValue)
    {
        PackageHost.WriteInfo("Verrouillage de la session : fermeture de la lampe");
    }
    else
    {
        PackageHost.WriteInfo("Session déverrouillée : allumage de la lampe");
    }
};
</pre>
Maintenant pour allumer la lumière, il suffit d'envoyer un message pour déclencher le MessageCallback permettant de contrôler votre lampe. Comme expliqué ci-dessus, dans mon cas, la lampe sur mon bureau est contrôlée par une prise Z-Wave AN158 pilotable par le package Vera via le MessageCallback "<em>SetSwitchState</em>".

On crée donc <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">un proxy dynamique vers le package</a> Vera et on invoque le MC comme on invoquerait une méthode .NET en passant en paramètre un objet contenant l'ID du device Z-Wave et l'état souhaité :
<pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = 42, State = true });</pre>
Pour connaitre l'ID du device Z-Wave on peut soit le rechercher dans le StateObjects Explorer et l'écrire en dur dans notre code C#, ou soit ajouter un autre StateObjectLink afin de récupérer dynamiquement son Id (car contenu dans le StateObject du device).

Le code de notre package sera donc :
<pre class="lang:c# decode:true">public class Program : PackageBase
{
    [StateObjectLink("Vera", "Lampe Bureau Seb")]
    public StateObjectNotifier LampeBureau { get; set; }

    [StateObjectLink("PC-SEB_UI", "WindowsControl", "SessionLocked")]
    public StateObjectNotifier SessionLocked { get; set; }

    static void Main(string[] args)
    {
        PackageHost.Start&lt;Program&gt;(args);
    }

    public override void OnStart()
    {
        this.SessionLocked.ValueChanged += (s, e) =&gt;
        {
            if ((bool)this.SessionLocked.DynamicValue)
            {
                PackageHost.WriteInfo("Verrouillage de la session : fermeture de la lampe");
                PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.LampeBureau.DynamicValue.Id, State = false });
            }
            else
            {
                PackageHost.WriteInfo("Session déverrouillée : allumage de la lampe");
                PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.LampeBureau.DynamicValue.Id, State = true });
            }
        };
    }
}</pre>
On peut maintenant lancer notre package en mode "Debug On Constellation" pour le tester depuis Visual Studio tout en le connectant dans votre Constellation :

<img class="alignnone size-full wp-image-1675 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-196.png" alt="" width="104" height="34" />

Vous pouvez tester le fonctionnel de votre package : on verrouille la session, la lampe s'éteint, on la déverrouille, la lampe s'allume !

Une fois votre package testé et validé, vous pouvez <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">le publier</a> dans votre Constellation et le déployer sur une de vos sentinelles (<a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Publier_son_package_dans_Constellation">voir le guide</a>).
<h3>Etape 4 : ajouter un capteur de luminosité</h3>
Jusqu'à présent dès que la session est déverrouillée la lampe du bureau s'allume et dès qu'on la ferme elle s’éteint. Seulement en pleine journée ce n'est peut être pas très judicieux d'allumer la lumière !

Pour cela nous pouvons ajouter une condition supplémentaire basée sur la luminosité ambiante.

On pourrait utiliser un capteur sans fil du marché comme le Chacon DIO54783 connecté via le package RFXcom ou bien un capteur Z-Wave via le package Vera ou Jeedom. On peut aussi concevoir son propre capteur avec un Arduino ou un ESP8266 <a href="/tutorials/creer-un-capteur-de-luminosite-dans-une-prise-220v/">comme vu dans ce tutoriel</a> ou avec un Raspberry Pi <a href="/tutorials/un-capteur-de-luminosite-exterieur-pilote-par-raspberry/">comme vu ici</a>.
<p style="text-align: center;"><a href="/tutorials/creer-un-capteur-de-luminosite-dans-une-prise-220v/"><img class="alignnone wp-image-4818" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-84.png" alt="" width="351" height="312" /></a></p>
<p style="text-align: center;"><a href="/tutorials/un-capteur-de-luminosite-exterieur-pilote-par-raspberry/"><img class="alignnone wp-image-4749 size-full" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1170883_thumb-1.jpg" alt="" width="354" height="227" /></a></p>
Dans notre article, prenons par exemple l'un des deux capteurs ci-dessus qui publient chacun un StateObject nommé "Lux" composé de 3 propriétés : Broadband, IR et Lux.

Pour intégrer cette "information" dans notre code C#, ajoutons un StateObjectLink vers le StateObject "Lux" :
<pre class="lang:c# decode:true">[StateObjectLink("LightSensor", "Lux")]
public StateObjectNotifier LuxSensor { get; set; }</pre>
On peut maintenant modifier la condition d'allumage avec un "else if" qui vérifiera que la luminosité est inférieure à un seuil qu'on <a href="/client-api/net-package-api/settings/">va déclarer dans les settings</a> Constellation de façon à pouvoir modifier cette valeur à la volée depuis la Console Constellation :
<pre class="lang:c# decode:true">else if (this.LuxSensor.DynamicValue.Lux &lt; PackageHost.GetSettingValue&lt;int&gt;("DaylightThreshold"))</pre>
Bien entendu comme tout setting, il est vivement recommandé de le déclarer dans le <a href="/concepts/package-manifest/">manifeste du package</a> (le fichier <em>PackageInfo.xml</em>). De plus on pourra y définir la valeur par défaut ici fixée à 50 (lux) :
<pre class="lang:xhtml decode:true">&lt;Settings&gt;
  &lt;Setting name="DaylightThreshold" type="Int32" defaultValue="50" description="Seuil de luminosité minimale pour l'allumage de la lampe du bureau" /&gt;
&lt;/Settings&gt;</pre>
Pour finir, prenons le cas où la session est déjà déverrouillée et la soirée arrivant, la luminosité passe en dessous du seuil : il faut alors allumer la lumière bien que l'état de la session n'a pas changé !

Pour cela nous allons ajouter un "handler" pour réagir à chaque mise à jour du StateObject "Lux" du capteur de luminosité. A chaque nouvelle mesure, si la valeur est inférieure au seuil défini dans les settings et que la session est bien déverrouillée, on allumera la lumière :
<pre class="lang:c# decode:true">this.LuxSensor.ValueChanged += (s, e) =&gt;
{
    if (e.NewState.DynamicValue.Lux &lt; PackageHost.GetSettingValue&lt;int&gt;("DaylightThreshold") &amp;&amp; (bool) this.SessionLocked.DynamicValue == false)
    {
        PackageHost.WriteInfo("Session déverrouillée à la tombée de la nuit : allumage de la lampe");
        PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.LampeBureau.DynamicValue.Id, State = true });
    }
};</pre>
Ainsi notre lumière de bureau est maintenant synchronisée à notre session et ne s'allumera que si la luminosité est faible. Elle sera donc éteinte dès qu'on ferme ou verrouille la session et s'allumera si la luminosité baisse sous le seuil défini dans les settings alors que la session est déjà déverrouillée ou vient d'être déverrouillée.
<h3>Etape 5 : fermer la lampe en cas de déconnexion du PC</h3>
Pour vraiment aller au fond des choses il y a encore un cas à gérer : que faire si le PC s’arrête proprement ou brutalement. En somme l'idée est de fermer la lampe si le PC est déconnecté de Constellation, que ce soit suite à un arrêt de l'ordinateur, un crash, une déconnexion quelconque, etc..

Pour savoir si le PC est bien connecté on va surveiller l'état de la sentinelle UI de votre PC. Pour cela il faut se connecter sur le hub de contrôle.

L'utilisation du hub de contrôle depuis un package C# est expliqué en détail <a href="/client-api/net-package-api/controlmanager/">dans cet article </a>que je vous recommande de lire.

La première étape consiste donc à déclarer dans le <a href="/concepts/package-manifest/">manifeste du package</a> (le fichier <em>PackageInfo.xml</em>) l'utilisation du hub de contrôle par votre package en ajoutant l'attribut suivant sur la balise <em>Package</em> :
<pre class="lang:xhtml decode:true">EnableControlHub="true"</pre>
De plus il faut également, sur votre package, définir une <a href="/constellation-platform/constellation-console/gerer-credentials-avec-la-console-constellation/">clé d'accès ayant les droits d'accès au hub de contrôle</a>. Vous pouvez vérifier la bonne connexion au hub de contrôle dans votre code par la propriété : "<em>PackageHost.HasControlManager</em>".

Une fois connecté au hub de contrôle on va demander à recevoir les mises à jour des statuts des sentinelles de notre Constellation par la ligne :
<pre class="lang:c# decode:true">PackageHost.ControlManager.RequestSentinelUpdates();</pre>
Cela nous permet ensuite d'attacher un "handler" qu'on filtrera sur notre sentinelle (ici nommée "PC-SEB_UI"). On stockera dans la variable "pcConnected" un booléen indiquant si la sentinelle de notre PC est connectée ou non. Si notre sentinelle n'est pas/plus connectée, on fermera immédiatement la lampe du bureau.
<pre class="lang:c# decode:true">PackageHost.ControlManager.SentinelUpdated += (s, e) =&gt;
{
    if (e.Sentinel.Description.SentinelName == "PC-SEB_UI")
    {
        this.pcConnected = e.Sentinel.IsConnected;
        PackageHost.WriteInfo("Le PC de Seb est '{0}'", e.Sentinel.IsConnected ? "connecté" : "déconnecté");
        if (!this.pcConnected)
        {
            // Éteindre la lampe si le PC est déconnecté !
            PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.LampeBureau.DynamicValue.Id, State = false });
        }
    }
};</pre>
De plus, dans la condition d'allumage, on rajoutera le fait de vérifier que le PC est bien connecté avant de prendre la décision d'allumer la lampe :
<pre class="lang:c# decode:true">else if (this.pcConnected &amp;&amp; this.LuxSensor.DynamicValue.Lux &lt; PackageHost.GetSettingValue&lt;int&gt;("DaylightThreshold"))</pre>
Et voilà, notre package est opérationnel ! La lampe est parfaitement synchronisée avec l'état de notre session Windows en tenant compte de la luminosité ambiante et des différents cas de figure pouvant apparaître, comme le redémarrage, l’arrêt ou le crash de notre PC !

Le code final est :
<pre class="lang:c# decode:true crayon-selected">public class Program : PackageBase
{
    // Nom de la sentinelle Windows à syncrhoniser avec la lampe
    private const string SEB_PC_SENTINEL_NAME = "PC-SEB_UI";
    
    // PC Connecté? oui ou non !
    private bool pcConnected = false;

    // Lien vers le SO de la lampe du bureau
    [StateObjectLink("Vera", "Lampe Bureau Seb")]
    public StateObjectNotifier LampeBureau { get; set; }

    // Lien vers l'état de la session Windows
    [StateObjectLink(SEB_PC_SENTINEL_NAME, "WindowsControl", "SessionLocked")]
    public StateObjectNotifier SessionLocked { get; set; }

    // Lien vers le capteur de luminosité
    [StateObjectLink("LightSensor", "Lux")]
    public StateObjectNotifier LuxSensor { get; set; }

    // Démarrage du package
    static void Main(string[] args)
    {
        PackageHost.Start&lt;Program&gt;(args);
    }

    // Méthode de démarrage de notre package
    public override void OnStart()
    {
        // Si accès au ControlHub
        if (PackageHost.HasControlManager)
        {
            // Demande de reception des mise à jour des sentinelles
            PackageHost.ControlManager.RequestSentinelUpdates();
            // En cas de mise à jour de l'état d'une sentinelle
            PackageHost.ControlManager.SentinelUpdated += (s, e) =&gt;
            {
                // Si cela concerne notre sentinelle
                if (e.Sentinel.Description.SentinelName == SEB_PC_SENTINEL_NAME)
                {
                    // On stocke dans la variable "pcConnected" l'état connecté ou déconnecté de notre PC
                    this.pcConnected = e.Sentinel.IsConnected;
                    PackageHost.WriteInfo("Le PC de Seb est '{0}'", e.Sentinel.IsConnected ? "connecté" : "déconnecté");
                    // Si le PC est maintenant déconnecté, on ferme la lampe !
                    if (!this.pcConnected)
                    {
                        SetLight(false);
                    }
                }
            };
        }
        else
        {
            // Pas d'accès au COntrolHub, on log une erreur ! Vérifiez votre clé d'accès !
            PackageHost.WriteError("Accès au ControlHub impossible !");
        }

        // En cas de mise à jour du StateObject "Lux" du package "LightSensor"
        this.LuxSensor.ValueChanged += (s, e) =&gt;
        {
            // Si le PC est connecté, que la luminosité actuelle est inférieure au seuil défini dans les settings et que la session est bien dévérouillée
            if (this.pcConnected &amp;&amp; e.NewState.DynamicValue.Lux &lt; PackageHost.GetSettingValue&lt;int&gt;("DaylightThreshold") &amp;&amp; (bool) this.SessionLocked.DynamicValue == false)
            {
                // On allume la lampe !
                SetLight(true);
                PackageHost.WriteInfo("Session dévérouillée à la tombée de la nuit : allumage de la lampe");
            }
        };

        // En cas de mise à jour du StateObject "SessionLocked" du package "WindowsControl"
        this.SessionLocked.ValueChanged += (s, e) =&gt;
        {
            // Si la session est vérouillée (ou fermée)
            if ((bool)this.SessionLocked.DynamicValue)
            {
                // On eteint la lampe
                SetLight(false);
                PackageHost.WriteInfo("Verrouillage de la session : fermeture de la lampe");
            }
            // Si le PC est connecté, que la luminosité actuelle est inférieure au seuil défini dans les settings et que la session est donc dévérouillée
            else if (this.pcConnected &amp;&amp; this.LuxSensor.DynamicValue.Lux &lt; PackageHost.GetSettingValue&lt;int&gt;("DaylightThreshold"))
            {
                // On allume la lampe !
                SetLight(true);
                PackageHost.WriteInfo("Session déverrouillée avec luminosité faible : allumage de la lampe");
            }
        };
    }

    private void SetLight(bool state)
    {
        // Pour allumer la lumière, on invoque le MC "SetSwitchState" du package "Vera" en passant l'ID de la lampe (récupérée par son StateObjet)
        PackageHost.CreateMessageProxy("Vera").SetSwitchState(new { DeviceID = (int)this.LampeBureau.DynamicValue.Id, State = state });
    }
}</pre>