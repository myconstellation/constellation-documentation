---
ID: 3368
post_title: 'NetworkTools : surveillez et testez vos ressources r&eacute;seau'
author: Sebastien Warin
post_date: 2016-10-21 16:55:01
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/networktools/
published: true
post_modified: 2016-10-25 13:32:02
---
Le package NetworkTools permet de monitorer des ressources réseau et offre différents outils pour faire des Pings, tests d’ouverture de port TCP, tests de site Web (HTTP), scanner de port, Wake-On-Lan ou encore résolution DNS.

Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/NetworkTools">https://github.com/myconstellation/constellation-packages/tree/master/NetworkTools</a>
<h3>Installation</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package NetworkTools :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-97.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-89.png" alt="image" width="350" height="216" border="0" /></a></p>
Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.

Pour finir, sur la page de Settings, vous pouvez définir les ressources à monitorer :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-98.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-90.png" alt="image" width="350" height="286" border="0" /></a></p>
<p align="left">Si vous n’avez rien à monitorer vous pouvez supprimer (bouton “Remove”) le setting ou encore effacer le JSON. La configuration du monitoring réseau est expliquée ci-dessous.</p>
<p align="left">Bien entendu vos  pouvez également déployer ce package manuellement dans la configuration de votre Constellation. Par exemple sans aucun monitoring :</p>

<pre class="lang:html5 decode:true">&lt;package name="NetworkTools" / &gt;</pre>
<h3>Configurer le monitoring</h3>
Ce package expose différentes fonctions (MessageCallbacks) dans votre Constellation pour tester des ressources réseau (Site Web HTTP, port TCP, Ping, résolution DNS, scanner de port, etc..). Il peut aussi monitorer à intervalle régulier des ressources réseau et publier le résultat dans les StateObjects Constellation.

Pour cela le paramètre  “Monitoring” est un tableau JSON qui contient la liste des ressources à monitoring.

Il y a trois type de monitoring :
<ul>
 	<li><u>Ping</u> : effectue un ping (IMCP Echo) vers une adresse IP ou DNS</li>
 	<li><u>Tcp</u> : effectue un test d’ouverture de socket que un port TCP</li>
 	<li><u>Http</u> : effectue un appel HTTP et optionnellement une vérification de la réponse</li>
</ul>
Chaque ressource à monitorer est identifiée par un nom qui sera utilisé comme nom de StateObject pour la publication du résultat.

De plus chaque ressource peut spécifier son intervalle de temps entre deux test (par défaut fixé à 60 secondes).

Par exemple pour pinger une machine toutes les minutes :
<pre class="lang:javascript">{ Name: "Ping My Machine", Type: "Ping", Hostname: "myhostname.mydomain.com" }</pre>
Pour pinger la même machine mais toutes les 10 secondes :
<pre class="lang:javascript">{ Name: "Ping My Machine", Type: "Ping", Hostname: "myhostname.mydomain.com", Interval:10 }</pre>
Pour monitorer un service TCP sur le port 80 toutes les 10 secondes :
<pre class="lang:javascript">{ Name: "My Web Server", Type: "TCP", Hostname: "myhostname.mydomain.com", Port: 80, Interval:10 }</pre>
Pour monitorer un site Web HTTP :
<pre class="lang:javascript">{ Name: "Check Sebastien.warin.fr", Type: "Http", Address: "http://sebastien.warin.fr" }</pre>
La même chopse mais en vérifiant le réponse HTTP avec une expression régulière :
<pre class="lang:javascript">{ Name: "Check Sebastien.warin.fr", Type: "Http", Address: "http://sebastien.warin.fr", Regex: "Le blog personnel et technique de Sebastien Warin", Interval: 30 }</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="10"><u>Détail</u></td>
<td valign="top" width="456"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>Monitoring</strong></td>
<td valign="top" width="10">Object JSON</td>
<td valign="top" width="10">Optionnel</td>
<td valign="top" width="456">La liste des ressources réseau à monitoring (Ping, port TCP ou site web)</td>
</tr>
</tbody>
</table>
<h4>Les StateObjects</h4>
Vous retrouverez autant de StateObjects que vous avez de ressources monitorées.

Par exemple
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; resource monitorée &gt;&gt;</strong></td>
<td valign="top" width="10">NetworkTools.MonitoringResult</td>
<td valign="top" width="446">Le StateObject représente le dernier état connu de la ressource (état et temps de réponse)</td>
</tr>
</tbody>
</table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-99.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-91.png" alt="image" width="350" height="120" border="0" /></a></p>

<h4 align="left">Les MessageCallbacks</h4>
Le package expose 6 MessageCallbacks :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>CheckHttp</strong></td>
<td valign="top" width="141">Int64</td>
<td valign="top" width="407">Test une URL HTTP et retourne le temps de réponse en seconde (ou –1 si pas de réponse).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>CheckPort</strong></td>
<td valign="top" width="141">Int64</td>
<td valign="top" width="407">Test un port TCP et retourne le temps de réponse en seconde (ou –1 si pas de réponse).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>DnsLookup</strong></td>
<td valign="top" width="141">String[]</td>
<td valign="top" width="407">Résolution DNS d’une adresse et retourne la liste des adresses IP</td>
</tr>
<tr>
<td valign="top" width="10"><strong>Ping</strong></td>
<td valign="top" width="141">Int64</td>
<td valign="top" width="407">Ping une adresse IP ou DNS et retourne le temps de réponse en seconde (ou –1 si pas de réponse).</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ScanPort</strong></td>
<td valign="top" width="141">Dictionary&lt;int, bool&gt;</td>
<td valign="top" width="407">Scanne les ports TCP d’une IP ou DNS et retourne un dictionnaire des ports TCP testés associés à leurs états (ouvert ou fermé)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>WakeUp</strong></td>
<td valign="top" width="141">Boolean</td>
<td valign="top" width="407">Envoi un paquet “Wake On Lan” à une adresse MAC</td>
</tr>
</tbody>
</table>
<h3 align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-100.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-92.png" alt="image" width="350" height="494" border="0" /></a></h3>
<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Envoyer une notification si une ressource réseau ne répond plus</li>
 	<li>Surveiller ses ressources réseau (serveurs, services ou sites web) avec une bande de LED WS2801 et un Arduino/ESP</li>
</ul>