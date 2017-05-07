---
ID: 1379
post_title: 'Envoyer des messages &amp; Invoquer des MessageCallbacks'
author: Sebastien Warin
post_date: 2016-03-16 17:41:17
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/
published: true
post_modified: 2017-05-07 14:35:41
---
Avant de suivre la lecture de cet article vous devez avoir compris les <a href="/concepts/messaging-message-scope-messagecallback-saga/">concepts de base</a> des MessageCallbacks.

<h3>Créer un scope</h3>

Un message Constellation est envoyé à un Scope qui représente le ou les destinataires du message.

Un scope Constellation est décrit dans l'API.NET par la classe “MessageScope”. Vous pouvez créer un scope avec la méthode statique “Create”.

Par exemple pour créer un scope vers les instances du package “MonPackage” de votre Constellation  :

<pre class="lang:c# decode:true">MessageScope.Create("MonPackage")</pre>

Par défaut le type de scope est “Package”. La ligne ci-dessus est donc identique à la ligne :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MonPackage")</pre>

Si vous souhaitez cibler une instance d’un package en particulier, vous devez spécifier la sentinelle sur laquelle est hébergée le package que vous ciblez :

<pre class="lang:c# decode:true">MessageScope.Create("MaSentinelle/MonPackage")</pre>

Si vous souhaitez cibler plusieurs packages :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MonPackage", "MonAutrePackage", "EncoreUnAutrePackage")</pre>

Bien entendu vous pouvez donner le nom d’une instance en particulier (sentinelle + package) ou juste le nom du package :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MaSentinelle/MonPackage", "MonAutrePackage")</pre>

Le scope “Sentinel” permet de cibler tous les packages sur une sentinelle donnée. Vous pouvez définir autant de sentinelle que vous souhaitez :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Sentinel, "MaSentinelle", "MonAutreSentinelle")</pre>

Le scope “Group” permet de cibler les packages qui <a href="/client-api/net-package-api/messagecallbacks/#Sabonner_et_recevoir_les_messages_dun_groupe">sont abonnés à un groupe</a>. Ici créons un scope pour tous les packages membre du groupe “GroupA” :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Group, "GroupA")</pre>

Vous pouvez bien sûr définir autant de groupe dans un scope que vous souhaitez :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Group, "GroupA", "GroupB", "GroupC")</pre>

Enfin vous pouvez créer des scopes pour cibler tous les clients connectés dans votre Constellation (All) ou tout le monde sauf l’émetteur (Others). Dans ces deux cas, il n’y a pas d’argument à spécifier :

<pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Others)</pre>

<h3>Envoyer un message avec le SendMessage</h3>

La méthode de base pour envoyer un message dans Constellation est “<em>PackageHost.SendMessage</em>”.

Vous devez définir trois arguments pour envoyer un message :

<ul>
    <li>Le scope (les destinataires de votre message)</li>
    <li>La clé du message (MessageKey)</li>
    <li>Le contenu du message (Data)</li>
</ul>

Comme vous le savez, on se sert des messages pour invoquer des méthodes d’autre packages. La clé du message est en fait le nom de la méthode (= le MessageCallback) à invoquer.

Prenons pour exemple les MessageCallbacks définis dans <a href="/client-api/net-package-api/messagecallbacks/">l’article précédent</a>.

Pour invoquer le MessageCallback “MyMethod” sans paramètre (Data = null) on pourrait écrire :

<pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "MyMethod", null);</pre>

Pour une méthode avec plusieurs paramètres, ils faut les encapsuler dans un tableau d’objet :

<pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "MyMethodWithMultipleParameters", new object[] { "Un String", 123, true });</pre>

Cela fonctionne aussi avec des objets complexes. Par exemple pour invoquer notre MessageCallback “Logon” :

<pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "Logon", new { User = "seb", Password = "seb" });</pre>

Avec l’API .NET on ne se sert généralement jamais de cette méthode “SendMessage” car vous avez à disposition le “SendMessageProxy” que nous allons découvrir ci-dessous.

<h3>Envoyer un message avec un proxy dynamique</h3>

La classe “SendMessageProxy” est un objet dynamique qui permet d’envoyer un message de la même manière que vous appelez une méthode en programmation .NET.

Pour créer un proxy dynamique vous devez au préalable avoir créer votre scope :

<pre class="lang:c# decode:true">MessageScope scope = MessageScope.Create("MonPremierPackage");
dynamic proxy = new SendMessageProxy(scope);</pre>

Pour simplifier le code, vous pouvez utiliser la méthode d’extension “GetProxy()” sur le MessageScope :

<pre class="lang:c# decode:true">MessageScope scope = MessageScope.Create("MonPremierPackage");
dynamic proxy = scope.GetProxy();</pre>

Attention, pour accéder aux méthodes d’extension du <em>MessageScope</em> vous devez ajouter le namespace “Constellation.Package” :

<pre class="lang:c# decode:true">using Constellation.Package;</pre>

Une fois le proxy  dynamique créé il suffit d’appeler '”fictivement” une méthode pour envoyer le message.

Par exemple pour envoyer le message “MyMethod” avec un contenu du vide, on écrira simplement :

<pre class="lang:c# decode:true">proxy.MyMethod();</pre>

Si votre message doit contenir plusieurs arguments :

<pre class="lang:c# decode:true">proxy.MyMethodWithMultipleParameters("Un String", 123, true);</pre>

Ou encore avec notre MessageCallback “Logon” qui prend en argument un objet complexe :

<pre class="lang:c# decode:true">proxy.Logon(new { User = "seb", Password = "seb" })</pre>

Ainsi vous pouvez invoquer un MessageCallback de n’importe quel package de la même façon que vous invoquerez une méthode de votre code .NET. La méthode invoquée est la clé du message et les arguments sont inclus dans le contenu du message.

Bien sûr avec la méthode d’extension vous pouvez faire tout cela en une ligne de code : créer le scope, récupérer le proxy dynamique et envoyer le message :

<pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>

Vous avez également à disposition la méthode “<em>CreateMessageProxy</em>” sur le PackageHost. Cette méthode permet de créer un scope et vous retourne le proxy dynamique.

Les deux lignes ci-dessous sont donc identiques :

<pre class="lang:c# decode:true">dynamic proxy = MessageScope.Create("MonPremierPackage").GetProxy();
dynamic proxy = PackageHost.CreateMessageProxy("MonPremierPackage");</pre>

De ce fait, pour invoquer nos trois MessageCallbacks, on peut écrire simplement :

<pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("MonPremierPackage").MyMethod();
PackageHost.CreateMessageProxy("MonPremierPackage").MyMethodWithMultipleParameters("Un String", 123, true);
PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });</pre>

Tout comme la méthode “MessageScope.Create”, le “CreateMessageProxy” peut créer tout type de scope.

Par exemple, invoquons le MessageCallbacks “MyMessageCallback” avec deux arguments (un string et un datetime) sur tous les packages du groupe A et B de notre Constellation :

<pre class="lang:c# decode:true">PackageHost.CreateMessageProxy(MessageScope.ScopeType.Group, "GroupA", "GroupB").MyMessageCallback("Mon Argument", DateTime.Now);</pre>

<h3>Invoquer un MessageCallback avec réponse – Utilisation des Sagas</h3>

Lorsque vous envoyez un message dans un scope vous pouvez ajouter un identifiant de Saga sur ce scope. Cela indiquera au destinataire que vous souhaitez une réponse en retour (<a href="/client-api/net-package-api/messagecallbacks/#Repondre_a_une_saga">plus d’info</a>).

L’identifiant de Saga doit être aléatoire et unique. Vous avez une méthode d’extension “WithSaga” sur un MessageScope vous permettant de définir l’identifiant de la saga (SagaId) avec un GUID :

<pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").WithSaga();</pre>

La méthode d’extension vous retourne le MessageScope, vous pouvez donc directement récupérer le proxy dynamique et envoyer votre message :

<pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").WithSaga().GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>

Pour la réception des réponses, vous avez une méthode “RegisterSagaResponseCallback” qui permet d’enregistrer la fonction à invoquer lors de la réponse :

<pre class="lang:c# decode:true">PackageHost.RegisterSagaResponseCallback(sagaId, (reponse) =&gt;
{
      // Response pour la saga "sagaId"
});</pre>

Pour simplifier le code, vous avez une méthode d’extension “OnSagaResponse” sur un MessageScope qui permet à la fois d’ajouter un identifiant de saga sur le scope (WithSaga) et d’enregistrer la fonction de retour.

De ce fait, vous pouvez en une seule ligne créer votre scope avec saga, définir le code qui sera invoqué à la réception la réponse, récupérer le proxy dynamique et invoquer votre MessageCallback avec ses paramètres :

<pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").OnSagaResponse((response) =&gt;
{
    // Reponse de la saga
}).GetProxy().Logon(new { User = "seb", Password = "seb" });
</pre>

Comme pour les MessageCallbacks, la méthode de réception d’une réponse d’une saga est invoquée dans un thread dédié dans lequel vous avez accès au contexte du message via la propriété “MessageContext.Current”.

Vous pouvez donc savoir quel est le package qui vous a répondu (<em>Sender</em>) ou encore quel était l’identifiant de la saga utilisée.

De plus, notez que la méthode <em>OnSagaResponse</em> est générique afin de pouvoir définir le type de retour (le cast sera réalisé automatiquement).

On peut donc écrire :

<pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").OnSagaResponse&lt;bool&gt;((response) =&gt;
{
    PackageHost.WriteInfo("Reponse de {0} pour la saga {1}",                 
        MessageContext.Current.SagaId,
        MessageContext.Current.Sender.FriendlyName);

    PackageHost.WriteInfo(response ? "OK" : "DENIED");

}).GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>

Notez également que si votre scope cible plusieurs packages (ex : si “MonPremierPackage” est déployé sur plusieurs sentinelles), votre fonction de traitement de la réponse sera invoquée plusieurs fois, pour chaque package qui répondra (car l’identifiant de Saga est le même).

<h3>Utiliser les “Tasks” (awaitable) pour attendre la réponse d’une saga</h3>

C’est une nouveauté de la 1.8 de Constellation. Pour bien comprendre, résumons ce que l’on connait déjà !

Lorsque vous invoquez une méthode sur le proxy dynamique, celui-ci envoie un message dans Constellation où la clé du message est le nom de la méthode invoquée et le contenu du message sont les paramètres de la méthode invoquée.

Ces deux lignes sont donc identiques :

<pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "Logon", new { User = "seb", Password = "seb" });
MessageScope.Create("MonPremierPackage").GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>

Sur la 2ème ligne, vous invoquez dynamiquement la méthode “Logon” sur le <em>SendMessageProxy</em> (créé par la méthode GetProxy()) pour envoyer (<em>PackageHost.SendMessage</em>) le message “Logon”.

Ces deux lignes font donc bien la même chose. Dans les deux cas le message est envoyé dans un scope qui ne porte pas de “SagaId” (donc aucune réponse n’est attendue).

Je rappelle également que vous pouvez créer un scope et récupérer son proxy dynamique directement avec la méthode <em>PackageHost.CreateMessageProxy</em>. On peut donc encore simplifier le code par la ligne :

<pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });</pre>

D’un point de vue .NET, l’invocation dynamique de notre MessageCallback  “Logon" sur le proxy dynamique ne retourne rien, du moins il retourne “null”.

<pre class="lang:c# decode:true">object result = PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });
// ici result = null</pre>

Observez maintenant le code ci-dessous :

<pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });</pre>

Si vous examinez attentivement le MessageCallback que nous invoquons sur le proxy dynamique, on utilise une forme générique : au lieu d’avoir “Logon(xxxxx)”, on a ”Logon<strong>&lt;bool&gt;</strong>(xxxxx)<em>”</em>

Le fait de définir un type générique indique au proxy dynamique que vous attendez une réponse et donc qu’il s’agit d’une saga !

Le type générique est le type de réponse que vous attendez. Ici le MessageCallback “Logon” que vous avons écrit dans l’article précédent renvoi un booléen (bool). Vous pouvez utiliser n’importe quel type de base ou types complexes.

Si c’est la réponse est un objet anonyme ou si vous n’avez pas la définition de l’objet de retour dans votre code,  vous pouvez utiliser un “dynamic” (ex <em>Logon<strong>&lt;dynamic&gt;</strong>(xxxx)</em>)<em>.</em>

Quoi qu’il en soit il est obligatoire d’invoquer le MessageCallback  en utilisant la syntaxe générique pour que le proxy dynamique ajoute automatiquement un identifiant de saga (<em>WithSaga</em>) lors de l’envoi du message.

Revenons donc à notre appel :

<pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });</pre>

Le proxy dynamique nous retourne une ”Task&lt;dynamic&gt;”, c’est à dire une tache qui se charge d’envoyer et d’attendre la réception de la (première) réponse.

Vous pouvez bloquer le thread appelant jusqu'à ce que l'opération asynchrone soit terminée,  c’est à dire jusqu’à l’obtention de la réponse :

<pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
PackageHost.WriteInfo((bool)reponse.Result ? "OK" : "DENIED");</pre>

<h4>Ajouter des timeouts</h4>

Comme vous bloquez le thread appelant, prenez garde de mettre des gardes fou car nous n’avez aucune garantie qu’un package va répondre à votre saga. La tache pourrait ne jamais être complétée! Vous devriez donc attendre un certain temps avant de considérer la tache “hors délai”.

Par exemple attendons pendant 3 secondes maximum pour une réponse à notre Saga “Logon” :

<pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
if (reponse.Wait(3000) &amp;&amp; reponse.IsCompleted)
{
    PackageHost.WriteInfo((bool)reponse.Result ? "OK" : "DENIED");
}
else
{
    PackageHost.WriteError("Aucune réponse !");
}</pre>

<h4>Async/Await</h4>

Aussi vous pouvez utiliser le pattern async/await, très pratique pour les packages UI WPF/Winform :

<pre class="lang:c# decode:true">public async void Logon()
{
    bool reponse = await PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
    PackageHost.WriteInfo(reponse ? "OK" : "DENIED");
}</pre>

<h4>Résultat de la tache</h4>

Notez bien que le proxy dynamique vous renverra toujours une “Task&lt;<strong>dynamic</strong>&gt;” quelque soit le type générique précisé au niveau de l’appel du MessageCallback (“bool” dans l’exemple du “Logon”).

Cependant le résultat de la tache (<em>Task.Result</em>) est bien du type que celui passé au niveau de l’appel du MessageCallback.

Si vous souhaitez toutefois récupérer une Task&lt;T&gt; où T est le type de retour de votre saga, vous devez créer une nouvelle tache pour réaliser le “cast” à la suite de la tache créée par Constellation.

Par exemple :

<pre class="lang:c# decode:true">Task&lt;dynamic&gt; task = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
Task&lt;bool&gt; logonTask = task.ContinueWith&lt;bool&gt;(t =&gt; (bool)t.Result);</pre>

<h4>Annuler les taches</h4>

Lorsque l’on créé des taches en .NET on peut avoir besoin de leurs associer un jeton d’annulation : le <em>CancellationToken</em>.

Pour cela, il vous suffit de passer un “CancellationToken” comme dernier argument de votre invocation du MessageCallback :

<pre class="lang:c# decode:true">CancellationTokenSource source = new CancellationTokenSource(1000);
CancellationToken token = source.Token;
try
{
    Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("ConstellationPackageConsole1").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" }, token);
    PackageHost.WriteInfo("Result = {0}", reponse.Result);
}
catch (AggregateException ae)
{
    foreach (Exception e in ae.InnerExceptions)
    {
        if (e is TaskCanceledException)
            PackageHost.WriteError("Unable to compute mean: {0}", ((TaskCanceledException)e).Message);
        else
            PackageHost.WriteError("Exception: " + e.GetType().Name);
    }
}
finally
{
    source.Dispose();
}</pre>

Dans l’exemple ci-dessus on crée un <em>CancellationToken</em> à partir d’un <em>CancellationTokenSource</em> que l’on passe en tant que dernier paramètre du MessageCallback.

Le <em>CancellationTokenSource</em> est ici construit pour annuler la tache après 1 seconde (1000 ms). Vous pouvez aussi annuler la tache en en appelant la méthode “Cancel” sur l'objet <em>CancellationTokenSource</em> :

<pre class="lang:c# decode:true">source.Cancel();</pre>

Bien entendu, le proxy dynamique se sert de votre <em>CancellationToken</em> pour savoir quand annuler la tache. De plus notez bien que le proxy retire ce <em>CancellationToken</em> des paramètres du message (= le <em>CancellationToken</em> n’est pas envoyé dans le message !).

Prenez garde à bien gérer les exceptions, si la tache est annulée une exception “<em>TaskCanceledException</em>” est levée.

<h4>Le contexte du message</h4>

Dans une méthode marquée comme “MessageCallback” ou dans le callback de retour d’une saga (<em>OnSagaResponse</em>) vous pouvez toujours accéder au contexte de réception du message avec la propriété “<em>MessageContext.Current</em>”.

En effet les MessageCallbacks ou les callbacks de retour des sagas sont invoqués dans un thread à part ce qui permet de l’attacher au contexte de réception du message.

Cependant avec les taches, le résultat est dispatché dans le thread de l’appelant. Autrement dit, vous ne pouvez pas accéder au contexte courant (<em>MessageContext.Current</em>) !

La solution consiste à “demander” au proxy dynamique de vous “remplir” un contexte que vous avez initialisé au préalable.

Pour ce faire, commencez par déclarer un contexte vide (<em>MessageContext.None</em>) et passer l’instance comme dernier paramètre d'un appel :

<pre class="lang:c# decode:true">MessageContext context = MessageContext.None;
Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("ConstellationPackageConsole1").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" }, context);
PackageHost.WriteInfo("Result = {0} - Response received from : {1}", reponse.Result, context.Sender.FriendlyName);</pre>

Une fois la tache complétée, la variable “context” sera remplie avec le contexte de la réponse lors de la réception. Vous pourrez donc accéder au contexte même dans vos taches.

Le MessageContext est toujours le dernière paramètr et le CancellationToken est donc l’avant dernier paramètre dans le cas où vous utilisez les deux :

<pre class="lang:c# decode:true">CancellationTokenSource source = new CancellationTokenSource(1000);
CancellationToken token = source.Token;
MessageContext context = MessageContext.None;
try
{
    Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("ConstellationPackageConsole1").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" }, token, context);
    PackageHost.WriteInfo("Result = {0} - Response received from : {1}", reponse.Result, context.Sender.FriendlyName);
}
catch (AggregateException ae)
{
    foreach (Exception e in ae.InnerExceptions)
    {
        if (e is TaskCanceledException)
            PackageHost.WriteError("Unable to compute mean: {0}", ((TaskCanceledException)e).Message);
        else
            PackageHost.WriteError("Exception: " + e.GetType().Name);
    }
}
finally
{
    source.Dispose();
}</pre>

<h3>Générateur de code</h3>

Comme l’ensemble des MessageCallbacks sont déclarés dans la Constellation (sauf si ils sont marqués comme “<a href="/client-api/net-package-api/messagecallbacks/#MessageCallback_cache">Hidden</a>”), il est possible de générer du code. Vous pouvez <a href="/constellation-platform/constellation-sdk/generateur-de-code/">lire un article très complet sur le sujet ici</a>.

Dans le menu contextuel de votre projet Visual Studio, sélectionnez “Generate Code” :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-125.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Générer du code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-102.png" alt="Générer du code" width="424" height="330" border="0" /></a></p>

<p align="left">Sélectionnez votre Constellation et cliquez sur “Discover” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-126.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Générer du code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-103.png" alt="Générer du code" width="424" height="469" border="0" /></a></p>

<p align="left">Sélectionnez le ou les packages que vous souhaitez invoquer depuis votre package et cliquez sur “Generate”.</p>

<p align="left">Le générateur de code va créer différentes classes présentant votre Constellation avec les packages sélectionnés (MessageCallbacks et StateObjects).</p>

<p align="left">Dans notre cas nous allons inclure la définition des MessagesCallbacks de notre package “MonPremierPackage”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-127.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout du using" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-104.png" alt="Ajout du using" width="424" height="132" border="0" /></a></p>

<p align="left">Commençons donc par ajouter le using vers le MessageCallbacks de MonPremierPackage :</p>

<pre class="lang:c# decode:true">using ConstellationPackageConsole1.MonPremierPackage.MessageCallbacks;</pre>

<p align="left">Dans la classe générée “MyConstellation” vous retrouverez la liste des sentinelles et packages de votre Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-128.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Enumération générée" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-105.png" alt="Enumération générée" width="424" height="65" border="0" /></a></p>

Vous pourrez alors créer un scope personnalisé pour le package sélectionné :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-129.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageScope généré par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-106.png" alt="MessageScope généré par package" width="424" height="84" border="0" /></a></p>

<p align="left">Ce scope personnalisé contiendra l’ensemble des MessageCallbacks :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-130.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-107.png" alt="image" width="424" height="69" border="0" /></a></p>

<p align="left">Et sur chaque MessageCallback, vous retrouvez les bons types de retour, les commentaires et les types complexes proprement générés :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-131.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks générés" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-108.png" alt="MessageCallbacks générés" width="424" height="67" border="0" /></a></p>

De ce fait, pour reprendre l’exemple du MessageCallback “Logon”, on pourra écrire le code :

<pre class="lang:c# decode:true">var context = MessageContext.None;
var token = new CancellationTokenSource(5000).Token;
var user = new UserInfo() { User = "seb", Password = "seb" };

var task = MyConstellation.Packages.MonPremierPackage.CreateMonPremierPackageScope().Logon(user, token, out context);

PackageHost.WriteInfo("Logon user {0} - Result = {1} - Sender : {2}", user.User, task.Result, context.Sender.FriendlyName);</pre>

<a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-132.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="Démonstration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-109.png" alt="Démonstration" width="424" height="205" border="0" /></a>

N’hésitez pas <a href="/constellation-platform/constellation-sdk/generateur-de-code/">lire cet article très complet sur la génération de code C#</a> dans Constellation.