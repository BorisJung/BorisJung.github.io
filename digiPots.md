---
layout: default
title: Digital Potentiometer Evaluation
---

```
(under construction)
```

[//]: # Das ist ein Kommentar

<img src="https://raw.githubusercontent.com/BorisJung/digiPots/master/pics/digiPots.jpg"/>



# Evaluation of general purpose digital potentiometers

<a id="forkme_banner" href="https://github.com/BorisJung/digiPots">View GitHub Repository</a>

Since I've encountered and actually have debugged circuits that used digital potentiometers but never really used them in a design myself, I wanted to start a simple evaluation of cheap general purpose models, which shall be logged here. 

The first practical application will be using a digital potentiometer to replace the 10V-pwm circuitry, which I'm currently using to dim some LED fixtures using a MeanWell powersupply with 3-in-1 dimming function. (see page 4 in [HLG-40H-20-B datasheets](https://www.meanwell-web.com/content/files/pdfs/productPdfs/MW/HLG-40H/HLG-40H-spec.pdf))

This would save me the 10V supply, physical space as well as unnecessarily wasted energy (although admittedly not very much).


### First eval boards

The first steps were taken with 2 different cheap general purpose digital potentiometers mounted on [multi-package breakout-boards](https://www.digikey.de/product-detail/en/adafruit-industries-llc/1212/1528-1071-ND/5022800) as depicted in Figure 1.

<figure>
<img src="https://raw.githubusercontent.com/BorisJung/digiPots/master/pics/1212-04.jpg" alt="my alt text" height="160" /><br>
<figcaption style="text-align:left">Fig. 1: Adafruit breakout board for MSOP8/TSSOP8 and SOIC8 packages</figcaption>
</figure>

The two potentiometers currently under evaluation are: 

- Texas Instruments TPL0501 ([Datasheet](https://www.ti.com/lit/ds/symlink/tpl0501-100.pdf?ts=1594580841545))
- Microchip MCP4141 ([Datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/22059b.pdf))

Both have a nominal value of 100kΩ and can be controlled via [SPI interface](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface). In the final LED-dimming application mentioned above, the will be controlled by a raspberry pi zero w, for developing purposes however, I used an [Adafruit FT232H Breakout Board](https://www.adafruit.com/product/2264), which allows communication between USB hosts and various interfaces (SPI, I2C, UART). 
