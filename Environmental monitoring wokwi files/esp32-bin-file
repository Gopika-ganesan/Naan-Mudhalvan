#include <WiFi.h>
#include "DHTesp.h"
#include "ThingSpeak.h"

const int DHT_PIN = 15;
const int LDR_PIN = 34; 
const int PIR_PIN = 5;  
const char* WIFI_NAME = "Wokwi-GUEST"; 
const char* WIFI_PASSWORD = ""; 
const int myChannelNumber = 2321864;
const char* myApiKey = "3LND9Z733HTLPCAW";
const char* server = "api.thingspeak.com";

DHTesp dhtSensor;
WiFiClient client;

void setup() {
  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(PIR_PIN, INPUT);  
  WiFi.begin(WIFI_NAME, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED){
    delay(1000);
    Serial.println("WiFi not connected");
  }
  Serial.println("WiFi connected!");
  Serial.println("Local IP: " + WiFi.localIP().toString());
  ThingSpeak.begin(client);
}

void loop() {
  TempAndHumidity data = dhtSensor.getTempAndHumidity();
  int ldrValue = analogRead(LDR_PIN);
  int pirValue = digitalRead(PIR_PIN);  
  ThingSpeak.setField(1, data.temperature);
  ThingSpeak.setField(2, data.humidity);
  ThingSpeak.setField(3, ldrValue); 
  ThingSpeak.setField(4, pirValue); 

  int x = ThingSpeak.writeFields(myChannelNumber, myApiKey);

  Serial.println("Temperature: " + String(data.temperature, 2) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("LDR Sensor : " + String(ldrValue));
  Serial.println("PIR Sensor : " + String(pirValue));

  if (x == 200){
    Serial.println("Data provided successfully");
  } else {
    Serial.println("No Data Provided " + String(x));
  }
  

  delay(10000);
}
