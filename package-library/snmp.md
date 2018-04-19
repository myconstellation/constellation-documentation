---
ID: 4922
post_title: 'SNMP : collectez les compteurs SNMP de vos ressources réseau dans Constellation'
author: Sebastien Warin
post_date: 2017-05-21 10:37:33
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/package-library/snmp/
published: true
publish_post_category:
  - "7"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 11:21:34
---
Le package SNMP vous permet de collecter différentes informations sur vos ressources informatique (switchs, routeurs, serveurs, imprimantes, etc...) en utilisant le <b>Simple Network Management Protocol</b> (SNMP).

Toutes les informations collectées sont ensuite publiées dans Constellation sous forme de StateObject que vous pouvez suivre en temps réel depuis vos pages Web, objets connectés, applications ou tout autre système connecté à votre Constellation.

Le code source de ce package est en ligne sur : <a href="https://github.com/myconstellation/constellation-packages/tree/master/Snmp">https://github.com/myconstellation/constellation-packages/tree/master/Snmp</a>
<h3>Installation</h3>
Depuis le “Online Package Repository” de votre Console Constellation, déployez le package SNMP.

Une fois le package télécharger votre repository local, sélectionnez la sentinelle sur laquelle déployer le package et spécifiez le setting "<em>snmpConfiguration</em>" que nous décrions dans la section suivante.

Vous pouvez également déployer ce package manuellement dans la configuration de votre Constellation, par exemple :
<pre class="lang:xhtml decode:true">&lt;package name="Snmp"&gt;
  &lt;settings&gt;
    &lt;setting key="snmpConfiguration"&gt;
      &lt;content&gt;
        &lt;snmpConfiguration queryInterval="00:00:10" multipleStateObjectsPerDevice="false"&gt;
          &lt;devices&gt;
            &lt;device host="myDevice.domain.com" /&gt;
            &lt;device host="192.168.0.1" /&gt;
            &lt;device host="192.168.0.10" community="demo" /&gt;
          &lt;/devices&gt;
        &lt;/snmpConfiguration&gt;
      &lt;/content&gt;
    &lt;/setting&gt;	
  &lt;/settings&gt;
&lt;/package&gt;</pre>
<h3>Détails du package</h3>
<h4>Les Settings</h4>
Ce package ne comporte qu'un seul setting nommé "<em>snmpConfiguration</em>" qui est une ConfigurationSection XML obligatoire.

Cette section XML définit la liste des hôtes SNMP à collecter. Pour chaque hôte (<em>device</em>), vous devez obligatoirement définir l’attribut "<em>host</em>" avec l'adresse IP ou DNS. Vous pouvez optionnellement spécifier le nom de la communauté SNMP (par défaut "public").

Sur la section racine "<em>snmpConfiguration</em>" vous pouvez optionnellement définir ces deux attributs :
<ul>
 	<li><strong>queryInterval</strong> (TimeSpan) : l'intervalle de temps  d’interrogation SNMP pour chaque hôte (par défaut à 5 secondes)</li>
 	<li><strong>multipleStateObjectsPerDevice</strong> (Boolean) :  spécifie si l'ensemble des compteurs SNMP d'un équipement doivent être publiés en plusieurs StateObject ou dans un seul et unique StateObjet (choix par défaut).</li>
</ul>
<h4>Les StateObjects</h4>
Si l'attribut de configuration "<em>multipleStateObjectsPerDevice = false</em>" (par défaut), vous retrouverez un StateObject par <em>&lt;device&gt;</em> déclaré dans la section de configuration du package.
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;</strong></td>
<td valign="top" width="10"><span class="pl-en">SnmpDevice</span></td>
<td valign="top" width="446">Objet principal décrivant l'intégralité de l'équipement. Contient les propriétés : Description (<span class="pl-en"><em>SystemDescription</em>), Host (<em>Host</em>), la liste des interfaces réseau (<em>Sequence&lt;NetworkInterface&gt;</em>), la liste des adresses réseau (<em>Sequence&lt;Address&gt;</em>), la liste des équipements de stockage (<em>Sequence&lt;Storage&gt;</em>) ou encore la liste des processeurs (<em>Sequence&lt;Processor&gt;</em>).</span></td>
</tr>
</tbody>
</table>
<p align="left">A l'inverse, si l'attribut de configuration "<em>multipleStateObjectsPerDevice = true</em>", vous retrouverez plusieurs StateObjects par  <em>&lt;device&gt;</em> déclaré dans la section de configuration du package.</p>

<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="10"><u>Type</u></td>
<td valign="top" width="446"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Description</strong></td>
<td valign="top" width="10">SystemDescription</td>
<td valign="top" width="446">Objet décrivant l'équipement : type de matériel, OS, uptime, etc...</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Addresses/&lt;&lt; Address Key &gt;&gt;</strong></td>
<td valign="top" width="10">Address</td>
<td valign="top" width="446">Un StateObject par adresse IP attachée au système : IP, masque réseau, adresse de broadcast, ...)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Interfaces/&lt;&lt; Interface Key &gt;&gt;</strong></td>
<td valign="top" width="10">NetworkInterface</td>
<td valign="top" width="446">Un StateObject par interface réseau attachée au système : type, vitesse, adresse physique, état, consommation, etc ...</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Host</strong></td>
<td valign="top" width="10">System</td>
<td valign="top" width="446">Un StateObject décrivant l’hôte (mémoire, nombre d'utilisateur, nombre de processus, etc..)</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Processors/&lt;&lt; Processor Key &gt;&gt;</strong></td>
<td valign="top" width="10">Processor</td>
<td valign="top" width="446">Un StateObject par processeur indiquant la charge et l'ID.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>&lt;&lt; snmpDeviceId &gt;&gt;/Storages/&lt;&lt;Storage Key &gt;&gt;</strong></td>
<td valign="top" width="10">Storage</td>
<td valign="top" width="446">Un StateObject par unité de stockage logique (taille libre et utilisée, type, etc..)</td>
</tr>
</tbody>
</table>
<h4 align="left">Les MessageCallbacks</h4>
Le package expose 2 MessageCallbacks :
<table border="0" width="100%" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="10"><u>Nom</u></td>
<td valign="top" width="141"><u>Réponse (saga)</u></td>
<td valign="top" width="407"><u>Description</u></td>
</tr>
<tr>
<td valign="top" width="10"><strong>CheckAgent</strong></td>
<td valign="top" width="141"><span class="pl-k">bool</span></td>
<td valign="top" width="407">Test si l'agent SNMP est fonctionnelle pour l'adresse spécifiée.</td>
</tr>
<tr>
<td valign="top" width="10"><strong>ScanDevice</strong></td>
<td valign="top" width="141"><span class="pl-en">SnmpDevice</span></td>
<td valign="top" width="407">Collecte et retourne les informations SNMP pour l'adresse spécifiée</td>
</tr>
</tbody>
</table>
<h3 align="left">Quelques exemples</h3>
<ul>
 	<li>Création un dashboard Web ou mobile en Javascript de supervision des indicateurs clés</li>
 	<li>Surveillance des disques dur avec envoie de notification en cas de saturation</li>
</ul>