---
layout: default
title: HM7044 Repair
---


```
(under construction)
```


![fronPanel](./pics/hm7044.jpg)

# Repair of a Hameg HM7044 Power Supply

I was lucky enough to get a Hameg HM7044 384W Power Supply practically for free. It got sorted out, because the current meter display is supposedly defect.

This page and repository serves as documentation for the steps in the repair process.

## Error diagnostics and description

The Current meter does only show 0s on every channel in normal state.

Basic tests on all outputs suggested other functionality is still in order. For setting the current limit as well as when current limit is reached, values are correctly displayed in the current meter. 

In another test remote functionality via RS-232 was confirmed succesfully.



**Fault details:**
- current meter shows 0.0 A during normal state
- current meter shows current limit value during setup and when in limiting state
- other functionality seems to be ok



## First Conclusions

After having spent some time with the device, I think it's time for a small summary of the information I could gather so far. Figure 1 shows the simplified internal device structure.

<div style="clear:both"><p>
<figure style="display: inline-block; width:98%">
<a href="https://github.com/BorisJung/BorisJung.github.io/blob/master/pics/inkscape/device_blocks_min.png?raw=true
" target="_blank">
<img src="https://github.com/BorisJung/BorisJung.github.io/blob/master/pics/inkscape/device_blocks_min.png?raw=true
" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Fig. 1: HM7044 PCB diagram </figcaption>
</figure>
</p></div>


The grey arrows symbolize 20-pin board-to-board connectors, the yellow line indicates the connection between supply circuitry on the supply board and the output connectors on the front board.



### CPU board

- ASIC: HM7044-2 Ver.: 2.01
- Level translator for RS-232 remote control input

### CPU board <-> Supply board

The supply board pcb consists of 4 identical sections representing the 4 output channels, each of them connected to the CPU board with its own 20-pin connector. On the CPU board those signals are fed via four opto-couplers. Three of them connect signals from the CPU board to the supply board, while the other one has the opposed direction and connects 2 signals from the supply board to the CPU board, on of them being the ADC Output.

- 4 x 20 Pin connectors (1 for each channel)
- 4 opto-couplers (CPU->Supply: 3 , Supply->CPU: 1)
- MCP3202 ADC output signal from supply board to CPU

### CPU board <-> Front board

- Serial/Parallel converters
- Signals directly coupled
- 






___


### Notes
___




**Display drivers 7228:**
- Shutdown Pin (10) shorted to VDD -> all drivers always on


**Block Diagram**
- separate Drivers for Current Meter (bottom row LED digits KW1-391AGA) and Voltage Meter (top row LED digits KW1-391AGA)

for current meter:

KW1-391AGA <-> 7228 <-> HCT164 <-> Connector Pin 17


___

-> There must be a switch for switching between saved limit value and momentary sense reading value!

- Must be on CPU board, since LED Digits input signal comes from B2B-connector

data input Serial/Parallel conv <-> Pin 17 front board to cpu board connector <-> Pin 2 4094 shift register & Pin 27 of main asic / CPU 

-> CPU sends serial signal to both shift register for optocoupler drive, as well as Serial/Parallel conv for driving meter displays

TLP2630 output pins to Pin 16/18 of B2B connector, pulled high via 10k resistors to VCC (of secondary/supply board side), Pin 8 of B2B Connector


___
Next:

- optocoupler going where exactly? (switch between limit and sensed current value?)
- optocoupler defect? input/control signal working? (4094?)


___

- HCT164 Pin 1 (data input A) driven by supply board B2B pin 19 and 20 
- all supply board channels drive/connected to same HCT164 data input A (the one sitting in the middle, connected to current meter) 

___
more readable:

Pin 27 of main asic/CPU connected to 
- Pin 2 (data input B) of HCT164D Ser>Par shift register (CPU board, middle)
AND
- Pin 17 front board connector 



Middle HCT164 Ser>Par shift register:
- DSA input driven by supply board (all channels connected to same input)
- DSB input driven by main asic/CPU
- Outputs high only for DSA&DSB


___

Supply Board Connector Pin 19 

- drives TLP2630 optocoupler connected to HCT164 Ser>Par data input DSA

Origin:
Supply Board Connector Pin 19   <---  Resistor divider  <--- BC846B darlington stage  <--- 61k9  <---  MCP3202 A/D Pin 6 Data Out 


MCP3202 A/D Pin 3 Data In CH1  <---  MC33172(#2) Pin 7 Output 2 
MC33172(#2) Pins 5/6 (inputs 2)  <--- Divider/Filter  <--- MC33172(#1) Pin 1 Output 1  
MC33172(#1) Pin 3 (input)  <---  Sense Connector     


---> MC33172 (#2) Amplifier 2 (Pins 5,6,7): pre-amplification of Sense Connector Signals, Output connected to MCP3202 A/D CH1


MC33172 #1: getting Sense Connector Voltages, feeding MC33172 #2  

MC33172 #2: feeding A/D converter  

MC33172 #3: getting MC33172 #2 Output onto Pin 2 Input  

___


## Pictures

### Front PCB
<div style="clear:both">
<p>
<figure style="float:left; vertical-align:bottom; width:45%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/front_top.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/front_top.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Front PCB of HM7044 (top side)</figcaption>
</figure>

<figure style="float:right; vertical-align:bottom; width:45%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/front_bottom.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/front_bottom.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Front PCB of HM7044 (bottom side)</figcaption><br>
</figure>
</p>
</div>


<div style="clear:both">
</div>


___

### Supply PCB  


<div style="clear:both">
<p>
<figure style="float:left; vertical-align:top; width:44%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/top.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/top.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Supply PCB of HM7044 (top side)</figcaption>
</figure>

<figure style="float:right; vertical-align:top; width:44%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/bottom.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/bottom.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Supply PCB of HM7044 (bottom side)</figcaption>
</figure>
</p>
</div>

<div>
<figure style="float:right; vertical-align:top; width:90%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/supply_top_2.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/supply_top_2.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">Supply PCB of HM7044 (top side)</figcaption>
</figure>
</div>

<div style="clear:both">
</div>

___

### ASIC/CPU PCB  

<div style="clear:both">
<p>
<figure style="float:left; vertical-align:top; width:90%">
<a href="https://github.com/BorisJung/HM7044/blob/master/Pics/cpu_board.jpg?raw=true" target="_blank">
<img src="https://github.com/BorisJung/HM7044/blob/master/Pics/cpu_board.jpg?raw=true" alt="my alt text" /></a><br>
<figcaption style="text-align:left">CPU PCB of HM7044 (top side)</figcaption>
</figure>

</p>
</div>
<div style="clear:both">
</div>



___  



## Identified ICs

- **LM317L** 3-Terminal Adjustable Regulator
([datasheet](https://www.ti.com/lit/ds/symlink/lm317l.pdf?ts=1596796656076&ref_url=https%253A%252F%252Fwww.google.com%252F))

- **LM337LM** 3-Terminal Adjustable Regulator
([datasheet](https://www.ti.com/lit/ds/symlink/lm337l.pdf?ts=1596795119863&ref_url=https%253A%252F%252Fwww.google.com%252F))


- **LTC1446** Dual 12-Bit Rail-to-Rail Micropower DAC ([datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/1446fa.pdf)) 

- **MCP3202** 2.7V Dual Channel 12-Bit A/D Converter
with SPI Serial Interface ([datasheet](https://asset.conrad.com/media10/add/160267/c1/-/en/001083119DS01/datenblatt-1083119-microchip-technology-mcp3202-bisn-datenerfassungs-ic-analog-digital-wandler-adc-extern-soic-8-n.pdf))

- **MC33172** Low power dual bipolar operational amplifiers ([datasheet](https://www.st.com/resource/en/datasheet/mc33172.pdf))



- **KW1-391AGA** Single Digit 7-segment LED Display ([datasheet](https://www.luckylight.cn/media/component/data-sheet/KW1-391AGA.pdf))

- **ICM7228** 8-Digit, Microprocessor-Compatible, LED Display Decoder Driver ([datasheet](https://www.renesas.com/cn/en/www/doc/datasheet/icm7228.pdf))

- **HCT4094** 8-stage shift-and-store bus register ([datasheet](https://assets.nexperia.com/documents/data-sheet/74HC_HCT4094.pdf))

- **HCT164** 8-bit serial-in, parallel-out shift register ([datasheet](https://assets.nexperia.com/documents/data-sheet/74HC_HCT164.pdf))

- **HCT165** High-Speed CMOS Logic 8-Bit Parallel-In/Serial-Out Shift Register ([datasheet](https://www.ti.com/lit/ds/schs156c/schs156c.pdf?ts=1596879013114&ref_url=https%253A%252F%252Fwww.google.com%252F))

- **TLP2630** Photocoupler (photo-IC output) ([datasheet](https://toshiba.semicon-storage.com/eu/semiconductor/product/optoelectronics/detail.TLP2630.html))


- **1Bs 71** mark on SOT23 package might be BC846B ( [datasheet](https://www.infineon.com/dgdl/Infineon-BC846SERIES_BC847SERIES_BC848SERIES_BC849SERIES_BC850SERIES-DS-v01_01-en.pdf?fileId=db3a30431441fb5d011449cec9bd0241))
