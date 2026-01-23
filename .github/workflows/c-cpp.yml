#include <WiFiS3.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include "DHT.h"
#include <Servo.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);
DHT dht(2, DHT11); // DHT11 sensor on pin 2

char ssid[] = "WiFi_SSID";
char pass[] = "WiFi_PASSWORD";
WiFiServer server(80);

// Pin definitions
int ledPin = 9;        // PWM LED
int speakerPin = 8;    // Passive speaker (tone)
int servoPin = 6;
int potPin = A0;
int soilPin = A1;
int rainPin = A2;
int soundPin = A3;
int trigPin = 4;
int echoPin = 5;

Servo myServo;

void setup() {
  Serial.begin(9600);
  pinMode(ledPin, OUTPUT);
  pinMode(speakerPin, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  myServo.attach(servoPin);
  dht.begin();

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0,0);
  lcd.print("Connecting WiFi...");

  if(WiFi.begin(ssid, pass) == WL_CONNECTED){
    server.begin();
    lcd.clear();
    lcd.print("IP: ");
    lcd.print(WiFi.localIP());
  }
}

void loop() {
  // Sensor readings
  int potVal = analogRead(potPin);
  int ledVal = map(potVal, 0, 1023, 0, 255);
  analogWrite(ledPin, ledVal);

  int soilVal = analogRead(soilPin);
  int rainVal = analogRead(rainPin);
  int soundVal = analogRead(soundPin);
  float temp = dht.readTemperature();
  float hum = dht.readHumidity();

  // Ultrasonic distance
  digitalWrite(trigPin, LOW); delayMicroseconds(2);
  digitalWrite(trigPin, HIGH); delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH);
  int distance = duration * 0.034 / 2;

  // Web server
  WiFiClient client = server.available();
  if (client) {
    String request = client.readStringUntil('\r');
    client.flush();

    // LED control
    if (request.indexOf("/LED_ON") != -1) digitalWrite(ledPin, HIGH);
    if (request.indexOf("/LED_OFF") != -1) digitalWrite(ledPin, LOW);

    // Speaker control (880 Hz tone)
    if (request.indexOf("/SPEAKER_ON") != -1) tone(speakerPin, 880);
    if (request.indexOf("/SPEAKER_OFF") != -1) noTone(speakerPin);

    // Servo control
    if (request.indexOf("/SERVO_LEFT") != -1) myServo.write(0);
    if (request.indexOf("/SERVO_RIGHT") != -1) myServo.write(180);

    // LCD message
    if (request.indexOf("/LCD=") != -1) {
      int pos = request.indexOf("/LCD=");
      String msg = request.substring(pos+5);
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print(msg);
    }

    // HTML interface
    client.println("HTTP/1.1 200 OK");
    client.println("Content-type:text/html");
    client.println();
    client.println("<html><body>");
    client.println("<h1>UNO R4 WiFi Dashboard</h1>");
    client.println("<a href=\"/LED_ON\">LED ON</a> | <a href=\"/LED_OFF\">LED OFF</a><br>");
    client.println("<a href=\"/SPEAKER_ON\">SPEAKER ON (880Hz)</a> | <a href=\"/SPEAKER_OFF\">SPEAKER OFF</a><br>");
    client.println("<a href=\"/SERVO_LEFT\">SERVO LEFT</a> | <a href=\"/SERVO_RIGHT\">SERVO RIGHT</a><br>");
    client.println("<form action=\"/LCD=\" method=\"GET\">LCD: <input type=\"text\" name=\"msg\"><input type=\"submit\" value=\"Send\"></form><br>");
    client.println("<h2>Sensor Data</h2>");
    client.println("Potentiometer: " + String(potVal) + "<br>");
    client.println("Soil Moisture: " + String(soilVal) + "<br>");
    client.println("Rain Sensor: " + String(rainVal) + "<br>");
    client.println("Sound Sensor: " + String(soundVal) + "<br>");
    client.println("Temperature: " + String(temp) + " C<br>");
    client.println("Humidity: " + String(hum) + " %<br>");
    client.println("Distance: " + String(distance) + " cm<br>");
    client.println("</body></html>");
    client.stop();
  }
}
