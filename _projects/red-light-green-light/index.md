---
layout: post
title: Red Light Green Light
description:  Designed and prototyped a tabletop electronic version of the classic game 'Red Light Green Light'.
skills: 
- CAD
- Onshape
- Documentation
- Technical Presentation
- 3D Printing
- Arduino
- Circuit Design
main-image: /main.png
date: 2021-12-12
---

**Project Type** - High School

**Collaborators** - Benny Marenco

The Red Light Green Light project was a translation of the classic game Red Light Green Light into an electronic board game. 4 motors drove 4 rubber conveyor belts that carried 3D printed figurines down the board. A light post with a green and red LED would flash at random intervals, signaling players to press buttons under their lanes to advance their characters. Pressing a button during a red light would move the player backwards automatically as a penalty. First one to the end won!

This project was not completed due to time restrictions. It was manufactured and assembled with circuitry, but was not ever properly operated. 

This project was done through the Product Design class offered by the Space & Engineering Academy at Merrill F West High School.

## CAD and Design
The main base of the game was laser cut from balsa wood. The top of it, the light post, the motor mounts, and the figurines were 3D printed. The conveyor belts were laser cut from stamp rubber and sewn together to form loops.

See the [Assembly Drawing](../../assets/red-light-green-light/assembly.pdf) and [Belt Sub-Assembly Drawings](../../assets/red-light-green-light/belt-sub/pdf) (PDF downloads).

## Electronics and Software
Red Light Green Light ran off an Arduino and used extension boards called shift registers to control the four motors, along with an additional battery pack. The Arduino also controlled the 4 buttons and the 2 LEDs. 

{% include image-gallery.html images="circuit-diagram.png" height="400" caption="Circuit diagram for the game." %}

### **Code**
```c++
// Buttons
int buttPin1 = 9;
int buttPin2 = 10;
int buttPin3 = 11;
int buttPin4 = 12;

// LEDs
int redPin = 2;
int greenPin = 13;
////////////////////////////////////////////////// ShiftReg1:           ShiftReg2:
// Shift Register 1                             // [0]: N/A             [0]: N/A
int datapin = 8;                                // [1]: N/A             [1]: Keep at 1
int clockpin = 6;                               // [2]: 1: Motor1 Rev   [2]: 1: Motor3 Rev
int latchpin = 7;                               // [3]: 1: Motor1 Fwd   [3]: 1: Motor3 Fwd
byte data = 0;                                  // [4]: Keep at 1       [4]: N/A
int shiftReg1[8] = {0, 0, 0, 0, 1, 0, 1, 0};    // [5]: 1: Motor2 Rev   [5]: 1: Motor4 Rev
////////////////////////////////////////////////// [6]: Keep a 1        [6]: Keep at 1
// Shift Register 2                             // [7]: 1: Motor2 Fwd   [7]: 1: Motor4 Fwd
int datapin2 = 5;
int clockpin2 = 4;
int latchpin2 = 3;
byte data2 = 0;
int shiftReg2[8] = {0, 1, 0, 0, 0, 0, 1, 0};

// Other
int rNum;
unsigned long saveTime = millis();
unsigned long greenTime = millis();
bool isGreen = true;
bool isCaught1 = false;
bool isCaught2 = false;
bool isCaught3 = false;
bool isCaught4 = false;
bool doStartup = true;

void setup() {

  //Buttons
  pinMode(buttPin1, INPUT);
  pinMode(buttPin2, INPUT);
  pinMode(buttPin3, INPUT);
  pinMode(buttPin4, INPUT);

  //LEDs
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);

  //Shift Register
  pinMode(datapin, OUTPUT);
  pinMode(clockpin, OUTPUT);
  pinMode(latchpin, OUTPUT);
  pinMode(datapin2, OUTPUT);
  pinMode(clockpin2, OUTPUT);
  pinMode(latchpin2, OUTPUT);

  //Misc
  randomSeed(analogRead(0));
  Serial.begin(115200);

}

void doShift() {

  /* =============== SHIFT REGISTER CONTROL =============== */

  // ShiftRegister 1
  for (int i = 0; i < 8; i++) {
    bitWrite(data, i, shiftReg1[i]);
  }
  shiftOut(datapin, clockpin, MSBFIRST, data);
  digitalWrite(latchpin, HIGH);
  digitalWrite(latchpin, LOW);

  // ShiftRegister 2
  for (int i = 0; i < 8; i++) {
    bitWrite(data2, i, shiftReg2[i]);
  }
  shiftOut(datapin2, clockpin2, MSBFIRST, data2);
  digitalWrite(latchpin2, HIGH);
  digitalWrite(latchpin2, LOW);

  /* ====================================================== */

}

void loop() {

  while (doStartup) {
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, LOW);
    delay(500);
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, HIGH);
    delay(500);
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, LOW);
    delay(500);
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, HIGH);
    delay(500);
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, HIGH);
    delay(250);
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, LOW);
    delay(250);
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, HIGH);
    delay(250);
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, LOW);
    delay(250);
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, HIGH);
    delay(250);
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, LOW);
    delay(2000);
    digitalWrite(greenPin, HIGH);
    doStartup = false;
  }

  doShift();

  if (isGreen) {

    isCaught1 = false;
    isCaught2 = false;
    isCaught3 = false;
    isCaught4 = false;

    // Turn Motors On/Off w/ Buttons

    if (digitalRead(buttPin1) == LOW) {         // Motor 1
      shiftReg1[2] = 1;
      shiftReg1[3] = 1;
    }
    else {
      shiftReg1[2] = 0;
      shiftReg1[3] = 1;
    }
    if (digitalRead(buttPin2) == LOW) {         // Motor 2
      shiftReg1[5] = 1;
      shiftReg1[7] = 1;
    }
    else {
      shiftReg1[5] = 0;
      shiftReg1[7] = 1;
    }
    if (digitalRead(buttPin3) == LOW) {         // Motor 3
      shiftReg2[2] = 1;
      shiftReg2[3] = 1;
    }
    else {
      shiftReg2[2] = 0;
      shiftReg2[3] = 1;
    }
    if (digitalRead(buttPin4) == LOW) {         // Motor 4
      shiftReg2[5] = 1;
      shiftReg2[7] = 1;
    }
    else {
      shiftReg2[5] = 0;
      shiftReg2[7] = 1;
    }

    if ((millis() - saveTime >= 2000 ) && (millis() - greenTime) >= 2000) {
      rNum = random(100);
      Serial.print(saveTime);
      Serial.print(" ");
      Serial.println(rNum);
      if (rNum % 2 == 0) {                // Every second, generate a new random number
        digitalWrite(greenPin, LOW);      // If random number is even, switch light to red
        digitalWrite(redPin, HIGH);       // Stay green otherwise
        isGreen = false;
      }
      saveTime = millis();
    }

  }
  while (!isGreen) {

    // If button is not being pressed while red, flip a bool. If bool is true, stop belts and reverse motors.

    if (digitalRead(buttPin1) == HIGH) {    // Motor 1
      isCaught1 = true;
    }

    if (digitalRead(buttPin2) == HIGH) {    // Motor 2
      isCaught2 = true;
    }

    if (digitalRead(buttPin3) == HIGH) {    // Motor 3
      isCaught3 = true;
    }

    if (digitalRead(buttPin4) == HIGH) {    // Motor 4
      isCaught4 = true;
    }

    delay(500);
    shiftReg1[2] = 0;
    shiftReg1[3] = 0;
    shiftReg1[5] = 0;
    shiftReg1[7] = 0;
    shiftReg2[2] = 0;
    shiftReg2[3] = 0;
    shiftReg2[5] = 0;
    shiftReg2[7] = 0;

    doShift();

    if (isCaught1) {
      shiftReg1[2] = 1;
    }
    if (isCaught2) {
      shiftReg1[5] = 1;
    }
    if (isCaught3) {
      shiftReg2[2] = 1;
    }
    if (isCaught4) {
      shiftReg2[5] = 1;
    }

    doShift();

    delay(2000);
    shiftReg1[2] = 0;
    shiftReg1[3] = 0;
    shiftReg1[5] = 0;
    shiftReg1[7] = 0;
    shiftReg2[2] = 0;
    shiftReg2[3] = 0;
    shiftReg2[5] = 0;
    shiftReg2[7] = 0;

    doShift();

    delay(3000);                          // Keep light red for 2 seconds
    digitalWrite(greenPin, HIGH);         // Reset light back to green
    digitalWrite(redPin, LOW);
    isGreen = true;
    greenTime = millis();

  }

}
```