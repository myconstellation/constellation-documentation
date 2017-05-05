---
ID: 2465
post_title: 'Utiliser Lua sur NodeMCU pour connecter des ESP8266 &agrave; Constellation'
author: Sebastien Warin
post_date: 2016-08-23 14:21:29
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/arduino-esp-api/utiliser-lua-sur-nodemcu-pour-connecter-des-esp8266/
published: true
post_modified: 2017-05-05 10:07:13
---
<h3>Développer sur ESP8266 en Lua avec NodeMCU</h3>

Développer des applications natives sur ESP peut être assez compliqué pour celui qui ne connait pas le développement bas niveau !

<p style="text-align: center;"><img class="alignnone wp-image-4491 size-full" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/nodemcu-style5-150px.png_150x150.png" alt="" width="100" height="100" /></p>

<a href="http://nodemcu.com/index_en.html">NodeMCU</a> a eu l’idée de créer un firmware ESP8266 embarquant eLua, un interpréteur Lua très minimaliste pour système embarqué. Grace à ce firmware vous pouvez développer des programmes pour votre ESP très facilement en Lua.

Concrètement il faut commencer par flasher votre ESP avec le firmware NodeMCU en utilisant un outil comme ESP Flash Download (<a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/">voir ici</a>). Vous pouvez télécharger le firmware directement sur le <a href="https://github.com/nodemcu/nodemcu-firmware">GitHub de NodeMCU</a> ou bien sur <a href="http://nodemcu-build.com/">http://nodemcu-build.com/</a>  pour compiler son firmware à la demande en personnalisant différentes options (branche à compiler, modules à inclure, support ou non du SSL, activation du mode debug, etc..).

Une fois installé utilisez l’outil <a href="https://esp8266.ru/esplorer/">Esplorer</a>, une application Java (compatible Linux/Windows et Mac OS) pour éditer les scripts Lua directement depuis le système de fichier de votre ESP.

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-5-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image-5" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-5_thumb.png" alt="image-5" width="354" height="195" border="0" /></a></p>

A gauche vous retrouverez l’éditeur, à droite la sortie de votre ESP. Chaque fichier est enregistré dans le système de fichier de votre ESP avant d’être exécuté.

Par exemple pour se connecter au Wifi :

<pre class="lang:lua decode:true">print("Démarrage")
wifi.setmode(wifi.STATION)
print("set mode=STATION (mode="..wifi.getmode()..")")
wifi.sta.config("MON RESEAU WIFI", "Ma clé Wifi")
 
tmr.alarm(0, 1000, 1, function()
   if wifi.sta.getip() == nil then
      print("Connecting to AP...")
   else
      print("Connected! IP: ",wifi.sta.getip())
      tmr.stop(0)
end)</pre>

On commence par définir le mode Wifi à Station (client) puis on se connecte en spécifiant le SSID et la clé Wifi. Ensuite on lance une tache qu’on exécute sur le timer « 0 » (vous pouvez aller de 0 à 6), à une seconde d’intervalle (1000ms) de façon répétitive afin de récupérer l’IP. Si la connexion n’est pas encore effective on affiche le message « Connecting » sinon on coupe le timer « 0 » et on affiche l’adresse IP de votre ESP.

Pour plus d’information je vous invite à lire ou relire cet article : <a href="http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/">http://sebastien.warin.fr/2016/07/12/4138-decouverte-des-esp8266-le-microcontroleur-connecte-par-wifi-pour-2-au-potentiel-phenomenal-avec-constellation/</a>

<h3>Connexion à Constellation</h3>

Commencez tout d’abord par télécharger la librairie “<em>Constellation pour NodeMCU</em>” et transférez les fichiers du fichier ZIP sur votre ESP avec Esplorer.

[wpfilebase tag=file id=23 tpl=default /]

Créez ensuite un script Lua en incluant cette librairie afin d'initialiser le client Constellation avec l’adresse du serveur, votre Sentinelle &amp; package virtuel ainsi que la clé d’accès :

<pre class="lang:lua decode:true">local constellation = require("constellation")
constellation.init("constellation.monserveur.com", 8088, "MyVirtualSentinel", "MyVirtualPackage", "123456789")</pre>

<h3>Fonctionnalités de base</h3>

Vous pouvez écrire dans les logs Constellation grâce à la méthode “writeLog”. Par exemple :

<pre class="lang:lua decode:true">constellation.writeLog("Je suis pret avec " .. node.heap())</pre>

Par défaut les logs sont envoyés comme “Information”, mais vous pouvez spécifier “Warn” ou “Error” dans le 2ème arguments de cette méthode :

<pre class="lang:lua decode:true">constellation.writeLog("Ceci est un warning", "Warn")
constellation.writeLog("Ceci est une erreur", "Error")</pre>

Vous pouvez également récupérer les settings de votre package virtuel du serveur Constellation avec la méthode “getSettings” :

<pre class="lang:lua decode:true">constellation.getSettings(function(messages)
      print("Settings received :")
      for k,v in pairs(messages) do
          print(" -&gt; " .. k .. " = " .. v)
      end        
  end)</pre>

<h3>Publier des StateObjects</h3>

Pour publier un StateObject, vous disposez de la méthode “push” prenant en paramètre le nom et la valeur. Dans la version 1.0 le “lifetime”, “type” ou les “métadatas” ne sont pas supportés.

Vous pouvez publier soit des types simples, par exemple :

<pre class="lang:lua decode:true">constellation.push("Lux", math.floor(lux))</pre>

Soit des types complexes, c’est à dire des objets formatés en JSON. Vous pouvez formater vous même vos objets par concaténation :

<pre class="lang:lua decode:true">constellation.push("Lux", "{ Broadband:" .. broadband .. ", IR:" .. ir .. ", Lux:" .. math.floor(lux) .. "}")</pre>

Ou bien via le module Lua “cjson” :

<pre class="lang:lua decode:true">constellation.push("Lux", cjson.encode({ Broadband=broadband, IR=ir, Lux= math.floor(lux) }))</pre>

<h3>Recevoir des messages</h3>

Vous pouvez également recevoir des messages sur votre ESP8266 en enregistrant une fonction “callback” qui sera invoquée à chaque message reçu.

Par exemple :

<pre class="lang:lua decode:true">constellation.subscribeToMessage(function(message)
    print(message["Sender"]["FriendlyName"], message["Key"], message["Data"][2])  
    if message["Key"] == "Restart" then
        node.restart()
    end        
end)
</pre>

Dans cette première version de la librairie la description des messages callback (<em>Package Descriptor</em>) n’est pas supportée.