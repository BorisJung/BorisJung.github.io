---
layout: default
title: Digital Potentiometer Evaluation
---

```
(under construction)
```


<img src="https://raw.githubusercontent.com/BorisJung/digiPots/master/pics/digiPots.jpg"/>


# Evaluation of general purpose digital potentiometers

<a id="forkme_banner" href="https://github.com/BorisJung/digiPots">View GitHub Repository</a>

Since I've encountered and actually have debugged circuits that used digital potentiometers but never really used them in a design myself, I wanted to start a simple evaluation of cheap general purpose models, which shall be logged here. 

The first practical application will be using a digital potentiometer to replace the 10V-pwm circuitry, which I'm currently using to dim some LED fixtures using a MeanWell powersupply with 3-in-1 dimming function. (see page 4 in datasheets: [HLG-40H-20-B](https://www.meanwell-web.com/content/files/pdfs/productPdfs/MW/HLG-40H/HLG-40H-spec.pdf))

This would save me the 10V supply, physical space as well as unnecessarily wasted energy (although admittedly not very much).


### First eval boards

The first steps were taken with 2 different cheap general purpose digital potentiometers mounted on [multi-package breakout-boards](https://www.digikey.de/product-detail/en/adafruit-industries-llc/1212/1528-1071-ND/5022800).

{% include image.html url="https://media.digikey.com/Photos/Adafruit%20Industries%20LLC/MFG_1212.jpg" width="125" align="right" description="Adafruit breakout board" %}

The two potentiometers currently under evaluation are: 

- Texas Instruments [TPL0501](https://www.ti.com/lit/ds/symlink/tpl0501-100.pdf?ts=1594580841545)
- Microchip [MCP4141](http://ww1.microchip.com/downloads/en/DeviceDoc/22059b.pdf)




This script was developed as part of a portfolio exam during my master's studies at TU Berlin. It requires the Auditory Modeling Toolbox, the Large Time-Frequency Analysis Toolbox as well as the SOFA MatLab API included in the corresponding subfolder.

Once all set up, it calculates the binaural room impulse response of a rectangular room with parameters given by input variables. See pictures subfolder or [``project_paper.pdf``](https://github.com/BorisJung/ISM_BRIR_Demo/blob/master/project_paper.pdf?raw=true) for an overview of the functionality.


___

![IS_Pos](https://github.com/BorisJung/ISM_BRIR_Demo/blob/master/pics/01_IS_pos.jpg?raw=true)

___

![IS_Pos](https://github.com/BorisJung/ISM_BRIR_Demo/blob/master/pics/02_BRIR_roh.png?raw=true)

___

![IS_Pos](https://github.com/BorisJung/ISM_BRIR_Demo/blob/master/pics/03_gewPulse.jpg?raw=true)

___

![IS_Pos](https://github.com/BorisJung/ISM_BRIR_Demo/blob/master/pics/04_xbinCoh.jpg?raw=true)

___
