#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define  trig  D0
#define  echo  D1
long duration;
int distance;
char auth[] = "s6IOSSiHRs5wbIbQZUvJyCjlWMr24biL";
char ssid[] = "R&D LABS";   // your ssid
char pass[] = "12345678"; // your wifi pass
void send_event(const char *event);
const char *host = "maker.ifttt.com";
const char *privateKey = "c38AtQofh4odOXzvhEmoS3";
int n;
int carbon=D5;
BlynkTimer timer;
void setup()
{
  pinMode(trig, OUTPUT);  // Sets the trigPin as an Output
  pinMode(echo, INPUT);   // Sets the echoPin as an Inpu
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass);
  timer.setInterval(1000L, sendSensor);
}
void loop()
{
  Blynk.run();
  timer.run();
  void send_event(const char *event);
  n =analogRead(A0);
  carbon = digitalRead(D5);
  Serial.print("Methane:");        //Output distance on arduino serial monitor
  Serial.println(n);
  Serial.print("CO2:");        //Output distance on arduino serial monitor
  Serial.println(carbon);
  if (distance <= 15)
  {
    Blynk.tweet("My Arduino project is tweeting using @blynk_app and it’s awesome!\n #arduino #IoT #blynk");
    Blynk.notify("Post has been twitted");
  }
  if (distance <= 15) {
    send_event("jar_event");
  }
  Blynk.virtualWrite(V0, distance);
  Blynk.virtualWrite(V1, n);
  Blynk.virtualWrite(V2, carbon);
  delay(2000);
}
void sendSensor()
{
  digitalWrite(trig, LOW);   // Makes trigPin low
  delayMicroseconds(2);       // 2 micro second delay
  digitalWrite(trig, HIGH);  // tigPin high
  delayMicroseconds(10);      // trigPin high for 10 micro seconds
  digitalWrite(trig, LOW);   // trigPin low
  duration = pulseIn(echo, HIGH);   //Read echo pin, time in microseconds
  distance = duration * 0.034 / 2;   //Calculating actual/real distance
  Serial.print("Distance = ");        //Output distance on arduino serial monitor
  Serial.println(distance);
}
void send_event(const char *event)
{
  Serial.print("Connecting to ");
  Serial.println(host);
  WiFiClient client;
  const int httpPort = 80;
  if (!client.connect(host, httpPort)) {
    Serial.println("Connection failed");
    return;
  }
  String url = "/trigger/";
  url += event;
  url += "/with/key/";
  url += privateKey;
  Serial.print("Requesting URL: ");
  Serial.println(url);
  client.print(String("GET ") + url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" +
               "Connection: close\r\n\r\n");
  while (client.connected())
  {
    if (client.available())
    {
      String line = client.readStringUntil('\r');
      Serial.print(line);
    } else {
      // No data yet, wait a bit
      delay(50);
    };
  }
}
