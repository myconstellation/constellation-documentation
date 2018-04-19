---
ID: 3994
post_title: 'Etre alerté d&rsquo;un SMS avec une LED et un Arduino/ESP'
author: Sebastien Warin
post_date: 2016-12-21 14:28:54
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/tutorials/etre-alerte-dun-sms-avec-led-arduino-esp/
published: true
publish_post_category:
  - "10"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 07:14:20
---
Dans ce tutoriel, nous allons découvrir comment faire clignoter une LED branchée à un ESP8266 dès lors qu’on a un SMS non lu, en clair un “Alerteur d’SMS connecté" !
<h3>Prérequis</h3>
Pour réaliser ce tutoriel, vous aurez besoin :
<ul>
 	<li>D’un ESP8266</li>
 	<li>D’une LED (et sa résistance)</li>
 	<li>Du package <a href="/package-library/pushbullet/">PushBullet</a></li>
 	<li>D’un smartphone Android</li>
</ul>
<h3>Configuration</h3>
// A venir ..

&nbsp;
<h3>Montage</h3>
// A venir ..

&nbsp;
<h3>Code</h3>
// A venir ..

&nbsp;
<pre class="lang:csharp decode:true">#include &lt;Constellation.h&gt;
#include &lt;ESP8266WiFi.h&gt;

char ssid[]     = "MON-RESEAU-WIFI";
char password[] = "clé WIfi ici !!!!";

Constellation&lt;WiFiClient&gt; constellation("serveur.constellation.lan", 8888, "ESP8266", "SMSNotifier", "AccessKeyIci");

bool blinkLed = false;
int ledState = LOW;
unsigned long previousMillis = 0;
const int ledPin = D5; // ou LED_BUILTIN;
const long interval = 1000;

void setup(void) {
  // Init Serial
  Serial.begin(115200);  delay(10);

  // Init LED
  pinMode(ledPin, OUTPUT);

  // Connecting to Wifi
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("WiFi connected. IP: ");
  Serial.println(WiFi.localIP());

  // Register the MessageCallback
  constellation.registerMessageCallback("ReceiveEphemeral",
  [](JsonObject &amp; json) {
    String type = String(json["Data"]["type"].asString());
    if (type == "sms_changed") {
      Serial.println("New SMS received");
      blinkLed = true;
    }
    else if (type == "dismissal") {
      Serial.println("SMS readed");
      blinkLed = false;
    }
  });

  // Subscribe to PushBullet group
  constellation.subscribeToGroup("PushBullet");

  // Ready
  constellation.writeInfo("SMS Notifier started !");
}

void driveLed() {
  if (blinkLed) {
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis &gt;= interval) {
      previousMillis = currentMillis;
      ledState = (ledState == LOW) ? HIGH : LOW; // Switches led state
    }
  }
  else ledState = LOW;
  digitalWrite(ledPin, ledState);
}

void loop(void) {
  constellation.loop();
  driveLed();
}
</pre>
// A venir ..

&nbsp;
<h3>Conclusion</h3>
// A venir ..