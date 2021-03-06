---
layout: default
title: LTSpice Simulations
---


```
(under construction)
```

# LTspice Projects

<a id="forkme_banner" href="https://github.com/BorisJung/LTSpice#ltspice-projects">View GitHub Repository</a>


This repository presents some samples of SPICE-simulations I have worked on. 


### Angelov Model Equivalent Circuit

For my master's thesis I was working on a Spice-Model for the Angelov Model to be able to do time-domain simulations of an RF Power GaN HEMT in LTSpice. The extracted parameters had been used and verified in various ADS Simulations and were saved in the [.mdif-File format](http://literature.cdn.keysight.com/litweb/pdf/ads2005a/cktsim/ck0418.html). 


<img src="https://github.com/BorisJung/LTSpice/blob/master/AngelovModel/SCHEMATIC___Angelov_Model_Equivalent_Circuit.jpg?raw=true" align="left" />

The image above shows the equivalent circuit for the angelov model modeled in LTSpice. Below are some DC curves extracted from the LTSpice simulation. The results for different .mdif-Files/transistors have been compared to the Keysight ADS DC-simulations and showed close match.



___

[Keysight ADS Angelov_Model Documentation](https://edadocs.software.keysight.com/pages/viewpage.action?pageId=6063274)

[First original Paper](https://ieeexplore.ieee.org/document/179888)


- Class D Audio Amplifier

- Angelov Model Equivalent Circuit

- Gate Driver
  - Si-BJT Totem Pole Driver





