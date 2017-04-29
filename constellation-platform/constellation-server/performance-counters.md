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
post_modified: 2017-04-29 19:20:48
---
Sur un environnement Windows, le serveur Constellation peut publier des compteurs de performance afin de suivre son activité en temps réel.

Pour cela, vous devez manuellement <a href="/constellation-platform/constellation-server/fichier-de-configuration/">éditer le serveur de configuration</a> pour ajouter la section et la clé de configuration suivante :
<pre class="lang:xml decode:true">&lt;appSettings&gt;
  &lt;add key="EnablePerformanceCounters" value="true" /&gt;
&lt;/appSettings&gt;</pre>
Vous pouvez éditer ce fichier depuis la <a href="/constellation-platform/constellation-console/configuration-editor/">Console Constellation</a>, depuis le <a href="/constellation-platform/constellation-sdk/editer-configuration-constellation-depuis-visual-studio/">SDK</a> ou directement sur votre serveur avec votre éditeur favori.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-18.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Edition de la configuration depuis la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-18.png" alt="Edition de la configuration depuis la Console" width="450" height="227" border="0" /></a></p>
Comme il s’agit d’une modification de bas niveau, vous devez nécessairement redémarrer le service Constellation :
<ul>
 	<li>Soit via la MMC “Services” :</li>
</ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-19.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="MMC Services" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-19.png" alt="MMC Services" width="450" height="274" border="0" /></a></p>

<ul>
 	<li>Soit en ligne de commande :</li>
</ul>
<pre class="lang:bash decode:true">net stop ConstellationServer &amp;&amp; net start ConstellationServer</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-20.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Redémarrage du service Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-20.png" alt="Redémarrage du service Constellation" width="450" height="199" border="0" /></a></p>
Vous pourrez ensuite suivre les indicateurs de performance via WMI ou l’Analyseur de performances de Windows.

Tous les compteurs sont disponibles dans la catégories “Constellation Server” :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-21.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Compteurs de performance Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-21.png" alt="Compteurs de performance Constellation" width="450" height="322" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-22.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-22.png" alt="image" width="450" height="431" border="0" /></a></p>
<p align="left">A noter que cette fonctionnalité n’est disponible que sur un environnement Windows, la plateforme Linux n’ayant pas d’équivalent.</p>