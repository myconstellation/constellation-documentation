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
post_modified: 2016-03-21 15:13:08
---
<p>Avant de tout, il est recommandé de lire <a href="/concepts/messaging-message-scope-messagecallback-saga/">cet article d’introduction aux concepts de Message, Scope, MessageCallback &amp; Saga</a> dans Constellation.</p> <h3>Exposer des méthodes</h3> <p>Pour exposer une méthode .NET dans Constellation, il suffit de déclarer la méthode comme “MessageCallback” en ajoutant un attribut :</p><pre class="lang:c# decode:true">[MessageCallback]
void MyMethod()
{
    PackageHost.WriteInfo("This is a MessageCallback !");
}
</pre>
<p>Même si votre méthode est privée au niveau de votre classe, elle sera exposée comme MessageCallback si l’attribut est présent.</p>
<p>Un MessageCallback peut accepter zéro, un ou plusieurs paramètres :</p><pre class="lang:c# decode:true">[MessageCallback]       
void MyMethodWithMultipleParameters(string input, int number, bool boolean)
{
    PackageHost.WriteInfo("Input= {0} - Number: {1} - Boolean: {2}", input, number, boolean);
}</pre>
<p><br>De plus chaque paramètre peut être un type simple ou un type complexe :<br></p><pre class="lang:c# decode:true">[MessageCallback] 
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password); 
}</pre>
<p><br>Par défaut seuls les MessageCallbacks de l’instance de votre ”IPackage” sont enregistrés. Si vous souhaitez enregistrer les MessageCallbacks d’une autre instance vous devez appeler la méthode “RegisterMessageCallbacks” en passant l’instance de l’objet à enregistrer :<br></p><pre class="lang:c# decode:true">PackageHost.RegisterMessageCallbacks(source);</pre>
<h3>MessageCallbacks Explorer</h3>
<p>Si vous publiez un package dans votre Constellation avec les trois méthodes ci-dessus, vous retrouverez les trois MessageCallbacks dans l’explorateur :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-116.png"><img title="MessageCallback Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-93.png" width="424" height="191"></a></p>
<p align="left">Tout y est décrit, y compris les types complexes :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-117.png"><img title="Description des types" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Description des types" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-94.png" width="424" height="165"></a></p>
<p align="left">Vous pourrez alors directement tester vos MC depuis la Console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-118.png"><img title="MessageCallback Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-95.png" width="424" height="58"></a></p>
<p align="left">Ce qui compte tenu du code ci-dessus, m’affichera ce message dans les logs :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-119.png"><img title="Demo" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-96.png" width="424" height="25"></a></p>
<h3 align="left">Personnaliser le nom du MessageCallback</h3>
<p align="left">Vous pouvez également changer le nom du MessageCallback sans forcement changer le nom de votre méthode :</p><pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);
}</pre>
<p align="left">Ici la méthode se nomme toujours “MyMethodWithComplexParameter” mais est vu dans Constellation comme “Logon” et pour invoquer cette méthode, elle devra recevoir des messages dont la clé est “Logon”.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-120.png"><img title="MessageCallback Explorer" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-97.png" width="424" height="212"></a></p>
<h3 align="left">MessageCallback caché</h3>
<p align="left">Il se peut aussi que votre package expose des MessageCallbacks que vous ne souhaitez “rendre publique”.</p>
<p align="left">En clair, vous ne voulez pas que votre MC se retrouve dans la description de votre package.</p>
<p align="left">Exemple : vous avez un package qui envoie des messages à un groupe pour prévenir de l’arrivée d’un évènement (ex: le téléphone sonne). Vous souhaitez, dans le code de votre package, déclencher une méthode lorsque cet évènement se produit. Autrement dit, vous créez une méthode en “MessageCallback“ qui porte le même nom que le MessageKey envoyé par l’autre package (même nom de méthode ou via la propriété Key comme vu ci-dessus).</p>
<p align="left">Ainsi, comme votre package fait partie du groupe cible, lorsque le téléphone sonne, l’autre package envoie un message au groupe, vous recevez donc le message et comme vous avez un MessageCallback du même temps, votre méthode est invoquée !</p>
<p align="left">C’est parfait à l’exception que tout le monde “voit l’existence” de cette méthode et peuvent tous l’invoquer !</p>
<p align="left">Deux solutions :</p>
<ul>
<li>
<div align="left">Cacher le MessageCallback (= ne pas le déclarer dans la description du package)</div>
<li>
<div align="left">Vérifier l’émetteur</div></li></ul>
<p align="left">Pour cacher un MC, il suffit simple de définir la propriété “IsHidden” à vrai :</p><pre class="lang:c# decode:true">[MessageCallback(IsHidden = true)]
void HiddenMethod()
{
    PackageHost.WriteInfo("This is a MessageCallback !");
}</pre>
<p align="left">Ainsi la méthode n’apparaitra nul part, personne ne pourra savoir que votre package “écoute” les messages dont la clé est ici “HiddenMethod”.</p>
<h3 align="left">MessageContext</h3>
<p align="left">Cependant si vous souhaitez aller plus loin et contrôler dans votre code l’émetteur du message, vous pouvez depuis chaque “MessageCallback”, accéder au contexte du message via la propriété “MessageContext.Current”.</p>
<p align="left">Le contexte du message contient notamment le MessageSender (identitié de l’emetteur du message) et le MessageScope (scope dans lequel a été envoyé le message).</p>
<p align="left">Par exemple on pourrait écrire :</p><pre class="lang:c# decode:true">[MessageCallback(IsHidden = true)]
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
<p>Comme nous l’avons vu dans les concepts, il est possible de répondre à un message en renvoyant un message avec le même identifiant de saga. De ce fait, un MessageCallback peut renvoyer une réponse.</p>
<p>Dans la pratique, il suffit de regarder dans le contexte du message si c’est une saga (c’est à dire que le scope porte un identifiant de saga). Si c’est une saga, vous pouvez appeler la méthode d’extension “SendResponse” qui se charge pour vous d’envoyer le contenu de votre message dans un scope portant le même identifiant de saga et à destination du package émetteur.</p>
<p>De plus, pour indiquer que le MessageCallback peut répondre au saga, on indique quel est le type d’objet de réponse.</p>
<p>Par exemple :</p><pre class="lang:c# decode:true">[MessageCallback(Key="Logon", ResponseType=typeof(bool))]
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    if (MessageContext.Current.IsSaga)
    {
        MessageContext.Current.SendResponse(user.User == user.Password);
    }
}</pre>
<p><br>Avec Constellation 1.8, la syntaxe est encore plus simple. Vous pouvez simplement écrire :<br></p><pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
bool MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    return user.User == user.Password;
}</pre>
<p>La propriété “ResponseType” du MessageCallback est automatiquement définie avec le type de retour de la méthode. C’est l’API .NET qui se chargera d’envoyer la réponse si le message est reçu dans une saga. La réponse renvoyée étant l’objet retourné par votre méthode, dans l’exemple un booléen.</p>
<p>Ainsi en republiant le package avec le code ci-dessus, vous constaterez dans le MessageCallbacks Explorer que votre MessageCallback peut répondre au Saga :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-121.png"><img title="MessageCallback avec saga" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallback avec saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-98.png" width="424" height="209"></a></p>
<p align="left">Vous pourrez alors tester votre MessageCallback :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-122.png"><img title="Envoyer un message dans une saga" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Envoyer un message dans une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-99.png" width="424" height="244"></a></p>
<p align="left">Et visualisez le message de retour, ici un booléen :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-123.png"><img title="R&eacute;ponse d'une saga" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="R&eacute;ponse d'une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-100.png" width="424" height="438"></a></p>
<h3>Décrire ses MessageCallbacks</h3>
<p>Pour finir, il est conseillé de décrire ses MessageCallbacks en ajoutant des descriptions sur vos messages Callbacks et les types utilisées par les paramètres et les réponses.</p>
<p>Vous avez deux possibilités :</p>
<ul>
<li>Utilisez la propriété “Description” sur l’attribut MessageCallback (mais limité aux MC et non aux types) 
<li>Utilisez les commentaires de documentation XML </li></ul>
<p>Pour gagner en productivité, je vous recommande l’extension Visual Studio <a href="http://submain.com/products/ghostdoc.aspx">GhostDoc de SubMain</a> pour générer automatiquement des commentaires de documentation XML dans votre code .NET.</p>
<p>Ainsi ajoutons des commentaires de documentation XML sur notre MessageCallbacks et la classe UserInfo utilisée comme paramètre de notre MC :</p><pre class="lang:c# decode:true">/// &lt;summary&gt;
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
<p>Si vous redéployer votre package, vous observerez vos commentaires XML directement dans la description de vos MessageCallbacks :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-124.png"><img title="MessageCallback document&eacute;" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MessageCallback document&eacute;" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-101.png" width="424" height="128"></a></p>
<p align="left">Par défaut seuls les MessageCallbacks de l’instance de votre ”IPackage” sont enregistrés et donc ajoutés à la description de votre package. Comme nous l’avons vu ci-dessus, vous pouvez enregistrer les méthodes marquées [MessageCallbacks] d’autres classes en invoquant la méthode&nbsp; “RegisterMessageCallbacks”.</p>
<p align="left">Une fois que vous avez enregistré tous vos autres MessageCallbacks vous devez renvoyer la description de votre package dans la Constellation avec la méthode “DeclarePackageDescriptor” :</p><pre class="lang:c# decode:true">PackageHost.DeclarePackageDescriptor();</pre>
<h3>S’abonner et recevoir les messages d’un groupe</h3>
<p>Comme <a href="/concepts/messaging-message-scope-messagecallback-saga/">vous le savez</a>, il existe plusieurs type de scope pour recevoir un message. Par défaut votre package recevra les messages adressés au scope “All” et “Other” (si il n’est pas l’émetteur du message) mais aussi ceux destinés au scope de sa sentinelle et/ou pour son package.</p>
<p>Il y a un dernier type de scope fort utile : “les groupes”. Pour recevoir des messages destinés à un groupe, il faut s’abonnez à ce groupe.</p>
<p>Vous avez deux manières d’ajouter votre package dans un groupe :</p>
<ul>
<li>Depuis la configuration du serveur 
<li>Depuis le code de votre package</li></ul>
<p>La première méthode se réalise dans la configuration de votre package directement dans la configuration du serveur Constellation.</p>
<p>Il suffit d’ajouter un élément “group” en indiquant les noms des groupes à rejoindre.</p><pre class="lang:xml decode:true">&lt;sentinel name="PO-SWARIN" credential="StandardAccess"&gt;
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
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-149.png"><img title="Ajout de groupe dans la configuration" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Ajout de groupe dans la configuration" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-126.png" width="424" height="261"></a></p>
<p align="left">L’autre manière, dans le code, avec la méthode “<em>PackageHost.SubscribeMessages</em>” (ou <em>PackageHost.UnsubscribeMessages</em>). Par exemple :</p><pre class="lang:c# decode:true">PackageHost.SubscribeMessages("MonGroupDemo");</pre>