# Air Quality Monitor
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

| **Engineer** | **School** | **Area of Interest**| **Grade** |
|:--:|:--:|:--:|:--:|
| Ari R | Frisch | Electrical Engineering | Incoming Sophomore

![Headstone Image](WhatsApp Image 2023-08-11 at 10.01.57 AM (1).jpeg)
  
# Final Milestone

  For my final milestone I added several modifcations to my project. Firstly, I added a buzzer that goes off when the air quality is very poor which gives a warning to the user that they should avoid being exposed to that air. Secondly, I added a 3 LEDs that correspond to the air quality. For example, if there is awful air quality then the red LED will go on but if there is good air then the green LED will go on. Previously, the imformation from the sensors was displayed on an OLED screen. However, My biggest modification was that I added a ESP8266 which is a wifi moduule. Incorporating a wifi module enabled me to send the imformation from all the sensors onto Blynk which is a webesite that displays the imformation that the sensors were reading. Such as, air quality, humidity, and temperature. This project, and BlueStamp as a whole as given me more experience with both hardware and software whether it was working on the code, buidling the circuit, setting up the sensors, or configuring Blynk. In the future I hope to expand my knowledge of these topics as well as gain more engineering experience.
<iframe width="560" height="315" src="https://www.youtube.com/embed/3n60GqFJ_KM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


# Second Milestone
  For my second milestone, I used a temperature and humidity sensor and an air quality monitor to sense the temperature, humidity, and air quality, and then display all of that information on an OLED screen. The main components of this milestone are the Arduino which is connected to the breadboard through jumper wires, a MQ135 which is an air quality sensor, and a DHT11 which is a temperature and humidity sensor. I was surprised when the OLED screen displayed all the information because it came from different sensors and I wasn’t sure how it would be able to display information from multiple sources. Before my final milestone, I might modify this project to include a housing for all the essential modules or incorporate wifi which will enable the user to read the data from the sensors online.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZiqZOsmDcI8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


# First Milestone
  For the 1st part of this project, I used a DHT11 which is a temperature and humidity sensor, and a MQ 135 which is an air quality monitor. After trial and error, I got the temperature and humidity monitor as well as the air quality monitor to print their measurements on the serial monitor. One challenge I encountered during this milestone was that it was difficult to get the DHT11 sensor to work and print simultaneously with the air sensor. Another issue I had was that the DHT11 monitor wasn’t printing the humidity and temperature. I solved that problem by reconnecting the wires and then uploading the code again.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QBXinuajtAc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


# Schematics 
![Headstone Image](Epic Gogo-Uusam (1).png)
# Code
```c++

#include <DHT.h> //including libraries and setting up variables
#define BLYNK_TEMPLATE_ID "TMPL2JF4CBDhH"
#define BLYNK_TEMPLATE_NAME "Quickstart Template"
#define BLYNK_AUTH_TOKEN "TokrsqDByGLi6pvNhXYA3sk7lByda0eh"
#define BLYNK_PRINT Serial
#define ESP8266_BAUD 9600
#include <ESP8266_Lib.h>
#include <BlynkSimpleShieldEsp8266.h>
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Rosenfeld8";
char pass[] = "Redbird8";
float roomHumidity = 0;
float roomTemperature = 0;
#include <SoftwareSerial.h>
SoftwareSerial EspSerial(2, 3); // RX, TX
ESP8266 wifi(&EspSerial);
BlynkTimer timer;
#define sensor A0
#define DHTPIN 7
#define DHTTYPE DHT11
int gasLevel = 0;
String quality = "";
DHT dht(DHTPIN, DHTTYPE);
int buzzerPin = 8;
float hum = 0;
float temp_f = 0;
float temp_c = 0;
void readDHT(){ //reading the DHT in celcius and then converting that number to farenheit
  hum = dht.readHumidity();
  temp_c = dht.readTemperature();
  temp_f =  temp_c * 1.8 + 32;
  if (isnan(hum) || isnan(temp_c)) {
    Serial.println("Failed  to read from DHT sensor!");
    return;
  }
  
}



void air_sensor() {
  //turning the LEDs off
  gasLevel = analogRead(sensor);
digitalWrite (13,LOW);
digitalWrite (12,LOW);
digitalWrite (11,LOW);
// making the gas level correlate to the air quality
  if (gasLevel < 181) {
    quality = "  GOOD!";
    digitalWrite(12, HIGH);
  } else if (gasLevel > 181 && gasLevel < 225) {
    quality = "  Poor!";
    digitalWrite(13, HIGH);
  } else if (gasLevel > 225 && gasLevel < 300) {
    quality = "Very Poor!";
  } else if (gasLevel > 300 && gasLevel < 350) {
    quality = "Awful!";
  } else {
    quality = " Toxic";
  }
  if  (gasLevel > 225) {
    digitalWrite(11, HIGH);
  }

}
// the buzzer plays if the gas level is very bad
  // if (gasLevel > 225) {
  //   tone(buzzerPin, 440);
  //   delay(1000);

  //   tone(buzzerPin, 494);
  //   delay(1000);

  //   tone(buzzerPin, 523);
  //   delay(1000);

  //   tone(buzzerPin, 587);
  //   delay(1000);

  //   tone(buzzerPin, 659);
  //   delay(1000);

  //   tone(buzzerPin, 698);
  //   delay(1000);

  //   tone(buzzerPin, 784);
  //   delay(1000);

  //   noTone(buzzerPin);
  //   delay(1000);
  
  
void myTimerEvent(){ //Setting up virtual pins and sensors in order to send that imformation to Blynk 
  air_sensor();
  readDHT();
  Blynk.virtualWrite(V0,quality);
  Blynk.virtualWrite(V1,hum);
  Blynk.virtualWrite(V2,temp_f);
  Blynk.virtualWrite(V3,temp_c);
  Blynk.virtualWrite(V4,gasLevel);



void setup() {
  Serial.begin(9600);// Setting up the Serial Monitor
//seting up inputs and  outputs 
  pinMode(sensor, INPUT); 
  pinMode(12,OUTPUT);
  pinMode(13,OUTPUT);
  pinMode(11,OUTPUT);
  //pinMode(buzzerPin, OUTPUT);
  dht.begin(); 
  EspSerial.begin(ESP8266_BAUD); // setting up the ESP8266 wifi module
  delay(10);
  Blynk.begin(auth, wifi, ssid, pass);
  timer.setInterval(1000L, myTimerEvent);
  

  
}

void loop() { // Initiates Blynk
    Blynk.run();
  timer.run();  
  
}

```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
|Arduino Uno |Electronics platform| $29.95 |<a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| MQ135 | Air quality sensor | $8.99 | <a href="https://www.amazon.com/MQ-135-Quality-Hazardous-Detection-Arduino/dp/B07ZZ61LQT/"> Link </a> |
|ESP8266 | Wifi Module | $6.59 | <a href="https://www.amazon.com/DIYmall-ESP8266-ESP-01S-Serial-Transceiver/dp/B00O34AGSU/"> Link </a> |
|DHT11| Temperature and humidity monitor | $2.00 | <a href="https://www.amazon.com/ESP8266-ESP-01-Temperature-Humidity-Module/dp/B0B37NK7VN/ref=sr_1_7?crid=3KWWBSPUJVLMN&keywords=dht11+1+piece&qid=1690554840&sprefix=dht11+1+piece%2Caps%2C87&sr=8-7/"> Link </a> |
|LED (X3)| Source of light | $0.0499| <a href="https://www.amazon.com/MCIGICM-Circuit-Assorted-Science-Experiment/dp/B07PG84V17/ref=sr_1_29?keywords=led&qid=1691963655&sr=8-29&th=1/"> Link </a> |
|Passive Buzzer| Plays noises | $0.799| <a href="[https://www.amazon.com/a15091400ux0103-Electronic-Mounting-Passive-Sounder/dp/B018I1WBNQ/ref=sr_1_3?crid=1RV07S9JFLHEK&keywords=passive+buzzer&qid=1691967166&sprefix=passive+buzze%2Caps%2C125&sr=8-3/"> Link </a> |
