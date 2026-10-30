# Open-Loop-Flyback-Convertor
Design and simulation of an isolated DC-DC Flyback converter in open loop with and without RCD clamp circuit to understand all different aspects of design. Implementation of soft-start to understand how transient surges are suppressed.

## Design Specifications
Input = 325V (DC)<br>
Output = 12V (DC)<br>
Output power = 24W<br>
Output current = 2A<br>
Switching frequency = 65 kHz<br>
Target efficiency = 85%<br>

## SCHEMATICS
Open-Loop isolated flyback convertor with RCD clamp(left) and without RCD clamp(right).
<p align="center">
  <img src="Screenshots/simulation/Schematics.png" width="48%">
  <img src="Screenshots/simulation/WITHOUT_CLAMP.png" width="48%">
</p>

## Design 
### Modes Of Operation
There are three possible modes of operation
1- Continous Conduction Mode(CCM)
2- Boundary Conduction Mode(BCM)
3- Discontinous Conduction Mode(DCM)

The flyback convertor here is designed for operation in Discontinuous conduction mode.

### GENERAL DESIGN 

![ GENERAL ](Screenshots/Design/1.jpeg)

### COUPLED INDUCTOR DESIGN 

![ Transformer ](Screenshots/Design/2.jpeg)

### DCM CONDITION 

![ DCM ](Screenshots/Design/3.jpeg)

### INDUCTANCE DESIGN 

![inductance](Screenshots/Design/4.jpeg)

### CAPACITOR DESIGN 

![ cap-1](Screenshots/Design/5.jpeg)
![cap-2](Screenshots/Design/6.jpeg)

### RCD CLAMP DESIGN

![clamp-1](Screenshots/Design/7.jpeg)
![clamp-2](Screenshots/Design/8.jpeg)


## TUNING
After running simulation with design elements the output voltage is found somewhere between 10-11V which is lesser then the required target ( incurring all the losses). So a open loop converter is unregulated and can give voltages that are off the target.

![Tuning ](Screenshots/Design/9.jpeg)

This is manual calibration and is done to match the design target.

## SIMULATION RESULTS
The following results are obtained after the simulation. Here the left hand side is results for circuit with RCD clamp and right hand side show results without one.

### PRIMARY SIDE 

<p align="center">
  <img src="Screenshots/simulation/1.png" width="48%">
  <img src="Screenshots/simulation/1_without_clamp.png" width="48%">
</p>

Without clamp the MOSFET drain voltage experiences a larger voltage spike during turn off this is due to energy stored in transformer leakage inductance that oscillates with drain capacitance. By applying clamp these spikes are reduced as clamp dissipate this energy through its resistance and thus protect MOSFET against excessive voltage stress.

### SECONDARY SIDE

<p align="center">
  <img src="Screenshots/simulation/2.png" width="48%">
  <img src="Screenshots/simulation/2_without_clamp.png" width="48%">
</p>

Without clamp there is much more ringing in primary and secondary side current. By applying clamp ringing are reduced and this reduce rms stress on diode and capacitor with less conduction losses.

### MOSFET SWITCHING LOSSES

<p align="center">
  <img src="Screenshots/simulation/mosfet_switchingloss_withclamp.png" width="48%">
  <img src="Screenshots/simulation/mos_switchingloss_without_clamp.png" width="48%">
</p>

These are instantaneous switching loss in MOSFET. With clamp the Vds overshoot and reduced and thus reduce instantaneous switching losses. 

### OUTPUT POWER 

![ Output Power ](Screenshots/simulation/3.png)


These simulation results show that RCD clamp effectively suppresses the voltage spikes caused by transformer leakage inductance and reduces MOSFET voltage stress.

## With vs Without RCD Clamp: Results
All values are measured over one steady-state switching period (5.01388 ms to 5.02926 ms).

### Power and efficiency

| Parameter | With clamp | Without clamp | Change |
| :--- | ---: | ---: | ---: |
| Input power (avg) | 26.92 W | 27.09 W | -0.6 % |
| Output power (avg) | 23.75 W | 23.88 W | -0.5 % |
| Total loss | 3.18 W | 3.21 W | -0.9 % |
| Input power (inst. peak) | 271 W | 275 W | -1.5 % |
| Output power (inst. peak) | 24.7 W | 25.3 W | -2.4 % |

### Currents

| Parameter | With clamp | Without clamp | Change |
| :--- | ---: | ---: | ---: |
| Primary I(L1), avg | 0.0856 A | 0.0833 A | +2.8 % |
| Primary I(L1), peak | 0.841 A | 0.845 A | -0.5 % |
| Secondary I(L2), avg | 1.989 A | 1.995 A | -0.3 % |
| Secondary I(L2), peak | 9.19 A | 11.96 A | -23.2 % |
| D1 current, RMS | 3.00 A | 3.18 A | -5.7 % |
| D1 current, peak | 9.19 A | 11.96 A | -23.2 % |
| MOSFET drain current, peak | 0.835 A | 0.845 A | -1.2 % |

### Voltages

| Parameter | With clamp | Without clamp | Change |
| :--- | ---: | ---: | ---: |
| Vout, avg | 11.94 V | 11.97 V | -0.3 % |
| Vout ripple (pk-pk) | 0.370 V | 0.482 V | -23.2 % |
| MOSFET Vds, peak | 499 V | 699 V | -28.6 % |
| Vsec, max | +13.3 V | +13.4 V | -0.7 % |
| Vsec, min | -46.6 V | -53.7 V | -13.2 % |

### MOSFET loss

| Parameter | With clamp | Without clamp | Change |
| :--- | ---: | ---: | ---: |
| Average loss | 0.563 W | 0.958 W | -41.2 % |

### Clamp 

| Parameter | Value |
| :--- | ---: |
| R4 loss, avg / peak | 0.460 W / 0.484 W |
| D2 reverse voltage, peak | 491 V |

## STARTUP TRANSIENTS
During the initial start of convertor the output capacitor is initially uncharged. When MOSFET switching operation begins initial surges are observed. This happens as we applied the complete duty-cycle altogether and due to this the output capacitor charges (act like short circuit) demanding high current.

![Tuning ](Screenshots/Transient_waveforms/without_soft_start/19.png)
![Tuning ](Screenshots/Transient_waveforms/without_soft_start/20.png)
![Tuning ](Screenshots/Transient_waveforms/without_soft_start/21.png)

| Quantity | Pulse start | Steady state | Ratio |
| :--- | ---: | ---: | ---: |
| Primary current I(L1), max | 3.64 A | 0.841 A | 4.3x |
| Secondary current I(L2), max | 37.5 A | 9.19 A | 4.1x |
| MOSFET Vds, max | 640 V | 499 V | +141 V |
| Vsec, min | -350 V | -46.6 V | 7.5x |
| Vout, max | 13.98 V | 11.94 V | 

### SOFT START 
Soft start is the most common technique applied to remove these surges. In this the duty cycle is gradually increased to the value over a time interval rather then applying complete. Two soft start with ramp times 1.5ms and 3ms are simulated as follow:

### 3ms RAMP

<p align="center">
  <img src="Screenshots/Transient_waveforms/with_soft_start/3ms_soft_start/16.png" width="48%">
  <img src="Screenshots/Transient_waveforms/with_soft_start/3ms_soft_start/17.png" width="48%">
</p>

![Tuning ](Screenshots/Transient_waveforms/with_soft_start/3ms_soft_start/18.png)


### 1.5ms RAMP

![Tuning ](Screenshots/Transient_waveforms/with_soft_start/1.5ms_soft_start/15.png)


The following table depicts the comparison over a safer period of 0-6ms.

| Quantity | Pulse start | 1.5 ms ramp | 3 ms ramp | Steady state |
| :--- | ---: | ---: | ---: | ---: |
| I(L1) max | 3.643 A | 0.932 A | 0.831 A | 0.841 A |
| I(L1) min | -1.143 A | -0.342 A | -0.340 A | - |
| I(L2) max | 37.54 A | 9.48 A | 9.16 A | 9.19 A |
| I(L2) min | -7.77 A | -2.11 A | -1.08 A | - |
| Vds max | 640.1 V | 496.6 V | 495.6 V | 499 V |
| Vout max | 13.977 V | 11.994 V | 11.892 V | 11.94 V (avg) |
| Overshoot above 12 V | +16.48 % | -0.05 % | -0.90 % | - |
| Vsec max | +15.16 V | +13.08 V | +12.97 V | +13.3 V |
| Vsec min | -350.3 V | -200.1 V | -104.0 V | -46.6 V |
| Output power peak | 32.56 W | 23.98 W | 23.57 W | 24.7 W |
| Rise time (10 % to 90 %) | 0.120 ms | 2.05 ms | 3.05 ms | - |
| Time to 95 % (11.4 V) | 0.157 ms | 3.17 ms | 4.46 ms | - |

Both ramps remove the surges by keeping the currents IL1 and IL2 close to steady state values and voltage don't overshoot. The 1.5ms ramp reaches 95% of output voltage earlier but has more overshoots as compared to 3ms ramp. The design trade-off depend entirely on the situation demand.

## NOTES
K = 0.9978 contributes about 5 µH of leakage in addition to the explicit 5 µH leakage inductor, so the simulated effective leakage is about 10 µH. This is why the clamp voltage, Vds peak (499 V) are higher than the hand-calculated values.

Pulse start gives Vsec = -350 V, and the RFUH20NS3S is rated 350 V for both repetitive peak and DC reverse voltage. So a suitable diode with higher rating can be used.
