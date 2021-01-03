---
layout: default
title: Apple Home Enabled Horticulture Lighting
---

```
(under construction)
```

[//]: # Das ist ein Kommentar


[//]: Bild von Pflanzen oder IoT Logos (homebridge, Apple Home, ...)
[//]: <img src="https://raw.githubusercontent.com/BorisJung/digiPots/master/pics/digiPots.jpg"/>



# Apple Home Enabled Horticulture Lighting

<a id="forkme_banner" href="https://github.com/BorisJung/digiPots">View GitHub Repository</a>

Finally there was some time to follow up on my [digital potentiometer evaluation](digiPots.html), using them for a real application: dimming a lighting fixture.

<div class="outer">
<figure class="centeredFigure" style="width:97%">
<img src="{{ site.url }}/assets/images/resDimming.png" alt="my alt text" />
<figcaption style="text-align:left">Additive resistance dimming of the MeanWell HLG-240H-C1400-B LED Driver</figcaption>
</figure>
</div>      	

The fixture consists of various horticulture lighting LEDs and is driven by a [MeanWell HLG-240H-C1400-B](https://www.meanwell-web.com/content/files/pdfs/productPdfs/MW/HLG-240H-C/HLG-240H-C-spec.pdf){:target="_blank"} "Constant Current Mode LED Driver" supply. The supply's 3-in-1 dimming function supports regulating the output current by applying an additive resistance to the DIM+/DIM- inputs. Therefore dimming is achieved by regulating the output current via the digital potentiometer value. 

For ease of use and the option to integrate it into a real home automation environment, the lamp is controlled using Apple's Home App on various devices. 

---

The steps taken to realise this project will be documented in the following paragraphs. [This tutorial by Frank Meffert](https://frankmeffert.de/raspberry-pi-4-ws2801-apple-homekit/){:target="_blank"} was the main reference, other sources will be mentioned in the footnotes.

---

## Prerequisites 

- Raspberry Pi Zero W 
- Homebridge

This projects is based on a Raspberry Pi Zero W single board computer with Raspbian installed and a WiFi connection configured.

[Homebridge](homebridge.io) is used to enable communication with iOS devices using Apple's HomeKit framework and devices which natively do not support HomeKit. For easier configuration the [config-ui-x](https://github.com/oznu/homebridge-config-ui-x)-Plugin is used.   

[Homebridge installation guide](https://github.com/homebridge/homebridge/wiki/Install-Homebridge-on-Raspbian){:target="_blank"}

## Technology Stack

<div class="outer">
<figure class="centeredFigure" style="width: 97%">
<img src="{{ site.url }}/assets/images/techStack.png" alt="my alt text" />
<figcaption style="text-align:left">Technology Stackup</figcaption>
</figure>
</div>

The tech stack used to realise the communication between Apple's Home App and the digital potentiometer controlling the current output of the LED driver includes the following:

- HomeKit
- Homebridge
- Flask
- Python
- spidev
- Raspberry Pi Zero W

The following paragraphs will describe setting up the tech stack from bottom to top, starting with the digital potentiometer up to the final user interface in Apple's Home app. 

___

## Controlling the digital potentiometer via SPI

The potentiometer used in this project is the [TPL0501](https://www.ti.com/lit/ds/symlink/tpl0501-100.pdf?ts=1594580841545){:target="_blank"} by Texas Instruments. This is a general purpose 256-Taps Single-Channel potentiometer with SPI interface. To be able to use SPI with the raspberry pi, it has to be enabled first. There are several ways to achieve this, most of them listed [here](https://www.raspberrypi-spy.co.uk/2014/08/enabling-the-spi-interface-on-the-raspberry-pi/){:target="_blank"}. For general info about the SPI on the raspberry pi, see [here](https://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md){:target="_blank"}.   

<div class="outer">
<figure class="centeredFigure" style="width: 85%">
<img src="{{ site.url }}/assets/images/TPL0501_block.png" alt="my alt text" />
<figcaption style="text-align:left">TPL0501 digital potentiometer simplified schematic</figcaption>
</figure>
</div>          

Once SPI is activated, the hardware must be connected. I still had a TPL0501 mounted on a general purpose breakout board from my [digital potentiometer evaluation](digiPots.html) lying around, which got re-purposed here. 

The following table shows the connections between the raspberry pi zero w and the TPL0501:

| Pi Zero W   |   |   |   | TPL0501 |   |
| Name | Pin  |   | Pin | Name  |
| ---                     | --- |               | --- | --- |
| MOSI (GPIO10) | 19 | &#8594; | 5 | DIN |
| CE0_N (GPIO8) | 24 | &#8594; | 6 | <span style="text-decoration:overline">CS</span> |
| SCLK (GPIO11) | 23 | &#8594; | 4 | SCLK |
| 5V | 2 | &#8594; | 2 | VDD |
| GND | 20 | &#8594; | 3 | GND |

The TPL0501 digital potentiometer uses a 3-wire, write-only serial interface. This means that the value can't be read back (which is acceptable for this application). For detailed description of the SPI see the [TPL0501 datasheet](https://www.ti.com/lit/ds/symlink/tpl0501-100.pdf?ts=1594580841545#page=11){:target="_blank"}. 

The basic functionality of the potentiometer can be tested using a few simple lines of python code (either in a script or the python console):

```
import time # only needed for loop timing
import spidev # python module to the SpiDev driver of raspbian/linux

spi = spidev.SpiDev() # create SpiDev object
spi.open(0, 0) # open connection

while true:
    spi.writebytes(bytearray([255])) # write 1 byte of data with value 0xFF
    time.sleep(3) # wait for 3 seconds
    spi.writebytes(bytearray([0])) # write 1 byte of data with value 0x00
    time.sleep(3) # wait for 3 seconds
```

These commands should switch the potentiometer between its maximum and zero resistance value.   

___

## Simple API with Flask

With the help of a flask server it's very easy to make python scripts accessible via http. Since I'm neither an expert with flask nor is this the main topic for this project, I'll keep this section very brief, for more info see [here](https://flask.palletsprojects.com/en/1.1.x/){:target="_blank"} or [here](https://de.wikipedia.org/wiki/Flask){:target="_blank"}. 

The complete script is available in this projects GitHub repository, here I'll go through the relevant parts.


### Class and methods for the digital potentiometer

First a class for the digital potentiometer is defined, already having in mind the final user interface in Apple's Home app. A standard lightbulb accessory in Apple Home consists of a button and a fader. So we'll need methods to activate and deactivate the button as well as setting and reading values of the fader.

```
import flask # framework for web service / API
from flask import jsonify

# dPot class 
class dPot:
    def __init__(self):
        # open connection
        self.spi = spidev.SpiDev()
        self.spi.open(0, 0)
        self.spi.max_speed_hz = 976000
        self.last = 30 # default last value
        self.ison = 0 # default state

    def on(self):
        print('Turned on!')
        self.ison = 1
        return jsonify(1)

    def off(self):
        print('Turned off!')
        self.writeVal(0)
        self.ison = 0
        return jsonify(0)

    def setVal(self, Val):
        print('Value set to ',Val)
        wVal = int(float(Val)*2.55) # answer value is in percent, dPot has 256 steps
        self.last = Val
        self.ison = 1
        self.writeVal(wVal)
        return jsonify(self.last)

    def writeVal(self, Val):
        self.spi.writebytes(bytearray([Val]))
        print('Value written:', Val, type(Val))

    def is_on(self):
        return jsonify(self.ison)

    def getVal(self):
        return jsonify(self.last)
```

The ***\_\_init\_\_\(\)*** method sets up the SPI, similar to the test script, and sets some default values. Other methods use the [jsonify function](https://flask.palletsprojects.com/en/1.1.x/api/#flask.json.jsonify){:target="_blank"} out of the flask module, providing return values as a *Response Object* with the appropriate JSON header. 

**writeVal()** writes the input value to the SPI device, using the [bytearray()](https://docs.python.org/3/library/functions.html#func-bytearray){:target="_blank"} python function to convert the integer value to a single byte. 

___

### Flask server

Next we need to make the python class accessible via HTTP through a flask server. 

First we create a flask object as well as an object using the just defined *dPot*-class. The next code block defines the [dispatcher](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Flask.route){:target="_blank"} and action method, routing the commands to the corresponding python methods of the *dPot*-object. A minimal error checking has been added with the *else* clause.

```
# flask app
app = flask.Flask(__name__)

# dPot object
digPot = dPot()

# dispatcher
@app.route("/<path:cmdstr>", methods=['GET','POST'])
def action(cmdstr):
    cmdv = cmdstr.split("/")
    cmd = cmdv[0]

    if cmd == "on":
        return digPot.on()
    elif cmd == "off":
        return digPot.off()
    elif cmd == "status":
        return digPot.is_on()
    elif cmd == "set":
        if len(cmdv) > 1:
            return digPot.setVal(cmdv[1])
        else:
            return digPot.getVal()
    else:
        msg = "Unkown command: %s." % cmd
        sys.stderr.write(msg)
        return jsonify(msg)

# main()
if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80, debug=True)
```

Finally, by running this script as main, the server gets started on port 80. HTTP requests will now be received by the flask server, either running the corresponding method of the *dPot*-object or returning an error message. This can be manually tested by opening the address of a method in a browser. (e.g. "192.168.10.1/status" or "192.168.10.1/off")


___

## Homebridge

As mentioned before, [homebridge](homebridge.io){:target="_blank"} enables devices without native HomeKit support to be integrated into Apple's Home app. 

For this application, the plugin [Homebridge Http Lightbulb](https://github.com/Supereg/homebridge-http-lightbulb){:target="_blank"} provides the communication between flask and homebridge via HTTP. 

It can be easily installed via the config-ui-x web-interface, (xxx.xxx.xxx.xxx:8080) or via npm:

```
sudo npm install -g homebridge-http-lightbulb
```
This plugin manages a homebridge accessory called HTTP-LIGHTBULB, which needsd to be added and configured first. This can easily done in the web-interface, by opening the *Homebridge Http Lightbulb* settings on the *Plug-Ins* page and adding the accessory by pasting the following code: 

```
{
    "accessory": "HTTP-LIGHTBULB",
    "name": "Pflanzenbeleuchtung NEU",
    "onUrl": "http://localhost/on",
    "offUrl": "http://localhost/off",
    "statusUrl": "http://localhost/status",
    "brightness": {
        "setUrl": "http://localhost/set/%s",
        "statusUrl": "http://localhost/set"
    }
}
```

This tells the homebridge plugins about the URLs which invoke the python functions. 

___

## Running the script as a service

To automatically start and restart the python script containing the flask server, a service was set up.

This is done by creating a new file `/etc/systemd/system/<service-name>.service`  with the following contents:


```
[Unit]
Description=Flask server for controlling a custom light via SPI dig pot
After=network.target

[Service]
WorkingDirectory=/home/pi
ExecStart=/usr/bin/python /home/pi/dPot.py
Restart=always

[Install]
WantedBy=multi-user.target
```

After saving, systemd needs to be reloaded, the new service enabled and started.


`sudo systemctl daemon-reload`

`sudo systemctl enable <service-name>`

`sudo systemctl start <service-name>`


Now the flask server should restart in cases of power outage, program crash or similar cases. 

___

Given Homebridge has been already added to the Home app as a bridge device, the newly created "Http Lightbulb" accessory should immediately be available.



