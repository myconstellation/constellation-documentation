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
post_modified: 2016-09-19 14:09:11
---
<h3>Interroger des StateObjects à un instant T</h3> <p>Pour récupérer un ou plusieurs StateObject (à un instant T), vous pouvez utiliser la méthode “requestStateObjects” : </p><pre class="lang:cpp decode:true">JsonArray&amp; myStateObject = constellation.requestStateObjects(sentinel, package, name, type);</pre>
<p>Utilisez le wildcard “*” pour ne pas appliquer de filtre. Le symbole “*” est aussi accessible via la constante “WILDCARD”.</p>
<p>Par exemple :</p><pre class="lang:cpp decode:true">// All StateObjects for MyPackage on MySentinel
JsonArray&amp; so = constellation.requestStateObjects("MySentinel", "MyPackage", "*", "*");
// All StateObjects named "Demo" in your Constellation
JsonArray&amp; so = constellation.requestStateObjects(WILDCARD, WILDCARD, "Demo", WILDCARD);</pre>
<p>Autre exemple, vous souhaitez afficher dans les logs de votre Arduino, la consommation CPU de toutes vos machines Windows (vous avez au préalable déployé le package “HWMonitor” sur toutes vos sentinelles Windows) :</p><pre class="lang:cpp decode:true">// Example : print the all SO's value named "/intelcpu/0/load/0" and produced by the "HWMonitor" package (on all sentinels)
JsonArray&amp; cpus = constellation.requestStateObjects("*", "HWMonitor", "/intelcpu/0/load/0");
for(int i=0; i &lt; cpus.size(); i++) {	
  constellation.writeInfo("CPU on %s is currently %d %", cpus[i]["SentinelName"].asString(), cpus[i]["Value"]["Value"].as&lt;float&gt;());
}</pre>
<h3>S’abonner aux mises à jours des StateObjects en temps réel</h3>
<p>Vous pouvez également vous abonnez aux mises à jour des StateObjects pour être notifier en temps réel de la modification des valeurs des StateObjects. Pour cela vous devez définir une fonction callback nommée “<em>setStateObjectUpdateCallback</em>” qui sera appelée à chaque mise à jour de SO puis vous abonnez aux différents StateObjects que vous souhaitez “suivre” avec la méthode “<em>subscribeToStateObjects</em>”.</p>
<p>Par exemple, pour être notifié en temps réel de la consommation CPU des différentes sentinelles Windows :</p><pre class="lang:cpp decode:true">// set a StateObject update callback and subscribe to SO
constellation.setStateObjectUpdateCallback([] (JsonObject&amp; so) {
constellation.writeInfo("StateObject updated ! StateObject name = %s", so["Name"].asString()); 
});
// Subscribe to SO named "/intelcpu/0/load/0" and produced by the "HWMonitor" package (on all sentinels)
constellation.subscribeToStateObjects("*", "HWMonitor", "/intelcpu/0/load/0");</pre>
<p>Nous avons défini notre fonction callback dans une expression lambda mais vous pouvez aussi bien “extraire” cette fonction callback dans un fonction de votre fichier Arduino.</p>
<p>Enfin dans la boucle principale (loop), vous devez invoquer la méthode “<em>constellation.loop()</em>” aussi souvent que possible afin que la libraire puisse dispatcher les SO dans votre méthode callback :</p><pre class="lang:cpp decode:true">constellation.loop();</pre>
<h3>Enregistrer des StateObjectLinks</h3>
<p>Avec la méthode “<em>subscribeToStateObjects</em>” présentée ci-dessous vous avez une fonction callback qui recevra tous SO pour lesquels vous êtes abonné. Vous devez donc “dispatcher” vous même les différents SO que vous recevrez.</p>
<p>Pour simplifier les choses, la libraire Arduino propose comme en <a href="/client-api/net-package-api/consommer-des-stateobjects/#Les_StateObjectLink">.NET</a> ou <a href="/client-api/python-api/consommer-des-stateobjects-en-python/">Python</a>, la notion de StateObjectLink : il s’agit de “lier” un abonnement à des StateObjects à une fonction de votre code.</p>
<p>Pour reprendre l’exemple du suivi des CPU en temps réel, nous pourrions écrire :</p><pre class="lang:cpp decode:true">constellation.registerStateObjectLink("*", "HWMonitor", "/intelcpu/0/load/0", [](JsonObject&amp; so) {
  constellation.writeInfo("CPU on %s is currently %d %", so["SentinelName"].asString(), so["Value"]["Value"].as&lt;float&gt;());
});</pre>
<p>Sans oublier bien sûr d'appeler la méthode <em>constellation.loop()</em>” aussi souvent que possible dans la boucle principale de votre programme.</p>