---
ID: 4298
post_title: >
  Surveiller et monitorer les indicateurs
  de performance du serveur Constellation
author: Sebastien Warin
post_date: 2017-04-29 19:12:45
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/constellation-platform/constellation-server/performance-counters/
published: true
post_modified: 2017-04-29 19:12:45
---
<p>Sur un environnement Windows, le serveur Constellation peut publier des compteurs de performance afin de suivre son activité en temps réel.</p> <p>Pour cela, vous devez manuellement <a href="/constellation-platform/constellation-server/fichier-de-configuration/">éditer le serveur de configuration</a> pour ajouter la section et la clé de configuration suivante :</p><pre class="lang:xml decode:true">&lt;appSettings&gt;
  &lt;add key="EnablePerformanceCounters" value="true" /&gt;
&lt;/appSettings&gt;</pre>
<p>Vous pouvez éditer ce fichier directement depuis la <a href="/constellation-platform/constellation-console/configuration-editor/">Console Constellation</a>, depuis le <a href="/constellation-platform/constellation-sdk/editer-configuration-constellation-depuis-visual-studio/">SDK</a> ou directement en éditant le fichier sur votre serveur.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-18.png"><img title="Edition de la configuration depuis la Console" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="Edition de la configuration depuis la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-18.png" width="450" height="227"></a></p>
<p>Comme il s’agit d’une modification de bas niveau, vous devez nécessairement redémarrer le service Constellation :</p>
<ul>
<li>Par la MMC “Services locaux”</li></ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-19.png"><img title="MMC Services" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="MMC Services" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-19.png" width="450" height="274"></a></p>
<ul>
<li>En ligne de commande</li></ul><pre class="lang:bash decode:true">net stop ConstellationServer &amp;&amp; net start ConstellationServer</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-20.png"><img title="Red&eacute;marrage du service Constellation" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="Red&eacute;marrage du service Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-20.png" width="450" height="199"></a></p>

<p>Vous pouvez ensuite suivre les indicateurs de performance via WMI ou l’Analyseur de performances de Windows.</p>
<p>Tous les compteurs sont disponibles dans la catégories “Constellation Server” :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-21.png"><img title="Compteurs de performance Constellation" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="Compteurs de performance Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-21.png" width="450" height="322"></a></p>
<p align="left">Vous pourrez suivre différentes métriques mesurées par le serveur :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-22.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-22.png" width="450" height="431"></a></p>
<p align="left">A noter que cette fonctionnalité n’est disponible que sur un environnement Windows, la plateforme Linux n’ayant pas d’équivalent.</p>