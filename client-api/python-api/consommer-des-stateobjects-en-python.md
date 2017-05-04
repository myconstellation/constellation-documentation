---
ID: 2507
post_title: 'Consommer des StateObjects depuis l&rsquo;API Python'
author: Sebastien Warin
post_date: 2016-08-24 10:46:27
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/python-api/consommer-des-stateobjects-en-python/
published: true
post_modified: 2017-05-04 15:40:28
---
Pour consommer des StateObjects vous pouvez simplement déclarer une méthode acceptant en paramètre un StateObject et rajouter sur cette méthode le décorateur “<em>Constellation.StateObjectLink</em>” en spécifiant le lien vers le ou les StateObjects.

Par exemple si vous voulez récupérer en temps réel le StateObject correspondant à un thermostat Nest (partant de l’hypothèse où le package Nest est déployé dans votre Constellation).

Via le StateObjects Explorer de la Console Constellation, on retrouvera le StateObject ici nommé “Living Room” publié par le package Nest qui représente l’état du thermostat du salon :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-76.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-65.png" alt="image" width="350" height="200" border="0" /></a></p>

On peut donc écrire une méthode qui sera liée à ce StateObject pour afficher un message de log à chaque fois que l’état du thermostat sera mis à jour :

<pre class="lang:python decode:true">@Constellation.StateObjectLink(package = "Nest", name = "Living Room")
def OnNestTargetTemperatureChanged(stateObject):
    Constellation.WriteInfo("The Nest target temperature has changed to : %s°C" % stateObject.Value.target_temperature_c)</pre>

Le décorateur “StateObjectLink” accepte les paramètres suivants : “sentinel”, “package”, “name” et “type”. Tous ces paramètres sont optionnels et par défaut défini à “*”, c’est à dire qu’aucun filtre n’est appliqué !

Une méthode décorée “StateObjectLink” doit accepter qu’un seul argument : le StateObject.

Un StateObject contient les propriétés suivantes :

<ul>
    <li><u>SentinelName</u> : nom de la sentinelle qui a produit le StateObject</li>
    <li><u>PackageName</u> : nom du package qui a produit le StateObject</li>
    <li><u>Name</u> : le nom du StateObject</li>
    <li><u>UniqueId</u> : identifiant unique du StateObject dans la Constellation (concaténation des 3 propriétés ci-dessus)</li>
    <li><u>Type</u> : type du StateObject</li>
    <li><u>Lifetime</u> : durée de vie en seconde du StateObject avant d’être considéré “expiré” (0 si infini)</li>
    <li><u>LastUpdate</u> : date de la dernière publication du StateObject</li>
    <li><u>IsExpired</u> : indique si le StateObject est expiré (c’est à dire que DateTime.Now &gt;  LastUpdate  + Lifetime si et seulement si Lifetime &gt; 0)</li>
    <li><u>Metadatas</u> : dictionnaire de clé / valeur</li>
    <li><u>Value</u> : la valeur du StateObject (peut être un type simple ou un objet complexe)</li>
</ul>

Notez que la méthode sur laquelle est appliquée cet attribut sera invoqué au démarrage de votre package avec la valeur actuelle du ou des StateObjects (Request) puis à chaque mise à jour des StateObjects (Subscribe).

Dernier exemple, affichons dans les logs de notre package Python la consommation des CPU de l’ensemble des sentinelles Windows connectées dans notre Constellation en utilisant le package HWMonitor :

<pre class="lang:python decode:true">@Constellation.StateObjectLink(package = "HWMonitor", name = "/intelcpu/0/load/0")
def CPUUpdated(stateobject):
    Constellation.WriteInfo("CPU on %s is currently %s %" % (stateobject.SentinelName, stateobject.Value.Value))</pre>