---
ID: 1297
post_title: 'Settings : paramètres de configuration de vos packages'
author: Sebastien Warin
post_date: 2016-03-16 13:15:58
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/settings/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:53:40
---
Chaque package peut définir des paramètres de configuration centralisés au niveau du serveur Constellation.

L'ensemble des paramètres des configurations de vos différents packages, virtuels ou non, sont donc centralisés dans un fichier unique. Une modification de valeur est automatiquement mis à jour sur le package cible.

Il y a deux types de settings :
<ul>
 	<li>Le “Setting Value” : il s’agit d’un couple clé/valeur défini dans des attributs XML à l’instar des &lt;appSettings&gt; d’une application .NET</li>
 	<li>Le “Setting Content”  : au lieu de définir la valeur d’un paramètre dans un attribut XML, on peut la définir dans un élément XML enfant permettant d’avoir des settings renfermant du XML ou JSON</li>
</ul>
<h3>Les Settings Values</h3>
Voici par exemple des “SettingValues”  déclarés dans la configuration d'un package :
<pre class="lang:default decode:true ">&lt;setting key="MyBoolSetting" value="true" /&gt;      
&lt;setting key="MyStringSetting" value="This is a string" /&gt;
&lt;setting key="MyIntSetting" value="123" /&gt;</pre>
Ces trois settings définissent la valeur dans l’attribut “value” (= SettingValue).

Pour récupérer la valeur du paramètre “MyStringSetting”  dans votre code C#, vous pouvez utiliser la méthode “<em>GetSettingValue</em>” :
<pre class="lang:c# decode:true">PackageHost.WriteInfo("My String = {0}", PackageHost.GetSettingValue("MyStringSetting"));</pre>
La méthode “<em>GetSettingValue</em>” vous retourne par défaut la valeur d’un setting en “string” mais vous pouvez également convertir cette valeur en précisant le type attendu :
<pre class="lang:c# decode:true">int myIntSetting = PackageHost.GetSettingValue&lt;int&gt;("MyIntSetting");
bool myBoolSetting = PackageHost.GetSettingValue&lt;bool&gt;("MyBoolSetting");</pre>
<h3>Les Settings Contents</h3>
Si vous avez un modèle de configuration plus compliqué qu’une série de clé/valeur vous pouvez utiliser les “Setting Contents” pour définir du XML ou JSON comme valeur de setting.

Par exemple la configuration de votre package peut définir deux autres settings, l’un contenant du XML et l’autre du JSON de cette façon :
<pre class="lang:default decode:true">&lt;setting key="MyXmlDocument"&gt;
  &lt;content&gt;
    &lt;note date="09-02-2016"&gt;
      &lt;to&gt;Tove&lt;/to&gt;
      &lt;from&gt;Jani&lt;/from&gt;
      &lt;heading&gt;Reminder&lt;/heading&gt;
      &lt;body&gt;Don't forget me this weekend!&lt;/body&gt;
    &lt;/note&gt;
  &lt;/content&gt;
&lt;/setting&gt;

&lt;setting key="MyJsonObject"&gt;
  &lt;content&gt;
    &lt;![CDATA[
    {
      "Number": 123,
      "String" : "This is a test (local)",
      "Boolean": true
    }
    ]]&gt;
  &lt;/content&gt;
&lt;/setting&gt;
</pre>
Voici les méthodes pour récupérer ces settings :
<ul>
 	<li><u>GetSettingValue</u> : récupère le contenu brute de votre setting sous forme d’un string</li>
 	<li><u>GetSettingAsJsonObject</u> : dé-sérialise le contenu JSON de votre setting et vous retourne un objet dynamique</li>
 	<li><u>GetSettingAsJsonObject&lt;T&gt;</u> : dé-sérialise le contenu JSON de votre setting et le converti dans un objet de votre type (T)</li>
 	<li><u>GetSettingAsXmlDocument</u> : dé-sérialise le contenu XML de votre setting et vous retourne un XmlDocument</li>
 	<li><u>GetSettingAsConfigurationSection&lt;TConfigurationSection&gt;</u> : dé-sérialise le contenu XML de votre setting sous forme d’un ConfigurationSection .NET</li>
</ul>
Par exemple, pour manipuler le setting XML on pourrait écrire :
<pre class="lang:c# decode:true">var xml = PackageHost.GetSettingAsXmlDocument("MyXmlDocument");
PackageHost.WriteInfo("My XmlDocument Attribute = {0} , first node value = {1}", xml.ChildNodes[0].Attributes["date"].Value, xml.ChildNodes[0].FirstChild.InnerText);</pre>
Autre exemple avec le setting JSON :
<pre class="lang:c# decode:true">dynamic json = PackageHost.GetSettingAsJsonObject("MyJsonObject");
PackageHost.WriteInfo("My JsonObject String={0}, Int={1}, Boolean={2}", json.String, json.Number, json.Boolean);
</pre>
<h3>TryGetSettings</h3>
Notez que toutes ces méthodes citées ci-dessus ont un équivalent de type “<em><strong>Try</strong>GetSettingXXX</em>” :
<ul>
 	<li>TryGetSettingAsConfigurationSection&lt;TConfigurationSection&gt;</li>
 	<li>TryGetSettingAsJsonObject</li>
 	<li>TryGetSettingAsJsonObject&lt;T&gt;</li>
 	<li>TryGetSettingAsXmlDocument</li>
 	<li>TryGetSettingValue&lt;T&gt;</li>
</ul>
Ces méthodes placent le résultat dans un paramètre de sortie (“out”) et vous retourne un booléen indiquant si l’opération est réussite ou non.

Par exemple :
<pre class="lang:c# decode:true">int monParametre = 0;
if (PackageHost.TryGetSettingValue&lt;int&gt;("MyIntSetting", out monParametre))
{
    PackageHost.WriteInfo("Lecture de 'MyIntSetting' en int OK. Valeur = {0}", monParametre);
}
else
{
    PackageHost.WriteError("Impossible de récupérer le setting 'MyIntSetting' en int");
}
</pre>
<h3>Déclarer les settings dans le Package Manifest</h3>
Tous les settings devraient être déclarés dans le <a href="/concepts/package-manifest/">manifeste du package</a> (fichier PackageInfo.xml).

La déclaration des settings dans le manifeste n’a pas impact dans le fonctionnement du package mais il permet de décrire la configuration du package pour que des outils tel que la Console puisse proposer une interface graphique de configuration pour chaque package. Il est donc vivement recommandé de décrire les settings de ses packages. De plus il est possible de définir des valeurs par défaut dans le manifeste.

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
Ensuite vous pouvez définir d’autre attribut pour décrire vos settings :
<ul>
 	<li>“<u>isRequired</u>“ : indique si le setting est obligatoire ou non (par défaut “false”)</li>
 	<li>“<u>description</u>” : la description du setting (texte qui sera affiché à l’utilisateur)</li>
 	<li>“<u>defaultValue</u>” : la valeur par défaut du setting si le setting n’est pas déclaré (ni sur le serveur, ni dans le fichier local App.config et seulement si le setting n’est pas obligatoire)</li>
 	<li>“<u>schemaXSD</u>” : indique le nom du fichier du schéma XSD (chemin relatif au package) que la valeur XML doit valider (seulement pour les settings de type XmlDocument ou ConfigurationSection)</li>
 	<li>“<u>ignoreLocalValue</u>” : si “true” alors la valeur définie dans le fichier local App.config est ignorée</li>
 	<li>“<u>ignoreDefaultValue</u>” : si “true” alors la valeur par défaut définie dans le manifeste est ignorée</li>
</ul>
Pour les settings <em>XmlDocument</em>, <em>ConfigurationSection</em> et <em>JsonObject</em> vous pouvez également ajouter une sous-section &lt;defaultContent&gt; sur vos settings pour définir le contenu par défaut.

Pour plus d’information, veuillez lire l'article sur le <a href="/concepts/package-manifest/">Package Manifest</a>.
<h3>Déclarez des settings dans votre configuration locale</h3>
Les settings d’un package sont déclarés au niveau du serveur Constellation mais vous pouvez également les déclarer dans le fichier App.config de votre package. Cela est très utile pendant la phase de développement.

Pour définir les settings dans le fichier App.config vous devez utiliser la section “constellationSettings". Le schéma et le fonctionnement sont exactement les mêmes que pour la configuration définie sur le serveur Constellation.

Par exemple, dans le fichier App.config de votre package, on peut écrire :
<pre class="lang:xml decode:true">&lt;constellationSettings xmlns="http://schemas.myconstellation.io/Constellation/1.8/ConstellationSettings"&gt;
  &lt;settings&gt;
    &lt;setting key="MyStringSetting" value="This is a string" /&gt;
    &lt;setting key="MyBoolSetting" value="true" /&gt;
    &lt;setting key="MyXmlDocument"&gt;
      &lt;content&gt;
        &lt;note date="09-02-2016"&gt;
          &lt;to&gt;Tove&lt;/to&gt;
          &lt;from&gt;Jani&lt;/from&gt;
          &lt;heading&gt;Reminder&lt;/heading&gt;
          &lt;body&gt;Don't forget me this weekend!&lt;/body&gt;
        &lt;/note&gt;
      &lt;/content&gt;
    &lt;/setting&gt;
    &lt;setting key="MyJsonObject"&gt;
      &lt;content&gt;
        &lt;![CDATA[
        {
          "Number": 123,
          "String" : "This is a test (local)",
          "Boolean": true
        }
        ]]&gt;
      &lt;/content&gt;
    &lt;/setting&gt;
  &lt;/settings&gt;
&lt;/constellationSettings&gt;
</pre>
La structure est présente par défaut dans les projets Constellation créés depuis le SDK Constellation pour Visual Studio.

Si un même setting (= même clé) est déclaré à la fois sur le serveur Constellation et dans le fichier local, c’est toujours la valeur définie sur le serveur qui gagne (<a href="#Resolution_des_settings">voir ci-dessous</a>).

Notez également que si un setting est déclaré comme obligatoire dans le manifeste, il est automatiquement supprimé du fichier App.config lorsque <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">vous publiez ce package depuis Visual Studio</a> (que ce soit en local ou sur un serveur). Vous pouvez donc mettre vos “credentials” ou autre information personnel pour vos développements tout en sachant qu’ils seront supprimés lorsque vous publierez votre package.
<h3>Résolution des settings</h3>
Par défaut, la valeur d’un setting est en priorité récupérée depuis la configuration du package défini sur le serveur Constellation.

Si le setting n’existe pas sur le serveur (et qu’il n’est pas déclaré comme obligatoire dans le manifeste du package), le package va chercher la valeur du setting dans le fichier de configuration local App.config.

Si le fichier local App.config ne défini pas non plus de valeur pour ce setting ou que le manifeste demande d’ignorer cette valeur (<em>ignoreLocalValue</em> à true), le package va alors chercher la valeur par défaut de ce setting dans le manifeste (attribut “<em>defaultValue</em>” ou section”<em>defaultContent</em>”).

Si le manifeste du package ne défini pas de valeur par défaut pour ce setting, ou que cette valeur doit être ignorée (<em>ignoreDefaultValue</em> à true), la valeur du setting sera nulle.

Le processus de résolution des settings peut se résumé ainsi :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/SettingResolution-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Résolution des settings" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/SettingResolution_thumb-1.png" alt="Résolution des settings" width="420" height="236" border="0" /></a></p>
Prenons un exemple pour bien comprendre. Si votre package est déclaré sur le serveur de cette façon :
<pre class="lang:default decode:true">&lt;package name="MonPackage" enable="true"&gt;
  &lt;settings&gt;
    &lt;setting key="MyStringSetting" value="Valeur du serveur" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;
</pre>
Et que le package contient dans son fichier local App.config la section suivante :
<pre class="lang:default decode:true">&lt;constellationSettings xmlns="http://schemas.myconstellation.io/Constellation/1.8/ConstellationSettings"&gt;
  &lt;settings&gt;
    &lt;setting key="MyStringSetting" value="Valeur locale" /&gt;
    &lt;setting key="MyIntSetting" value="42" /&gt;
  &lt;/settings&gt;
&lt;/constellationSettings&gt;</pre>
Avec dans son manifeste, la déclaration suivante :
<pre class="lang:default decode:true">&lt;Settings&gt;
   &lt;Setting name="MyStringSetting" type="String" description="This is a String setting" /&gt;
   &lt;Setting name="MyIntSetting" type="Int32" isRequired="false" ignoreLocalValue="true"  defaultValue="1234" /&gt;
&lt;/Settings&gt;
</pre>
Imaginez maintenant le code C# suivant :
<pre class="lang:c# decode:true">PackageHost.WriteInfo("My String = {0}", PackageHost.GetSettingValue("MyStringSetting"));
PackageHost.WriteInfo("My Int = {0}", PackageHost.GetSettingValue("MyIntSetting"));</pre>
Si vous lancez le package en debug depuis Visual Studio :
<ul>
 	<li>My String = “Valeur locale”</li>
 	<li>My Int = 123</li>
</ul>
Les valeurs sont simplement lues depuis le fichier de configuration locale. Si maintenant vous supprimez le setting dans votre App.config, la valeur de MyIntSetting sera 1234, c’est à dire la valeur par défaut du manifest.

Maintenant si vous déployez votre package dans votre Constellation :
<ul>
 	<li>My String = “Valeur du serveur”</li>
 	<li>My Int = 1234</li>
</ul>
My String est bien récupéré depuis la configuration du serveur (car prioritaire), cependant “MyIntSetting” n’étant pas  déclaré sur le serveur, le package va tenter de lire cette valeur depuis la configuration locale (123) mais le manifeste demande à ignorer la valeur locale (<em>ignoreLocalValue</em>="true" dans le manifeste) ce qui l’amène à récupérer la valeur par défaut (<em>defaultValue</em>) déclaré dans le manifeste du package. Donc MyIntSetting = 1234.

<u>Note sur le “ignoreDefaultValue”</u>

On peut se demander quel est l’intérêt de définir une valeur par défaut tout demandant de l’ignorer (<em>ignoreDefaultValue</em> = true).

En fait la valeur par défaut du manifeste peut être considérée comme un “exemple”. La <em>defaultValue</em> ou <em>defaultContent</em> est affiché à titre d’exemple à l’utilisateur lors de la configuration de son package dans la Console Constellation. Cela est surtout fort utile pour les “Setting Contents”, car ça renseigne l’utilisateur la structure du XML ou du JSON du setting.

Mais une valeur d’ “exemple” ne doit pas être chargée ! De ce fait on déclare un setting avec une defaultValue/defaultContent pour l’exemple et on demande à ne jamais tenir compte de cette valeur en l'ignorant (ignoreDefaultValue = true).

Si on veut une valeur “par défaut”, on utilisera le fichier local App.config.

En clair, on peut voir les choses ainsi :
<ul>
 	<li>Valeur déclarée dans la configuration du package au niveau du serveur Constellation = configuration de l’utilisateur</li>
 	<li>Valeur déclarée dans la configuration locale du package (App.config) = configuration pour le développement ou valeur par défaut</li>
 	<li>Valeur déclarée dans le manifeste du package (PackagInfo.xml) = exemple de configuration ou valeur par défaut</li>
</ul>
<h3>Mise à jour dynamique</h3>
Les settings d'un package sont récupérés du serveur Constellation au démarrage du package. Il est également possible de “pusher” les settings d’un package lorsque le package est en fonctionnement.

Il y a trois façons de “pusher” les settings sur un package :
<ul>
 	<li>Lors du rechargement de la configuration Constellation sur le serveur</li>
 	<li>Lors d’un “UpdateSettings” sur un package en particulier via le hub contrôle</li>
 	<li>A la demande d’un package via la méthode “RequestSettings”</li>
</ul>
Le hub de contrôle (control hub) expose une méthode “reloadServerConfiguration” permettant de recharger la configuration de la Constellation au niveau du serveur.  Cette méthode prend en paramètre un booléen (optionnel) indiquant si il faut ou non déployer la configuration après son rechargement sur le serveur. Par défaut ce paramètre est à “false” (la configuration est seulement rechargée).

Lorsque la configuration est déployée, le serveur Constellation informe toutes les sentinelles et tous les packages que la configuration a changé. Chaque package recevra la valeurs des settings déclarées dans la nouvelle configuration et chaque sentinelle se conformera à sa configuration (arrêt des packages supprimés et déploiement/démarrage des packages ajoutés).

Il est possible de lancer cette action depuis la Console Constellation (soit depuis la page “<a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/">Packages</a>” ou soit depuis la page “<a href="/constellation-platform/constellation-console/configuration-editor/">Configuration Editor</a>").
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-140.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Rechargement de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-117.png" alt="Rechargement de la configuration" width="244" height="109" border="0" /></a></p>
<p align="left">L’autre possibilité, toujours sur le hub de contrôle, avec une méthode “<em>updateSettings</em>” permettant de pusher les settings du serveur vers un package en particulier. Vous retrouverez également cette action depuis la Console Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-141.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Mise à jour des settings d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-118.png" alt="Mise à jour des settings d'un package" width="244" height="173" border="0" /></a></p>
<p align="left">Enfin, le package lui-même peut faire la demande au serveur de lui renvoyer ses settings par la méthode suivante :</p>

<pre class="lang:c# decode:true">PackageHost.RequestSettings();</pre>
<p align="left">Dans tous les cas, le dictionnaire des settings de votre package (propriété <em>PackageHost.Settings</em>) sera mis à jour de façon de transparent (en tenant compte du processus de résolution des settings décrit ci-dessus).</p>
<p align="left">Ainsi, lorsque vous appelez les méthodes <em>GetSettingXXXX</em>, elles vous retourneront toujours les dernières valeurs des settings. C'est pourquoi il est conseillé de ne jamais mettre en cache les valeurs des settings et de toujours invoquer les méthodes <em>GetSettingsXXX</em> pour tenir compte des modifications en temps réel.</p>
<p align="left">Vous avez également la possibilité de vous abonnez à l’événement “<em>PackageHost.SettingsUpdated</em>” pour être informé de ces mises à jour et agir en conséquence :</p>

<pre class="lang:c# decode:true">PackageHost.SettingsUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Mise à jours des settings");
    PackageHost.WriteInfo("Il y a {0} setting(s)", PackageHost.Settings.Count);
};</pre>
<h3>Les groupes de Settings</h3>
Vous pouvez grouper des settings (Content ou Value) dans des groupes au niveau du serveur et importer ces groupes dans les settings de vos packages ou dans d’autres groupes.

Par exemple, créons un groupe pour “HWMonitorSettings” qui contient le setting “Interval” :
<pre class="lang:xml decode:true">&lt;settingsGroups&gt;
  &lt;group name="HWMonitorSettings"&gt;
    &lt;settings&gt;
      &lt;setting key="Interval" value="500" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
&lt;/settingsGroups&gt;</pre>
Ainsi pour chaque instance du package “HWMonitor”, vous pouvez importer le groupe “HWMonitorSettings” afin de centraliser la configuration de ce package :
<pre class="lang:xml decode:true">&lt;package name="HWMonitor"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="HWMonitorSettings" /&gt;
    &lt;/import&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
Vous pouvez importer des groupes dans des groupes sans limite.

C’est toujours la valeur du setting la plus proche du package qui gagne (surcharge).

Bien entendu, vous pouvez créer autant de groupe que vous voulez et chaque groupe peut contenir des SettingValues ou des SettingContents :
<pre class="lang:xml decode:true">&lt;settingsGroups&gt;
  &lt;group name="test"&gt;
    &lt;settings&gt;
      &lt;setting key="Demo1" value="Seb" /&gt;
      &lt;setting key="Demo2" value="123" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
  &lt;group name="test2"&gt;
    &lt;settings&gt;
      &lt;import&gt;
        &lt;settingGroup groupName="common" /&gt;
      &lt;/import&gt;
      &lt;setting key="Demo1" value="Sebastien" /&gt;
      &lt;setting key="Demo2" value="2015" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
  &lt;group name="common"&gt;
    &lt;settings&gt;
      &lt;setting key="MyStringSetting" value="This is a string" /&gt;
      &lt;setting key="MyBoolSetting" value="true" /&gt;
      &lt;setting key="MyXmlDocument"&gt;
        &lt;content&gt;
          &lt;note date="09-02-2016"&gt;
            &lt;to&gt;Tove&lt;/to&gt;
            &lt;from&gt;Jani&lt;/from&gt;
            &lt;heading&gt;Reminder&lt;/heading&gt;
            &lt;body&gt;Don't forget me this weekend!&lt;/body&gt;
          &lt;/note&gt;
        &lt;/content&gt;
      &lt;/setting&gt;
      &lt;setting key="MyJsonObject"&gt;
        &lt;content&gt;
          &lt;![CDATA[
        {
          "Number": 123,
          "String" : "This is a test (local)",
          "Boolean": true
        }
        ]]&gt;
        &lt;/content&gt;
      &lt;/setting&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
&lt;/settingsGroups&gt;
</pre>
Pour bien comprendre, imaginez le package suivant :
<pre class="lang:xml decode:true">&lt;package name="DemoPackage"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="test" /&gt;
      &lt;settingGroup groupName="test2" /&gt;
    &lt;/import&gt;
    &lt;setting key="numberTest" value="42" /&gt;
    &lt;setting key="Demo1" value="It’me" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
Ici cette instance du package “DemoPackage” contient 7 settings (sans prendre en compte les settings déclarés dans le fichier local et le manifeste) :
<ul>
 	<li>numberTest = 42 (défini dans les settings du package)</li>
 	<li>Demo1 = “It’s me” (défini dans les settings du package, cette valeur écrase celle des groupes importés)</li>
 	<li>Demo2 = 2015 (défini par le groupe “test2”. Cette valeur écrase la valeur du groupe “test”, car le groupe “test2” est importé APRES le groupe “test”)</li>
 	<li>MyStringSetting, MyBoolSetting, MyXmlDocument et MyJsonObject définis par le groupe “common” (groupe importé dans le groupe “test2” lui même importé sur le package “DemoPackage”).</li>
</ul>