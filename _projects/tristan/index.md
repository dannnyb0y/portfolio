---
layout: post
title: TRISTAN
description:  Led a team of engineers to design and manufacture a prototype device that collected characterizing flight data and tested future ejection hardware.
skills: 
- CAD
- Onshape
- Leadership
- Project Management
main-image: /isometric-cad.png
date: 2025-06-15
---

**Project Type** - Space Technologies and Rocketry

**Collaborators** - [Roman Silivra](https://www.linkedin.com/in/roman-silivra/), [Kevin Sengsourichanh](https://www.linkedin.com/in/kevin-sengsourichanh-4a4743217/), [Kari Martinez-Espindola](https://www.linkedin.com/in/karista-espindola/), [James Huntzinger](https://www.linkedin.com/in/jameshuntzinger/), [Alli Wang](https://www.linkedin.com/in/alli-wang/), [Adnan Kapadia](https://www.linkedin.com/in/adnan-kapadia/), Tristan Steen, [Kush Mahanjan](https://www.linkedin.com/in/kush-mahajan-8030461b8/), [Austin Mei](https://www.linkedin.com/in/austinrmei/)

TRISTAN (Thruster-Based Reconnaissance Instrument for Scientific Tracking and Atmospheric Navigation) was a proof-of-concept for an ejectable payload. The goal was to develop a system that would detach itself from a rocket airframe, descend independently, and achieve a controlled landing on the ground. Ultimately, the system was not ejected from the airframe, but rather showed that the relevant electronics and hardware functioned properly, and could be used after further development for a full ejection in the future.

## Design
TRISTAN was a 5U CubeSat design, with dimensions of 10x10x50 cm. This modular design aided in integration with the airframe. The body was comprised of flat aluminum pieces bolted together with M5 hardware and right-angle brackets. 

All internal components were fastened to the inside of these walls with 3D printed enclosures and further M5 hardware.

{% include image-gallery.html images="isometric-cad.png" height="400" caption="Full CAD model of the payload." %}

## Ejection Mechanism
TRISTAN used an electromechanical system for ejection, though this system was never used to separate the system from the airframe. It was used solely for telemetry and testing the hardware and software involved. 

Linear actuators were used to unlock ball-lock pins housed on either end of the payload. These pins ran through bulkheads in the rocket airframe and once released, allowed the payload to move freely. Heavy springs would then help separate the airframe and the payload would roll out along rails. During the actual flight of this payload, the actuators were fired and the pins unlocked, but the airframe was externally fastened together and the payload was not separated from the airframe.

{% include image-gallery.html images="ejection-detail.png" height="400" caption="The linear actuator (bottom), unlocks the ball-lock pin (top) and allows it to slide through the circular bulkhead." %}

## Electronics and Software
TRISTAN had an ESP32 on board that interfaced with a custom-designed PCB to control the linear actuators for the ejection mechanism. This board handled signals from COTS altimeters to trigger the ‘ejection’ script at the correct time. 

{% include image-gallery.html images="pcb-render.png" height="400" caption="Render of the custom PCB on board TRISTAN." %}

Signals from the altimeters were routed into this board, and then the ESP32 would trigger the actuators to extend and unlock the ball-lock pins. 

*This summary was adapted from the [Excalibur Final Design Report](https://docs.google.com/document/d/1zMoW6IU2J_XAg5joyO1CykrFtCasofWYZe8JZcpNa5A/edit?usp=sharing) for the 2025 IREC competition.*

