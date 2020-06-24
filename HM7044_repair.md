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


- current meter only reads 0s in normal use state
- current limit value is displayed correctly
- other functionality seems to be ok


## IC Identification


- **LTC1446** Dual 12-Bit Rail-to-Rail Micropower DAC ([datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/1446fa.pdf)) 

- **MCP3202** 2.7V Dual Channel 12-Bit A/D Converter
with SPI Serial Interface ([datasheet](https://asset.conrad.com/media10/add/160267/c1/-/en/001083119DS01/datenblatt-1083119-microchip-technology-mcp3202-bisn-datenerfassungs-ic-analog-digital-wandler-adc-extern-soic-8-n.pdf))


## Next Steps

___

![top](https://github.com/BorisJung/HM7044/blob/master/Pics/top.jpg?raw=true)

___

![top](https://github.com/BorisJung/HM7044/blob/master/Pics/bottom.jpg?raw=true)
___
