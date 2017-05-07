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
post_modified: 2017-05-07 14:06:35
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
Par défaut seules les méthodes marquées [MessageCallback] sur votre classe ”<a href="/client-api/net-package-api/les-bases-des-packages-net/">IPackage</a>” sont enregistrées.

Si vous souhaitez également enregistrer les MessageCallbacks référencés de vos autres types, vous devez appeler la méthode “RegisterMessageCallbacks” en passant en paramètre l'instance ou le type à enregistrer :
<pre class="lang:c# decode:true">PackageHost.RegisterMessageCallbacks(source);</pre>
<h3>Testez vos MessageCallbacks depuis la Console Constellation</h3>
Si vous <a href="/constellation-platform/constellation-sdk/publier-package-visual-studio/">publiez votre package</a> ou <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/">démarrer votre package depuis VS</a>  avec les trois méthodes ci-dessus, vous retrouverez ces trois MessageCallbacks dans <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">l’explorateur </a>:
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-116.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-93.png" alt="MessageCallback Explorer" width="424" height="191" border="0" /></a></p>
<p align="left">Tout y est décrit, y compris les types complexes :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-117.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Description des types" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-94.png" alt="Description des types" width="424" height="165" border="0" /></a></p>
<p align="left">Vous pourrez alors directement tester vos MC depuis la Console :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-118.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-95.png" alt="MessageCallback Explorer" width="424" height="58" border="0" /></a></p>
<p align="left">Ce qui compte tenu du code donné en exemple ci-dessus affichera des messages dans les logs Constellation visible sur la <a href="/constellation-platform/constellation-console/console-log/">Console Log</a> :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-119.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Demo" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-96.png" alt="Demo" width="424" height="25" border="0" /></a></p>

<h3 align="left">Personnaliser le nom du MessageCallback</h3>
<p align="left">Vous pouvez également changer le nom du MessageCallback sans forcement changer le nom de votre méthode :</p>

<pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
void MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);
}</pre>
<p align="left">Ici la méthode se nomme toujours “MyMethodWithComplexParameter” dans notre code C# mais est vue dans Constellation comme le MessageCallback nommé “Logon” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-120.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-97.png" alt="MessageCallback Explorer" width="424" height="212" border="0" /></a></p>

<h3 align="left">MessageCallback caché</h3>
<p align="left">Vous pouvez aussi exposer des MessageCallbacks que vous ne souhaitez “rendre publique”. En d'autre mots, vous ne souhaitez pas que votre MC soit référencé dans la description de votre package.</p>
<p align="left">Exemple : vous avez un package qui envoie des messages à un groupe pour prévenir de l’arrivée d’un événement (ex: le téléphone sonne). Vous souhaitez, dans le code de votre package, déclencher une méthode lorsque cet événement se produit. Autrement dit, vous créez une méthode marquée “MessageCallback“ qui porte le même nom que le MessageKey envoyé par l’autre package (le même nom de méthode ou via la propriété Key comme vu ci-dessus).</p>
<p align="left">Ainsi, comme votre package fait partie du groupe cible, lorsque le téléphone sonne, l’autre package envoie un message au groupe, vous recevez donc le message et comme vous avez un MessageCallback du même nom, votre méthode est invoquée !</p>
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
<p align="left">Ainsi la méthode, n'apparaîtra nulle part, personne ne pourra savoir que votre package “écoute” des messages dont la clé est ici “HiddenMethod”.</p>

<h3 align="left">MessageContext</h3>
<p align="left">Vous pouvez, dans le code d'un “MessageCallback”, accéder au contexte du message reçu via la propriété “<em>MessageContext.Current</em>”.</p>
<p align="left">Le contexte du message contient notamment le MessageSender (identifié de l’émetteur du message) et le MessageScope (scope dans lequel le message a été envoyé ).</p>
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

Dans la pratique, il suffit de regarder dans le contexte du message si c’est une saga (c’est à dire que le scope porte un identifiant de saga). Si c’est une saga, vous pouvez appeler la méthode d’extension “<em>SendResponse</em>” qui se chargera pour vous d’envoyer le contenu de votre message dans un scope portant le même identifiant de saga et à destination du package émetteur.

De plus, pour indiquer que le MessageCallback répond aux sagas, on indique quel est le type de l’objet de réponse dans l'attribut [MessageCallback] avec la propriété <em>ResponseType</em>.

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
Avec Constellation 1.8, la syntaxe est encore plus simple. Vous pouvez simplement écrire votre MessageCallback avec une fonction .NET, c'est à dire une méthode qui retourne (<em>return</em>) une valeur :
<pre class="lang:c# decode:true">[MessageCallback(Key="Logon")]
bool MyMethodWithComplexParameter(UserInfo user)
{
    PackageHost.WriteInfo("User: {0} &amp; Password = {1}", user.User, user.Password);

    return user.User == user.Password;
}</pre>
La propriété “ResponseType” du MessageCallback est automatiquement définie avec le type de retour de la méthode. C’est l’API .NET qui se chargera d’envoyer la réponse si le message est reçu est une saga. La réponse renvoyée étant l’objet retourné par votre méthode, dans l’exemple ci-dessus un booléen.

Ainsi en republiant le package avec le code ci-dessus, vous constaterez dans le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallbacks Explorer</a> que votre MessageCallback indique son type de réponse dans le cas d'une saga :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-121.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback avec saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-98.png" alt="MessageCallback avec saga" width="424" height="209" border="0" /></a></p>
<p align="left">Vous pourrez alors tester votre MessageCallback :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-122.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Envoyer un message dans une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-99.png" alt="Envoyer un message dans une saga" width="424" height="244" border="0" /></a></p>
<p align="left">Et visualisez le message de retour, ici un booléen :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-123.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Réponse d'une saga" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-100.png" alt="Réponse d'une saga" width="424" height="438" border="0" /></a></p>

<h3>Décrire ses MessageCallbacks</h3>
Pour finir il est vivement conseillé de décrire ses MessageCallbacks et les types utilisés en paramètre ou en réponse.

Vous avez deux possibilités :
<ul>
 	<li>Utilisez la propriété “Description” sur l’attribut MessageCallback (mais limité aux MC et non aux types)</li>
 	<li>Utilisez les commentaires de documentation XML</li>
</ul>
Pour gagner en productivité, je vous recommande l’extension Visual Studio <a href="http://submain.com/products/ghostdoc.aspx">GhostDoc de SubMain</a> pour générer automatiquement des commentaires de documentation XML dans votre code .NET.

Ainsi ajoutons des commentaires de documentation XML sur nos MessageCallbacks et sur la classe UserInfo utilisée dans nos exemples comme paramètre de notre MC :
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
Si vous redéployez le package, vous observerez que l'ensemble des MessageCallbacks et des paramètres sont décrits par vos commentaires XML :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-124.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallback documenté" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-101.png" alt="MessageCallback documenté" width="424" height="128" border="0" /></a></p>
<p align="left"></p>

<h3>S’abonner et recevoir les messages d’un groupe</h3>
Comme <a href="/concepts/messaging-message-scope-messagecallback-saga/">vous le savez</a>, il existe plusieurs type de scope pour recevoir un message. Par défaut votre package recevra les messages adressés au scope “All” et “Other” (si il n’est pas l’émetteur du message) mais aussi et surtout ceux destinés au scope de sa sentinelle et/ou de son package.

Il y a un dernier type de scope fort utile : “les groupes”. Pour recevoir des messages destinés à un groupe, il faut d'abord s’abonner au groupe.

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
<p style="text-align: left;" align="center">Vous pouvez aussi utiliser<a href="/constellation-platform/constellation-console/gerer-packages-avec-la-console-constellation/#Editer_les_groupes_dun_package"> la Console Constellation pour éditer les groupes</a> de chaque package.</p>
<p align="left">Autre manière, directement dans votre code en utilisant la méthode “<em>PackageHost.SubscribeMessages</em>” (ou <em>PackageHost.UnsubscribeMessages</em>).</p>
<p align="left">Par exemple :</p>

<pre class="lang:c# decode:true crayon-selected">PackageHost.SubscribeMessages("MonGroupDemo");</pre>