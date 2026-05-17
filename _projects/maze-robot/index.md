---
layout: post
title: Maze Solving Robot
description: Equipped a robot with sensors and coded it to navigate through a maze.
skills: 
- Arduino
- C++
- Electronics
main-image: /main.jpeg
---

**Project Type** - High School

**Collaborators** - Hamzah Shir

This project’s challenge was to customize and code an existing robotic frame to navigate a maze as efficiently as possible. The project was scored by the amount of “tiles” that the robot covered within a given time frame - the more tiles covered, the better. 

## Component Choice
The robot frame used for this project was a Lego Mindstorm. The robot as given had two motors to drive its wheels, and we were given the options to equip it with any of the following:

- Light sensor - detected light reflected off of a surface and output an analog reading (0 to 255). This was used to detect a small centered white strip along the otherwise black floor of the maze.
- Ultrasonic sensor - detected when walls were near the sensor via ultrasonic waves. This was used to tell the robot when it was about to hit a wall.
- Touch sensor - detected when wall were near by physically hitting them and causing a button to be pressed. This was used to tell the robot when it had hit a wall.

Our team went with the light and ultrasonic sensors. This allowed our robot to stay centered in the maze while being able to navigate around walls.

## Software
The robot was coded using the Arduino IDE. Our team’s approach to maximizing our area coverage was simple. The robot would follow the white line along the floor, using the light sensor as guidance. When the robot encountered a wall, it would turn to the left 90 degrees. This algorithm allowed our robot to navigate through the entire maze within the time limit.

This code was used on the robot:
```c++
//declarations
//black on the floor is 19 and 20 on light sensor
//evshield
#include <Wire.h>
#include <EVShield.h>
EVShield evshield(0x34, 0x36);

//light sensor
#include <EVs_NXTLight.h>
EVs_NXTLight light1;
int lightvalue;

//ultrasonic sensor
#include <NewPing.h>
NewPing sonar(7, 8, 190);
unsigned int sonarvalue;

void setup() {

  //start evshield
  evshield.init( SH_HardwareI2C );
  evshield.waitForButtonPress(BTN_GO);

  //start light sensor
  light1.init(&evshield, SH_BAS1);
  light1.setReflected();

  //start serial monitor
  Serial.begin(9600);

}

void loop() {

  //read light sensor
  lightvalue = map(light1.readRaw(), 1023, 0, 0, 100);

  Serial.print("light sensor reading: ");
  Serial.println(lightvalue);

  if (lightvalue > 40)  {

    //drive forward with veer to the right
    evshield.bank_b.motorRunUnlimited(SH_Motor_1, SH_Direction_Reverse, 40);
    evshield.bank_b.motorRunUnlimited(SH_Motor_2, SH_Direction_Forward, 20);

    lightvalue = map(light1.readRaw(), 1023, 0, 0, 100);

    Serial.print("light sensor reading: ");
    Serial.println(lightvalue);
  }



  //get back onto the line
  lightvalue = map(light1.readRaw(), 1023, 0, 0, 100);

  Serial.print("light sensor reading: ");
  Serial.println(lightvalue);

  if (lightvalue <= 40) {
    //pivot left
    evshield.bank_b.motorRunUnlimited(SH_Motor_1, SH_Direction_Forward, 20);
    evshield.bank_b.motorRunUnlimited(SH_Motor_2, SH_Direction_Reverse, 40);

    lightvalue = map(light1.readRaw(), 1023, 0, 0, 100);

    Serial.print("light sensor reading: ");
    Serial.println(lightvalue);

    sonarvalue = sonar.ping() / US_ROUNDTRIP_CM;

  }

  if (sonarvalue <= 8 && sonarvalue != 0) {


    sonarvalue = sonar.ping() / US_ROUNDTRIP_CM;
    if (sonarvalue <= 8 && sonarvalue != 0) {

      evshield.bank_b.motorRunDegrees(SH_Motor_1, SH_Direction_Forward, 49, 195, SH_Completion_Dont_Wait,  SH_Next_Action_Brake);
      evshield.bank_b.motorRunDegrees(SH_Motor_2, SH_Direction_Reverse, 45, 195, SH_Completion_Wait_For,  SH_Next_Action_Brake); //tweak motor speeds

      delay(800);

      sonarvalue = sonar.ping() / US_ROUNDTRIP_CM;

      if (sonarvalue <= 8 && sonarvalue != 0) {

        evshield.bank_b.motorRunDegrees(SH_Motor_1, SH_Direction_Forward, 49, 195, SH_Completion_Dont_Wait,  SH_Next_Action_Brake);
        evshield.bank_b.motorRunDegrees(SH_Motor_2, SH_Direction_Reverse, 45, 195, SH_Completion_Wait_For,  SH_Next_Action_Brake); //tweak motor speeds

        delay(800);

        sonarvalue = sonar.ping() / US_ROUNDTRIP_CM;

        if (sonarvalue <= 8 && sonarvalue != 0) {

          evshield.bank_b.motorRunDegrees(SH_Motor_1, SH_Direction_Forward, 49, 195, SH_Completion_Dont_Wait,  SH_Next_Action_Brake);
          evshield.bank_b.motorRunDegrees(SH_Motor_2, SH_Direction_Reverse, 45, 195, SH_Completion_Wait_For,  SH_Next_Action_Brake); //tweak motor speeds

          delay(800);

        }
      }
    }
  }
}

```

This code was used to calibrate our sensors during testing:
```c++

//evshield
#include <Wire.h>
#include <EVShield.h>
EVShield evshield(0x34, 0x36);

//ultrasonic sensor
#include <NewPing.h>s
NewPing sonar(7, 8, 213);
unsigned int sonarvalue;

//light sensor
#include <EVs_NXTLight.h>
EVs_NXTLight light1;
int lightvalue;

//touch sensor
#include <EVs_NXTTouch.h>
EVs_NXTTouch touch1;
bool touchvalue;

void setup() {

  //start evshield
  evshield.init( SH_HardwareI2C );
  evshield.waitForButtonPress(BTN_GO);

  //start serial monitor
  Serial.begin(9600);

  //start touch sensor
  touch1.init(&evshield, SH_BAS1); //possible port change

  //start light sensor
  light1.init(&evshield, SH_BAS2);
  light1.setReflected();

}

void loop() {

  sonarvalue = sonar.ping() ? US_ROUNDTRIP_CM;
  lightvalue = map(light1.readRaw(), 1023, 0, 0, 100);
  touchvalue = touch1.isPressed();

  Serial.print("ultrasonic reading: ");
  Serial.println(sonarvalue);

  Serial.print("light sensor reading: ");
  Serial.println(lightvalue);

  Serial.print("touch sensor reading: ");
  Serial.println(touchvalue);


}
```