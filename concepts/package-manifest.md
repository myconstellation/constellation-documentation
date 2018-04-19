---
ID: 1345
post_title: Le Package Manifest
author: Sebastien Warin
post_date: 2016-03-16 15:16:21
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/concepts/package-manifest/
published: true
publish_post_category:
  - "12"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:21:10
---
Le fichier XML “PackageInfo.xml” représente le manifeste d’un package Constellation. Il doit être présent à la racine de chaque package. Il contient toutes les informations décrivant un package.

Ce fichier est automatiquement créé par Visual Studio lorsque vous créez un projet de type Constellation.

Il contient trois parties :
<ol>
 	<li>Les informations générales sur votre package</li>
 	<li>Les informations de compatibilité du package</li>
 	<li>Les informations sur les settings du packages</li>
</ol>
Exemple :
<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.myconstellation.io/Constellation/1.8/PackageManifest"
         Name="DemoPackage" 
         Version="1.0"
         Author="Sebastien Warin"         
         URL="http://sebastien.warin.Fr"
         Description="Demo package for Constellation"
         RequestLastStateObjectsOnStart="true"
         EnableControlHub="false"&gt;
  &lt;Compatibility constellationVersion="1.8"&gt;
    &lt;Platforms&gt;
      &lt;Platform id="Win32NT" isCompliant="true" /&gt;
      &lt;Platform id="MacOSX" isCompliant="false" /&gt;
      &lt;Platform id="Unix" isCompliant="true" /&gt;
    &lt;/Platforms&gt;
  &lt;/Compatibility&gt;
  &lt;Settings&gt;
    &lt;Setting name="MonInterval" type="Int32" defaultValue="2016" isRequired="false" /&gt;
    &lt;Setting name="MyConfigElement" type="ConfigurationSection" isRequired="true" schemaXSD="MyPackageConfigurationDesigner.xsd" /&gt;
  &lt;/Settings&gt;
&lt;/Package&gt;</pre>
<h3>Informations générales</h3>
Vous pouvez définir sur l’élément racine “Package” les attributs suivants :
<ul>
 	<li><u>Name</u> : le nom du package (il s’agit de l’identifiant). Si pas défini, le nom du package est le nom du fichier (sans l’extension ”.zip”)</li>
 	<li><u>Description</u> : le texte de description de votre package tel qu’il apparaîtra sur le catalogue</li>
 	<li><u>Author</u> : nom de l’auteur du package</li>
 	<li><u>URL</u> : URL de l’auteur</li>
 	<li><u>PackageUrl</u> : URL spécifique au package</li>
 	<li><u>Version</u> : version du package. Si pas défini, la version du package est le numéro de version de l’assembly.</li>
 	<li><u>Icon</u> : chemin relatif vers l’icone du package</li>
 	<li><u>ExecutableFilename</u> : le nom de l’exécutable du package. Si pas défini, l’exécutable est le “&lt;nom du package&gt;.exe”</li>
 	<li><u>RequestLastStateObjectsOnStart</u> : booléen indiquant si vous souhaitez recevoir les StateObjects produits par votre package lors de sa dernière exécution. (<a href="/client-api/net-package-api/persistance-de-donnes-dans-un-package/#Utiliser_les_StateObjects_comme_stockage">voir ici</a>)</li>
 	<li><u>EnableControlHub</u> : booléen indiquant si vous souhaitez connecter votre package au hub de contrôle. (<a href="/client-api/net-package-api/controlmanager/">voir ici</a>)</li>
</ul>
<h3>Informations de compatibilité</h3>
Vous pouvez définir des informations quant à la compatibilité de votre package.

Sur l’élément “Compatibility” vous pouvez indiquer les attributs suivants :
<ul>
 	<li><u>dotNetTargetPlatform</u> : version du framework .NET utilisée (net40 ou net45)</li>
 	<li><u>constellationVersion</u> : version de la Constellation utilisée (“1.7” ou “1.8” à l’heure actuelle)</li>
</ul>
Vous pouvez également ajouter des éléments enfants “Platforms” pour indiquer les plateformes supportées. Chaque “Platform” définie les attributs :
<ul>
 	<li><u>id</u> : identifiant de la plateforme (Win32NT, Unix ou MacOSX)</li>
 	<li><u>isCompliant</u> : booléen indiquant si la plateforme est supportée ou non</li>
</ul>
Par exemple si vous développez un package Python exploitant les GPIO d’un Raspberry, vous allez indiquer :
<pre class="lang:xml decode:true">&lt;Platforms&gt;
  &lt;Platform id="Win32NT" isCompliant="false" /&gt;
  &lt;Platform id="Unix" isCompliant="true" /&gt;
  &lt;Platform id="MacOS" isCompliant="false" /&gt;
&lt;/Platforms&gt;</pre>
A l’inverse, un package comme HWMonitor qui utilise des API spécifiques à Windows, vous allez indiquer qu’il n’est pas compatible sur Unix/Linux et MacOS par les lignes :
<pre class="lang:xml decode:true">&lt;Platforms&gt;
  &lt;Platform id="Win32NT" isCompliant="true" /&gt;
  &lt;Platform id="Unix" isCompliant="false" /&gt;
  &lt;Platform id="MacOS" isCompliant="false" /&gt;
&lt;/Platforms&gt;</pre>
Au démarrage d’un package, la sentinelle lancera des “Warnings” dans les logs Constellation si le package ne respecte pas le contrat de compatibilité sans toutefois bloquer le démarrage.

Ces informations servent également pour la Console Constellation, afin de vous donner des informations lors de la ajout et configuration des packages de votre Constellation.
<h3>Informations sur les Settings du package</h3>
Enfin, la troisième et dernière partie du manifeste sert à décrire les settings utilisés par un package.

La déclaration des settings dans le manifeste n’a pas impact sur le fonctionnement du package mais il permet de décrire les clés de configuration utilisées par le package pour que des outils tel que la Console Constellation puisse proposer une interface graphique de configuration pour chaque package. Il est donc vivement recommandé de décrire les settings de ses packages. De plus il est possible de définir des valeurs par défaut dans le manifeste.

Chaque setting déclaré dans le manifeste doit obligatoirement comporter les attributs suivants  :
<ul>
 	<li><u>Name</u> : le nom (clé) du setting</li>
 	<li><u>Type</u> : le type du setting</li>
</ul>
Le type peut être :
<ul>
 	<li><u>Boolean</u> : un booléen (true/false)</li>
 	<li><u>Double</u> : un double</li>
 	<li><u>String</u> : un chaine de caractère</li>
 	<li><u>Int32</u> : un entier (32 bits)</li>
 	<li><u>Int64</u> : un long (64 bits)</li>
 	<li><u>ConfigurationSection</u> : un section de configuration .NET</li>
 	<li><u>DateTime</u> : un DateTime</li>
 	<li><u>TimeSpan</u> : une durée</li>
 	<li><u>XmlDocument</u> : un document XML</li>
 	<li><u>JsonObject</u> : un objet JSON (objet ou tableau)</li>
</ul>
Par exemple :
<pre class="lang:default decode:true">&lt;Settings&gt;
  &lt;Setting name="MyStringSetting" type="String"  /&gt;
  &lt;Setting name="MyIntSetting" type="Int32" /&gt;
  &lt;Setting name="MyBoolSetting" type="Boolean" /&gt;
  &lt;Setting name="MyXmlDocument" type="XmlDocument" /&gt;
  &lt;Setting name="MyJsonObject" type="JsonObject" /&gt;
&lt;/Settings&gt;</pre>
Sur chaque setting déclaré, vous pouvez également définir les attributs suivants :
<ul>
 	<li>“<u>isRequiered</u>“ : indique si le setting est obligatoire ou non (par défaut “false”). Si le setting n’est pas déclaré, une erreur est levée.</li>
 	<li>“<u>description</u>” : permettant de donner une explication sur le setting (affichée à l’utilisateur sur la Console Constellation)</li>
 	<li>“<span style="text-decoration: underline;">defaultValue</span>” : la valeur par défaut du setting si le setting n’est pas déclaré (<a href="/client-api/net-package-api/settings/#Resolution_des_settings">plus d'info</a>)</li>
 	<li>“<u>schemaXSD</u>” : indique le nom du fichier du schéma XSD (chemin relatif au package) que la valeur XML doit valider (seulement pour les settings de type XmlDocument ou ConfigurationSection)</li>
 	<li>“<u>ignoreLocalValue</u>” : si “true” alors la valeur définie dans le fichier local App.config est ignorée  (<a href="/client-api/net-package-api/settings/#Declarez_des_settings_dans_votre_configuration_local">plus d’info</a>)</li>
 	<li>“<u>ignoreDefaultValue</u>” : si “true” alors la valeur par défaut définie dans le manifeste est ignorée</li>
</ul>
Pour les settings XmlDocument, ConfigurationSection et JsonObject vous pouvez également ajouter une sous-section &lt;defaultContent&gt; sur vos settings pour définir le contenu par défaut.

Par exemple :
<pre class="lang:default decode:true">&lt;Settings&gt;
  &lt;Setting name="MyStringSetting" type="String" description="This is a String setting" ignoreLocalValue="true" /&gt;
  &lt;Setting name="MyIntSetting" type="Int32" isRequired="false" defaultValue="1234"  /&gt;
  &lt;Setting name="MyBoolSetting" type="Boolean" /&gt;
  &lt;Setting name="MyXmlDocument" type="XmlDocument" schemaXSD="MonSchema.xsd" /&gt;
  &lt;Setting name="MyJsonObject" type="JsonObject" isRequired="true" ignoreDefaultValue="true"&gt;
    &lt;defaultContent&gt;
      &lt;![CDATA[
      {
        "Number": 123,
        "String" : "This is a test (local)",
        "Boolean": true
      }
      ]]&gt;
    &lt;/defaultContent&gt;
  &lt;/Setting&gt;
&lt;/Settings&gt;</pre>
A lire également : <a href="/client-api/net-package-api/settings">Les Settings d'un package</a>