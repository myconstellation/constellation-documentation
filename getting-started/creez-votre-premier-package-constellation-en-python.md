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
post_modified: 2017-05-03 10:03:07
---
<p>Vous pouvez développer des packages Constellation avec le langage Python. Cela est très utile pour créer des packages à destination de vos sentinelles Linux comme vos Raspberry Pi où vous pourrez profiter des différentes libraires pour l’accès aux GPIO et autres ressources de ce SoC.</p> <p>Découvrons dans cet article comment créer et déployer votre premier package Constellation en Python.</p><!--more--><h3>Prérequis</h3> <p>Tout d’abord, que vous soyez sur Windows ou Linux, il vous faudra le runtime Python et quelques libraires indispensables pour Constellation.</p> <p>Notez que ces prérequis sont automatiquement installés sur Linux avec le <em><a href="/dowload/">Web Plateform Installer</a></em>.</p> <h4>Installer Python sur Windows</h4> <p>Télécharger <a href="https://www.python.org/downloads/windows/">Python 2.7 pour Windows</a> puis lancez l’installation en choisissant une installation pour tous les utilisateurs :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-1.png"><img title="Installation Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Installation Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-1.png" width="424" height="364"></a></p> <p align="left">Sélectionnez le composant “PIP” ( le gestionnaire de package Python) et ajoutez “Python.exe” dans les “Paths” Windows :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-2.png"><img title="Installation des composants Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Installation des composants Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-2.png" width="424" height="363"></a></p> <p align="left">Une fois l’installation terminée, lancez une invite de commande (cmd.exe) et assurez-vous que la commande “python” fonctionne :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-3.png"><img title="Python.exe fonctionnel" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Python.exe fonctionnel" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-3.png" width="424" height="130"></a></p> <p align="left">Maintenant dans une nouvelle invite de commande Windows, installez via PIP les librairies “enum34” et “pyzmq” grâce aux commandes suivantes :</p><pre lang="lang:shell">pip install pyzmq
pip install enum34</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows.png"><img title="Installation des libs pour Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Installation des libs pour Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows_thumb.png" width="424" height="330"></a></p>
<p align="left">Et voilà, votre environnement est prêt !</p>
<h4>Installer Python sur Linux</h4>
<p>Sur un système Debian ou similaire (Raspbian / Ubuntu), vous n’avez qu’à installer (et enregistrer dans votre Constellation) <a href="/getting-started/ajouter-des-sentinelles/#Installation_dune_sentinelle_sur_un_systeme_Linux">une sentinelle Constellation</a>.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-1.png"><img title="Installation des pr&eacute;requis via PIP" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Installation des pr&eacute;requis via PIP" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-1.png" width="350" height="222"></a></p>
<p><br>Le <a href="/dowload/"><em>Web Plateform Installer</em></a> se chargera d’installer tous les prérequis nécessaires à savoir Python et les librairies “pyZmq” et “enum34” via PIP.</p>
<p>Pour installer manuellement ces prérequis, vous pouvez utiliser les mêmes commandes que sur Windows :</p><pre lang="lang:shell">sudo easy_install pip
sudo pip install pyzmq
sudo pip install enum34</pre>
<p>Et voilà, votre environnement est prêt !</p>
<h3>Créez le package Python dans Visual Studio</h3>
<p>Lancez Visual Studio et créez un nouveau package Constellation de type “Python” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image.png"><img title="Cr&eacute;ation d'un package Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Cr&eacute;ation d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb.png" width="424" height="294"></a></p>
<p align="left">Une fois créé le projet Visual Studio à la structure suivante :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-4.png"><img title="Structure d'un package Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Structure d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-4.png" width="244" height="199"></a></p>
<p align="left">Vous retrouverez la structure classique d’un package Constellation :</p>
<ul>
<li>
<div align="left"><u>App.config</u> : le fichier de configuration local pour définir les valeurs locales de vos settings (<a href="/client-api/net-package-api/settings/">lire ici</a>)</div>
<li>
<div align="left"><u>PackageInfo.xml</u> : le <a href="/concepts/package-manifest/">manifeste de votre package</a></div>
<li>
<div align="left"><u>packages.config</u> : le fichier des packages Nuget de votre projet (réservé à Nuget)</div>
<li>
<div align="left"><u>Program.cs</u> : le point d’entrée (C#) de votre package qui se chargera de lancer le “Python Proxy” (le pont entre vos scripts Python et Constellation)</div>
<li>
<div align="left">Le répertoire “Scripts” (qui contient vos scripts Python)</div>
<ul>
<li>
<div align="left"><u>Constellation.py</u> : le proxy Python Constellation (la librairie Python) : <u>ne pas modifier</u> !</div>
<li>
<div align="left"><u>Demo.py</u> : un script Python d’exemple</div></li></ul></li></ul>
<p align="left">Vous pouvez ajouter autant de scripts Python (.py) dans le dossier “Scripts” comme vous le souhaitez. Chaque script est démarré dans un processus dédié et est connecté à Constellation.</p>
<p align="left"><u>Attention</u> : si vous rajoutez des scripts Python dans ce dossier, n’oubliez pas de les inclure dans le package en sélectionnant “Copy Always” ou “Copy if newer” pour la propriété “Copy to Output” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-5.png"><img title="Inclure les fichiers Py dans le package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Inclure les fichiers Py dans le package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-5.png" width="244" height="139"></a></p>
<p align="left"><u>Note</u> : les fichiers se terminant par “lib.py” ne seront pas démarrés ce qui vous permet d’ajouter des libraires dans ce répertoire.</p>
<p align="left">Le script “Demo.py” est créé à titre d’exemple. Vous pouvez le supprimer, renommer ou modifier à votre convenance.</p>
<p align="left">Grâce à Visual Studio et au SDK Constellation, vous bénéficiez d’un environnement de développement agréable pour créer vos packages Python avec la coloration syntaxique et l’IntelliSense :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-6.png"><img title="IntelliSense VisualStudio" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="IntelliSense VisualStudio" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-6.png" width="420" height="276"></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-7.png"><img title="IntelliSense VisualStudio" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="IntelliSense VisualStudio" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-7.png" width="420" height="61"></a></p>
<h4 align="left">Les bases</h4>
<p align="left">Chaque script Python doit impérativement importer la librairie Constellation et démarrer la “connexion” avec le proxy Python en invoquant la méthode “Start” :</p><pre lang="lang:python decode:true">import Constellation
# Votre code !
Constellation.Start()</pre>
<p align="left">Vous avez trois façon de démarrer cette connexion :</p>
<p align="left">1 – La plus simple est d’invoquer la méthode “Start” :</p><pre lang="lang:python decode:true">Constellation.Start()</pre>
<p align="left">La méthode “Start” démarre la connexion et maintient le script en “vie” grâce à une boucle qui tourne tant que le package Constellation est lancé.</p>
<p align="left">Autrement dit le code après la méthode “Start” ne sera jamais appelé !</p>
<p align="left">2 – Invoquer la méthode “Start” en passant une fonction de démarrage :</p>
<p align="left">Vous pouvez passer le nom d’une fonction en paramètre de la méthode “Start” qui sera invoquée dès que le script Python est connecté à Constellation.</p>
<p align="left">Cela vous permet de définir du code au démarrage du package :</p><pre lang="lang:python decode:true">def OnStart():
    # Mon code de démarrage ici
    pass

Constellation.Start(OnStart);</pre>
<p align="left">3 – Troisième méthode : invoquer la méthode “StartAsync” (non bloquante)</p>
<p align="left">Mais attention vous devrez vous même créer une boucle pour maintenant votre script “en vie” autrement il s’arrêtera automatiquement !</p><pre lang="lang:python decode:true">Constellation.StartAsync()
while Constellation.IsRunning:
    pass
    time.sleep(1) 
</pre>
<h4 align="left">Produire des logs</h4>
<p align="left">Pour écrire des logs dans le hub Constellation, <a href="/client-api/net-package-api/les-bases-des-packages-net/#Ecrire_des_logs">tout comme en C#,</a> vous disposez des méthodes suivantes :</p>
<ul>
<li>
<div align="left">WriteInfo</div>
<li>
<div align="left">WriteWarn</div>
<li>
<div align="left">WriteError</div></li></ul>
<p align="left">Exemple :</p><pre lang="lang:python decode:true">Constellation.WriteInfo("Hello world from Python !")
Constellation.WriteWarn("This is a warning !")
Constellation.WriteError("This is an error !")</pre>
<p align="left">En Python vous pouvez formater vos messages avec des variables grâce à l’opérateur “%”.</p>
<p align="left">Par exemple, affichons dans les logs Constellation l’état de notre package :</p><pre lang="lang:python decode:true">Constellation.WriteInfo("Hi I'm '%s' and I run on %s" % (Constellation.PackageName, Constellation.SentinelName))
Constellation.WriteInfo("IsConnected = %s | IsStandAlone = %s " % (Constellation.IsConnected, Constellation.IsStandAlone))</pre>
<p align="left">Vous remarquez par la même occasion <a href="/client-api/net-package-api/les-bases-des-packages-net/#Proprietes_du_PackageHost">quelques propriétés</a> accessibles dans vos scripts Python (<em>IsConnected</em>, <em>IsStandAlone</em>, <em>SentinelName</em>, <em>PackageName</em>, etc…).</p>
<h4 align="left">Accès aux settings</h4>
<p align="left">Le fonctionnement des settings est strictement identique à l’API .NET <a href="/client-api/net-package-api/settings/">décrite ici</a>.</p>
<p align="left">Les settings sont définis sur le serveur au niveau du package ou via des groupes, et par héritage peuvent être définis dans le fichier local App.config et/ou dans le manifeste. Je vous invite vivement <a href="/client-api/net-package-api/settings/">à lire cet article</a> pour bien comprendre le fonctionnement des settings.</p>
<p align="left">Avec l’API Python de Constellation vous disposez de la méthode “GetSetting” pour récupérer la valeur de vos settings.</p>
<p align="left">Par exemple pour afficher la valeur du setting “Demo1” :</p><pre lang="lang:python decode:true">Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<p align="left">Si la valeur du setting n’existe pas, la méthode vous retourne un “Null” :</p><pre lang="lang:python decode:true">if Constellation.GetSetting("Demo1") &lt;&gt; Null:
     Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<h4 align="left">Publier des StateObjects</h4>
<p align="left"><a href="/client-api/net-package-api/stateobjects/">Comme pour l’API .NET</a>, vous disposez de la méthode PushStateObject pour publier un StateObject.</p>
<p align="left">Chaque StateObject a obligatoirement un nom et une valeur de n’importe quel type :</p><pre lang="lang:python decode:true">Constellation.PushStateObject("MyString", "OK")
Constellation.PushStateObject("MyNumber", 123)
Constellation.PushStateObject("MyDecimal", 123.12)
Constellation.PushStateObject("MyBoolean", True)</pre>
<p align="left">Vous pouvez également avoir des objets complexes :</p><pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 })</pre>
<p align="left">Enfin, vous pouvez également spécifier un type, un dictionnaire de méta-données ou encore une durée de vie sur vos StateObjects :</p><pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 }, type = "MonTypeDemo", metadatas = { "DeviceId": "RPi", "SerialNumber":"123" }, lifetime = 300)</pre>
<h4 align="left">Tester son package dans Visual Studio</h4>
<p align="left">Laissons le code de démo du script “Demo.py” créé par le Template du projet tel quel :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-8.png"><img title="Demo Python" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Demo Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-8.png" width="420" height="130"></a></p>
<p align="left">Notons simplement qu’au démarrage du package on enregistre la méthode “OnStart” pour :</p>
<ul>
<li>
<div align="left">Produire différents logs via des “WriteInfo” (Warning &amp; Error) contenant notamment différentes propriétés comme le <em>IsConnected</em> &amp; <em>IsStandalone</em>.</div>
<li>
<div align="left">Publier le StateObject “Demo”</div></li></ul>
<h5 align="left">Debug local (hors Constellation)</h5>
<p align="left">Commençons par tester notre package hors Constellation en lançant simplement le package avec le bouton “Start” de Visual Studio (F5) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-9.png"><img title="Debug local" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Debug local" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-9.png" width="424" height="55"></a></p>
<p align="left">Votre package démarre et vous pouvez suivre dans la console les différents logs produits par votre package.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-10.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-10.png" width="420" height="138"></a></p>
<p align="left">Vous observerez que votre package n’est pas connecté à Constellation (<em>IsConnected = false</em>).</p>
<h5 align="left">Debug dans Constellation</h5>
<p align="left">Maintenant lançons toujours le debugging de votre package Constellation depuis Visual Studio mais en connectant votre package à Constellation. (Assurez-vous d’avoir défini dans Visual Studio la Constellation à utiliser pour le debug comme nous l’avons vu dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Tester_son_package_dans_sa_Constellation">ce guide</a>).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-11.png"><img title="Debug On Constellation" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-11.png" width="424" height="55"></a></p>
<p align="left">Cette fois ci, votre package démarre dans Visual Studio mais en étant connecté à Constellation (<em>IsConnected = true</em>).</p>
<p align="left">Vous pouvez donc suivre en temps réel les logs depuis la Console Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-12.png"><img title="Debug On Constellation" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-12.png" width="424" height="226"></a></p>
<p align="left">Vous retrouverez également dans le StateObject Explorer de la Console, le StateObject “Demo” publié par notre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-13.png"><img title="StateObject Demo" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="StateObject Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-13.png" width="424" height="302"></a></p>
<h4 align="left">Publiez votre package</h4>
<p align="left">Lorsque votre package est testé et validé, nous pouvons le publier dans Constellation.</p>
<p align="left">Pour cela cliquez sur le bouton de publication :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-14.png"><img title="Publication du package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Publication du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-14.png" width="424" height="55"></a></p>
<p align="left">Sélectionnez votre serveur Constellation cible et cliquez sur “Publish” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-15.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-15.png" width="424" height="212"></a></p>
<p align="left">Une fois publié, vous retrouverez votre package dans le “Package Repository” de la Console.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-16.png"><img title="Package Repository" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-16.png" width="424" height="158"></a></p>
<p align="left">N’oubliez pas que vous pouvez personnaliser les différentes informations (nom du package, auteur, URL, version, description, etc…) en <a href="/concepts/package-manifest/">éditant le manifeste</a> de votre package.</p>
<h4 align="left">Déployez votre package sur une sentinelle</h4>
<p align="left">Maintenant que votre package est dans le catalogue de votre Constellation, vous pouvez le déployer sur autant de sentinelle que vous le souhaitez!</p>
<p align="left">Pour cela, vous pouvez éditer manuellement la configuration (via le <em><a href="/constellation-platform/constellation-console/configuration-editor/">Configuration Editor</a></em>) pour ajouter une instance de package sur la sentinelle de votre choix par la ligne :</p><pre class="lang:default decode=true">&lt;package name="MonPackagePython" /&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-17.png"><img title="Deploiement du package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Deploiement du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-17.png" width="424" height="195"></a></p>
<p align="left">On peut également déclarer le setting “Demo1” pour cette instance de package :</p><pre class="lang:default decode=true">&lt;package name="MonPackagePython"&gt;
  &lt;settings&gt;
    &lt;setting key="Demo1" value="Hello Python !!!!" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<p align="left">Cliquez sur le bouton “Save &amp; Deploy” et votre package sera automatiquement démarré sur la sentinelle ici nommée “PO-SWARIN” !</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-18.png"><img title="Package d&eacute;ploy&eacute;" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Package d&eacute;ploy&eacute;" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-18.png" width="424" height="182"></a></p>
<p align="left">Autre moyen, plus intuitif et rapide pour déployer un package, cliquez sur le bouton “Deploy” dans menu contextuel de votre package Python :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-6.png"><img title="D&eacute;ploiement d'un package" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="D&eacute;ploiement d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-6.png" width="428" height="149"></a></p>
<p align="left">Vous pouvez également cliquer sur le bouton “<a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Deployer_un_package">Deploy new package</a>” sur la page “Packages”.</p>
<p align="left">Un assistant vous proposera de sélectionner la sentinelle sur laquelle déployer votre package Python :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-7.png"><img title="D&eacute;ploiement d'un package" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="D&eacute;ploiement d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-7.png" width="428" height="158"></a></p>
<p align="left">Si les settings de votre package sont déclarés dans le manifeste (<a href="/concepts/package-manifest/#Informations_sur_les_Settings_du_package">voir ici</a>), la Console vous affichera une fenêtre de paramétrage :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-8.png"><img title="Configuration des settings" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="Configuration des settings" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-8.png" width="428" height="246"></a></p>
<p align="left">Une fois votre package déployé, vous pourrez suivre dans la Console Log votre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-9.png"><img title="D&eacute;marrage du package" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="D&eacute;marrage du package" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-9.png" width="428" height="163"></a></p>
<p align="left">Et voilà votre premier package Python créé et déployé dans votre Constellation !</p>
<h4 align="left">Next steps</h4>
<ul>
<li>
<div align="left"><a href="/client-api/python-api/messagecallbacks-exposer-des-methodes-python/">MessageCallback : exposer des méthodes Python dans la Constellation</a></div>
<li>
<div align="left"><a href="/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/">Envoyer des messages et invoquer des MessageCallbacks de la Constellation</a></div>
<li>
<div align="left"><a href="/client-api/python-api/consommer-des-stateobjects-en-python/">StateObjectLink : consommer des StateObjects de votre Constellation</a></div></li></ul>