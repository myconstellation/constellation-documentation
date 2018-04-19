---
ID: 2511
post_title: 'MessageCallback : Exposer des méthodes Python'
author: Sebastien Warin
post_date: 2016-08-24 10:47:40
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/python-api/messagecallbacks-exposer-des-methodes-python/
published: true
publish_post_category:
  - "17"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:36:02
---
<h3>Exposer une méthode</h3>
Pour exposer une méthode Python dans Constellation vous devez tout simplement ajouter le décorateur “<em>Constellation.MessageCallback</em>” sur votre méthode :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def SimpleMessageCallback(data):
    "MessageCallback description with one string parameter"
    Constellation.WriteInfo('Input  = ' + str(data))</pre>
Ici nous déclarons le MessageCallback “SimpleMessageCallback” avec un seul paramètre nommé “data”.

Le paramètre peut être aussi un objet complexe :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def ComplexMessageCallback(data):
    "MessageCallback with object parameter"
    Constellation.WriteInfo(data.A)
    Constellation.WriteInfo(type(data.A))
    Constellation.WriteInfo(type(data.B))
    Constellation.WriteInfo(data.C)</pre>
Et vous pouvez également définir plusieurs arguments sur votre MessageCallback (simple ou complexe) :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def MultipleParameterCallback(a, b, c):
    "MessageCallback with 3 parameters"
    Constellation.WriteInfo(a)
    Constellation.WriteInfo(b)
    Constellation.WriteInfo(c)</pre>
Ou bien même exposer des méthodes sans argument :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def Demo():
    "Test sans parametre"
    Constellation.WriteInfo("Test sans parametre")</pre>
L’ensemble de ces méthodes déclarées “MessageCallbacks” seront alors référencées dans la Constellation. Vous pouvez les découvrir en interrogeant le Package Descriptor de votre package.

Par exemple, lancez la Console Constellation et rendez-vous dans le “MessageCallback Browser”. Vous aurez la liste de vos méthodes avec la possibilité de les invoquer directement depuis l’interface :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-71.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-60.png" alt="image" width="350" height="200" border="0" /></a></p>

<h3>Documenter son MessageCallback</h3>
Comme vous l’avez remarqué dans les exemples précédents, nous utilisons la documentation des méthodes de Python :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def Demo():
    "Ceci est la description du MessageCallback !"
    pass</pre>
La documentation de la méthode est utilisée comme description du MessageCallback :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-72.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-61.png" alt="image" width="350" height="109" border="0" /></a></p>

<h3>Personnaliser le MessageCallback</h3>
Comme pour l’API.NET, par défaut le nom du MessageCallback déclaré dans la Constellation est le nom de votre méthode mais vous pouvez redéfinir le nom exposé dans la Constellation :
<pre class="lang:python decode:true">@Constellation.MessageCallback("MySuperName")
def MyInternalMethodName():
    Constellation.WriteInfo("Hello")</pre>
Aussi vous pouvez “cacher” ce MessageCallback à la Constellation.
<pre class="lang:python decode:true">@Constellation.MessageCallback(False)
def HiddenMessageCallback():
    "Hidden message callback"
    Constellation.WriteInfo("I'm here !!")</pre>
Tous les packages ou consommateurs de votre Constellation pourront envoyer un message “HiddenMessageCallback” à votre package Python et donc invoquer votre méthode mais celle-ci ne sera pas déclarée dans le “Package  Descriptor”.
<h3>Récupérer le contexte</h3>
Comme avec l’API.NET, vous pouvez récupérer le contexte de réception du message en déclarant comme dernier argument de votre MessageCallback, le paramètre “context”.

Par exemple :
<pre class="lang:python decode:true">@Constellation.MessageCallback("MaMethode") # méthode renommée !
def DemoSeb5(context):
    "Test du contexte"
    Constellation.WriteInfo("Sender : %s" % context.Sender.FriendlyName)
    Constellation.WriteInfo("test sans parametre")</pre>
Ou avec des paramètres :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def DemoSeb5(a, b, context):
    "Test du contexte avec parametre"
    Constellation.WriteInfo("Sender : %s" % context.Sender.FriendlyName)
    Constellation.WriteInfo("a = %s et b = %s" % (a, b))</pre>
Il faut juste que le dernier argument soit nommé “context”. Notez qu’il ne fait pas partie de la signature de votre MessageCallback.

Ci-dessus, le 1er exemple est un MessageCallback caché et sans paramètre et le 2ème n’a que deux paramètres “a” et “b”.

L’objet “context” correspondant à la classe “<em>Constellation.Package.MessageContext</em>”. Vous retrouvez les propriétés suivantes :
<ul>
 	<li>IsSaga : indique si le message reçu appartient à une Saga</li>
 	<li>SagaId : identifiant de la Saga (si <em>IsSaga = true</em>)</li>
 	<li>Scope : scope d’envoi du message reçu
<ul>
 	<li>Type : type du scope (All, Group, Package, Sentinel, Others)</li>
 	<li>Args : arguments du scope (ex : nom du groupe, du package ou de la sentinelle)</li>
</ul>
</li>
 	<li>Sender : représente l’émetteur du message
<ul>
 	<li>ConnectionId : identifiant de connexion unique de l’émetteur</li>
 	<li>Type : type de l’émetteur (ConsumerHub, ConstellationHub, ConsumerHttp ou ConstellationHttp)</li>
 	<li>FriendlyName : nom de l’émetteur</li>
</ul>
</li>
</ul>
<h3>Répondre aux Sagas</h3>
Enfin votre méthode peut également retourner une réponse à l’appelant, c’est ce que l’on appelle <a href="/concepts/messaging-message-scope-messagecallback-saga/">une saga</a>. Pour cela vous devez “retourner” une valeur de retour et déclarer le type de cet objet de réponse dans le décorateur.
<pre class="lang:python decode:true">@Constellation.MessageCallback(returnType='System.Boolean')
def DemoSaga(user, password):
    "Test de saga"
    Constellation.WriteInfo("User=%s Password=%s" % (user, password))
    return user == password</pre>
Cela marche également en combinant la récupération du contexte :
<pre class="lang:python decode:true">@Constellation.MessageCallback(returnType='System.Boolean')
def DemoSaga(user, password, context):
    "Test de saga"
    Constellation.WriteInfo("Sender : %s" % context.Sender.FriendlyName)
    Constellation.WriteInfo("User=%s Password=%s" % (user, password))
    return user == password</pre>
<p align="left">Par exemple en ouvrant le “Message Callback Explorer” de la Console Constellation vous constaterez que votre MC “DemoSaga” est bien référencé comme retournant un “Booléen” en prenant deux arguments (car “context” n’est pas reconnu comme argument du MC) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-73.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-62.png" alt="image" width="350" height="150" border="0" /></a></p>
<p align="left">En invoquant cette méthode depuis la console, vous recevrez alors la réponse du package, ici un booléen :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-74.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-63.png" alt="image" width="350" height="241" border="0" /></a></p>
<p align="left">Dans les logs, on retrouve bien la trace de l’invocation de notre Saga avec le “contexte” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-75.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-64.png" alt="image" width="350" height="104" border="0" /></a></p>
(le FriendlyName de la Console Constellation est “<em>Consumer/ControlCenter:&lt;user&gt;</em>”).