---
layout: post
title: KELSE-Y
description:  Worked with a team of engineers to design, manufacture, and document a life-support system intended to support life on board a rocket. 
skills: 
- CAD
- SOLIDWORKS
- FEA
- FMEA
- Documentation
main-image: /kelsey.png
---

**Project Type** - Space Technologies and Rocketry

**Collaborators** - Adnan Kapadia, Andrew Wu, Tristan Steen, Aarabhi Achanta, Aidan Rickert

The Kinetically Engineered Life Support Experiment - Yeast (KELSE-Y) is a life support system prototype designed to support life on a rocket. The biological subject used in KELSE-Y is yeast, and the system contains a pulmonary component and a cardiovascular component that mirror modern life support systems.

The yeast in KELSE-Y is mixed with sugar and warm water, causing it to begin fermentation, which causes production of carbon dioxide. The pH changes this carbon dioxide causes in water are measured by the system before and after it moves past a semi-permeable membrane to allow the carbon dioxide to diffuse into the atmosphere.

KELSE-Y was the payload flown on Caldera, STAR’s 2 stage separation rocket, at IREC 2024.

## Structure
KELSE-Y follows the CubeSat form factor. The stack is 50 cm tall with a 10 cm by 10 cm footprint (5U). This modular form factor makes integration simpler and aided in our manufacturing processes. The frame of the stack is composed of 20 mm square aluminum T-rails, screwed together with L-brackets using sliding nuts that fit between the grooves in the T-rails. The top and bottom of the stack are connected with treated wood blocks of dimensions 4 cm by 6 cm. The wood blocks allow mounting of the pump, along with the interface for integration. 

{% include image-gallery.html images="kelsey-cad.png, post-launch.png" height="400" caption="CAD render of KELSE-Y's stack structure and the assembled structure after launch and recovery at IREC 2024." %}

## Airframe Integration
The integration of the payload into the airframe consists of four threaded rods that thread into the lower wood block inside the Payload stack. These rods run through a removable bulkhead that is secured using two nuts on each rod . The stack and bulkhead are slotted into the rocket’s payload tube and screwed into a permanent bulkhead using eight 10-24 screws. Integration takes less than ten minutes and will not compromise the stability of the stack.

## Flow System

### Cardiovascular Component
KELSE-Y’s cardiovascular component features a peristaltic pump, which circulates the sugar water solution through polyurethane tubing connected across our components using push-to-connect (PTC) fittings. The yeast is stored and reacts in a thermos in order to maintain the best temperatures for fermentation. 

### Pulmonary Component
KELSE-Y’s pulmonary component is a flow cell that houses a 100 μm thick polydimethylsiloxane (PDMS) membrane. This membrane is permeable to carbon dioxide gas but not to any form of liquid. The carbon dioxide produced through fermentation of the yeast is pumped past this membrane in the sugar water solution and diffuses out of the system. 

The flow cell’s structure is comprised of CNC machined acetal copolymer, and a 3D printed mesh bracket provides air exposure to the membrane. The manifold that handled the flow of water in and out of the flow cell was sealed with custom-made O-rings, as was the flow cell itself. A pH sensor is placed on either side of the flow cell to measure carbon dioxide levels in the water. The differences in pH give clear signs about the diffusion performance of the system.

{% include image-gallery.html images="pH.png, flow-cell.png" height="400" caption="CAD renders of the pH sensor and flow cell subassemblies." %}

### Activation
KELSE-Y was designed to autonomously activate at launch. After the power-on and arming sequence, the avionics will enter a standby mode with no data being recorded apart from acceleration. If an acceleration stronger than 5 G is detected, the avionics will switch on the peristaltic pump and the syringe pump (which has a separately enabled LDO). The syringe pump will push in the sugar syringe, then KELSEY’s avionics will shut off the syringe LDO after 1 minute as a stall overcurrent failsafe and to reduce further power usage. The peristaltic pump will remain powered on until power loss, and pH data is constantly recorded, with COTS DAQs amplifying sensor voltage to usable levels for the ESP32’s analog inputs.

## Filling Operations
Two points in KELSE-Y’s loop contain T-fittings that are used for filling. Barbs are attached to the fittings, one to an external filling pump and another to an empty container. 

The filling pump, which draws from an external water source, is turned on, along with the avionics of KELSE-Y. Water is pumped into the system until it is filled. The system will be rotated as needed to remove air bubbles.

All electronics are then turned off and the barbs in the T-fittings are disconnected from fill lines and plugged. Leak tests are then performed to ensure that the stack is ready for launch.

## Avionics
A SRAD avionics system (KELSE-Y Avionics) turns on the pump when activated and opens the valve to release water into the system. After the water is in the system, the valve is closed. The pump remains on and sensor data is logged for the duration of the flight. Payload Avionics are integrated on a PCB. Circuitry is mainly controlled by an ESP32-S3 microcontroller. Up to 6 inductive loads, solenoid valves and pumps mainly, can be individually driven using MOSFETs (the MOSFET chip BTS3080EJ was used). Each individual MOSFET is rated for 3A while the loads draw about 0.5A in steady state; double physical redundancy for both MOSFETs and flyback diodes (SK56A-LTP, max. reverse voltage = 60V) ensures reliability. Screw terminals are included to connect sensors and actuators. Power provided by nickel-metal hydride batteries at 3.3V for logic and at 12V for power electronics.

See [Payload Demo Code](#payload-demo-code) below.

## Testing
Our testing for KELSE-Y centered largely around ensuring the system would hold pressure, even during the stresses caused by flight. Air bubbles that were introduced during the filling process caused major leakage and pressure issues, so KELSE-Y’s filling procedures were designed to minimize air in the system. Originally, KELSE-Y used solenoids opening to start testing, but this was not power efficient or reliable, so a new syringe-injection system was developed and implemented successfully. Many types of adhesive were tested for the acetal parts, as it was tricky to bond them to anything. The final bonding material we used was a strong epoxy.

## Simulations
Flow analysis simulations were completed within SOLIDWORKS fluid analysis. A simulation across the flow cell was conducted, revealing a pressure drop of about 400 Pa across the flow cell for the pump’s volumetric flow rate of 500 ml/min as an inlet condition and an outlet condition of atmospheric pressure. The simulation results are pictured below.

Another flow simulation was done for the pH sensor housings with the same boundary conditions, which revealed a smaller pressure loss of approximately 40 Pa.

{% include image-gallery.html images="ph-sim.png, flow-cell-sim.png" height="400" caption="SOLIDWORKS simulations of fluid flow through pH sensor and flow cell." %}

## Manufacturing
KELSE-Y’s stack structure contains a combination of COTS and manufactured parts. The T-rails and wood blocks that form the stack structure were COTS, but were modified in student machine shops. T-rails were cut down to 4 cm by 50 cm pieces, and 4 cm by 6 cm pieces using a bandsaw equipped with a metal cutting blade. The wood blocks were cut using a table saw, and were sanded down to nominal sizes using a belt sander and sandpaper. Holes were drilled using a hand drill, and the holes for the threaded rods were tapped for threading.

Many of KELSE-Y’s components were 3D printed. The pH sensor housings and flow cell manifolds were printed with SLA printers, as they needed high dimensional accuracy and waterproof material. The mounting brackets for the flow cell were printed with Onyx filament on a Markforged X7 for increased structural strength. 

The flow cell body was CNC machined out of acetal copolymer in order to achieve precise cuts. This material was also used for spacers to interface with the 3D printed parts. Finding an adhesive to seal these connections was difficult, but Permabond TA4611 acrylic epoxy ended up being our final choice. 

The pH sensor mounting brackets were cut out of eighth-inch thick aluminum for rigidity.

*This summary was adapted from the [Caldera Final Report](https://docs.google.com/document/d/15UWt7Nx078HOP_l3uSlkQTRXH3hPgQmj_iYFd7CG2FA/edit?usp=sharing) submitted for IREC 2024.*

### Payload Demo Code

```c++
#include <ESP32Servo.h>


#include <Arduino.h>
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>

#include <Adafruit_SPIFlash.h>
#include <Wire.h>
#include <SPI.h>

Adafruit_MPU6050 mpu;

#define BAT_ADC 1
#define SERVO_PWM 2
#define pH1 4
#define pH2 5
#define ACCELSDA 6
#define ACCELSCL 7
#define SDMISO 11
#define SDMOSI 12
#define SDCS 13 //can be always on (low)
#define SDCLK 14
#define MOT1EN 15
#define MOT2EN 16
#define STATUS 21

const float ADC_MAX_VALUE = 4095.0; // 12-bit ADC
const float VREF = 3.3; // Reference voltage
unsigned long previousMillis = 0;
const unsigned long motorDelay = 1000; // Time delay in milliseconds
bool motor1State = false;
bool motor2State = false;

bool thresholdExceeded = false;

Servo myservo; 

int pos = 0;    // variable to store the servo position
// Recommended PWM GPIO pins on the ESP32 include 2,4,12-19,21-23,25-27,32-33 
int servoPin = 2;
int servoAngle = 0;
bool servoState = false;



void setup() {

  pinMode(STATUS, OUTPUT);
  pinMode(MOT1EN, OUTPUT); //pump
  pinMode(MOT2EN, OUTPUT); //servo

  digitalWrite(STATUS,LOW);
  delay(100);
  digitalWrite(STATUS,HIGH);
  Serial.begin(115200);
  Wire.begin(ACCELSDA,ACCELSCL);
  while (!Serial)
    delay(10);

  Serial.println("IREC24 Payload Test!");

  if (!mpu.begin()) {
    Serial.println("Failed to find MPU6050 chip");
    while (1) {
      delay(10);
    }
  }
  Serial.println("MPU6050 Found!");

  mpu.setAccelerometerRange(MPU6050_RANGE_8_G);
  Serial.print("Accelerometer range set to: ");
  switch (mpu.getAccelerometerRange()) {
  case MPU6050_RANGE_2_G:
    Serial.println("+-2G");
    break;
  case MPU6050_RANGE_4_G:
    Serial.println("+-4G");
    break;
  case MPU6050_RANGE_8_G:
    Serial.println("+-8G");
    break;
  case MPU6050_RANGE_16_G:
    Serial.println("+-16G");
    break;
  }
  mpu.setGyroRange(MPU6050_RANGE_500_DEG);
  Serial.print("Gyro range set to: ");
  switch (mpu.getGyroRange()) {
  case MPU6050_RANGE_250_DEG:
    Serial.println("+- 250 deg/s");
    break;
  case MPU6050_RANGE_500_DEG:
    Serial.println("+- 500 deg/s");
    break;
  case MPU6050_RANGE_1000_DEG:
    Serial.println("+- 1000 deg/s");
    break;
  case MPU6050_RANGE_2000_DEG:
    Serial.println("+- 2000 deg/s");
    break;
  }

  mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);
  

 myservo.attach(servoPin); 
  pinMode(BAT_ADC, INPUT);
  Serial.println("");
  delay(1000);
  
  digitalWrite(STATUS,LOW);


  digitalWrite(MOT1EN, LOW);
  digitalWrite(MOT2EN, LOW);
}

void loop() {

  /* Get new sensor events with the readings */
  sensors_event_t a, g, temp;
  mpu.getEvent(&a, &g, &temp);


   float accelerationMagnitude = sqrt(a.acceleration.x * a.acceleration.x +
                                     a.acceleration.y * a.acceleration.y +
                                     a.acceleration.z * a.acceleration.z);

    Serial.print("Acceleration: ");
  Serial.print(accelerationMagnitude);
  Serial.println(" m/s^2");
  // Check if acceleration exceeds 50 m/s^2
   if (accelerationMagnitude > 20.0) {
    // Set the threshold exceeded flag
    thresholdExceeded = true;
  }
 
  if (thresholdExceeded) {
    // Activate the specified pins
      // Read pH sensor 1
  int pH1Value = analogRead(pH1);
  float pH1Voltage = (pH1Value / ADC_MAX_VALUE) * VREF;
  Serial.print("pH Sensor 1 Voltage: ");
  Serial.print(pH1Voltage);
  Serial.println(" V (RAW)");

  // Read pH sensor 2
  int pH2Value = analogRead(pH2);
  float pH2Voltage = (pH2Value / ADC_MAX_VALUE) * VREF;
  Serial.print("pH Sensor 2 Voltage: ");
  Serial.print(pH2Voltage);
  Serial.println(" V (RAW)");

  
  Serial.print("Temperature: ");
  Serial.print(temp.temperature);
  Serial.println(" degC");

  digitalWrite(MOT2EN,HIGH);
    Serial.println("Servo Engaged");

  for (pos = 100; pos <= 180; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(10);                       // waits 15ms for the servo to reach the position
  }
    Serial.println("Pump Enabled");

  digitalWrite(MOT1EN,HIGH);

  delay(5000); // delay 5 seconds for pump
  digitalWrite(MOT1EN,LOW);
      Serial.println("Pump Disabled");


  for (pos = 180; pos >= 100; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(10);                       // waits 15ms for the servo to reach the position
  }
      Serial.println(" Servo Disengaged");

  //PUMP NEVER ENABLES!!!
  thresholdExceeded=false; //REMOVE FOR TEST, MAKES IT ONLY 1 DEMO
  } else {
    // Deactivate the specified pins

  }

  delay(50); //delay required for accel to register x 2
}
```