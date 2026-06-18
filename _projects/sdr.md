---
name: Software-Defined AM Radio
tools: [SystemVerilog, Vivado, Python, FPGA]
image: /assets/img/projects/sdr/cover.png
description: An FPGA-based software-defined AM radio built from the ground up, including a fully digital DSP pipeline with a custom PCB.
---

# Software-Defined AM Radio

**Duration:** January 2026 – April 2026\
**Role:** Project Lead & Software Lead | 6-person team

![SDR Cover](/assets/img/projects/sdr/cover.png)

A fully digital, software-defined AM radio, designed, built, and tuned from scratch on the Basys-3 FPGA. This project covers the complete signal chain: RF front end, custom PCB, and a SystemVerilog DSP pipeline including FIR filtering, demodulation, and DC offset correction.

---

## The Spark: Where This Started

<!--
  REPLACE THIS WITH YOUR OWN STORY:
  Write 1-2 short paragraphs here about the BJT AM radio from your transistors
  class — what it was, why the audio quality was poor, and what specifically
  got you hooked on RF. This is the "origin story" section that gives the
  whole project narrative weight before jumping into the SDR itself.
-->

It started in a transistors course, where I built a simple BJT-based AM radio receiver on a breadboard. It worked... barely. The audio was faint, noisy, and several stations could be heard at once, but hearing an actual broadcast come through a circuit I'd built was enough to get me hooked on RF. That curiosity is what eventually became this project: an attempt to take the same basic idea and rebuild it the right way, replacing analog guesswork with a fully digital, precisely tunable signal chain.

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE VIDEO_ID_HERE with your YouTube video ID (the part after watch?v= in the URL) -->
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/gyfGUxuU7ds" title="BJT AM Radio Demo" allowfullscreen></iframe>
</div>
<p class="text-center text-muted"><small>BJT AM radio, transistors class: functional, but noisy and quiet.</small></p>

---

### Breadboard Demo

The first SDR iteration. It had a sharp FIR filter to prevent adjacent-channel bleeding, and an LM386 audio amplifier was used to bring up the volume. Before committing to a custom PCB, the full signal chain was validated on a breadboard to confirm that the DSP pipeline and RF front end worked together.

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE VIDEO_ID_HERE with your YouTube video ID -->
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/z7dXpS4ZZGg" title="Breadboard Demo" allowfullscreen></iframe>
</div>
<p class="text-center text-muted"><small>Breadboard validation: testing the signal chain before PCB fabrication.</small></p>

## Final Demo

This was the result of our efforts. A custom PCB, gain adjustments, and some nifty coding were used to nearly eliminate noise. Volume control and channel selection were integrated, allowing the user to tune in to all 5 local AM stations. The addition of a screen allowed for the display of the current channel and the FFT audio spectrum. And the whole system was encased in a 3D-printed enclosure.

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE VIDEO_ID_HERE with your YouTube video ID -->
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/mcBP4-E0OeI" title="Final SDR Demo" allowfullscreen></iframe>
</div>
<p class="text-center text-muted"><small>Final system: Full functionality with custom PCB and 3D-printed shell.</small></p>

---

## Design Specifics

<!--
  Write freely below — this is the narrative section. Suggested structure left
  as headers; feel free to add/remove/reorder. Each <details> block is collapsed
  by default so the page stays skimmable, but expands for anyone who wants depth.
-->

<details open>
<summary><strong>Front End Hardware</strong></summary>

<!-- Why an AM SDR specifically? What was the goal beyond "improve on the transistors-class radio"? -->
We built a Tayloe detector to act as our RF front end. This system uses a 74CBTLV3253D 1:4 demultiplexer that splits the signal from the antenna into 4 in-phase and quadrature signals (I+, I-, Q+, Q-), where each signal is delayed by a 90-degree offset. The in-phase and quadrature signals are then combined by their own MCP6022 differential amplifier, before being input into the FPGA. By controlling the rate at which the demux select-bits change, we can essentially mix the antenna signals with any signal of our choice. In this way, the Tayloe detector acts as a coherent demodulator, and has the additional benefit of converting a high-frequency signal down to baseband.
</details>

<details>
<summary><strong>DSP Chain Overview</strong></summary>

<!-- What was the first working version? What did it get right/wrong? -->
The Basys-3 starts by sending out demux select-bits at a rate equal to the frequency of the desired AM radio station, allowing the FPGA to fill the role of a local oscillator. Then, the Basys-3 uses its integrated XADC to sample both I and Q. Thanks to the Tayloe detector, our desired signal is at roughly 0 Hz, and since adjacent AM stations are 10 KHz apart, we can design an FIR low-pass filter with a sharp cutoff at 8 KHz to eliminate channel bleeding. I made a 128-tap FIR filter using coefficients generated by Scipy's firwin function. The I and Q signals are then put through an envelop-detector demodulator, which just uses the CORDIC IP to calculate the magnitude if the vector [I,Q]. The demodulated signal is then passed through a simple IIR DC block to remove any low-frequency noise before the audio is output using a 10-bit, 97.7kHz PWM DAC.
</details>

<details>
<summary><strong>Audio Output Circuit</strong></summary>

<!-- What changed and why? What problem prompted this iteration? -->
The PWM output is put through a low-pass reconstruction filter, becoming a smooth analog voltage. This audio signal becomes the input of an LM386 audio amplifier, which provides a gain of 26 dB, before the audio is used to drive two series 8-ohm speakers.
</details>

<details>
<summary><strong>The Time Sink</strong></summary>

<!-- What's different in the final version? What pushed SNR from 9.8 dB to 30.6 dB? -->
Due to our heavy budget constraints, we decided to make use of the existing features of the Basys-3 to prevent buying external ADCs and local oscillators, which made the design process much more difficult. There were small inaccuracies in the local oscillator. The XADC sampled I and Q sequentially, meaning that there was a small delay between an I signal and its corresponding Q signal. Because of these hardware issues, the CORDIC demodulator was unable to completely get rid of the carrier frequency, which left a bubbling effect on the output that caused the audio to cut in and out sinusoidally. Ultimately, the local oscillator inaccuracy was mitigated by the 4-layer PCB, and the I/Q offset was fixed by intentionally delaying "I", leading to the clean audio shown in our final demo. However, many hours were spent trying to diagnose and patch these issues.
</details>

<!-- #<details>
#<summary><strong>Biggest Challenges</strong></summary>

<!-- PCB noise debugging, timing closure, team coordination, whatever was hardest -->

<!--</details>

<details>
<summary><strong>What I'd Do Differently</strong></summary>

<!-- Optional but strong for interviews — shows reflection and engineering maturity -->

<!--</details> -->

---

## System Stats

| Metric | Value |
|---|---|
| Operating Voltage | <!-- e.g. 3.3V logic / 5V analog --> |
| Continuous Runtime | <!-- e.g. Tested stable over 4+ hours --> |
| Software Latency (end-to-end) | <!-- e.g. ~12 ms input to audio out --> |
| FIR Filter Cutoff | <!-- e.g. 5 kHz low-pass, 64-tap --> |
| PWM Resolution | <!-- e.g. 10-bit @ 48 kHz --> |
| Signal-to-Noise Ratio | 9.8 dB → 30.6 dB |
| Clock Frequency | <!-- e.g. 100 MHz system clock --> |
| ADC Sampling Rate | <!-- fill in --> |

---

## Design Gallery

<!--
  CAROUSEL — each slide is one labelled image. Duplicate the <div class="carousel-item">
  block for each picture (PCB layouts, block diagrams, circuit schematics, enclosure/shell
  renders). Just change the image src and the caption text. Order them however tells the
  best visual story — e.g. block diagram first, then schematic, then PCB, then enclosure.
-->
<style>
  #sdrCarousel .carousel-item.active {
    display: flex;
    flex-direction: column;
  }
  #sdrCarousel .carousel-caption {
    position: static;
    background-color: rgba(255, 255, 255, 0.85) !important;
    color: #000 !important;
  }
  #sdrCarousel .carousel-caption p {
    color: #000 !important;
    margin: 0;
  }
  #sdrCarousel .carousel-control-prev-icon,
  #sdrCarousel .carousel-control-next-icon {
    filter: invert(1) grayscale(100);
  }
</style>

<div id="sdrCarousel" class="carousel slide mb-4" data-ride="false">

  <div class="carousel-inner">

    <div class="carousel-item active">
      <img src="/assets/img/projects/sdr/block-diagram.png" class="d-block w-100" alt="System block diagram">
      <div class="carousel-caption d-none d-md-block">
        <p>System block diagram — RF front end through DSP pipeline to audio output.</p>
      </div>
    </div>

    <div class="carousel-item">
      <img src="/assets/img/projects/sdr/circuit-schematic.png" class="d-block w-100" alt="Circuit schematic">
      <div class="carousel-caption d-none d-md-block">
        <p>Circuit schematic for the analog front end.</p>
      </div>
    </div>

    <div class="carousel-item">
      <img src="/assets/img/projects/sdr/pcb-layout.png" class="d-block w-100" alt="PCB layout">
      <div class="carousel-caption d-none d-md-block">
        <p>Custom PCB layout, designed for the analog-to-digital interface.</p>
      </div>
    </div>

    <div class="carousel-item">
      <img src="/assets/img/projects/sdr/enclosure.png" class="d-block w-100" alt="Enclosure design">
      <div class="carousel-caption d-none d-md-block">
        <p>Final enclosure design.</p>
      </div>
    </div>

  </div>

  <a class="carousel-control-prev" href="#sdrCarousel" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#sdrCarousel" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

---

<p class="text-center">
{% include elements/button.html link="https://github.com/ayush-mazumdar" text="View on GitHub" %}
</p>
