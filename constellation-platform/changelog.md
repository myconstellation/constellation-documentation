---
ID: 2122
post_title: Changelog
author: Sebastien Warin
post_date: 2016-07-21 13:30:04
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/changelog/
published: true
post_modified: 2017-11-23 20:11:57
---
<h3>23/11/2017 : Update Console 1.8.3.17326</h3>
<ul>
 	<li>Console / MessageCallbacks Explorer : typage des paramètres Number (Int, Decimal, Float, etc..) et Boolean dans l'envoi de message depuis l'explorateur</li>
 	<li>Console / Package : bugfix de l'appel du 'RefreshSettings' doublé en cas de mise à jour des settings</li>
</ul>
<h3>15/09/2017 : Release 1.8.3 (1.8.3.17258)</h3>
<ul>
 	<li>Common : Correction interne d'un bug fatal entraînant le crash du serveur au démarrage, des sentinelles et dans certain cas des packages (si le package invoque <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/#Utiliser_les_Tasks_awaitable_pour_attendre_la_reponse_dune_saga">une saga avec une Task&lt;T&gt;</a>) sur les systèmes utilisant la version 5.2 (ou supérieure) du runtime Mono (inclus nativement depuis Debian / Raspbian 9). Ce bug est lié au PR Mono <a href="https://github.com/mono/mono/pull/4404">#4404</a> publié depuis Mono 5.2.0.179 (remplacement du Microsoft.CSharp par l'implémentation du CoreFX)</li>
 	<li>Common : durcissement de la classe ci-dessus pour éviter qu'un bug de ce type ne soit "fatal" en cas de changement d'implémentation du RuntimeBinder.</li>
 	<li>Sentinel : mise à jour de la libraire Common pour bénéficier du correctif sur un environnement Mono &gt;= 5.2</li>
 	<li>Server : mise à jour de la libraire Common pour bénéficier du correctif sur un environnement Mono &gt;= 5.2</li>
 	<li>Server : enregistrement de toutes erreurs Fatales dans le fichier de log "ConstellationServerManager" avant la fermeture du processus en cas de crash</li>
 	<li>SDK : Mise à jour des templates de projet avec le package Constellation 1.8.3.17258</li>
</ul>
<h3>26/08/2017 : Release 1.8.3 (1.8.3.17238)</h3>
<ul>
 	<li>Common : ajout de surcharges acceptant un string seulement sur les méthodes WriteInfo/WriteDebug/WriteWarn/WriteError pour bypasser le string.Format si aucun argument n'est passé (évitant également les erreurs où la valeur d'un string contient des accolades)</li>
 	<li>Server / Licensing : bugfix impossibilité de renouveller une licence expirée au démarrage du serveur (bug introduit dans la release 1.8.3.17190)</li>
 	<li>Server / Management API : contrôle de l'existance d'un package sur une sentinelle lors d'un RemovePackage (autrement le serveur lève une NullReferenceException)</li>
 	<li>Console : le bouton de déconnexion est caché si page de login est désactivée (AccessKey dans le fichier config.js)</li>
 	<li>Console : redirection automatique vers la console lors du chargement de la page login si déjà loggé (cookie valide) ou que la page de login est désactivée (AccessKey dans le fichier config.js)</li>
 	<li>Console : bugfix du retour vers la page de login toutes les 10 secondes alors que désactivée (accesskey dans le fichier config.js)</li>
 	<li>SDK : Mise à jour du template Python</li>
 	<li>SDK : Mise à jour des templates de projet avec le package Constellation 1.8.3.17238</li>
 	<li>SDK : Mise à jour des templates de projet avec le package PythonProxy 1.8.3.17238</li>
</ul>
<h3>25/08/2017 : Constellation Package Tools 1.0.5</h3>
<ul>
 	<li>Publication de la première version du Constellation Package Tools sur PyPi (pip)</li>
</ul>
<h3>22/08/2017 : Update SDK 1.8.3.17234</h3>
<ul>
 	<li>SDK : Mise à jour du template de projet PythonPackage avec le PythonProxy 1.8.3.17234</li>
 	<li>SDK : Mise à jour des reférences du VS Package &amp; Code Generator avec Constellation 1.8.3.17190, SignalR 2.2.2 et Json.net 9.0.1</li>
 	<li>SDK : Compatibilité VS2017 Update 3 (15.3)</li>
</ul>
<h3>10/07/2017 : Release 1.8.3 (1.8.3.17190)</h3>
<ul>
 	<li>Common : Dépendances mises à jour vers JSON.NET 9.0.1 et SignalR 2.2.2</li>
 	<li>Common : Le PackageHost passe le ConstellationClientType à "net40" ou "net45" lors de la connexion à Constellation</li>
 	<li>Server : Dépendances mises à jour vers JSON.NET 9.0.1, SignalR 2.2.2, NLog 4.4.11 et OWIN 3.1.0</li>
 	<li>Server : Ajout du ConstellationClientType dans les informations des packages sur le hub "Controller"</li>
 	<li>Server : Extraction des propriétés 'PackageVersion', 'ConstellationClientVersion' et 'ConstellationClientType' depuis les QueryString si manquantes des headers HTTP (pour les packages Constellation utilisant la lib JS &gt; 1.8.2)</li>
 	<li>Server / ControlHub : nouvelle gestion des packages (réécriture complète, chargé en mémoire et parfaitement synchronisé au niveau du serveur entre la configuration XML et les contrôleurs)</li>
 	<li>Server / ControlHub : le ReloadServerConfiguration envoi la liste des packages aux sentinelles seulement connectées et "Update settings" sur les packages connectés seulement et non à tous les groupes</li>
 	<li>Server / ControlHub : prise en charge des mises à jour de configuration des packages à volée</li>
 	<li>Server / Management API : ajout d'un objet de synchronisation pour les I/O vers le fichier de configuration pour éviter les écritures concurrentes</li>
 	<li>Server / Licensing : LeaseLicensing dans le domaine serveur si multi-tenant sinon dans le domaine de la constellation si mono-instance (permettant ainsi de récupérer les erreurs de licence dans la Console Log)</li>
 	<li>Server / API REST : Gestion du IsConnected sur les packages virtuels en se basant sur les abonnements de messages et/ou SO (SubscriptionId)</li>
 	<li>Server / API REST : Timeout des sessions de 150 secondes par défaut configurable dans le fichier de configuration</li>
 	<li>Server : Ajout de la clé "Constellation.BaseDirectory" permettant de définir le répertoire de base de la Constellation dans lequel trouver le fichier de configuration (cela permet de séparer les binaires de la configuration, très utile dans un environnement Docker)</li>
 	<li>Server : refactoring et optimisations du code</li>
 	<li>Sentinel : Dépendances mises à jour vers JSON.NET 9.0.1, SignalR 2.2.2, NLog 4.4.11 (et NLog.Windows.Form 4.2.3 pour les SentinelUI)</li>
 	<li>Sentinel : Refactoring complet des sentinelles (suppression du ProcessManager présent depuis la 1.0, réécriture complète, chargement en mémoire et ne relance pas les packages au reload, plus stable et meilleure cohérence des états, etc..)</li>
 	<li>Sentinel : ne lève pas d'erreur si la collection est modifiée au CheckProcessUsage (InvalidOperationException)</li>
 	<li>Sentinel : les packages virtuels déployés sur une sentinelles réelles sont exclus (et ne lève pas d'erreur)</li>
 	<li>Console : nouvelle gestion des packages basée sur les nouveautés du Server 1.8.3 (il n'y a plus de lecture des package en XML)</li>
 	<li>Console : support complet des utilisateurs sans les droits de Management</li>
 	<li>Console / Sentinels : Ajout du nom du credential utilisé par une sentinelle sur la fenêtre de détail d'une Sentinelle</li>
 	<li>Console / Package : Ajout du ConstellationClientType dans la liste et fenêtre de détail</li>
 	<li>Console / Package : Gestion des packages virtuels avec un statut Unknown, menu contextuel synchronisé et mise à jour de l'activité pour les package virtuel</li>
 	<li>Console / Packages : "Update Settings" si le package est connecté et démarré (package virtuel compris)</li>
 	<li>Console / Packages : bugfix déploiement de package avec un setting de type DateTime (module $filter non injecté dans le contrôleur principal)</li>
 	<li>Console / Packages : bugfix déploiement de la configuration une fois que le SetRecoveryOption est terminé et non en parallèle</li>
 	<li>Console / Console Log : ajout d'un bouton pour vider la console</li>
 	<li>Console / MessageCallbacks Explorer : Mise à jour des code snippets avec les nouveautés des libs JS/NG 1.8.2</li>
 	<li>Console : déconnexion des deux hubs (consumer et controller) si l'un des deux est déconnecté (cas très rare où l'un des deux hubs parvient à se reconnecter et pas l'autre)</li>
 	<li>Console : Mise à jour de l'API Constellation Javascript &amp; AngularJS en version 1.8.2 et de la librairie SignalR 2.2.2</li>
 	<li>SDK : Mise à jour des templates de projet avec les librairies Constellation 1.8.3.17190, SignalR 2.2.2 et Json.net 9.0.1</li>
</ul>
<h3>05/07/2017 : Update API Javascript &amp; AngularJS 1.8.2</h3>
<ul>
 	<li>API Javascript : ajout du proxy "Package" pour écrire des packages (virtuels) en Javascript (eg. NodeJS)</li>
 	<li>API Javascript : paramètre "data" optionnel sur la méthode sendMessage (avec ou sans Saga)</li>
 	<li>API Javascript : possibilité de passer directement plusieurs paramètres sur la méthode sendMessage (avec ou sans Saga) pour les MessageCallbacks avec plusieurs arguments sans devoir déclarer un tableau de paramètre</li>
 	<li>API Javascript : le callback de retour d'une saga est désormais le premier paramètre de la méthode sendMessageWithSaga. Gestion de la rétro-compatibilité permettant de passer le callback comme dernier paramètre (pas de breaking change en cas d'update)</li>
 	<li>API AngularJS : basée sur l'API Javascript 1.8.2 (intégration des nouveautés concernant les sendMessage et sendMessageWithSaga)</li>
 	<li>API AngularJS : ajout de la méthode disconnect sur les deux modules (Controller et Consumer)</li>
</ul>
<h3>29/06/2017 : Update Console 1.8.2.17180</h3>
<ul>
 	<li>Console : rechargement automatique du repository de package local lorsqu'un package est uploadé via la Management API</li>
 	<li>Console : ajout d'un timestamp sur les appels "RequestStateObjects" et de contrôle des dernières versions pour éviter les erreurs de cache navigateur</li>
</ul>
<h3>28/06/2017 : Release 1.8.2.17178</h3>
<ul>
 	<li>Common : déclenchement de la procédure d’arrêt (OnPreShutsown puis OnShutdown) sur les packages lancés hors sentinelle (mode Standalone)</li>
 	<li>PythonProxy : Mécanisme de Ping/Pong entre le package C# et les scripts Python pour vérifier l'état de vie des deux parties. Auto-fermeture du processus Python si pas de Ping reçu pdt plus de 30 secondes pour éviter les processus orphelins (notamment en cas de debug)</li>
 	<li>PythonProxy : Ajout d'une section de configuration dans App.config pour définir les scripts Python à lancer (plutôt que se baser sur les fichiers .py du dossier Scripts)</li>
 	<li>PythonProxy : Gestion d'un timeout du démarrage des scripts Python (30sec par défaut configurable en fichier de config)</li>
 	<li>PythonProxy : Mode unbuffered par défaut (configurable dans la section XML) et redirection du stream de sortie et d'erreur des processus Python dans les logs Constellation</li>
 	<li>PythonProxy : Monitoring des processus Python avec logging en cas d’arrêt</li>
 	<li>PythonProxy : Autre changement mineur (refactoring &amp; improvment)</li>
 	<li>SDK : Mise à jour des templates de projet avec le package Constellation 1.8.2.17178</li>
 	<li>SDK : Mise à jour des templates de projet avec le package PythonProxy 1.8.2.17178</li>
 	<li>SDK : Mise à jour du SDK sur la libraire Constellation 1.8.2.17178 et VisualStudio 15.0.26606</li>
 	<li>SDK : Nettoyage automatique de la solution avant compilation lors d'une publication de package</li>
 	<li>SDK : Ajout automatique de l'icone déclarée dans le manifeste dans le package (ajout de l'attribut CopyIfNewer &amp; Build Action: Content)</li>
 	<li>SDK : Ajout d'un ItemTemplate "Constellation Python Script" (attribut CopyIfNewer sur le fichier et ajout dans le App.config automatique)</li>
 	<li>SDK : Ajout du schéma XSD de configuration des packages Python dans le SDK</li>
</ul>
<h3>22/06/2017 : Update Console 1.8.2.17173</h3>
<ul>
 	<li>Console / Package : bugfix sur l'édition d'un setting de type boolean ou number sur un package sans manifeste (eg. Virtual Package)</li>
 	<li>Console / StateObjects Explorer : Refresh automatique du SO lorsqu'il arrive à expiration dans la modal de détail</li>
 	<li>Console / StateObjects Explorer : bugfix de la mise à jour des StateObjects qui n'étaient pas maintenus en cas de changement de page</li>
</ul>
<h3>20/05/2017 : Update Client 1.8.2.17140</h3>
<ul>
 	<li>Common : bugfix lors des réponses de Saga sur le framework Mono (veuillez mettre à jour la librairie sur vos packages depuis Nuget)</li>
 	<li>Common : détection du framework .NET 4.6.2 et .NET 4.7 dans la description du package au serveur</li>
</ul>
<h3>28/04/2017 : Release 1.8.2.17118</h3>
<ul>
 	<li>Server / Management API : enregistrement des tableaux JSON dans un SettingContent (et non un SettingValue)</li>
 	<li>Console : édition des settings XML et JSON dans CodeMirror même s'ils ne sont pas déclarés dans un manifeste (eg. package virtuel) en se basant sur la détection du contenu</li>
 	<li>Console : injection d'un HttpInterceptor pour ajouter dynamiquement le n° de version sur les URI des templates Angular afin d'éviter les problèmes de cache lors des mises à jour de la Console</li>
</ul>
<h3>26/04/2017 : Release 1.8.2.17117</h3>
<ul>
 	<li>Server / Control Hub : contrôle de la cohérence des changements d'état des packages sur le ControlHub (pour éviter les packages dans un état incohérent dans certaines conditions rares)</li>
 	<li>Server / Management API : les méthodes REST d'accès à la configuration (Get et Post) utilisent explicitement l'encoding UTF8 pour éviter les erreurs d'encodage</li>
 	<li>Console : correction de l'éditeur de configuration qui ne chargeait pas correctement la configuration dans certaines conditions (bug lié à CodeMirrorUI)</li>
 	<li>Console : actualisation des notifications de mises à jour des composants à la reconnexion au serveur</li>
</ul>
<h3>24/04/2017 : Update Console 1.8.2.17114</h3>
<ul>
 	<li>Console / Online Package Repository : fenêtre de déploiement des mises à jour des packages affichée au format "large"</li>
 	<li>Console : rechargement du n° de version du serveur, de la licence, du package repository et des recovery options à chaque reconnexion au serveur</li>
</ul>
<h3>22/04/2017 : Release 1.8.2 (1.8.2.17112)</h3>
<ul>
 	<li>Server : section de configuration Constellation "constellationSection" renommée en "constellation"</li>
 	<li>Server : ajout de la clé de configuration "EnablePerformanceCounters" pour activer/désactiver les PerfCounters (par défaut désactivé si non présente)</li>
 	<li>Server : bugfix SubscribeStateObjects non fonctionnel sur Linux</li>
 	<li>Console : mise à jour dynamique des vues en cas d'ajout ou suppression de sentinelles ou de package</li>
 	<li>Console : ajout d'une fenetre de selection des package à recharger (reload) après le téléchargement d'une nouvelle version de package avec vérification de la compatibilité des settings obligatoires</li>
 	<li>Console : extinction automatique des packages d'une sentinelle supprimée</li>
 	<li>Console : contrôle de la compatibilité des packages &lt;&gt; sentinelle (décrite dans la section Platforms du Package Manifest)</li>
 	<li>Console : les filtres sont conservés lorsque l'on revient sur la page au sein d'une même fenetre</li>
 	<li>Console : affichage du nombre d'élément filtré et total sur chaque page</li>
 	<li>Console : le label d'expiration se met automatiquement à jour en cas d'expiration d'un StateObject sur la fenetre de détail d'un StateObject</li>
 	<li>Console : limite de 500 lignes dans la Console Log et de 20 erreurs max dans la sidebar de notification pour éviter les dérives de performances</li>
 	<li>Console : rafraichissement automatique du cookie d'authentification lorsque l'on est connecté (pour éviter les déconnexions au changement de page)</li>
 	<li>Console : mise en forme des types génériques sur le StateObjects Explorer &amp; MessageCallbacks Explorer</li>
 	<li>Console : possibilité de détacher chaque page dans une fenetre ou un onglet à part</li>
 	<li>Console : possibilité de déployer différentes instances d'un même package sur une même sentinelle (le Filename est automatiquement défini si doublon)</li>
 	<li>Console : bugfix graphique sur le rédimensionnement de la Console log &amp; editeur de configuration</li>
 	<li>Console : mise à jour des codes snippets pour les appels HTTP (exemples pour l'API Constellation et l'API Consumer) dans le MessageCallbacks Explorer</li>
 	<li>Console : appel des ressources JS/CSS et templates HTML avec le numéro de version pour éviter les problèmes de cache lors des mises à jour de la Console</li>
 	<li>SDK : Support de Visual Studio 2017 (toutes éditions)</li>
 	<li>SDK : Ouverture des fenêtres centrées par rapport au parent (fenêtre de Visual Studio) et non sur l'écran principal</li>
 	<li>SDK : Mise à jour du Code Generator (support des Booleans optionels dans la signature des MC, ajout de méthode d'extension pour les StateObjects de type générique et ajout de commentaire XML dans le code généré)</li>
 	<li>SDK : Mise à jour du schéma XSD de la configuration du serveur en version 1.8.2</li>
 	<li>SDK : Mise à jour des templates de projet</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.1.16288 dans les templates de projet</li>
 	<li>SDK : Nouveau système d'installation du SDK (basé sur VSIXBootstrapper et vswhere)</li>
</ul>
<h3>31/10/2016 : Update Console 1.8.1.16305</h3>
<ul>
 	<li>Console / Online Package Repository : ajout d'une zone de recherche pour le catalogue en ligne</li>
 	<li>Console / Online Package Repository: ajout d'un lien vers la documentation du package et suppression de la fenêtre d'information complémentaire</li>
 	<li>Console / MessageCallbacks Explorer : gestion des types "Byte" (de 0 à 255)</li>
 	<li>Librairie JavaScript / sendMessageWithSaga : génération du SagaId seulement si pas il n'est spécifié sur le scope et ajout d'un nombre aléatoire au timestamp</li>
</ul>
<h3>14/10/2016 : Release 1.8.1.16288</h3>
<ul>
 	<li>Common : la méthode GetSettingValue accepte un IFormatProvider en paramètre optionnel (par défaut le CurrentCulture est utilisé)</li>
 	<li>Common : la méthode GetSettingValue remplace les "," par des "." et utilise le InvariantCulture si T est de type Double, Single, Float ou Decimal que le IFormatProvider n'est pas explicitement spécifié</li>
 	<li>Common : remplacement des comparaisons de string de InvariantCulture par Ordinal</li>
 	<li>Console / MessageCallback Explorer : envoi de la valeur par défaut pour un paramètre optionnel si la valeur n'est pas spécifiée</li>
 	<li>Console / MessageCallback Explorer : suppression des MC qui ont été supprimés du package lors d'une mise à jour du PackageDescriptor</li>
</ul>
<h3>06/10/2016 : Update Console 1.8.1.16280</h3>
<ul>
 	<li>Console / Console Log : ajout d'une checkbox "Autoscroll" pour activer ou non le défilement automatique</li>
 	<li>Console / Console Log : scroll automatiquement à la fin de la console lors l'activation de la page</li>
 	<li>Console / Configuration Editor : bugfix où l’éditeur XML reste vide lors d'un changement de page</li>
 	<li>Console : repositionnement des toasts de notification en mode mobile</li>
 	<li>Console : éléments des menus en noir sur blanc quelque soit le media CSS sélectionné (bugfix visuel en mode mobile)</li>
 	<li>Console : correction du mécanisme de chargement des pages (<em>event onLoad</em>) de la release 16272</li>
 	<li>Console : amélioration du redimensionnement dynamique des tables (bugfix visuel dans certain cas)</li>
 	<li>Console : bugfix où le bouton "Save &amp; Deploy" sur la page de Setting n’était pas désactivé si erreur dans la page</li>
 	<li>Console : mise à jour de la libraire jQuery Terminal 0.11.11 et minification du CSS</li>
</ul>
<h3>29/09/2016 : Release 1.8.1.16273</h3>
<ul>
 	<li>Console : ajout de la meta "robots:noindex,nofollow" pour interdire l'indexation de la Console Constellation par les moteurs de recherche</li>
 	<li>Console / StateObject Explorer : rafraichit la valeur du StateObject avant s’abonner (<em>requestSubscribeStateObjects</em>) lorsque l’on clique le bouton “Subscribe” sur la fenêtre de visualisation d’un StateObject pour être certain de voir la dernière valeur</li>
 	<li>Server : ajout des metas "robots" et "pragma:no-cache" sur la home page du serveur Constellation</li>
 	<li>SDK : mise à jour du package Constellation 1.8.1.16272 dans les templates</li>
 	<li>SDK : mise à jour du Code Generator (support des sentinelles/packages virtuels et prise en jour des types personnalisées avec caractères spéciaux)</li>
</ul>
<h3>28/09/2016 : Release 1.8.1.16272</h3>
<ul>
 	<li>Common : ajout de la propriété 'HasValue' sur l’objet StateObject et sur le StateObjectNotifier</li>
 	<li>Common : la méthode <em>StateObject.GetValue&lt;T&gt;()</em> retourne le default(T) si le StateObject n'a pas de Value (<em>HasValue = false</em>)</li>
 	<li>Console : la barre de menu est en "top-most" devant les notifications et placement des toasts de notification juste en dessus cette barre de menu</li>
 	<li>Console : révision de la fenêtre d'information des versions</li>
 	<li>Console : affichage du catalogue de package en ligne lorsque l'on clique sur la notification de mise à jour d'un package</li>
 	<li>Console / Package Repository : affichage des updates de package disponible en haut de la liste du "Package Store"</li>
 	<li>Console : suppression du contrôle du cookie d'authentification à chaque changement de page mais renouvèlement de ce cookie à chaque connexion à Constellation</li>
 	<li>Console : vérification des credentials directement depuis la page de login avant l'ouverture de la console</li>
 	<li>Console : révision du mécanisme de chargement des pages (<em>event onLoad</em>) pour éviter d'invoquer le handler plusieurs fois</li>
 	<li>Console / StateObject Explorer : bug-fix de l'affichage des valeurs des StateObject où la valeur numérique ou booléenne est égale 0 ou false</li>
 	<li>Server / Management API : rétrocompatibilité de la route pour l'action "SetServerConfiguration" (rétrocompatibilité pour les anciennes versions de la Console et SDK)</li>
 	<li>Server / Controller API : ajout de l'interface REST "Controller" pour tester les credentials de la Console (action CheckAccess)</li>
 	<li>Server : enregistrement &amp; restauration des PackageDescriptors dans un fichier plat à chaque arrêt/démarrage du serveur et suppression lorsque le package disparait de la configuration</li>
 	<li>Server : mise en attente de la requête pendant 1 seconde (par défaut) si accès refusé (en cas de 403 Forbidden) pour éviter les attaques de type brute-force. Valeur personnalisable par le appSetting "<em>DelayAfterAuthentificationError</em>" (0 pour désactiver)</li>
 	<li>Server : optimisations minimes sur l'ajout des PackageDescriptors, Sentinelles et Settings (suppression d'un contrôle inutile à l'ajout)</li>
</ul>
<h3>23/09/2016 : Update SDK 1.8.1.16267</h3>
<ul>
 	<li>SDK : Bugfix de l'enregistrement de la configuration suite au changement de route du serveur 1.8.1</li>
</ul>
<h3>20/09/2016 : Update Console 1.8.1.16264 &amp; librairies Javascript</h3>
<ul>
 	<li>Console : mise à jour des code templates Arduino dans le MessageCallback Explorer (synchronisation avec la lib Arduino/ESP 2.x)</li>
 	<li>Console : mise à jour des librairies Javascript 1.8.1</li>
 	<li>Librairies Javascript &amp; AngularJS version 1.8.1 : revu de la gestion des sagas et ajout des méthodes registerMessageCallback et registerStateObjectLink</li>
</ul>
<h3>15/09/2016 : Release 1.8.1 (1.8.1.16259)</h3>
<ul>
 	<li>Console : bug-fix lorsque l'on accède à la Console Constellation par la page "StateObject Explorer" (la fonction refreshStateObjectsList() n'est pas trouvée)</li>
 	<li>Server : bug-fix des PerformanceCounters sous environnement Mono/Linux</li>
 	<li>Release To Web (RTW) - Version 1.8.1 stable</li>
</ul>
<h3>14/09/2016 : Release 1.8.1 RC3 (1.8.1.16258)</h3>
<ul>
 	<li>Common : Ajout de surcharges sur les WriteDebug/WriteInfo/WriteError/WriteWarn pour prendre en argument un simple 'object'</li>
 	<li>Server / Management API : bug-fix sur la méthode legacy 'AddSentinel' empêchant l'enregistrement des sentinelles via l'installation Windows &amp; Linux</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.1.16258 dans les templates</li>
</ul>
<h3>09/09/2016 : Update Server 1.8.1.16253</h3>
<ul>
 	<li>Server : correction d'une erreur pouvant apparaitre lors de la fermeture du service Windows</li>
 	<li>Server / Web API : l'action SendMessage accepte le parametre "sagaId"</li>
 	<li>Server / Web API : suppression de l'action SendMessageWithSaga (passer le sagaId dans la méthode SendMessage)</li>
</ul>
<h3>05/09/2016 : Update Server 1.8.1.16249</h3>
<ul>
 	<li>Server / Web API : bug-fix du SendMessage et PushStateObject en POST HTTP</li>
 	<li>Server / Consumer REST API : bug-fix du type de "Sender" à "ConsumerHttp" sur les actions "SendMessage" et bug-fix du SendMessage en POST HTTP</li>
</ul>
<h3>03/09/2016 : Update Console 1.8.1.16247</h3>
<ul>
 	<li>Console : requestPackageDescriptor pour tous les packages de la configuration (dont les Virtuels)</li>
 	<li>Console : requestPackageDescriptor lorsqu'un package déclare son PackageDescriptor</li>
</ul>
<h3>21/07/2016 : Release 1.8.1 RC2 (1.8.1.16203)</h3>
<ul>
 	<li>Console : bugfix sur la page des Credentials (mauvaise gestion de l'erreur)</li>
 	<li>Console : beautify des settings (ConfigSection, XML ou Json) existants</li>
 	<li>Console : bugfix du générateur de code pour les MC sans parametre</li>
 	<li>Server : bugfix sur le PerfCounter du nombre de consumer connecté</li>
 	<li>Server / Management API : les packages téléchargés ne portent plus le n° de version dans leurs fichiers</li>
 	<li>Server / Management API : retrocompatibilité corrigé pour l'upload de package</li>
</ul>
<h3>21/07/2016 : Update SDK 1.8.1.16203</h3>
<ul>
 	<li>SDK : Mise à jour du package Constellation 1.8.1.16201 dans les templates</li>
 	<li>SDK : Mise à jour du Code Generator (gestion des paramètres optionnels)</li>
</ul>
<h3>20/07/2016 : Release 1.8.1 RC1 (1.8.1.16202)</h3>
<ul>
 	<li>Common : surcharge des opérateurs d'équalité pour le RecoveryOptions</li>
 	<li>Common / PackageManifest : ajout d'une section "dependencies" permettant de définir des dépendances entre packages (Dependency = Name &amp; Version)</li>
 	<li>Common / Sentinel : ajout des propriétés OSCaption (pour Windows), CLRImplementation et FxVersion sur l'objet SentinelDescription</li>
 	<li>Common / Sentinel / Server : Task en LongRunning pour les taches en background</li>
 	<li>Common / Server : ajout du package description dans le "ReportPackageState"</li>
 	<li>Common : support des valeurs par defaut/params optionnels dans la description des MC</li>
 	<li>Sentinel : envoie dans le SentinelDescription l' OSCaption (pour Windows), la CLRImplementation (Mono ou Microsoft) et la version du .NET Fwk pour Mono ou MS</li>
 	<li>Sentinel : mise à jour des RecoveryOptions lors d'un Deploy même si le package tourne déjà et renvoi du "PackageStateReport"</li>
 	<li>Sentinel : lors d'un deploy, si le package tourne déjà, message de Warning (et non une erreur)</li>
 	<li>Sentinel : ajout d'un Warning au démarrage du package si le DotNetTargetPlatform .NET45 n'est pas respecté</li>
 	<li>Sentinel : revu du log de démarrage d'un package (ajout du nom du package et sa version si manifest trouvé)</li>
 	<li>Server : le filename d'un package est soit explicitement défini dans le tag "filename" si se termine par ".zip", soit retrouvé à partir du repository par le filename (si présent) ou name en prenant la version la plus haute, sinon &lt;name&gt;.zip si rien n'est trouvé !</li>
 	<li>Server / Config : renommage de la section des authorizations des groupes ('messageGroupAuthorizationElement' en 'authorization') /!\ UPDATE DU SCHEMA XSD !!</li>
 	<li>Server / StateObjectProvider : ajout d'une propriété Count et modification de la méthode Remove pour retourner le nombre de SO supprimés</li>
 	<li>Server / StateObject : ajout du "ConstellationStateObjectManager" pour la gestion des SO sur le serveur (et non accès direct au StateObjectProvider depuis le ConstellationHub)</li>
 	<li>Server / Messaging : bugfix si authorization fail (probleme dans message de log)</li>
 	<li>Server / Messaging : ne check pas l'authorization pour envoyer un message de reponse d'une saga</li>
 	<li>Server / PerformanceCounter : ajout de la classe ConstellaitonPerformanceCounter pour la gestion des compteurs de performances (35 compteurs actuellements)</li>
 	<li>Server / PerformanceCounter : ajout du "RequestCounterMiddleware" dans le pipeline OWIN et modifications des différents Hub &amp; Controlleurs WebAPI pour incrémenter les PerfCounters</li>
 	<li>Server / GroupManager : bugfix (ajout de doublons systématique) &amp; optimisation (basé sur un HasSet et controles des valeurs ou renvoi null)</li>
 	<li>Server / ConstellationHub &amp; ConsumerHub : optimisation de l'envoi de message avec compteurs de performance</li>
 	<li>Server / Authorization : bugfix sur le Check des authorizations si il existe plusieurs credentials avec la même access key</li>
 	<li>Server / Web API : revu de la purge des sessions (busy loop)</li>
 	<li>Server / Web API : supression des "_BackField" sur les objets marqués serializable (IgnoreSerializableAttribute = true)</li>
 	<li>Server / Management API : refonte des routes des actions du controlleur de Management</li>
 	<li>Server / Management API : refactoring avec ajout d'Helper pour la manipulation du fichier de config XML et des packages ZIP</li>
 	<li>Server / Management API : ajout d'un cache sur le Package Repository</li>
 	<li>Server / Management API : ajout de nouvelles &amp; mise à jour des actions pour la gestion complete de la configuration du serveur</li>
 	<li>Console : mise à jour Angular Core &amp; Anime 1.5.5 jQuery 2.2.3 Angular UI router 0.2.18</li>
 	<li>Console : ajout du module UI-Bootstrap (Bootstrap for Angular)</li>
 	<li>Console : révision du menu de la toolbox</li>
 	<li>Console : optimisations d'execution des filters et affichage du nombre d'éléments visibles dans les listes</li>
 	<li>Console : bouton/dropdown affiché sur la gauche et vers le haut si pas assez de place pour afficher tout le menu</li>
 	<li>Console : ajout d'une modal d'affichage des n° de version (console &amp; server) avec check des updates</li>
 	<li>Console : notification des nouvelles versions (composants ou packages)</li>
 	<li>Console / Sentinel : ajout d'un menu contextuel et d'une modal pour l'ajout, l'edition des sentinels et la suppression</li>
 	<li>Console / Sentinel : affichage des sentinels non connectés (dont les virtuels)</li>
 	<li>Console / Sentinel : ajout des infos liées à la CLR</li>
 	<li>Console / Packages : affichage des packages non connectés (dont les virtuels) et désactivé</li>
 	<li>Console / Packages : ajout de filtres pour l'affichage</li>
 	<li>Console / Packages : nouveau menu contextuel en fonction du type de package avec supression</li>
 	<li>Console / Package : ajout d'une modal pour la configuration d'un package</li>
 	<li>Console / Package : ajout d'une modal pour l'edition des settings d'un package</li>
 	<li>Console / Package : ajout d'une modal pour l'edition des groupes d'un package</li>
 	<li>Console / Log : bugfix des notifications d'erreur qui ne s'effacaient pas !</li>
 	<li>Console / Log : dropdown list editable sur les filtres des sentinels &amp; package</li>
 	<li>Console / Message Callback : bugfix où tout était envoyé en saga refonte des modals</li>
 	<li>Console / Message Callback : bouton Inovke sans selection de la sentinelle, si une seule</li>
 	<li>Console / Message Callback : support des parametres optionels et valeurs par defaut</li>
 	<li>Console / Message Callback : bugfix du code generator et ajout des templates avec saga</li>
 	<li>Console / StateObjects : refonte des modals et meilleur gestion des refresh</li>
 	<li>Console / Credentials : ajout d'une page de gestion des credentials</li>
 	<li>Console / Credentials / Authorization : ajout d'une page de gestion des authorizations des credentials</li>
 	<li>Console / Package Repository : ajout d'un menu contextuel pour le déploiement, renommage et supression des packages</li>
 	<li>Console / Package Repository : intégration du catalogue "online" (ajout et mise à jour)</li>
 	<li>Console / Configuration Editor : ajout de l'intellisense XML sur le fichier de configuration Constellation</li>
 	<li>Console : déplacement des modals communs dans un fichier à part (injecté dans la Default avec ng-include) et refonte totale avec UI-Bootstrap</li>
 	<li>Lib AngularJS : séparation des factories Constellation/Consumer versus Management =&gt; deux modules Angular différents !</li>
 	<li>Lib AngularJS : renommage de la fonction "intializeClient" en "initializeClient" ("i" manquant !) /!\ BREAKING CHANGE</li>
</ul>
<h3>05/07/2016 : Update SDK 1.8.0.16182</h3>
<ul>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16182 dans les templates</li>
 	<li>SDK : Mise à jour du Code Generator (tri alphabétique des types générés et gestion des MC sans paramètre)</li>
</ul>
<h3>30/06/2016 : Update Client 1.8.0.16182</h3>
<ul>
 	<li>Common / PackageHost.RegisterMessageCallbacks : prise en compte des MC sur des méthodes Statiques et des classes parents (privé ou public)</li>
</ul>
<h3>14/06/2016 : Update Client 1.8.0.16166</h3>
<ul>
 	<li>Common / PackageHost : gestion des MessageCallbacks avec des parametres optionnels et WriteError en cas d'erreur de dispatch</li>
 	<li>Common / StateObject : ajout de la propriété "UniqueId" sur l'object StateObject (= Sentinel/Package/Name)</li>
 	<li>Common / StateObjectCollectionNotifier : l'indexeur est maintenant le UniqueId (et non la fullkey utilisant le :: comme séparateur), idem pour la méthode ContainsStateObject</li>
 	<li>Common / StateObjectCollectionNotifier : ajout d'un indexeur avec parametres optionnels (sentinel/package/name/type) et qui retourne un nouveau StateObjectCollectionNotifier (méthode ContainsStateObject associée)</li>
 	<li>Common / StateObjectCollectionNotifier : la méthode "AddOrUpdate" est maintenant publique</li>
</ul>
<h3>24/05/2016 : Update Console 1.8.0.16145</h3>
<ul>
 	<li>Console : bugfix du redimensionnement du tableau des StateObjects après ouverture de la fenetre de detail ou suppression du filtre</li>
 	<li>Console : ajout d'un bouton "Back" lorsque l'on navigue dans le Type Descriptor</li>
 	<li>Console : refactoring (séparation des controlleurs AngularJS, remplacement des ng-if par des ng-show/hide, revu des filtres de la console, ..)</li>
 	<li>Console : ajout d'un message si une recherche ne donne pas de resultat</li>
 	<li>Console : popup de license n'affiche les attributs que si existant !</li>
 	<li>Console : correction du bug de performance sur l'affichage des icones du package repository</li>
 	<li>Lib AngularJS : on passe un timestamp "global" sur la requete HTTP des icones des packages (prb de performance)</li>
</ul>
<h3>16/05/2016 : Update Console 1.8.0.16136</h3>
<ul>
 	<li>Console / StateObject : description des types des StateObjects (bugfix du getTypeDescriptor sur les MessageCallbackTypes seulement)</li>
</ul>
<h3>05/05/2016 : Release 1.8.0.16126 (Mono compliance)</h3>
<ul>
 	<li>Server : prise en charge du "ReloadConstellationConfiguration" sur Mono (workaround par reflection car Mono n'implemente pas le ConfigurationManager.RefreshSection)</li>
 	<li>Server : ajout d'un Middleware OWIN de néttoyage du Path de la requete pour supprimer les "/" en doublons lorsque fonctionne sous Mono (natif sous .NET Fmk)</li>
 	<li>Server : ajout de la page "Default.html" en page par défaut dans les options du FileServer (en Pascal-case car Mono sensible à la casse)</li>
 	<li>Console : renaming Sentinel.html (en Pacal-case pour l'uniformisation)</li>
</ul>
<h3>01/05/2016 : Release 1.8.0.16122</h3>
<ul>
 	<li>Server / License : nouvelle politique de rejeu si erreur lors du contact avec le serveur de license</li>
</ul>
<h3>28/04/2016 : Release 1.8.0.16119</h3>
<ul>
 	<li>Server / Multitenant : Bootstraper sans durée de vie (remoting) et bug-fix de l'arret du service</li>
 	<li>Server / License : bug-fix doublon du reload de la license (SetLicense remplacé par SaveLicense, Reload via le File Watcher) revu des logs</li>
 	<li>WebConsole : getLicense &amp; checkManagementAccess au démarrage et à chaque connexion (et supression du requestSentinelUpdates au 'reconnecting')</li>
</ul>
<h3>27/04/2016 : Update Server 1.8.0.16118</h3>
<ul>
 	<li>Server / License : date d'expiration des licenses en temps universel (UTC)</li>
</ul>
<h3>23/04/2016 : Update Server 1.8.0.16114</h3>
<ul>
 	<li>Server / SentinelHub : limite basée sur la license (Sentinel.Count)</li>
 	<li>Server / Multitenant : limite basée sur la license (Multitenant.Count) et nommage des domaines</li>
 	<li>Server / ManagementAPI : Get &amp; Set de la license seulement si pas dans un domaine multi-tenant</li>
 	<li>Server / ManagementAPI : SetConfigurationServer : validation du schéma XSD de la configuration XML</li>
 	<li>Server / License : méthodes pour la récupération et contrôle des attributs, ajout de la compagnie et Load à partir d'un fichier ou contenu</li>
 	<li>Server / License : sur le domaine du serveur (!= Constellation(s)) démarrage d'une boucle pour appel au service de license avec récupération/set de la nouvelle license</li>
 	<li>Server / License : license Developer pour un système Windows non Server</li>
 	<li>WebConsole : gestion de la dropdown pour les enums dans les propriétés d'un objet</li>
 	<li>WebConsole : visualisation de la license</li>
 	<li>WebConsole : ajout des n° de lignes sur le config editor</li>
</ul>
<h3>18/04/2016 : Release 1.8.0.16109</h3>
<ul>
 	<li>Common : les constantes sont "Internal" et non plus "Public" (sauf ConstellationDefaultNames)</li>
 	<li>Common : "Internals Visible" pour les assemblies Server.Core et Sentinel.Core</li>
 	<li>Server : ajout du LicenseManager</li>
 	<li>Server : mode Multitenant et "BaseDirectory" configurable dans les AppSettings</li>
 	<li>Server : signature des assemblies Server.Core et Server.Interfaces /!\ Ajouter le PublicToken dans les &lt;configSections&gt; de la configuration du serveur</li>
 	<li>Sentinel : signature de l'assembly Sentinel.Core</li>
</ul>
<h3>13/04/2016 : Update Server 1.8.0.16104</h3>
<ul>
 	<li>Server : chargement des Constellations dans un AppDomain spécifique</li>
 	<li>Server : le serveur est multi-tenant (peut heberger plusieurs Constellations par serveur)</li>
</ul>
<h3>31/03/2016 : Release 1.8.0.16091</h3>
<ul>
 	<li>Console : bugfix - l'invoquation de MC était toujours dans associée à une Saga !</li>
 	<li>Console : Request les PackageDescriptors lors de la reception d'un log commencant par "Declaring PackageDescriptor" pour une sentinelle "Developer"</li>
 	<li>SDK : Mise à jour du package Python 1.8.0.16091</li>
 	<li>SDK : Mise à jour du template Python</li>
 	<li>SDK : Bugfix des boutons de la toolbar lorsqu'il y a plusieurs StartupProjects et/ou des projets rangés dans des SolutionFolders</li>
 	<li>SDK : Refactoring des méthodes d'accès au DTE dans le VS Package</li>
</ul>
<h3>28/03/2016 : Update SDK 1.8.0.16088</h3>
<ul>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16088</li>
 	<li>SDK : Mise à jour du Code Generator (StateObjkectLink avec un ctor sur la sentinelle seulement)</li>
 	<li>SDK : Mise à jour du template Console (suppression des settings de test dans le manifeste)</li>
 	<li>SDK : Séparation des commandes de la toolbar du menu contextuel des projets =&gt; Action sur la toolbar (et shortcut) basé sur le projet "Startup" &amp; menu contextuel sur le 'selected project'</li>
 	<li>SDK : Bugfix des boutons de la toolbar grisée</li>
 	<li>SDK : Refactoring du VS Package</li>
</ul>
<h3>30/03/2016 : Release 1.8.0.16090</h3>
<ul>
 	<li>Server / SentinelHub : bugfix des enregistrements impossibles en cas de reconnection des sentinelles (Registration fail)</li>
 	<li>Console : header "Last registration" sur les sentinelles renommé</li>
 	<li>Console : refresh du StateObject lors de l'ouverture de la popin de visualisation</li>
 	<li>Lib AngularJS : on passe un timestamp sur les requetes HTTP de l'API de Management pouyr éviter les mises en cache (Edge)</li>
</ul>
<h3>28/03/2016 : Update Client 1.8.0.16088</h3>
<ul>
 	<li>Common : le StateObjectNotifier lève un event PropertyChanged sur le DynamicValue également (et pas seulement Value)</li>
 	<li>Common : le StateObjectCollectionNotifier est une ObservableCollection&lt;StateObjectNotifier&gt; (et non plus une List&lt;&gt;)</li>
</ul>
<h3>26/03/2016 : Update SDK 1.8.0.16086</h3>
<ul>
 	<li>SDK : Ajout automatique de la toolbar Constellation au 1er démarrage</li>
 	<li>SDK : Bugfix de l'upload de la configuration du serveur après enregistrement</li>
 	<li>SDK : Réorganisation du code du VS Package</li>
</ul>
<h3>25/03/2016 : Release 1.8.0.16085</h3>
<ul>
 	<li>Common : DeclarePackageDescriptor check la connexion pour ne pas bloquer le thread si pas connecté (Standalone par exemple)</li>
 	<li>Common : le PackageVersion retourne le numéro de version du package tel quel défini dans le manifeste du package. Si il n’y a pas de manifeste, on retourne le PackageAssemblyVersion. <!--EndFragment--></li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16085</li>
 	<li>SDK : Mise à jour du template WPF</li>
 	<li>SDK : Ajout d'une toolbar Constellation</li>
 	<li>SDK : Debugger lancé sans Wait() avec un IDebugEventCallback2 pour traquer la fin du debug</li>
 	<li>SDK : Buttons (Publish, Debug &amp; Code Gen) désactivés si en session en debug ou que le projet n’a pas de PackageInfo</li>
 	<li>SDK : Package automatiquement chargé dans toutes les circonstances</li>
</ul>
<h3>17/03/2016 : Release 1.8.0.16077</h3>
<ul>
 	<li>Common : Le SendMessageProxy accepte un CancellationToken et non un CancellationTokenSource</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16077</li>
 	<li>SDK : Mise à jour du template Console (plus de Demo.cs et settings commentés)<!--EndFragment--></li>
</ul>
<h3>15/03/2016 : Update Console 1.8.0.16075</h3>
<ul>
 	<li>Console : Bugfix sur l'upload de package</li>
</ul>
<h3>14/03/2016 : Update Server 1.8.0.16074</h3>
<ul>
 	<li>Server : suppression du package Microsoft.AspNet.WebApi.Cors</li>
 	<li>Server : utilisation du module Owin CORS (Microsoft.AspNet.Cors) au début du pipeline (pour les hubs SignalR &amp; controlleurs WebAPI)</li>
 	<li>Server : bugfix de l'API de Management non disponible depuis la console en cas de requete CORS (l'AuthentificationMiddleware bloquait la requête 'OPTIONS')</li>
</ul>
<h3>07/03/2016 : Release 1.8.0.16067</h3>
<ul>
 	<li>Common : StateObjectChangedEventArgs déplacé dans le namespace Constellation.Package</li>
 	<li>Common : Ajout d'attribut [Serializable] sur les objets de données</li>
 	<li>Common : revu des commentaires</li>
 	<li>Common : RegisterStateObjectsLink : support des propriétés Static, privées et héritées.</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16067</li>
 	<li>SDK : Mise à jour du Code Generator<!--EndFragment--></li>
</ul>
<h3>04/03/2016 : Release 1.8.0.16064</h3>
<ul>
 	<li>Console : Bugfix sur la page StateObject Explorer suite à l'utilisation des headers dans l'API NG</li>
 	<li>SDK : Mise à jour du générateur de code (problème sur le retour des Task&lt;dynamic&gt; des sagas)</li>
</ul>
<h3>03/03/2016 : Update SDK 1.8.0.16063</h3>
<ul>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16058 &amp; Python 1.8.0.16060</li>
 	<li>SDK : Mise à jour du template Console/Demo pour update 16058</li>
 	<li>SDK : Bugfix du générateur de code (fonctionnement dans un AppDomain enfant pour bypasser le bug lié à Json.net 6.0 chargé dans VS)</li>
 	<li>SDK : Mise à jour de la fenêtre du générateur de code (se souvient de la sélection, async, revu de la gestion des erreurs, progress bar et attente des descriptors)</li>
 	<li>SDK : Mise à jour des templates du générateur de code (SendMessage &amp; Saga liés à la lib 16058, As&lt;T&gt; sur les StateObject, revu des méthodes d'extension, commentaires)</li>
 	<li>SDK : Bugfix sur le 'Cancel' des fenêtres et gestion des erreurs de publication</li>
</ul>
<h3>29/02/2016 : Release 1.8.0.16060</h3>
<ul>
 	<li>Server / ManagementAPI : ajout de la méthode GetServerVersion &amp; controle du XML avant enregistrement dans la méthode SetServerConfiguration</li>
 	<li>Console : affichage de la raison de l'erreur pour le SetServerConfiguration</li>
 	<li>Console : support des CodeTemplates sur les MC avec Saga (mise à jour des CodeTemplates C# &amp; Python)</li>
 	<li>API NG : Mise à jour de méthodes &amp; passage des authorizations dans les headers</li>
</ul>
<h3>27/02/2016 : Update client 1.8.0.16058</h3>
<ul>
 	<li>Common : Supression des methodes CreateSaga &amp; propriété Proxy sur le MessageScope et supression de la méthode SendResponse &amp; propriété ResponseProxy sur le MessageScope /!\ BREAKING CHANGE</li>
 	<li>Common : Ajout de la propriété IsSaga drectement sur le MessageScope (et non seulement le MessageContext)</li>
 	<li>Common : Ajout d'une classe d'extension sur les MessageScope &amp; MessageContext pour ajouter un SagaId (WithSaga), un SagaCallback (OnSagaResponse), recupérer le proxy (GetProxy), générer le scope de response, etc...</li>
 	<li>Common : Ajout des méthodes CreateMessageProxy sur le PackageHost</li>
 	<li>Common : Ajout d'une surcharge RegisterSagaResponseCallback&lt;TResponse&gt; pour convertion de la réponse en TResponse</li>
 	<li>Common : la classe SendMessageProxy retourne une Task&lt;TResponse&gt; pour l'obtention de la réponse si c'est le scope est une saga (avec utilisation d'un CancellationToken passé en argument)</li>
 	<li>Common : la classe SendMessageProxy défini le MessageContext si passé dans le dernier parametre (permettant de recuperer le context si utilisation des Tasks)</li>
 	<li>Common : la classe SendMessageProxy crée automatique une saga sur le scope courant si un type de retour est spécifié dans la méthode dynamique invoquée et convertie l'object de réponse avec ce type</li>
 	<li>Common : Ajout de la classe interne ObjectConverter pour la convertion des objet Newtonsoft en objet .NET (utilisé par le GetValue&lt;T&gt; des SO, les SagaResponseCallbacks, les MessagesExternsions &amp; RegisterMessageCallbacks&lt;TMessage&gt;)</li>
</ul>
<h3>24/02/2016 : Release 1.8 RC6 (1.8.0.16055)</h3>
<ul>
 	<li>Common : revu de la méthode AttachToParent pour detecter le cas où le parent n'est pas trouvé (mort de la sentinelle au démarrage du package créant des packages orphelins)</li>
 	<li>Common, Sentinel &amp; Server : rollback complet sur l'arret d'un package (abandon du NamedPipe non compatible Linux). Retour "PackageControlAction" côté client sur le ConstellationHub</li>
 	<li>Server / SentinelHub &amp; Sentinels : revu du process de "Registration" : renvoi un "bool" à la sentinel qui retente si pas reussi avec contrôle si déjà connecté</li>
 	<li>Server / SentinelHub : maintient une liste des connections et des sentinels. Même si déconnecté, on la garde à dispo dans le ControlHub.</li>
 	<li>Server : les "PackageControlAction" log une erreur si la sentinelle cible n'est pas connectée</li>
 	<li>Sentinels : dispatching des "PackageControlAction" dans des threads à part</li>
 	<li>Sentinel UI : revu du systeme de fermeture de l'application Windows (basée sur le CloseReason)</li>
 	<li>Console : filter &amp; orderBy via les filtres natifs d'Angular après convertion des objets en tableau (ajout du filtre toArray)</li>
 	<li>Console : 'twilight' est le theme par defaut pour CodeMirror</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16055</li>
 	<li>SDK : Correction / réécriture de la gestion de la StatusBar de VS</li>
</ul>
<h3>23/02/2016 : Release 1.8.0.16054</h3>
<ul>
 	<li>Console : SetServerConfiguration PUIS si OK, Reload Configuration (action par défaut)</li>
 	<li>Console : ajout des "orderBy" sur l'ensemble des pages (sentinels, packages, stateobjects, message callbacks, packages repository)</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16053 dans les templates</li>
 	<li>Utilisation des MessageBox de VisualStudio (et non de l'API Winform)</li>
 	<li>Revu des messages du processus de publication</li>
</ul>
<h3>22/02/2016 : Release 1.8 RC5 Refresh (1.8.0.16053)</h3>
<ul>
 	<li>Common : bugfix NamedPipe pour l'ordre de "shutdown" se basait sur le PackageName au lieu du PackageInstanceName</li>
 	<li>SDK : Mise à jour du template Package Console avec classe Demo</li>
 	<li>SDK : Mise à jour du package Constellation 1.8.0.16052 dans les templates</li>
 	<li>SDK : Bugfix sur l'ouverture de la Console Web après publication</li>
 	<li>SDK : Sauvegarde du serveur utilisé pour la publication et nom du package filename</li>
 	<li>SDK : Revu de l'UI de la publication et gestion des erreurs plus fine</li>
</ul>
<h3>21/02/2016 : Release 1.8 RC5 (1.8.0.16052)</h3>
<ul>
 	<li>Common : passage d'un nouvel header HTTP lors d'une reconnection au serveur Constellation (permet d'indiquer qu'il s'agit d'une reconnexion en cas de redémarrage du serveur)</li>
 	<li>Common : un seul Request d'un même identifiant de StateObject lors du rafraichissement des SOLink à la reconnexion d'un package</li>
 	<li>Server / ConstellationHub : si reconnection (header "IsReconnection"), on ne supprime pas les StateObjects ni ne repond au RequestLastStateObjects</li>
 	<li>Server / ControlHub : bugfix sur le ReportPackageStateToControlHub où le PackageDescription n'est pas encore fourni par la sentinel (ajout d'une description partielle)</li>
</ul>
<h3>20/02/2016 : Release 1.8 RC4 Refresh 2 (1.8.0.16051)</h3>
<ul>
 	<li>Server : bug-fix du "RequestLastStateObject" qui renvoyait toujours 0 StateOBjects (car purge avant envoi !)</li>
 	<li>Sentinels : bug-fix sur les NamedPipes de contrôle des sentinels (multi-instances impossible car même nom de pipe)</li>
 	<li>Sentinels : on peut lancer plusieurs instances des sentinels si (et seulement si) le filename du processus est différent (donc configuration différente)</li>
 	<li>Console : Notification à la réception d'un message si réponse reçu après plus de 5 secondes après envoi</li>
</ul>
<h3>19/02/2016 : Update SDK 1.8.0.16050</h3>
<ul>
 	<li>SDK : Bugfix sur la lecture des anciennes config de serveur du SDK 1.7</li>
</ul>
<h3>17/02/2016 : Release 1.8 RC4 Refresh (1.8.0.16048)</h3>
<ul>
 	<li>Server : refonte de la homepage du serveur</li>
 	<li>Console : ajout du favicon sur la page de login</li>
</ul>
<h3>15/02/2016 : Release 1.8 RC4 (1.8.0.16046)</h3>
<ul>
 	<li>Common : renaming du namespace "Constellation.Host" par "Constellation.Package" /!\ BREAKING CHANGE</li>
 	<li>Common : renaming des méthodes de gestion des MessageCallbacks</li>
 	<li>Common : gestion des fonctions de MessageCallback (le "return" permet de renvoyer l'objet de reponse d'une saga)</li>
 	<li>Common &amp; Server : supression du "PackageControlAction" sur le PackageHost/ConstellationHub =&gt; les ordres d'arret sont gérés par la sentinel via un NamedPipe entre les packages et la sentinelle</li>
 	<li>Sentinels : exposition d'un serveur "NamedPipe" permettant d'envoyer des "PackageControlAction" au processus de la sentinel</li>
 	<li>Sentinels : si la sentinel (Service ou UI) est déjà démarré et que la "Command Line Arguments" contient un "PCA" nom de package, on l'envoi en NamedPipe</li>
 	<li>Sentinels : envoi de l'ordre d'arret du package depuis la sentinel aux packages via un NamedPipe</li>
 	<li>Sentinels : "CurrentDirectory" est celui du répertoire de la sentinel (pour que les liens relatifs fonctionnent bien)</li>
</ul>
<h3>11/02/2016 : Release 1.8 RC3 (1.8.0.16042)</h3>
<ul>
 	<li>Schemas XSD : ajout d'un namespace pour le PackageManifest, ConstellationSettings &amp; ConstellationConfiguration</li>
 	<li>Common : ne pas re-forcer les Requests/Subscribes de StateObjects à la 1ere connexion après avoir déjà enregistré des StateObjectsLinks</li>
 	<li>Server : ajout de la méthode CheckAccess sur la Constellation API &amp; Consumer API</li>
 	<li>Server / ControlHub : aucune action sur le OnConnected &amp; OnDisconnected si la sentinel est "Developer"</li>
 	<li>Server / Management API : ajout des méthode GetSentinels, AddSentinel, GetCredentials et ReloadServerConfiguration</li>
 	<li>WebConsole : possibilité de se connecter directement sans page de login en passant l'AccessKey dans les parametres de l'URL</li>
 	<li>SDK : Revu du menu et des icones</li>
 	<li>SDK : Refonte de la fenêtre de "Publish" avec mode "Local" ou "Upload sur Server" (Management API)</li>
 	<li>SDK : Process de build : création du package via le code et non via 7za</li>
 	<li>SDK : Process de build : supprime les settings "IsRequiered" du package &amp; copie le PackageManifest si pas dans l'Output directory</li>
 	<li>SDK : Manage Server : Check access + ajout de l'URI de la Console + generator d'access key à partir d'un login/pass + nouveau système de test de la connection</li>
 	<li>SDK : Ajout d'un menu item pour ouvrir la Console (avec page de selection si plusieurs)</li>
 	<li>SDK : Ajout d'un menu item pour éditer la configuration du serveur &amp; upload lors de l'enregistrement</li>
 	<li>SDK : Ajout des ConstellationSettings dans le App.config des templates + activation de la documentation XML</li>
 	<li>SDK : Enrichissement du template "Console" avec du code sample</li>
 	<li>SDK : Ajout des schémas XSD (Setting &amp; Manifest) dans l'installation du SDK</li>
 	<li>SDK : Code generator : prend en charge les "-" dans le nom des packages</li>
</ul>
<h3>09/02/2016 : Release 1.8 RC2 (1.8.0.16040)</h3>
<ul>
 	<li>Common : WriteInfo du chargement des settings dans le App.local seulement si non StandAlone</li>
 	<li>Common : ajout du "ConsumerHttp" dans les "SenderType"</li>
 	<li>Common : bugfix sur le COntrolManager en standalone utilisait le nom de la sentinelle "Consumer" au lieu de "Controller"</li>
 	<li>Server : ajout du controlleur HTTP "Consumer" : WebAPI REST de consommation (Send/Receive message Requets/Subscribe StateObject)</li>
 	<li>Server : refactorisation du code dans la classe WebApiManager pour la gestion des fonctionnalités mutualisées entre le ConstellationController &amp; ConsumerController</li>
 	<li>WebConsole : le StateObject Explorer utilise la Consumer Web API pour recuperer l'ensemble des StateObjects du serveur (plus performant)</li>
 	<li>WebConsole : bugfix des inputs "readonly" après avoir pris le focus sur la Console log (le terminal bloquait le "Keypress/Keydown")</li>
 	<li>WebConsole : bugfix si constellationUri non défini et que serveur sur un sub-directory (le origin ne tennait pas compte du repertoire)</li>
 	<li>WebConsole : notification des messages entrants seulement lorsque l'on a recu la réponse d'une saga</li>
 	<li>WebConsole : le lien "Switch theme" est invisible sur le Configuration Editor (probleme de classe CSS)</li>
 	<li>WebConsole : ajout du n° de version dans la meta "generator"</li>
</ul>
<h3>08/02/2016 : Release 1.8 RC1 (1.8.0.16039)</h3>
<ul>
 	<li>PackageManifest : erreur de casse sur l'attribut "PackageUrl"</li>
 	<li>Common : bugfix du bloquage d'un package si pas de "constellationSection" dans sa configuration local</li>
 	<li>Common : gestion des erreurs lors de la lecture des settings du fichier de configuration local &amp; manifest</li>
 	<li>Sentinel : bugfix sur le format de l'URI du serveur (le '/' à la fin de l'URI est optionnel)</li>
 	<li>WebConsole : erreur de frappe dans le message d'erreur en cas de connexion echouée</li>
 	<li>WebConsole : bugfix si constellationUri non défini et que serveur sur un sub-directory (le origin ne tennait pas compte du repertoire)</li>
 	<li>Client libs : bugfix demandant le ControlHub sur le hub Consumer</li>
 	<li>SDK : Compatible Visual Studio 2012, 2013 et 2015 toutes éditions</li>
 	<li>SDK : Mise à jours des packages Nuget</li>
 	<li>SDK : Mise à jours des templates</li>
</ul>
<h3>07/02/2016 : Release 1.8 Beta 3 (1.8.0.16038)</h3>
<ul>
 	<li>Global : revu des AssemblyInfos avec description et titre</li>
 	<li>PackageManifest : ajout d'un attribut "PackageUrl"</li>
 	<li>PackageManifest : attribut "RestoreStateObjects" renommé en "RequestLastStateObjectsOnStart" /!\ BREAKING CHANGE</li>
 	<li>PackageManifest / Settings : ajout des "defaultContent", attributs "IgnoreLocalValue" (ignore la valeur dans le fichier de config local) et "IgnoreDefaultValue" (ingore la default value du manisfest) et nouveau type de settings "XmlDocument" et "JsonObject"</li>
 	<li>Common : ajout de la section ConstellationSettings dans le App.config local =&gt; identique à la config serveur (setting, content &amp; data) /!\ LES APPSETTINGS et GETSECTION en local ne sont plus utilisés !!!</li>
 	<li>Common : evenement "RestoreStateObjects" renommé en "LastStateObjectsReceived" /!\ BREAKING CHANGE</li>
 	<li>Common / Settings : Charge les settings du serveur, puis comble avec les valeurs dans le App.config local (si pas ignoré) puis avec les valeurs par defaut du manifest (si pas ignoré)</li>
 	<li>Common / Settings : nouvelles méthodes ContainsSetting et GetSettingValue sans générique (= string par defaut)</li>
 	<li>Common / Settings : nouvelles méthodes pour récupérer les settings "AsJsonObject" (dynamic ou de T) et "AsXmlDocument" (ajout des TryGetXXXX associés)</li>
 	<li>Common / Settings : "GetSettingContent&lt;T&gt;" renommé en "GetSettingAsConfigurationSection" /!\ BREAKING CHANGE</li>
 	<li>Server : refonte de la gestion des logs (tout est géré par NLog avec un Target vers le hub Constellation)</li>
 	<li>Server : Adaptation du moteur d'autorisation (CheckAccess) plus strict</li>
 	<li>Server / Configuration : l'element &lt;content&gt; sous les Settings peut accepter du texte ou un CDATA en plus du XML.</li>
 	<li>Server / Constellation Hub &amp; Controller : ajout d'une méthode "PurgeStateObjects" pour permettre les packages à supprimer ses StateObjects</li>
 	<li>Server / Control Hub : la méthode "PurgeStateObjects" peut être invoquée même si le package est démarré</li>
 	<li>Server / Control Hub : les packages passent en "Stopped" lorsqu'ils sont deconnectés avec leurs sentinelles deconnectées</li>
 	<li>Server / Management API : ajout d'un attribut "enableManagementAPI" sur les credentials pour l'autorisation d'accès à l'API de Management</li>
 	<li>Server / Management API : renommage "DevelopperController" en "ManagementController" (= Management API)</li>
 	<li>Server / Management API : ajout des méthodes GetPackageIcon, GetPackage(s), CheckAccess</li>
 	<li>Server : ajout du Allow-Origin All sur les Web API (Constellation &amp; Management Web APIs)</li>
 	<li>WebConsole : Request la liste des packages si on recoit l'état d'un package inconnu</li>
 	<li>WebConsole : Ajout des Warnings dans les notifications</li>
 	<li>WebConsole : boutons pur Subscribe/Unsubscribe en temps reel au SO Refresh et Delete sur la modal de détail des StateObjects</li>
 	<li>WebConsole : reorganisation de la WebConsole basée sur le routeur de AngularUI (ui.router)</li>
 	<li>WebConsole : gestion des liens unique par page historique du navigateur</li>
 	<li>WebConsole : ajout d'une modal pour supprimer les SO d'un package (arreté) ou tous les SO expirés</li>
 	<li>WebConsole : ajout d'un bouton pour copier la valeur d'un StateObject dans le presse-papier</li>
 	<li>WebConsole : ajout d'un panneau de filtre pour filtrer les messages de log de la console</li>
 	<li>WebConsole : la configuration de l'UI du serveur devient optionnel (par défaut basé sur le location.origin de la WebConsole, dans le cas d'un self-host)</li>
 	<li>WebConsole : redimensionnement du terminal lors du resize et en fonction de l'état du menu</li>
 	<li>WebConsole : ajout d'une modal de description des types &amp; ajout sur la page de Messages et les StateObjects</li>
 	<li>WebConsole : ajout d'une modal de generation des ligne de code pour l'envoi de message</li>
 	<li>WebConsole : gestion de la Management API et désactivation du menu redirection si pas d'accès</li>
 	<li>WebConsole : implémentation du ConfigurationEditor pour l'édition du fichier de configuration</li>
 	<li>WebConsole : ajout de shortcuts (Crtl+S pour l'enregistrement de la config, F11 pour l'editeur en fullscreen et Ctrl+R poru refresh les SO)</li>
 	<li>WebConsole : Gestion du return_url dans la page de login</li>
 	<li>WebConsole : ajout du Packages Repository (Upload &amp; listing des packages)</li>
 	<li>WebConsole : ajout d'une checkbox "With Saga" affichage du ResponseType sur les MessageCallbacks</li>
 	<li>WebConsole : notification des messages de reponse des sagas dans la barre du haut avec modal de visualisation du contenu des messages</li>
 	<li>Librairie ngConstellation : ajout d'une factory pour le service de Management</li>
 	<li>Nouvelle icone sur les EXE et Sentinel UI</li>
</ul>
<h3>24/01/2016 : Release 1.8 Beta 2 (1.8.0.160024)</h3>
<ul>
 	<li>WebConsole : Nouvelle WebConsole version 2 basée sur BootStrap &amp; AngularJS</li>
 	<li>Server / ControlHub : RefrehServerConfiguration renommé en ReloadServerConfiguration et applyConfiguration en deployConfiguration (updated sur le ControlManger, libs JS &amp; WebConsole)</li>
 	<li>Server / ControlHub : ReloadServerConfiguration : gestion des erreurs en cas d'erreur de lecture/application du fichier de configuration</li>
 	<li>Server &amp; Sentinel : Ajout de l'attribut "AutoStart" pour indiquer si le package doit démarrer automatiquement lors de la réception des paquets sur la sentinelle</li>
</ul>
<h3>21/01/2016 : Release 1.8 Beta 1.1 (1.8.0.16021)</h3>
<ul>
 	<li>Sentinel : gestion des erreurs lorsqu'un répertoire local d'un package ne peut être supprimé/créé (et passe au suivant)</li>
 	<li>Sentinel : CurrentDirectory du package est son répertoire d'extraction (et non le répertoire de la sentinelle)</li>
</ul>
<h3>19/01/2016 : Release 1.8 Beta 1 (1.8.0.16019)</h3>
<ul>
 	<li>Upgrade JSON.NET 8.0</li>
 	<li>PackageManifest : les types des settings sont dans une Enum ajout d'un attribut DefaultValue</li>
 	<li>Common : chargement la DefaultValue du PackageManifest d'un Setting qui n'est pas trouvé dans sa config (Log un Warning si pas de DefaultValue et une Error si IsRequired)</li>
 	<li>Common : Renaming ScopeType.Groups en ScopeType.Group (au singulier) pour harmoniser ! /!\ BREAKING CHANGE</li>
 	<li>Common / StateObject : les Metadatas sont des dictionnaires de String/Object (et non plus String/String) /!\ BREAKING CHANGE</li>
 	<li>Common : RegisterStateObjectLinks réalisé automatiquement sur la classe IPackage au démarrage</li>
 	<li>Common : PackageHost.AttachMessageCallbacks renommé en PackageHost.RegisterMessageCallback /!\ BREAKING CHANGE</li>
 	<li>Common : objet StateObject et StateObjectsLink &amp; Notifier ainsi que les EventsArgs pour les Messages &amp; StateObjects ont changé de namespace (ne dépend plus de .Control) /!\ BREAKING CHANGE</li>
 	<li>Common : le ControlManager n'a plus accès à l'envoi et la reception de message ni d'intérrogation de StateObject</li>
 	<li>Common : la notion des StateObjectsLink &amp; Notifier est déplacé dans le PackageHost /!\ BREAKING CHANGE</li>
 	<li>Common : supression des balises XML dans les descriptions des MC avec extraction de l'attribut "cref"</li>
 	<li>Common &amp; Server / ConstellationHub : AddToGroup/RemoveToGroup deviennent SubscribeMessage/UnsubscribeMessage</li>
 	<li>Common &amp; Server : Renaming sentinel virtuel : "Debugger" to "Developer"</li>
 	<li>Server / ConstellationController : convertion de la "value" d'un PushStateObject avec le Type précisé en parametre de la requete (FQN du type .NET)</li>
 	<li>Server / DeveloperController : ajout du controlleur HTTP "DeveloperController" permettant d'uploader un package et la lecture/écriture de la configuration du serveur</li>
 	<li>Server / ConstellationHub : ajout des RequestStateObjects &amp; (Un)SubscribeStateObjects</li>
 	<li>Server / ControlHub : supression de l'envoi &amp; reception de message et Request/Subscribe de SO</li>
 	<li>Server / ConsumerHub : ajout d'un nouveau hub : le ConsumerHub avec envoi &amp; reception de message et Request/Subscribe de SO</li>
 	<li>Server / Config : ajout des schemas pour l'authorization des credentials</li>
 	<li>Server / AccessKey : ConsumerHub, ConstellationHub &amp; ControllerHTTP : SubscribeMessage, SendMessage, RequestStateObject &amp; SubscribeStateObject dépends des authorisations configurées</li>
 	<li>Server : ajout du "FileServer" permettant de servir des pages statiques (comme la WebConsole). Configurable avec la section &lt;fileServer&gt;</li>
 	<li>Server / Optim : Supression du flag "Prefer-32bits" en compilation RELEASE pour tourner en 64 bits par défaut (bugfix)</li>
 	<li>Server : Bugfix : purge des SO d'un package à son (re)démarrage</li>
 	<li>Server : envoi des WriteLogs dans un logger "PackageLog.SentinelName.PackageName" permettant d'extraire les logs d'un package en particulier</li>
 	<li>Server : Refactoring &amp; renaming</li>
 	<li>Sentinel : Warning si la version du PackageManifest est différente de la version courante de Constellation</li>
 	<li>API Javascript &amp; NG : migration vers les hubs de la 1.8 (createConstellationConsumer et createConstellationController), ajout des sendMessageWithSaga et requestSubscribeStateObject</li>
 	<li>WebConsole : Migration 1.8</li>
 	<li>WebConsole : Correction des liens du menu</li>
 	<li>WebConsole : Correction de l'Invoke sur les MC avec objets complexes</li>
</ul>
<h3>15/12/2015 : Release 1.7.6.15349</h3>
<ul>
 	<li>Upgrade JSON.NET 7.0.1 &amp; NLOG 4.2.2 (Client, Sentinel &amp; Server) et supression des packages inutilisés</li>
 	<li>Server : merge de la branche "StateObjectProvider" -&gt; gestion des SO par providers</li>
 	<li>Server / StateObjectProvider : InMemoryStateObjectProvider complétement réécrit pour être plus performant et safe</li>
 	<li>Server / StateObjectProvider : ajout d'une 1ere version (POC) du RedisStateObjectProvider basé sur Redis pour le stockage des SO</li>
 	<li>Server / Optim : Supression des groupes de connection (ConstellationGroupManager) des connections du ConstellationHub &amp; ControlHub lors des deconnections -&gt; optimisation et fix de l'emballement du CPU dans le temps</li>
 	<li>Server / Optim : Supression du flag "Prefer-32bits" pour tourner en 64 bits par défaut</li>
</ul>
<h3>15/09/2015 : Update Server 1.7.5.15258</h3>
<ul>
 	<li>Server / Middleware : refactorisation de la création des réponses dans les Middlewares OWIN avec renvoi du Content-Type &amp; Content-Length à chaque réponse (bug requete mal formée)</li>
 	<li>Server : optimisation/refactorisation de l'extraction des valeurs dans les headers HTTP correction des GetSentinelName/GetPackageName lorsque passés en QueryString</li>
 	<li>Server : Correction du Sender.FriendlyName lorsque le message est envoyé depuis le ControlHub</li>
</ul>
<h3>14/09/2015 : Update Server 1.7.5.15257</h3>
<ul>
 	<li>Client lib : ajout des attributs DataContract/DataMember sur la classe StateObjects pour le contrôleur HTTP/REST</li>
 	<li>HTTP Controller : ajout du paramètre "limit" (nb max de résultat dans la réponse) pour le GetMessage &amp; GetStateObjects ! Paramètre optionnel, par défaut à 0 (= pas de limite)</li>
 	<li>HTTP Controller :le paramètre "timeout" peut être passé à 0 pour renvoi de la réponse immédiate (pas de long polling)</li>
 	<li>HTTP Controller : la Subscription aux SO supprime les anciens SO si pas dépilé (on ne garde que la dernière valeur du SO pour éviter l'accumulation) ajout d'un lock lors de l'enrichissement des Subscriptions</li>
</ul>
<h3>17/08/2015 : Update SDK 1.7.15229</h3>
<ul>
 	<li>SDK : prise en compte du namespace dans le template T4 &amp; n° de version du générateur</li>
 	<li>SDK : Ajout de méthodes d'extension pour créer un scope typé de chaque package pour chaque Enums Sentinels/Allpackages/MyPackage</li>
</ul>
<h3>06/07/2015 : Update Client 1.7.5.15187</h3>
<ul>
 	<li>Le GetValue&lt;T&gt;() d'un StateObject prend en compte des JArray &amp; JObject</li>
</ul>
<h3>28/05/2015 : Release 1.7.5.15178</h3>
<ul>
 	<li>Server : autorisation du fichier de référence des hubs signalr (/signalr/hubs) lorsque compilé en debug</li>
 	<li>Ajout de la librairie Constellation for AngularJS page de demo</li>
 	<li>Update de la librairy Constellation for Javascript (update des descriptors)</li>
 	<li>WebConsole : Migration des pages du Control Center sur la lib ConstellationNG</li>
 	<li>Changement de licence (Apache vers propriétaire)</li>
</ul>
<h3>23/04/2015 : Update SDK 1.7.5.15113</h3>
<ul>
 	<li>SDK : Update du package PythonProxy 1.3.1 avec reférence vers Constellation 1.7.5 signé</li>
 	<li>SDK : Bugfix du template Python (génére un EXE et non un DLL)</li>
 	<li>SDK : Bugfix du PublishPackage : pause avant le ZIP (évite les ZIP vide) + bugfix sur la sauvegarde du path de publication par projet + clean de la progress bar lors des erreurs et fermeture de la fenêtre</li>
</ul>
<h3>14/04/2015 : Release 1.7.5.15104</h3>
<ul>
 	<li>Server / Controller HTTP : bugfix PushStateObject en passant le "type" ajout des metadatas dans la méthode POST ajout du lifetime en GET &amp; POST</li>
</ul>
<h3>09/04/2015 : Release 1.7.5.15099</h3>
<ul>
 	<li>Client lib : ajout des propriétés SentinelName &amp; PackageInstanceName sur le PackageHost</li>
 	<li>Client lib / ControlManager : modification de la signature du UpdatePackageDescriptor</li>
 	<li>Server : Modification de la réponse du RequestPackageDescriptor -&gt; le UpdatePackageDescriptor contient le nom du package (instance) et la propriété Descriptor</li>
</ul>
<h3>24/03/2015 : Update Client 1.7.4.15083</h3>
<ul>
 	<li>Signature des Client lib</li>
 	<li>Ajout de l'attribut DebuggerNonUserCode sur la 2ème surcharge de la méthode Start</li>
 	<li>Selection de la InnerException lors d'une AggregateException dans le Start pour donner plus de detail !</li>
 	<li>Bugfix sur le SerializationHelper : les methodes From/To File ne prennaient pas en compte le serializer passé explicitement</li>
</ul>
<h3>13/03/2015 : Release 1.7 RC3.1 (1.7.4.15072)</h3>
<ul>
 	<li>Global : ajout d'un LevelLog sur les WriteLog de la Constellation</li>
 	<li>Client lib : nouvelles méthodes : WriteDebug/WriteInfo/WriteWarn/WriteError</li>
 	<li>Client lib : /!\ Breaking changes : plus de WriteLog(string) !!! (utiliser WriteInfo)</li>
 	<li>Client lib : exclusion du "Type" lors d'un PushStateObject pour les objets anonymes</li>
 	<li>Server/ControlHub : push la SentinelInfo au ControlHub après un Register</li>
 	<li>SDK : Mise à jour du Code Generator</li>
 	<li>SDK : Mise à jour de la libraire Constellation 1.7.4.15072</li>
</ul>
<h3>12/03/2015 : Release 1.7 RC3 (1.7.4.15071)</h3>
<ul>
 	<li>Client lib : ajout d'une 3ème methode sur l'interface IPackage : "OnPreShutdown" appelé avant la coupure de connexion</li>
 	<li>Client lib : la méthode Shutdown appelle le OnPreShutdown, puis coupe la connection au(x) hub(s), puis appelle le OnShutdown et pour finir Kill le process</li>
 	<li>Client lib : ajout d'une classe abstraite "PackageBase" implémentant IPackage sans code</li>
 	<li>Client lib : correction tu typage du StateObject lors du PushStateObject (bugfix)</li>
 	<li>Client lib : suppression des méthodes "Restart" et "Kill"</li>
 	<li>Client lib : suppression du Singleton sur le ControlManager pour permettre plusieurs instances ! Par contre, une seule instance référencée dans le PackageHost</li>
 	<li>Client lib : le PackageManisfestReader peut attraper les exceptions et renvoyer un null en cas d'erreur de lecture</li>
 	<li>Server / CLient lib : suppression des PackageStates "Connected/Disconnected" et ajout d'une propriété "IsConnected" sur les PackageInfos pour différencier l'état du process vs le status de connection du package</li>
 	<li>ControlHub : RequestSentinelUpdates (Update chaque Sentinel) &amp; RequestSentinelList (demande à la liste des sentinels connectées)</li>
 	<li>ControlHub : Connected renommée en IsConnected sur les Sentinels pour harmonisation avec les Packages States</li>
 	<li>Sentinel : Try/Catch de la lecture du manifest avec log ensuite que le démarrage du processus</li>
 	<li>Sentinel : Revu de la procedure d'arret : on attends tant que le process soit tué avec un timeout défini par défaut à 10secondes (et non on attends X secons et on Kill)</li>
 	<li>Sentinel : Ajout de constantes bugfix sur la libération du process qui ne doit pas redémarré après un crash (rester en Stopping)</li>
 	<li>WebConsole : ajout des icones Connected/Disconnected actualisation de toutes les infos sur les packages au PackageState (Connecté, Versions, etc..)</li>
</ul>
<h3>11/03/2015 : Release 1.7 RC2 (1.7.3.15070)</h3>
<ul>
 	<li>Sentinel UI : affichage dans le systray clé de registre dans LocalMachine (et non LocalUser)</li>
 	<li>Sentinel Core : log review mini pause de 10ms avant le restart</li>
 	<li>Server/Client lib : l'attribut 'restoreStateObject' est déporté de la config du serveur au manifest du Package</li>
 	<li>Server : scindage des méthodes d'extensions dans une nouvelle classe "ConstellationExtensions"</li>
 	<li>Server : ajout de la lib 'SharpZip' pour lecture des manifests dans le ConstellationHelper revision du Helper (tout public)</li>
 	<li>Server : revision de la gestion des connections/déconnections sur le ControlHub (plus de "bug" où les packages sont vus comme Disconnected)</li>
 	<li>Server/ControlHub : ajout d'un Etat "IsConnected" sur les PackageStates et "Unknown" (état normalement impossible) attribution d'un int</li>
 	<li>PackageInfo : State sous forme d'Enum côté Server &amp; CLient Lib mais toujours transferé en string (plus simple pour le JS) ajout du ConnectionId SignalR</li>
 	<li>ControlHub : renommage des propriétés string "Sentinel" &amp; "Package" en "SentinelName" &amp; "PackageName" pour rester uniforme</li>
 	<li>SDK : Mise à jour du Code Generator</li>
 	<li>SDK : Mise à jour de la libraire Constellation 1.7.4.15070<!--EndFragment--></li>
</ul>
<h3>09/03/2015 : Release 1.7 RC 1.2 (1.7.2.15068)</h3>
<ul>
 	<li>Server : modification du schéma de configuration -&gt; il n'y a plus d'attribut "SettingsGroupName" sur le Package, mais une balise "&lt;import&gt;" dans les Settings pour pouvoir importer plusieurs groupes (idem sur les groupes qui peuvent aussi importer eux même)</li>
 	<li>SDK : Ajout du Code Generator
<ul><!--EndFragment--></ul>
</li>
</ul>
<h3>06/03/2015 : Release 1.7 RC 1.1 (1.7.2.15066)</h3>
<ul>
 	<li>Sentinel Service : paramètre "Console" pour démarrer le Service en console (pour Rpi v1)</li>
 	<li>Sentinel Core : controle du process avant le ReportUsage</li>
 	<li>Server : Lors des reconnexions, le ReportPackageVersion redéfini bien le status du package à "Started"</li>
 	<li>Server : Ajout de la méthode POST "DeclarePackageDescriptor" sur l'interface REST</li>
 	<li>Client lib : le XmDocumentationReader extrait le InnerXml pour conserver les balises XML dans les commentaires</li>
</ul>
<h3>06/03/2015 : Release 1.7 RC1 (1.7.2.15065)</h3>
<ul>
 	<li>Ajout d'un attribut StateObjectKnownTypes sur le package pour enregistrer des types connu dans des StateObjects</li>
 	<li>Describe des StateObjects au Start connecté ou non</li>
 	<li>AttachMessageCallback seulement si connecté</li>
 	<li>DescribeStateObjectTypesFromAssembly =&gt; GetEntryAssembly par defaut (et non le CallingAssembly)</li>
 	<li>XmlDocumentationReader - bugfix : les Enums &amp; Types n'exploitent plus le DeclaringType</li>
 	<li>Server : log des erreurs au OnPackageConnected</li>
 	<li>Sentinel : Plus de ReportPackageStatus lorsque pas Connected</li>
</ul>
<h3>05/03/2015 : Release 1.7 Beta 5 (1.7.2.15064)</h3>
<ul>
 	<li>Ajout d'un attribut "StateObject" sur des classes pour décrire le SO au serveur (descriptor)</li>
 	<li>Description des StateObjects en plus des MessageCallbacks</li>
 	<li>Chargement des commentaires XML les enums, propriétés, types et méthodes dans les descriptors</li>
 	<li>Ne prends pas en cas les types primitifs et propriété avec JsonIgnore dans les descriptors</li>
 	<li>Ajout automatique du type dans le PushStateObject</li>
 	<li>Renommage des MessageCallbackDescriptions en PackageDescriptor revu de la strucutre du container</li>
 	<li>Renommage du message Key des Saga en "__Response"pour eviter des conflit !</li>
 	<li>Lifetime des StateObject en seconde</li>
 	<li>Correction de la SentinelUI : se "cache" bien au démarrage</li>
</ul>
<h3>04/03/2015 : Release 1.7 Beta 4 (1.7.2.15063)</h3>
<ul>
 	<li>Renommage du nom reservé "Web" à "ControlHub" pour la connexion au control hub (accès Web par exemple)</li>
 	<li>Renommage MethodCallback en MessageCallback</li>
 	<li>Client lib : AttachMessageCallback n'enregistre qu'un seul callback par method lorsqu'on invoque plusieur fois la méthode</li>
 	<li>Client lib : gestion des messages avec plusieurs arguments de type complexe (ou mixte Complexe/Primitif)</li>
 	<li>Client lib : refonte complète du MessageCallbackDescription</li>
 	<li>Client lib/ControlManager : possibilité de l'utiliser comme une API standalone en dehors du context d'un package ! (= API .NET)</li>
 	<li>Client lib : Le GetSettingContent est basé sur un ConfigurationSection en mode standalone fait un GetSection sur le ConfiguraitonManager renvoit un default(TConfigurationSection) si erreur</li>
 	<li>Client lib : IsStandalone à "True" si la sentinel est le Debugger</li>
 	<li>Client lib : PackageHost.Start est taggé "DebuggerNonUserCode"</li>
 	<li>Sentinel : possibilité d'overrider le temps d'attente avant Kill du process (1 second par défaut mais pousser à 5 seconds sur des devices comme un RPi)</li>
 	<li>Server : Suppression de l'attribut "EnableWebAccess"</li>
</ul>
<h3>03/03/2015 : Release 1.7 Beta 3 (1.7.1.15062)</h3>
<ul>
 	<li>Update du schema PackageManifest corrigé</li>
 	<li>Sentinel : détermination du nom de l'executable dans l'ordre &gt; ExecutableFilename dans le manifest &gt; Name du manifiest "exe" &gt; Nom du pakcage ".exe"</li>
 	<li>Sentinel : stop le package et log une erreur sur la Constellation si le fichier du process n'est pas trouvé</li>
 	<li>Server : HomepageMiddleware traite les requetes avant le middleware de l'authentification (plus performant en evitant le double test sur le path) délivre le "favicon.ico" avec l'icone de la Constellation</li>
 	<li>Server : report de la deconnexion d'un package lorsque stopCalled n'est pas vrai lock sur l'accès des sentinels</li>
 	<li>Server/ControlHub : mise à jour du PackageDescription lors de la reconnexion d'une sentinel report des Versions alors que les packages sur le ControlHub ne sont pas encore déclarés</li>
 	<li>Client lib : Ajout de Log avant les OnStart &amp; OnShutdown des packages</li>
</ul>
<h3>02/03/2015 : Release 1.7 Beta 2 (1.7.1.15061)</h3>
<ul>
 	<li>Deux librairies Common -&gt; l'une en 4.0 et l'autre 4.5 : les deux livrées dans le même packages Nuget</li>
 	<li>La Sentinelle est buildée en .NET 4.0</li>
 	<li>Révision de la Sentinelle UI avec fenêtre de Log, possibilité de filtrer la verbosité, install/uninstall dans le registre&gt;Run check de l'instance unique</li>
 	<li>ControlHub : Push des Disconnected si seulement le package est lancé !</li>
 	<li>Suppression de l'attribut "ExecutableFilename" dans la configuration du serveur et déport dans le PackageManifest du package lui même</li>
 	<li>Suppression des attributs .NET "EnableControlHub" &amp; "PackageDescription" -&gt; à définir dans le PackageManifest</li>
 	<li>Ajout de l'attribut DotNetTargetPlatform dans le PackageManifest pour indiquer la version du .NET (sera utilisée par la WebConsole pour l'édition de la configuration)</li>
 	<li>Autorisation sur l'accès au favicon.ico interdiction sur le fichier généré /signalr/hubs</li>
 	<li>SDK : mise à jour des templates Visual Studio</li>
</ul>
<h3>01/03/2015 : Release 1.7 Beta 1 (1.7.0.15060)</h3>
<ul>
 	<li>Mise à jour des librairies SignalR 2.2, JSON.NET 6.0, OWIN 3.0</li>
 	<li>Migration du système de log vers NLog (en remplacement d'EntLib 6.0)</li>
 	<li>Constellation (Server/Sentinel/Package) full compliant sous Linux supression de la MonoSentinel et MonoCommon</li>
 	<li>Schéma de configuration Server : remplacement des balises "host(s)" par "sentinel(s)" &amp; l'attribut 'machineName' devient 'name'</li>
 	<li>Rename global : Machine devient Sentinel !</li>
 	<li>Suppression de l'attribut "serverURI" et remplacement dans un collection d'élements "ListenUris" pour permettre d'ajouter plusieur endpoints aux serveurs (HTTP ou HTTPS)</li>
 	<li>Enrichissement des Settings : un Setting est associé à un attribut de String nommé "Value" (mode actuel) ou à un element XML "&lt;content&gt;" permettant d'embarquer un ConfigurationElement complet</li>
 	<li>Ajout de l'attribut "EnableDeveloperAccess" sur les credentials permettant de connecter fictivement n'importe quel package sous la Sentinel "Debugger"</li>
 	<li>Client lib : ajout d'une methode "GetSettingContent" récupérer un l'élement de configuration ajout des méthodes TryGetSettingValue et TryGetSettingContent</li>
 	<li>Package : ajout d'un "PackageManifest", fichier XML permettant de décrire le package (info, auteur, settings, compatibilité, etc...)</li>
 	<li>Client lib : AJout de la propriété "PackageManifest" pour accès à l'info rename du CurrentPackageName en PackageName (chargement par manifest sinon attribut, sinon type name)</li>
 	<li>Client lib : connexion au ControlHub si défini dans le manifest</li>
 	<li>Client lib : récupération des settings avant le OnStart du package suppression du GetSettingsAsync (les settings sont déjà chargé au lancement) RefreshSettings devient RequestSettings</li>
 	<li>ControlHub : ajout d'un event à chaque message envoyé pour l'interface REST</li>
 	<li>ControlHub : sur l'API JS, utilisation du "PackageName" comme FriendlyName</li>
 	<li>ControlHub : RefreshConfig prend un bool en parametre pour indiquer si on applique la nouvelle config sur la Constellation (Push Settings on Package &amp; Push PackageList on Sentinel) ou si on veut seulement un rechargement de la config section sur le serveur !</li>
 	<li>ControlHub : ajout du PurgeStateObject pour supprimer tous les SO d'une instance de package ou seulement ceux avec un Name ou Type défini d'une instance de package ! Autorisé si et seulement si l'instance est arretée !</li>
 	<li>ControlHub : rename des infos du "UpdateSentinel" : RegistrationDate et Description pour l'unification des propriétés Server/Client !</li>
 	<li>Server : Report des déconnexions d'un package au ControlHub et suppression des MC</li>
 	<li>Server : Possibilité d'associer un package à des groupes directement dans la config XML du serveur</li>
 	<li>Server : Refonte de la homepage HTML du serveur Constellation accessible en anonymous</li>
 	<li>Server : Refonte de l'authentification</li>
 	<li>Server : Persistance des variables et/ou objets depuis des packages : notion de restauration des StateObjects. A activer en config, on récupére la liste des StateObjects dans l'event "StateObjectsRestored" lors d'un démarrage (récupération des etats) !</li>
 	<li>Server : Refonte de l'envoi des messages cross-hub/controlleurs avec notion de MessageSender universel</li>
 	<li>Server : Support des Sagas sur le ControlHub et ControllerHTTP</li>
 	<li>HTTP Controller : formattage JSON par defaut identification (Machine/Package a déclarer dans la config)</li>
 	<li>HTTP Controller : long pooling pour récupération des messages et stateobjects avec notion de subscription</li>
 	<li>HTTP Controller : implémentation du WriteLog, GetSettings, SendMessage, SendMessageWithSage, Subscribe sur Message, MessageGroup et SO, Request de SO</li>
 	<li>Sentinel : possibilité d'overrider la SentinelName par défaut en ajoutant dans la configuration la clé "SentinelName" !</li>
 	<li>Sentinel : la description différencie le MachineName (nom de la machine) de la SentinelName (identifiant unique de la sentinel)</li>
 	<li>Sentinel : ajout de la plateforme version de la Sentinel, CLR &amp; OS dans sa description lors de son enregistrement au serveur</li>
 	<li>Sentinel : refonte du ProcessMonitor (supression du CpuUtil &amp; MemoryUtil) pour la compatibilité Windows/Linux (basée sur la classe Process en remplacement des appels Kernel32 PerfCounter)</li>
 	<li>Sentinel : ajout d'un setting "UseMonoRuntime" pour forcer l'utilisation de Mono sur une machine Windows</li>
 	<li>Les repositories de package côté server et sentinelle peuvent être en relatif</li>
 	<li>SDK : Mise à jour du SDK pour Constellation 1.7 (templates, package nuget, …)</li>
</ul>
<h3>19/02/2015 : Update Sentinels 1.6.10</h3>
<ul>
 	<li>Bug-fix : problème de "/" manquant dans l'URI du package à télécharger (ajout de log)</li>
</ul>
<h3>18/02/2015 : Update Server 1.6.9</h3>
<ul>
 	<li>Bug-fix dans le ReportPackageState si le package n'est pas hosté dans une Sentinel (debug VS) causait une NullReference</li>
 	<li>Ajout des FileVersionAttribute sur le Server &amp; Server.Core</li>
</ul>
<h3>17/02/2015 : Création du SDK</h3>
<ul>
 	<li>Création du SDK Constellation pour Visual Studio</li>
</ul>
<h3>08/01/2015 : Update Sentinels 1.6.9</h3>
<ul>
 	<li>Bugfix : les sentinelles ne redémarre plus un package après un crash s'il a été arrêté, redémarré ou reloadé depuis le ControlHub</li>
 	<li>Ajout des FileVersionAttribute sur les Sentinel.Core</li>
</ul>
<h3>10/12/2014 : Release 1.6.9</h3>
<ul>
 	<li>Ajout de l'attribut 'JsonIgnore' sur la 'DynamicValue' d'un StateObject pour éviter d'envoyer la Value deux fois dans un StateObjet</li>
 	<li>Refonte du SerializationHelper par appel de 'providers' (interface ISerializer) : DataContractSerializer ou JsonSerializer (par défaut). Les méthodes "From/To file" se basent sur les File.Read/Write</li>
 	<li>Sauvegarde/restauration des SO du serveur se base sur le nouveau SerializationHelper par utilison du JSON serializer (correction du problème où les SO ne pouvaient etre serialisés avec le DC)</li>
</ul>
<h3>07/12/2014 : Update Client 1.6.8</h3>
<ul>
 	<li>Ajout d'une méthode public 'DeclareMessageCallbacks' sur le PackageHost pour l'envoi de MC description sur le serveur. Cette méthode est automatiquement appelée lors des (re)connections au serveur.</li>
</ul>
<h3>07/12/2014 : Update Client 1.6.7</h3>
<ul>
 	<li>Paramètre pour attraper les exceptions dans le WriteLog : le WriteLog du Disconnected dans un PackageHost ne leve plus d'exception (évite l UnhandledException)</li>
 	<li>Démystification : WriteLog(format, object[] args) vs WriteLogWithOptions(message, bool wait = false, bool catchException = false)</li>
</ul>
<h3>19/11/2014 : Update Client 1.6.6</h3>
<ul>
 	<li>StateObjectCollectionNotifier : la clé peut être le nom du SO (SOKey) seul, ou le Package::SOKey, ou Machine::Package::SOKey</li>
 	<li>StateObjectCollectionNotifier : ajout d'une méthode ContainsStateObjects en spécifiant le clé recherchée</li>
 	<li>StateObject : ajout des méthodes "GetValue&lt;T&gt;" et "TryGetValue&lt;T&gt;"</li>
</ul>
<h3>05/09/2014 : Update Client 1.6.5</h3>
<ul>
 	<li>Bugfix sur la levée des évènements "SentinelUpdated" et "UpdatePackageList" du ControlManager (problème de désérialisation)</li>
</ul>
<h3>22/08/2014 : Release 1.6.4</h3>
<ul>
 	<li>Correction du UpdateSentinel sur le ControlManager de l'API client avec envoi des propriétés "IsConnected" et "RegistrationDate"</li>
 	<li>Le PackageHost a connaissance des FileVersions du package et de la lib Constellation envoi dans les headers au server et propagation de l'information avec le ReportPackageState sur le ControlHub</li>
</ul>
<h3>21/08/2014 : Update Server 1.6.4</h3>
<ul>
 	<li>Ajout d'un ConstellationGroupManager pour gérer les groupes : permet de pusher aux client qu'une seule fois quand ils font partie de plusieurs groupes (pour les messages et SO)</li>
</ul>
<h3>19/08/2014 : Update Client 1.6.3</h3>
<ul>
 	<li>Ajout du type de paramètre sur les MethodCallbackDescription.ParameterDescription</li>
</ul>
<h3>18/08/2014 : Update Client 1.6.2</h3>
<ul>
 	<li>Bugfix : envoi des CallBackDescriptions sur méthode sans params</li>
</ul>
<h3>18/08/2014 : Release 1.6.1</h3>
<ul>
 	<li>Bugfix sur l'authentification sur le path URI est redéfini en configuration</li>
 	<li>Suppression du report des packages Déconnectés (problème de déconnexion non contrôlé et pas de retour à la normal !)</li>
 	<li>ControlManager : Request des SO attachés lors de la reconnexion pour forcer le rafraichissement</li>
</ul>
<h3>14/08/2014 : Release 1.6</h3>
<ul>
 	<li>Support des StateObjectLink sur des collections de StateObjects (StateObjectCollectionNotifier)</li>
 	<li>A chaque connexion, le PackageHost et le ControlManager se rattache aux groupes, messages et StateObjects auquel il est inscrit</li>
 	<li>Ajout des méthodes Restart/Shutdown et Kill sur le PackageHost et refactorisation des procédures d'arrêt</li>
 	<li>Bugfix : modification de l'objet de reponse du Request des Sentinels permettant d'indiquer si la sentinel est bien connectée</li>
 	<li>Ajout d'une durée de vie en minute (Lifetime) sur un StateObject à titre informatif uniquement pour l'instant</li>
 	<li>Ajout de la notion de "Saga" sur les messages permettant de lier des Requests/Responsse. On crée une saga depuis un MessageScope et on récupére le context depuis le MessageContext.Current</li>
 	<li>Ajout d'un paramètre booléen sur le RegisterStateObjectCallback &amp; StateObjectLink permettant de Request ou non le SO à l'initialisation</li>
 	<li>Gestion des MessageCallbacks sans parametre et ceux avec plusieurs parametres (&gt; 1) sans devoir les encapsuler dans un objet type "Request"</li>
 	<li>Log des erreurs de dispatch de MessageCallback</li>
 	<li>Possibilité d'associer un credential à un package en particulier (si pas, c'est le credential de la sentinel qui est utilisé). Utile pour n'autoriser que certain package à l'accès au ControlHub.</li>
 	<li>Log des erreurs d'authentification au serveur (dans une catégorie dédiée et pushées aux clients du ControlHub)</li>
 	<li>WriteLog si exception dans PackageHost.Start (encapsulant OnStart et OnShutdown) par ajout d'un parametre "await" sur le WriteLog (On attend d'envoyer l'exception avant de tuer le process avant d'avoir le détail de l'erreur sur la Console Constellation)</li>
 	<li>Possibilité de linker un StateObjectLink sans préciser le nom de la machine source. Si pas défini, on link vers un "*" (donc tout package qui match avec le packageName et soName)</li>
 	<li>Ajout d'une propriété "IsHidden" sur le MessageCallbackAttribute pour exclure une méthode de la description des MC sur la constellation (idéal pour cacher les méthodes "handler" de certain event qui ne peuvent être considérées comme des méthodes de service)</li>
 	<li>Breaking change : l'event ValueChanged sur le StateObjectNotifier contient maintenant le OldState et NewState (renommage de la propiété StateObject par NewState en breakin change) permettant de comparer le changement d'état lors du MAJ</li>
 	<li>Breaking change : remplacement la methode GetDynamicValue() par la propriété DynamicValue (uniformisation des objects SONotifier et SO)</li>
 	<li>Correction de la Sentinel : gestion des (sous)dossiers dans les ZIP des packages</li>
 	<li>La propriété "PackageName" d'un MethodCallbackDescription est correctement affecté au nom du package défini sur le serveur (plus d'erreur avec le ".exe" en suffixe)</li>
 	<li>Ajout du type et metadatas sur la méthode PushStateObject depuis l'interface REST</li>
</ul>
<h3>17/07/2014 : Release 1.5</h3>
<ul>
 	<li>Envoi de la description de l'objet de paramètre de chaque MethodCallbacks</li>
 	<li>Ajout d'un attribut "EnableControlHub" dans la configuration de credential pour filtrer les accès au hub de control</li>
 	<li>Ajout du ControlManager sur le PackageHost pour accéder au hub de control depuis un package</li>
 	<li>Ajout d'un type (en string) et métadatas (clé/valeur) sur un StateObject</li>
 	<li>Récupération des SO &amp; abonnement aux updates depuis le ControlManager</li>
 	<li>Méthode permettant d'enregistrer des callbacks sur les MAJ des SO</li>
 	<li>Ajout de la classe StateObjectNotifier permettant de fournir une classe container avec évènement PropertyChanged lors des mise à jour des StateObjects</li>
 	<li>Ajout de l'attribut StateObjectLink permettant de lier automatique une propriété aux updates d'un StateObject défini pleinnement (machine/package/nom)</li>
 	<li>Prise en charge du "Type" pour les Subscribe/Unsubscribe des SO &amp; RequestStateObject revu de l'algo de sélection (tous les cas sont possible, soit 2^4 = 16 combinaisons de groupe)</li>
</ul>
<h3>02/07/2014 : Release 1.4</h3>
<ul>
 	<li>Remonté de l'état courant des packages à la reconnexion au serveur</li>
 	<li>Sauvegarde et restauration des SO au redémarrage du serveur</li>
 	<li>Ajout des AccessKey sur les Sentinelles et app de Control (les packages se connectent avec la clé de la Sentinelle)</li>
 	<li>Ajout d'un middleware OWIN pour l'authentification (control des AccessKey)</li>
 	<li>Interface HTTP/REST basée sur WebAPI 2.2 (module OWIN) pour exposée l'envoi de message et le push de SO directement sur une interface legacy HTTP</li>
 	<li>Déclaration des MessagesCallbacks (avec description) à chaque connexion et exposition sur le hub de control afin de déclarer tous les callbacks de messages des packages sur le serveur</li>
 	<li>Bugfixes</li>
</ul>
<h3>20/05/2014 : Release 1.3</h3>
<ul>
 	<li>Reconnexion automatique des Sentinelles et Packages si perte de connexion au serveur</li>
 	<li>Remonté de la consommation des packages (cpu et ram)</li>
 	<li>Portage sur Mono</li>
 	<li>Bugfixes</li>
</ul>
<h3>12/05/2014 : Release 1.2</h3>
<ul>
 	<li>Ajout des MessageCallbacks</li>
 	<li>Nouvelle API pour la gestion des scopes des messages et exposition d'un Proxy dynamique</li>
 	<li>Gestion des messages entre le hub de control et de la constellation</li>
 	<li>Request des SO par filtre et notion d'abonnement</li>
 	<li>Notion d'abonnement pour les messages également</li>
 	<li>Ajout de méthodes Ajout et suppression à des groupes</li>
 	<li>Bugfixes</li>
</ul>
<h3>06/05/2014 : Release 1.1</h3>
<ul>
 	<li>Request de StateObject</li>
 	<li>Reload des packages</li>
 	<li>Amélioration du contrôle des packages et ajout du "Reload"</li>
 	<li>Ajout de fonctionnalité dans le ControlHub</li>
 	<li>Amélioration de la gestion de la configuration (mode local, lazy loading, cast, ...)</li>
 	<li>Ajout des Status des packages en temps réel</li>
 	<li>Date d'enregistrement des Sentinelles</li>
</ul>
<h3>30/04/2014 : Release 1.0</h3>
<ul>
 	<li>Ajout des settings sur les packages</li>
 	<li>Ajout du hub de contrôle</li>
 	<li>Ajout d’un bus de message avec notion de scope</li>
 	<li>Ajout des “recovery options”</li>
 	<li>Ajout de la sentinelle UI</li>
</ul>
<h3>18/04/2014 : Constellation 1.0 Beta 1</h3>
<ul>
 	<li>Architecteur Serveur / Sentinelle / Serveur</li>
 	<li>Console log temps réel</li>
 	<li>Notion de StateObject</li>
</ul>