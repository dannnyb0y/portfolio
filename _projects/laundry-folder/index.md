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

add circuit diagram, code, photos

---

*This summary was adapted from the [Final Report](https://docs.google.com/document/d/1Q8-mAUrHsxxh8q5Bb3gG4dTP_dOAcJHgd8cTut5wHz4/edit?usp=sharing) written for ME102B.*

*The full CAD document can be viewed [here](https://cad.onshape.com/documents/7ef667d42e2f7cedd0cd0583/w/68efd183ed38f5b916108f58/e/3e34a5b84dbeeddc8ce5f76b).*