---
ID: 2509
post_title: >
  Envoyer des messages et invoquer des
  MessageCallbacks en Python
author: Sebastien Warin
post_date: 2016-08-24 10:47:10
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/python-api/envoyer-des-messages-et-invoquer-des-messagecallbacks-en-python/
published: true
publish_post_category:
  - "17"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:37:01
---
<h3>Envoyer un message</h3>
Pour envoyer un message et donc <a href="/concepts/messaging-message-scope-messagecallback-saga/">invoquer un MessageCallback</a> depuis un package Python vous devez utiliser la méthode “<em>Constellation.SendMessage</em>”. Par exemple :
<pre class="lang:python decode:true">Constellation.SendMessage("SMS", "SendMessage", [ "+33676903634", "Bonjour Constellation" ])</pre>
Ici par exemple on envoi un message au package “SMS” pour invoquer le MessageCallback “SendMessage” en passant en argument deux paramètres de type string.

Le MessageCallbacks Explorer de la Console Constellation permet de lister l’ensemble des MC déclarés par les packages de votre Constellation.

Par exemple, avec le package <a href="/package-library/nest/">Nest</a> déployé dans votre Constellation, on retrouvera un MessageCallback  “SetTargetTemperature” pour piloter la température de consigne d’un thermostat Nest.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-20.png" alt="image" width="350" height="99" border="0" /></a></p>
Pour l’invoquer depuis notre code Python :
<pre class="lang:python decode:true">Constellation.SendMessage("Nest", "SetTargetTemperature", [ "thermostatId", 19 ], Constellation.MessageScope.package)</pre>
On peut également passer en paramètre un objet complexe. Par exemple le MC “AreaArm” du package Paradox permet d’armer le système d’alarme en passant en paramètre un objet “ArmingRequestData” contenant le secteur à armer, le mode d’armement et le PIN.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-21.png" alt="image" width="350" height="121" border="0" /></a></p>
Le code Python pour invoquer ce MC sera alors :
<pre class="lang:python decode:true">Constellation.SendMessage("Paradox", "AreaArm", { "Area":"All", "Mode":1, "PinCode":"0000" }, Constellation.MessageScope.package)</pre>
Vous pouvez également passer plusieurs arguments à votre MC en combinant type simple et type complexe. Par exemple le MC “ShowNotification” du package <a href="/package-library/xbmc/">Xbmc</a> permettant d’afficher une notification sur une interface Kodi/XBMC prend deux paramètres : le nom d l’hôte XBMC (un type simple) et le détail de la notification à afficher (un type complexe) :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-23.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-22.png" alt="image" width="350" height="143" border="0" /></a></p>

<pre class="lang:python decode:true">Constellation.SendMessage("Xbmc", "ShowNotification", [ xbmcName, { "Title":"Hello Constellation", "Message":"Hello I'm Python" } ], Constellation.MessageScope.package)</pre>
Les exemples de message ci-dessus sont tous envoyés à un package mais vous pouvez également envoyer vos messages à <a href="/concepts/messaging-message-scope-messagecallback-saga/">différents scopes</a>.

Par exemple pour envoyer le message “Demo” sans aucun paramètre au groupe A, on écrira :
<pre class="lang:python decode:true">Constellation.SendMessage("A", "Demo", {}, Constellation.MessageScope.group)</pre>
Les scopes disponibles sont :
<ul>
 	<li>group</li>
 	<li>package</li>
 	<li>sentinel</li>
 	<li>others</li>
 	<li>all</li>
</ul>
Notez par ailleurs que le paramètre “MessageScope” de la méthode “SendMessage” est optionnel. Si il n’est pas spécifié le scope par défaut est “package”.
<h3>Envoyer des messages avec réponse : les Sagas</h3>
Si le MC que vous invoquez retourne un message de réponse, vous devez envoyer votre message dans une saga. Pour cela vous pouvez utiliser la méthode “SendMessageWithSaga” dans laquelle vous devrez spécifier la fonction qui sera invoquée lors de la réception de la réponse.

Par exemple reprenons le MessageCallback “AreaArm” du package Paradox :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/09/image-24.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/09/image_thumb-23.png" alt="image" width="350" height="121" border="0" /></a></p>
<p align="left">Comme vous le constatez, ce MessageCallback retourne un “Boolean” pour indiquer si l’armement a bien été déclenché.</p>
<p align="left">On peut donc invoquer ce MC dans une saga et attacher un callback de réponse pour afficher dans le logs de notre package si l’armement a réussi :</p>

<pre class="lang:python decode:true">Constellation.SendMessageWithSaga(lambda response:
   Constellation.WriteInfo("Arm result is %s" % response),
"Paradox", "AreaArm", { "Area":"All", "Mode":1, "PinCode":"0000" })</pre>
Ici nous avons utilisé une fonction lambda pour simplifier la lecture mais vous pouvez également définir une fonction classique dans votre code.

Le paramètre “response” utilisé dans la fonction callback est l’objet de la réponse envoyé par le package qui a reçu votre message. Ici “response” est un “Boolean” car le MC “AreaArm” retourne un booléen mais il peut être de n’importe quel type (simple ou complexe).

Le MessageCallback Explorer de la Console Constellation vous donnera tous les détails.