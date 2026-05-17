---
layout: post
title: Wedgelock Redesign
description:  Redesigned an off-the-shelf electronics securement device for in-house manufacturing to reduce costs and lead times for the Lawrence Livermore National Laboratory's Space Science and Security Program.
skills: 
- CAD
- SOLIDWORKS
- Project Management
- FEA
- Documentation
- 3D Printing
- GD&T Drawings
main-image: /pcb-lock.JPG
date: 2024-08-09
pinned: true
---
**Project Type** - Internship

**Collaborators** - Charlin Wang (LLNL), Rebecca Griffith (LLNL), Tiffany Yslas (LLNL), Jordan Smilo (LLNL), Jordan Karburn (LLNL), & Joshua Robles (LLNL).

**Released under LLNL-POST-867582**

## Wedgelocks and PCB Mounting
Wedgelocks are used to fasten PCBs in slotted mechanical enclosures. They are mounted on both sides of a PCB and the assembly is slotted into place. The Wedgelock is then engaged with a screw, which causes it to expand against the slot walls, generating high force to secure the PCB.

{% include image-gallery.html images="filled-enclosure.jpeg, pcb-lock.JPG" height="400" caption="Filled enclosure with PCBs and detail view of wedgelocks attached to PCB." %}

## Traditional Design
Traditional wedgelocks contain trapezoidal wedges that, when pushed on by a screw, move perpendicular to the axis of engagement of the screw. This outward movement generates the force to secure the PCB.

Traditional wedgelocks lack customization and have procurement times of over 6 weeks due to their high part counts and complex assembly needs. They also lack size options, making working with different sizes of PCBs difficult.

{% include image-gallery.html images="traditional.png" height="400" caption="CAD model of traditional wedgelock unengaged (top) and engaged (bottom)." %}

## Compliant Design
Compliant mechanisms bend to create motion. They typically have less parts than traditional designs, making them easier and faster to manufacture. 

This hybrid design combines the engagement mechanism of the traditional wedgelock and the advantages of compliant design. This wedgelock is a single part that can be easily manufactured.

{% include image-gallery.html images="compliant.png" height="400" caption="Compliant Wedgelock Design unengaged (top) and engaged (bottom)." %}

Screw engagement causes the outer faces to bow outwards, generating force against the PCB and slot walls. A second screw holds the wedgelock to the PCB during installation into an enclosure. This design is able to be scaled in length in increments of 3 mm.

{% include image-gallery.html images="engage-detail.jpeg, attach-detail.jpeg" height="400" caption="Detail views of engagement mechanism and how wedgelock attaches to PCB." %}

## Finite Element Analysis (FEA)
A first estimate for the needed force output from Wedgelocks was about 1500 Newtons (N). This was determined using the assumption that the PCBs weighed 1 pound, were undergoing 50 gs, and were held with 2 Wedgelocks.

Simulation done on the compliant design (material choice of 7075 Aluminum) via FEA fun in SOLIDWORKS showed the design’s capability of outputting upwards of 8000 N.

{% include image-gallery.html images="fea.png" height="400" caption="FEA results compared with minimum force estimate. " %}

## Next Steps
This design performed well in FEA and prototype environments. Further testing will need to be done to determine effectiveness under real loading, along with using different materials and manufacturing methods. 

**Released under LLNL-POST-867582**

*This summary was adapted from my [LLNL Summer Student Symposium Poster](../../assets/symposium-poster.pdf).*