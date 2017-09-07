---
ID: 1737
post_title: 'Cr&eacute;ez votre premier package Constellation en Python'
author: Sebastien Warin
post_date: 2016-04-01 09:28:19
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-python/
published: true
post_modified: 2017-09-07 23:58:44
---
Vous pouvez développer des packages Constellation avec le langage Python. Cela est très utile pour créer des packages à destination de vos sentinelles Linux comme vos Raspberry Pi où vous pourrez profiter des différentes libraires pour l’accès aux GPIO et autres ressources de ce SoC.

Découvrons dans cet article comment créer et déployer votre premier package Constellation en Python.

<!--more-->
<h3>Prérequis</h3>
Tout d’abord, que vous soyez sur Windows ou Linux, il vous faudra le runtime Python et quelques libraires indispensables pour Constellation.

Notez que ces prérequis sont automatiquement installés sur Linux avec le <em><a href="/dowload/">Web Platform Installer</a></em>.

<span style="text-decoration: underline;">Important</span> : actuellement l'API Constellation pour Python ne supporte que la version 2.x de Python (autrement dit Python 3.x n'est pas encore supporté).
<h4>Installer Python sur Windows</h4>
Télécharger <a href="https://www.python.org/downloads/windows/">Python 2.7 pour Windows</a> puis lancez l’installation en choisissant une installation pour tous les utilisateurs :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-1.png" alt="Installation Python" width="424" height="364" border="0" /></a></p>
<p align="left">Sélectionnez le composant “PIP” ( le gestionnaire de package Python) et ajoutez “Python.exe” dans les “Paths” Windows :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des composants Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-2.png" alt="Installation des composants Python" width="424" height="363" border="0" /></a></p>
<p align="left">Une fois l’installation terminée, lancez une invite de commande (cmd.exe) et assurez-vous que la commande “python” fonctionne :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Python.exe fonctionnel" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-3.png" alt="Python.exe fonctionnel" width="424" height="130" border="0" /></a></p>
<p align="left">Maintenant dans une nouvelle invite de commande Windows, installez via PIP les librairies “enum34” et “pyzmq” grâce aux commandes suivantes :</p>

<pre lang="lang:shell">pip install pyzmq
pip install enum34</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des libs pour Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows_thumb.png" alt="Installation des libs pour Python" width="424" height="330" border="0" /></a></p>
<p align="left">Et voilà, votre environnement Windows est prêt !</p>

<h4>Installer Python sur Linux</h4>
Sur un système Debian ou similaire (Raspbian / Ubuntu), vous n’avez qu’à installer (et enregistrer dans votre Constellation) <a href="/getting-started/ajouter-des-sentinelles/#Installation_dune_sentinelle_sur_un_systeme_Linux">une sentinelle Constellation</a>.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des prérequis via PIP" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-1.png" alt="Installation des prérequis via PIP" width="350" height="222" border="0" /></a></p>
Le <a href="/dowload/"><em>Web Platform Installer</em></a> se chargera d’installer tous les prérequis nécessaires à savoir Python et les librairies “pyZmq” et “enum34” via PIP.

Autrement, pour installer manuellement ces prérequis, vous pouvez utiliser les mêmes commandes que sur Windows :
<pre lang="lang:shell">sudo easy_install pip
sudo pip install pyzmq
sudo pip install enum34</pre>
Si vous utilisez un Raspberry Pi, vous pouvez également <a href="/constellation-platform/constellation-server/constellation-raspberry-pi/">consulter cet article en particulier</a>.

Et voilà, votre environnement Linux est prêt !
<h3>Développer un package Python en ligne de commande</h3>
Vous pouvez soit utiliser le SDK Constellation basé sur Visual Studio pour créer, tester et déployer des packages Constellation (.NET ou Python) ou bien, utiliser l'outil en ligne de commande nommé <a href="/client-api/python-api/developper-avec-le-package-tools-cli/"><strong><em>"Constellation Package Tools CLI"</em></strong></a>.

Le développement sous Visual Studio est abordé ci-dessous. Pour l'outil en ligne de commande, rendez-vous sur la page : <a href="/client-api/python-api/developper-avec-le-package-tools-cli/">Créer, tester et déployer des packages Python en ligne de commande</a>
<h3>Développer un package Python avec Visual Studio</h3>
Après avoir installé les prérequis et le <a href="/getting-started/installer-constellation/#Etape_5_selectionnez_les_composants_Constellation_a_installer">SDK Constellation</a>, lancez Visual Studio et créez un nouveau package Constellation de type “Python” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Création d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb.png" alt="Création d'un package Python" width="424" height="294" border="0" /></a></p>
<p align="left">Une fois créé le projet Visual Studio a la structure suivante :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Structure d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-4.png" alt="Structure d'un package Python" width="244" height="199" border="0" /></a></p>
<p align="left">Vous retrouverez une structure classique pour un package Constellation :</p>

<ul>
 	<li>
<div align="left"><u>App.config</u> : le fichier de configuration local pour définir les valeurs locales de vos settings (<a href="/client-api/net-package-api/settings/">lire ici</a>) ainsi que<span style="text-decoration: underline;"> les scripts Python à démarrer</span></div></li>
 	<li>
<div align="left"><u>PackageInfo.xml</u> : le <a href="/concepts/package-manifest/">manifeste de votre package</a></div></li>
 	<li>
<div align="left"><u>packages.config</u> : le fichier des packages Nuget de votre projet (réservé à Nuget)</div></li>
 	<li>
<div align="left"><u>Program.cs</u> : le point d’entrée (C#) de votre package qui se chargera de lancer le “Python Proxy” (le pont entre vos scripts Python et Constellation)</div></li>
 	<li>
<div align="left">Le répertoire “Scripts” (qui contient vos scripts Python)</div>
<ul>
 	<li>
<div align="left"><u>Constellation.py</u> : le proxy Python Constellation (la librairie Python) : <u>ne pas modifier</u> !</div></li>
 	<li>
<div align="left"><u>Demo.py</u> : un script Python d’exemple</div></li>
</ul>
</li>
</ul>
<h4 align="left">Configurer les scripts à démarrer</h4>
<p align="left">Vous pouvez ajouter autant de scripts Python dans le dossier “Scripts” comme vous le souhaitez. Pour cela, cliquez-droit sur ce répertoire et sélectionnez "<em>Ajouter un nouvel élément</em>".</p>
<p align="left">Dans la catégorie "Constellation" sélectionnez "<strong><em>Constellation Python Script</em></strong>" :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/06/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Création d'un nouveau script Constellation Python" src="https://developer.myconstellation.io/wp-content/uploads/2017/06/image_thumb-1.png" alt="Création d'un nouveau script Constellation Python" width="354" height="246" border="0" /></a></p>
<p align="left">Chaque script sera démarré dans un processus dédié et connecté à Constellation.</p>
<p align="left"><u>Attention</u> : si vous rajoutez des scripts Python dans ce dossier, il faut obligatoirement les inclure dans le package en sélectionnant “<em>Copy if newer</em>” pour la propriété “<em>Copy to Output directory</em>”. Cette propriété est automatiquement définie à cette valeur si vous avez créé votre fichier Python en sélectionnant l’élément "<em>Constellation Python Script</em>" comme expliqué ci-dessus.</p>
<p align="left"><strong>Dans le fichier "<em>App.config</em>" vous devez déclarer les fichiers Python à démarrer</strong>. Les scripts Python créé en sélectionnant l’élément "Constellation Python Script" sont automatiquement ajoutés dans ce fichier lors de la création. <strong>Si vous renommez ou supprimer vos scripts, n'oubliez pas de mettre à jour ce fichier</strong>.</p>

<pre class="lang:xhtml decode:true">&lt;pythonProxy xmlns="urn:Constellation.PythonProxy"&gt;
  &lt;scripts&gt;
    &lt;script filename="Scripts\Demo.py" /&gt;
    &lt;script filename="Scripts\LightSensor.py" /&gt;
    &lt;script filename="Scripts\Camera.py" /&gt;
  &lt;/scripts&gt;
&lt;/pythonProxy&gt;
</pre>
<p align="left">Le script “Demo.py” est créé à titre d’exemple. Vous pouvez le supprimer, renommer ou modifier à votre convenance.</p>
<p align="left">Grâce à Visual Studio et au SDK Constellation, vous bénéficiez d’un environnement de développement agréable pour créer vos packages Python avec la coloration syntaxique et l’IntelliSense :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="IntelliSense VisualStudio" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-6.png" alt="IntelliSense VisualStudio" width="420" height="276" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="IntelliSense VisualStudio" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-7.png" alt="IntelliSense VisualStudio" width="420" height="61" border="0" /></a></p>

<h4 align="left">Les bases</h4>
<p align="left">Chaque script Python doit impérativement importer la librairie Constellation et démarrer la “connexion” avec le proxy Python en invoquant la méthode “Start” :</p>

<pre lang="lang:python decode:true">import Constellation
# Votre code !
Constellation.Start()</pre>
<p align="left">Vous avez trois façon de démarrer cette connexion :</p>
<p align="left">1 – La plus simple est d’invoquer la méthode “Start” :</p>

<pre lang="lang:python decode:true">Constellation.Start()</pre>
<p align="left">La méthode “Start” démarre la connexion et maintient le script en “vie” grâce à une boucle qui tourne tant que le package Constellation est lancé.</p>
<p align="left">Autrement dit le code après la méthode “Start” ne sera jamais appelé !</p>
<p align="left">2 – Invoquer la méthode “Start” en passant une fonction de démarrage :</p>
<p align="left">Vous pouvez passer le nom d’une fonction en paramètre de la méthode “Start” qui sera invoquée dès que le script Python est connecté à Constellation.</p>
<p align="left">Cela vous permet de définir du code au démarrage du package :</p>

<pre lang="lang:python decode:true">def OnStart():
    # Mon code de démarrage ici
    pass

Constellation.Start(OnStart);</pre>
<p align="left">3 – Troisième méthode : invoquer la méthode “StartAsync” (non bloquante)</p>
<p align="left">Mais attention vous devrez vous même créer une boucle pour maintenant votre script “en vie” autrement il s’arrêtera automatiquement !</p>

<pre lang="lang:python decode:true">Constellation.StartAsync()
while Constellation.IsRunning:
    pass
    time.sleep(1) 
</pre>
<h4 align="left">Produire des logs</h4>
<p align="left">Pour écrire des logs dans le hub Constellation, <a href="/client-api/net-package-api/les-bases-des-packages-net/#Ecrire_des_logs">tout comme en C#,</a> vous disposez des méthodes suivantes :</p>

<ul>
 	<li>
<div align="left">WriteInfo</div></li>
 	<li>
<div align="left">WriteWarn</div></li>
 	<li>
<div align="left">WriteError</div></li>
</ul>
<p align="left">Exemple :</p>

<pre lang="lang:python decode:true">Constellation.WriteInfo("Hello world from Python !")
Constellation.WriteWarn("This is a warning !")
Constellation.WriteError("This is an error !")</pre>
<p align="left">En Python vous pouvez formater vos messages avec des variables grâce à l’opérateur “%”.</p>
<p align="left">Par exemple, affichons dans les logs Constellation l’état de notre package :</p>

<pre lang="lang:python decode:true">Constellation.WriteInfo("Hi I'm '%s' and I run on %s" % (Constellation.PackageName, Constellation.SentinelName))
Constellation.WriteInfo("IsConnected = %s | IsStandAlone = %s " % (Constellation.IsConnected, Constellation.IsStandAlone))</pre>
<p align="left">Vous remarquez par la même occasion <a href="/client-api/net-package-api/les-bases-des-packages-net/#Proprietes_du_PackageHost">quelques propriétés</a> accessibles dans vos scripts Python (<em>IsConnected</em>, <em>IsStandAlone</em>, <em>SentinelName</em>, <em>PackageName</em>, etc…).</p>

<h4 align="left">Accès aux settings</h4>
<p align="left">Le fonctionnement des settings est strictement identique à l’API .NET <a href="/client-api/net-package-api/settings/">décrite ici</a>.</p>
<p align="left">Les settings sont définis sur le serveur au niveau du package ou via des groupes, et par héritage peuvent être définis dans le fichier local App.config et/ou dans le manifeste. Je vous invite vivement <a href="/client-api/net-package-api/settings/">à lire cet article</a> pour bien comprendre le fonctionnement des settings.</p>
<p align="left">Avec l’API Python de Constellation vous disposez de la méthode “GetSetting” pour récupérer la valeur de vos settings.</p>
<p align="left">Par exemple pour afficher la valeur du setting “Demo1” :</p>

<pre lang="lang:python decode:true">Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<p align="left">Si la valeur du setting n’existe pas, la méthode vous retourne un “Null” :</p>

<pre lang="lang:python decode:true">if Constellation.GetSetting("Demo1") &lt;&gt; Null:
     Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<h4 align="left">Publier des StateObjects</h4>
<p align="left"><a href="/client-api/net-package-api/stateobjects/">Comme pour l’API .NET</a>, vous disposez de la méthode PushStateObject pour publier un StateObject.</p>
<p align="left">Chaque StateObject a obligatoirement un nom et une valeur de n’importe quel type :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("MyString", "OK")
Constellation.PushStateObject("MyNumber", 123)
Constellation.PushStateObject("MyDecimal", 123.12)
Constellation.PushStateObject("MyBoolean", True)</pre>
<p align="left">Vous pouvez également avoir des objets complexes :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 })</pre>
<p align="left">Enfin, vous pouvez également spécifier un type, un dictionnaire de méta-données ou encore une durée de vie sur vos StateObjects :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 }, type = "MonTypeDemo", metadatas = { "DeviceId": "RPi", "SerialNumber":"123" }, lifetime = 300)</pre>
<h4 align="left">Tester son package dans Visual Studio</h4>
<p align="left">Laissons le code de démo du script “Demo.py” créé par le Template du projet tel quel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Demo Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-8.png" alt="Demo Python" width="420" height="130" border="0" /></a></p>
<p align="left">Notons simplement qu’au démarrage du package on enregistre la méthode “OnStart” pour :</p>

<ul>
 	<li>
<div align="left">Produire différents logs via des “WriteInfo” (Warning &amp; Error) contenant notamment différentes propriétés comme le <em>IsConnected</em> &amp; <em>IsStandalone</em>.</div></li>
 	<li>
<div align="left">Publier le StateObject “Demo”</div></li>
</ul>
<h5 align="left">Debug local (hors Constellation)</h5>
<p align="left">Commençons par tester notre package hors Constellation en lançant simplement le package avec le bouton “Start” de Visual Studio (F5) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug local" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-9.png" alt="Debug local" width="424" height="55" border="0" /></a></p>
<p align="left">Votre package démarre et vous pouvez suivre dans la console les différents logs produits par votre package.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-10.png" alt="image" width="420" height="138" border="0" /></a></p>
<p align="left">Vous observerez que votre package n’est pas connecté à Constellation (<em>IsConnected = false</em>).</p>

<h5 align="left">Debug dans Constellation</h5>
<p align="left">Maintenant lançons toujours le debugging de votre package Constellation depuis Visual Studio mais en connectant votre package à Constellation. (Assurez-vous d’avoir défini dans Visual Studio la Constellation à utiliser pour le debug comme nous l’avons vu dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Tester_son_package_dans_sa_Constellation">ce guide</a>).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-11.png" alt="Debug On Constellation" width="424" height="55" border="0" /></a></p>
<p align="left">Cette fois ci, votre package démarre dans Visual Studio mais en étant connecté à Constellation (<em>IsConnected = true</em>).</p>
<p align="left">Vous pouvez donc suivre en temps réel les logs depuis la Console Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-12.png" alt="Debug On Constellation" width="424" height="226" border="0" /></a></p>
<p align="left">Vous retrouverez également dans le StateObject Explorer de la Console, le StateObject “Demo” publié par notre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-13.png" alt="StateObject Demo" width="424" height="302" border="0" /></a></p>

<h4 align="left">Publiez votre package</h4>
<p align="left">Lorsque votre package est testé et validé, nous pouvons le publier dans Constellation.</p>
<p align="left">Pour cela cliquez sur le bouton de publication :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publication du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-14.png" alt="Publication du package" width="424" height="55" border="0" /></a></p>
<p align="left">Sélectionnez votre serveur Constellation cible et cliquez sur “Publish” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-15.png" alt="image" width="424" height="212" border="0" /></a></p>
<p align="left">Une fois publié, vous retrouverez votre package dans le “Package Repository” de la Console.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-16.png" alt="Package Repository" width="424" height="158" border="0" /></a></p>
<p align="left">N’oubliez pas que vous pouvez personnaliser les différentes informations (nom du package, auteur, URL, version, description, etc…) en <a href="/concepts/package-manifest/">éditant le manifeste</a> de votre package.</p>

<h4 align="left">Déployez votre package sur une sentinelle</h4>
<p align="left">Maintenant que votre package est dans le catalogue de votre Constellation, vous pouvez le déployer sur autant de sentinelle que vous le souhaitez!</p>

<h5 align="left">Par l'édition du fichier de configuration</h5>
<p align="left">Pour cela, vous pouvez éditer manuellement la configuration (via le <em><a href="/constellation-platform/constellation-console/configuration-editor/">Configuration Editor</a></em>) pour ajouter une instance de package sur la sentinelle de votre choix par la ligne :</p>

<pre class="lang:default decode=true">&lt;package name="MonPackagePython" /&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Deploiement du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-17.png" alt="Deploiement du package" width="424" height="195" border="0" /></a></p>
<p align="left">On peut également déclarer le setting “Demo1” pour cette instance de package :</p>

<pre class="lang:default decode=true">&lt;package name="MonPackagePython"&gt;
  &lt;settings&gt;
    &lt;setting key="Demo1" value="Hello Python !!!!" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<p align="left">Cliquez sur le bouton “Save &amp; Deploy” et votre package sera automatiquement démarré sur la sentinelle ici nommée “PO-SWARIN” !</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package déployé" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-18.png" alt="Package déployé" width="424" height="182" border="0" /></a></p>

<h5 align="left">Par l'interface graphique de la Console Constellation</h5>
<p align="left">Autre moyen, plus intuitif et rapide pour déployer un package, cliquez sur le bouton “Deploy” dans menu contextuel de votre package Python :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Déploiement d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-6.png" alt="Déploiement d'un package" width="428" height="149" border="0" /></a></p>
<p align="left">Vous pouvez également cliquer sur le bouton “<a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Deployer_un_package">Deploy new package</a>” sur la page “Packages”.</p>
<p align="left">Un assistant vous proposera de sélectionner la sentinelle sur laquelle déployer votre package Python :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Déploiement d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-7.png" alt="Déploiement d'un package" width="428" height="158" border="0" /></a></p>
<p align="left">Si les settings de votre package sont déclarés dans le manifeste (<a href="/concepts/package-manifest/#Informations_sur_les_Settings_du_package">voir ici</a>), la Console vous affichera une fenêtre de paramétrage :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Configuration des settings" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-8.png" alt="Configuration des settings" width="428" height="246" border="0" /></a></p>
<p align="left">Une fois votre package déployé, vous pourrez suivre dans la Console Log votre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Démarrage du package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-9.png" alt="Démarrage du package" width="428" height="163" border="0" /></a></p>
<p align="left">Et voilà votre premier package Python créé et déployé dans votre Constellation !</p>

<h4 align="left">Next steps</h4>
<ul>
 	<li>
<div align="left"><a href="/client-api/python-api/messagecallbacks-exposer-des-methodes-python/">MessageCallback : exposer des méthodes Python dans la Constellation</a></div></li>
 	<li>
<div align="left"><a href="/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/">Envoyer des messages et invoquer des MessageCallbacks de la Constellation</a></div></li>
 	<li>
<div align="left"><a href="/client-api/python-api/consommer-des-stateobjects-en-python/">StateObjectLink : consommer des StateObjects de votre Constellation</a></div></li>
</ul>