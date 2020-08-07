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

- **LM317L** 3-Terminal Adjustable Regulator
([datasheet](https://www.ti.com/lit/ds/symlink/lm317l.pdf?ts=1596796656076&ref_url=https%253A%252F%252Fwww.google.com%252F))

- **LM337LM** 3-Terminal Adjustable Regulator
([datasheet](https://www.ti.com/lit/ds/symlink/lm337l.pdf?ts=1596795119863&ref_url=https%253A%252F%252Fwww.google.com%252F))


- **LTC1446** Dual 12-Bit Rail-to-Rail Micropower DAC ([datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/1446fa.pdf)) 

- **MCP3202** 2.7V Dual Channel 12-Bit A/D Converter
with SPI Serial Interface ([datasheet](https://asset.conrad.com/media10/add/160267/c1/-/en/001083119DS01/datenblatt-1083119-microchip-technology-mcp3202-bisn-datenerfassungs-ic-analog-digital-wandler-adc-extern-soic-8-n.pdf))

- **MC33172** Low power dual bipolar operational amplifiers ([datasheet](https://www.st.com/resource/en/datasheet/mc33172.pdf))



- **KW1-391AGA** Single Digit 7-segment LED Display ([datasheet](https://www.luckylight.cn/media/component/data-sheet/KW1-391AGA.pdf))






## Next Steps

___

{% include image.html url="https://github.com/BorisJung/HM7044/blob/master/Pics/top.jpg?raw=true" description="Top Layer" %}

___

{% include image.html url="https://github.com/BorisJung/HM7044/blob/master/Pics/bottom.jpg?raw=true" description="Bottom Layer" %}
___
