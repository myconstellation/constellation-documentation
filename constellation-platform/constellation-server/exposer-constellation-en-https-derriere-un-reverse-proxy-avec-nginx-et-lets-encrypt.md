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
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-25 12:35:13
---
Pour sécuriser votre Constellation vous devez exposer votre serveur sur le protocole HTTPS afin de chiffrer toutes les communications en SSL.

Si vous êtes sur Windows vous pouvez consulter ces deux articles :
<ul>
 	<li><a href="/constellation-platform/constellation-server/exposer-constellation-derrire-un-serveur-web-reverse-proxy/">Reverse Proxy : exposer Constellation derrière IIS</a></li>
 	<li><a href="/constellation-platform/constellation-server/configuration-ssl/">Configuration du serveur Constellation en SSL</a></li>
</ul>
Dans cet article nous allons voir comment réaliser cela sur Linux/Unix.

Pour ce faire nous allons exposer Constellation derrière un serveur Nginx protéger avec un certificat SSL Let’s Encrypt.
<h3>Prérequis : une Constellation fonctionnelle avec un nom DNS</h3>
Avant de démarrer vous devez avoir installer le serveur Constellation sur votre serveur Linux, typiquement un Debian ou Ubuntu.

Cela se résume à lancer la commande suivante :
<pre title="WPI Pour Linux" class="lang:shell decode:true">wget -O install.sh https://developer.myconstellation.io/download/installers/install-linux.sh &amp;&amp; chmod +x install.sh &amp;&amp; ./install.sh</pre>
Pour plus d’information, veuillez suivre le guide suivant : <a href="/constellation-platform/constellation-server/installer-constellation-sur-linux/">Installer Constellation sur Linux</a>

On considéra à ce stade que votre serveur Constellation ainsi que la Console sont bien démarrées. Vous pouvez lancer la commande suivante pour vérifier le statuts :
<pre title="Status des services" class="lang:shell decode:true">sudo supervisorctl status</pre>
Vous devriez voir le “constellation-server” au status “RUNNING” (et la sentinelle si vous l’avez également déployé) :
<pre title="Status des services" class="lang:shell decode:true">sebastien@ubuntu:~$ sudo supervisorctl status
constellation-sentinel           RUNNING   pid 1194, uptime 1 day, 17:36:48
constellation-server             RUNNING   pid 2888, uptime 1 day, 15:06:41</pre>
Pour finir, si vous souhaitez activer le HTTPS et donc ajouter un certificat SSL, vous devez nécessairement avec un nom DNS qui pointe vers l’adresse IP (public) de votre Constellation.
<ul>
 	<li>Si vous avez un nom de domaine, modifiez votre DNS pour ajouter un “host” (enregistrement A) vers l’IP de votre Constellation.</li>
 	<li>SI vous n’avez pas de nom de domaine
<ul>
 	<li>Achetez en un ! Comptez environ 15€/an pour les extensions standards (.fr, .net, .com) ou bien 2,99€ HT par an pour <a href="https://www.ovh.com/fr/domaines/" target="_blank" rel="noopener">un .ovh</a></li>
 	<li>Utilisez un service de “Dynamic DNS” comme dyndns.fr, dyn.com, etc..</li>
 	<li>Certains FAI comme Free proposent un nom DNS type xxxx.hd.free.fr</li>
 	<li>Certains NAS comme Synology permettent aussi de créer un DDNS type xxx.synology.me</li>
</ul>
</li>
</ul>
Dans le cas présent, j’ai crée l’entrée DNS “demo.internal.myconstellation.io” qui pointe vers l’adresse IP public du serveur Constellation installé sur un Ubuntu.

Bien évidement, si votre serveur Constellation est installé dernière un routeur avec du NAT (typiquement sur un réseau local derrière une box Internet) vous devez configurer la redirection de port sur votre routeur/box internet.

Dans un 1er temps redirigez seulement le port 8088 en tcp sur l’IP interne de votre serveur Constellation. A noter que nous supprimerons cette redirection une fois le reverse proxy installé.

Donc, avant de démarrer vous devez maintenant avoir votre serveur Constellation ainsi que sa console qui répondent sur l’URL : <a href="http://&lt;mon_nom_dns&gt;:8088">http://&lt;mon_nom_dns&gt;:8088</a>

Dans mon cas : <a href="http://demo.internal.myconstellation.io:8088">http://demo.internal.myconstellation.io:8088</a> :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Serveur Constellation sur un nom DNS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb.png" alt="Serveur Constellation sur un nom DNS" width="484" height="291" border="0" /></a></p>
Voilà vous êtes prêt, votre Constellation est démarrée et répond bien sur un nom DNS public !
<h3>Exposer Constellation derrière Nginx</h3>
Avant de démarrer, il est recommandé de mettre à jour le référentiel du gestionnaire de package par la commande suivante :
<pre title="Update APT" class="lang:shell decode:true">sudo apt-get update
</pre>
Ensuite installez le serveur Web NGINX par la commande :
<pre title="Installer NGINX" class="lang:shell decode:true">sudo apt-get install nginx
</pre>
Les fichiers de configuration sont dans le répertoire “/etc/nginx/sites-available/” et pour les activer on crée un lien symbolique vers ces fichiers dans le répertoire “/etc/nginx/sites-enabled”.

Pour commencer désactivez la configuration par défaut par la commande
<pre title="Supression de la configuration par défaut" class="lang:shell decode:true">sudo rm /etc/nginx/sites-enabled/default</pre>
Comme vous l’aurez compris le contenu de cette configuration par défaut est toujours présente dans le répertoire “/etc/nginx/sites-available/” mais non activée car supprimée du répertoire “site-enabled”.

Maintenant créez la configuration suivante avec la commande :
<pre title="Création de la configuration Constellation" class="lang:shell decode:true">sudo nano /etc/nginx/sites-available/constellation</pre>
Dans le fichier, copiez le contenu suivant :
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
Vous devez modifier le paramètre “server_name” avec le nom DNS de votre serveur Constellation, dans le cas présent “demo.internal.myconstellation.io”.

Il est important de bien reprendre les mêmes options, notamment le “proxy_buffering” pour permettre le reverse proxy du “<em>server-Sent events</em>” vers le serveur Constellation.

Tapez ensuite sur la combinaison de touches “Ctrl+X” pour quitter en prenant suivant d’enregistrer le fichier. Puis pour activer cette configuration créez le lien symbolique suivant :
<pre title="Activation de la configuration" class="lang:shell decode:true">sudo ln -s /etc/nginx/sites-available/constellation  /etc/nginx/sites-enabled/constellation</pre>
Et pour finir recharger NGINX pour prendre en compte la configuration fraichement créée :
<pre title="Recharger NGINX" class="lang:shell decode:true">sudo systemctl reload nginx</pre>
Votre Constellation est maintenant accessible derrière NGINX sur le port 80.

Rendez-vous donc sur l’adresse http://&lt;votre_nom_dns&gt; (sans spécifier le port, car 80 par défaut) pour vérifier que le reverse proxy est opérationnel. Dans le cas présent <a href="http://demo.internal.myconstellation.io">http://demo.internal.myconstellation.io</a>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-1.png" alt="image" width="484" height="290" border="0" /></a></p>
<p align="left">Ce n’est donc plus Constellation qui répond mais NGINX qui lui même consulte le serveur Constellation en “local”.</p>
(Note : si vous êtes dernière un routeur avec NAT, n’oubliez pas d’ajouter la redirection du port 80 vers votre serveur Constellation interne. Par la même occasion vous pouvez supprimer la redirection du port 8088, ainsi tout passera nécessairement par NGINX).

(Note 2 : ici le reverse proxy Nginx est installé sur la même machine que le serveur Constellation mais il est aussi envisageable de dissocier les deux pour certaine architecture robuste : un serveur frontend avec Nginx et un serveur Constellation)
<h3>Activer le HTTPS avec des certificats SSL Let’s Encrypt</h3>
Maintenant que votre service Constellation est exposé dernière le serveur NGINX vous allez pouvoir configurer différentes choses sur NGINX comme par exemple des restrictions , l’authentification, des limites (throttling) et bien d’autre chose. Dans le cas présent on va s’intéresser à configurer le HTTPS sur votre serveur NGINX.

Pour activer le HTTPS il vous faut un certificat SSL. Pour simplifier le process, Let’s Encrypt est une autorité de certification lancée en 2015 qui permet d’automatiser la génération de certificat. De plus le service est gratuit ! Les certificats sont valides 3 mois, il faudra donc régulièrement les renouveler mais vous allez voir que cette action est entièrement automatisable.

Ici nous allons utiliser <a href="https://certbot.eff.org/" target="_blank" rel="noopener">Certbot</a>, un agent installé sur le serveur qui permet de générer et renouveler automatiquement les certificats SSL Let’s Encrypt.

Commencez par ajouter le repository :
<pre title="Installer le repository APT" class="lang:shell decode:true">sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update</pre>
Ensuite installer Certbot pour Nginx:
<pre title="Installer Certbot pour Nginx" class="lang:shell decode:true">sudo apt-get install python-certbot-nginx</pre>
Une fois installé, lancez la commande “<em>certbot –nginx</em>” en spécifiant le nom DNS vers votre serveur.

Par exemple dans le cas présent :
<pre title="Générer le certificat" class="lang:shell decode:true">sudo certbot --nginx -d demo.internal.myconstellation.io
</pre>
L’assistant va tout d’abord vous demander de remplir votre adresse mail :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-2.png" alt="image" width="484" height="100" border="0" /></a></p>
Après avoir accepté les conditions du service, l’assistant va automatiquement valider un chalenge en interrogeant votre site Web (d’où la nécessité d’avoir exposer votre serveur sur Internet), créer le certificat SSL et le configurer sur Nginx.

Pour finir l’assistant vous proposera de modifier automatiquement la configuration de votre Nginx pour rediriger tous les appels HTTP (port 80) vers HTTPS (port 443).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Activation de la redirection HTTPS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-3.png" alt="Activation de la redirection HTTPS" width="484" height="438" border="0" /></a></p>
Choisissiez l’option 2 pour activer cette redirection.

Et voilà, c’est fini, votre serveur Nginx est configurer avec le certificat SSL Let’s Encrypt et tous les appels sont redirigés en HTTPS et donc chiffrés !
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-4.png" alt="image" width="484" height="108" border="0" /></a></p>
<p align="left"><strong>Votre Constellation est maintenant protégée !</strong></p>
<p align="left">Rechargez (F5) votre navigateur sur l’URL <em>http://&lt;mon_nom_dns&gt;</em> vous constaterez la redirection automatique en https:// et le cadenas vert vous informant du chiffrement SSL :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-6.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Constellation en HTTPS" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-5.png" alt="Constellation en HTTPS" width="484" height="290" border="0" /></a></p>
<p align="left">(Note : pour les personnes derrières un NAT, n’oubliez pas non plus la redirection du port 443 vers votre serveur Constellation interne)</p>
<p align="left">Pour vérifier le bon renouvèlement du certificat vous pouvez utiliser la commande suivante :</p>

<pre title="Tester le renouvellement du certificat" class="lang:shell decode:true">sudo certbot renew --dry-run</pre>
<p align="left">Certbot a automatiquement installé un trigger dans systemd (ou crontab si pas de systemd) pour réaliser le renouvèlement et déploiement automatique de vos certificats !</p>
<p align="left">Pour les plus curieux, vous pouvez jeter un œil au fichier “/etc/nginx/sites-available/constellation” pour voir dans quelle mesure Certbot l’a modifié.</p>

<h3>Pour aller plus loin</h3>
<h4>Sécuriser le serveur avec un firewall UFW et fail2ban</h4>
Pour aller plus loin sur l’aspect sécurité :
<h5>Installer fail2ban</h5>
<a href="http://fail2ban.sourceforge.net/">Fail2ban</a> est un script tournant en tâche de fond et qui va vérifier si des tentatives d'authentification via SSH et Nginx échouent ; et en cas d'attaque, bannir l'IP grâce à <code>iptables</code>.
<pre title="Tester le renouvellement du certificat" class="lang:shell decode:true">sudo apt-get install fail2ban</pre>
Pour plus d’info:  <a title="https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04" href="https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04">https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04</a>
<h5>Installer UFW</h5>
UFW est un firewall applicatif. Pour l’installer :
<pre title="Installer UFW" class="lang:shell decode:true">sudo apt-get install ufw</pre>
Autorisez ensuite les services HTTP (nécessaire pour le challenge Let’s Encrypt), HTTPS (notre reverse proxy vers Constellation) et SSH :
<pre title="Configurez UFW" class="lang:shell decode:true">sudo ufw allow http
sudo ufw allow https
sudo ufw allow ssh
</pre>
Pour l’activer :
<pre title="Activer UFW" class="lang:shell decode:true">sudo ufw enable</pre>
Ainsi votre serveur Linux sera correctement protéger. Vous noterez que le port 8088 ne sera plus accessible car bloquer par le firewall UFW, il faudra forcement se connecter à Constellation par Nginx en HTTPS. N’oubliez pas alors de modifier l’URI de votre Constellation sur votre sentinelle existante pour prendre en compte ce changement.
<h4>Ajouter d’autre application Web dernière votre reverse proxy SSL</h4>
Actuellement vous avez un nom DNS type “demo.internal.myconstellation.io” qui pointe vers le serveur Linux sur lequel le service Nginx répond en HTTP (80) et HTTPS (443) avec un certificat Let’s Encrypt pour “proxifier” les requêtes au service Constellation accessible localement sur le port 8088.

Vos utilisateurs se connectent donc forcement en HTTPS sur le reverse proxy pour accéder à Constellation !

La règle configurée dans Ngnix est de “proxifier” toutes les requêtes à partir de ”/” vers Constellation :
<pre title="Extrait de la configuration Nginx" class="lang:shell decode:true">location / {
        proxy_pass http://localhost:8088;
....
}
</pre>
Mais il est également possible d’adapter ces règles pour que votre R.P. ne sont plus exclusivement un passe-plat pour Constellation mais aussi pour vos autres services en Interne (une box domotique, un serveur Web, un objet connecté X, une camera IP Y, etc…).

Vous aurez ainsi la possibilité d’exposer différent service Web interne^dernière votre R.P. profitant ainsi d’un même port TCP (le 443) et donc d’un même canal de communication sécurisé (cryptage SSL).

Parmi les différentes options pour ce type de configuration :
<ol>
 	<li>Proxification en fonction du “server_name”</li>
 	<li>Proxification en fonction de la “location”</li>
</ol>
<h5>Reverse Proxy en fonction du server_name</h5>
Le principe est de refaire ce tutoriel depuis le début pour chacune de vos applications à exposer.

Vous devez ainsi créer une adresse DNS vers votre IP public pour chaque site, créer une configuration Nginx en changeant le “proxy_pass” vers la ressource interne à exposer (et le server_name) et a créer un certificat SSL pour tous les sites créés.

Vous autres ainsi une multitude de nom DNS qui pointerons tous vers votre serveur Nginx qui lui redirigera vers les ressources internes. Par exemple “constellation.mondomaine.fr”, “macamera.mondomaine.fr”, “jeedom.mondomaine.fr”, etc.. etc..

La configuration est simple car il s’agit de répéter ce tutoriel pour chaque service, par contre l’administration est un peu lourde et vous aurez autant de sous domaine et donc de certificat que de service à exposer.
<h5>Reverse Proxy en fonction de la “location”</h5>
Dans ce mode, nous gardons un seul site Nginx (celui configuré dans le fichier /etc/nginx/sites-available/constellation) et donc un seul nom de domaine (ici demo.internal.myconstellation.io) avec son certificat associé.

Cependant au lieu de “rediriger” les requêtes depuis la racine “/” vers Constellation nous allons créer plusieurs “location”.

Prenons un exemple pour bien comprendre. Ici le serveur Ngnix est installé sur la même que le serveur Constellation, donc localhost. Imaginons que dans ce même réseau LAN j’ai un autre serveur Linux (ou autre) avec par exemple le service ZoneMinder sur 192.168.0.10 et Jeedom sur 192.168.0.11.

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

Pour cela modifiez le ficher “/opt/constellation-server/Constellation.Server.exe.config” :
<pre title="Modification de la configuration Constellation" class="lang:shell decode:true">sudo nano /opt/constellation-server/Constellation.Server.exe.config</pre>
Et modifier le listenUri pour reprendre la même structure d’URI, ici en ajoutant “/constellation” :
<pre title="Modification de la configuration Constellation" class="lang:xml decode:true">&lt;listenUris&gt;
    &lt;uri listenUri="http://+:8088/constellation" /&gt;
&lt;/listenUris&gt;
</pre>
Pour finir, il faudra relancer le service Constellation pour prendre en compte ce changement :
<pre title="Rédémarrage du serveur Constellation" class="lang:xml decode:true">sudo supervisorctl restart constellation-server</pre>
Voilà votre Constellation répond maintenant sur le /constellation, dans notre cas <a title="https://demo.internal.myconstellation.io/constellation" href="https://demo.internal.myconstellation.io/constellation">https://demo.internal.myconstellation.io/constellation</a>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-7.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Constellation derrière Nginx" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-6.png" alt="Constellation derrière Nginx" width="484" height="290" border="0" /></a></p>
<p align="left">Si maintenant, on change le path pour “/zm” soit ici <a href="https://demo.internal.myconstellation.io/zm">https://demo.internal.myconstellation.io/zm</a>, c’est maintenant notre serveur ZoneMinder qui répondra :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2018/04/image-8.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="ZoneMinder derrière Nginx" src="https://developer.myconstellation.io/wp-content/uploads/2018/04/image_thumb-7.png" alt="ZoneMinder derrière Nginx" width="484" height="290" border="0" /></a></p>
On a donc un seul serveur en frontal, Ngnix qui écoute en HTTPS avec le certificat Let’s Encrypt pour sécuriser TOUS les échanges et qui, selon le “path” demandé, transféra les requêtes vers les différents services internes de votre réseau.

A noter que vous pouvez également utiliser des expressions régulières (regex) pour définir vos “locations”. Pour plus d’information : <a title="https://www.scalescale.com/tips/nginx/nginx-location-directive/" href="https://www.scalescale.com/tips/nginx/nginx-location-directive/">https://www.scalescale.com/tips/nginx/nginx-location-directive/</a>