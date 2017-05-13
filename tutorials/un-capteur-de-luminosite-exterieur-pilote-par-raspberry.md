---
ID: 4730
post_title: 'Un capteur de luminosit&eacute; ext&eacute;rieur pilot&eacute; par un Raspberry avec un package Python'
author: Sebastien Warin
post_date: 2017-05-13 14:46:15
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/un-capteur-de-luminosite-exterieur-pilote-par-raspberry/
published: true
post_modified: 2017-05-13 14:46:15
---
Dans un précédent tutoriel, nous avons découvert comment créer un <a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/">capteur de luminosité avec un ESP8266</a> connecté par Wifi. Ici je vous propose de créer un capteur extérieur piloté par un Raspberry Pi.

Pour être exact, le Raspberry Pi sera installé en intérieur, connecté au réseau filaire Ethernet et relié par un câble à un boitier étanche installé sur le toit qui contiendra une photorésistance et/ou un capteur TSL2561.

Nous allons utiliser un <a href="/constellation-platform/constellation-sentinel/constellation-raspberry-pi/">Raspberry B+ version 1</a> sur lequel <a href="/getting-started/ajouter-des-sentinelles/">nous installerons une sentinelle</a> pour pouvoir déployer notre package depuis Visual Studio. Le package d’acquisition des données sera <a href="/getting-started/creez-votre-premier-package-constellation-en-python/">écrit en Python</a>.
<h3>Prérequis</h3>
<ul>
 	<li>Un serveur Constellation</li>
 	<li>Un <a href="/constellation-platform/constellation-sentinel/constellation-raspberry-pi/">Raspberry Pi </a>sur lequel <a href="/getting-started/ajouter-des-sentinelles/">nous installerons une sentinelle</a></li>
 	<li>Une photorésistance avec un condensateur 1uF et/ou un capteur TSL2561</li>
 	<li>Visual Studio avec le SDK Constellation</li>
</ul>
<h3>Mesurer la luminosité ambiante avec une photorésistance</h3>
<h4>Le montage</h4>
Tout d'abord, il nous faut un boitier étanche avec un couvercle transparent (pour mesurer la luminosité c'est mieux !) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160386-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Boite étanche" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160386_thumb-1.jpg" alt="Boite étanche" width="379" height="243" border="0" /></a></p>
Sur une plaque epoxy, soudons la photorésistance avec un condensateur 1uF.

Une des deux pattes de la photorésistance sera reliée sur la pin "3v3" du Raspberry et la deuxième sera reliée à une entrée du Raspberry, disons la pin n°12 (soit la GPIO #18). Toujours sur cette 2ème patte, on soudera le pole "+" du condensateur. L'autre pole du condensateur, celle marquée "-", devra être reliée à la masse, sur l'une des pins "GND" du Raspberry.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160398-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Photorésistance" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160398_thumb-1.jpg" alt="Photorésistance" width="310" height="298" border="0" /></a></p>
On a donc trois fils connectés entre le Raspberry et notre capteur : le 3V3 et la masse (Gnd) pour l'alimentation et un pour le signal (GPIO #18). L'idée est donc de "charger" le condensateur et de chronométrer le temps qu'il mets à se décharger.

Plus il y a de lumière et moins la résistance est forte, autrement dit le condensateur se déchargera très rapidement. A l'inverse, plus il fait sombre et plus la résistance sera forte. Autrement dit, moins il y a de lumière et moins vite se déchargera le condensateur.

On voit sur les photos ci-dessous, le fil Orange pour le 3V3, le fil Bleu pour la GPIO et le fil Blanc pour la masse (Gnd) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160424-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation dans le boitier" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160424_thumb-1.jpg" alt="Installation dans le boitier" width="347" height="264" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160435-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation dans le boitier" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160435_thumb-1.jpg" alt="Installation dans le boitier" width="351" height="264" border="0" /></a></p>
Il fois les connexions réalisées, on peut refermer proprement notre boitier en vérifiant soigneusement son étanchéité avant de l'installer sur le toit.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160450-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation sur le toit" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160450_thumb-1.jpg" alt="Installation sur le toit" width="354" height="300" border="0" /></a></p>
Côté intérieur, on relie les 3 fils de notre boitier au Raspberry (3v3, Gnd et GPIO18), le câble Ethernet et l'alimentation MicroUSB.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160442-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Raspberry Pi B" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1160442_thumb-1.jpg" alt="Raspberry Pi B" width="354" height="335" border="0" /></a></p>
<p style="text-align: left;" align="center">Pour l'installation du <a href="/constellation-platform/constellation-sentinel/constellation-raspberry-pi/">système sur le Raspberry suivez ce guide</a> puis <a href="/getting-started/ajouter-des-sentinelles/">installez-y une sentinelle</a>. Votre Raspberry est prêt et connecté à Constellation, reste à développer notre package Python.</p>

<h4>Programmation</h4>
Depuis Visual Studio, <a href="/getting-started/creez-votre-premier-package-constellation-en-python/">créons un nouveau projet</a> de type "Constellation Python Package" que nous nommerons “LightSensor”.
<p align="center"><img class="wp-image-1736 size-medium aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-300x208.png" alt="" width="300" height="208" /></p>
Renommez le fichier "Demo.py" en "Light.py" et remplacez l’intégralité du contenu par le code suivant :
<pre class="lang:python decode:true">import Constellation
import RPi.GPIO as GPIO, time, os      

MAX_READING = 30000
LIGHT_SENSOR_GPIO = 18
MEASURE_INTERVAL = 10

def OnExit():
    GPIO.cleanup()

def RCtime (RCpin):
    reading = 0
    GPIO.setup(RCpin, GPIO.OUT)
    GPIO.output(RCpin, GPIO.LOW)
    time.sleep(0.1) 
    GPIO.setup(RCpin, GPIO.IN)
    # This takes about 1 millisecond per loop cycle
    while (GPIO.input(RCpin) == GPIO.LOW):
        if (reading &gt; MAX_READING):
            break
        reading += 1
    return reading
 
def Start():
    global INTERVAL
    GPIO.setmode(GPIO.BCM)
    Constellation.OnExitCallback = OnExit
    lastSend = 0
    currentValue = 0
    count = 0
    Constellation.WriteInfo("LightSensor is ready !")
    while Constellation.IsRunning:
        currentValue = currentValue + RCtime(LIGHT_SENSOR_GPIO)
        count = count + 1
        ts = int(round(time.time()))
        if ts - lastSend &gt;= int(Constellation.GetSetting("Interval")):
            avg = int(round(currentValue / count))
            Constellation.PushStateObject("Light", avg, "int")
            currentValue = 0
            count = 0
            lastSend = ts
        time.sleep(MEASURE_INTERVAL)

Constellation.Start(Start)</pre>
N’hésitez pas à <a href="/getting-started/creez-votre-premier-package-constellation-en-python/">relire le guide</a> sur la création de package Python pour bien comprendre les bases d'un package Python.

Ici on démarre notre package par la méthode "Start" qui démarre une boucle pour appeler la méthode <em>RCtime</em> toutes les 10 secondes (variable MEASURE_INTERVAL).

La méthode <em>RCtime</em> charge le condensateur (mode output) puis passe l'I/O en "input" et incrémente la variable "reading" en continue tant que le condensateur n'est pas déchargé (input == "low"). Plus la valeur "reading" est élevée, plus le condensateur a mis du temps à se décharger et donc moins il y a de lumière. On a un garde-fou à "30.000" pour ne pas attendre indéfiniment dans le cas où il fait trop sombre.

Bien entendu on ne mesure pas une unité précise. Si vous déployez votre code sur un RPi V1 et V2 (plus puissant), vous n'aurez pas la même valeur pour les mêmes conditions.

Chaque mesure incrémente la variable "currentValue", puis dès qu'on atteint l'intervalle configuré dans les Settings Constellation, on fait la moyenne des mesures on <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Publier_des_StateObjects">publie un StateObjec</a>t nommé "Light" de type "int".

Dans le <a href="/concepts/package-manifest/">manifeste du package</a> (<em>PackageInfo.xml</em>), n'oubliez pas de déclarer le setting "Interval" en lui spécifiant une valeur par défaut, ici de 10 seconde :
<pre class="lang:xhtml decode:true">&lt;Settings&gt;
  &lt;Setting name="Interval" isRequired="false" type="Int32" defaultValue="10" description="Interval to read sensors in second" /&gt;
&lt;/Settings&gt;</pre>
De même vous pouvez également modifier les informations de compatibilité des plateformes pour exclure la plateforme Windows dans la mesure où le package exploite les GPIO du Raspberry.
<pre class="lang:xhtml decode:true">&lt;Platform id="Win32NT" isCompliant="false" /&gt;</pre>
Et voilà le package est prêt, vous pouvez depuis Visual Studio le <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Publiez_votre_package">publier</a> dans votre Constellation :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-72.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Publication du projet" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-72.png" alt="Publication du projet" width="454" height="283" border="0" /></a></p>
… puis <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Deployez_votre_package_sur_une_sentinelle">le déployer</a> sur votre sentinelle Raspberry depuis la Console Constellation, en cliquant sur le bouton “Deploy new package” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-73.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Deploiement" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-73.png" alt="Deploiement" width="454" height="291" border="0" /></a></p>
Dans l’assistant de déploiement vous pourrez alors personnaliser le setting “Interval” ou bien laisser la valeur par défaut que nous avons fixé à 10 (secondes) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-74.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Settings" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-74.png" alt="Settings" width="454" height="248" border="0" /></a></p>
<p align="left">Sur la Console Log, le package va être téléchargé et déployé par votre sentinelle Raspberry avant de démarrer :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-75.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Console Log" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-75.png" alt="Console Log" width="454" height="252" border="0" /></a></p>
<p align="left">Et en vous rendant dans le StateObjects Explorer, vous verrez votre package “LightSensor” publier un StateObject “Light” avec un entier comme valeur :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-76.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="StateObjects Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-76.png" alt="StateObjects Explorer" width="454" height="160" border="0" /></a></p>
Votre package Python est opérationnel, on peut dés à présent exploiter cette valeur dans une page Web, un script, un programme .NET ou Python, un Arduino, etc… comme nous le verrons dans la suite de cet article.
<h3>Mesurer des lux avec un capteur TSL2561</h3>
Pour obtenir une mesure plus fiable et surtout exploitant une échelle de mesure universelle, nous allons utiliser un capteur de luminosité TSL2561 capable de mesurer des Lux.
<h4>Le montage</h4>
Dans cette deuxième version, j'ai conservé la photorésistance et simplement ajouté le capteur TSL2561. Celui-ci est également alimenté en 3V3, j'ai donc réutilisé les deux fils (Orange et Blanc) de la photorésistance pour alimenter le capteur également.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1170883-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout d'un capteur TSL2561" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1170883_thumb-1.jpg" alt="Ajout d'un capteur TSL2561" width="354" height="227" border="0" /></a></p>
<p style="text-align: left;" align="center">Le TSL2561 utilise l'I²C, j'ai donc ajouté deux fils entre le boitier étanche et le Raspberry pour connecter le capteur en i²C (SDA et SCL).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1170888-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout d'un capteur TSL2561" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1170888_thumb-1.jpg" alt="Ajout d'un capteur TSL2561" width="354" height="201" border="0" /></a></p>
<p style="text-align: left;" align="center">Sur le Raspberry, le SDA est la GPIO#2 et le SCL la GPIO #3, c'est à dire juste en dessus de la pin 3v3.</p>

<h4>La programmation</h4>
Le plus simple et le plus fiable pour récupérer les données du capteur TSL2561 depuis un Raspberry est d'utiliser un programme natif. Pour cela nous allons utiliser le code publié à cette adresse : <a title="http://dino.ciuffetti.info/2014/03/tsl2561-light-sensor-on-raspberry-pi-in-c/" href="http://dino.ciuffetti.info/2014/03/tsl2561-light-sensor-on-raspberry-pi-in-c/">http://dino.ciuffetti.info/2014/03/tsl2561-light-sensor-on-raspberry-pi-in-c/</a>.

Il suffit récupérer les 3 fichiers C et de les compiler avec GCC pour générer le binaire. Pour vous simplifier la tache, vous pouvez directement récupérer le binaire <a href="https://developer.myconstellation.io/download/resources/tutorials/GetTSL2561">GetTSL2561 ici</a>.

&nbsp;

Ajoutez ensuite un deuxième script nommé "Lux.py" dans le répertoire "Scripts" de votre projet et <strong>n'oubliez pas</strong> d'inclure votre script dans le package en définissant la Build Action à "Content" et en activant la copie du fichier dans le répertoire de sortie :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-77.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Inclure les scripts Python dans le package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-77.png" alt="Inclure les scripts Python dans le package" width="254" height="339" border="0" /></a></p>
Copiez également le binaire <a href="https://developer.myconstellation.io/download/resources/tutorials/GetTSL2561">GetTSL2561</a> dans le répertoire "Scripts" de votre package en l'incluant également dans le package (Build Action à Content et Copy if newer).

Le contenu du script "Lux.py" sera :
<pre class="lang:python decode:true">import Constellation
import os, re, subprocess, time, stat

# Const
EXECUTABLE_FILENAME = "GetTSL2561"

def DoMeasure():
    # Start process
    process = subprocess.Popen("./" + EXECUTABLE_FILENAME, stdout=subprocess.PIPE)
    # Reading  output
    for line in iter(process.stdout.readline, ''):
        # Parse line
        matchObj = re.match('RC: (\d*)\((.*)\), broadband: (\d*), ir: (\d*), lux: (\d*)', line)
        if matchObj:
            # Reading value
            returnCode = int(matchObj.group(1))
            broadband = int(matchObj.group(3))
            ir = int(matchObj.group(4))
            lux = int(matchObj.group(5))
            # Push StateObject
            if returnCode != 0:
                Constellation.WriteWarn("Unknow return code %s : %s" % (returnCode, line))
            else:
                Constellation.PushStateObject("Lux", { "Broadband": broadband, "IR" : ir, "Lux" : lux }, "LightSensor.Lux")
        else:
            Constellation.WriteError("Unable to parse the output: %s" % line)

def Start():	
    # Make the "GetTSL2561" file as executable
    st = os.stat(EXECUTABLE_FILENAME)
    os.chmod(EXECUTABLE_FILENAME, st.st_mode | stat.S_IEXEC)
    Constellation.WriteInfo("LuxSensor is ready !")
    while Constellation.IsRunning:	
        DoMeasure()
        time.sleep(int(Constellation.GetSetting("Interval")))

Constellation.Start(Start)</pre>
Comme pour le script "Light.py", on déclare la méthode "<em>Start</em>" comme méthode de démarrage. Celle-ci boucle tant que le package est démarré à l'intervalle défini par le setting "Interval" qu'on a déclaré à 10 secondes par défaut dans le manifeste.

A chaque itération, on invoque la méthode "<em>DoMeasure</em>" qui démarre le programme "<em>GetTSL2561</em>" et on parse avec une regex le résultat de la sortie du programme (process.stdout) pour extraire le code de retour, le broadband (spectre visible), l'infrarouge et le nombre de lux courant mesuré par le capteur.

Si le code de retour est égal à 0 c'est que la mesure est correcte, on publie alors un StateObject nommé "Lux" contenant les trois propriétés mesurées : Broadband, IR et Lux.
<pre class="lang:python decode:true">Constellation.PushStateObject("Lux", { "Broadband": broadband, "IR" : ir, "Lux" : lux }, "LightSensor.Lux")</pre>
Attention avant de déployer cette nouvelle version<strong> il faut activer l'I²C sur le Raspberry</strong>. Pour cela lancer la commande "<em>sudo raspi-config</em>" puis dans le menu "<em>Advanced Options</em>" activez l'I²C et chargez le module par défaut au démarrage.

On peut maintenant publier cette nouvelle version depuis Visual Studio et lancer un "Reload" sur notre package pour déployer cette nouvelle version sur notre Rapsberry.

Désormais nous avons deux StateObjects publiés par ce package : "<em>Light</em>" via la photorésistance et "<em>Lux</em>" via le TSL2561 :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-78.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="StateObjects Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-78.png" alt="StateObjects Explorer" width="454" height="133" border="0" /></a></p>
Notre nouveau StateObject "Lux" est un objet complexe contenant trois propriétés :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-79.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Detail du StateObject Lux" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-79.png" alt="Detail du StateObject Lux" width="454" height="330" border="0" /></a></p>

<h3>Exploitez les données</h3>
Notre package Python vis sa vie sur notre Raspberry et publie à intervalle régulier deux StateObjects sur base des mesures réalisées par la photoresistance et/ou le capteur de lux TSL2561.

On peut maintenant écrire des pages Web, des scripts, des packages .NET ou Python, Arduino, etc… qui exploiterons ces mesures en temps réel.
<h4>Un dashboard Web</h4>
// page web / dashboard

Depuis <a href="/getting-started/connectez-vos-pages-web-constellation/">une page Web</a>, il suffit d'ajouter un <a href="/client-api/javascript-api/consommer-constellation-angular-js/">StateObjectLink</a>. Vous pouvez vous inspiré <a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/#Etape_3_Une_page_Web_pour_afficher_votre_capteur_en_temps_reel">de ce tutoriel</a> par exemple.
<pre class="lang:js decode:true ">constellation.registerStateObjectLink("*", "LightSensor", "Lux", "*", function (so) {
  $scope.$apply(function () {
    console.log("Lux = ", so.Value.Lux);
  });
});</pre>
Par exemple sur le <a href="https://sebastien.warin.fr/2015/07/15/3033-s-panel-une-interface-domotique-et-iot-multi-plateforme-avec-cordova-angularjs-et-constellation-ou-comment-crer-son-dashboard-domotique-mural/" target="_blank" rel="noopener noreferrer">projet S-Panel</a> avec quelques composants Bootstrap :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-80.png"><img title="S-Panel" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-80.png" alt="S-Panel" width="244" height="131" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-81.png"><img title="S-Panel" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-81.png" alt="S-Panel" width="354" height="210" border="0" /></a></p>

<h4>Un programme .NET</h4>
Vous pouvez là aussi vous inspirer <a href="/tutorials/creer-un-capteur-de-temperature-humidite-et-luminosite-connecte/#Etape_4_optionnelle_creer_un_package_NET_pour_exploiter_les_donnees_du_capteur">de ce tutoriel</a>. En clair, il suffit dans votre code C# d'ajouter des <a href="/client-api/net-package-api/consommer-des-stateobjects/">StateObjectLinks </a>:
<pre class="lang:c# decode:true ">[StateObjectLink("LightSensor", "Lux")]
public StateObjectNotifier Lux { get; set; }</pre>
Ainsi la propriété .NET nommée "Lux" ci-dessus sera liée en temps réel au StateObject "Lux" du package "LigthSensor".

Vous pouvez ajouter des événements dès que la valeur change :
<pre class="lang:c# decode:true ">this.Lux.ValueChanged += (s, e) =&gt;
{
    PackageHost.WriteInfo($"Nouvelle mesure à {this.Lux.Value.LastUpdate} = {this.Lux.DynamicValue.Lux} lux");
};</pre>
<img class="alignnone size-medium wp-image-4596 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-57-300x153.png" alt="" width="300" height="153" />

Vous pouvez ainsi faire ce que bon vous semble avec cette information. Par exemple allumer automatiquement les lumières si la luminosité est trop faible, fermer les volet, enregistrer les données dans un fichier Excel ou une base de données, etc.. etc..
<h4>Historisation avec ElasticSearch / Kibana</h4>
En utilisant le package <a href="/package-library/graylogconnector">Graylog </a>du <a href="/plateforme/package-repository/">catalogue de package</a>, on peut historiser les mesures de notre capteur dans une base ElasticSearch.

Dans la configuration du package Graylog, on aura quelque chose comme :
<pre class="lang:xhtml decode:true">&lt;graylogConfiguration xmlns="urn:GraylogConnector" sendPackageLogs="false" sendPackageStates="true" sendSentinelUpdates="true"&gt;
    &lt;subscriptions&gt;
        &lt;subscription package="LightSensor" name="Lux" /&gt;
        &lt;!-- ..... --&gt;
    &lt;/subscriptions&gt;
    &lt;outputs&gt;
        &lt;gelfOutput name="Graylog server UDP" host="graylog.ajsinfo.loc" port="12201" protocol="Udp" /&gt;
    &lt;/outputs&gt;
&lt;/graylogConfiguration&gt;</pre>
La documentation du package <a href="/package-library/graylogconnector">Graylog</a> est disponible <a href="/package-library/graylogconnector">ici</a>. En résume on crée ici un abonnement au StateObject "Lux" du package "LightSensor" et dans les "outputs" on a déclaré le serveur Graylog.

De ce fait dès que le StateObject "Lux" est mis à jour par notre package Python, le package Graylog enregistre la nouvelle version du StateObject sur le serveur Graylog qui lui même utilise ElasticSearch comme base de stockage.

Ainsi on pourra utiliser un outils comme Kibana pour visualiser l'évolution de notre StateObject :

<img class="aligncenter" title="Kibana" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-83.png" alt="Kibana" width="454" height="304" border="0" />

Un article complet sur le sujet <a href="https://sebastien.warin.fr/2015/10/09/3180-creez-votre-home-analytics-analyse-et-reporting-de-votre-domotique-informatique-et-objets-connectes-avec-elasticsearch-graylog-kibana-constellation/" target="_blank" rel="noopener noreferrer">a été publié ici</a>.
<h4>Stats avec Cacti</h4>
On peut également écrire un script pour récupérer notre StateObject "Lux" depuis l'<a href="/client-api/rest-api/interface-rest-consumer/">interface REST</a> qu'on ajoutera en tant que "<em>Data Input Methods</em>" dans Cacti pour pouvoir créer des graphiques facilement :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-82.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Cacti" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-82.png" alt="Cacti" width="454" height="358" border="0" /></a></p>