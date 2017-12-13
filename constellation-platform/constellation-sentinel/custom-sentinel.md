---
ID: 2151
post_title: Personnalisation de la sentinelle
author: Sebastien Warin
post_date: 2016-08-09 13:53:23
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-sentinel/custom-sentinel/
published: true
post_modified: 2017-12-13 17:30:55
---
Une sentinelle (Service ou UI) installée sur un système Linux ou Windows n’a besoin que de deux informations pour fonctionner :

<ul>
    <li>L’URI de la Constellation sur laquelle elle doit se connecter</li>
    <li>La clé d’accès (AccessKey) qu’elle doit utiliser pour s’authentifier sur la Constellation</li>
</ul>

Bien entendu pour fonctionner, il faudra que cette sentinelle soit correctement déclarée dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/#Section_sentinels">configuration</a> de votre Constellation (nom de sentinelle et credential associé).

Ces deux paramètres  de configuration sont définis dans le fichier “<em>Constellation.Sentinel.exe.config</em>” dans le cas de la sentinelle “Service” ou “<em>Constellation.Sentinel.UI.exe.config</em>” pour une sentinelle “UI”.

<pre class="lang:xml decode:true">&lt;add key="ConstellationServerURI" value="http://myconstellationserver.mylocalNetwork.lan:8088/" /&gt;
&lt;add key="ConstellationAccessKey" value="123456789" /&gt;</pre>

Dans ce fichier vous avez également la possibilité de personnaliser d’autres paramètres de façon facultative.

<h3>Chemin local des packages déployés</h3>

Clé de configuration : “LocalPackagesDirectory”.

Il s’agit du répertoire local dans lequel les packages Constellation seront déployés. Ce chemin peut être relatif au répertoire d’installation de la sentinelle ou absolu.

Par défaut, il s’agit du sous-répertoire “Packages”.

<h3>Nom de la sentinelle</h3>

Clé de configuration : “SentinelName”.

Par défaut le nom de la sentinelle est le nom de la machine mais vous avez la possibilité de personnaliser ce nom en définissant ce paramètre de configuration.

Dans le cas par exemple où vous avez deux machines différentes sur des réseaux différents (ou non) mais ayant le même nom (hostname), vous pouvez personnaliser le nom de la sentinelle sur une des deux machines pour pouvoir les différencier au niveau de votre Constellation.

<strong>Attention</strong> : le nom de la sentinelle doit être unique sur une Constellation. Il ne peut pas y avoir deux sentinelles (ou plus) connectées sur une même Constellation avec le même nom.

<u>Cas particulier</u> : sur une sentinelle UI (pour Windows seulement), le nom de la sentinelle (que ce soit le nom de la machine ou un nom personnalisé dans le fichier de configuration) est toujours concaténé avec le suffixe “_UI”.

<h3>Temps d’arrêt maximum d’un package</h3>

Clé de configuration : “ShutdownPackageTimeout”.

Il s’agit du temps maximal d’arrêt d’un package au delà duquel le processus du package est tué par la sentinelle.

En effet, lorsque vous ordonnez l’arrêt d’un package via le hub de contrôle (par exemple à partir de la Console Constellation ou votre propre application connectée au hub de contrôle), l’ordre d’arrêt du package est envoyé au package ainsi qu’à la sentinelle.

Lorsque le package reçoit l’ordre de s’arrêter, il invoque les méthodes OnPreShutdown puis OnShutdown (<a href="/client-api/net-package-api/les-bases-des-packages-net/#Fonctionnement_de_base">plus d’information ici</a>) définissant sa propre procédure d’arrêt, avant de fermer son processus. Le package est alors correctement arrêté.

La sentinelle, quant à elle, surveille le bon arrêt du package. Pour cela elle déclenche un chronomètre pour contrôler que le package se ferme bien dans le temps imparti.

Si le package dépasse ce temps imparti (quelque soit la raison), la sentinelle tuera le processus du package afin de garantir l’arrêt du package.

Ce temps maximal d’arrêt d’un package est défini par le paramètre “ShutdownPackageTimeout” qui est par défaut fixé à 10 secondes. C’est à dire qu’un package à 10 secondes pour s’arrêter avant d’être tué par sa sentinelle.

<h3>Intervalle de temps pour le report de la consommation des packages (PackageUsage)</h3>

Clé de configuration : “ReportPackageUsageInterval”.

Par défaut, chaque seconde (1000 ms) la sentinelle envoie un rapport contenant la consommation CPU et RAM de chaque package qu'elle héberge.

Vous pouvez personnaliser cette valeur en ajoutant le paramètre "ReportPackageUsageInterval" dans la configuration. La valeur est exprimée en milliseconde.

Sur un système faible en ressource (un Raspberry par exemple), il peut être intéressant d'espacer l'envoi de ce report.

<h3>Utilisation du moteur Mono</h3>

Clé de configuration : “UseMonoRuntime”.

Lorsqu’une sentinelle tourne sur un environnement Linux, elle utilise le moteur d’exécution Mono pour démarrer le package. Si la sentinelle tourne sur un environnement Windows, elle utilise le moteur .NET de Microsoft.

Vous pouvez cependant forcer l’utilisation du moteur Mono sur un environnement Windows en définissant le paramètre “UseMonoRuntime” à <em>true</em>.