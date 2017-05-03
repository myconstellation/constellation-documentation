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
post_modified: 2017-05-03 15:17:21
---
<p>Sur un environnement Windows le serveur Constellation peut publier des compteurs de performance afin de suivre son activité en temps réel.</p> <p>Vous pourrez alors exploiter ces indicateurs de performance avec des outils de supervision tel quel <a href="https://newrelic.com/plugins/52projects/115">New Relic</a>, <a href="https://www.nagios.com/solutions/performance-counter-monitoring/">Nagios</a>, <a href="http://docs.cacti.net/usertemplate:data:windows:typeperf">Cacti</a>, <a href="https://www.dotcom-monitor.com/windows-performance-counter-monitoring/">dotcom-monitor</a>, <a href="https://www.manageengine.com/products/applications_manager/windows-performance-counters.html">Applications Manager</a>, ou plus simplement en utilisant la MMC “<a href="https://technet.microsoft.com/fr-fr/library/cc749154(v=ws.11).aspx">Analyseur de Performances</a>” de Windows.</p> <p>Vous pouvez également développer vos propres applications pour suivre les indicateurs de performance de votre serveur Constellation étant donné que ces compteurs Windows sont exposés en WMI (<a href="https://msdn.microsoft.com/en-us/library/aa392397(v=vs.85).aspx">voir la documentation</a>). Pour les développeurs .NET, la classe <a href="https://msdn.microsoft.com/fr-fr/library/system.diagnostics.performancecounter(v=vs.110).aspx">PerformanceCounter</a> vous permettra d’interroger ces compteurs très facilement.</p> <h3>Activer les compteurs de performance du serveur Constellation</h3> <p>Pour activer les compteurs de performance, vous devez manuellement <a href="/constellation-platform/constellation-server/fichier-de-configuration/">éditer le serveur de configuration</a> pour ajouter la section et la clé de configuration suivante :</p><pre class="lang:xml decode:true">&lt;appSettings&gt;
  &lt;add key="EnablePerformanceCounters" value="true" /&gt;
&lt;/appSettings&gt;</pre>
<p>Vous pouvez éditer ce fichier depuis la <a href="/constellation-platform/constellation-console/configuration-editor/">Console Constellation</a>, depuis le <a href="/constellation-platform/constellation-sdk/editer-configuration-constellation-depuis-visual-studio/">SDK</a> ou directement sur votre serveur avec votre éditeur favori.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-18.png"><img title="Edition de la configuration depuis la Console" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Edition de la configuration depuis la Console" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-18.png" width="450" height="227"></a></p>
<p>Comme il s’agit d’une modification de bas niveau, vous devez nécessairement redémarrer le service Constellation :</p>
<ul>
<li>Soit via la MMC “Services” : </li></ul>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-19.png"><img title="MMC Services" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="MMC Services" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-19.png" width="450" height="274"></a></p>
<ul>
<li>Soit en ligne de commande : </li></ul><pre class="lang:bash decode:true">net stop ConstellationServer &amp;&amp; net start ConstellationServer</pre>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-20.png"><img title="Red&eacute;marrage du service Constellation" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Red&eacute;marrage du service Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-20.png" width="450" height="199"></a></p>
<p align="left">Vous pourrez ensuite suivre les indicateurs de performance de votre Constellation avec un outil de supervision, l’analyseur de performances de Windows ou même développer vos propres outils en exploitant l’API PerformanceCounter sous .NET ou WMI.</p>
<h3>Exemple : utiliser l’analyseur de performance Windows</h3>
<p>Dans les outils d’administration Windows, lancez la MMC “Analyseur de Performances” et cliquer sur le bouton “Ajouter” dans la toolbar. </p>
<p>Tous les compteurs Constellation sont disponibles dans la catégories “Constellation Server” puis sélectionnez le ou les compteurs à suivre :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-21.png"><img title="Compteurs de performance Constellation" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Compteurs de performance Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-21.png" width="450" height="322"></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/04/image-22.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2017/04/image_thumb-22.png" width="450" height="431"></a></p>
<p align="left">A noter que cette fonctionnalité n’est disponible que sur un environnement Windows, la plateforme Linux n’ayant pas d’équivalent.</p>