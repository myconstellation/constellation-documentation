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
post_modified: 2016-12-16 15:57:31
---
Vous pouvez développer des packages Constellation avec le langage et l’écosystème Python. Très utile pour les packages à destination de vos sentinelles Linux et notamment Raspberry où vous pourrez profiter des différentes libraires pour l’accès aux GPIO et autres ressources de ce SoC.

Découvrons dans cet article comment créer et déployer votre premier package Constellation en Python.

<!--more-->
<h3>Prérequis</h3>
Tout d’abord, que vous soyez soyez sur un Windows ou un Linux vous devez installer le runtime Python et quelques libraires.

Il est d’ailleurs conseillé d’installer cet environnement sur votre poste de développement Windows avec le SDK Constellation.
<h4>Installer Python sur Windows</h4>
Télécharger Python 2.7 pour Windows (<a title="https://www.python.org/downloads/windows/" href="https://www.python.org/downloads/windows/">https://www.python.org/downloads/windows/</a>) puis lancez l’installation en choisissant une installation pour tous les utilisateurs :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-1.png" alt="Installation Python" width="424" height="364" border="0" /></a></p>
<p align="left">Sélectionnez le composant “PIP” ( le gestionnaire de package Python) et ajoutez “Python.exe” dans les “Paths” Windows :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des composants Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-2.png" alt="Installation des composants Python" width="424" height="363" border="0" /></a></p>
<p align="left">Lancez ensuite une invite de commande (cmd.exe) et assurez-vous que la commande “python” fonctionne :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Python.exe fonctionnel" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-3.png" alt="Python.exe fonctionnel" width="424" height="130" border="0" /></a></p>
<p align="left">Maintenant dans une nouvelle invite de commande Windows, installez via PIP les librairies “enum34” et “pyzmq” grâce aux commandes suivantes :</p>

<pre lang="lang:shell">pip install pyzmq
pip install enum34</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des libs pour Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/install-python-tools-windows_thumb.png" alt="Installation des libs pour Python" width="424" height="330" border="0" /></a></p>
<p align="left">Votre environnement est prêt !</p>

<h4>Installer Python sur Linux</h4>
Sur un système Debian ou similaire (Raspbian / Ubuntu), installez <a href="/getting-started/ajouter-des-sentinelles/#Installation_dune_sentinelle_sur_un_systeme_Linux">une sentinelle Constellation</a> puis l’environnement Python avec les outils de dev par la commande :
<pre class="lang:default decode:true ">sudo apt-get install python-dev python-setuptools</pre>
Installez également PIP et les librairies “pyZmq” et “enum34” utilisées par l’API Python de Constellation avec les commandes suivantes :
<pre lang="lang:shell">sudo easy_install pip
sudo pip install pyzmq
sudo pip install enum34</pre>
Votre environnement est prêt !
<h3>Créez le package Python dans Visual Studio</h3>
Lancez Visual Studio et créez un nouveau package Constellation de type “Python” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Création d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb.png" alt="Création d'un package Python" width="424" height="294" border="0" /></a></p>
<p align="left">Une fois créé, le projet Visual Studio à la structure suivante :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Structure d'un package Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-4.png" alt="Structure d'un package Python" width="244" height="199" border="0" /></a></p>
<p align="left">Vous retrouverez la structure classique d’un package Constellation :</p>

<ul>
 	<li>
<div align="left"><u>App.config</u> : le fichier de configuration local pour définir la valeur locale de vos settings (<a href="/client-api/net-package-api/settings/">lire ici</a>)</div></li>
 	<li>
<div align="left"><u>PackageInfo.xml</u> : le <a href="/concepts/package-manifest/">manifeste de votre package</a></div></li>
 	<li>
<div align="left"><u>packages.config</u> : le fichier des package Nuget de votre projet (réservé à Nuget)</div></li>
 	<li>
<div align="left"><u>Program.cs</u> : le point d’entrée (C#) de votre package qui lance le “Python Proxy”.</div></li>
 	<li>
<div align="left">Le répertoire “Scripts” (contient vos scripts Python)</div>
<ul>
 	<li>
<div align="left"><u>Constellation.py</u> : le proxy Python Constellation (la librairie Python) : <u>ne pas modifier</u> !</div></li>
 	<li>
<div align="left"><u>Demo.py</u> : un script Python d’exemple</div></li>
</ul>
</li>
</ul>
<p align="left">Vous pouvez ajouter autant de scripts Python (.py) dans le dossier “Scripts”. Chaque script est démarré dans un processus dédié et est connecté à Constellation.</p>
<p align="left"><u>Attention</u> : si vous rajoutez des scripts Python dans ce dossier, n’oubliez pas de les inclure dans le package en sélectionnant “Copy Always” ou “Copy if newer” pour la propriété “Copy to Output” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Inclure les fichiers Py dans le package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-5.png" alt="Inclure les fichiers Py dans le package" width="244" height="139" border="0" /></a></p>
<p align="left"><u>Note</u> : les fichiers se terminant par “lib.py” ne sont pas démarrés, ce qui vous permet d’ajouter des libraires dans ce répertoire.</p>
<p align="left">Le script “Demo.py” est créé à titre d’exemple. Vous pouvez le supprimer, renommer ou modifier à votre convenance.</p>
<p align="left">Grace à Visual Studio et au SDK Constellation, vous bénéficiez d’un environnement de développement agréable pour créer vos package Python avec coloration syntaxique et IntelliSense :</p>
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
<p align="left">La méthode “Start” démarre la connexion et maintient le script en “vie” grâce à une boucle vite qui tourne tant que le package Constellation est lancé.</p>
<p align="left">Autrement dit, le code qui serait après la méthode “Start” ne sera jamais appelé !</p>
<p align="left">2 – Invoquer la méthode “Start” en passant une fonction de démarrage :</p>
<p align="left">Vous pouvez passer le nom d’une fonction en paramètre de la méthode “Start” qui sera invoquée dès que le script Python est connecté à Constellation. Cela vous permet votre code de démarrage.</p>

<pre lang="lang:python decode:true">def OnStart():
    # Mon code de démarrage ici
    pass

Constellation.Start(OnStart);</pre>
<p align="left">3 – Troisième méthode, invoquez la méthode “StartAsync” qui ne bloque pas le script !</p>
<p align="left">Mais attention, vous devrez créer une boucle pour maintenant “en vie” votre script, autrement il s’arrêtera automatiquement !</p>

<pre lang="lang:python decode:true">Constellation.StartAsync()
while Constellation.IsRunning:
    pass
    time.sleep(1) 
</pre>
<h4 align="left">Produire des logs</h4>
<p align="left">Pour écrire des logs dans le hub Constellation, <a href="/client-api/net-package-api/les-bases-des-packages-net/#Ecrire_des_logs">tout comme en C#,</a> vous disposez des méthodes :</p>

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
<p align="left">Vous remarquez par la même occasion quelques propriétés mis à disposition dans vos scripts Python (IsConnected, IsStandAlone, SentinelName, PackageName, etc…).</p>

<h4 align="left">Accès aux settings</h4>
<p align="left">Le fonctionnement des settings est strictement identique qu’avec l’API .NET <a href="/client-api/net-package-api/settings/">décrite ici</a>.</p>
<p align="left">Les settings sont définis sur le serveur au niveau du package ou via des groupes, et par héritage peuvent être définis dans le fichier local App.config et/ou dans le manifeste.</p>
<p align="left">Avec l’API Python de Constellation vous disposez de la méthode “GetSetting” pour récupérer la valeur de vos settings.</p>
<p align="left">Par exemple pour afficher la valeur du setting “Demo1” :</p>

<pre lang="lang:python decode:true">Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<p align="left">Si la valeur du setting n’existe pas, la méthode vous retourne un “Null”.</p>

<pre lang="lang:python decode:true">if Constellation.GetSetting("Demo1") &lt;&gt; Null:
     Constellation.WriteInfo("Demo1 = " + str(Constellation.GetSetting("Demo1")))</pre>
<h4 align="left">Publier des StateObjects</h4>
<p align="left"><a href="/client-api/net-package-api/stateobjects/">Comme pour l’API .NET</a>, vous disposez de la méthode PushStateObject pour publier un StateObject.</p>
<p align="left">Chaque StateObject doit avoir un nom et une valeur qui peut être de n’importe quel type :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("MyString", "OK")
Constellation.PushStateObject("MyNumber", 123)
Constellation.PushStateObject("MyDecimal", 123.12)
Constellation.PushStateObject("MyBoolean", True)</pre>
<p align="left">Vous pouvez également avoir des objets complexes comme valeur de StateObject :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 })</pre>
<p align="left">Enfin, vous pouvez également spécifier un type, un dictionnaire de méta-données ou encore une durée de vie sur vos StateObjects :</p>

<pre lang="lang:python decode:true">Constellation.PushStateObject("Demo", { "UneString": "DemoPython", "UnNombre": 123 }, type = "MonTypeDemo", metadatas = { "DeviceId": "RPi", "SerialNumber":"123" }, lifetime = 300)</pre>
<h4 align="left">Tester son package dans Visual Studio</h4>
<p align="left">Laissons tel quel le code de démo du script “Demo.py” créé par le Template :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Demo Python" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-8.png" alt="Demo Python" width="420" height="130" border="0" /></a></p>
<p align="left">Notons simplement, qu’au démarre au enregistre la méthode “OnStart” qui réalise un tas de fonction à titre d’exemple dont :</p>

<ul>
 	<li>
<div align="left">Différent “WriteInfo” (Warning &amp; Error) contenant notamment différent propriétés comme le IsConnected &amp; IsStandalone.</div></li>
 	<li>
<div align="left">Publie le StateObject “Demo”</div></li>
</ul>
<h5 align="left">Debug local (hors Constellation)</h5>
<p align="left">Commençons par tester notre package hors Constellaiton, en lançant simplement le package avec le bouton “Start” de Visual Studio (F5) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug local" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-9.png" alt="Debug local" width="424" height="55" border="0" /></a></p>
<p align="left">Votre package démarre et vous pouvez suivre dans la console les différents logs produits par votre package.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-10.png" alt="image" width="420" height="138" border="0" /></a></p>
<p align="left">Vous observerez que votre package n’est pas connecté à Constellation (IsConnected = false).</p>

<h5 align="left">Debug dans Constellation</h5>
<p align="left">Maintenant lançons toujours le debugging de votre package Constellation depuis Visual Studio, mais en connectant votre package à Constellation. (Assurez-vous d’avoir défini dans Visual Studio la Constellation à utiliser pour le debug comme nous l’avons vu dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Tester_son_package_dans_sa_Constellation">ce guide</a>).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-11.png" alt="Debug On Constellation" width="424" height="55" border="0" /></a></p>
<p align="left">Cette fois ci, votre package démarre dans VIsual Studio mais en étant connecté à Constellation (IsConnected = true).</p>
<p align="left">Vous pouvez donc suivre en temps réel les logs depuis la Console Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-12.png" alt="Debug On Constellation" width="424" height="226" border="0" /></a></p>
<p align="left">Vous retrouverez également dans le StateObject Explorer de la Console, le StateObject “Demo” publié par notre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObject Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-13.png" alt="StateObject Demo" width="424" height="302" border="0" /></a></p>

<h4 align="left">Publiez votre package</h4>
<p align="left">Lorsque votre package est testé et validé, publions-le sur votre serveur Constellation.</p>
<p align="left">Pour cela cliquez sur le bouton de publication :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publication du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-14.png" alt="Publication du package" width="424" height="55" border="0" /></a></p>
<p align="left">Sélectionnez l’adresse de la Constellation et cliquez sur “Publish” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-15.png" alt="image" width="424" height="212" border="0" /></a></p>
<p align="left">Une fois publié, vous retrouverez votre package dans le “Package Repository” de la Console.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-16.png" alt="Package Repository" width="424" height="158" border="0" /></a></p>
<p align="left">N’oubliez pas que vous pouvez personnalisé les différentes informations (nom du package, auteur, URL, version, description, etc…) en <a href="/concepts/package-manifest/">éditant le manifeste</a> de votre projet.</p>

<h4 align="left">Déployez votre package sur une sentinelle</h4>
<p align="left">Maintenant que votre package est dans le catalogue de votre Constellation, vous pouvez le déployer sur autant de sentinelle que vous souhaitez!</p>
<p align="left">Pour cela, éditez la configuration (“Configuration Editor”) et ajoutons une instance de package sur la sentinelle de notre choix en ajoutons la ligne :</p>

<pre class="lang:default decode=true">&lt;package name="MonPackagePython" /&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Deploiement du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-17.png" alt="Deploiement du package" width="424" height="195" border="0" /></a></p>
<p align="left">Cliquez sur le bouton “Save &amp; Deploy” et votre package sera automatiquement instancié par la sentinelle ici nommée “PO-SWARIN” !</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Package déployé" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-18.png" alt="Package déployé" width="424" height="182" border="0" /></a></p>
<p align="left">Pour terminer, allons un cran plus loin en définissant le setting “Demo1” utilisé dans le code de notre script.</p>
<p align="left">Editez une dernière fois la configuration pour ajouter le setting “Demo1” sur votre package :</p>

<pre class="lang:default decode=true">&lt;package name="MonPackagePython"&gt;
  &lt;settings&gt;
    &lt;setting key="Demo1" value="Hello Python !!!!" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout des settings" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-19.png" alt="Ajout des settings" width="424" height="244" border="0" /></a></p>
<p align="left">Cliquez ensuite sur “Save &amp; Reload” ou “Save &amp; Deploy” !</p>

<ul>
 	<li>
<div align="left"><u>Save</u> : enregistre la configuration sur le serveur</div></li>
 	<li>
<div align="left"><u>Reload</u> : recharge la configuration sur le serveur</div></li>
 	<li>
<div align="left"><u>Deploy</u> : envoie la configuration sur toutes les sentinelles et les packages de votre Constellation</div></li>
</ul>
<p align="left">Si vous cliquez sur “Save &amp; Deploy”, votre package, alors qu’il est déjà lancé, aura dans ces settings la nouvelle valeur pour le setting “Demo1”.</p>
<p align="left">Seulement dans l’exemple “Demo.py”, le package affiche cette valeur dans la méthode “OnStart” au démarrage. Il faudra donc relancer le package pour voir la valeur de notre setting :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-20.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Redémarrage du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-20.png" alt="Redémarrage du package" width="424" height="193" border="0" /></a></p>
<p align="left">Dans la Console log, vous verrez bien l’ordre de redémarrage ordonné par la Console, la sentinelle arrêter ou relancer notre package, avant de votre le setting “Demo1” associé à la valeur que nous avons configurer dans les settings de votre package :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/04/image-21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Setting à jour !" src="https://developer.myconstellation.io/wp-content/uploads/2016/04/image_thumb-21.png" alt="Setting à jour !" width="424" height="123" border="0" /></a></p>
<p align="left">Et voilà votre premier package Python créé et déployé !</p>

<h4 align="left">Next steps</h4>
<ul>
 	<li>
<div align="left"><a href="/client-api/python-api/messagecallbacks-exposer-des-methodes-python/">MessageCallback : exposer des méthodes Python dans la Constellation</a></div></li>
 	<li>
<div align="left"><a href="/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/">Envoyer des messages et invoquer des MessageCallbacks de la Constellation</a></div></li>
 	<li>
<div align="left"><a href="/client-api/python-api/consommer-des-stateobjects-en-python/">StateObjectLink : consommer des StateObjects de votre Constellation</a></div></li>
</ul>