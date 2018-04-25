---
ID: 5890
post_title: 'Exposer Constellation en HTTPS derri&egrave;re un reverse proxy avec Nginx et Let&rsquo;s Encrypt'
author: Sebastien Warin
post_date: 2018-04-25 10:11:01
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/exposer-constellation-en-https-derrire-un-reverse-proxy-avec-nginx-et-lets-encrypt/
published: true
wpdc_xmlrpc_failure_sent:
  - "1"
post_modified: 2018-04-25 10:11:01
---
<p>Pour sécuriser votre Constellation vous devez exposer votre serveur sur le protocole HTTPS afin de chiffrer toutes les communications en SSL.</p> <p>Si vous êtes sur Windows vous pouvez consulter ces deux articles :</p> <ul> <li><a href="/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/">Reverse Proxy : exposer Constellation derrière IIS</a></li> <li><a href="/constellation-platform/constellation-server/configuration-ssl/">Configuration du serveur Constellation en SSL</a></li></ul> <p>Dans cet article nous allons voir comment réaliser cela sur Linux/Unix.</p> <p>Pour ce faire nous allons exposer Constellation derrière un serveur Nginx protéger avec un certificat SSL Let’s Encrypt.</p> <h3>Prérequis</h3> <p>Avant de démarrer vous devez avoir installer le serveur Constellation sur votre serveur Linux, typiquement un Debian ou Ubuntu.</p> <p>Cela se résume à lancer la commande suivante :</p><pre title="WPI Pour Linux" class="lang:shell decode:true">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
<p>Pour plus d’information, veuillez suivre le guide suivant : </p>
<p>Installer Constellation sur Linux</p>