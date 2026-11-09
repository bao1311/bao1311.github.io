---
layout: post
title: "My Flyback Simulation Journey"
date: 2026-10-22 10:00:00 -0400
categories: [Power Electronics, Projects]
tags: [flyback converter, simulation, power electronics, dc-dc]
description: "Learning to understand a flyback circuit in LTspice."
published: true
toc: false
---

Early in my power electronics career, my boss asked me to simulate an ADC and calculate its error. That sounded manageable. Then he asked me to simulate the flyback circuit used in a 12 kW unit.

I spent three weeks on it before giving up. I was new to the field, and the schematic felt overwhelming. Eventually, my boss simulated it himself.

At the time, I was relieved. Looking back, I wish I had stayed with it. The assignment was a chance to learn from an experienced engineer with a PhD. As a new graduate, his feedback and recognition would have meant a lot to me.

Difficult projects can offer more than a finished result. This one offered guidance I wish I had made better use of.

## The controller

The hardware uses a UC3845. For the LTspice simulation, I chose an LT1245 as a similar controller. Differences between the devices still matter when comparing the simulation with the hardware.

The LT1245 is an eight-pin, current-mode PWM controller in the LT1241 series. Its pins handle feedback, timing, current sensing, and MOSFET gate drive:

- **COMP:** Error-amplifier output and compensation.
- **FB:** Feedback input.
- **ISENSE:** Current-sense input.
- **RT/CT:** Oscillator timing, set by an external resistor and capacitor.
- **GND:** Controller ground.
- **OUTPUT:** MOSFET gate drive.
- **VCC:** Controller supply.
- **VREF:** Nominal 5 V reference.

These are controller functions. The reference voltage and gate-drive current are separate from the converter's output voltage and load current. The [LT1245 documentation](https://www.analog.com/en/products/lt1245.html) provides the device specifications.

## The feedback loop

The TL431 and PC817 optocoupler are the parts I want to understand better. They connect the output-voltage feedback to the primary-side controller while maintaining isolation.

In a typical circuit, a resistor divider samples the output voltage. The TL431 responds by adjusting the optocoupler LED current. The optocoupler passes that feedback across the isolation barrier, allowing the controller to adjust the switching action. Texas Instruments describes this in its [optocoupler feedback guide](https://www.ti.com/document-viewer/lit/html/SSZTCW5/GUID-176CD139-5619-4F61-9C64-F032884185A1).

Following that signal—from the output divider, through the TL431 and PC817, back to the controller—gives me a smaller part of the circuit to study first.
