---
ID: 2161
post_title: >
  Configuration du serveur Constellation
  en SSL
author: Sebastien Warin
post_date: 2016-08-09 13:56:36
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/configuration-ssl/
published: true
publish_post_category:
  - "19"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 10:15:12
---
Afin de sécuriser votre Constellation, il est recommandé d’utiliser un cryptage SSL pour les communications avec votre serveur Constellation.

Vous pouvez soit <a href="/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/">exposer votre serveur Constellation derrière un reverse-proxy</a> (Apache, IIS, ngnix, ou autre) sur lequel vous activerez le cryptage SSL ou soit exposer Constellation directement en SSL.

Pour cette dernière option voici la procédure :

<h3>Etape 1 : créer un certificat SSL</h3>

La première étape consiste à créer un certificat SSL portant le nom de l’URI de votre Constellation (par exemple <em>constellation.mondomaine.com</em>).

Plusieurs options :

<ul>
    <li>Créer un certificat auto-signé</li>
    <li>Créer un certificat privé avec votre propre autorité de certification</li>
    <li>Créer/acheter un certificat à une autorité de certification reconnue</li>
</ul>

Dans le premier cas, il faudra installer ce certificat sur chaque poste client sous peine d’avoir des avertissements de sécurité.

Vous pouvez créer des certificats auto-signés depuis la <a href="https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html">console IIS</a>, <a href="http://windowsitpro.com/blog/creating-self-signed-certificates-powershell">Powershell</a> ou en ligne de commande avec l’utilitaire “<a href="https://www.jayway.com/2014/09/03/creating-self-signed-certificates-with-makecert-exe-for-development/">makecert</a>” sur Windows par exemple. Il existe également <a href="http://www.selfsignedcertificate.com/">des générateurs de certificats auto-signés en ligne</a>.

Si vous avez un domaine Active Directory avec une autorité de certification (ADCS) vous pouvez générer des certificats SSL depuis la console IIS (<a href="https://www.youtube.com/watch?v=DLH7G_3XD2w">voir ici</a>). Il faudra que chaque client de votre Constellation ait le certificat publique de votre autorité de certification (AC) pour fonctionner.

Autrement pour éviter ce type de prérequis, vous devez générer un certificat SSL signé par une véritable autorité de certification tel que GlobalSign, GeoTrust, Gandi, etc. Chez <a href="https://www.startssl.com/">StartCom</a> ou <a href="https://www.wosign.com/english/freessl.htm">WoSign</a>, les certificats SSL sont gratuits. Chez <a href="https://www.gandi.net/ssl">Gandi</a>, vous avez la première année offerte pour chaque domaine que vous commandez chez eux.

<h3>Etape 2 : installer le certificat</h3>

Sous Windows, vous devez ouvrir la MMC de gestion des certificats pour installer le certificat créé à l’étape 1.

Vous pouvez par exemple suivre ces guides :

<ul>
    <li><a title="https://fr.godaddy.com/help/iis-8-install-a-certificate-4951" href="https://fr.godaddy.com/help/iis-8-install-a-certificate-4951">https://fr.godaddy.com/help/iis-8-install-a-certificate-4951</a></li>
    <li><a title="https://www.startssl.com/Support?v=31" href="https://www.startssl.com/Support?v=31">https://www.startssl.com/Support?v=31</a> (étape 9 à 12).</li>
</ul>

<h3>Etape 3 : lier le certificat à un port TCP</h3>

Lorsque vous installer votre certificat dans la MMC lors de l’étape précédente, copiez l’empreinte (thumbprint) visible dans les propriétés de votre certificat.

Puis dans une invite de commande, entrez la commande suivante :

<pre class="lang:bash">netsh http add sslcert ipport=0.0.0.0:XXX appid={12345678-db90-4b66-8b01-88f7af2e36bf} certhash=yyy</pre>

Vous devez remplacer “XXX” par le port TCP que vous souhaitez utiliser pour exposer Constellation en SSL et “YYY” par le thumbprint de votre certificat SSL.

<h3>Etape 4 : configurer Constellation</h3>

Pour finir éditez le fichier de configuration Constellation (<em>Constellation.Server.exe.config</em>) et ajoutez une “listenUri” sur le port défini dans l’étape précédente.

Par exemple, configurons Constellation pour écouter en HTTP sur le port par défaut 8088 et en HTTPS (SSL) sur le port 8089 :

<pre class="lang:xml decode:true ">&lt;listenUris&gt;
  &lt;uri listenUri="http://+:8088/" /&gt;
  &lt;uri listenUri="https://+:8089/" /&gt;
&lt;/listenUris&gt;
</pre>