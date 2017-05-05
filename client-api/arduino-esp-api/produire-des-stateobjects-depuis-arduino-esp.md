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
post_modified: 2017-05-05 09:38:36
---
<h3>Publication de valeur “simple”</h3>

Pour produire et publier un StateObject dans votre Constellation vous devez invoquer la méthode “pushStateObject” en spécifiant au minimum le nom du StateObject et sa valeur :

<pre class="lang:cpp decode:true">constellation.pushStateObject(name, value);</pre>

Vous pouvez aussi spécifier “type” de votre StateObject :

<pre class="lang:cpp decode:true">constellation.pushStateObject(name, value, type);</pre>

Notez que pour les types simples (int,bool, long, float et double) le type est spécifié implicitement. Par exemple, pour publier un StateObject dont la valeur est un chiffre :

<pre class="lang:cpp decode:true">constellation.pushStateObject("Temperature", 21);</pre>

Vous pouvez aussi spécifier le “lifetime” de votre StateObject, c’est à dire sa durée de vie (en seconde) avant qu’il soit considéré comme “expiré”. Par exemple si votre valeur de T° (variable ‘temp’) est considérée comme valide que 60 secondes, on écrira :

<pre class="lang:cpp decode:true">constellation.pushStateObject("Temperature", temp, 60);</pre>

En ouvrant le “StateObject Explorer” de la Console Constellation vous retrouverez votre StateObject :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-2.png" alt="image" width="350" height="251" border="0" /></a></p>

<p style="text-align: left;" align="center">Si le StateObject n'est pas mis à jour dans les 60 secondes il sera marqué comme expiré.</p>

<h3>Publication de valeur “complexe”</h3>

<h4>Avec un “string format”</h4>

Vous pouvez publier des StateObject avec un objet complexe en utilisant la fonction “stringFormat” pour formater votre valeur en JSON. Par exemple pour publier un StateObject nommé “Lux” étant un objet contenant 3 propriétés, on pourrait écrire :

<pre class="lang:cpp decode:true">constellation.pushStateObject("Lux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir));</pre>

<h4>Avec un JsonObject</h4>

Vous pouvez également faire la même chose en créant un JsonObject de la façon suivante :

<pre class="lang:cpp decode:true">StaticJsonBuffer&lt;JSON_OBJECT_SIZE(3)&gt; jsonBuffer;
JsonObject&amp; myStateObject = jsonBuffer.createObject();
myStateObject["Lux"] = lux;
myStateObject["Broadband"] = full;
myStateObject["IR"] = ir;
constellation.pushStateObject("Lux", myStateObject);</pre>

Sur l’explorateur de StateObjects de la Console :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-3.png" alt="image" width="350" height="252" border="0" /></a></p>

<h3>Définir la durée de vie de votre StateObject</h3>

Comme pour une valeur simple, vous pouvez également spécifier un “type” et le “lifetime” de votre StateObject. Par exemple :

<pre class="lang:cpp decode:true">constellation.pushStateObject("DemoLux", stringFormat("{ 'Lux':%d, 'Broadband':%d, 'IR':%d }", lux, full, ir), "MyLuxData", 20);</pre>

Ou en utilisant le JsonObject “myStateObject” créé ci-dessus :

<pre class="lang:cpp decode:true">constellation.pushStateObject("DemoLux", myStateObject, "MyLuxData", 20);</pre>

Dans les deux cas nous avons publié un StateObject nommé “DemoLux” de type “MyLuxData” avec une durée de vie de 20 secondes contenant comme valeur un objet avec trois propriétés (Lux, Broadband et IR).

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-4.png" alt="image" width="350" height="140" border="0" /></a></p>

<h3>Décrire ses types complexes</h3>

Lorsque vous utilisez des types personnalisés (ici “MyLuxData”) il vivement recommandé de les décrire dans le “<a href="/concepts/messaging-message-scope-messagecallback-saga/#Auto-description_des_MessageCallbacks">Package Descriptor</a>”.

Pour ce faire, au démarrage, invoquez la méthode “addStateObjectType” pour chaque type à déclarer puis appelez la méthode “declarePackageDescriptor” pour envoyer le “Package Descriptor” dans la Constellation.

Dans notre exemple nous écrirons :

<pre class="lang:cpp decode:true">// Describe your custom StateObject types  
constellation.addStateObjectType("MyLuxData", TypeDescriptor().setDescription("MyLuxData demo").addProperty("Broadband", "System.Int32").addProperty("IR", "System.Int32").addProperty("Lux", "System.Int32")); 

// Declare the package descriptor
constellation.declarePackageDescriptor();</pre>

Vous remarquerez que le type “MyLuxData” est surligné vous permettant d’afficher le descriptif de votre StateObject :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-5.png" alt="image" width="350" height="124" border="0" /></a></p>

<h3>Ajout de metadatas</h3>

Pour finir vous pouvez également ajouter des méta-données sur vos StateObjects.

Par exemple ajoutons l’ID de la puce ESP (ChipID) et le timestamp (millis) dans les méta-données de notre StateObject :

<pre class="lang:cpp decode:true">const int BUFFER_SIZE = JSON_OBJECT_SIZE(6);
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

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-6.png" alt="image" width="254" height="321" border="0" /></a></p>

Note : dans ce dernier exemple vous remarquerez que nous avons défini la taille du buffer JSON (<em>BUFFER_SIZE</em>) à “JSON_OBJECT_SIZE(6)”, c’est à dire 6 propriétés et non 3 comme dans les exemples précédents de façon à tenir compte des propriétés JSON rajoutées par les metadatas (l’objet “Metadatas” lui même et ses deux propriétés “ChipId” et “Timestamp”). Pour plus d’info, consultez la documentation d’<a href="https://github.com/bblanchon/ArduinoJson/wiki">ArduinJson</a> et plus particulièrement l’article concernant <a href="https://github.com/bblanchon/ArduinoJson/wiki/Memory%20model">la gestion de la mémoire</a>.