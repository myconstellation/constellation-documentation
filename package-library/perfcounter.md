---
ID: 3410
post_title: 'PerfCounter : int&eacute;grez les compteurs de performance Windows dans Constellation'
author: Sebastien Warin
post_date: 2016-10-25 14:40:00
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/perfcounter/
published: true
post_modified: 2016-10-25 16:11:11
---
<p>Le package PerfCounter permet de suivre des compteurs de performance Windows et de les injecter en tant que StateObject dans Constellation.</p> <p>Le code source du package est disponible sur : <a title="https://github.com/myconstellation/constellation-packages/tree/master/PerfCounter" href="https://github.com/myconstellation/constellation-packages/tree/master/PerfCounter">https://github.com/myconstellation/constellation-packages/tree/master/PerfCounter</a></p> <h3>Installation</h3> <p>Depuis le “Online Package Repository” de votre Console Constellation, déployez le package PerfCounter :</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-101.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-93.png" width="350" height="214"></a></p> <p>Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package.</p> <p>Pour finir, sur la page de Settings, vous devez obligatoirement définir le setting “PerfCounters”. Il s’agit d’un setting de type “ConfigurationSection” c’est à dire que la configuration de ce setting est décrite en XML.</p> <p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-103.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-95.png" width="350" height="306"></a></p><pre class="lang:html5 decode:true">&lt;perfCounterSection xmlns="urn:PerfCounter"&gt;
  &lt;perfCounters&gt;     
    &lt;perfCounter id="&lt;&lt; StateObject Name &gt;&gt;" categoryName="&lt;&lt;  Performance Counter Category Name &gt;&gt;" counterName="&lt;&lt; Performance Counter Counter Name &gt;&gt;"  instanceName="&lt;&lt; Optionnal Performance Counter Instance Name &gt;&gt;" /&gt;
  &lt;/perfCounters&gt;
&lt;/perfCounterSection&gt;</pre>
<p>Vous devez spécifier une liste de <em>&lt;perfCounter&gt;</em> dont les attributs sont :</p>
<ul>
<li><u>id</u> (obligatoire) : l’identifiant unique de ce compteur performance qui sera utilisé comme nom pour le StateObject 
<li><u>categoryName</u> (obligatoire) : la catégorie du compteur de performance 
<li><u>counterName</u> (obligatoire) : le nom du compteur de performance 
<li><u>instanceName</u> (optionnel) : le nom de l’instance du compteur de performance si nécessaire 
<li><u>machineName</u> (optionnel) : le nom de la machine sur laquelle récupérer le compteur de performance (si vide, c’est la machine il s’agit local) </li></ul>
<p>Par exemple pour récupérer la mémoire RAM disponible (catégorie “Memory”, nom “Available MBytes”, aucune instance) :</p><pre class="lang:html5 decode:true">&lt;perfCounter id="MemoryAvailableMBytes" categoryName="Memory" counterName="Available MBytes"  /&gt;</pre>
<p>Pour la consommation globale du CPU (catégorie “Processor”, nom “% Processor Time”, instance “_Total”) :</p><pre class="lang:html5 decode:true">&lt;perfCounter id="ProcessorTime" categoryName="Processor" counterName="% Processor Time" instanceName="_Total" /&gt;</pre>
<p>Chaque produit (Constellation Server y compris) peuvent publier des compteurs de performances (ASP.NET, IIS, SQL Server, Windows, etc..). Vous pouvez donc facilement les injecter dans Constellation.</p>
<p>Par exemple, ci-dessous une configuration du package injectant dans Constellation les compteurs de performance relatif au processeur, mémoire RAM, interfaces réseau, disque physique C:, ASP.NET, la CLR .net, le service IIS et SQL Server :</p><pre class="lang:html5 decode:true">&lt;perfCounterSection xmlns="urn:PerfCounter"&gt;
   &lt;perfCounters&gt;     

     &lt;!-- Processor --&gt;
     &lt;perfCounter id="ProcessorTime" categoryName="Processor" counterName="% Processor Time" instanceName="_Total" /&gt;
     &lt;perfCounter id="ProcessorQueueLength" categoryName="System" counterName="Processor Queue Length" /&gt;

     &lt;!-- Disk --&gt;
     &lt;perfCounter id="Disk0_IdleTime" categoryName="PhysicalDisk" counterName="% Idle Time" instanceName="0 C:" /&gt;
     &lt;perfCounter id="Disk0_SecPerRead" categoryName="PhysicalDisk" counterName="Avg. Disk sec/read"   instanceName="0 C:"/&gt;
     &lt;perfCounter id="Disk0_SecPerWrite" categoryName="PhysicalDisk" counterName="Avg. Disk sec/write"  instanceName="0 C:" /&gt;
     &lt;perfCounter id="Disk0_QueueLength" categoryName="PhysicalDisk" counterName="Current Disk Queue Length"  instanceName="0 C:" /&gt;

     &lt;!-- Memory --&gt;
     &lt;perfCounter id="MemoryAvailableMBytes" categoryName="Memory" counterName="Available MBytes"  /&gt;
     &lt;perfCounter id="MemoryPagesPerSec" categoryName="Memory" counterName="Pages/sec"  /&gt;
     &lt;perfCounter id="MemoryCacheBytes" categoryName="Memory" counterName="Cache Bytes"  /&gt;

     &lt;!-- Network Interface --&gt;
     &lt;perfCounter id="NetworkIntefaceBytesPerSec" categoryName="Network Interface" counterName="Bytes Total/sec" instanceName="Broadcom NetXtreme 57xx Gigabit Controller"  /&gt;
     &lt;perfCounter id="NetworkIntefaceOutputQueueLength" categoryName="Network Interface" counterName="Output Queue Length" instanceName="Broadcom NetXtreme 57xx Gigabit Controller" /&gt;

     &lt;!-- Paging File --&gt;
     &lt;perfCounter id="PagingFileUsage" categoryName="Paging File" counterName="% Usage" instanceName="_Total"  /&gt;

     &lt;!-- .NET CLR --&gt;
     &lt;perfCounter id="DotNetExceptionThrownPerSec" categoryName=".NET CLR Exceptions" counterName="# of Exceps Thrown / Sec" instanceName="_Global_" /&gt;
     &lt;perfCounter id="DotNetTotalCommittedBytes" categoryName=".NET CLR Memory" counterName="# Total Committed Bytes" instanceName="_Global_" /&gt;

     &lt;!-- ASP.NET --&gt;
     &lt;perfCounter id="AspNetRequestPerSec" categoryName="ASP.NET Applications" counterName="Requests/Sec" instanceName="__Total__" /&gt;
     &lt;perfCounter id="AspNetApplicationRestart" categoryName="ASP.NET" counterName="Application Restarts" /&gt;
     &lt;perfCounter id="AspNetRequestWaitTime" categoryName="ASP.NET" counterName="Request Wait Time" /&gt;
     &lt;perfCounter id="AspNetRequestQueued" categoryName="ASP.NET" counterName="Requests Queued" /&gt;

     &lt;!-- IIS --&gt;
     &lt;perfCounter id="IISGetRequestPerSec" categoryName="Web Service" counterName="Get Requests/sec" instanceName="_Total"  /&gt;
     &lt;perfCounter id="IISPostRequestPerSec" categoryName="Web Service" counterName="Post Requests/sec" instanceName="_Total"  /&gt;
     &lt;perfCounter id="IISCurrentConnections" categoryName="Web Service" counterName="Current Connections" instanceName="_Total"  /&gt;

     &lt;!-- SQL Server --&gt;
     &lt;perfCounter id="SqlBufferCacheHitRatio" categoryName="MSSQL$SQL2012:Buffer Manager" counterName="Buffer cache hit ratio" /&gt;
     &lt;perfCounter id="SqlPageLifeExpectancy" categoryName="MSSQL$SQL2012:Buffer Manager" counterName="Page life expectancy" /&gt;
     &lt;perfCounter id="SqlBatchRequestsPerSec" categoryName="MSSQL$SQL2012:SQL Statistics" counterName="Batch Requests/Sec" /&gt;
     &lt;perfCounter id="SqlSQLCompilationsPerSec" categoryName="MSSQL$SQL2012:SQL Statistics" counterName="SQL Compilations/Sec" /&gt;
     &lt;perfCounter id="SqlSQLReCompilationsPerSec" categoryName="MSSQL$SQL2012:SQL Statistics" counterName="SQL Re-Compilations/Sec" /&gt;
     &lt;perfCounter id="SqlUserConnections" categoryName="MSSQL$SQL2012:General Statistics" counterName="User Connections" /&gt;
     &lt;perfCounter id="SqlLockWaitsPerSec" categoryName="MSSQL$SQL2012:Locks" counterName="Lock Waits/sec" instanceName="_Total" /&gt;
     &lt;perfCounter id="SqlPageSplitsPerSec" categoryName="MSSQL$SQL2012:Access Methods" counterName="Page Splits/sec" /&gt;
     &lt;perfCounter id="SqlProcessesBlocked" categoryName="MSSQL$SQL2012:General Statistics" counterName="Processes Blocked" /&gt;
     &lt;perfCounter id="SqlCheckpointPagesPerSec" categoryName="MSSQL$SQL2012:Buffer Manager" counterName="Checkpoint Pages/sec" /&gt;

   &lt;/perfCounters&gt;
 &lt;/perfCounterSection&gt;</pre>
<p>Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation :</p><pre class="lang:html5 decode:true">&lt;package name="perfcounter"&gt;
  &lt;settings&gt;
    &lt;setting key="PerfCounters"&gt;
        &lt;content&gt;
          &lt;perfCounterSection xmlns="urn:PerfCounter"&gt;
            &lt;perfCounters&gt;     
              &lt;perfCounter id="&lt;&lt; StateObject Name &gt;&gt;" categoryName="&lt;&lt;  Performance Counter Category Name &gt;&gt;" counterName="&lt;&lt; Performance Counter Counter Name &gt;&gt;"  instanceName="&lt;&lt; Optionnal Performance Counter Instance Name &gt;&gt;" /&gt;
            &lt;/perfCounters&gt;
          &lt;/perfCounterSection&gt;
        &lt;/content&gt;
      &lt;/setting&gt;
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="10"><u>Détail</u></td>
<td valign="top" width="456"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>PerfCounters</strong></td>
<td valign="top" width="10">ConfigurationSection(XML)</td>
<td valign="top" width="10">Obligatoire</td>
<td valign="top" width="456">Configuration des compteurs de performance à suivre</td></tr>
<tr>
<td valign="top" width="10"><strong>RefreshInterval</strong></td>
<td valign="top" width="10">Int32</td>
<td valign="top" width="10">Optionnel<br>Par défaut : 1000</td>
<td valign="top" width="456">Intervalle de temps en milliseconde pour interroger les compteurs de performance enregistrés et mettre à jour les StateObjects correspondants.</td></tr></tbody></table>
<h4>Les StateObjects</h4>
<p>Vous retrouverez autant de StateObject que de compteur de performance enregistrés dans la configuration de votre package.</p>
<table cellspacing="0" cellpadding="2" width="100%" border="0">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td></tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; ID du PerfCounter &gt;&gt;</strong></td>
<td valign="top" width="10">Single</td>
<td valign="top" width="446">Valeur numérique du compteur de performance</td></tr></tbody></table>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/10/image-102.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/10/image_thumb-94.png" width="350" height="280"></a></p>
<h4 align="left">Les MessageCallbacks</h4>
<p>Ce package n’expose aucun MessageCallback.</p>
<h3 align="left">Quelques exemples</h3>
<ul>
<li>
<div align="left">Afficher les valeurs des PerfCounters de différents serveurs Windows sur une page Web</div>
<li>
<div align="left">Piloter une bande de LED en fonction du nombre de requête HTTP sur un serveur IIS avec un Arduino/ESP</div></li></ul>