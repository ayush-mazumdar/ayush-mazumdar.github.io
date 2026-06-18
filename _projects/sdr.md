---
name: Software-Defined AM Radio
tools: [SystemVerilog, Vivado, Python, FPGA, PCB Design]
image: /assets/img/projects/sdr/cover.png
description: An FPGA-based software-defined AM radio built from the ground up — from a noisy BJT receiver in a transistors class to a fully digital DSP pipeline with a custom PCB.
---

# Software-Defined AM Radio

**Duration:** January 2026 – April 2026
**Role:** Project Lead & Software Lead | 6-person team

![SDR Cover](/assets/img/projects/sdr/cover.png)

A fully digital, FPGA-based software-defined AM radio — designed, built, and tuned from scratch. This project covers the complete signal chain: RF front end, custom PCB, and a SystemVerilog DSP pipeline including FIR filtering, demodulation, and DC offset correction.

---

## The Spark: Where This Started

<!--
  REPLACE THIS WITH YOUR OWN STORY:
  Write 1-2 short paragraphs here about the BJT AM radio from your transistors
  class — what it was, why the audio quality was poor, and what specifically
  got you hooked on RF. This is the "origin story" section that gives the
  whole project narrative weight before jumping into the SDR itself.
-->

It started in a transistors course, where I built a simple BJT-based AM radio receiver on a breadboard. It worked — barely. The audio was faint, noisy, and unstable, but hearing an actual broadcast come through a circuit I'd built myself was enough to get me hooked on RF. That curiosity is what eventually became this project: an attempt to take the same basic idea and rebuild it the right way, replacing analog guesswork with a fully digital, precisely tunable signal chain.

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE SRC: upload your .mov as .mp4 to assets/video/sdr/bjt-radio-demo.mp4 -->
  <video class="embed-responsive-item" controls poster="/assets/img/projects/sdr/bjt-poster.png">
    <source src="/assets/video/sdr/bjt-radio-demo.mp4" type="video/mp4">
    Your browser does not support embedded video.
  </video>
</div>
<p class="text-center text-muted"><small>BJT AM radio, transistors class — functional, but noisy and unstable.</small></p>

---

### Breadboard Demo

Before committing to a custom PCB, the full signal chain was validated on a breadboard to confirm the DSP pipeline and RF front end worked together before investing in fabrication.

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE SRC: upload your .mov as .mp4 to assets/video/sdr/breadboard-demo.mp4 -->
  <video class="embed-responsive-item" controls poster="/assets/img/projects/sdr/breadboard-poster.png">
    <source src="/assets/video/sdr/breadboard-demo.mp4" type="video/mp4">
    Your browser does not support embedded video.
  </video>
</div>
<p class="text-center text-muted"><small>Breadboard validation — proving out the signal chain before PCB fabrication.</small></p>

## Final Demo

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE SRC: upload your .mov as .mp4 to assets/video/sdr/final-demo.mp4 -->
  <video class="embed-responsive-item" controls poster="/assets/img/projects/sdr/final-poster.png">
    <source src="/assets/video/sdr/final-demo.mp4" type="video/mp4">
    Your browser does not support embedded video.
  </video>
</div>
<p class="text-center text-muted"><small>Final system — custom PCB, full DSP pipeline, enclosure.</small></p>

---

## System Stats

<div class="table-responsive mb-4">
<table class="table table-bordered table-hover">
  <thead>
    <tr>
      <th>Metric</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Operating Voltage</td><td><!-- e.g. 3.3V logic / 5V analog --></td></tr>
    <tr><td>Continuous Runtime</td><td><!-- e.g. Tested stable over 4+ hours --></td></tr>
    <tr><td>Software Latency (end-to-end)</td><td><!-- e.g. ~12 ms input to audio out --></td></tr>
    <tr><td>FIR Filter Cutoff</td><td><!-- e.g. 5 kHz low-pass, 64-tap --></td></tr>
    <tr><td>PWM Resolution</td><td><!-- e.g. 10-bit @ 48 kHz --></td></tr>
    <tr><td>Signal-to-Noise Ratio</td><td>9.8 dB → 30.6 dB</td></tr>
    <tr><td>Clock Frequency</td><td><!-- e.g. 100 MHz system clock --></td></tr>
    <tr><td>ADC Sampling Rate</td><td><!-- fill in --></td></tr>
  </tbody>
</table>
</div>

---

## Design Gallery

<!--
  CAROUSEL — each slide is one labelled image. Duplicate the <div class="carousel-item">
  block for each picture (PCB layouts, block diagrams, circuit schematics, enclosure/shell
  renders). Just change the image src and the caption text. Order them however tells the
  best visual story — e.g. block diagram first, then schematic, then PCB, then enclosure.
-->
<div class="carousel-item active">
  <img src="/assets/img/projects/sdr/block-diagram.png" class="d-block w-100" alt="System block diagram">
</div>

<div class="carousel-item">
  <img src="/assets/img/projects/sdr/circuit-schematic.png" class="d-block w-100" alt="Circuit schematic">
</div>

<div class="carousel-item">
  <img src="/assets/img/projects/sdr/pcb-layout.png" class="d-block w-100" alt="PCB layout">
</div>

<div class="carousel-item">
  <img src="/assets/img/projects/sdr/enclosure.png" class="d-block w-100" alt="Enclosure design">
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
  <ol class="carousel-indicators">
    <li data-target="#sdrCarousel" data-slide-to="0" class="active"></li>
    <li data-target="#sdrCarousel" data-slide-to="1"></li>
    <li data-target="#sdrCarousel" data-slide-to="2"></li>
    <li data-target="#sdrCarousel" data-slide-to="3"></li>
  </ol>
  <p class="carousel-caption-external" id="sdrCaptionText">System block diagram — RF front end through DSP pipeline to audio output.</p>
</div>
<script>
  (function () {
    var captions = [
      "System block diagram — RF front end through DSP pipeline to audio output.",
      "Circuit schematic for the analog front end.",
      "Custom PCB layout, designed for the analog-to-digital interface.",
      "Final enclosure design."
    ];
    var carousel = document.getElementById('sdrCarousel');
    var captionEl = document.getElementById('sdrCaptionText');
    if (carousel) {
      carousel.addEventListener('slid.bs.carousel', function (e) {
        captionEl.textContent = captions[e.to] || "";
      });
    }
  })();
</script>

---

## Design Process

<!--
  Write freely below — this is the narrative section. Suggested structure left
  as headers; feel free to add/remove/reorder. Each <details> block is collapsed
  by default so the page stays skimmable, but expands for anyone who wants depth.
-->

<details open>
<summary><strong>Background & Motivation</strong></summary>

<!-- Why an AM SDR specifically? What was the goal beyond "improve on the transistors-class radio"? -->
test 1
</details>

<details>
<summary><strong>Iteration 1 — Initial Architecture</strong></summary>

<!-- What was the first working version? What did it get right/wrong? -->
test2
</details>

<details>
<summary><strong>Iteration 2 — Refinement</strong></summary>

<!-- What changed and why? What problem prompted this iteration? -->

</details>

<details>
<summary><strong>Iteration 3 — Final Design</strong></summary>

<!-- What's different in the final version? What pushed SNR from 9.8 dB to 30.6 dB? -->

</details>

<details>
<summary><strong>Biggest Challenges</strong></summary>

<!-- PCB noise debugging, timing closure, team coordination, whatever was hardest -->

</details>

<details>
<summary><strong>What I'd Do Differently</strong></summary>

<!-- Optional but strong for interviews — shows reflection and engineering maturity -->

</details>

---

<p class="text-center">
{% include elements/button.html link="https://github.com/ayush-mazumdar" text="View on GitHub" %}
</p>
