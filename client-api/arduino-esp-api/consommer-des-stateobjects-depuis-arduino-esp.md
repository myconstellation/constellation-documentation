---
ID: 2463
post_title: >
  Consommer des StateObjects depuis un
  Arduino/ESP
author: Sebastien Warin
post_date: 2016-08-23 14:20:32
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/arduino-esp-api/consommer-des-stateobjects-depuis-arduino-esp/
published: true
post_modified: 2017-09-06 11:37:27
---
<h3>Interroger des StateObjects à un instant T</h3>
Pour récupérer un ou plusieurs StateObject (à un instant T), vous pouvez utiliser la méthode “requestStateObjects” :
<pre class="lang:cpp decode:true">JsonArray&amp; myStateObject = constellation.requestStateObjects(sentinel, package, name, type);</pre>
Utilisez le wildcard “*” pour ne pas appliquer de filtre. Le symbole “*” est aussi accessible via la constante “WILDCARD”.

Par exemple :
<pre class="lang:cpp decode:true">// All StateObjects for MyPackage on MySentinel
JsonArray&amp; so = constellation.requestStateObjects("MySentinel", "MyPackage", "*", "*");
// All StateObjects named "Demo" in your Constellation
JsonArray&amp; so = constellation.requestStateObjects(WILDCARD, WILDCARD, "Demo", WILDCARD);</pre>
Autre exemple, vous souhaitez afficher dans les logs Constellation, la consommation CPU de toutes vos machines Windows (vous avez au préalable déployé le package “HWMonitor” sur toutes vos sentinelles Windows) depuis votre Arduino :
<pre class="lang:cpp decode:true">// Example : print the all SO's value named "/intelcpu/0/load/0" and produced by the "HWMonitor" package (on all sentinels)
JsonArray&amp; cpus = constellation.requestStateObjects("*", "HWMonitor", "/intelcpu/0/load/0");
for(int i=0; i &lt; cpus.size(); i++) {	
  constellation.writeInfo("CPU on %s is currently %d %", cpus[i]["SentinelName"].as&lt;char *&gt;(), cpus[i]["Value"]["Value"].as&lt;float&gt;());
}</pre>
<h3>S’abonner aux mises à jours des StateObjects en temps réel</h3>
Vous pouvez également vous abonnez aux mises à jour des StateObjects pour être notifié en temps réel de la modification des StateObjects.
<h4>En utilisant un callback global</h4>
Vous pouvez déclarer une fonction callback qui sera appelée à chaque mise à jour de StateObjects pour lesquels vous vous êtes abonné en invoquant la méthode <em>setStateObjectUpdateCallback</em>. Notez qu'il ne peut y avoir qu'une seule méthode callback enregistrée.

Ensuite vous abonnez aux différents StateObjects que vous souhaitez “suivre” avec la méthode “<em>subscribeToStateObjects</em>”.

Par exemple, pour être notifié en temps réel de la consommation CPU des différentes sentinelles Windows, on écrira :
<pre class="lang:cpp decode:true">// set a StateObject update callback and subscribe to SO
constellation.setStateObjectUpdateCallback([] (JsonObject&amp; so) {
    constellation.writeInfo("StateObject updated ! StateObject name = %s", so["Name"].as&lt;char *&gt;()); 
});
// Subscribe to SO named "/intelcpu/0/load/0" and produced by the "HWMonitor" package (on all sentinels)
constellation.subscribeToStateObjects("*", "HWMonitor", "/intelcpu/0/load/0");</pre>
Nous avons ici défini notre fonction callback dans une expression lambda mais vous pouvez aussi bien “extraire” cette fonction callback dans une fonction distincte.

Enfin dans la boucle principale (loop), vous devez invoquer la méthode “<em>constellation.loop()</em>” aussi souvent que possible afin que la libraire puisse dispatcher les SO dans votre méthode callback :
<pre class="lang:cpp decode:true">constellation.loop();</pre>
<h4>En utilisant des StateObjectLinks</h4>
Avec la méthode “<em>subscribeToStateObjects</em>” présentée ci-dessous vous avez une fonction callback qui recevra tous les StateObjects pour lesquels vous êtes abonné. Vous devez donc “dispatcher” vous même les différents StateObjects que vous recevrez.

Pour simplifier les choses, la libraire Arduino propose comme en <a href="/client-api/net-package-api/consommer-des-stateobjects/#Les_StateObjectLink">.NET</a> ou en <a href="/client-api/python-api/consommer-des-stateobjects-en-python/">Python</a>, la notion de StateObjectLink : il s’agit de “lier” un abonnement à des StateObjects à une fonction de votre code.

Pour reprendre l’exemple du suivi des CPU en temps réel, nous pourrions écrire :
<pre class="lang:cpp decode:true">constellation.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", [](JsonObject&amp; so) {
  constellation.writeInfo("CPU on %s is currently %d %", so["SentinelName"].as&lt;char *&gt;(), so["Value"]["Value"].as&lt;float&gt;());
});</pre>
Vous pouvez enregistrer autant de StateObjectLink que vous le désirez. Là encore nous utilisé une expression lambda mais vous pouvez aussi bien “extraire” cette fonction callback dans une fonction distincte.

N'oubliez pas non plus d'appeler la méthode <em>constellation.loop()</em>” aussi souvent que possible dans la boucle principale de votre programme.