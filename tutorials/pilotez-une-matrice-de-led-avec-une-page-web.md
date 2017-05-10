---
ID: 4532
post_title: 'Pilotez une matrice de LED avec une page Web en temps r&eacute;el'
author: Sebastien Warin
post_date: 2017-05-09 14:48:57
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/pilotez-une-matrice-de-led-avec-une-page-web/
published: true
post_modified: 2017-05-10 11:33:23
---
Découvrons dans ce tutoriel comment piloter en temps réel une matrice de LED bicolore depuis une page Web en quelque ligne.
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESP Matrix Controller" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/ESPMatrix2.gif" alt="ESP Matrix Controller" width="400" height="225" border="0" /></p>
Pour cela nous allons utiliser une matrice de 8x8 LEDs pilotable en I²C que vous pouvez trouver <a href="https://www.adafruit.com/product/902">chez Adafruit</a> pour environ 15€. Pour piloter cette matrice nous allons utiliser un ESP8266, ici un <a href="https://www.wemos.cc/product/d1-mini-pro.html">D1 Mini Pro</a> (environ 6€).
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040128.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESP Matrix Controller" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040128_thumb.jpg" alt="ESP Matrix Controller" width="404" height="304" border="0" /></a></p>

<h3 align="left">Prérequis</h3>
<p align="left">Pour réaliser ce tutoriel, vous aurez besoin :</p>

<ul>
 	<li>
<div align="left">Un serveur Constellation</div></li>
 	<li>
<div align="left">Un ESP8266</div></li>
 	<li>
<div align="left">Une matrice de LED I²C</div></li>
</ul>
<p align="left">Si c’est la première fois que vous utilisez un ESP8266 connecté à Constellation, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction</a> avant de commencer !</p>

<h3 align="left">Etape 1 : connecter la matrice de LED</h3>
<p align="left">Nous avons ici utilisé un D1 Mini Pro, un ESP8266 intégrant nativement une interface de programmation USB, très pratique pour créer des prototypes rapides.</p>
<p align="left">La matrice de LED se connecte à l’ESP8266 en I²C. Nous avons utilisé les fils Noir et Blanc pour alimenter la matrice depuis les pins “5V” et “GND” du D1 Mini ainsi que les fils Bleu et Vert pour connecter les ports D1 et D2 du D1 Mini aux ports SCL et SDA de la matrice de LED :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040132.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="D1 Mini avec un 8x8 LED Matrix" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040132_thumb.jpg" alt="D1 Mini avec un 8x8 LED Matrix" width="354" height="266" border="0" /></a></p>
<p align="left">Et voilà le montage est déjà fini ! Vous pouvez maintenant connecter votre D1 Mini en USB à votre PC pour le programmer.</p>

<h3 align="left">Etape 2 : programmer l’ESP8266</h3>
<p align="left">Pour démarrer vous devez dans Constellation déclarer une sentinelle associée à une clé d'accès et un package virtuel. Ici notre ESP8266 est représenté par la sentinelle nommée "ESP8266" et le package virtuel se nomme "MatrixController".</p>
<p align="left">Dans l'Arduino IDE, nous créons un nouveau projet à partir du Starter Kit Constellation pour ESP8266 dans lequel nous allons définir le nom de notre réseau Wifi (SSID) ainsi que sa clé d'accès  puis nous configurerons le client Constellation en spécifiant l'identité de notre package virtuel, sa clé d'accès et l'adresse/port de notre serveur Constellation :</p>

<pre class="lang:cpp decode:true">// ESP8266 Wifi
#include &lt;ESP8266WiFi.h&gt;
char ssid[] = "MON SSID";
char password[] = "macléWifi!!!!";

// Constellation client
Constellation&lt;WiFiClient&gt; constellation("X.X.X.X", 8088, "ESP8266", "MatrixController", "xxxxxxxxxxxxxxxxx");</pre>
<p align="left">Encore une fois si tout cela est nouveau pour vous, je vous recommande <a href="/getting-started/connecter-un-arduino-ou-un-esp8266-constellation/">de suivre ce guide d’introduction à l'API Constellation pour Arduino/ESP8266</a>.</p>
<p align="left">Maintenant que la coquille de notre package virtuel sur ESP8266 est prête, ajoutons les librairies d'Adafruit pour piloter la matrice de LED bicolore.</p>
<p align="left">Dans le gestionnaire de bibliothèque (menu<em> Croquis &gt; Inclure une bibliothèque</em>), installez les librairies suivantes :</p>

<ul>
 	<li>Adafruit GFX</li>
 	<li>Adafruit LED Backpack</li>
</ul>
Puis dans votre code, ajoutez ces librairies et déclarez une variable de type "Adafruit_BicolorMatrix" que nous nommerons "matrix" :
<pre class="lang:cpp decode:true crayon-selected">#include "Adafruit_LEDBackpack.h"
#include "Adafruit_GFX.h"
#include &lt;Wire.h&gt;
Adafruit_BicolorMatrix matrix = Adafruit_BicolorMatrix();</pre>
<p align="left">Au démarrage, dans la méthode "setup()", initialisez la matrice de LED. Pour rappel nous avons branché la matrice I²C sur les ports D1 et D2 :</p>

<pre class="lang:cpp decode:true">// Init matrix led
Wire.begin(D1, D2);
matrix.begin(0x70);</pre>
<p align="left">Toujours dans la méthode d'initialisation, enregistrons un premier MessageCallback que nous allons nommer "SetPixel". Ce MessageCallback permettra d'allumer ou éteindre un pixel de la matrice.</p>
<p align="left">Nous le déclarons avec 4 paramètres d'entrée :</p>

<ul>
 	<li>x et y pour définir la position du pixel à piloter</li>
 	<li>state : un boolean indiquant si le pixel doit être allumé ou éteint</li>
 	<li>clear: un paramètre optionnel de type boolean pour indiquer si il faut effacer la matrice avant (par défaut "true")</li>
</ul>
Comme vous le constatez ci-dessous, le code de ce MessageCallback commence par effacer la matrice si le dernière paramètre (<em>clear</em>) n'est pas défini (car optionnel) ou si il est à "<em>true</em>" en invoquant la méthode "<em>matrix.clear()</em>".

Ensuite on appelle la méthode "<em>drawPixel</em>" en passant les arguments de notre MC, à savoir le "x", le "y" et le "state".

Si le paramètre "<em>state</em>" est vrai, on allume le pixel en vert (LED_GREEN) sinon on l'éteint (LED_OFF).

Le code est donc :
<pre class="lang:cpp decode:true">// Register SetPixel
constellation.registerMessageCallback("SetPixel",
  MessageCallbackDescriptor().setDescription("Set Pixel On or Off").addParameter&lt;int&gt;("x").addParameter&lt;int&gt;("y").addParameter&lt;bool&gt;("state", "ON or OFF").addOptionalParameter&lt;bool&gt;("clear", true, "Clear matrix before drawn"),
  [](JsonObject&amp; json) {
    if(json["Data"].size() &lt; 4 || json["Data"][3].as&lt;bool&gt;()) {
      matrix.clear();
    }
    matrix.drawPixel(json["Data"][0].as&lt;int&gt;(), json["Data"][1].as&lt;int&gt;(), json["Data"][2].as&lt;bool&gt;() ? LED_GREEN : LED_OFF);  
    matrix.writeDisplay();
 });</pre>
<p align="left">Comme il s'agit d'une matrice bicolore, on peut allumer chaque pixel en vert ou en rouge, ou bien en vert ET en rouge ce qui donne du orange.</p>
<p align="left">Nous allons donc ajouter un deuxième MessageCallback que nous nommerons "SetPixelColor" sensiblement identique au premier, sauf que le paramètre "state" est maintenant de type  "int" et se nomme "color".</p>
<p align="left">D'après les constantes de la librairie d'Adafruit, LED_GREEN = 2, LED_OFF = 0, le rouge (LED_RED) = 1 et le Orange/Jaune = 2. Le code est donc :</p>

<pre class="lang:cpp decode:true">// Register SetPixelColor
constellation.registerMessageCallback("SetPixelColor",
  MessageCallbackDescriptor().setDescription("Set Pixel On or Off").addParameter&lt;int&gt;("x").addParameter&lt;int&gt;("y").addParameter&lt;int&gt;("color", "0 = Off, 1 = red, 2 = orange, 3 = green").addOptionalParameter&lt;bool&gt;("clear", true, "Clear matrix before drawn"),
  [](JsonObject&amp; json) {
    if(json["Data"].size() &lt; 4 || json["Data"][3].as&lt;bool&gt;()) {
      matrix.clear();
    }
    matrix.drawPixel(json["Data"][0].as&lt;int&gt;(), json["Data"][1].as&lt;int&gt;(), json["Data"][2].as&lt;int&gt;());  
    matrix.writeDisplay();
 });</pre>
<p align="left">Pour finir, ajoutons un troisième et dernier MessageCallback pour effacer l'écran :</p>

<pre class="lang:cpp decode:true">// Register Clear
constellation.registerMessageCallback("Clear",
  MessageCallbackDescriptor().setDescription("Clear matrix"),
  [](JsonObject&amp; json) {
    matrix.clear();
    matrix.writeDisplay();
 });</pre>
<p align="left">Vous pouvez téléverser le programme puis en vous connectant sur le <a href="/constellation-platform/constellation-console/messagecallbacks-explorer/">MessageCallbacks Explorer</a> de la Console Constellation vous constaterez que les trois MessageCallbacks sont bien référencés dans votre Constellation.</p>
<p align="left">Votre ESP8266 est prêt ! Vous pouvez tester les MC directement depuis la Console pour vous assurer que tout fonctionne correctement :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-48.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-48.png" alt="MessageCallbacks Explorer" width="404" height="284" border="0" /></a></p>
<p style="text-align: left;" align="center">Par exemple en invoquant le MessageCallback "SetPixel" avec<em> x=1</em>,<em> y=2</em> et <em>state=true</em>, le résultat est instantanément visible sur la matrice :</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040131-1.jpg"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Test du package virtuel" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/P1040131_thumb-1.jpg" alt="Test du package virtuel" width="404" height="304" border="0" /></a></p>

<h3 align="left">Etape 3 : créer une page Web</h3>
<p align="left">Maintenant que notre matrice connectée est prête, créons une page Web pour pouvoir la piloter.</p>
<p align="left">Pour cela nous allons créer une grille de 8x8 représentant la matrice sur laquelle nous pourrions dessiner à la sourie ou au droit (touch) avec la possibilité de choisir la couleur entre le verte, le roue et le orange. Nous allons utiliser le Canvas HTML5 pour cela.</p>
<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2017/05/image-49.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Page de contrôle en HTML5/JS" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/image_thumb-49.png" alt="Page de contrôle en HTML5/JS" width="204" height="278" border="0" /></a></p>
<p align="left">Inutile d'utiliser le framework AngularJS pour cela, nous utiliserons la librairie Constellation pour Javascript.</p>
<p align="left">Commençons donc par créer une page HTML classique dans laquelle ajoutons dans l’entête <em>&lt;head&gt;</em> les scripts nécessaires pour <a href="/getting-started/connectez-vos-pages-web-constellation/">connecter la page à Constellation</a> :</p>

<pre class="lang:html5 decode:true">&lt;meta name="viewport" content="width=device-width" /&gt;
&lt;script type="text/javascript" src="https://code.jquery.com/jquery-2.2.4.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://cdn.myconstellation.io/js/Constellation-1.8.1.min.js"&gt;&lt;/script&gt;
</pre>
Nous avons également ajouté la meta "viewport" pour adapter proprement notre page sur un mobile.

Dans le corps <em>&lt;body&gt;</em> de notre page ajoutons un canvas pour la grille, la liste des couleurs possibles et un bouton pour effacer l'écran :
<pre class="lang:html5 decode:true">&lt;canvas id="canvas" width="400" height="400"&gt;&lt;/canvas&gt;

&lt;ul id="colors"&gt;
    &lt;li&gt;&lt;a href="#" data-colorId="0" data-color="white"&gt;Eraser&lt;/a&gt;&lt;/li&gt;
    &lt;li&gt;&lt;a href="#" data-colorId="1" data-color="red" class="selected"&gt;Rouge&lt;/a&gt;&lt;/li&gt;
    &lt;li&gt;&lt;a href="#" data-colorId="2" data-color="orange"&gt;Orange&lt;/a&gt;&lt;/li&gt;
    &lt;li&gt;&lt;a href="#" data-colorId="3" data-color="green"&gt;Vert&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;

&lt;button id="clear"&gt;Clear&lt;/button&gt;
</pre>
Vous remarquerez que nous stockons dans des attributs "data-" l'ID de la couleur de la matrice de LED (0 à 3) et le nom de la couleur HTML équivalente (ex. red = 1). La couleur sélectionnée est repérée par la classe CSS "selected".

Pour la mise en page justement, ajoutez le code CSS dans une balise <em>&lt;style&gt;</em> dans la page :
<pre class="lang:css decode:true">body {
    text-align:center; 
    background:#e9e9e9
}        
#canvas {
    margin:0 auto 20px auto; 
    display:block; 
    background:#fff; 
    cursor:crosshair
}
#colors {
    list-style:none; 
    margin:0 0 20px 0;
    padding:0
}
#colors li {
    display:inline-block
}
#colors a {
    display:inline-block; 
    width:50px; 
    height:50px; 
    margin-right:10px; 
    text-indent:-4000px; 
    overflow:hidden; 
    border-radius:50%
}
#colors a.selected {
    border:2px solid #000; 
    width:45px; 
    height:45px
}
button {
  border-radius: 8px;
  padding: 10px 20px 10px 20px;
  color: #ffffff;
  font-size: 20px;
  background: #808080;
}
</pre>
Enfin, passons au code Javascript de notre page. Ouvrez une balise &lt;script&gt; pour ajouter le code ci-dessous.

On commence tout d'abord par déclarer la taille de notre matrice (8x8) ainsi que le client "consumer" de Constellation en spécifiant l'adresse/port de votre serveur Constellation ainsi qu'une clé d'accès pour pouvoir s'y connecter.

Ensuite on déclare les variables globales de notre script.
<pre class="lang:javascript decode:true">// Config
var matrixWidth = 8, matrixHeight = 8;
var constellation = $.signalR.createConstellationConsumer("http://x.x.x.x:8088/", "xxxxxxxxxxxxxx", "WebMatrixController"); 

// Global variables
var pixels = [];
var selectedColor = {};
var mouseDown = false;
var canvas, context;
var boxWidth, boxHeight;
</pre>
On ajoute une fonction "initBoard" pour dessiner notre grille dans le canvas HTML et initialiser le tableau "pixels" qui contiendra l'état de la matrice :
<pre class="lang:javascript decode:true">function initBoard() {
    // Init the pixels array
    pixels = [];
    for (var x = 0; x &lt;= matrixWidth; x++) {
        pixels[x] = [];
        for (var y = 0; y &lt;= matrixHeight; y++) {
            pixels[x][y] = 0;
        }
    }
    // Create the vertical lines
    for (var x = 0; x &lt;= matrixWidth; x++) {
        context.moveTo(1 + x * boxWidth, 0);
        context.lineTo(1 + x * boxWidth, canvas.height());
    }
    // Create the horizontal lines
    for (var x = 0; x &lt;= matrixHeight; x++) {
        context.moveTo(0, 1 + x * boxHeight);
        context.lineTo(canvas.width(), 1 + x * boxHeight);
    }
    // Draw lines
    context.lineWidth=1;
    context.strokeStyle = "black";
    context.stroke();
}
</pre>
On déclare ensuite une fonction "setPixel" qui se chargera à la fois de dessiner dans le canvas la couleur sélectionnée du pixel mais également d'envoyer un message à notre ESP8266 pour invoquer le MessageCallback "SetPixelColor" en spécifiant la position du pixel et la couleur sélectionnée. Le tableau "pixel" permet de n'envoyer le message que si la couleur a changé pour éviter d’envoyer plusieurs fois le même ordre.
<pre class="lang:javascript decode:true">function setPixel(x, y) {
    if(pixels[x][y] != selectedColor.color) {
        // Save pixel
        pixels[x][y] = selectedColor.color;
        // Draw on canvas
        context.fillStyle=selectedColor.color;
        context.fillRect(2 + boxWidth * x, 2 +  boxHeight * y, boxWidth - 2, boxHeight - 2);
        // SetPixelColor on ESP8266
        constellation.server.sendMessage({ Scope: 'Package', Args: ['MatrixDesign'] }, 'SetPixelColor', [ x, y, selectedColor.id ]);  
    }
}
</pre>
Pour l’interaction nous ajoutons trois méthodes :
<ul>
 	<li>moveStart : lorsque la sourie/le doigt (si touch) est posé sur la grille</li>
 	<li>move : lorsque la sourie/le doigt (si touch) se déplace sur la grille</li>
 	<li>moveEnd : lorsque la sourie/le doigt (si touch) "quitte" la grille</li>
</ul>
Dans les deux premières méthodes l'idée est de déduire la position du pixel en fonction des arguments de l'événement levé afin d'invoquer la méthode "setPixel" que nous avons déclarée ci-dessus qui se chargera à son tour de dessiner la bonne couleur dans le canvas tout envoyant le message à notre ESP8266.
<pre class="lang:javascript decode:true">function moveStart(e, mobile, obj) {
    mouseDown = true;
    // Determine the pixel coordinates
    var event = mobile ? e.originalEvent : e;
    var x = Math.trunc((event.pageX - obj.offsetLeft) / boxWidth);
    var y = Math.trunc((event.pageY - obj.offsetTop) / boxHeight);
    // Block mobile event
    if (mobile) {
        e.preventDefault();
    }        
    // Set pixel
    setPixel(x, y);
}

function move(e, mobile, obj) {
    if (mouseDown) {
        // Determine the pixel coordinates
        var pageX = mobile ? e.originalEvent.targetTouches[0].pageX : e.pageX;
        var pageY = mobile ? e.originalEvent.targetTouches[0].pageY : e.pageY;
        var x = Math.trunc((pageX - obj.offsetLeft) / boxWidth);
        var y = Math.trunc((pageY - obj.offsetTop) / boxHeight);
        // Block mobile event                
        if (mobile) {
            e.preventDefault();
        }     
        // Set pixel
        setPixel(x, y);
    }
}

function moveEnd() {
    mouseDown = false;
}
</pre>
Il ne reste plus qu'à écrire le code de démarrage de notre page.

On commence par initialiser nos variables globales (instance et taille du canvas, contexte de rendu 2D du canvas). On initialise ensuite la grille par notre méthode "<em>iniBoard</em>" et on se connecte à notre Constellation (<em>constellation.connection.start()</em>).

Pour chaque couleur de notre liste, on affecte la couleur du background définie par son attribut <em>data-color</em> et on ajoute un handler au click de façon à récupéré dans l'objet "selectedColor" la couleur sélectionnée par l'utilisateur.

On affecte également un handler au click du bouton "Clear" pour effacer le Canvas et invoquer le MessageCallback "<em>clear</em>" sur notre ESP8266 afin de vider la matrice.

Et pour finir on attache des handlers aux événements de la souris (<em>mouseDown</em>, <em>mouseMove</em> et <em>mouseUp</em>) et du tactile (<em>touchStart</em>, <em>touchMove</em> et <em>touchEnd</em>) sur nos tris méthodes ci-dessus pour gérer les interactions avec notre grille.
<pre class="lang:javascript decode:true">// On page load
$(document).ready(function() {
    // Variables
    canvas = $("#canvas");
    context = canvas[0].getContext('2d');
    boxWidth = (canvas.width() - 2) / matrixWidth;
    boxHeight = (canvas.height() - 2) / matrixHeight;
    // Init board
    initBoard();            
    // Constellation connection
    constellation.connection.stateChanged(function (change) {
        if (change.newState === $.signalR.connectionState.connected) {
            console.log("Je suis connecté !");
        }
    });                
    constellation.connection.start();            
    // For each color buttons
    $("#colors a").each(function() {
        // Set background color
        $(this).css("background", $(this).attr("data-color"));
        // Attach handler on click
        $(this).click(function() {
            // Load the selected color
            selectedColor.id = $(this).attr("data-colorId");
            selectedColor.color = $(this).attr("data-color");
            // Set the selected class
            $("#colors a").removeAttr("class", "");
            $(this).attr("class", "selected");
            return false;                    
        });
        // Load the selected color
        if( $(this).attr("class") == "selected") {
            selectedColor.id = $(this).attr("data-colorId");
            selectedColor.color = $(this).attr("data-color");
        }
    });            
    // Clear button
     $("#clear").click(function() {
        // Clear the matrix
        constellation.server.sendMessage({ Scope: 'Package', Args: ['MatrixDesign'] }, 'Clear', {});
        // Clear the canvas
        var s = document.getElementById ("canvas");
        var w = s.width;    
        s.width = 10;
        s.width = w;
        // Redraw the board
        initBoard();                
     });             
     // Attach Touch events
    canvas.bind('touchstart', function(e) { moveStart(e, true, this); });
    canvas.bind('touchmove', function(e) { move(e, true, this); });
    $(this).bind('touchend', function() { moveEnd(); });
    // Attach Mouse events
    canvas.mousedown(function(e) {  moveStart(e, false, this); });
    canvas.mousemove(function(e) { move(e, false, this); });
    $(this).mouseup(function() { moveEnd(); });
});
</pre>
Et voilà notre page HTML est prête ! Lancez-la dans notre navigateur et profitez du résultat.
<h3 align="left">Demos</h3>
Quelques démonstrations animées ....
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESP Matrix Controller" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/ESPMatrix1.gif" alt="ESP Matrix Controller" width="400" height="225" border="0" /></p>
<p align="center"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="ESPMatrix3" src="https://developer.myconstellation.io/wp-content/uploads/2017/05/ESPMatrix3.gif" alt="ESPMatrix3" width="400" height="225" border="0" /></p>