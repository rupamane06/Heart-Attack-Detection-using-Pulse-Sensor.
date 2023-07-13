# Heart-Attack-Detection-using-Pulse-Sensor.
The proposed system continuously monitors the vital parameter, heartbeat, and concerned person by implanting chip on to the body. The chip performs a function of continuous sensing and comparing of sense data with standard threshold. If any abnormalities are present there then the buzzer will buzz. 


# ABSTRACT
Heart attack is a global leading cause for death among both genders men and women. However, occurrence of heart attack is not predictable. Currently there are number of health monitoring system available for patient, but all these system works mainly when there is emergency occurs. Also not capable of transmitting data continuously. The proposed system continuously monitors the vital parameter, heartbeat, and concerned person by implanting chip on to the body. The chip performs a function of continuous sensing and comparing of sense data with standard threshold. If any abnormalities are present there then the buzzer will buzz. The main aim is to develop a circuit for detecting a heart attack and if, so then alert to doctor and family members.

# INTRODUCTION

Rapid economic and industrial development leads to increased intensity in daily life, which brings people negative sentiments, such as nervousness, anxiety, and disturbance. These emotions along with changes of quickly lifestyle result that chronic cardiovascular disease becomes the major adult illness. Therefore, degenerative disease has increase rapidly and at the same time it has resulted in the increase in medical treatment cost. The main focus is to implement a model for the real time patient monitoring system. This project is useful in medical application and it has less cost and size. This is working model which incorporate sensor to measure parameter like heart beat rate and ECG; Arduino is used for analyzing the input from patient and any abnormality felt by the patient causes the monitoring system to give an alarm. In the case of emergency for people who are suffering with heart diseases continuous monitoring of the patient are required which is sometime not possible in the hospital.
The proposed system continuously monitors the vital parameter, heartbeat, ECG, and concerned person by implanting chip on to the body. The chips perform a function of continuous sensing and comparing of sense data with standard threshold. If any abnormalities are present there then buzzer will buzz loudly. The main aim is to develop a circuit for detecting a heart attack and if, so then alert to doctor and family members.


# BLOCK DIAGRAM WITH EXPLAINATION
 	 
![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/6f7130b2-4f79-4d75-a65f-1fc81b3649bf)


Fig :- Block diagram of heart attack detection using pulse sensor.

# Components required 
1.	Arduino Uno 
2.	Pulse sensor
3.	Buzzer
4.	Oled
5.	Jumping wires 
6.	PCB

# Pulse Sensor:
![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/cd06c58d-ca65-4e9f-aacd-17e7a6a548e9)

Fig :- Pulse sensor

The Pulse Sensor is a plug-and-play heart-rate sensor for Arduino. It can be used by students, artists, athletes, makers, and game & mobile developers who want to easily incorporate live heart-rate data into their projects. The essence is an integrated optical amplifying circuit and noise eliminating circuit sensor. Clip the Pulse Sensor to your earlobe or fingertip and plug it into your Arduino, you can ready to read heart rate. Also, it has an Arduino demo code that makes it easy to use.
The pulse sensor has three pins: VCC, GND & Analog Pin.
 
There is also a LED in the center of this sensor module which helps in detecting the heartbeat. Below the LED, there is a noise elimination circuitry that is supposed to keep away the noise from affecting the readings.

# 0.96″ I2C OLED Display:
This is a 0.96 inch blue OLED display module. The display module can be interfaced with any microcontroller using SPI/IIC protocols. It is having a resolution of 128×64. The package includes display board, display,4 pin male header pre-soldered to board.
![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/df9f5e85-8e9f-4cdc-8b8f-6af4028a394c)
 
Fig :- OLED

OLED (Organic Light-Emitting Diode) is a self light-emitting technology composed of a thin, multi-layered organic film placed between an anode and cathode. In contrast to LCD technology, OLED does not require a backlight. OLED possesses high application potential for virtually all types of displays and is regarded as the ultimate technology for the next generation of flat-panel displays.








# DETAILS OF DESIGN, WORKING AND PROCESSES
 
CIRCUIT DIAGRAM  
 
![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/3ee72a4c-530f-4ac3-9717-e09a44b58dad)
 
 
Fig :- Circuit diagram of heart attack detection using pulse sensor 

# WORKING & SETUP:

Pulse Sensor:
The pulse sensor works by emitting an Infra-Red signal from an IR-Diode onto the skin. Just underneath the skin, there are capillaries carrying blood. Every time heart pumps there is a small increase in blood flow/pressure. This swells the capillaries slightly, this slightly more filled capillary reflects more infra-red than at times when the heart is not giving your blood a “push”. An Infra-detector on the device senses the different reflected IR levels. Some simple comparator circuitry converts this into a voltage signal which we can read with the Arduino Analog inputs.

OLED 128×64 (SSD1306 Driver) display:
In this project, we’re adding an ECG waveform plotter along with BPM on the OLED display. For this we need 2 different libraries, i.e. SSD1306 Driver & GFX Library. Firstly ensure you’ve bought an OLED 128×68 I²C display (SSD1306 driver) display. It should have four connections, i.e. 5V, GND, SDA, and SCK.

OLED Address:
Since the OLED used here is an I2C OLED, so it has a particular device address. To find the I2C address first scans the OLED with the I2C Scanner program. Normally the OLED Display has a device address of 0x3C or 0X3D.



# CODE 
#include <Adafruit_SSD1306.h>
#define OLED_Address 0x3C // 0x3C device address of I2C OLED. Few other OLED has 0x3D
Adafruit_SSD1306 oled(128, 64); // create our screen object setting resolution to 128x64
 
int a=0;
int lasta=0;
int lastb=0;
int LastTime=0;
int ThisTime;
bool BPMTiming=false;
bool BeatComplete=false;
int BPM=0;
int buzz=8;
#define UpperThreshold 560
#define LowerThreshold 530
 
void setup() {
pinMode(buzz,OUTPUT);
oled.begin(SSD1306_SWITCHCAPVCC, OLED_Address);
oled.clearDisplay();
oled.setTextSize(2);
}
 
void loop()
{
if(a>127)
{
oled.clearDisplay();
a=0;
lasta=a;
}
 
ThisTime=millis();
int value=analogRead(0);
oled.setTextColor(WHITE);
int b=60-(value/16);
oled.writeLine(lasta,lastb,a,b,WHITE);
lastb=b;
lasta=a;
 
if(value>UpperThreshold)
{
if(BeatComplete)
{
BPM=ThisTime-LastTime;
BPM=int(60/(float(BPM)/1000));
BPMTiming=false;
BeatComplete=false;
tone(8,1000,250);
}
if(BPMTiming==false)
{
LastTime=millis();
BPMTiming=true;
}
}
if((value<LowerThreshold)&(BPMTiming))
BeatComplete=true;
 
oled.writeFillRect(0,50,128,16,BLACK);
oled.setCursor(0,50);
oled.print("BPM:");
oled.print(BPM);
 
oled.display();
a++;

if(BPM>200)
{
 digitalWrite(buzz,HIGH);
}
else
{
 digitalWrite(buzz,LOW);
}

} 
 



 
# RESULTS
![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/de98d0f1-7eec-450b-b702-8328e229a983)

![image](https://github.com/rupamane06/Heart-Attack-Detection-using-Pulse-Sensor./assets/139439712/c296e6b2-1a76-4f99-b6ba-18e4ea5462c0)
 

# APPLICATIONS

•	Chest-band devices: Because they use electrical detection, chest-band monitors are the most accurate, especially when used properly. That’s because they measure your heart rate directly — rather than your pulse rate — which gives them higher accuracy regardless of whether you’re resting, running, cycling or using various exercise devices.
•	Wrist- or forearm-located wearables: These tend to be very accurate when you’re resting or walking. Many of these devices are also very accurate if you’re running or cycling. Using your arms for exercise activities — like with an elliptical that has hand levers to work your arms — can cause inaccurate readings.
•	Smart rings: These devices are very new, so there aren’t many commercially available yet. Still, available research already indicates these are very accurate while you’re resting. More research is necessary to show if they’re accurate for exercise and other activities
•	Pulse oximeters: In medical settings, these attach or stick to your finger with adhesive and help healthcare providers with certain types of tests. Pulse oximeters found in non-medical settings aren’t usually suitable for use during exercise.
•	Smartphones: Smartphone apps that have you touch the camera lens as part of the measurement tend to be more accurate than the apps that use the camera to scan your face. However, they’re still prone to errors because the phone and its camera weren’t designed for this purpose.

 # CONCLUSION  

We designed health monitoring system for patients who are suffering from heart diseases. In this project, we have design heart rate and ECG and then a buzzer buzzes to alert the patient. It is portable and cost efficient system and very easy to handle and provide great flexibility and serves as a great improvement over other conventional monitoring and alert system. We believe that our system is a step towards promoting patient‘s autonomy and by providing Personalized monitoring and advice we hope that it will give the patients more confidence and improve their quality of life.

# FUTURE SCOPE:
The abnormalities which are detected can be sent to the doctor or to the family members so that they can help the patient in time with help of GSM module.
The system can be used to measure the physical parameters like body temperature, detect fall with the help of biosensors using Arduino microcontroller and to trace the location of patient using GPS.

# REFERENCES
 
https://how2electronics.com/pulse-sensor-with-oled-arduino/ 
https://edubirdie.com/examples/heart-attack-detection-by-using-arduino-uno/ 
 









                                                                                                                                                                                                                                                                                                                                                                                                                                                                        

              
     





