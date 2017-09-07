---
ID: 5341
post_title: 'Cr&eacute;er, tester et d&eacute;ployer des packages Python en ligne de commande'
author: Sebastien Warin
post_date: 2017-09-07 23:50:15
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/python-api/developper-avec-le-package-tools-cli/
published: true
post_modified: 2017-09-07 23:50:15
---
Vous pouvez <a href="https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-python/">créer des packages Constellation Python avec Visual Studio</a> ou alors sans IDE avec l'outil en ligne de commande nommé <em><strong>"Constellation Package Tools CLI".</strong></em>

Nous allons dans cet article créer, tester et déployer sur votre Constellation un package Python avec cet outil.
<h2>Prérequis</h2>
Le "Constellation Package Tools CLI" est un outil en ligne de commande compatible Windows et Linux. Il est écrit en Python et s'installe via PIP avec la commande :
<pre class="lang:default decode:true">pip install constellation-pkgtools-cli</pre>
Sur Windows vous devriez au préalable avoir installé Python 2.7 ainsi que l'outil "pip" et les libraires "pyzmq" et "enum34". La procédure complète d'installation est <a href="https://developer.myconstellation.io/getting-started/creez-votre-premier-package-constellation-en-python/#Installer_Python_sur_Windows">décrite sur cette page</a>.

Via une invite de commande, tapez la commande ci-dessus :
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation sur Windows" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb.png" alt="Installation sur Windows" width="354" height="246" border="0" /></a></p>
Sur Linux, vous devez également avoir installé Python 2.7 (généralement installé par défaut),  l'outil "pip" et les libraires "pyzmq" et "enum34" sans oublier le moteur Mono. Pour cela, utilisez le WPI (<em>Web Platform Installer</em>) pour <a href="/getting-started/ajouter-des-sentinelles/#Installation_dune_sentinelle_sur_un_systeme_Linux">installer une sentinelle Constellation</a>. Celui-ci se chargera de déployer l'ensemble des prérequis nécessaire.
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation sur Linux" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-1.png" alt="Installation sur Linux" width="354" height="226" border="0" /></a></p>

<h2>La commande "ctln"</h2>
Une fois le package Python "<em>constellation-pkgtools-cli</em>" installé via PIP, vous aurez accès à une commande nommée "ctln" (abréviation de <strong>C</strong>ons<strong>t</strong>e<strong>l</strong>latio<strong>n</strong>)
<p style="text-align: center;"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Commande &quot;ctln&quot;" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-3.png" alt="Commande &quot;ctln&quot;" width="454" height="229" border="0" /></a></p>
Cette commande vous permettra de créer de nouveau package, les tester et déployer en local ou directement sur un serveur Constellation.
<h2>Créer son premier package Python</h2>
Pour créer un nouveau package utilisez simplement la commande “create” en spécifiant le nom de votre package. Par exemple :
<pre class="lang:default decode:true">ctln create &lt;mon projet&gt;</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Création d'un nouveau package" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-4.png" alt="Création d'un nouveau package" width="354" height="234" border="0" /></a></p>
<p align="left">Et voilà, votre package “MyFirstPackage” est créé dans le répertoire du même nom !</p>

<h3 align="left">Template de package</h3>
Chaque package est créé à partir d’un template. Vous pouvez lister les templates avec la commande :
<pre class="lang:default decode:true">ctln template list</pre>
A l’heure où est écrit cet article il y a deux templates de type “projet” :
<ul>
 	<li>Python Base Template : le template de base (créé par défaut)</li>
 	<li>Python Demo template : package Python avec du code de “demo”</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Listing des templates de package" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-5.png" alt="Listing des templates de package" width="354" height="234" border="0" /></a></p>
<p align="left">Lorsque vous créez un package avec la commande “ctln create”, le template “python-base” est sélectionné par défaut.</p>
<p align="left">Vous pouvez cependant préciser l’Id du template à utiliser dans la commande “ctln create”. Par exemple pour créer un package sur base du template de demo :</p>

<pre class="lang:default decode:true">ctln create &lt;mon projet&gt; python-demo</pre>
<p align="left">Il y a aussi des templates de type “item”, c’est à dire des templates pour les scripts que vous allez rajouter par la suite à votre package.</p>

<h3 align="left">Mise à jour du template</h3>
<p align="left">Comme vous le constatez dans le listing des templates ci-dessus, chaque template comporte un numéro de version car ils sont amenés à évoluer notamment en cas de mise à jour des librairies Constellation.</p>
<p align="left">Avec le SDK Constellation Visual Studio, les nouvelles versions des librairies sont diffusées avec NuGet. Ici avec cet outil en ligne de commande, les nouvelles librairies sont embarquées dans des mises à jour des templates.</p>
<p align="left">Ainsi pour mettre à jour vos packages, vous pouvez tout simplement mettre à jour le template de votre package par la commande :</p>

<pre class="lang:default decode:true">ctln update</pre>
<p align="left">Cette commande doit être lancé depuis le répertorie racine de votre package.</p>
<p align="left">Notez également que la mise à jour est “intelligente” dans la mesure où elle n’écrase pas vos fichiers de configuration, le manifeste du package ou encore vos scripts Python.</p>

<h2>Développer son package</h2>
Le développement de package Python est <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Creez_le_package_Python_dans_Visual_Studio">expliqué dans cet article</a>. Bien que l'article se base sur Visual Studio, l'API Python est strictement identique.

Ainsi, pour rappel, vous avez à la racine de votre projet le fichier "<a href="/concepts/package-manifest/">PackageInfo.xml</a>" qui contient l'ensemble des informations de votre package (<a href="/concepts/package-manifest/">plus de détail ici</a>) mais aussi le fichier de configuration nommé "PythonPackageHost.exe.config" qui contient la liste des scripts Python à lancer.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Structure du package" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-7.png" alt="Structure du package" width="354" height="234" border="0" /></a></p>
Les <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Les_bases">bases </a>du développement d'un script Constellation Python <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Les_bases">sont expliquées ici</a>.

Pour faciliter le développement, vous n'êtes pas obligé de modifier manuellement le fichier de configuration pour gérer les scripts Python. Pour cela vous avez à disposition différentes commandes pour lister, ajouter, renommer ou supprimer des scripts.
<h3>Lister les scripts du package</h3>
Les scripts Constellation Python lancés par votre package peuvent être listés par la commande :
<pre class="lang:default decode:true ">ctln pyscript list</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Listing des scripts Python du package" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-6.png" alt="Listing des scripts Python du package" width="354" height="234" border="0" /></a></p>
<p style="text-align: left;" align="center">Ici votre package "MyFirstPackage" ne contient qu'un seul script nommé "Main.py" dans le répertoire Scripts du package.</p>

<h3 style="text-align: left;" align="center">Ajouter des scripts Python</h3>
<p style="text-align: left;" align="center">Pour ajouter un script Constellation Python, vous pouvez soit ajouter un script existant sur votre système de fichier dans votre package ou soit créer un nouveau script à partir d'un modèle (template).</p>

<h4 style="text-align: left;" align="center">Ajouter un script existant</h4>
<p style="text-align: left;" align="center">Pour un script existant, il suffit d'utiliser l'option "add" :</p>

<pre class="lang:default decode:true">ctln pyscript add &lt;filepath&gt; [--filename=&lt;name&gt;]
</pre>
Vous devez spécifier le chemin complet vers votre fichier à ajouter (filepath) et optionnellement le nom du fichier une fois ajouté dans votre package.

Par exemple la commande ci-dessous ajoutera dans votre package le fichier "~/demo/MyDemo.py" sous le nom "DemoSeb.py"
<pre class="lang:default decode:true">ctln pyscript add ~/demo/MyDemo.py --filename=DemoSeb.py</pre>
<h4 style="text-align: left;" align="center">Ajouter de nouveau script</h4>
<p style="text-align: left;" align="center">L'action la plus courante sera de créer un nouveau script Python dans votre package avec l'action "new" via la commande suivante :</p>

<pre class="lang:default decode:true ">ctln pyscript new &lt;filename&gt; [&lt;item_template_name&gt;]</pre>
Vous devez obligatoirement spécifier le nom du fichier (filename) avec ou sans l’extension .py (rajoutée automatiquement). Le script sera créé dans le répertoire Scripts de votre package et automatiquement déclaré dans la configuration.

Vous pouvez également spécifier l'identifiant du template à utiliser. Les templates peuvent être listés avec la commande "<em>ctln template list</em>" comme vu ci-dessus.  A l'heure où cet article est écrit il y a trois templates de type "item" :
<ul>
 	<li>"python-base" : le template de base</li>
 	<li>"python-demo"  : un template avec du code de demonstration</li>
 	<li>"python-empty"  : un squelette vide</li>
</ul>
Si vous ne précisez pas l'identifiant, c'est le template "python-base" qui sera utilisé.

Par exemple ajoutons à notre package un deuxième script (avec le template de base) :
<pre class="lang:default decode:true ">ctln pyscript new Main2</pre>
Rajoutons également un troisieme scriupt de démonstation en utilisant le template de demo (python-demo) :
<pre class="lang:default decode:true ">ctln pyscript new DemoConstellation python-demo</pre>
Notre package a maintenant trois scripts : Main.py (créé avec le template du package), Main2.py (créé ci-dessus avec le template de base), DemoConstellation.py (créé avec le template de demo) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout de scripts Python" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-8.png" alt="Ajout de scripts Python" width="354" height="226" border="0" /></a></p>

<h3 style="text-align: left;" align="center">Renommer ou supprimer des scripts</h3>
<p style="text-align: left;" align="center">Vous pouvez renommer les scripts du package avec l’option “rename”. Par exemple renommons “DemoConstellation” par “Demo” (les extensions “.py” sont optionnelles) :</p>

<pre class="lang:default decode:true">ctln pyscript rename DemoConstellation Demo</pre>
<a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-9.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="Renommer des scripts" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-9.png" alt="Renommer des scripts" width="354" height="226" border="0" /></a>

Pour supprimer des scripts il suffit d’utiliser l’option “remove” en spécifiant le nom du script (avec ou sans son extension .py).

Par exemple supprimons les deux fichiers “Main2.py” et “Demo.py” que nous avons ajoutés (et renommés) ci-dessus :
<pre class="lang:default decode:true">ctln pyscript remove Main2
ctln pyscript remove Demo</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Supprimer des scripts" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-10.png" alt="Supprimer des scripts" width="354" height="226" border="0" /></a></p>

<h3>Editer les scripts Python du package</h3>
Une fois vos scripts ajoutés, renommés ou supprimés avec la commande “ctln”, libre à vous d’utiliser votre éditeur de texte préféré pour éditer votre code !

Par exemple de le cas ici présent d’un accès SSH Linux, je pourrais utiliser “nano” ou “vim” (ou encore Notepad++ sur ma station WIndows via un tranfert SCP avec WInSCP par exemple).
<pre class="lang:default decode:true">nano Scripts/Main.py</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Edition des scripts Python avec Nano" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-11.png" alt="Edition des scripts Python avec Nano" width="354" height="226" border="0" /></a></p>
<p align="left">Ici le script “Main” défini une fonction de démarrage (<em>OnStart</em>) et une fonction de fermeture (<em>OnExit</em>). Pour rappel, les <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Les_bases">bases </a>de l’API Constellation Python <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Les_bases">sont expliquées ici</a>.</p>
<p align="left">Notez simplement que dans cet exemple, le WriteInfo déclenché au démarrage (<em>OnStart</em>) écrit un log dasn Constellation de type “Information” en concaténant dans un message textuel les propriétés suivantes :</p>

<ul>
 	<li>
<div align="left">“<span style="text-decoration: underline;">PackageName</span>” : le nom du package</div></li>
 	<li>
<div align="left">“<span style="text-decoration: underline;">SentinelName</span>” : le nom de la sentinelle sur laquelle est déployée ce package</div></li>
 	<li>
<div align="left">“<span style="text-decoration: underline;">IsStandAlone</span>” : un booléen indiquant si le package tourne de façon indépendante ou hébergée au sein d’une sentinelle</div></li>
 	<li>
<div align="left">“<span style="text-decoration: underline;">IsConnected</span>” : un booléen indiquant si le package est connecté ou non à une Constellation</div></li>
</ul>
<h2>Tester son package</h2>
Pour tester notre package en local, il suffit de lancer la commande suivante :
<pre class="lang:default decode:true">ctln run</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Démarrage du package en local" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-12.png" alt="Démarrage du package en local" width="354" height="215" border="0" /></a></p>
Comme vous pouvez le lire sur la console, le nom de la sentinelle affichée est “local sandbox” étant donné que la propriété <em>IsStandalone</em> est <em>true</em> (la propriété <em>SentinelName</em> est None). En effet votre package n’est pas déployé/démarré par Constellation sur une véritable sentinelle. Il est juste démarré en “standalone” par la commande “run”.

De plus la propriété “<em>IsConnected</em>” est <em>false</em> car votre package n’est pas connecté à une Constellation.

Vous pouvez donc tester des éléments de base de votre package mais de façon déconnecté, il manquera donc plusieurs fonctionnalités (les logs produits resterons local dans votre console, pas d'accès aux settings hormis ceux déclarés en local, la publication ou consommation de StateObjects sera impossible tout comme l’invocation ou l'exposition de MessageCallbacks).

Pour tester votre package complètement, il est possible de le lancer en local tout en le connectant à une Constellation.

Tout d'abord, il faudra déclarer des serveurs Constellation dans l'outil <em>ctln</em>.
<h3>Ajouter et gérer des serveurs Constellation</h3>
Les commandes sont les suivantes :
<pre class="lang:default decode:true">ctln server list
ctln server (add | set) &lt;name&gt; --url=&lt;url&gt; --accesskey=&lt;key&gt;
ctln server (add | set) &lt;name&gt; --url=&lt;url&gt; --username=&lt;user&gt;
ctln server check &lt;name&gt;
ctln server remove &lt;name&gt;
</pre>
Vous pouvez lister les serveurs, ajouter ou mettre à jour des serveurs avec la clé d’accès ou un couple login/password, tester la connectivité ou supprimer des serveurs.

Par exemple je dispose d’une Constellation installée à l’adresse : <a title="http://pc-seb.ajsinfo.loc:8088" href="http://pc-seb.ajsinfo.loc:8088">http://pc-seb.ajsinfo.loc:8088</a>. Un credential basé sur le couple login/password “admin/seb” a été configuré. Je peux donc ajouter ce serveur que je nommerai ici “pc-seb” via la commande :
<pre class="lang:default decode:true">ctln server add pc-seb --url=http://pc-seb.ajsinfo.loc:8088 --username=admin</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout de serveur Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-13.png" alt="Ajout de serveur Constellation" width="354" height="215" border="0" /></a></p>
Le serveur ajouté est testé au moment de l’ajout mais vous pouvez à tout moment tester la connectivité vers cette Constellation avec la commande “<em>ctln server check</em>”. Par exemple pour tester la Constellation nommée “pc-seb” :
<pre class="lang:default decode:true">ctln server check pc-seb</pre>
<h3>Tester son package dans Constellation</h3>
Maintenant que nous avons ajouté un serveur Constellation dans l’outil, nous pouvons relancé notre package en le connectant à notre Constellation nommée “pc-seb” via la commande :
<pre class="lang:default decode:true">ctln run --server=pc-seb
</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Démarrage du package en local mais connecté à un serveur Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-14.png" alt="Démarrage du package en local mais connecté à un serveur Constellation" width="354" height="215" border="0" /></a></p>
<p align="left">Le package démarre toujours dans la "sandbox locale" (<em>IsStandalone = true</em>) mais cette fois ci il est connecté à ma Constellation nommée "pc-seb" (<em>IsConnected = true</em>).</p>
<p align="left">D’ailleurs en lançant la Console Constellation je peux maintenant suivre en temps réel les logs de ce package, les StatesObjects qu’il produit ou encore les MessageCallbacks qu’il expose.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-15.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Connexion du package avec sa Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-15.png" alt="Connexion du package avec sa Constellation" width="354" height="208" border="0" /></a></p>

<h2>Publier son package</h2>
Une fois le package développé et testé, il ne reste plus qu’à le packager pour pouvoir le déployer dans Constellation.

Vous pouvez soit le packager dans un fichier en local (que vous pourrez uploader sur une Constellation ou échanger avec d'autre) ou soit le déployer directement dans une Constellation référencée dans l'outil.

Par exemple pour publier ce package dans un fichier local dans le répertoire “/home/pi” :
<pre class="lang:default decode:true">ctln publish --output=/home/pi</pre>
Maintenant, pour déployer ce même package directement sur une Constellation, il suffit simplement de spécifier le nom du serveur Constellation enregistré sur la commande “publish”.

Par exemple pour publier ce package sur la Constellation nommée ici “pc-seb” :
<pre class="lang:default decode:true">ctln publish pc-seb</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-16.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Publication d'un package sur une Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-16.png" alt="Publication d'un package sur une Constellation" width="354" height="215" border="0" /></a></p>
<p align="left">En vous rendant sur le <a href="/constellation-platform/constellation-console/package-repository/">Package Repository</a> de votre Console Constellation, votre package fraîchement publié sera prêt à être <a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Deployer_un_package">déployé sur vos sentinelles Windows ou Linux</a> de votre Constellation.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/09/image-17.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Package Repository" src="https://developer.myconstellation.io/wp-content/uploads/2017/09/image_thumb-17.png" alt="Package Repository" width="354" height="268" border="0" /></a></p>

<h2>Next steps</h2>
Pour en savoir plus sur le développement de package en Python :
<ul>
 	<li>Retour sur <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Les_bases">les bases</a> de l’API Python  (<a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Produire_des_logs">produire des logs</a>, <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Acces_aux_settings">accéder aux settings</a>, <a href="/getting-started/creez-votre-premier-package-constellation-en-python/#Publier_des_StateObjects">publier des StateObjects</a>)</li>
 	<li><a href="/client-api/python-api/messagecallbacks-exposer-des-methodes-python/">Exposer vos fonctions</a> Python dans Constellation (les MessageCallbacks)</li>
 	<li><a href="/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/">Invoquer des MessageCallbacks</a> des autres packages de votre Constellation en Python</li>
 	<li><a href="/client-api/python-api/consommer-des-stateobjects-en-python/">Consommer les StateObjects</a> de votre Constellation</li>
</ul>