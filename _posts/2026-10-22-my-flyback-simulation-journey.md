---
title: "My Flyback Simulation Journey"
date: 2026-10-22 10:00:00 -0400
categories: [Power Electronics, Projects]
tags: [flyback converter, simulation, power electronics, dc-dc]
description: How did I successfully simulate my flyback circuit in LTspice 
published: true
---

<!--
Plain draft: replace the bracketed prompts with your own experience.
Preview with: bundle exec jekyll serve --livereload --unpublished
When ready to publish, remove the "published: false" line above.
-->

At the very beginning of my Power Electronics career, my boss first told me to simulate and calculate the error of ADC(which sounds handleable) but right after that, he told me to simulate the whole flyback circuit of a 12kw unit.

## Why I started

It took me 3 weeks and I gave up afterwards since being so immature in the Power Electronics field and the circuit looked too much complicated. 

Then, my boss simulated the flyback circuit by himself, LOL. I thought I escaped one of the most horrible nightmare of my career but turn out that is an absurd thought. If I had a chance to go back in time, I would put my head all in with the simulation. And I will tell you why. 

The world is like a game simulation. When you received a tough quest, you don't run away from it. Hard mission hides away the valuable prize that you won't imagine. If ever, I tried my best to simulate the circuit on that day, what I would receive is his feedback and recognition (from a PHD). You guys wouldn't understand how big it is to a new grad who have no exps in Power Electronics industry.

## What flyback converter IC that I am using?

For the project, we are using UC3845. However, in the simulation, I picked LT1245 which is an equivalent to UC3845.

LT1245 is in LT1241 series. In the end, it is a DC/DC converter which has 8 pins.

| Parameter | Target |
| --- | --- |
| Supply voltage | 25V |
| Input voltage | [Voltage or range] |
| Output voltage | 5V Typical |
| Output current | 1A |
| Power Dissipation at Ta < 25C | 1W |
| Switching frequency | To 500kHz |

There is 8 pins in the package that I am using:
- COMP (Pin 1)
- FB (Pin 2)
- Isense (Pin 3): Input to the current sense comparator
- Rt/Ct (Pin 4): Oscillator pin which set the frequency. This is defined by connecting a resistor Rt from Vref to Rt/Ct and a capacitor from Rt/Ct to GND
- GND (Pin 5)
- Output (Pin 6): This is output of high current totem pole output stage. Capable of driving up to 1A of current into capacitive load such as gate of MOSFET
- VCC (Pin 7): Voltage supply of the IC
- Vref (Pin 8): Reference pin. It would be 5V when VCC > 8.4V


## My first simulation

[Which simulator did you use? Describe your starting circuit, transformer model, switching signal, and load.]

<!-- Add your schematic when ready:
![My initial flyback simulation circuit](/assets/img/flyback/initial-circuit.png)
-->

## What happened on the first run

[Describe what you actually saw. Did the output reach the target? Which waveforms caught your attention? Add measured values when you have them.]

## The problems I ran into

### [First problem]

[What looked wrong? What did you think caused it? What did you change, and what happened afterward?]

### [Next problem]

[Repeat the same story for another issue, if needed.]

## Comparing the results

[Add the waveforms and results that best show your progress. Include the simulation conditions so the comparison is clear.]

| Measurement | First run | After changes |
| --- | --- | --- |
| Output voltage | [Value] | [Value] |
| Output voltage ripple | [Value] | [Value] |
| Peak switch voltage | [Value] | [Value] |
| Peak primary current | [Value] | [Value] |

## What I learned

[Explain the lessons in your own words: what surprised you, which assumptions changed, and what you now understand better.]

## What comes next

[What will you simulate, improve, or verify next?]
