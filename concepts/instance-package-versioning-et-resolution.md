---
ID: 2246
post_title: >
  Instance de package, package,
  versioning, multi-instances et
  résolution des packages
author: Sebastien Warin
post_date: 2016-08-11 13:27:29
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/instance-package-versioning-et-resolution/
published: true
publish_post_category:
  - "12"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:20:39
---
Un package est une application avec ou sans une interface graphique qu’on déploie sur une sentinelle sur laquelle il sera démarré (<a href="/concepts/architecture-constellation-sentinel-package/#Les_packages">plus d’information ici</a>).

Concrètement, un package est un fichier ZIP qui contient un exécutable (exe) qui sera exécuté et supervisé par une sentinelle.
<h3>Sentinel + Package = Instance de package</h3>
Par défaut, Constellation va “chercher” le package dont le nom est celui de l’instance que vous déclarez dans la <a href="/constellation-platform/constellation-server/fichier-de-configuration/">configuration</a>.

Par exemple, pour déployer le package “HWMonitor” sur la sentinelle nommée “SENTINEL-DEMO”, on écrira dans le <a href="/constellation-platform/constellation-server/fichier-de-configuration/">fichier de configuration</a> :
<pre class="lang:xml decode:true">&lt;sentinel name="SENTINEL-DEMO" credential="Standard"&gt;
    &lt;packages&gt;    
      &lt;package name="HWMonitor"&gt;&lt;/package&gt;
    &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
L’attribut “name” du package défint en fait le <strong>nom de l’instance</strong> du package. On parlera alors de l’instance “SENTINEL-DEMO/HWMonitor” pour identifier de manière unique l’instance de ce package dans la Constellation.

Lors du déploiement, <strong>Constellation va analyser chaque package dans son repository pour récupérer le package dont le nom déclaré le </strong><a href="/concepts/package-manifest/"><strong>manifeste</strong></a><strong> correspond</strong> au nom de notre instance.

Dans le cas où il ne trouve aucun package dont le <a href="/concepts/package-manifest/">manifeste</a> déclare le nom de package recherché, il va tenter de sélectionner le package “&lt;nom d’instance&gt;.zip”. Si le fichier n’existe pas alors une erreur sera levée dans les logs de la Constellation.

Dans notre cas, Constellation va analyser chaque package de son repository, c’est à dire chaque fichier .ZIP pour récupérer celui qui a dans son <a href="/concepts/package-manifest/">manifeste</a> (<em>PackageInfo.xml</em>) déclaré la propriété “Name” à “HWMonitor”. Si il ne trouve pas, il va tenter de récupérer le package par le nom “HWMonitor.zip”.
<h3>Versioning des packages</h3>
En plus du nom du package, le <a href="/concepts/package-manifest/">manifeste</a> (<em>PackageInfo.xml</em>) déclare également d’autre information comme la version du package.

Le “Package Repository” d’un serveur Constellation peut donc contenir plusieurs versions d’un même package.

Par exemple, imaginez les packages suivants stockés dans le “Package Repository” de votre serveur Constellation :
<ul>
 	<li>HWMonitor.zip (dont le manifeste définit Name=”HWMonitor” et Version=”1.0”)</li>
 	<li>HWMonitor-1.1.zip (dont le manifeste définit Name=”HWMonitor” et Version=”1.1”)</li>
 	<li>HWMonitor-1.2.zip (dont le manifeste définit Name=”HWMonitor” et Version=”1.2”)</li>
</ul>
<u>Note</u> : la nomenclature des noms des fichiers dans le repository n’a aucune importance.

Lorsque dans votre <a href="/constellation-platform/constellation-server/fichier-de-configuration/">fichier de configuration</a> vous déclarez :
<pre class="lang:xml decode:true">&lt;package name="HWMonitor"&gt;&lt;/package&gt;</pre>
Constellation trouve donc 3 packages qui correspondent (car name=”HWMonitor”). Dans ce cas, le serveur <strong>Constellation va sélectionner le package dont la version est la plus haute</strong>.

Ainsi, dans cet exemple, c’est le fichier “HWMonitor-1.2.zip” qui sera “envoyé” à la sentinelle pour l’instance “HWMonitor”.
<h3>Résolution explicite du fichier d’un package</h3>
Jusqu’à présent la règle est donc d’utiliser le fichier dont le nom du package défini dans son manifeste correspond au nom de l’instance déclarée (package “HWMonitor &lt;&gt; instance “HWMonitor”) et si plusieurs fichiers correspondent on sélectionne la version la plus haute. A contrario, si aucun fichier n’est trouvé, on tente d’utiliser le fichier “&lt;nom d’instance&gt;.zip”.

C’est donc une résolution “implicite” du fichier du package qui est réalisée par le serveur Constellation.

Toutefois il est possible de contourner ce comportement et de<strong> définir explicitement le fichier d’un package à utiliser grâce à l’attribut “filename”</strong>.

Lorsque dans votre <a href="/constellation-platform/constellation-server/fichier-de-configuration/">fichier de configuration</a> vous déclarez :
<pre class="lang:xml decode:true">&lt;package name="HWMonitor" filename="HWMonitor-1.1.zip"&gt;&lt;/package&gt;</pre>
Vous définissez explicitement que cette instance nommée “HWMonitor” utilisera le package contenu dans le fichier “HWMonitor-1.1.zip”. On peut donc forcer l’utilisation d’une version spécifique en déclarant explicitement le fichier à utiliser pour une instance d’un package.

On aurait pu aussi écrire :
<pre class="lang:xml decode:true">&lt;package name="HWMonitor" filename="DemoPackage.zip"&gt;&lt;/package&gt;</pre>
C’est à dire qu’on déclare une instance d’un package nommée “HWMonitor” utilisant le package contenu dans le fichier “DemoPackage.zip”, ce qui n’a pas de sens mais est tout à fait possible !

Ainsi pour résumer, <strong>Constellation utilise donc en priorité le fichier défini par l’attribut “filename” si une valeur est définie pour une instance</strong>. Dans le cas contraire on applique la règle implicite définie ci-dessus.
<h3>Instances multiples d’un même package</h3>
Avec l’attribut “filename” il est donc possible de déployer plusieurs instances d’un même package sur une même sentinelle.

Par exemple :
<pre class="lang:xml decode:true">&lt;sentinel name="SENTINEL-DEMO" credential="Standard"&gt;
    &lt;packages&gt;    
      &lt;package name="HWMonitor1" filename="HWMonitor.zip"&gt;&lt;/package&gt;
      &lt;package name="HWMonitor2" filename="HWMonitor.zip"&gt;&lt;/package&gt;
   &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
Dans ce cas, on a deux instances du package “HWMonitor.zip” qui seront déployées sur “SENTINEL-DEMO”.

Les instances “SENTINEL-DEMO/HWMonitor1” et “SENTINEL-DEMO/HWMonitor2” sont deux instances différentes dans votre Constellation et peuvent avoir des configuration (settings) différentes mais elles sont basées sur le même code, car elles partagent la même source (le package “HWMonitor.zip”).

<strong>Vous pouvez donc déployer le même package plusieurs fois sur une même sentinelle en déclarant plusieurs instances (name) du même package (filename)</strong>.
<h3>Résolution semi-implicite / semi-explicite d’un package</h3>
Le problème des instances multiples est que vous déclarez explicitement le fichier (filename) à utiliser.

Dans l’exemple précèdent “SENTINEL-DEMO/HWMonitor1” et “SENTINEL-DEMO/HWMonitor2” sont des instances du même package “HWMonitor.zip” qui d’après les exemples précédents correspond à la version “1.0” du package “HWMonitor”.

Toujours d’après les exemples précédents, on dispose également de la version 1.1 et 1.2 de ce package dans le "Package Repository"  (fichiers HWMonitor-1.1.zip et HWMonitor-1.2.zip).

Pour mettre à jour ces deux instances (“SENTINEL-DEMO/HWMonitor1” et “SENTINEL-DEMO/HWMonitor2”) vers une version plus récente, disons la version 1.2, il faudra modifier à la fois l’attribut “filename” de l’instance “HWMonitor1” mais également celui de l'instance de “HWMonitor2”.

Ce qui revient à écrire :
<pre class="lang:xml decode:true">&lt;sentinel name="SENTINEL-DEMO" credential="Standard"&gt;
    &lt;packages&gt;    
      &lt;package name="HWMonitor1" filename="HWMonitor-1.2.zip"&gt;&lt;/package&gt;
      &lt;package name="HWMonitor2" filename="HWMonitor-1.2.zip"&gt;&lt;/package&gt;
   &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
Pour éviter cela, il y a un “mode intermédiaire” qu’on pourrait nommer résolution “semi-implicite” ou “semi-explicite” car il s’agit d’un mixte des deux !

Pour l’utiliser, <strong>il suffit d’omettre l’extension “.zip” dans l’attribut “filename”</strong>.

Dans notre exemple, déclarez :
<pre class="lang:xml decode:true">&lt;sentinel name="SENTINEL-DEMO" credential="Standard"&gt;
    &lt;packages&gt;    
      &lt;package name="HWMonitor1" filename="HWMonitor"&gt;&lt;/package&gt;
      &lt;package name="HWMonitor2" filename="HWMonitor"&gt;&lt;/package&gt;
   &lt;/packages&gt;
&lt;/sentinel&gt;</pre>
On a toujours deux instances “HWMonitor1” et “HWMonitor2” et ces deux instances déclarent le “filename” à “HWMonitor”.

Sans l’extension “.zip”, “HWMonitor” ne correspond à aucun <u>fichier</u> de notre repository et c’est donc là que<strong> Constellation réalise une résolution partielle : mi-explicite / mi-implicite</strong>.

<strong>Cette résolution partielle sélectionne la dernière version du package qui porte le nom déclaré dans attribut “filename”</strong> (ici nommé “HWMonitor”).

Ainsi en ajoutant la version 1.3 du package “HWMonitor” dans notre repository, les deux instances seront mises à jour avec la version 1.3 de ce package sans avoir besoin de modifier la configuration de notre Constellation.
<h3>Résumé</h3>
Pour résumer cet article en quelques points importants à retenir :
<ul>
 	<li>Un package déployé sur une sentinelle est une instance de package</li>
 	<li>Chaque instance a son propre cycle de vie, sa propre configuration et sa propre identité dans la Constellation</li>
 	<li>Il n’y a pas de lien entre le nom du package et le nom de l’instance, vous pouvez nommer vos instances sans restriction à partir du moment où elles sont uniques pour une même sentinelle</li>
 	<li>Vous pouvez avoir plusieurs instances d’un même package sur une même sentinelle</li>
 	<li>Vous pouvez également avoir plusieurs instances de versions différentes d’un même package sur une même sentinelle</li>
 	<li>Les packages (fichiers ZIP) sont tous stockés dans le “Package Repository” du serveur</li>
 	<li>Pour résoudre le package à utiliser pour une instance :
<ol>
 	<li>Si l’attribut “filename” est défini :
<ol>
 	<li>Et qu’il correspond à un fichier du “Package Repository”, c’est ce package qui sera utilisé pour l’instance du package</li>
 	<li>Sinon, sans extension .ZIP, on utilise la version la plus haute du package dont le nom (déclaré dans le manisfeste) est celui déclaré dans l’attribut “filename”</li>
</ol>
</li>
 	<li>Sinon, sans l’attribut “filename” :
<ol>
 	<li>On utilise la version la plus haute du package dont le nom (déclaré dans le manisfeste) est celui déclaré dans l’attribut “name” (c’est à dire le nom de l’instance du package)</li>
 	<li>Sinon on utilise le package qui se nomme &lt;name&gt;.zip si ce fichier existe dans le “Package Repository”</li>
 	<li>Sinon une erreur est levée</li>
</ol>
</li>
</ol>
</li>
</ul>