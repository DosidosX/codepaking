#include <WiFi.h>
#include <PubSubClient.h>
#define MQTT_SERVER "driver.cloudmqtt.com"
#define MQTT_PORT 18989
#define MQTT_USERNAME "eophnhik"
#define MQTT_PASSWORD "TEOUOc5d5u7u"
#define MQTT_NAME "parkingproject"
#define LED_PIN 23
#define LED_ONBOARD 5
WiFiClient client;
PubSubClient mqtt(client);
void callback(char* topic, byte* message, unsigned int length) {
  Serial.print("Message arrived on topic: ");
  Serial.print(topic);
  Serial.print(". Message: ");
  String messageTemp;
  
  for (int i = 0; i < length; i++) {
    Serial.print((char)message[i]);
    messageTemp += (char)message[i];
  }
  Serial.println();

  // Feel free to add more if statements to control more GPIOs with MQTT

  // If a message is received on the topic esp32/output, you check if the message is either "on" or "off". 
  // Changes the output state according to the message
  if (String(topic) == "parkingproject") {
    Serial.print("Changing output to ");
    if(messageTemp == "เปิดไฟ"){
      Serial.println("เปิดไฟ");
      digitalWrite(LED_PIN, HIGH);
    }else{
      Serial.println("ปิดไฟ");
      digitalWrite(LED_PIN, LOW);
    }
  }
}
void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  pinMode(LED_ONBOARD, OUTPUT);
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  WiFi.mode(WIFI_STA);
  WiFi.begin("Wokwi-GUEST", "");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
    digitalWrite(LED_ONBOARD, !digitalRead(LED_ONBOARD));
  }
  digitalWrite(LED_ONBOARD, HIGH);
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  mqtt.setServer(MQTT_SERVER, MQTT_PORT);
  mqtt.setCallback(callback);
}
void loop() {
  if (mqtt.connected() == false) {
    Serial.print("MQTT connection... ");
    if (mqtt.connect(MQTT_NAME, MQTT_USERNAME,
                     MQTT_PASSWORD)) {
      Serial.println("connected");
      mqtt.subscribe("parkingproject");
    } else {
      Serial.println("failed");
      delay(5000);
    }
  } else {
    mqtt.loop();
  }
}
