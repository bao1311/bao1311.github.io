---
title: "Building a Buck Converter: From Requirements to Bench Test"
date: 2026-09-21 10:00:00 -0700
categories: [Power Electronics, Projects]
tags: [buck converter, dc-dc, power electronics, pcb, hardware]
description: A practical build log for designing, assembling, and validating a custom buck converter.
math: true
mermaid: true
---

<!--
  BUCK CONVERTER ARTICLE TEMPLATE
  Replace text inside [square brackets], add your own photos, and delete these
  comments before publishing. The page-specific styling lives in:
  /assets/css/jekyll-theme-chirpy.scss
-->

<div class="buck-article" markdown="1">

<section class="buck-hero" aria-labelledby="buck-hero-title">
  <div class="buck-hero__copy">
    <p class="buck-kicker">POWER ELECTRONICS · BUILD LOG 01</p>
    <h2 id="buck-hero-title">Turning <span>[12 V]</span> into a clean <span>[5 V]</span></h2>
    <p class="buck-lede">A hands-on record of the decisions, calculations, mistakes, and measurements behind my first custom step-down converter.</p>
    <div class="buck-hero__badges" aria-label="Project summary">
      <span>⚡ [3 A] output</span>
      <span>📐 [2-layer] PCB</span>
      <span>🌡️ [92%] target efficiency</span>
    </div>
  </div>
  <div class="buck-hero__visual" aria-hidden="true">
    <div class="buck-chip"><span>SW</span><i></i><i></i><i></i><i></i></div>
    <svg viewBox="0 0 360 190" role="img">
      <path class="buck-trace buck-trace--input" d="M6 95H92" />
      <path class="buck-trace buck-trace--switch" d="M145 95h42l8-18 16 36 16-36 16 36 16-18h94" />
      <circle cx="6" cy="95" r="5" />
      <circle cx="354" cy="95" r="5" />
      <path class="buck-pulse" d="M8 147h35v-27h35v27h35v-27h35v27h35" />
      <path class="buck-wave" d="M210 147c12-18 24-18 36 0s24 18 36 0 24-18 36 0" />
      <text x="4" y="78">VIN</text>
      <text x="322" y="78">VOUT</text>
      <text x="10" y="174">PWM</text>
      <text x="286" y="174">DC</text>
    </svg>
  </div>
</section>

<div class="buck-status">
  <span class="buck-status__dot"></span>
  <strong>Project status:</strong> [Planning / Prototyping / Tested]
  <span class="buck-status__divider"></span>
  <strong>Last bench run:</strong> [Month DD, YYYY]
</div>

## The challenge

[Open with the real reason for the project. What needs power, what supply is available, and why is an off-the-shelf module not the right answer? Keep this personal and concrete—it gives the calculations a purpose.]

> **Design brief:** Build a compact converter that accepts **[input range]**, regulates to **[output voltage]**, supplies **[maximum current]**, and survives **[the most important real-world constraint]**.
{: .prompt-info }

<div class="buck-spec-grid">
  <div class="buck-spec"><small>INPUT</small><strong>[9–16 V]</strong><span>Supply range</span></div>
  <div class="buck-spec"><small>OUTPUT</small><strong>[5.0 V]</strong><span>±[2]% target</span></div>
  <div class="buck-spec"><small>CURRENT</small><strong>[3.0 A]</strong><span>Continuous load</span></div>
  <div class="buck-spec"><small>SWITCHING</small><strong>[500 kHz]</strong><span>Nominal frequency</span></div>
</div>

## Architecture at a glance

![Buck converter power-flow diagram](/assets/img/posts/buck-converter/power-flow.svg){: .buck-wide-figure }
_Power moves left to right; the feedback path closes the loop around the output._

[Explain the topology in plain language. Introduce the controller or regulator IC, why you chose it, and which design constraints drove that choice.]

### Why this controller?

- **Input headroom:** [Why its voltage rating is appropriate.]
- **Control strategy:** [Current mode, voltage mode, constant on-time, etc.]
- **Practical features:** [Soft start, enable, current limit, thermal shutdown.]
- **Availability:** [Package, cost, and whether you can actually source it.]

## First-pass design

<div class="buck-equation-intro">
  <span>01</span>
  <div><strong>Start with the duty cycle</strong><br><small>This is the ideal estimate; losses will move the real operating point.</small></div>
</div>

For an ideal buck converter:

$$D \approx \frac{V_{OUT}}{V_{IN}} = \frac{[5\,V]}{[12\,V]} = [0.417]$$

### Inductor selection

I chose a ripple-current target of **[20–40]%** of the maximum output current:

$$\Delta I_L = [0.30] \times I_{OUT(max)} = [0.90\,A]$$

$$L = \frac{V_{OUT}(1-D)}{f_{SW}\Delta I_L} = [\;\;\;\mu H]$$

[Explain the standard value you selected and verify that its saturation-current rating has enough margin.]

### Feedback network

For a regulator with reference voltage $V_{REF}$:

$$V_{OUT} = V_{REF}\left(1 + \frac{R_{TOP}}{R_{BOTTOM}}\right)$$

| Part | Calculated | Selected | Key rating / reason |
|---|---:|---:|---|
| Inductor | [x µH] | [x µH] | [Isat, DCR] |
| Input capacitor | [x µF] | [x µF] | [voltage, ripple current] |
| Output capacitor | [x µF] | [x µF] | [ESR, voltage] |
| High-side feedback resistor | [x kΩ] | [x kΩ] | [tolerance] |
| Low-side feedback resistor | [x kΩ] | [x kΩ] | [tolerance] |

> Check the controller datasheet's equations and limits before committing the board. The ideal equations above are a starting point, not a substitute for the device-specific design procedure.
{: .prompt-warning }

## Schematic decisions

<!-- Replace the placeholder path below with your exported schematic image. -->

<div class="buck-media-placeholder" role="img" aria-label="Schematic image placeholder">
  <span>SCHEMATIC / REV A</span>
  <strong>Drop your exported schematic here</strong>
  <small>/assets/img/posts/buck-converter/schematic.png</small>
</div>

Use this section to call out decisions that are easy to miss in a full-page schematic:

1. **Protection:** [Reverse-polarity, fuse, TVS, or input filtering.]
2. **Gate-drive / bootstrap loop:** [What the datasheet requires and how you implemented it.]
3. **Feedback filtering:** [Any feed-forward capacitor or noise filtering.]
4. **Test access:** [VIN, VOUT, GND, SW, EN, and current-measurement points.]

## PCB layout: where the converter is won or lost

<div class="buck-layout-notes">
  <div><b>A</b><span><strong>Keep the hot loop tiny</strong><br>Input capacitor → switch → catch path → ground.</span></div>
  <div><b>B</b><span><strong>Protect the feedback node</strong><br>Route it away from SW and sample a quiet output point.</span></div>
  <div><b>C</b><span><strong>Give heat a path out</strong><br>Use copper area and thermal vias where the package allows.</span></div>
</div>

<!-- Replace this placeholder with a top/bottom PCB render or layout screenshot. -->

<div class="buck-media-placeholder buck-media-placeholder--board" role="img" aria-label="PCB layout image placeholder">
  <span>PCB / REV A</span>
  <strong>Add the board render or layout view</strong>
  <small>Annotate the hot loop, switch node, and feedback route</small>
</div>

[Describe what you changed between your first placement and the final layout. This is often the most valuable part of a hardware build log.]

## Bring-up plan

<div class="buck-timeline">
  <div><span>01</span><p><strong>Unpowered checks</strong><br>Inspect polarity, continuity, and VIN-to-GND resistance.</p></div>
  <div><span>02</span><p><strong>Current-limited start</strong><br>Begin at [voltage] with a [current] supply limit and no load.</p></div>
  <div><span>03</span><p><strong>Waveform check</strong><br>Probe SW, output ripple, and startup with a short ground spring.</p></div>
  <div><span>04</span><p><strong>Load sweep</strong><br>Increase in steps while watching regulation and temperature.</p></div>
</div>

> **Before power-up:** [Record the exact current limit, initial input voltage, and the stop conditions you will use.]
{: .prompt-danger }

## Bench results

<div class="buck-result-grid">
  <div><small>REGULATION</small><strong>[±0.8%]</strong><span>0 → [3 A]</span></div>
  <div><small>PEAK EFFICIENCY</small><strong>[91.7%]</strong><span>at [load]</span></div>
  <div><small>OUTPUT RIPPLE</small><strong>[24 mV]</strong><span>peak-to-peak</span></div>
  <div><small>HOTTEST PART</small><strong>[68 °C]</strong><span>at full load</span></div>
</div>

| Load | VIN | VOUT | IOUT | PIN | POUT | Efficiency |
|---:|---:|---:|---:|---:|---:|---:|
| [10%] | [12.0 V] | [5.01 V] | [0.30 A] | [x W] | [x W] | [x%] |
| [50%] | [12.0 V] | [5.00 V] | [1.50 A] | [x W] | [x W] | [x%] |
| [100%] | [12.0 V] | [4.98 V] | [3.00 A] | [x W] | [x W] | [x%] |

$$\eta = \frac{P_{OUT}}{P_{IN}} \times 100\%$$

### Waveforms

<!-- Add your oscilloscope screenshots. Keep captions explicit about probe setup and bandwidth limit. -->

<div class="buck-scope-grid">
  <figure><div class="buck-scope"><i></i><i></i><i></i></div><figcaption><strong>Switch node</strong><br>[VIN, load, time/div, volts/div]</figcaption></figure>
  <figure><div class="buck-scope buck-scope--ripple"><i></i><i></i><i></i></div><figcaption><strong>Output ripple</strong><br>[Load, bandwidth limit, probe method]</figcaption></figure>
</div>

## What did not work

[Document at least one surprise: ringing, excess heat, poor transient response, wrong footprint, unstable startup, or measurement error. Explain the symptom, the likely cause, and the change you made. Honest debugging makes the article useful.]

<div class="buck-retro">
  <div><small>OBSERVED</small><p>[What you saw on the bench.]</p></div>
  <div><small>ROOT CAUSE</small><p>[What evidence pointed to the cause.]</p></div>
  <div><small>REV B CHANGE</small><p>[The exact schematic or layout fix.]</p></div>
</div>

## Final thoughts

[Summarize whether the converter met the original design brief. Mention the one design choice you would keep and the one you would change on a second revision.]

### Project files

- [Schematic PDF](#) — [revision and date]
- [BOM](#) — [supplier / pricing note]
- [PCB source files](#) — [EDA tool and version]
- [Measurement data](#) — [CSV or notebook]

<div class="buck-next">
  <span>NEXT ITERATION</span>
  <h3>[Rev B: lower ripple, better thermal margin, and a smaller board]</h3>
  <p>[Add one sentence about the next experiment so the article ends with momentum.]</p>
</div>

</div>
