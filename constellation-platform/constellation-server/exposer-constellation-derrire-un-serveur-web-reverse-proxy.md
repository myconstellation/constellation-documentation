---
ID: 2183
post_title: >
  Exposer Constellation en HTTPS derrière
  un reverse proxy avec IIS et Let’s
  Encrypt
author: Sebastien Warin
post_date: 2016-08-09 15:17:31
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/
published: true
publish_post_category:
  - "19"
publish_to_discourse:
  - "0"
update_discourse_topic:
  - "1"
discourse_post_id:
  - "1424"
discourse_topic_id:
  - "942"
discourse_permalink:
  - >
    https://forum.myconstellation.io/t/reverse-proxy-exposer-constellation-derriere-un-serveur-web/942
post_modified: 2018-04-25 14:58:09
---
Pour assurer un maximum de sécurité et ajouter des fonctionnalités (SSL, authentification basic/windows/ssl, filtrage, ou autre) vous  pouvez exposer votre serveur Constellation derrière un serveur proxy (reverse-proxy) tel que IIS, Apache, <a href="/constellation-platform/constellation-server/exposer-constellation-en-https-derriere-un-reverse-proxy-avec-nginx-et-lets-encrypt/">nginx</a> ou autre.

Nous allons voir ici comment configurer un serveur Microsoft IIS en tant que reverse proxy pour Constellation. Vous pouvez également réaliser <a href="/constellation-platform/constellation-server/exposer-constellation-en-https-derriere-un-reverse-proxy-avec-nginx-et-lets-encrypt/">le reverse proxy avec nginx</a> notamment si vous êtes sur un système Linux.
<h3>Etape 1 : installer ARR et URL Rewrite</h3>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="IIS" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-2.png" alt="IIS" width="354" height="192" border="0" /></a></p>
Pour commencer, en utilisant le “Web Platform Installer”, installez :
<ul>
 	<li>Application Request Routing 3.0</li>
 	<li>URL Rewrite 2.0</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Installation des modules IIS" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-3.png" alt="Installation des modules IIS" width="354" height="243" border="0" /></a></p>

<h3 align="left">Etape 2 : créer un site Web IIS</h3>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/08/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="IIS Web Site" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/image_thumb-4.png" alt="IIS Web Site" width="354" height="192" border="0" /></a></p>
<p align="left">Toujours depuis la Console IIS, créez un site web IIS. Configurez le binding et les autres options de votre choix (port d’écoute, SSL ou non, authentification, restriction d’IP, filtrage, etc..).</p>
C’est ce site Web IIS qui devra être exposé sur Internet et qui sera le point d’entrée vers votre Constellation.
<h3>Etape 3 : configurer la règle de reverse proxy</h3>
Créez le fichier “web.config” dans le répertoire de votre site Web créé à l’étape précédente avec le contenu suivant :
<pre class="lang:xml decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;configuration&gt;
    &lt;system.webServer&gt;
        &lt;rewrite&gt;
            &lt;rules&gt;
                &lt;clear /&gt;
                &lt;rule name="Constellation on my private server" stopProcessing="true"&gt;
                    &lt;match url="^constellation/(.*)" /&gt;
                    &lt;conditions logicalGrouping="MatchAll" trackAllCaptures="false" /&gt;
                    &lt;action type="Rewrite" url="http://myprivateserver.mynetwork.lan:8088/{R:1}" /&gt;
                &lt;/rule&gt;
            &lt;/rules&gt;
        &lt;/rewrite&gt;
    &lt;/system.webServer&gt;
&lt;/configuration&gt;</pre>
Dans cet exemple toutes requêtes qui commence par “constellation/” seront “rewritées” vers le serveur local “myprivateserver.mynetwork.lan:8888”.
<h3>Etape 4 : activer le HTTPS avec des certificats SSL Let's Encrypt</h3>
Téléchargez et installez <a href="https://certifytheweb.com/">Certify SSL Manager</a>. Vous pourrez alors ajouter un certificat SSL en quelques clics à votre site IIS avec gestion automatique du renouvellement.