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
post_modified: 2016-04-28 15:56:38
---
<p>Avant de suivre la lecture de cet article vous devez avoir compris les <a href="/concepts/messaging-message-scope-messagecallback-saga/">concepts de base</a> et savoir comment <a href="/client-api/net-package-api/messagecallbacks/">exposer des MessageCallbacks</a>.</p> <h3>Créer un scope</h3> <p>Un scope Constellation est représenté par la classe “MessageScope”. Vous pouvez créer un scope avec la méthode statique “Create”.</p> <p>Par exemple pour créer un scope vers les instances du package “MonPackage” de votre Constellation&nbsp; :</p><pre class="lang:c# decode:true">MessageScope.Create("MonPackage")</pre>
<p>Par défaut le type de scope est “Package”. La ligne ci-dessus est donc identique à :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MonPackage")</pre>
<p>Si vous souhaitez cibler une instance d’un package en particulier, vous devez spécifier la sentinelle sur lequel est héberger le package que vous ciblez :</p><pre class="lang:c# decode:true">MessageScope.Create("MaSentinelle/MonPackage")</pre>
<p>Si vous souhaitez cibler plusieurs packages :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MonPackage", "MonAutrePackage", "EncoreUnAutrePackage")</pre>
<p>Bien entendu vous pouvez donner le nom d’une instance en particulier (sentinelle + package) ou juste le nom du package :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Package, "MaSentinelle/MonPackage", "MonAutrePackage")</pre>
<p>Le scope “Sentinel” permet de cibler tous les packages sur une sentinelle. Vous pouvez définir autant de sentinelle que vous souhaitez :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Sentinel, "MaSentinelle", "MonAutreSentinelle")</pre>
<p>Le scope “Group” permet de cibler les scope qui sont abonnés à un groupe. Ici créons un scope pour tous les packages du groupe “GroupA” :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Group, "GroupA")</pre>
<p>Vous pouvez bien sûr définir autant de groupe que vous souhaitez :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Group, "GroupA", "GroupB", "GroupC")</pre>
<p>Enfin vous pouvez créer des scopes pour cilber tous les clients connectés dans votre Constellation (All) ou tout le monde sauf vous (Others). Dans ces deux cas, il n’y a pas d’argument à spécifier :</p><pre class="lang:c# decode:true">MessageScope.Create(MessageScope.ScopeType.Others)</pre>
<h3>Envoyer un message avec le SendMessage</h3>
<p>La méthode de base pour envoyer un message dans Constellation est “<em>PackageHost.SendMessage</em>”.</p>
<p>Vous devez définir trois arguments pour envoyer un message :</p>
<ul>
<li>Le scope (les destinataires de votre message) 
<li>La clé du message (MessageKey) 
<li>Le contenu du message (Data)</li></ul>
<p>Comme vous le savez, on se sert des messages pour invoquer des méthodes d’autre packages. La clé du message est en fait le nom de la méthode (= le MessageCallback) à invoquer.</p>
<p>Prenons pour exemple les MessageCallbacks définis dans <a href="/client-api/net-package-api/messagecallbacks/">l’article précédent</a>.</p>
<p>Pour invoquer le MessageCallback “MyMethod” sans paramètre (Data = null) on pourrait écrire :</p><pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "MyMethod", null);</pre>
<p>Pour une méthode avec plusieurs paramètres, ils faut les encapsuler dans un tableau d’objet :</p><pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "MyMethodWithMultipleParameters", new object[] { "Un String", 123, true });</pre>
<p>Cela fonctionne avec des objets complexes. Par exemple pour invoquer notre MessageCallback “Logon” :</p><pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "Logon", new { User = "seb", Password = "seb" });</pre>
<p>Avec l’API .NET on ne se sert généralement jamais de cette méthode “SendMessage” car vous avez à disposition le “SendMessageProxy” que nous allons découvrir ci-dessous.</p>
<h3>Envoyer un message avec un proxy dynamique</h3>
<p>La classe “SendMessageProxy” est un objet dynamique qui permet d’envoyer un message de la même manière que vous appelez une méthode.</p>
<p>Pour créer un proxy dynamique vous devez au préalable avoir créer votre scope :</p><pre class="lang:c# decode:true">MessageScope scope = MessageScope.Create("MonPremierPackage");
dynamic proxy = new SendMessageProxy(scope);</pre>
<p>Pour simplifier le code, vous pouvez utiliser la méthode d’extension “GetProxy()” sur le MessageScope :</p><pre class="lang:c# decode:true">MessageScope scope = MessageScope.Create("MonPremierPackage");
dynamic proxy = scope.GetProxy();</pre>
<p>Attention, pour accéder aux méthodes d’extension du <em>MessageScope</em> vous devez ajouter le namespace “Constellation.Package” :</p><pre class="lang:c# decode:true">using Constellation.Package;</pre>
<p>Une fois le proxy&nbsp; dynamique créé il suffit d’appeler '”fictivement” une méthode pour envoyer le message.</p>
<p>Par exemple pour envoyer le message “MyMethod” avec un contenu du vide, on écrira simplement :</p><pre class="lang:c# decode:true">proxy.MyMethod();</pre>
<p>Si votre message doit contenir plusieurs arguments :</p><pre class="lang:c# decode:true">proxy.MyMethodWithMultipleParameters("Un String", 123, true);</pre>
<p>Ou encore avec notre MessageCallback “Logon” qui prend en argument un objet complexe :</p><pre class="lang:c# decode:true">proxy.Logon(new { User = "seb", Password = "seb" })</pre>
<p>Ainsi vous pouvez invoquer un MessageCallback de n’importe quel package de la même façon que vous invoquerez une méthode de votre code .NET. La méthode invoquée est la clé du message et les arguments sont inclus dans le contenu du message.</p>
<p>Bien sûr avec la méthode d’extension vous pouvez faire tout çà en une ligne de code : créer le scope, récupérer le proxy dynamique et envoyer le message :</p><pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>
<p>Vous avez également à disposition la méthode “CreateMessageProxy” sur le PackageHost. Cette méthode permet de créer un scope et vous retourne le proxy dynamique.</p>
<p>Les deux lignes ci-dessous sont identiques :</p><pre class="lang:c# decode:true">dynamic proxy = MessageScope.Create("MonPremierPackage").GetProxy();
dynamic proxy = PackageHost.CreateMessageProxy("MonPremierPackage");</pre>
<p>De ce fait, pour invoquer nos trois MessageCallbacks, on pourrait écrire :</p><pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("MonPremierPackage").MyMethod();
PackageHost.CreateMessageProxy("MonPremierPackage").MyMethodWithMultipleParameters("Un String", 123, true);
PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });</pre>
<p>Tout comme la méthode “MessageScope.Create”, le “CreateMessageProxy” peut créer tout type de scope.</p>
<p>Par exemple, invoquons le MessageCallbacks “MyMessageCallback” avec deux arguments (un string et un datetime) sur tous les packages du groupe A et B de notre Constellation :</p><pre class="lang:c# decode:true">PackageHost.CreateMessageProxy(MessageScope.ScopeType.Group, "GroupA", "GroupB").MyMessageCallback("Mon Argument", DateTime.Now);</pre>
<h3>Invoquer un MessageCallback avec réponse – Utilisation des Sagas</h3>
<p>Lorsque vous envoyez un message dans un scope vous pouvez ajouter un identifiant de Saga sur ce scope. Cela indiquera au destinataire que vous souhaitez une réponse pour cette saga (<a href="/client-api/net-package-api/messagecallbacks/#Repondre_a_une_saga">plus d’info</a>).</p>
<p>L’identifiant de Saga doit être aléatoire et unique. Vous avez une méthode d’extension “WithSaga” sur un MessageScope vous permettant de définir l’identifiant de la saga (SagaId) avec un GUID :</p><pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").WithSaga();</pre>
<p>La méthode d’extension vous retourne le MessageScope, vous pouvez donc directement récupérer le proxy dynamique et envoyer votre message : </p><pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").WithSaga().GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>
<p>Pour la réception des réponses, vous avez une méthode “RegisterSagaResponseCallback” qui permet d’enregistrer des fonctions invoquées lorsque la réponse pour une SagaID en particulier est reçue :</p><pre class="lang:c# decode:true">PackageHost.RegisterSagaResponseCallback(sagaId, (reponse) =&gt;
{
      // Response pour la saga "sagaId"
});</pre>
<p>Pour simplifier le code, vous avez une méthode d’extension “OnSagaResponse” sur un MessageScope qui permet à la soit d’ajouter un identifiant de saga sur le scope (WithSaga) et d’enregistrer la fonction de retour.</p>
<p>De ce fait, vous pouvez en une seule ligne créer votre scope avec saga, définir le code qui sera invoqué à la réception la réponse, récupérer le proxy dynamique et invoquer votre MessageCallback avec ses paramètres :</p><pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").OnSagaResponse((response) =&gt;
{
    // Reponse de la saga
}).GetProxy().Logon(new { User = "seb", Password = "seb" });
</pre>
<p>Comme pour les MessageCallbacks, la méthode de réception d’une réponse d’une saga est invoquée dans un thread dédié dans lequel vous avez accès au contexte du message via la propriété “MessageContext.Current”.</p>
<p>Vous pouvez donc savoir quel est le package qui vous a répondu (Sender) ou encore quel était l’identifiant de la saga utilisé.</p>
<p>De plus, notez que la méthode OnSagaResponse est générique et vous pouvez définir le type de retour (le cast est réalisé automatiquement).</p>
<p>On peut donc écrire :</p><pre class="lang:c# decode:true">MessageScope.Create("MonPremierPackage").OnSagaResponse&lt;bool&gt;((response) =&gt;
{
    PackageHost.WriteInfo("Reponse de {0} pour la saga {1}",                 
        MessageContext.Current.SagaId,
        MessageContext.Current.Sender.FriendlyName);

    PackageHost.WriteInfo(response ? "OK" : "DENIED");

}).GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>
<p>Notez également que si votre scope cible plusieurs packages (ex : si “MonPremierPackage” est déployé sur plusieurs sentinelles), votre fonction de traitement de la réponse sera invoquée plusieurs fois, pour chaque package qui répond, car l’identifiant de Saga est le même).</p>
<h3>Utiliser les “Tasks” (awaitable) pour attendre la réponse d’une saga</h3>
<p>C’est une nouveauté de la 1.8 du proxy dynamique de l’API .NET de Constellation. Pour bien comprendre, résumons ce que l’on sait !</p>
<p>Lorsque vous invoquez une méthode sur le proxy dynamique, celui-ci envoie un message dans Constellation où la clé du message est le nom de la méthode invoquée et le contenu du message les paramètres de la méthode invoquée.</p>
<p>Ces deux lignes sont donc identiques :</p><pre class="lang:c# decode:true">PackageHost.SendMessage(MessageScope.Create("MonPackage"), "Logon", new { User = "seb", Password = "seb" });
MessageScope.Create("MonPremierPackage").GetProxy().Logon(new { User = "seb", Password = "seb" });</pre>
<p>Sur la 2ème ligne, vous invoquez dynamiquement la méthode “Logon” sur le <em>SendMessageProxy</em> (créé par la méthode GetProxy()) pour envoyer (<em>PackageHost.SendMessage</em>) le message “Logon”.</p>
<p>Ces deux lignes font donc bien la même chose. Dans les deux cas le message est envoyé dans un scope qui ne porte pas de “SagaId” (donc aucune réponse n’est attendue).</p>
<p>Je rappelle également que vous pouvez créer un scope et récupérer son proxy dynamique avec la méthode <em>PackageHost.CreateMessageProxy</em>. On peut donc encore simplifier le code par la ligne :</p><pre class="lang:c# decode:true">PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });</pre>
<p>D’un point de vue .NET, l’invocation dynamique de notre MessageCallback&nbsp; “Logon" sur le proxy dynamique ne retourne rien, du moins il retourne “null”.</p><pre class="lang:c# decode:true">object result = PackageHost.CreateMessageProxy("MonPremierPackage").Logon(new { User = "seb", Password = "seb" });
// ici result = null</pre>
<p>Observez maintenant le code ci-dessous :</p><pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });</pre>
<p>Si vous examinez attentivement le MessageCallback que nous invoquons sur le proxy dynamique, on utilise une forme générique : au lieu d’avoir “Logon(xxxxx)”, on a ”Logon<strong>&lt;bool&gt;</strong>(xxxxx)<em>”</em></p>
<p>Le fait de définir un type générique indique au proxy dynamique que vous attendez une réponse et donc qu’il s’agit d’une saga !</p>
<p>Le type générique est le type de réponse que vous attendez. Ici le MessageCallback “Logon” que vous avons écrit dans l’article précédent renvoie un booléen (bool). Vous pouvez utiliser n’importe quel type de base ou des types complexes.</p>
<p>Si c’est un objet anonyme ou bien que vous n’avez pas la définition de l’objet de retour dans votre code,&nbsp; vous pouvez utiliser un “dynamic” (ex <em>Logon<strong>&lt;dynamic&gt;</strong>(xxxx)</em>)<em>.</em></p>
<p>Quoi qu’il en soit il est obligatoire d’invoquer le MessageCallback&nbsp; en utilisant la syntaxe générique pour que le proxy dynamique vous retourne une Task et surtout qu’il ajoute automatiquement un identifiant de saga (<em>WithSaga</em>) lors de l’envoi du message.</p>
<p>Revenons donc à notre appel :</p><pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });</pre>
<p>Le proxy dynamique nous retourne une ”Task&lt;dynamic&gt;”, c’est à dire une tache qui se charge d’envoyer et d’attendre la réception de la (première) réponse. </p>
<p>Vous pouvez bloquer le thread appelant jusqu'à ce que l'opération asynchrone soit terminée,&nbsp; c’est à dire jusqu’à l’obtention de la réponse :</p><pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
PackageHost.WriteInfo((bool)reponse.Result ? "OK" : "DENIED");</pre>
<h4>Ajouter des timeouts</h4>
<p>Comme vous bloquez le thread appelant, prenez garde de mettre des gardes fou car nous n’avez aucune garantie qu’un package va répondre à votre saga. La tache pourrait ne jamais être complétée! Vous devriez donc attendre un certain temps avant de considérer la tache “hors délai”.</p>
<p>Par exemple attendons pendant 3 secondes maximum une réponse à notre Saga “Logon” :</p><pre class="lang:c# decode:true">Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
if (reponse.Wait(3000) &amp;&amp; reponse.IsCompleted)
{
    PackageHost.WriteInfo((bool)reponse.Result ? "OK" : "DENIED");
}
else
{
    PackageHost.WriteError("Aucune réponse !");
}</pre>
<p>Ainsi vous bloquerez le thread au maximum 3 secondes.</p>
<h4>Async/Await</h4>
<p>Aussi vous pouvez utiliser le pattern async/await, très pratique pour les packages UI WPF/Winform :</p><pre class="lang:c# decode:true">public async void Logon()
{
    bool reponse = await PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
    PackageHost.WriteInfo(reponse ? "OK" : "DENIED");
}</pre>
<h4>Résultat de la tache</h4>
<p>Notez bien que le proxy dynamique vous renverra toujours une “Task&lt;<strong>dynamic</strong>&gt;” quelque soit le type générique précisé au niveau de l’appel du MessageCallback (“bool” dans l’exemple du “Logon”).</p>
<p>Cependant le résultat de la tache (Task.Result) est bien du type que celui passé au niveau de l’appel du MessageCallback.</p>
<p>Si vous souhaitez toutefois récupérer une Task&lt;T&gt; où T est le type de retour de votre saga, vous devez créer une nouvelle tache de la tache pour réaliser le “cast”.</p>
<p>Par exemple :</p><pre class="lang:c# decode:true">Task&lt;dynamic&gt; task = PackageHost.CreateMessageProxy("MonPremierPackage").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" });
Task&lt;bool&gt; logonTask = task.ContinueWith&lt;bool&gt;(t =&gt; (bool)t.Result);</pre>
<h4>Annuler les taches</h4>
<p>Lorsque l’on créé des taches en .NET on veut souvent leur associer un jeton d’annulation : le CancellationToken. Cela permet d’annuler la tache !</p>
<p>Pour cela, il vous suffit de passer un “CancellationToken” comme dernier argument de votre invocation du MessageCallback :</p><pre class="lang:c# decode:true">CancellationTokenSource source = new CancellationTokenSource(1000);
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
<p>Dans l’exemple ci-dessus on crée un CancellationToken à partir d’un CancellationTokenSource qu’on passe en tant que dernier paramètre du MessageCallback.</p>
<p>Le CancellationTokenSource est ici construit pour annuler la tache après 1seconde (1000 ms). Vous pouvez aussi vous même annuler la tache en en appelant la méthode “Cancel” :</p><pre class="lang:c# decode:true">source.Cancel();</pre>
<p>Bien entendu, le proxy dynamique se sert de votre CancellationToken pour savoir quand annuler la tache et le retire des paramètres du message (= le CancellationToken n’est pas envoyer dans le message !).</p>
<p>Prenez garde à bien gérer les exceptions, si la tache est annulée une exception “TaskCanceledException” est levée.</p>
<h4>Le contexte du message</h4>
<p>Dans une méthode marquée comme “MessageCallback” ou dans le callback de retour d’une saga (OnSagaResponse) vous pouvez toujours accéder au contexte du message avec la propriété “MessageContext.Current”.</p>
<p>En effet les MessageCallbacks ou les callbacks de retour des sagas sont invoqués dans un thread à part ce qui permet de l’attacher au contexte de réception du message.</p>
<p>Cependant avec les taches, le résultat est dispatcher dans le thread de l’appelant. Autrement dit, vous ne pouvez pas accéder au contexte courant (<em>MessageContext.Current</em>) !</p>
<p>La solution consiste à “demander” au proxy dynamique de vous “remplir” un contexte que vous avez initialisé.</p>
<p>Commencez donc par déclarer un contexte vide (<em>MessageContext.None</em>) et passer l’instance comme dernière paramètre :</p><pre class="lang:c# decode:true">MessageContext context = MessageContext.None;
Task&lt;dynamic&gt; reponse = PackageHost.CreateMessageProxy("ConstellationPackageConsole1").Logon&lt;bool&gt;(new { User = "seb", Password = "seb" }, context);
PackageHost.WriteInfo("Result = {0} - Response received from : {1}", reponse.Result, context.Sender.FriendlyName);</pre>
<p>Une fois la tache complétée, la variable “context” est remplie avec le contexte de la réponse lors de la réception. Vous pouvez donc accéder au contexte et par exemple afficher le “Sender”.</p>
<p>Le MessageContext est toujours le dernière paramètre. Le CancellationToken est donc l’avant dernier paramètre dans le cas où vous utilisez les deux :</p><pre class="lang:c# decode:true">CancellationTokenSource source = new CancellationTokenSource(1000);
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
<p>Comme l’ensemble des MessageCallbacks sont déclarés dans la Constellation (sauf si ils sont marqués comme “Hidden”), il est possible de gérer le code d’invocation dans votre code C#.</p>
<p>Dans le menu contextuel de votre projet Visual Studio, sélectionnez “Generate Code” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-125.png"><img title="G&eacute;n&eacute;rer du code" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="G&eacute;n&eacute;rer du code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-102.png" width="424" height="330"></a></p>
<p align="left">Sélectionnez votre Constellation et cliquez sur “Discover” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-126.png"><img title="G&eacute;n&eacute;rer du code" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="G&eacute;n&eacute;rer du code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-103.png" width="424" height="469"></a></p>
<p align="left">Sélectionnez les packages et cliquez sur “Generate”.</p>
<p align="left">Le générateur de code va créer différentes classes présentant votre Constellation avec les packages sélectionnés (MessageCallbacks et StateObjects).</p>
<p align="left">Dans notre cas nous allons inclure la définition des MessagesCallbacks de notre package “MonPremierPackage”. </p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-127.png"><img title="Ajout du using" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Ajout du using" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-104.png" width="424" height="132"></a></p>
<p align="left">Commençons donc par ajouter le using vers le MessageCallbacks de MonPremierPackage :</p><pre class="lang:c# decode:true">using ConstellationPackageConsole1.MonPremierPackage.MessageCallbacks;</pre>
<p align="left">Dans la classe générée “MyConstellation” vous retrouverez la liste des sentinelles et packages de votre Constellation :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-128.png"><img title="Enum&eacute;ration g&eacute;n&eacute;r&eacute;e" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Enum&eacute;ration g&eacute;n&eacute;r&eacute;e" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-105.png" width="424" height="65"></a></p>
<p>Vous pourrez alors créer un scope personnalisé pour le package sélectionné :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-129.png"><img title="MessageScope g&eacute;n&eacute;r&eacute; par package" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageScope g&eacute;n&eacute;r&eacute; par package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-106.png" width="424" height="84"></a></p>
<p align="left">Ce scope personnalisé contiendra l’ensemble des MessageCallbacks :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-130.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-107.png" width="424" height="69"></a></p>
<p align="left">Sur chaque MessageCallback, vous retrouvez les bons types de retour, les commentaires et les types complexes proprement générés :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-131.png"><img title="MessageCallbacks g&eacute;n&eacute;r&eacute;s" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallbacks g&eacute;n&eacute;r&eacute;s" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-108.png" width="424" height="67"></a></p>
<p>De ce fait, pour reprendre l’exemple du MessageCallback “Logon”, on pourra écrire le code :</p><pre class="lang:c# decode:true">var context = MessageContext.None;
var token = new CancellationTokenSource(5000).Token;
var user = new UserInfo() { User = "seb", Password = "seb" };

var task = MyConstellation.Packages.MonPremierPackage.CreateMonPremierPackageScope().Logon(user, token, out context);

PackageHost.WriteInfo("Logon user {0} - Result = {1} - Sender : {2}", user.User, task.Result, context.Sender.FriendlyName);</pre>
<p><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-132.png"><img title="D&eacute;monstration" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; border-top-width: 0px; margin-right: auto" border="0" alt="D&eacute;monstration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-109.png" width="424" height="205"></a></p>