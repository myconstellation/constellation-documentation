---
ID: 5890
post_title: 'Exposer Constellation en HTTPS derrière un reverse proxy avec Nginx et Let&rsquo;s Encrypt'
author: Sebastien Warin
post_date: 2018-04-25 10:11:01
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/exposer-constellation-en-https-derriere-un-reverse-proxy-avec-nginx-et-lets-encrypt/
published: true
wpdc_xmlrpc_failure_sent:
  - "1"
publish_post_category:
  - "19"
publish_to_discourse:
  - "0"
update_discourse_topic:
  - "1"
discourse_post_id:
  - "1629"
discourse_topic_id:
  - "996"
discourse_permalink:
  - >
    https://forum.myconstellation.io/t/exposer-constellation-en-https-derriere-un-reverse-proxy-avec-nginx-et-lets-encrypt/996
post_modified: 2018-04-25 14:45:57
---
Pour sécuriser votre Constellation vous devez utilise le protocole HTTPS afin de chiffrer toutes les communications en SSL.

<img class="alignnone size-full wp-image-3796 aligncenter" src="https://developer.myconstellation.io/wp-content/uploads/2016/08/ssl.jpg" alt="" width="253" height="189" />

Si vous avez un serveur Windows, vous pouvez également <a href="/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/">utiliser IIS pour configurer un reverse proxy</a> vers Constellation.

Dans cet article nous allons découvrir comment exposer le serveur Constellation derrière un serveur Nginx protégé avec un certificat SSL Let’s Encrypt.
<h3>Prérequis : avoir une Constellation exposée publiquement avec un nom DNS</h3>
Avant de démarrer vous devez avoir un serveur Constellation opérationel.

Si vous installez Constellation sur un Linux, cela se résume à lancer la commande ci-dessous et à suivre l'assistant :
<pre title="WPI Pour Linux" class="lang:shell decode:true">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
Pour plus d’information, veuillez suivre le guide : <a href="/constellation-platform/constellation-server/installer-constellation-sur-linux/">Installer Constellation sur Linux.</a>

On considéra à ce stade que votre serveur Constellation est démarré et opérationnel. Vous pouvez lancer la commande suivante pour vérifier le statut des services Constellation :
<pre class="lang:default decode:true" title="Statut des services">sudo supervisorctl status</pre>
Vous devriez voir le “<em>constellation-server</em>” avec le statut “RUNNING” (et la sentinelle si déployée) :
<pre class="lang:default decode:true" title="Statut des services">sebastien@ubuntu:~$ sudo supervisorctl status
constellation-sentinel           RUNNING   pid 1194, uptime 1 day, 17:36:48
constellation-server             RUNNING   pid 2888, uptime 1 day, 15:06:41</pre>
Vous pouvez également installer Constellation sur un système Windows (voir <a href="/getting-started/installer-constellation/">le guide</a>).

En effet, le reverse proxy (ici Ngnix) qui sera installé sur un système Linux peut exposer un service tel que Constellation quelque soit le serveur sur lequel il est déployé. Le reverse proxy n'est pas nécessairement sur la même machine sur le ou les services à exposer.

Il est donc possible d'installer un serveur Linux avec Nginx pour exposer en SSL un serveur Constellation sur un système Windows de la même manière que nous pouvons installer un serveur <a href="/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/">Windows avec IIS</a> pour exposer en SSL un serveur Constellation sur un système Linux !

Dans ce guide nous avons installer le serveur Constellation et le reverse proxy Nginx sur le même serveur, sous Linux Ubuntu 16.

Si vous souhaitez activer le HTTPS et donc ajouter un certificat SSL, vous devez nécessairement avoir un nom DNS qui pointe vers l’adresse IP (public) de votre serveur de reverse proxy.

Plusieurs options s'offre à vous :
<ul>
 	<li>Si vous avez un nom de domaine, modifiez votre zone DNS pour ajouter un “host” (enregistrement A ou AAAA) vers l’adresse IP (public) de votre Constellation.</li>
 	<li>SI vous n’avez pas de nom de domaine :
<ul>
 	<li>Achetez-en un ! Comptez environ 15€/an pour les extensions standards (.fr, .net, .com), 2,99€ HT par an pour  un <a href="https://www.ovh.com/fr/domaines/" target="_blank" rel="noopener">.ovh</a></li>
 	<li>Utilisez un service de “Dynamic DNS” comme dyndns.fr, dyn.com ou autre</li>
 	<li>Certains FAI comme Free proposent d'attacher un nom DNS type xxxx.hd.free.fr à votre IP de connexion</li>
 	<li>Certains NAS comme Synology permettent aussi de créer un DDNS type xxx.synology.me vers votre IP de connexion</li>
</ul>
</li>
</ul>
Dans le cas présent, j’ai crée l’entrée DNS “demo.internal.myconstellation.io” qui pointe vers l’adresse IP public du serveur Constellation installé sur un Ubuntu (le DNS doit pointer votre le serveur où sera installé le reverse proxy dans la mesure où tout passera par lui. Etant donné que le reverse proxy sera sur le même serveur que le service Constellation on peut dire que le DNS pointe vers le serveur Constellation).

Bien évidement, si votre serveur est installé dernière un routeur avec du NAT (typiquement sur un réseau local derrière une box Internet) vous devez configurer la redirection de port sur votre routeur/box internet.

Encore une fois, on part du principe que le serveur de R.P et Constellaiton sont sur le même serveur, donc la même IP interne.

Dans un premier temps redirigez seulement le port 8088 en tcp sur l’IP interne de votre serveur Constellation. A noter que nous supprimerons cette redirection une fois le reverse proxy installé.

Donc pour résumer et avant de démarrer, vous devez avoir votre serveur Constellation démarré répondant sur l’URL : <a href="http://&lt;mon_nom_dns&gt;:8088">http://&lt;mon_nom_dns&gt;:8088</a>

Dans mon cas : <a href="http://demo.internal.myconstellation.io:8088">http://demo.internal.myconstellation.io:8088</a> :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Serveur Constellation sur un nom DNS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb.png" alt="Serveur Constellation sur un nom DNS" width="484" height="291" border="0" /></a></p>
Voilà vous êtes prêt, votre Constellation est démarrée et elle répond bien sur un nom DNS public depuis l'internet.
<h3>Exposer Constellation derrière Nginx</h3>
Avant de démarrer il est recommandé de mettre à jour le référentiel du gestionnaire de package par la commande suivante :
<pre title="Update APT" class="lang:shell decode:true">sudo apt-get update
</pre>
Ensuite installez le serveur Web Nginx avec la commande :
<pre title="Installer NGINX" class="lang:shell decode:true">sudo apt-get install nginx
</pre>
Les fichiers de configuration sont situés dans le répertoire “<em>/etc/nginx/sites-available/</em>” et pour les activer on crée un lien symbolique vers ces fichiers dans le répertoire “<em>/etc/nginx/sites-enabled</em>”.

Pour commencer désactivez la configuration par défaut par la commande ci-dessous :
<pre title="Supression de la configuration par défaut" class="lang:shell decode:true">sudo rm /etc/nginx/sites-enabled/default</pre>
Comme vous l’aurez compris le contenu de cette configuration par défaut est toujours présente dans le répertoire “<em>/etc/nginx/sites-available/</em>” mais non activée car on a supprimé le lien symbolique vers ce fichier dans le répertoire “<em>site-enabled</em>”.

Maintenant nous allons créer une configuration pour notre reverse proxy avec la commande :
<pre title="Création de la configuration Constellation" class="lang:shell decode:true">sudo nano /etc/nginx/sites-available/constellation</pre>
Dans ce fichier, copiez le contenu suivant :
<pre title="Configuration du proxy Constellation" class="lang:shell decode:true">server {
    listen 80; listen [::]:80;
    server_name demo.internal.myconstellation.io;

    location / {
        proxy_pass http://localhost:8088;

        proxy_set_header Host $host;
        proxy_set_header Connection "";
        proxy_http_version 1.1;

        proxy_buffering off;
        proxy_cache off;
        proxy_connect_timeout 30;
        proxy_send_timeout 30;
    }
}</pre>
Vous devez modifier le paramètre “<em>server_name</em>” avec le nom DNS de votre serveur Constellation, dans le cas présent “demo.internal.myconstellation.io”.

Il est important de bien reprendre les mêmes options, notamment le “<em>proxy_buffering</em>” pour permettre les “<em>server-Sent events</em>” vers le serveur Constellation.

Dans le cas présent Nginx va "proxifer" les requêtes vers <em>http://localhost:8088</em>, c'est à dire au service Constellation (port 8088) installé sur le même serveur (localhost).

Tapez ensuite sur la combinaison de touches “Ctrl+X” pour quitter en prenant suivant d’enregistrer le fichier. Puis, pour activer cette configuration, créez le lien symbolique suivant :
<pre title="Activation de la configuration" class="lang:shell decode:true">sudo ln -s /etc/nginx/sites-available/constellation  /etc/nginx/sites-enabled/constellation</pre>
Et pour finir recharger Nginx pour prendre en compte notre configuration fraîchement créée :
<pre title="Recharger NGINX" class="lang:shell decode:true">sudo systemctl reload nginx</pre>
Votre Constellation est maintenant accessible derrière Nginx sur le port 80.

Rendez-vous donc sur l’adresse http://&lt;votre_nom_dns&gt; (sans spécifier le port, car 80 par défaut) pour vérifier que le reverse proxy est opérationnel. Dans le cas présent <a href="http://demo.internal.myconstellation.io">http://demo.internal.myconstellation.io</a>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-1.png" alt="image" width="484" height="290" border="0" /></a></p>
<p align="left">Ce n’est donc plus Constellation qui répond mais notre serveur Nginx qui lui même communique avec le serveur Constellation local.</p>
<p align="left">Donc pour résumer, sur le port 80 c'est le serveur Nginx qui répond en transférant au serveur Constellation sur le port 8088.</p>
<span style="text-decoration: underline;">Note</span> : si vous êtes dernière un routeur avec du NAT, n’oubliez pas d’ajouter la redirection du port 80 vers votre serveur Constellation interne. Par la même occasion vous pouvez supprimer la redirection du port 8088, ainsi tout passera nécessairement par Nginx.
<h3>Activer le HTTPS avec des certificats SSL Let’s Encrypt</h3>
Maintenant que votre service Constellation est exposé dernière le serveur Nginx vous allez pouvoir configurer différentes choses sur Nginx comme par exemple des restrictions d'accès , l’authentification, des limites (throttling) et bien d’autre chose. Dans le cas présent on va s’intéresser au support du SSL.

Pour activer le HTTPS, il vous faut un certificat SSL. Pour simplifier la démarche, Let’s Encrypt est une autorité de certification lancée en 2015 qui permet d’automatiser la génération de certificat. De plus le service est gratuit ! Les certificats sont valides trois mois, il faudra donc régulièrement les renouveler mais vous allez voir que cette action est entièrement automatisable.

Ici nous allons utiliser <a href="https://certbot.eff.org/" target="_blank" rel="noopener">Certbot</a>, un agent installé sur le serveur qui permet de générer et renouveler automatiquement les certificats SSL Let’s Encrypt.

Commencez par ajouter le repository suivant :
<pre class="lang:default decode:true" title="Installer le repository pour Certbot">sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update</pre>
Ensuite installez Certbot pour Nginx :
<pre title="Installer Certbot pour Nginx" class="lang:shell decode:true">sudo apt-get install python-certbot-nginx</pre>
Une fois installé, lancez la commande “<em>certbot –nginx</em>” en spécifiant le nom DNS vers votre serveur. Dans le cas présent :
<pre title="Générer le certificat" class="lang:shell decode:true">sudo certbot --nginx -d demo.internal.myconstellation.io
</pre>
L’assistant vous demandera tout d’abord votre adresse mail :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-2.png" alt="image" width="484" height="100" border="0" /></a></p>
Après avoir accepté les conditions du service, l’assistant va automatiquement valider un challenge en interrogeant votre site Web (d’où la nécessité d’avoir exposé correctement votre serveur sur Internet avec le nom DNS spécifié), créer le certificat SSL et l'ajouter dans votre configuration Nginx.

Pour finir l’assistant vous proposera de modifier automatiquement la configuration de votre Nginx pour rediriger tous les appels HTTP (port 80) vers HTTPS (port 443).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Activation de la redirection HTTPS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-3.png" alt="Activation de la redirection HTTPS" width="484" height="438" border="0" /></a></p>
Choisissiez l’option 2 pour activer cette redirection.

Et voilà, votre serveur Nginx est configuré avec le certificat SSL généré par Let’s Encrypt et toutes les requêtes seront redirigés en HTTPS et donc chiffrés !
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-4.png" alt="image" width="484" height="108" border="0" /></a></p>
<p align="left"><strong>Votre Constellation est maintenant protégée !</strong></p>
<p align="left">Rechargez la page dans votre navigateur (F5) sur l’URL <em>http://&lt;mon_nom_dns&gt;</em> vous constaterez la redirection automatique en <em>https://</em> et le cadenas vert vous informant du chiffrement SSL :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Constellation en HTTPS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-5.png" alt="Constellation en HTTPS" width="484" height="290" border="0" /></a></p>
<p align="left">(Note : pour les personnes derrières un NAT, n’oubliez pas non plus d'ajouter la redirection du port 443 vers votre serveur Nginx interne)</p>
<p align="left">Pour vérifier le bon renouvellement du certificat vous pouvez utiliser la commande suivante :</p>

<pre title="Tester le renouvellement du certificat" class="lang:shell decode:true">sudo certbot renew --dry-run</pre>
<p align="left">Certbot a automatiquement installé un trigger dans systemd (ou crontab selon le système) pour réaliser le renouvellement et déploiement automatique de vos certificats avant leurs expirations.</p>
<p align="left">Pour les plus curieux, vous pouvez jeter un œil dans le fichier “<em>/etc/nginx/sites-available/constellation</em>” pour voir les modifications apportées par Certbot.</p>

<h3>Pour aller plus loin</h3>
<h4>Sécuriser le serveur avec un firewall UFW et fail2ban</h4>
<h5>Installer fail2ban</h5>
<a href="http://fail2ban.sourceforge.net/">Fail2ban</a> est un script tournant en tâche de fond qui va vérifier si les tentatives d'authentification SSH et Nginx; et en cas d'attaque, bannir l'IP grâce à <code>iptables</code>.
<pre class="lang:default decode:true" title="Installer fail2ban">sudo apt-get install fail2ban</pre>
Pour plus d’info:  <a title="https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04" href="https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04">https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04</a>
<h5>Installer UFW</h5>
UFW est un firewall applicatif. Pour l’installer :
<pre title="Installer UFW" class="lang:shell decode:true">sudo apt-get install ufw</pre>
Autorisez ensuite les services HTTP (nécessaire pour le challenge Let’s Encrypt), HTTPS (notre reverse proxy Nginx vers Constellation) et SSH :
<pre title="Configurez UFW" class="lang:shell decode:true">sudo ufw allow http
sudo ufw allow https
sudo ufw allow ssh
</pre>
Pour l’activer :
<pre title="Activer UFW" class="lang:shell decode:true">sudo ufw enable</pre>
Votre serveur Linux ne répondra plus que sur les ports 22 (ssh), 80 (http), 443 (https) en IPv4 et IPv6.
<pre class="lang:sh decode:true" title="Status du firewall">sebastien@ubuntu:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
80                         ALLOW       Anywhere
443                        ALLOW       Anywhere
22                         ALLOW       Anywhere
80 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
22 (v6)                    ALLOW       Anywhere (v6)

</pre>
Vous noterez que le port 8088 n'est maintenant plus accessible car bloqué par le firewall UFW. Il faudra forcement se connecter à Constellation via Nginx en HTTPS. N’oubliez pas alors de modifier l’URI de votre Constellation sur vos sentinelles existantes pour prendre en compte ce changement.
<h4>Ajouter d’autre application Web dernière votre reverse proxy SSL</h4>
Jusqu'au présent vous avez configuré un nom DNS type “demo.internal.myconstellation.io” qui pointe vers votre serveur Linux sur lequel le service Nginx répond en HTTP (80) et HTTPS (443) avec un certificat Let’s Encrypt pour “proxifier” les requêtes vers le service Constellation accessible localement sur le port 8088.

Vos utilisateurs se connectent donc forcement en HTTPS sur le reverse proxy pour accéder à Constellation !

La règle configurée dans Ngnix “proxifie” toutes les requêtes à partir de la racine "/" vers Constellation :
<pre title="Extrait de la configuration Nginx" class="lang:shell decode:true">location / {
        proxy_pass http://localhost:8088;
....
}
</pre>
Mais il est également possible d’adapter cette règle pour que votre reverse proxy ne soit plus exclusivement un passe-plat pour Constellation mais aussi pour vos autres services internes (une box domotique, un serveur Web, un objet connecté, une camera IP, etc…).

Vous aurez ainsi la possibilité d’exposer différents services Web internes dernière votre reverse proxy profitant d’un même port TCP (le 443) et donc d’un même canal de communication sécurisé (par le cryptage SSL).

Parmi les différentes options pour ce type de configuration :
<ol>
 	<li>Reverse proxy en fonction du “server_name”</li>
 	<li>Reverse proxy en fonction de la “location”</li>
</ol>
<h5>Reverse Proxy en fonction du server_name</h5>
Le principe est de rejouer ce tutoriel depuis le début pour chacune de vos applications à exposer.

Vous devrez ainsi créer une adresse DNS vers votre IP public pour chaque site, créer une configuration Nginx en changeant le "<em>server_name</em>" et "<em>proxy_pass</em>" vers la ressource interne à exposer et créer un certificat SSL avec Certbot pour tous vos sites créés.

Vous aurez ainsi une multitude de nom DNS qui pointerons tous vers votre serveur Nginx qui lui redirigera vers les ressources internes en fonction du "server_name". Par exemple “constellation.mondomaine.fr”, “macamera.mondomaine.fr”, “jeedom.mondomaine.fr”, etc.. etc..

La configuration est simple car il s’agit de répéter ce tutoriel pour chaque service, par contre l’administration est un peu lourde et vous aurez autant de sous domaine et donc de certificat que de service à exposer.
<h5>Reverse Proxy en fonction de la “location”</h5>
Dans ce mode, nous gardons un seul site Nginx (celui configuré dans le fichier <em>/etc/nginx/sites-available/constellation</em>) et donc un seul nom de domaine (ici <em>demo.internal.myconstellation.io</em>) avec son certificat associé.

Cependant au lieu de “rediriger” les requêtes depuis la racine "/" vers Constellation nous allons créer plusieurs “location”.

Prenons un exemple pour bien comprendre. Ici le serveur Ngnix est installé sur la même que le serveur Constellation, donc localhost. Imaginons que dans ce même réseau LAN, j’ai par exemple un serveur ZoneMinder qui répond sur 192.168.0.10 et une box Jeedom sur 192.168.0.11.

Je pourrais éditer la configuration Nginx de cette façon :
<pre title="Exemple de configuration multi-sites" class="lang:shell decode:true">location /constellation/ {
        proxy_pass http://localhost:8088/constellation/;

        proxy_set_header Host $host;
        proxy_set_header Connection "";
        proxy_http_version 1.1;

        proxy_buffering off;
        proxy_cache off;
        proxy_connect_timeout 30;
        proxy_send_timeout 30;
    }
    
    
location /zm/ {
        proxy_pass <a href="http://192.168.0.10/zm/;">http://192.168.0.10/zm/;

</a>        proxy_set_header Host $host;
        proxy_set_header Connection "";
        proxy_http_version 1.1;
    }
    
location /jeedom/ {
        proxy_pass http://192.168.0.11;

        proxy_set_header Host $host;
        proxy_set_header Connection "";
        proxy_http_version 1.1;
    }</pre>
Ainsi, en fonction du “path” demandé, un service différent me répondra :
<ul>
 	<li><a href="https://&lt;mon_nom_DNS&gt;/constellation">https://&lt;mon_nom_DNS&gt;/constellation</a> : réponse du serveur Constellation</li>
 	<li><a href="https://&lt;mon_nom_DNS&gt;/zm">https://&lt;mon_nom_DNS&gt;/zm</a>  : réponse du serveur ZoneMinder</li>
 	<li><a href="https://&lt;mon_nom_DNS&gt;/jeedom">https://&lt;mon_nom_DNS&gt;/jeedom</a> : réponse du serveur Jeedom</li>
</ul>
<u>Attention</u> : si le serveur Constellation répond sur un “sous path”, comme ici “/constellation” il faut également modifier la configuration du serveur Constellation pour l’informer.

Pour cela modifiez le ficher “<em>/opt/constellation-server/Constellation.Server.exe.config</em>” :
<pre class="lang:default decode:true crayon-selected" title="Edition de la configuration Constellation">sudo nano /opt/constellation-server/Constellation.Server.exe.config</pre>
Et modifier le <em>listenUri</em> pour reprendre la même structure d’URI, ici en ajoutant “/constellation” :
<pre class="lang:default decode:true" title="Modification des URI d'écoute du serveur Constellation">&lt;listenUris&gt;
    &lt;uri listenUri="http://+:8088/constellation" /&gt;
&lt;/listenUris&gt;
</pre>
Pour finir, il faudra relancer le service Constellation pour prendre en compte ce changement :
<pre title="Rédémarrage du serveur Constellation" class="lang:xml decode:true">sudo supervisorctl restart constellation-server</pre>
Voilà votre Constellation répond maintenant sur le /constellation, dans notre cas <a title="https://demo.internal.myconstellation.io/constellation" href="https://demo.internal.myconstellation.io/constellation">https://demo.internal.myconstellation.io/constellation</a> :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Constellation derrière Nginx" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-6.png" alt="Constellation derrière Nginx" width="484" height="290" border="0" /></a></p>
<p align="left">Si maintenant on change le path pour “/zm” soit ici <a href="https://demo.internal.myconstellation.io/zm">https://demo.internal.myconstellation.io/zm</a>, c’est notre serveur ZoneMinder qui répondra :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="ZoneMinder derrière Nginx" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-7.png" alt="ZoneMinder derrière Nginx" width="484" height="290" border="0" /></a></p>
On a donc un seul serveur en frontal, Ngnix qui écoute en HTTPS avec le certificat Let’s Encrypt pour sécuriser TOUS les échanges et qui, selon le “path” demandé, transféra les requêtes vers les différents services internes de votre réseau.

A noter que vous pouvez également utiliser des expressions régulières (regex) pour définir vos “locations”. Pour plus d’information : <a title="https://www.scalescale.com/tips/nginx/nginx-location-directive/" href="https://www.scalescale.com/tips/nginx/nginx-location-directive/">https://www.scalescale.com/tips/nginx/nginx-location-directive/</a>