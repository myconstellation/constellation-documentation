---
ID: 1343
post_title: 'MessageCallbacks : exposez vos m&eacute;thodes'
author: Sebastien Warin
post_date: 2016-03-16 15:14:25
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/messagecallbacks/
published: true
post_modified: 2017-05-07 12:40:33
---
Avant de tout, il est recommandé de lire <a href="/concepts/messaging-message-scope-messagecallback-saga/">cet article d’introduction aux concepts de Message, Scope, MessageCallback &amp; Saga</a> dans Constellation pour bien comprendre le principe des MessageCallbacks.

<h3>Exposer des méthodes</h3>

Pour exposer une méthode .NET dans Constellation, il suffit d'ajouter l'attribut “MessageCallback” sur une méthode :

<pre class="lang:c# decode:true">[MessageCallback]
void MyMethod()
{
    PackageHost.WriteInfo("This is a MessageCallback !");
}
</pre>

Cela fonctionne que votre méthode soit privée ou public, d'instance ou statique.

Une méthode marquée MessageCallback peut accepter zéro, un ou plusieurs paramètres :

<pre class="lang:c# decode:true">[MessageCallback]       
void MyMethodWithMultipleParameters(string input, int number, bool boolean)
{
    PackageHost.WriteInfo("Input= {0} - Number: {1} - Boolean: {2}", input, number, boolean);
}</pre>

De plus chaque paramètre peut être de type simple ou complexe :

<pre class="lang:c# decode:true">[MessageCallback] 
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password); 
}</pre>

Par défaut seuls les méthodes marquées [MessageCallback] de votre classe ”IPackage” sont enregistrées.

Si vous souhaitez également enregistrer les MessageCallbacks référencés dans vos autres types, vous devez appeler la méthode “RegisterMessageCallbacks” en passant un instance ou un type à enregistrer :

<pre class="lang:c# decode:true">PackageHost.RegisterMessageCallbacks(source);</pre>

<h3>Testez vos MessageCallbacks depuis la Console Constellation</h3>

Si vous <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">publiez votre package</a> ou <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">démarrer votre package depuis VS</a>  avec les trois méthodes ci-dessus, vous retrouverez les trois MessageCallbacks dans <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">l’explorateur </a>:

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-116.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-93.png" alt="MessageCallback Explorer" width="424" height="191" border="0" /></a></p>

<p align="left">Tout y est décrit, y compris les types complexes :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-117.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Description des types" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-94.png" alt="Description des types" width="424" height="165" border="0" /></a></p>

<p align="left">Vous pourrez alors directement tester vos MC depuis la Console :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-118.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-95.png" alt="MessageCallback Explorer" width="424" height="58" border="0" /></a></p>

<p align="left">Ce qui compte tenu du code ci-dessus affichera des messages dans les logs Constellation visible sur la <a href="/constellation-platform/constellation-console/console-log/">Console Log</a> :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-119.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-96.png" alt="Demo" width="424" height="25" border="0" /></a></p>

<h3 align="left">Personnaliser le nom du MessageCallback</h3>

<p align="left">Vous pouvez également changer le nom du MessageCallback sans forcement changer le nom de votre méthode :</p>

<pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);
}</pre>

<p align="left">Ici la méthode se nomme toujours “MyMethodWithComplexParameter” mais est vu dans Constellation comme “Logon” et pour invoquer cette méthode, elle devra recevoir des messages dont la clé est “Logon”.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-120.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-97.png" alt="MessageCallback Explorer" width="424" height="212" border="0" /></a></p>

<h3 align="left">MessageCallback caché</h3>

<p align="left">Il se peut aussi que votre package expose des MessageCallbacks que vous ne souhaitez “rendre publique”.</p>

<p align="left">En clair, vous ne voulez pas que votre MC se retrouve dans la description de votre package.</p>

<p align="left">Exemple : vous avez un package qui envoie des messages à un groupe pour prévenir de l’arrivée d’un évènement (ex: le téléphone sonne). Vous souhaitez, dans le code de votre package, déclencher une méthode lorsque cet évènement se produit. Autrement dit, vous créez une méthode en “MessageCallback“ qui porte le même nom que le MessageKey envoyé par l’autre package (même nom de méthode ou via la propriété Key comme vu ci-dessus).</p>

<p align="left">Ainsi, comme votre package fait partie du groupe cible, lorsque le téléphone sonne, l’autre package envoie un message au groupe, vous recevez donc le message et comme vous avez un MessageCallback du même temps, votre méthode est invoquée !</p>

<p align="left">C’est parfait à l’exception que tout le monde “voit l’existence” de cette méthode et peuvent tous l’invoquer !</p>

<p align="left">Deux solutions :</p>

<ul>
    <li>
<div align="left">Cacher le MessageCallback (= ne pas le déclarer dans la description du package)</div></li>
    <li>
<div align="left">Vérifier l’émetteur</div></li>
</ul>

<p align="left">Pour cacher un MC, il suffit simple de définir la propriété “IsHidden” à vrai :</p>

<pre class="lang:c# decode:true">[MessageCallback(IsHidden = true)]
void HiddenMethod()
{
    PackageHost.WriteInfo("This is a MessageCallback !");
}</pre>

<p align="left">Ainsi la méthode n’apparaitra nul part, personne ne pourra savoir que votre package “écoute” les messages dont la clé est ici “HiddenMethod”.</p>

<h3 align="left">MessageContext</h3>

<p align="left">Cependant si vous souhaitez aller plus loin et contrôler dans votre code l’émetteur du message, vous pouvez depuis chaque “MessageCallback”, accéder au contexte du message via la propriété “MessageContext.Current”.</p>

<p align="left">Le contexte du message contient notamment le MessageSender (identitié de l’emetteur du message) et le MessageScope (scope dans lequel a été envoyé le message).</p>

<p align="left">Par exemple on pourrait écrire :</p>

<pre class="lang:c# decode:true">[MessageCallback(IsHidden = true)]
void HiddenMethod()
{
    if(MessageContext.Current.Sender.FriendlyName == "SENTINEL-DEMO/MySafePackage")
    {
        PackageHost.WriteInfo("OK j’autorise le package MySafePackage de la sentinelle SENTINEL-DEMO");
    }
    else
    {
        PackageHost.WriteWarn("Je refuse que {0} puisse invoquer cette méthode !", MessageContext.Current.Sender.FriendlyName);
    }
}
</pre>

<h3>Répondre à une saga</h3>

Comme nous l’avons vu dans les concepts, il est possible de répondre à un message en renvoyant un message avec le même identifiant de saga. De ce fait, un MessageCallback peut renvoyer une réponse.

Dans la pratique, il suffit de regarder dans le contexte du message si c’est une saga (c’est à dire que le scope porte un identifiant de saga). Si c’est une saga, vous pouvez appeler la méthode d’extension “SendResponse” qui se charge pour vous d’envoyer le contenu de votre message dans un scope portant le même identifiant de saga et à destination du package émetteur.

De plus, pour indiquer que le MessageCallback peut répondre au saga, on indique quel est le type d’objet de réponse.

Par exemple :

<pre class="lang:c# decode:true">[MessageCallback(Key="Logon", ResponseType=typeof(bool))]
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    if (MessageContext.Current.IsSaga)
    {
        MessageContext.Current.SendResponse(user.User == user.Password);
    }
}</pre>

Avec Constellation 1.8, la syntaxe est encore plus simple. Vous pouvez simplement écrire :

<pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
bool MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    return user.User == user.Password;
}</pre>

La propriété “ResponseType” du MessageCallback est automatiquement définie avec le type de retour de la méthode. C’est l’API .NET qui se chargera d’envoyer la réponse si le message est reçu dans une saga. La réponse renvoyée étant l’objet retourné par votre méthode, dans l’exemple un booléen.

Ainsi en republiant le package avec le code ci-dessus, vous constaterez dans le MessageCallbacks Explorer que votre MessageCallback peut répondre au Saga :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-121.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback avec saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-98.png" alt="MessageCallback avec saga" width="424" height="209" border="0" /></a></p>

<p align="left">Vous pourrez alors tester votre MessageCallback :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-122.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Envoyer un message dans une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-99.png" alt="Envoyer un message dans une saga" width="424" height="244" border="0" /></a></p>

<p align="left">Et visualisez le message de retour, ici un booléen :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-123.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Réponse d'une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-100.png" alt="Réponse d'une saga" width="424" height="438" border="0" /></a></p>

<h3>Décrire ses MessageCallbacks</h3>

Pour finir, il est conseillé de décrire ses MessageCallbacks en ajoutant des descriptions sur vos messages Callbacks et les types utilisées par les paramètres et les réponses.

Vous avez deux possibilités :

<ul>
    <li>Utilisez la propriété “Description” sur l’attribut MessageCallback (mais limité aux MC et non aux types)</li>
    <li>Utilisez les commentaires de documentation XML</li>
</ul>

Pour gagner en productivité, je vous recommande l’extension Visual Studio <a href="http://submain.com/products/ghostdoc.aspx">GhostDoc de SubMain</a> pour générer automatiquement des commentaires de documentation XML dans votre code .NET.

Ainsi ajoutons des commentaires de documentation XML sur notre MessageCallbacks et la classe UserInfo utilisée comme paramètre de notre MC :

<pre class="lang:c# decode:true">/// &lt;summary&gt;
/// Methode d'identification d'un utilisateur.
/// &lt;/summary&gt;
/// &lt;param name="user"&gt;L'utilisateur à identifier.&lt;/param&gt;
/// &lt;returns&gt;&lt;c&gt;true&lt;/c&gt; si identifié&lt;/returns&gt;
[MessageCallback(Key="Logon")]
bool MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    return user.User == user.Password;
}

/// &lt;summary&gt;
/// Classe decrivrant un utilisateur
/// &lt;/summary&gt;
public class UserInfo
{
    /// &lt;summary&gt;
    /// Le nom d'utilisateur
    /// &lt;/summary&gt;
    public string User { get; set; }

    /// &lt;summary&gt;
    /// Le mot de passe de l'utilisateur
    /// &lt;/summary&gt;
    public string Password { get; set; }
}</pre>

Si vous redéployer votre package, vous observerez vos commentaires XML directement dans la description de vos MessageCallbacks :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-124.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback documenté" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-101.png" alt="MessageCallback documenté" width="424" height="128" border="0" /></a></p>

<p align="left">Par défaut seuls les MessageCallbacks de l’instance de votre ”IPackage” sont enregistrés et donc ajoutés à la description de votre package. Comme nous l’avons vu ci-dessus, vous pouvez enregistrer les méthodes marquées [MessageCallbacks] d’autres classes en invoquant la méthode  “RegisterMessageCallbacks”.</p>

<p align="left">Une fois que vous avez enregistré tous vos autres MessageCallbacks vous devez renvoyer la description de votre package dans la Constellation avec la méthode “DeclarePackageDescriptor” :</p>

<pre class="lang:c# decode:true">PackageHost.DeclarePackageDescriptor();</pre>

<h3>S’abonner et recevoir les messages d’un groupe</h3>

Comme <a href="/concepts/messaging-message-scope-messagecallback-saga/">vous le savez</a>, il existe plusieurs type de scope pour recevoir un message. Par défaut votre package recevra les messages adressés au scope “All” et “Other” (si il n’est pas l’émetteur du message) mais aussi ceux destinés au scope de sa sentinelle et/ou pour son package.

Il y a un dernier type de scope fort utile : “les groupes”. Pour recevoir des messages destinés à un groupe, il faut s’abonnez à ce groupe.

Vous avez deux manières d’ajouter votre package dans un groupe :

<ul>
    <li>Depuis la configuration du serveur</li>
    <li>Depuis le code de votre package</li>
</ul>

La première méthode se réalise dans la configuration de votre package directement dans la configuration du serveur Constellation.

Il suffit d’ajouter un élément “group” en indiquant les noms des groupes à rejoindre.

<pre class="lang:xml decode:true">&lt;sentinel name="PO-SWARIN" credential="StandardAccess"&gt;
    &lt;packages&gt;        
        &lt;package name="HWMonitor" enable="true"&gt;&lt;/package&gt;  
        &lt;package name="ConstellationPackageConsole1" enable="true"&gt;&lt;/package&gt; 
        &lt;package name="MonPremierPackage" enable="true"&gt;
            &lt;groups&gt;
                &lt;group name="MonGroupDemo" /&gt;
            &lt;/groups&gt;
            &lt;settings&gt;
                &lt;setting key="Interval" value="2000" /&gt;
            &lt;/settings&gt;
        &lt;/package&gt;  
    &lt;/packages&gt;                
 &lt;/sentinel&gt;</pre>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-149.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout de groupe dans la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-126.png" alt="Ajout de groupe dans la configuration" width="424" height="261" border="0" /></a></p>

<p align="left">L’autre manière, dans le code, avec la méthode “<em>PackageHost.SubscribeMessages</em>” (ou <em>PackageHost.UnsubscribeMessages</em>). Par exemple :</p>

<pre class="lang:c# decode:true">PackageHost.SubscribeMessages("MonGroupDemo");</pre>