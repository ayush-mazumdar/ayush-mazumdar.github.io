---
name: Discrete ADC with Successive Approximation
tools: [SystemVerilog, Vivado, FPGA]
image: /assets/img/projects/sar/cover.png
description: An FPGA-based ramp-compare analog-to-digital converter, with toggleable successive approximation.
---

# Discrete ADC with Successive Approximation

**Duration:** September 2025 – December 2025\
**Platform:** Basys 3 FPGA (Artix-7) | SystemVerilog, Vivado

---

## Base Design Overview

This project implements and compares multiple analog-to-digital conversion techniques using an FPGA. A potentiometer-controlled analog voltage serves as the input, and the system converts it to a digital reading using a comparator. The FPGA uses external PWM-based and R2R-ladder-based 8-bit digital-to-analog converters to generate a known ramp voltage. The comparator compares this known ramp value with the input voltage, outputting HIGH when the ramp voltage is higher than the input, and LOW when the ramp voltage is lower than the input. By incrementally increasing the ramp voltage and monitoring the exact location of the HIGH-to-LOW transition, the system can determine the exact value of the input voltage. The results are displayed on the onboard seven-segment display, and switches can be used to toggle between the 2 ADCs or the Basys 3's built-in XADC which serves as a reference for accuracy comparison.

---

## Hardware Architecture

The analog input signal is split into three paths from a single potentiometer connected to the FPGA's 3.3V rail:

| Path | Method | DAC Type | Comparator |
|---|---|---|---|
| Path 1 (Reference) | XADC (internal) | N/A | N/A |
| Path 2 | Ramp / SAR | R2R Ladder (10kΩ) | TLV3701 |
| Path 3 | Ramp / SAR | PWM + RC Lowpass (~16kHz)| TLV3701 |

Path 1 passes through a voltage divider (22kΩ / 10kΩ) to bring the maximum voltage within the XADC's 1V input range (this value is later scaled back to 3.3V in software). Paths 2 and 3 go directly into the positive inputs of two external TLV3701 comparators. The DAC outputs (R2R ladder on 8 GPIO pins for Path 2, RC-filtered PWM on a single GPIO pin for Path 3) feed the comparators' negative inputs.

![System Block Diagram](/assets/img/projects/sar/system-block-diagram.png)
*Hardware block diagram.*

---

## Key Software Modules

### Ramp ADC

The Ramp ADC module generates a continuously rising reference voltage: a sawtooth waveform for PWM, a triangle wave for R2R. It monitors the external comparator for the moment it crosses the unknown analog input, where the comparator output makes the HIGH-to-LOW transition. That crossing point's duty cycle is captured as the ADC result.

The controller is a 4-state Moore FSM: **IDLE → WAIT_LOW → CAPTURE → DONE**. The IDLE state waits for the ramp to reset near zero before starting fresh. WAIT_LOW monitors for the comparator's falling edge (ramp crossing the signal). CAPTURE latches the current duty cycle as the result. DONE pulses `conversion_done` and returns to IDLE.

A timeout condition handles edge cases: if the ramp completes its full sweep without detecting a crossing, the comparator state determines whether the result is pinned to 0xFF (input above full range) or 0x00 (input below minimum).

![Ramp ADC FSM](/assets/img/projects/sar/ramp-fsm.png)
*Moore FSM state diagram for the Ramp ADC controller.*

### SAR ADC

The SAR ADC is the more algorithmically interesting module. Rather than sweeping through all 256 values linearly like the ramps do, it converges on the result in exactly 8 comparisons using a binary search. It starts by testing the most significant digit first, working its way down and keeping or discarding each bit based on the comparator output until all 8 digits are determined.

The controller uses a 5-state Moore FSM: **IDLE → LOAD_TEST → SETTLE → SAMPLE → DONE**.

- **IDLE** — waits for a start signal, asserts `busy`
- **LOAD_TEST** — builds the test value by OR-ing the current bit position into the accumulated result so far
- **SETTLE** — counts a fixed number of clock cycles to let the DAC output stabilize before sampling
- **SAMPLE** — reads the comparator: if HIGH (input > DAC voltage), the bit is kept; if LOW, it is discarded. Loops back to LOAD_TEST for the next lower bit, or advances to DONE at the LSB
- **DONE** — outputs the final result, pulses `conversion_done`, clears `busy`, returns to IDLE

The settle delay is critical — without it, the comparator samples the DAC before it has stabilized, producing corrupted results. This is especially noticeable with the PWM path, where the RC filter introduces additional settling time that the R2R path doesn't need.

![SAR FSM](/assets/img/projects/sar/sar-fsm.png)
*5-state Moore FSM for the SAR ADC controller.*

### Display Controller

A MUX controlled by onboard switches selects which of the nine available signals to display (XADC raw/averaged/scaled, PWM raw/averaged/scaled, R2R raw/averaged/scaled). A separate switch toggles between hexadecimal and decimal display modes. In decimal voltage mode, a binary-to-BCD converter feeds the seven-segment subsystem and the decimal point is automatically placed at the correct position for X.XX volt formatting.

### PLL Clock Extension

This design extension pushed the system clock beyond the Basys 3's default 100 MHz using Vivado's PLL Clock Wizard IP. Timing closure was achieved iteratively: each time the clock period was tightened based on the Worst Negative Slack (WNS), Vivado re-optimized the netlist to fit the new constraint, yielding more slack than predicted. The final implementation achieved closure at **180 MHz** with a WNS of 0.447 ns, giving a theoretical maximum frequency of 184 MHz. This increased system clock noticeably improved the speed of the display windup, which occurred when booting or resetting the system.

---

## SAR in Action

<div class="embed-responsive embed-responsive-16by9 mb-3">
  <!-- REPLACE VIDEO_ID_HERE with your YouTube video ID -->
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/VIDEO_ID_HERE" title="SAR ADC Oscilloscope Demo" allowfullscreen></iframe>
</div>
<p class="text-center text-muted"><small>Oscilloscope capture of the SAR binary search converging on the input voltage — each step visible as the DAC output homes in from the MSB down.</small></p>

---

## Results

| Metric | Value |
|---|---|
| ADC Resolution | 8-bit (0–255) |
| Input Voltage Range | 0–3.3V |
| SAR Comparisons per Conversion | 8 (vs. 256 for Ramp) |
| Clock Frequency | 180 MHz (PLL) |
| Worst Negative Slack | 0.447 ns |
| Max Theoretical Frequency | 184 MHz |
| LUT Utilization | 12.5% |
| IO Utilization | 47.2% |

---


<p class="text-center">
{% include elements/button.html link="https://github.com/ayush-mazumdar" text="View on GitHub" %}
</p>
