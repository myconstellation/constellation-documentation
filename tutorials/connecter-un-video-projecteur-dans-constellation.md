---
ID: 4888
post_title: >
  Connecter un vidéo projecteur standard
  à Constellation et synchroniser les
  volets
author: Lucas
post_date: 2017-05-16 12:23:36
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/connecter-un-video-projecteur-dans-constellation/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:05:48
---
<em>Par Lucas Dupuis</em>

Ayant fait récemment l'acquisition d'un vidéo-projecteur pour mes soirées films, je me suis vite rendu compte qu'avec les jours qui rallongent, j'ai besoin de fermer les volets de mon salon afin de rester dans une certaine pénombre.

La présentation de mon système de pilotage de volets n'est plus à faire, <a href="/showcases/connecter-volets-constellation-arduino-raspberry/">vous la retrouverez ici</a>. Vous pouvez aussi utiliser des modules Z-Wave comme le FGR-211 qu'on connectera à Constellation via le package <a href="/package-library/vera/">Vera</a> ou <a href="/package-library/jeedom/">Jeedom</a>. Dans tous les cas, nous disposons un <a href="/concepts/messaging-message-scope-messagecallback-saga/">MessageCallback </a>pour ouvrir ou fermer nos volets !

La problématique qui se pose est la suivante : comment savoir que le projecteur est allumé et que je m'apprête à regarder un film ?

J'ai réfléchi à plusieurs solutions :
<ul>
 	<li>Monitorer la consommation de la prise électrique afin de déduire que le projecteur est allumé
<ul>
 	<li>Avantages :
<ul>
 	<li>Permet d'exposer un booléen indiquant que le projecteur est allumé et d'ouvrir/fermer les volets en conséquence</li>
 	<li>Permet de monitorer la consommation en temps réel</li>
 	<li>Permet de monitorer la durée d'utilisation de la lampe du vidéo-projecteur</li>
</ul>
</li>
 	<li>Inconvénients :
<ul>
 	<li>Nécessite une prise connectée avec conso-mètre (à fabriquer ou à acheter)</li>
 	<li>C'est potentiellement complexe et coûteux</li>
</ul>
</li>
</ul>
</li>
 	<li>Surveiller le StateObject de mon médiacenter Kodi (exposé par le package <a href="/package-library/xbmc/">xbmc</a>)
<ul>
 	<li>Avantages :
<ul>
 	<li>J'ai déjà une routine surveillant le stateobject pour allumer et éteindre le système de son lorsqu'un média est joué</li>
 	<li>Il n'y a que du code à mettre en place dans le package "cerveau" de la maison</li>
</ul>
</li>
 	<li>Inconvénients :
<ul>
 	<li>Il n'y a pas de lien direct entre l'allumage du projecteur et une action sur les volets</li>
 	<li>La sélection du film ou du média sur l'écran se fait volets ouverts, et donc c'est potentiellement gênant en cas de luminosité importante</li>
 	<li>Cela ne tient pas compte des autres sources branchées sur le projecteur (TV, console, ...)</li>
</ul>
</li>
</ul>
</li>
 	<li>Utiliser un déclencheur sur le projecteur
<ul>
 	<li>Avantages :
<ul>
 	<li>Montage simple</li>
 	<li>Peu d'investissement</li>
 	<li>Système embarqué trivial</li>
</ul>
</li>
 	<li>Inconvénients :
<ul>
 	<li>Pas de monitoring de consommation</li>
</ul>
</li>
</ul>
</li>
</ul>
J'ai retenu la seconde et la troisième option : la seconde option, consistant à surveiller le stateobject de <a href="/package-library/xbmc/">kodi</a> permet l'allumage du système de son lorsqu'un média est lu, que ce soit un film ou un morceau de musique et la troisième option pour déclencher la fermeture des volets à l'allumage du vidéo projecteur.
<h3>Prerequis</h3>
<ul>
 	<li>Un serveur Constellation</li>
 	<li>Un vidéo-projecteur avec une sortie 12V</li>
 	<li>Un ESP8266 (ou Arduino connecté) avec un régulateur de tension</li>
 	<li>Le SDK Visual Studio</li>
</ul>
<h3>Etape 1 : connecter le vidéo projecteur dans Constellation</h3>
En regardant les caractéristiques de mon projecteur, je me suis rendu compte qu'il existait une sortie 12V permettant de déclencher un moteur pour une toile de projection.
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-03-21.19.35.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Le projecteur vidéo" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-03-21.19.35_thumb.jpg" alt="Le projecteur vidéo" width="454" height="342" border="0" /></a></p>
À la maison je projette sur un mur blanc, je n'ai donc pas besoin de cette sortie. Il s'agit d'un connecteur jack mono que vous trouverez rapidement chez vous dans votre boite de récup' ou bien directement sur le net pour quelques centimes.

Il suffit donc d'un ESP8266 (par exemple un ESP-01, très petit et peu cher) et d'un régulateur de tension 3.3v (ex. LD1117v33) acceptant en entrée une tension entre 5 et 15V et le tour est joué !

En effet, le projecteur envoie du 12v sur la sortie dès qu'il est sous tension et du 0v lorsqu'il est éteint. Ainsi notre ESP-01 sera alimenté par cette sortie. Lorsqu'on allume le vidéo-projecteur, l'ESP-01 sera démarré et lorsqu'on éteint le vidéo projecteur, il sera éteint car plus d'alimentation !

Aussi simple que cela !
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-16-10.27.17.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Schéma du montage avec l'ESP8266" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-16-10.27.17_thumb.jpg" alt="Schéma du montage avec l'ESP8266" width="454" height="193" border="0" /></a></p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-06-15.37.28.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Le vidéo projecteur avec l'ESP8266" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/2017-05-06-15.37.28_thumb.jpg" alt="Le vidéo projecteur avec l'ESP8266" width="454" height="342" border="0" /></a></p>
L'ESP, de son côté, publie simplement toutes les secondes un StateObject ayant une durée de vie de 5 secondes pour indiquer que le projecteur est allumé.

Pour connecter un ESP8266 (ou un Arduino) <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">suivez ce guide</a> ! Une fois connecté dans ma Constellation, je <a href="/client-api/arduino-esp-api/produire-des-stateobjects-depuis-arduino-esp/">publie le StateObject</a> dans la boucle principale <em>loop()</em>
<pre lang="cpp" escaped="true">void loop() {
  constellation.loop();
 
  if(((millis() - lastTime) &gt; 1000) 
    || (millis() &lt; lastTime))
  {
    constellation.pushStateObject("Uptime",  millis(), 5);
    lastTime = millis();
  } 
}
</pre>
On a donc dans les StateObjects de notre Constellation, un StateObject "Uptime" publié dans mon cas par le package (virtuel)  "ESP01_Projector" qui contient l'uptime de mon projecteur :
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/Screenshot-SO.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Le StateObject de l'uptime du projecteur" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/Screenshot-SO_thumb.png" alt="Le StateObject de l'uptime du projecteur" width="457" height="332" border="0" /></a></p>
<p style="text-align: left;" align="center">Ce StateObject est mis à jour par l'ESP toutes les secondes avec une durée de vie de 5 secondes ! Ainsi je n'ai pas besoin de connaitre la valeur du StateObject, il me suffit juste de vérifier que le StateObject est "Valide" et non "Expiré" !</p>
<p style="text-align: left;" align="center">Si il est valide c'est que dans les 5 dernières secondes il y a bien publié son Uptime, donc l' ESP est démarré ce qui implique que mon vidéo projecteur est bien démarré (pour alimenter l'ESP) !</p>
<p style="text-align: left;" align="center">En revanche si ce StateObject est expiré, c'est à dire que le StateObject n'a pas été mis à jour durant les 5 dernières secondes or l'ESP est censé le faire à chaque seconde ! L'ESP-01 est donc déconnecté ce qui impliquerai que le vidéo projecteur est arrêté !</p>

<h3 style="text-align: left;" align="center">Etape 2 : synchroniser le vidéo projecteur avec mes volets</h3>
<p style="text-align: left;" align="center">Maintenant, dans mon package "cerveau" de Constellation, il suffit de surveiller ce StateObject et son état expiré ou non pour savoir quelle commande envoyer aux volets.</p>
<p style="text-align: left;" align="center">J'ai donc créé un package "cerveau" en C#.</p>
<p style="text-align: left;" align="center">Dans ma classe, je crée un <a href="/client-api/net-package-api/consommer-des-stateobjects/">StateObjectLink </a>pour lier le StateObject de l'ESP dans une propriété .NET de mon code :</p>

<pre lang="csharp" escaped="true" class="">/// &lt;summary&gt;
/// StateObject du projecteur. Permet de connaitre l'uptime du projecteur.
/// &lt;/summary&gt;
[StateObjectLink("ESP01_Projector", "Uptime")]
public StateObjectNotifier ProjectorUptime { get; set; }</pre>
Maintenant au démarrage de mon package, j'attache un "handler" sur l'événement "<em>ValueChanged</em>" permettant d'ajouter du code en cas de mise à jour du StateObject.

Ici je vérifie que mon StateObjectLink est bien lié au StateObject (<em>HasValue</em>) et que ce StateObject n'est pas expiré (<em>IsExpired</em>).

Si la condition est vrai, c'est que mon projecteur est allumé, alors je peux fermer mes volets autrement je restaure les volets dans leurs positions précédentes.
<pre class="lang:default decode:true">this.ProjectorUptime.ValueChanged += (s, e) =&gt;
{
    // Projecteur allumé ?
    if (this.ProjectorUptime.HasValue == true &amp;&amp; this.ProjectorUptime.Value.IsExpired == false)
    {
        // Si projecteur est allumé, on ferme les volets.
        foreach (var volet in this.voletConfig)
        {
            if (this.ShowDebug)
            {
                PackageHost.WriteWarn($"Fermeture du volet {volet.Name} à 100%.");
            }
            
            // On enregistre au passage la position de départ pour la restaurer à la fin.
            volet.PreviousPosition = volet.CurrentPosition; 
            
            // Ordre de fermeture (100%).
            PackageHost.CreateMessageScope("ESP_Shutters").ChangePercent(volet.Name, 100);
        }
    }
    else
    {
        // Si projecteur est éteint, on rouvre les volets.
        foreach (var volet in this.voletConfig)
        {
            if (this.ShowDebug)
            {
                PackageHost.WriteWarn($"Ouverture du volet {volet.Name} à l'ancienne position, {Convert.ToInt32(volet.Percent * 100.0)}-&gt;{volet.PreviousPosition}% après {this.tempoReouvertureVolets} secs d'inactivité.");
            }

            // Pour chaque volet, on revient à la position initiale
            PackageHost.CreateMessageScope("ESP_Shutters").ChangePercent(volet.Name, volet.PreviousPosition);
        }
    }
};</pre>
<h3>Conclusion</h3>
Une demi-heure de prototypage et de soudure, un petit quart d'heure de développement, et le challenge est relevé !

L'allumage du projecteur ferme les volets de mon salon selon une consigne, il déclenche également un scénario prédéfini pour les lampes Hue et active la prise du système de son. Le clic sur le bouton de la télécommande rend l'expérience du film beaucoup plus profitable avec Constellation !