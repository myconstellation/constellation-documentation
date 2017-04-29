---
ID: 2455
post_title: >
  Produire des StateObjects depuis un
  Arduino/ESP
author: Sebastien Warin
post_date: 2016-08-23 14:17:28
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/arduino-esp-api/produire-des-stateobjects-depuis-arduino-esp/
published: true
post_modified: 2016-09-16 15:37:09
---
<h3>Publication de valeur “simple”</h3> <p>Pour produire et publier un StateObject dans votre Constellation vous devez invoquer la méthode “pushStateObject” :</p><pre class="lang:cpp decode:true">constellation.pushStateObject(name, value);</pre>
<p>Vous pouvez aussi passer le “type” de votre StateObject :</p><pre class="lang:cpp decode:true">constellation.pushStateObject(name, value, type);</pre>
<p>Notez que pour les types simples (int,bool, long, float et double) le type est spécifié implicitement. Par exemple, pour publier un StateObject dont la valeur est un chiffre :</p><pre class="lang:cpp decode:true">constellation.pushStateObject("Temperature", 21);</pre>
<p>Vous pouvez aussi spécifier le “lifetime” de votre StateObject, c’est à dire sa durée de vie (en seconde) avant qu’il soit considéré comme “expiré”. Par exemple si votre valeur de T° (variable ‘temp’) n’est considéré comme valide que 60 secondes :</p><pre class="lang:cpp decode:true">constellation.pushStateObject("Temperature", temp, 60);</pre>
<p>En ouvrant le “StateObject Explorer” de la Console Constellation vous retrouverez votre StateObject :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-2.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-2.png" width="350" height="251"></a></p>
<h3>Publication de valeur “complexe”</h3>
<h4>Avec un “string format”</h4>
<p>Vous pouvez publier des StateObject avec un objet complexe en utilisant la fonction “stringFormat” pour formater votre valeur en JSON. Par exemple pour publier un StateObject nommé “Lux” étant un objet contenant 3 propriétés :</p><pre class="lang:cpp decode:true">constellation.pushStateObject("Lux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir));</pre>
<h4>Avec un JsonObject</h4>
<p>Vous pouvez également faire la même chose en créant un JsonObject :</p><pre class="lang:cpp decode:true">StaticJsonBuffer&lt;JSON_OBJECT_SIZE(3)&gt; jsonBuffer;
JsonObject&amp; myStateObject = jsonBuffer.createObject();
myStateObject["Lux"] = lux;
myStateObject["Broadband"] = full;
myStateObject["IR"] = ir;
constellation.pushStateObject("Lux", myStateObject);</pre>
<p>Sur l’explorateur de StateObjects de la Console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-3.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-3.png" width="350" height="252"></a></p>
<h3>Définir la durée de vie de votre StateObject</h3>
<p>Comme pour une valeur simple vous pouvez également spécifier un “type” et le “lifetime” de votre StateObject. Par exemple :</p><pre class="lang:cpp decode:true">constellation.pushStateObject("DemoLux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir), "MyLuxData", 20);</pre>
<p>Ou en utilisant le JsonObject “myStateObject” créé ci-dessus :</p><pre class="lang:cpp decode:true">constellation.pushStateObject("DemoLux", myStateObject, "MyLuxData", 20);</pre>
<p>Dans les deux cas nous avons publié un StateObject nommé “DemoLux” de type “MyLuxData” pour une durée de vie de 20 secondes contenant comme valeur un objet avec trois propriétés (Lux, Broadband et IR).</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-4.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-4.png" width="350" height="140"></a></p>
<h3>Décrire ses types complexes</h3>
<p>Lorsque vous utilisez des types personnalisés (ici “MyLuxData”) il vivement recommandé de les décrire dans le “package descriptor”.</p>
<p>Pour ce faire, au démarrage de votre code invoquer la méthode “addStateObjectType” avant de faire un “declarePackageDescriptor” pour envoyer le “Package Descriptor” dans la Constellation.</p>
<p>Dans notre exemple nous écrirons :</p><pre class="lang:cpp decode:true">// Describe your custom StateObject types  
constellation.addStateObjectType("MyLuxData", TypeDescriptor().setDescription("MyLuxData demo").addProperty("Broadband", "System.Int32").addProperty("IR", "System.Int32").addProperty("Lux", "System.Int32"));	

// Declare the package descriptor
constellation.declarePackageDescriptor();</pre>
<p>Vous remarquerez que le type “MyLuxData” est surligné vous permettant d’afficher le descriptif de votre StateObject :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-5.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-5.png" width="350" height="124"></a></p>
<h3>Ajout de metadatas</h3>
<p>Pour finir vous pouvez également ajouter des méta-données sur vos StateObjects.</p>
<p>Par exemple ajoutons l’ID de la puce ESP (ChipID) et le timestamp (millis) dans les méta-données de notre StateObject :</p><pre class="lang:cpp decode:true">const int BUFFER_SIZE = JSON_OBJECT_SIZE(6);
StaticJsonBuffer&lt;BUFFER_SIZE&gt; jsonBuffer;
JsonObject&amp; myStateObject = jsonBuffer.createObject();
myStateObject["Lux"] = lux;
myStateObject["Broadband"] = full;
myStateObject["IR"] = ir;
JsonObject&amp; metadatas = jsonBuffer.createObject();
metadatas["ChipId"] = ESP.getChipId();
metadatas["Timestamp"] = millis();
constellation.pushStateObject("Lux", myStateObject, "MyLuxData", 20, &amp;metadatas);
</pre>



<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-6.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-6.png" width="254" height="321"></a></p>
<p>Note : dans ce dernier exemple vous remarquerez que nous avons défini la taille du buffer JSON (<em>BUFFER_SIZE</em>) à “JSON_OBJECT_SIZE(6)”, c’est à dire 6 propriétés et non 3 comme dans les exemples précédents de façon à tenir compte des propriétés JSON rajoutées par les metadatas (l’objet “Metadatas” lui même et ses deux propriétés “ChipId” et “Timestamp”). Pour plus d’info, consultez la documentation d’<a href="https://github.com/bblanchon/ArduinoJson/wiki">ArduinJson</a> et plus particulièrement l’article concernant <a href="https://github.com/bblanchon/ArduinoJson/wiki/Memory%20model">la gestion de la mémoire</a>.</p>