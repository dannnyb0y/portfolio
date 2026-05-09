---
layout: post
title: ME103 Wind Turbine Project
description:  Worked with a team of engineers to design various airfoils and characterize their performance using a wind tunnel. This project was done through UC Berkeley's ME103 course.
skills: 
- CAD
- Onshape
- Documentation
- Technical Presentation
- 3D Printing
main-image: /overview.png
---

**Project Type** - UC Berkeley (ME103)

**Collaborators** - Omar Cochinwala, Hamzah Shir, Ezenbaatar Batjargal

This project aimed to characterize airfoil performance for the sake of improving fuel efficiency for aircraft. Multiple surface geometries were explored, all of which involved dimples or cutouts in the airfoil surface. The project also helped foster a greater understanding of principles of aerodynamics and fluid dynamics. The project was structured as an experiment meant to validate a hypothesis.

Our hypothesis was:

> Adding dimples will improve performance through an increase in lift and a decrease in drag.

## Wind Tunnel Calibration
Before we could run any tests, we needed to characterize the measurements output by the various measurement devices present in the Hesse Hall wind tunnel. We calibrated the response from a strain gauge and a pitot tube in order to understand both the speed of the wind moving through the tunnel and the force being exerted on the stand. 

### Strain Gauge
The strain gauge output was recorded at 1 gram increments from 1 to 10 grams, and then at 10 gram increments from 10 to 100 grams. An additional test was done to avoid the effect of hysteresis on the sensor, where weight was strictly increased from 1 to 100 grams, and then strictly decreased back down to 1. This additional data ensured the calibration curves properly represented the sensor output. During our calibration tests, the strain gauge was oriented at 5 degrees to the horizontal, as measured by a protractor built into the mount. This was the baseline orientation for measurement. Ten data points were recorded after each change in weight. This data was then used to create a calibration curve and an equation that related the raw voltage output of the strain gauge to the force being exerted on it. 

{% include image-gallery.html images="strain_calibration.jpeg" height="400" caption="Hanging weights from strain gauge during calibration." %}

After aggregating our data, it was found that the relationship between the output voltage $V$ and the weight on the gauge $W$ was $V =-1.85w -1.52$.

{% include image-gallery.html images="strain_calibration.png" height="400" caption="Calibration data for strain gauge." %}

### Wind Speed
The wind tunnel is equipped with a pitot tube used to measure the dynamic pressure of the air moving through the tunnel. Bernoulli's principle was used to convert this pressure reading ($p$) into wind speed ($v$):

$$v = \sqrt{\frac{2\Delta p}{\rho}} = \sqrt{\frac{2 \Delta p}{\frac{p}{RT}}} = 14.85\sqrt{p}$$

Using this relationship and calibration for the fan speed, we were able to deteremine the relationship between the fan setting and the wind speed to be $S = -0.105 +0.012f+8.78*10^{-7}f^2$, where $f$ is the fan setting and $S$ is the wind speed in meters per second.

## Testing
We tested eight airfoil types and measured the lift they generated and the drag exerted on them at a fixed wind speed of 8.5 meters per second.

The testing steps for each airfoil were as follows:
1. The airfoil was fastened into the tunnel using the mount provided by Hesse Staff.
2. The strain gauge was set to the baseline of 5 degrees.
3. The airfoil was set to an angle of 15 degrees to the horizontal as measured by an angle finder.
4. The wind tunnel was closed and turned on at our experiment speed of 8.5 m/s.
5. 20 data points were recorded using the LabView software after the wind reached our experiment speed.
6. The fan was turned off and the strain gauge was rotated 90 degrees clockwise relative to the airfoil to switch to measuring drag.
7. The fan was turned back on and 20 more data points were taken.

{% include image-gallery.html images="drag-lift-comparison-chart.png" height="400" caption="Lift and drag data for each airfoil." %}

This data ultimately disproved our hypothesis, but it was interesting to learn about the physics at play and about operating and using a wind tunnel. After experimentation, we gave a technical presentation about our findings.

## Airfoil Designs
Below are CAD images of each airfoil type we tested.

{% include image-gallery.html images="baseairfoil.png" height="400" %}

{% include image-gallery.html images="bigdimples.jpg" height="400" %}

{% include image-gallery.html images="dimples.jpg" height="400" %}

{% include image-gallery.html images="grooves3.png" height="400" %}

{% include image-gallery.html images="othergrooves.png" height="400" %}

{% include image-gallery.html images="sideways1.png" height="400" %}

{% include image-gallery.html images="sideways2.png" height="400" %}

{% include image-gallery.html images="tinydimples.jpg" height="400" %}


*This summary was adapted from the [Final Report](../../assets/ME103_Final_Report.pdf) written for the ME103 course.*