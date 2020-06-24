---
layout: default
title: BRIR from ISM
---


```
(under construction)
```

# Low Jitter Hard Limiter Design Tool

<a id="forkme_banner" href="https://github.com/BorisJung/Low_Jitter_Hard_Limiter_Design_Tool#low-jitter-hard-limiter-design-tool">View GitHub Repository</a>

This MatLab tool was developed while designing the hard limiter input stage for the phase noise measurement system during the work on my Bachelor's Thesis.

It makes use of the design guide for low jitter hard limiters by Oliver M. Collins published in the following IEEE paper:

[The Design of Low Jitter Hard Limiters](https://ieeexplore.ieee.org/document/494304)

The script is meant to be used by executing jitter_calc.m in MatLab. It starts with a user input window, where the given design parameters from the collins paper are entered. The script then calculates gain distribution, time constants, resulting jitter and some other parameters.


![inputDlg_pic](https://github.com/BorisJung/Low_Jitter_Hard_Limiter_Design_Tool/blob/master/pics/limiterCalc.png?raw=true)


![results](https://github.com/BorisJung/Low_Jitter_Hard_Limiter_Design_Tool/blob/master/pics/limiterCalc_results.png?raw=true)
