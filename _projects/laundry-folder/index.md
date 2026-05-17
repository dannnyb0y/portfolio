---
layout: post
title: Laundry Folder Machine
description:  Designed and built a prototype device capable of folding clothes for the user using a relay of motors and Arduino microcontrollers. Developed as part of UC Berkeley's ME102B (Mechatronics Design) course.
skills: 
- CAD
- Onshape
- C++
- Arduino
- Breadboarding
- Engineering Drawing
- Lathe
- Laser Cutting
- 3D Printing
- Technical Presentation
- Project Management
main-image: /folder.jpeg
date: 2025-12-16
---

<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

**Project Type** - UC Berkeley

**Collaborators** - [Kevin Sengsourichanh](https://www.linkedin.com/in/kevin-sengsourichanh-4a4743217/), [Kari Martinez-Espindola](https://www.linkedin.com/in/karista-espindola/)

## Opportunity
Course projects for ME102B are not just about design and prototyping, but about recognizing and solving a problem. This laundry folder was designed to aid anyone with mobility or dexterity issues in folding clothes. Similar devices are on the market but are either expensive or not fully automatic. 

## Design
This device uses five panels that each rotate through a 180-degree arc in sequence to fold a shirt. The user places a shirt down across the five panels and then presses a button to activate the machine. The panels fold over one by one to fold the shirt, with the fifth panel acting as a ‘deposit’ panel to drop the folded shirt into a basket or hamper. The device will not run at the button press unless a potentiometer is turned past an activation threshold. Our initial desired functionality was to fold a shirt successfully, but we were also able to design a device that can fold shorts, pants, and hoodies, along with depositing the article of clothing outside the machine once folded.

{% include image-gallery.html images="annotated.png" height="400" %}

## CAD Photos


### Torque Analysis

The main constraint in the design was selecting a motor with enough torque to rotate these panels. The needed torque was calculated as follows:

$$\textbf{Plywood Density} = 680 \frac{\textbf{kg}}{\textbf{m}^3}$$

$$\textbf{PLA Density} = 1240 \frac{\textbf{kg}}{\textbf{m}^3}$$

Assume all weight at end for factor of safety. Ignore mass of axle, as it is about the axis of rotation. Total volume of plywood is $$0.000223$$ m$$^3$$, and total volume of PLA is $$0.00000773$$ m$$^3$$. Total mass $$M_{tot}$$ is then:

$$M_{tot} = 680\frac{\textbf{kg}}{\textbf{m}^3} \cdot 0.000223 \textbf{m}^3 + 1240\frac{\textbf{kg}}{\textbf{m}^3} \cdot 0.00000773\textbf{m}^3 = 0.161 \textbf{kg}$$

Panel length $$l$$ is $$24.5 \textbf{in} = 0.6223 \textbf{m}$$.
Torque needed by motor is then:

$$0.161 \textbf{kg} \cdot 9.81 \frac{\textbf{m}}{\textbf{s}^2} \cdot 0.6223 \textbf{m} = 0.983 \textbf{N} \cdot \textbf{m}$$

Round to 1.5 N$$\cdot$$m to account for the weight of the shirt and for a safety factor.

After discussing with machine shop staff, we were lended a set of motors each capable of meeting this torque spec. The rest of the components were designed around the available manufacturing options for our team - laser cutting, basic machining, and 3D printing. 

## Electronics and Software
This device was programmed in C++ using the Arduino IDE. Each of the motors was controlled by an ESP32. 

{% include image-gallery.html images="circuit.png" height="400" caption="Circuit diagram for the device."%}

See [Laundry Folder Code](#code) below.

---

*This summary was adapted from the [Final Report](https://docs.google.com/document/d/1Q8-mAUrHsxxh8q5Bb3gG4dTP_dOAcJHgd8cTut5wHz4/edit?usp=sharing) written for ME102B.*

*The full CAD document can be viewed [here](https://cad.onshape.com/documents/7ef667d42e2f7cedd0cd0583/w/68efd183ed38f5b916108f58/e/3e34a5b84dbeeddc8ce5f76b).*

---

### Code

This first block of code was for the ESP32 responsible for handling the button press and communicating with the ESP32s controlling the motors.

```c++
#include <Arduino.h>
#include <ESP32Encoder.h>


#define PWM_PIN 26
#define DIR_PIN 25
#define LED_PIN 13
#define ENC_A   27
#define ENC_B   33
#define BTN_PIN 32
#define SEND_PIN 4
#define POT_PIN 36


ESP32Encoder encoder;


enum CaseState {
 NOT_FOLDING,
 FOLDING
};


CaseState caseState = NOT_FOLDING;
const int POT_THRESHOLD  = 2000;
const int SPEED_PWM = 90;        
const long TARGET_COUNTS = 700;  
volatile bool buttonIsPressed      = false;
volatile unsigned long lastPress   = 0;
const unsigned long debounceTimeMs = 200;


bool CheckForButtonPress();
void led_on();
void led_off();


void IRAM_ATTR isr() {
 unsigned long now = millis();
 if (now - lastPress > debounceTimeMs) {  
   buttonIsPressed = true;
   lastPress = now;
}
}


void sendPulseToReceiver() {
 digitalWrite(SEND_PIN, HIGH);
 delay(10);                
 digitalWrite(SEND_PIN, LOW);
}
void setup() {
 Serial.begin(115200);
 pinMode(LED_PIN, OUTPUT);
 pinMode(DIR_PIN, OUTPUT);
 pinMode(PWM_PIN, OUTPUT);
 pinMode(POT_PIN, INPUT);
 analogWrite(PWM_PIN, 0);
 encoder.attachHalfQuad(ENC_A, ENC_B);
 encoder.setCount(0);
 pinMode(BTN_PIN, INPUT_PULLDOWN); 
 attachInterrupt(BTN_PIN, isr, RISING);
 pinMode(SEND_PIN, OUTPUT);
 digitalWrite(SEND_PIN, LOW);
 Serial.println("State: CASE 1 - NOT FOLDING");
 led_off();
}


void loop() {
 switch (caseState) {
   case NOT_FOLDING: {
     int potValue = analogRead(POT_PIN);
     if (CheckForButtonPress()) {
       if (potValue > POT_THRESHOLD) {
         Serial.println("Allowed to Fold");
         Serial.println("FOLDING");
         led_on();
         caseState = FOLDING;
       } else {
         Serial.print("Not allowed to fold, turn potentiometer");
       }
     }
     break;
   }
  case FOLDING: {
     Serial.println("[CASE 2: FOLDING]");
     Serial.println("Rotating CW");
     encoder.setCount(0);
     digitalWrite(DIR_PIN, HIGH);   // CCW
     analogWrite(PWM_PIN, SPEED_PWM);
     while (abs((long)encoder.getCount()) < TARGET_COUNTS) {
       delay(1);
     }
     analogWrite(PWM_PIN, 0);
     Serial.println("CW complete");
     Serial.println("Rotating CCW");
     encoder.setCount(0);
     digitalWrite(DIR_PIN, LOW);    // CW
     analogWrite(PWM_PIN, SPEED_PWM);
     while (abs((long)encoder.getCount()) < TARGET_COUNTS - 50) {
       delay(1);
     }
     analogWrite(PWM_PIN, 0);
     Serial.println("CCW complete");
     Serial.println("Returning to CASE 1: NOT FOLDING");
     sendPulseToReceiver(); //send signal to next esp
     caseState = NOT_FOLDING;
     led_off();
   } break;
 }
}


bool CheckForButtonPress() {
 if (buttonIsPressed) {
   buttonIsPressed = false;
   return true;
 }
 return false;
}


void led_on() {
 digitalWrite(LED_PIN, HIGH);
}
void led_off() {
 digitalWrite(LED_PIN, LOW);
}
```

This block of code was running on the ESP32s controlling the motors.

```c++
#include <Arduino.h>
#include <ESP32Encoder.h>


#define PWM_PIN 26          // MD13S PWM pin
#define DIR_PIN 25          // MD13S DIR pin
#define LED_PIN 13        
#define ENC_A   27          // Encoder A
#define ENC_B   33          // Encoder B
#define RECV_PIN 14         // Signal from sender ESP32
#define SEND_PIN 4          // send signal pin


ESP32Encoder encoder;


enum CaseState {
 NOT_FOLDING,
 FOLDING
};


CaseState caseState = NOT_FOLDING;




const int SPEED_PWM = 128;         // speed
const long TARGET_COUNTS = 600;   // encoder desired counts
volatile bool buttonIsPressed      = false;
volatile unsigned long lastPress   = 0;     
const unsigned long debounceTimeMs = 100;


bool CheckForButtonPress();
void led_on();
void led_off();


void IRAM_ATTR isr() {
 unsigned long now = millis();
 if (now - lastPress > debounceTimeMs) {  
   buttonIsPressed = true;
   lastPress = now;
 }
}


void sendPulseToReceiver() {
 digitalWrite(SEND_PIN, HIGH);
 delay(10);                
 digitalWrite(SEND_PIN, LOW);
}


void setup() {
 Serial.begin(115200);
 pinMode(LED_PIN, OUTPUT);
 pinMode(DIR_PIN, OUTPUT);
 pinMode(PWM_PIN, OUTPUT);
 analogWrite(PWM_PIN, 0);             
 encoder.attachHalfQuad(ENC_A, ENC_B);
 encoder.setCount(0);
 pinMode(RECV_PIN, INPUT_PULLDOWN);
 attachInterrupt(RECV_PIN, isr, RISING);
 pinMode(SEND_PIN, OUTPUT);
 digitalWrite(SEND_PIN, LOW);
 Serial.println("State: CASE 1 — NOT FOLDING");
 led_off();
}


void loop() {
 switch (caseState) {
   case NOT_FOLDING: // first case
     if (CheckForButtonPress()) {
       Serial.println("FOLDING");
       led_on();     
       caseState = FOLDING;
     }
     break;


   case FOLDING: { // second case
     Serial.println("[CASE 2: FOLDING]");
     Serial.println("Rotating CW");
     encoder.setCount(0);
     digitalWrite(DIR_PIN, LOW);   // CW
     analogWrite(PWM_PIN, SPEED_PWM);
     while (abs((long)encoder.getCount()) < TARGET_COUNTS) {
       delay(1);
     }
     analogWrite(PWM_PIN, 0);
     Serial.println("CW complete");
     Serial.println("Rotating CCW");
     encoder.setCount(0);
     digitalWrite(DIR_PIN, HIGH);    // CCW
     analogWrite(PWM_PIN, SPEED_PWM);
     while (abs((long)encoder.getCount()) < TARGET_COUNTS - 50) {
       delay(1);
     }
     analogWrite(PWM_PIN, 0);
     Serial.println("CCW complete");
     Serial.println("Returning to CASE 1: NOT FOLDING");
     sendPulseToReceiver(); //send signal to next esp
     caseState = NOT_FOLDING;
     led_off();
   } break;
 }
}
// event checkers


bool CheckForButtonPress() {
 if (buttonIsPressed) {
   buttonIsPressed = false;
   return true;
 }
 return false;
}


void led_on() {
 digitalWrite(LED_PIN, HIGH);
}


void led_off() {
 digitalWrite(LED_PIN, LOW);
}

```