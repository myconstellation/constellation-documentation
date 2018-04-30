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
  - "0"
update_discourse_topic:
  - "0"
discourse_post_id:
  - "1378"
discourse_topic_id:
  - "898"
discourse_permalink:
  - >
    https://forum.myconstellation.io/t/messagecallback-exposer-des-m-eacute-thodes-python/898
wpdc_xmlrpc_failure_sent:
  - "1"
post_modified: 2018-04-30 14:40:00
---
<h3>Exposer une méthode</h3>
Pour exposer une méthode Python dans Constellation vous devez tout simplement ajouter le décorateur “<em>Constellation.MessageCallback</em>” sur votre méthode :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def SimpleMessageCallback():
    Constellation.WriteInfo('Hello Python !')</pre>
Ici nous déclarons le MessageCallback “SimpleMessageCallback” sans paramètre ni documentation.

Vous pouvez ajouter un ou plusieurs arguments à votre MC et utiliser la syntaxe de Python pour commenter vos MC :
<pre class="lang:default decode:true">@Constellation.MessageCallback()
def DemoArg(data):
    "Ceci est la description du MessageCallback DemoArg avec 1 argument"
    Constellation.WriteInfo('arg = %s', data)

@Constellation.MessageCallback()
def DemoArgs(a, b, c):
    "Ceci est la description du MessageCallback DemoArgs avec 3 arguments"
    Constellation.WriteInfo(a)
    Constellation.WriteInfo(b)
    Constellation.WriteInfo(c)</pre>
Le ou les paramètres peuvent être des types simples (string, int, bool, etc..) ou des types (objets) complexes :
<pre class="lang:python decode:true crayon-selected">@Constellation.MessageCallback()
def ComplexMessageCallback(data):
    "Ceci est la description du MessageCallback ComplexMessageCallback avec un objet complexe en argument"
    Constellation.WriteInfo(data.A)
    Constellation.WriteInfo(data.B)
    Constellation.WriteInfo(data.C)</pre>
L’ensemble de ces méthodes déclarées “MessageCallbacks” seront alors référencées dans la Constellation. Vous pouvez les découvrir en interrogeant le Package Descriptor de votre package.

Par exemple, lancez la Console Constellation et rendez-vous dans le “MessageCallback Browser”. Vous aurez la liste de vos méthodes avec la possibilité de les invoquer directement depuis l’interface :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-9.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-8.png" alt="MessageCallbacks Explorer" width="484" height="346" border="0" /></a></p>

<h3>Documenter son MessageCallback</h3>
<h4>Description des MessageCallbacks</h4>
Comme vous l’avez remarqué dans les exemples précédents, nous utilisons la documentation des méthodes de Python :
<pre class="lang:python decode:true">@Constellation.MessageCallback()
def Demo():
    "Ceci est la description du MessageCallback !"
    pass</pre>
La documentation de la méthode est utilisée comme description du MessageCallback :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-72.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-61.png" alt="image" width="350" height="109" border="0" /></a></p>
La description de la méthode peut être écrite sur plusieurs lignes :
<pre title="Documentation d'un MC sur plusieurs lignes" class="lang:python decode:true">@Constellation.MessageCallback()
def Demo():
    '''
    The first line is brief explanation, which may be completed with 
    a longer one. For instance to discuss about its methods.
    '''
    pass</pre>
<h4>Description des arguments des MessageCallbacks</h4>
<u>Note</u> : la description des arguments est disponible depuis l’API Python 1.8.4.x. Pensez à mettre à jour la référence de votre API par Nuget depuis Visual Studio ou en <a href="/client-api/python-api/developper-avec-le-package-tools-cli/#Mise_a_jour_du_template">mettant à jour le template depuis la CLI</a>.

Si vous regardez attentivement les arguments des MC dans le MessageCallback Explorer, ils sont reconnus comme "System.Object" étant donné que les arguments d'une fonction en Python ne sont pas typés.

Pour décrire les paramètres utilisez la syntaxe suivante dans la description du MC :
<pre title="Syntaxe de description d'un paramètre" class="lang:default decode:true ">    :param &lt;type&gt; &lt;name&gt; : &lt;description&gt;</pre>
Par exemple :
<pre title="MC avec plusieurs paramètres doucmentés" class="lang:default decode:true crayon-selected">@Constellation.MessageCallback()
def Demo(a, b, c):
    '''
    Ceci est un exemple de MC avec 3 parametres

    :param int a: My int value
    :param bool b: My boolean value
    :param string c: My string value
    '''
    Constellation.WriteInfo("a = %s - type: %s" % (a, type(a)))
    Constellation.WriteInfo("b = %s - type: %s" % (b, type(b)))
    Constellation.WriteInfo("c = %s - type: %s" % (c, type(c)))</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-10.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Description des arguments d'un MC en Python" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-9.png" alt="Description des arguments d'un MC en Python" width="484" height="166" border="0" /></a></p>
<p align="left">Ainsi en retournant sur le MC Explorer, chaque argument est correctement typé dans Constellation.</p>

<h4 align="left">Paramètres optionnels avec valeur par défaut</h4>
<p align="left">Vous pouvez également définir que certains paramètres sont optionnels en spécifiant leurs valeurs par défaut avec la syntaxe [default: XXX] :</p>

<pre title="Exemple de MC des paramètres optionnels" class="lang:default decode:true">@Constellation.MessageCallback()
def DemoOptional(a, b):
    '''
    Ceci est un exemple de MC des paramètres optionnels

    :param int a: My int value [default:10]
    :param bool b: My boolean value [default:true]
    '''
    Constellation.WriteInfo("a = %s" % a)
    Constellation.WriteInfo("b = %s" % b)</pre>
&nbsp;
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-11.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Arguments optionnels avec valeur par défaut" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-10.png" alt="Arguments optionnels avec valeur par défaut" width="484" height="138" border="0" /></a></p>

<h4>Décrire les arguments de type complexe</h4>
Dans le cas où un MC prend en paramètre un (ou plusieurs) type complexe, il faudra d'abord décrie le type dans la méthode "Start".

Par exemple, enregistrons au démarrage du package (méthode Start), le type “CredentialInfo” :
<pre title="Déclaration d'un type complexe" class="lang:default decode:true ">Constellation.DescribeMessageCallbackType("CredentialInfo", "Credential information", [
    { 'Name':'Username', 'Type':'string', 'Description': 'The username' },
    { 'Name':'Password', 'Type':'string', 'Description': 'The password' },
])</pre>
Une fois tous vos types déclarés, il faut publier le “Package Descriptor” de votre package à Constellation par la ligne :
<pre title="Déclaration du package descriptor" class="lang:python decode:true">Constellation.DeclarePackageDescriptor()</pre>
Nous pouvons maintenant déclarer un MC avec un parametre de type “CredentialInfo”. Par exemple :
<pre class="lang:default decode:true ">@Constellation.MessageCallback()
def DemoAuth(credential):
    '''
    Demo MC avec avec un paramètre de type complexe

    :param CredentialInfo credential: the credential info
    '''
    Constellation.WriteInfo("User=%s Password=%s" % (credential.Username, credential.Password))</pre>
Comme vous pourrez le voir dans le MessageCallbacks Explorer, votre méthode Python est correctement décrite ainsi que son paramètre “credential” de type “Credentialnfo”.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-12.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Argument de type complexe" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-11.png" alt="Argument de type complexe" width="484" height="150" border="0" /></a></p>
Notez également que la description du type CredentialInfo est accessible en cliquant sur le type :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-13.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Argument de type complexe" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-12.png" alt="Argument de type complexe" width="484" height="186" border="0" /></a></p>

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
Votre méthode peut également retourner une réponse à l’appelant, c’est ce que l’on appelle <a href="/concepts/messaging-message-scope-messagecallback-saga/">une saga</a>. Pour cela vous pouvez spécifier le type de la valeur de retour dans le décorateur de la façon suivante :
<pre class="lang:python decode:true">@Constellation.MessageCallback(returnType='System.Boolean')
def DemoSaga(user, password):
    "Test de saga"
    Constellation.WriteInfo("User=%s Password=%s" % (user, password))
    return user == password</pre>
Cependant il est recommandé d'utiliser la description du MC pour spécifier le type de retour avec la syntaxe
<pre title="Syntaxe de description d'un type de retour" class="lang:default decode:true ">:return &lt;type&gt;: &lt;description&gt;</pre>
Par exemple :
<pre title="Exemple d'une sage" class="lang:python decode:true">@Constellation.MessageCallback()
def DemoSaga(user, password):
    '''
    Test de saga

    :param string user: The username
    :param string password: The password
    :return bool: if user match
    '''
    Constellation.WriteInfo("User=%s Password=%s" % (user, password))
    return user == password</pre>
Cela marche également en combinant la récupération du contexte :
<pre title="Saga avec contexte" class="lang:python decode:true">@Constellation.MessageCallback()
def DemoAuth(user, password, context):
    '''
    Test de saga

    :param string user: The username
    :param string password: The password
    :return bool: if user match
    '''
    Constellation.WriteInfo("Sender : %s" % context.Sender.FriendlyName)
    Constellation.WriteInfo("User=%s Password=%s" % (user, password))
    return user == password</pre>
<p align="left">Par exemple en ouvrant le “Message Callback Explorer” de la Console Constellation vous constaterez que votre MC “DemoAuth” est bien référencé comme retournant un “Booléen” en prenant deux arguments (car l’argument “context” n’est pas pris en compte dans la signature du MC) :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-14.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Saga" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-13.png" alt="Saga" width="484" height="142" border="0" /></a></p>
<p align="left">En invoquant cette méthode depuis la console, vous recevrez alors la réponse du package, ici un booléen :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-74.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-63.png" alt="image" width="350" height="241" border="0" /></a></p>
<p align="left">Dans les logs, on retrouve bien la trace de l’invocation de notre Saga avec le “contexte” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-75.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-64.png" alt="image" width="350" height="104" border="0" /></a></p>
(le FriendlyName de la Console Constellation est “<em>Consumer/ControlCenter:&lt;user&gt;</em>”).