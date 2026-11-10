---
layout: post
title: "Caveats When Using Altium Designer: Notes for a PCB Learner"
date: 2026-11-10 00:00:00 -0500
categories: [PCB Design, Learning Notes]
tags: [Altium Designer, PCB, footprints, design rules, electronics]
description: "A learner-friendly checklist for footprints, schematic updates, design rules, copper pours, and manufacturing files in Altium Designer."
published: true
toc: true
---

Altium Designer is a powerful tool for drawing schematics and designing PCBs. I’m learning it through a practical task: turning my manager’s circuit design into a schematic.

## My first assignment

The first circuit I was asked to draw used a **precision reinforced isolated amplifier**. My job was to capture the design in Altium and check it against my manager’s schematic.

## Why we needed isolation

The earlier sensing circuit used an op-amp with a shared ground between the input and output. My manager identified the need to isolate the high-voltage sensing side from the controller circuit, which operates mainly at 3.3 V and 5 V.

We switched to an isolated amplifier to separate those two sides while still passing the measurement signal to the controller.

## A helpful feature: integrated isolated power

The part we selected also includes isolated power transfer. It powers the high-side sensing circuitry from the low-voltage supply, so we don’t need a separate high-side supply.

That feature depends on the specific IC. When choosing a part, check its datasheet for the supply requirements and whether isolated power is included.

## Bringing the design into Altium

With the circuit design provided by my manager, I could focus on drawing and checking the schematic. I’ll break down the steps here, starting with the component library.

### 1. Find the symbol and footprint

I mostly use **Ultra Librarian** to find the IC’s schematic symbol and PCB footprint, then add them to the library.

![Finding the component CAD models on Ultra Librarian](/assets/img/altium_designer/ultra_librarian.png)

Next, select **Altium Designer** as the CAD platform and download the ZIP file.

![Selecting the CAD platform and downloading the component files](/assets/img/altium_designer/cad_download.png)

It’s a helpful starting point. Before using them, I still need to compare the symbol’s pin assignments and the footprint’s pad numbering and dimensions with the datasheet for the exact part.
