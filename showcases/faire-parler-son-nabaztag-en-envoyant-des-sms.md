---
ID: 4670
post_title: >
  Faire parler son Nabaztag en envoyant
  des SMS grâce à la Constellation
author: Laurent Ellerbach
post_date: 2015-06-09 11:18:31
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/showcases/faire-parler-son-nabaztag-en-envoyant-des-sms/
published: true
publish_post_category:
  - "11"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:01:43
---
<em>Écrit par Laurent Ellerbach en Juin 2015</em>

J’ai la chance de faire partie des beta-testeurs de la plateforme Constellation développé par Sébastien Warin. J’utilise cette plateforme pour orchestrer des services que je fais tourner sur des serveurs répartis à travers la France et dans Microsoft Azure.
<p align="left">La Constellation fonctionne à la fois sous Windows et Linux me permettant de connecter des RaspberryPi v1 tournant sur lesquels j’ai installé Mono. Un de ces boards me permet de gérer une serre qui se trouve dans le jardin. J’ai aussi des Netduino tournant sous .NET Microframework (implémentation open source d’un sous ensemble de .NET et tournant directement sur des processeurs sans OS). Sur un de ces Netduino, j’ai une carte de prototypage GSM Quectel M95 que j’utilise comme passerelle SMS avec un abonnement free à 2€.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/sms.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Microframework .NET" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/sms_thumb.jpg" alt="Microframework .NET" width="394" height="295" border="0" /></a></p>
La grande difficulté est la gestion de la réception et l’émission de SMS, tout se faisant en port série avec des commandes AT. Beaucoup de « plaisir » à faire fonctionner tout cela. Pour avoir un minimum de sécurité, je gère directement dans le board les numéros de téléphones autorisés ainsi que leur niveau d’accréditation. Après ce premier niveau de sécurité, pour des commandes avancées, un code de sécurité aléatoire est renvoyé, le message doit ensuite être renvoyé. Il est en effet possible de spoofer un SMS facilement, mais la réponse à l’expéditeur arrive toujours au numéro d’envoi. Cela me permet de gérer un très bon niveau de sécurité.

Comme il existe différente API pour se connecter à la Constellation, en .NET, Python, Javascript ou bien .NET Microframework, je peux facilement connecter mon board dans ma Constellation.

La Constellation me permet, au démarrage du board ou à chaque fois que je fais une mise à jour des paramètres sur le serveur de recevoir automatiquement les nouveaux settings. Au niveau du code, rien de plus simple :
<pre class="lang:default decode:true crayon-selected">ConstellationProxy constellation = new ConstellationProxy("http://ipserver:port", "clédesurité", "NOMDUBOARD", "Notif");
constellation.SettingsReceived += constellation_SettingsReceived;
constellation.RequestSettings();

static void constellation_SettingsReceived(System.Collections.Hashtable settings)
{   // Numéros de téléphones
    if (settings.Contains("PhoneNumbers"))
    {
        AuthorisedNumbers = settings["PhoneNumbers"].ToString().Split(';');
    } 
    //Privilèges d'access 
    if (settings.Contains("Access"))
    {
        Access = settings["Access"].ToString().Split(';');
    }
}
</pre>
Constellation fourni une dll délivrée par Nuget permettant d’y avoir accès, une fois connecté et initialisé, il est possible de récupérer les paramètres dont j’ai besoin, dans mon cas, les numéros de téléphones et les privilèges d’accès.

A chaque SMS reçu et validé, j’envoie un message au groupe « SMS » qui a 3 propriétés, From (le numéro de téléphone de l’expéditeur), le Message (le corps du SMS) et le moment où le SMS a été reçu
<pre class="lang:csharp decode:true">constellation.SendMessage(new Scope(ScopeType.Group, "SMS"), "ReceiveSMS", new SMSNotif.SMS { From = mySMS.From, Message = mySMS.Message, Date = mySMS.Date });</pre>
J’ai d’autres packages tournant sur l’autres machines qui peuvent s’abonner à ce groupe et donc recevoir un événement lors de la réception d’un SMS. Leur consommation est également triviale :
<pre class="lang:csharp decode:true">[MessageCallback]
public void ReceiveSMS(dynamic mySMS)
{ 
  // Récupération des propriétés de l'objet
  string From = (string)mySMS.From;
  string Message = (string)mySMS.Message;
}
</pre>
<h3>Quel rapport avec mon Nabaztag ?</h3>
C’est là que les choses deviennent intéressantes. Beaucoup se souviennent certainement des <a href="http://www.nabaztag.com/">Nabaztag</a>, ces lapins super sympas et connectés. Précurseurs des objets connectés, la société qui les a créée n’a malheureusement pas survécu. Mais le code des lapins a été publiés et on a vu des quelques serveurs s’ouvrir pour leur apporter une nouvelle vie.

J’utilise <a href="http://openjabnab.fr/ojn_admin/">openJabNab.fr</a> qui est géré par un groupe de passionnés et offrent un service de qualité proposant des API. Et c’est notamment cet aspect qui m’intéressait particulièrement. En effet, je compte utiliser la seconde vie de mon Nabaztag pour les notifications de ma Constellation.

Pour ce faire, il est nécessaire de paramétrer son Lapin en créant un compte, la manipulation est vraiment simple et rapide. Il faut ensuite activer des plugins. Celui que je vais utiliser est très simple, c’est le “Plugin TTS, envoi de texte au lapin”, son nom court est “tts”. Nous en aurons besoin. En regardant rapidement la documentation, la fonction est “say” et prends le paramètre “text” en entrée.

Le fonctionnement de l’ensemble des API est similaire, il faut d’abord récupérer un token puis l’utiliser dans l’appel de l’API. L’appel à l’URL suivante en remplaçant mylogin et mypassword par le login et mot de passe, retourne ensuite un XML.

<a href="http://openjabnab.fr/ojn_api/accounts/auth?login=mylogin&amp;pass=myPassword">http://openjabnab.fr/ojn_api/accounts/auth?login=mylogin&amp;pass=myPassword</a>

En cas d’erreur:
<pre class="lang:html5 decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
&lt;error&gt;Access denied&lt;/error&gt;
&lt;/api&gt;
</pre>
En cas de succès:
<pre class="lang:html5 decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
&lt;value&gt;77a18144bb6adefbe29afdb16fb5e3ed&lt;/value&gt;
&lt;/api&gt;</pre>
Le token est la valeur value. L’appel de l’API text to speech se fait ensuite de la façon suivante:

<a href="http://openjabnab.fr/ojn_api/bunny/0123456789ab/tts/say?text=je+suis+une+lapine+qui+aime+les+lapins&amp;token=77a18144bb6adefbe29afdb16fb5e3ed">http://openjabnab.fr/ojn_api/bunny/0123456789ab/tts/say?text=je+suis+une+lapine+qui+aime+les+lapins&amp;token=77a18144bb6adefbe29afdb16fb5e3ed</a>

L’adresse MAC du lapin doit être positionnée à la place de la chaîne 0123456789ab. Le texte qui sera lu sur le Nabaztag sera donc “je suis une lapine qui aime les lapins”. Vous recevez également une confirmation de ce type:
<pre class="lang:default decode:true crayon-selected">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
&lt;ok&gt;Envoi de 'je suis une lapine qui aime les lapins' au lapin '0123456789ab'&lt;/ok&gt;
&lt;/api&gt;</pre>
Pour créer un Package, il suffit de créer un projet Constellation après l’installation du SDK Constellation pour Visual Studio.

Nous allons ensuite ajouter 4 settings, pour le login et mots de passe ainsi que pour les lapins. Je possède plusieurs Lapin et plutôt que de les appeler par leur adresse MAC, j’utilise un nom, je vais donc utiliser de façon vraiment simple un string contenant les noms séparés par des point-virgules et faire la même chose pour les adresses MAC. Côté des variables, lors de la lecture des settings, je stocke les informations dans des string et tableau de string:
<pre class="lang:csharp decode:true">private string myLogin;
private string myPassword;
private string[] MACaddresses;
private string[] FriendlyNames;

private void UpdateSettings()
{
    myLogin = PackageHost.GetSettingValue&lt;string&gt;("Login");
    myPassword = PackageHost.GetSettingValue&lt;string&gt;("Password");
    MACaddresses = PackageHost.GetSettingValue&lt;string&gt;("MACs").Split(';');
    FriendlyNames = PackageHost.GetSettingValue&lt;string&gt;("Friendlys").Split(';');
    PackageHost.WriteInfo("Package NabaztagNotif updated");
}</pre>
Ensuite, je crée ma fonction de call back que j’appellerais depuis un autre package quand je voudrais envoyer un texte à mon lapin:
<pre class="lang:csharp decode:true">[MessageCallback]
public void Say(string MACaddress, string texttosend) { 
    Try {
        WebClient instanceHTTP = new WebClient();
        string strReq = "http://openjabnab.fr/ojn_api/accounts/auth?login=" + myLogin + "&amp;pass=" + myPassword;
        Uri MyURI = new Uri(strReq);
        string retval = instanceHTTP.DownloadString(MyURI); 
        // Recherche du mot "value" contenu dans le XML renvoyé
        if (retval.Contains("value")) {
            int start = retval.IndexOf("&lt;value&gt;");
            int end = retval.IndexOf("&lt;/value&gt;");
            string token = retval.Substring(start + 7, end - start - 7);
            strReq = "http://openjabnab.fr/ojn_api/bunny/" + MACaddress + "/tts/say?text=" + WebUtility.HtmlEncode(texttosend) + "&amp;token=" + token;
            MyURI = new Uri(strReq);
            retval = instanceHTTP.DownloadString(MyURI);
            if (retval.Contains("&lt;ok&gt;"))
                PackageHost.WriteInfo("Success sending message to Nabaztag {0}", nabaztagname);
            else
                PackageHost.WriteError("Error sending message to Nabaztag {0}", nabaztagname);  }
        else {
            PackageHost.WriteError("Error sending message to Nabaztag {0}, error: authentication", nabaztagname);  } }
    catch (Exception ex) {
        PackageHost.WriteError("Error sending message to Nabaztag {0}, error: {1}", nabaztagname, ex.ToString());  }  }
</pre>
Il considère que le login et le mot de passe ont déjà été encodé pour l’URL. Il est nécessaire d’utiliser la fonction WebUtility.HtmlEncode s’ils ne le sont pas dans vos settings.

L’appel se fait de façon tout à fait classique comme cela (en considérant que le nom du package est “NabaztagNotif”):
<pre class="lang:csharp decode:true">PackageHost.CreateScope("NabaztagNotif").Proxy.Say("monlapin", “je suis un lapin qui aime les lapines”);</pre>
Comme notre package est connecté à la Constellation, une simple page Javascript ou un script Python ou Powershell peuvent également envoyer des messages à notre Nabaztag.

Pour ma part, depuis mon board et l’API Constellation pour NETMF, à la réception de SMS, j’envoie un message au package NabaztagNotif me permettant de lire les SMS reçus.

Voilà un exemple d’utilisation de Constellation mais aussi d’API permettant de faire revivre vaux lapins.

Bon lapinage :-)

<b><i>Laurent Ellerbach, </i></b><i>Evangelist Lead, Central and Eastern Europe, Microsoft</i>