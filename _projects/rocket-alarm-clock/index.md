---
layout: post
title: Rocket Alarm Clock
description:  Designed and prototyped a smart-alarm clock device.
skills: 
- CAD
- Onshape
- Documentation
- Technical Presentation
- 3D Printing
main-image: /main.jpg
date: 2022-04-01
---

**Project Type** - High School

**Collaborators** - [Benicio Marenco](https://www.linkedin.com/in/benicio-marenco-3380b7249/)

The Rocket Alarm Clock is an internet-enabled electronic device designed to be put on a nightstand. It displays the time, the local weather, and a quote of the user’s choice on the LCD display, and has customizable RGB lighting.

This project was completed through the Product Design class offered by the Space & Engineering Academy at Merrill F West High School.

## Design
The rocket body was designed in Onshape and 3D printed using a Prusa MK3.

{% include image-gallery.html images="cad-assembled.jpg, cad-exploded.jpg" height="400" caption="Assembled and exploded views of the CAD." %}

## Electronics and Software
The rocket is run by an Argon, comparable to an internet-enabled Arduino. The internet capabilities are enabled via the Node Red platform. The device is powered by a battery that can be charged via the Argon. 3 LEDs and an LCD display are controlled by the Argon, and the powered is turned on and off with a capacitive button. 

{% include image-gallery.html images="circuit-diagram.png" height="400" caption="Circuit diagram for the rocket." %}

### **Code**

{% include image-gallery.html images="code-1.jpg, code-2.png, code-3.png, code-4.png, code-5.png, code-6.png, code-7.png, " height="400" caption="Code used to run the alarm clock." %}

The alarm clock used Node Red to integrate with the app, where the user can control the LED color, weather, and display settings.

{% include image-gallery.html images="node-red.png, app-dashboard.png" height="400" caption="Node red flow and app dashboard." %}