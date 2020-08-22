---
layout: default
title: PlantPi sensor server
---


```
(under construction)
```

# "PlantPi" sensor server

Older project I have been pursuing, which has been waiting for a revision for quite some while. 

Based on a raspberry pi B+ with several sensors as well as some relays for switching 230V appliances, this web server was monitoring temperature and humidity while correspondingly switching ventilation. Data was saved into an SQL database and displayed on the web-interface with charts.js.

<div>
<p>
<figure style="float:left; width:95%">
<a href="https://github.com/BorisJung/PlantPi/blob/master/pics/plantpi_proof.png?raw=true" target="_blank">
<img src="https://github.com/BorisJung/PlantPi/blob/master/pics/plantpi_proof.png?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">PlantPi web-interface and script console output</figcaption>
</figure>
</p>
</div>
<br>






Revision shall include:
- multiple temperature sensors / measurement points
- other sensors (luminous flux, air pressure, air "quality", soil moisture, pot weight, etc.)
- updated interactive web-interface with settings page
