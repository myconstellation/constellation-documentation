---
ID: 1297
post_title: 'Settings : param&egrave;tres de configuration de vos packages'
author: Sebastien Warin
post_date: 2016-03-16 13:15:58
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/settings/
published: true
post_modified: 2016-08-11 15:12:45
---
<p>Chaque package peut définir des paramètres de configuration au niveau du serveur Constellation.</p> <p>Cela vous permet de changer ses paramètres directement depuis la Constellation qui se chargera de redescendre ces paramètres sur vos packages.</p> <p>Il y a deux types de settings :</p> <ul> <li>Les “Setting Value” : il s’agit d’un couple clé/value à l’instar des &lt;appSettings&gt; d’une application .NET  <li>Les “Setting Content”&nbsp; : au lieu de définir la valeur d’un paramètre dans un attribut XML, on peut la définir dans un élément XML enfant permettant d’avoir des settings qui renferme du XML ou JSON </li></ul> <h3>Les Settings Values</h3> <p>Voici par exemple des “SettingValues”&nbsp; déclarés dans notre configuration :</p><pre class="lang:default decode:true ">&lt;setting key="MyBoolSetting" value="true" /&gt;      
&lt;setting key="MyStringSetting" value="This is a string" /&gt;
&lt;setting key="MyIntSetting" value="123" /&gt;</pre>
<p>Ces trois settings définissent la valeur dans l’attribut “value” (= SettingValue).</p>
<p>Pour récupérer la valeur du paramètre “MyStringSetting”&nbsp; dans votre code, utilisez la méthode “GetSettingValue” :</p><pre class="lang:c# decode:true">PackageHost.WriteInfo("My String = {0}", PackageHost.GetSettingValue("MyStringSetting"));</pre>
<p>La méthode “GetSettingValue” vous retourne par défaut la valeur d’un setting en “string” mais vous pouvez également convertir cette valeur en utilisant la forme générique :</p><pre class="lang:c# decode:true">int myIntSetting = PackageHost.GetSettingValue&lt;int&gt;("MyIntSetting");
bool myBoolSetting = PackageHost.GetSettingValue&lt;bool&gt;("MyBoolSetting");</pre>
<h3>Les Settings Contents</h3>
<p>Si vous avez un modèle de configuration plus compliqué qu’une série de clé/valeur vous pouvez utiliser les “Setting Contents” pour définir du XML ou JSON comme valeur de setting.</p>
<p>Par exemple la configuration de votre package peut définir deux autres settings, l’un contenant du XML et l’autre du JSON de cette façon :</p><pre class="lang:default decode:true">&lt;setting key="MyXmlDocument"&gt;
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
<p>Voici les méthodes pour récupérer ces settings :</p>
<ul>
<li><u>GetSettingValue</u> : récupérer le contenu brute de votre setting sous forme d’un string 
<li><u>GetSettingAsJsonObject</u> : dé-sérialise le contenu JSON de votre setting et vous retourne un objet dynamique 
<li><u>GetSettingAsJsonObject&lt;T&gt;</u> : dé-sérialise le contenu JSON de votre setting et le convertie dans un objet de votre type (T) 
<li><u>GetSettingAsXmlDocument</u> : dé-sérialise le contenu XML de votre setting et vous retourne un XmlDocument 
<li><u>GetSettingAsConfigurationSection&lt;TConfigurationSection&gt;</u> : dé-sérialise le contenu XML de votre setting sous forme d’un ConfigurationSection .NET </li></ul>
<p>Par exemple, pour manipuler le setting XML on pourrait écrire :</p><pre class="lang:c# decode:true">var xml = PackageHost.GetSettingAsXmlDocument("MyXmlDocument");
PackageHost.WriteInfo("My XmlDocument Attribute = {0} , first node value = {1}", xml.ChildNodes[0].Attributes["date"].Value, xml.ChildNodes[0].FirstChild.InnerText);</pre>
<p>Autre exemple avec le setting JSON :</p><pre class="lang:c# decode:true">dynamic json = PackageHost.GetSettingAsJsonObject("MyJsonObject");
PackageHost.WriteInfo("My JsonObject String={0}, Int={1}, Boolean={2}", json.String, json.Number, json.Boolean);
</pre>
<h3>TryGetSettings</h3>
<p>Notez que toutes ces méthodes citées ci-dessus ont leurs équivalents en “Try” :</p>
<ul>
<li>TryGetSettingAsConfigurationSection&lt;TConfigurationSection&gt; 
<li>TryGetSettingAsJsonObject 
<li>TryGetSettingAsJsonObject&lt;T&gt; 
<li>TryGetSettingAsXmlDocument 
<li>TryGetSettingValue&lt;T&gt; </li></ul>
<p>Ces méthodes tentent de récupérer et convertir le setting dans le type souhaité et place le résultat dans un paramètre de sortie (“out”) et vous retourne un booléen indiquant si l’opération est réussie ou non.</p>
<p>Par exemple :</p><pre class="lang:c# decode:true">int monParametre = 0;
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
<p>Tous les settings doivent être déclarés dans le manifeste du package (fichier PackageInfo.xml).</p>
<p>La déclaration des settings dans le manifeste n’a pas impact dans le fonctionnement du package mais il permet de décrire la configuration du package pour que des outils tel que la Console puisse proposer une interface graphique de configuration pour chaque package. Il est donc vivement recommandé de décrire les settings de ses packages. De plus il est possible de définir des valeurs par défaut dans le manifeste.</p>
<p>Chaque setting déclaré dans le manifeste doit obligatoirement comporter les attributs suivants&nbsp; :</p>
<ul>
<li><u>Name</u> : le nom (clé) du setting 
<li><u>Type</u> : le type du setting </li></ul>
<p>Le type peut être :</p>
<ul>
<li><u>Boolean</u> : un booléen (true/false) 
<li><u>Double</u> : un double 
<li><u>String</u> : un chaine de caractère 
<li><u>Int32</u> : un entier (32 bits) 
<li><u>Int64</u> : un long (64 bits) 
<li><u>ConfigurationSection</u> : un section de configuration .NET 
<li><u>DateTime</u> : un DateTime 
<li><u>TimeSpan</u> : une durée 
<li><u>XmlDocument</u> : un document XML 
<li><u>JsonObject</u> : un objet JSON (objet ou tableau) </li></ul>
<p>Ensuite vous pouvez définir d’autre attribut sur vos settings :</p>
<ul>
<li>“<u>isRequiered</u>“ : indique si le setting est obligatoire ou non (par défaut “false”) 
<li>“<u>description</u>” : permettant de donner une explication sur le setting (qui sera affiché à l’utilisateur) 
<li>“<u>default</u>Value” : la valeur par défaut du setting si le setting n’est pas déclaré (ni sur le serveur, ni dans le fichier local App.config et seulement si le setting n’est pas obligatoire) 
<li>“<u>schemaXSD</u>” : indique le nom du fichier du schéma XSD (chemin relatif au package) que la valeur XML doit valider (seulement pour les settings XmlDocument ou ConfigurationSection) 
<li>“<u>ignoreLocalValue</u>” : si “true” alors la valeur définie dans le fichier local App.config est ignorée 
<li>“<u>ignoreDefaultValue</u>” : si “true” alors la valeur par défaut définie dans le manifeste est ignorée </li></ul>
<p>Pour les settings XmlDocument, ConfigurationSection et JsonObject vous pouvez également ajouter une sous-section &lt;defaultContent&gt; sur vos settings pour définir le contenu par défaut.</p>
<p>A lire également : <a href="/concepts/package-manifest/">le Package Manifest</a></p>
<h3>Déclarez des settings dans votre configuration local</h3>
<p>Les settings d’un package sont déclarés au niveau du serveur dans la déclaration du package mais vous pouvez également les déclarés dans le fichier App.config de votre package. Cela est très utile pendant la phase de développement.</p>
<p>Pour définir les settings dans le fichier App.config vous devez utiliser la section “constellationSettings". Le schéma et le fonctionnement sont exactement les mêmes sur la configuration définie sur le serveur Constellation.</p>
<p>Par exemple :</p><pre class="lang:xml decode:true">&lt;constellationSettings xmlns="http://schemas.myconstellation.io/Constellation/1.8/ConstellationSettings"&gt;
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
<p>La structure est présente par défaut dans les projets Constellation créés depuis le SDK Constellation pour Visual Studio.</p>
<p>Si un setting est déclaré pour un package sur le serveur et également dans le fichier local, c’est toujours la valeur définie sur le serveur qui gagne (<a href="#Resolution_des_settings">voir ci-dessous</a>).</p>
<p>Notez également que si un setting est déclaré comme obligatoire dans le manifeste, il est automatiquement supprimé du fichier App.config lorsque vous publierez ce package depuis Visual Studio (que ce soit en local ou sur un serveur). Vous pouvez donc mettre pour “credential” ou autre information personnel pour vos développements tout en sachant qu’ils seront supprimés lorsque vous publierez votre package.</p>
<h3>Résolution des settings</h3>
<p>Par défaut, la valeur d’un setting est récupérée depuis la configuration du package définie sur le serveur Constellation.</p>
<p>Si le setting n’existe pas sur le serveur (et qu’il n’est pas déclaré comme obligatoire dans le manifeste du package), l’API .NET va chercher la valeur du setting dans le fichier local App.config.</p>
<p>Si le fichier local App.config ne défini pas non plus de valeur pour ce setting ou que le manifeste demande d’ignorer cette valeur (ignoreLocalValue à true), l’API .NET va chercher la valeur par défaut du setting dans le manifeste (attribut “defaultValue” ou section”defaultContent”).</p>
<p>Si le manifeste du package ne défini pas ,on plus de valeur par défaut ou que cette valeur doit être ignorée (ignoreDefaultValue à true), la valeur du setting sera nulle.</p>
<p>Le processus de résolution des settings peut se résumer ainsi :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/SettingResolution-1.png"><img title="R&eacute;solution des settings" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="R&eacute;solution des settings" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/SettingResolution_thumb-1.png" width="420" height="236"></a></p>
<p>Prenons un exemple.. Si votre package est déclaré sur le serveur de cette façon :</p><pre class="lang:default decode:true">&lt;package name="MonPackage" enable="true"&gt;
  &lt;settings&gt;
    &lt;setting key="MyStringSetting" value="Valeur du serveur" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;
</pre>
<p>Que le package contient dans son fichier App.config, la section suivante :</p><pre class="lang:default decode:true">&lt;constellationSettings xmlns="http://schemas.myconstellation.io/Constellation/1.8/ConstellationSettings"&gt;
  &lt;settings&gt;
    &lt;setting key="MyStringSetting" value="Valeur locale" /&gt;
    &lt;setting key="MyIntSetting" value="42" /&gt;
  &lt;/settings&gt;
&lt;/constellationSettings&gt;</pre>
<p>Et pour finir, son manifeste déclare les settings de cette façon :</p><pre class="lang:default decode:true">&lt;Settings&gt;
   &lt;Setting name="MyStringSetting" type="String" description="This is a String setting" /&gt;
   &lt;Setting name="MyIntSetting" type="Int32" isRequired="false" ignoreLocalValue="true"  defaultValue="1234" /&gt;
&lt;/Settings&gt;
</pre>
<p>Imaginez maintenant le code suivant :</p><pre class="lang:c# decode:true">PackageHost.WriteInfo("My String = {0}", PackageHost.GetSettingValue("MyStringSetting"));
PackageHost.WriteInfo("My Int = {0}", PackageHost.GetSettingValue("MyIntSetting"));</pre>
<p>Si vous lancez le package en debug depuis Visual Studio :</p>
<ul>
<li>My String = “Valeur locale” 
<li>My Int = 123 </li></ul>
<p>Les valeurs sont simplement lues depuis le fichier de configuration locale. Si maintenant vous supprimez, le setting dans votre App.config, la valeur de MyIntSetting sera 1234, c’est à dire la valeur par défaut du manifest.</p>
<p>Maintenant si vous déployez votre package dans votre Constellation :</p>
<ul>
<li>My String = “Valeur du serveur” 
<li>My Int = 1234 </li></ul>
<p>My String est bien récupéré de la configuration du serveur, cependant “MyIntSetting” n’est pas&nbsp; déclaré sur le serveur, donc le package va tenter de lire la valeur de la configuration locale (123) mais le manifeste lui demander d’ignorer la valeur locale (ignoreLocalValue="true" dans le manifeste) ce qui l’amène à récupérer la valeur par défaut (defaultValue) dans le manifeste du package. Donc MyIntSetting = 1234.</p>
<p><u>Note sur le “ignoreDefaultValue”</u></p>
<p>On peut se demander quel est l’intérêt de définir une valeur par défaut et l’ignorer (ignoreDefaultValue = true).</p>
<p>En fait la valeur par défaut du manifeste peut être considérer comme un “exemple”. La defaultValue/defaultContent est affiché à titre d’exemple à l’utilisateur lors de la configuration de son package dans la Console Constellation. Cela est surtout fort utile pour les “Setting Contents”, car ca indique à l’utilisateur la structure du XML ou du JSON du setting.</p>
<p>Mais une valeur d’ “exemple” ne doit pas être chargée ! De ce fait on déclare un setting avec une defaultValue/defaultContent pour l’exemple et on demande à ne jamais charger cette valeur (ignoreDefaultValue = true).</p>
<p>Si on veut une valeur “par défaut”, on utilisera le fichier local App.config.</p>
<p>En clair, on peut voir les chose ainsi :</p>
<ul>
<li>Valeur déclarée dans la configuration du package au niveau du serveur Constellation = configuration de l’utilisateur 
<li>Valeur déclarée dans la configuration locale du package (App.config) = configuration pour le développement ou valeur par défaut 
<li>Valeur déclarée dans le manifeste du package (PackagInfo.xml) = exemple de configuration ou valeur par défaut </li></ul>
<h3>Mise à jour dynamique</h3>
<p>Les settings déclarés sur le serveur sont récupérés par le package à son démarrage. Il est également possible de “pusher” les settings d’un package lorsque le package est en fonctionnement.</p>
<p>Il y a trois façons de “pusher” les settings sur un package :</p>
<ul>
<li>Lors du déploiement de la configuration Constellation 
<li>Lors d’un “UpdateSettings” sur un package en particulier via le hub contrôle 
<li>A la demande d’un package via la méthode “RequestSettings” </li></ul>
<p>Le hub de contrôle (control hub) expose une méthode “reloadServerConfiguration” permettant de recharger la configuration de la Constellation au niveau du serveur. Les packages et leurs settings seront donc montés en mémoire mais les packages déjà lancés n’ont pas ces nouvelles valeurs.</p>
<p>Cette méthode prend en paramètre un booléen (optionnel) indiquant si il faut ou non déployer la configuration après son rechargement sur le serveur. Par défaut ce paramètre est à “false” (la configuration est seulement rechargée).</p>
<p>Lorsque la configuration est déployée, le serveur Constellation informe toutes les sentinelles et tous les packages que la configuration a changé. Chaque package actualisera ses settings selon la configuration définie sur le serveur et chaque sentinelle se conformera à sa configuration (arret des packages supprimés et déploiement/démarrage des packages ajoutés).</p>
<p>Il est également possible de lancer cette action depuis la Console Constellation (soit depuis la page “Packages” ou soit depuis la page “Configuration Editor).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-140.png"><img title="Rechargement de la configuration" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Rechargement de la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-117.png" width="244" height="109"></a></p>
<p align="left">L’autre possibilité, toujours sur le hub de contrôle, vous trouverez une méthode “updateSettings” permettant de pusher les settings du serveur vers un package en particulier. Vous retrouverez également cette action depuis la Console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-141.png"><img title="Mise &agrave; jour des settings d'un package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Mise &agrave; jour des settings d'un package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-118.png" width="244" height="173"></a></p>
<p align="left">Enfin, le package lui-même peut faire la demande au serveur de lui envoyer ses settings par la ligne :</p><pre class="lang:c# decode:true">PackageHost.RequestSettings();</pre>
<p align="left">Dans tous les cas, le dictionnaire des settings de votre package (propriété <em>PackageHost.Settings</em>) est mis à jour de façon de transparente de sorte que lorsque vous appelez les méthodes GetSettingXXXX, elles vous retourneront les dernières valeurs des settings.</p>
<p align="left">Vous avez également la possibilité de vous abonnez à l’évènement “PackageHost.SettingsUpdated” pour vous informer d’une mise à jour des settings par le serveur.</p><pre class="lang:c# decode:true">PackageHost.SettingsUpdated += (s, e) =&gt;
{
    PackageHost.WriteInfo("Mise à jours des settings");
    PackageHost.WriteInfo("Il y a {0} setting(s)", PackageHost.Settings.Count);
};</pre>
<h3>Les groupes de Settings</h3>
<p>Vous pouvez grouper des settings (Content ou Value) dans des groupes au niveau du serveur et importer ces groupes dans les settings de vos packages ou dans d’autres groupes.</p>
<p>Par exemple, créons un groupe pour “HWMonitorSettings” qui contient le setting “Interval” :</p><pre class="lang:xml decode:true">&lt;settingsGroups&gt;
  &lt;group name="HWMonitorSettings"&gt;
    &lt;settings&gt;
      &lt;setting key="Interval" value="500" /&gt;
    &lt;/settings&gt;
  &lt;/group&gt;
&lt;/settingsGroups&gt;</pre>
<p>Ainsi pour chaque instance du package “HWMonitor”, vous pouvez importer le groupe “HWMonitorSettings” afin de centraliser la configuration de ce package :</p><pre class="lang:xml decode:true">&lt;package name="HWMonitor"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="HWMonitorSettings" /&gt;
    &lt;/import&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<p>Vous pouvez importer des groupes dans des groupes sans limite.</p>
<p>C’est toujours la valeur du setting la plus proche du package qui gagne (surcharge).</p>
<p>Bien entendu, vous pouvez créer autant de groupe que vous voulez et chaque groupe peut contenir des SettingValue ou des SettingContent :</p><pre class="lang:xml decode:true">&lt;settingsGroups&gt;
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
<p>Pour bien comprendre, imaginez le package suivant :</p><pre class="lang:xml decode:true">&lt;package name="DemoPackage"&gt;
  &lt;settings&gt;
    &lt;import&gt;
      &lt;settingGroup groupName="test" /&gt;
      &lt;settingGroup groupName="test2" /&gt;
    &lt;/import&gt;
    &lt;setting key="numberTest" value="42" /&gt;
    &lt;setting key="Demo1" value="It’me" /&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<p>Ici cette instance du package “DemoPackage” définie au niveau du serveur (= sans prendre en compte les settings déclarés dans le fichier local et le manifeste) 7 settings :</p>
<ul>
<li>numberTest = 42 (définit dans les settings du package) 
<li>Demo1 = “It’s me” (définit dans les settings du package, cette valeur écrase celle des groupes importés) 
<li>Demo2 = 2015 (définit par le groupe “test2”. Cette valeur écrase la valeur du groupe “test”, car le groupe “test2” est importé APRES le groupe “test”) 
<li>MyStringSetting, MyBoolSetting, MyXmlDocument et MyJsonObject définies par le groupe “common” (groupe importé dans le groupe “test2” lui même importé sur le package “DemoPackage”). </li></ul>