# SPICE Simulations

# NgspiceSky130 – CMOS Inverter Design and Simulation

A comprehensive guide for **CMOS inverter design, simulation, and analysis** using **Ngspice with the SkyWater 130nm PDK (sky130)**.  
This repository covers theoretical foundations and practical SPICE implementations — including NMOS, PMOS, and CMOS inverter behavior across resistive, saturation, and velocity saturation regions.

---

### Basics of NMOS Drain Current (Id) vs. Drain-to-Source Voltage (Vds)

#### 1. Introduction to Circuit Design and SPICE Simulations
SPICE (Simulation Program with Integrated Circuit Emphasis) is an industry-standard circuit simulator that models transistor-level and system-level behavior.  
It allows circuit designers to verify functionality, noise margins, power, and timing before fabrication.

<div align="center" >
  <img src="./assets/CMOS_inverter.png" alt="CMOS_inverter" width="80%">
</div>

<div align="center" >
  <img src="./assets/Spice_sim.png" alt="Spice_sim" width="80%">
</div>

**Why Do We Need SPICE Simulations?**
- Enables transistor-level verification of non-linear and parasitic effects.
- Reduces fabrication iterations by simulating circuits virtually.
- Helps measure design metrics such as noise margin, power, and delay.
- Power Aware CTS and Delay Table

<div align="center" >
  <img src="./assets/DelayTable.png" alt="DelayTable" width="80%">
</div>

<div align="center" >
  <img src="./assets/QuesWeShouldAns.png" alt="QuesWeShouldAns" width="80%">
</div>

**NMOS as Basic Element in Circuit Design** \
An NMOS transistor acts as a switch controlled by gate voltage $(V_g)$. When $(V_{gs} > V_t)$, a conduction channel is formed between source and drain.

**Key Components:**

- P-Substrate: Base material (p-type silicon)
- N⁺ Source/Drain: Heavily doped n-type regions for current conduction
- Gate Oxide (SiO₂): Thin insulating layer for capacitive control
- Gate Electrode: Poly-Si or metal; controls inversion layer formation
- Isolation Oxide: Prevents electrical cross-talk between devices
- Body Terminal (B): Connected to substrate, usually tied to ground

**Operation Principle:**

Applying gate-to-source voltage Vgs > Vt forms a conductive channel between source and drain
Channel acts as voltage-controlled resistor in linear region
Current saturates when channel pinches off near drain

<div align="center" >
  <img src="./assets/NMOS.png" alt="NMOS" width="80%">
</div>

<div align="center" >
  <img src="./assets/Threshold_Voltage.png" alt="Threshold_Voltage" width="80%">
</div>

**Strong Inversion and Threshold Voltage** \
Strong inversion starts when $(V_{gs})$ exceeds threshold voltage $(V_t)$.  
$(V_t)$ is affected by:
- Substrate doping concentration
- Oxide thickness
- Source-body bias

<div align="center" >
  <img src="./assets/Body_gnd.png" alt="Body_gnd" width="80%">
</div>

**Threshold with Substrate Potential**
Increasing substrate potential raises $(V_t)$:
$$ V_t = V_{t0} + \gamma (\sqrt{|-2\phi_f + V_{sb}|} - \sqrt{|-2\phi_f|}) $$

<div align="center" >
  <img src="./assets/Vsb_applied.png" alt="Vsb_applied" width="80%">
</div>

<div align="center" >
  <img src="./assets/Threshold_Voltage_equation.png" alt="Threshold_Voltage_equation" width="80%">
</div>

---

#### NMOS Resistive and Saturation Region of Operation

**Resistive (Linear) Region**  
For small $(V_{ds})$:
$$ I_d = \mu_n C_{ox} \frac{W}{L}((V_{gs}-V_t)V_{ds} - \frac{V_{ds}^2}{2}) $$

<div align="center" >
  <img src="./assets/Resistive_Operation.png" alt="Resistive_Operation" width="80%">
</div>

<div align="center" >
  <img src="./assets/firstOrderAnalysis.png" alt="firstOrderAnalysis" width="80%">
</div>


**Drift Current Theory**  

<div align="center" >
  <img src="./assets/DriftCurrent.png" alt="DriftCurrent" width="80%">
</div>

<div align="center" >
  <img src="./assets/velocity.png" alt="velocity" width="80%">
</div>

<div align="center" >
  <img src="./assets/DrainCurrent_Eqn.png" alt="DrainCurrent_Eqn" width="80%">
</div>

**Linear Region Model**  

<div align="center" >
  <img src="./assets/ConditionforVds_in_linear_region.png" alt="ConditionforVds_in_linear_region" width="80%">
</div>


**Pinch-Off and Saturation**  

<div align="center" >
  <img src="./assets/PinchOffCondition.png" alt="PinchOffCondition" width="80%">
</div>

**Channel length Modulation**  

<div align="center" >
  <img src="./assets/ChannelLength_modulation.png" alt="ChannelLength_modulation" width="80%">
</div>


**Spice Set Up**  
Spice Model Parameters + Spice Netlist = Spice Software which generates the required waveforms for the analysis

<div align="center" >
  <img src="./assets/spice_software.png" alt="spice_software" width="80%">
</div>

### SPICE NETLIST
- Creating SPICE NETLIST \
A SPICE netlist provides a text description of a circuit.
<div align="center" >
  <img src="./assets/nmos_symbol.png" alt="nmos_symbol" width="80%">
</div>

<div align="center" >
  <img src="./assets/Netlist_syntax.png" alt="Netlist_syntax" width="80%">
</div>

**1. Element Declarations (Circuit Components)** \
The first letter of the element name defines its type. Node 0 represents ground.

| Type                | Syntax Format                                             | Example                                     | Description                                                                                                                                     |
| ------------------- | --------------------------------------------------------- | ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Resistor            | `Rname N+ N- Value`                                       | `R1 1 2 1k`                                 | Defines a resistor between Node 1 and Node 2 with a value of 1 kOhm.                                                                            |
| Capacitor           | `Cname N+ N- Value`                                       | `Cload 3 0 100pF`                           | Defines a capacitor between Node 3 and Ground with a value of 100 pF.                                                                           |
| Inductor            | `Lname N+ N- Value`                                       | `Lmatch 4 5 10nH`                           | Defines an inductor between Node 4 and Node 5 with a value of 10 nH.                                                                            |
| Voltage Source      | `Vname N+ N- [Type]`                                      | `Vdd 10 0 DC 3.3V`                          | Defines a DC voltage source (Vdd) of 3.3V between Node 10 and Ground.                                                                           |
| Current Source      | `Iname N+ N- [Type]`                                      | `Ib bias 0 DC 1mA`                          | Defines a DC current source (Ib) flowing from `bias` to Ground.                                                                                 |
| Diode               | `Dname N_anode N_cathode ModelName`                       | `D1 2 3 D_generic`                          | Defines a diode using a specific `.MODEL` called `D_generic`.                                                                                   |
| N-Channel MOS       | `Mname N_drain N_gate N_source N_bulk ModelName [params]` | `M1 out in 0 0 NMOS_mod W=10u L=0.5u`       | Defines an NMOS transistor (M1) with drain, gate, source, and bulk connected to specific nodes, using parameters like width (W) and length (L). |
| P-Channel MOS       | `Mname N_drain N_gate N_source N_bulk ModelName [params]` | `M2 vout vin vdd vdd PMOS_mod W=20u L=0.5u` | Defines a PMOS transistor (M2) using a PMOS model name. Note that bulk is typically tied to VDD.                                                |
| Subcircuit Instance | `Xname Node1 Node2 ... SubName`                           | `Xamp 1 2 3 0 OPAMP_VFB`                    | Instantiates a complex subcircuit block named `OPAMP_VFB` into the main schematic.                                                              |

**2. Control Statements (Directives)** \
These lines always start with a period (.) and control how the simulation runs.

| Directive          | Syntax Format                            | Description                                                                                                                          |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Transient Analysis | `.TRAN Tstep Tstop [Tstart [Tmax]]`      | Performs time-domain simulation for a specified duration (Tstop), saving data points every Tstep.                                    |
| AC Analysis        | `.AC [LIN/DEC/OCT] Npoints Fstart Fstop` | Performs frequency-domain simulation (e.g., for finding gain and phase margin) using linear, decade, or octave sweep types.          |
| DC Analysis        | `.DC Source Start Stop Step`             | Performs a DC sweep, varying an input source (voltage or current) through a range of values.                                         |
| Model Definition   | `.MODEL ModelName Type (params)`         | Defines specific electrical characteristics for diodes (`D`), NMOS (`NMOS`), PMOS (`PMOS`), etc., using foundry-provided parameters. |
| Subcircuit Def     | `.SUBCKT SubName Node1 Node2 ...`        | Starts the definition of a hierarchical block (like an operational amplifier).                                                       |
| End Subcircuit     | `.ENDS [SubName]`                        | Ends the subcircuit definition block.                                                                                                |
| End Netlist        | `.END`                                   | The final line in every SPICE netlist file, indicating the end of the description.                                                   |

**3. Syntax Modifiers and Units** \
SPICE accepts standard engineering multipliers for convenience.

| Unit/Modifier | Value                                  |
|---------------|----------------------------------------|
| `p`           | Pico $(10^{−12})$                      |
| `n`           | Nano $(10^{−9})$                       |
| `u`           | Micro $(10^{−6})$                      |
| `m`           | Milli $(10^{−3})$                      |
| `k`           | Kilo $(10^{3})$                        |
| `Meg` or `M`  | Mega $(10^{6})$ (Note:  `m`  is milli) |
| `G`           | Giga $(10^{9})$                        |

<div align="center" >
  <img src="./assets/Technology_File.png" alt="Technology_File" width="80%">
</div>

## Regions of Operation

| Region | Condition | Drain Current | Application |
|--------|-----------|---------------|-------------|
| **Cutoff** | Vgs < Vt | Id ≈ 0 | OFF state, logic '0' |
| **Linear** | Vgs > Vt, Vds < Vgs-Vt | Id ∝ Vds | Resistive switches, transmission gates |
| **Saturation** | Vgs > Vt, Vds ≥ Vgs-Vt | Id ∝ (Vgs-Vt)² | Amplifiers, current sources |


### Lab1

**Objective:** Observe linear and saturation regions for a long-channel NMOS device.

Steps 1. Clone the repository 
```bash 
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```
Steps 2. Open the file `day1_nfet_idvds_L2_W5.spice` in design folder

```bash 
vim ./design/day1_nfet_idvds_L2_W5.spice
```

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc

.end
```
Steps 3. Use ngspice for simulating `day1_nfet_idvds_L2_W5.spice` 
``` 
ngspice day1_nfet_idvds_L2_W5.spice
```
``` 
gnuplot plot -vdd#branch
```
<div align="center" >
  <img src="./assets/day1_nfet_gnuplot.png" alt="day1_nfet_gnuplot" width="80%">
</div>


**Observations:**
- **Linear Region**: Current increases linearly with Vds for low Vds
- **Saturation Region**: Current plateaus at higher Vds
- **Family of Curves**: Different Vgs values produce different saturation currents
- **Peak Current**: ≈ 410 μA at Vgs = 1.8V, Vds = 1.8V

## Analysis 

Linear Region and Saturation regions can be clearly observed.
<div align="center" >
  <img src="./assets/analysis_w1.8u_L1.2u.png" alt="analysis_w1.8u_L1.2u" width="80%">
</div>

### Effect of Velocity Saturation
Velocity saturation is a short-channel effect in MOS transistors where the velocity of charge carriers (electrons or holes) in the channel stops increasing linearly with the electric field and instead reaches a maximum, constant value, known as the saturation velocity
<div align="center" >
  <img src="./assets/ShortChannelMos.png" alt="ShortChannelMos" width="80%">
</div>

### Id Vgs Comparison with short channel MOS

<div align="center" >
  <img src="./assets/idvgs_Comparison.png" alt="idvgs_Comparison" width="80%">
</div>

Slope becomes linear on higher Vgs in Short Channel MOS due to Velocity Saturation Effects.

<div align="center" >
  <img src="./assets/velocity_sat_effect.png" alt="velocity_sat_effect" width="80%">
</div>

### Drain Current Expression including Velocity Saturation Effect

<div align="center" >
  <img src="./assets/derivingDrainCurrent_including_velocity_sat_effect.png" alt="derivingDrainCurrent_including_velocity_sat_effect" width="80%">
</div>

### Generalised Drain Current Expression for all operating modes

<div align="center" >
  <img src="./assets/operation_modes.png" alt="operation_modes" width="80%">
</div>

### Peak Current reduces due to Velocity Saturation Effect

<div align="center" >
  <img src="./assets/PeakCurrent_reduces_dueto_vel_sat_effect.png" alt="PeakCurrent_reduces_dueto_vel_sat_effect" width="80%">
</div>


## Lab2

**Objective:** Extract threshold voltage from transfer characteristics.

Steps 1. Clone the repository 
```bash 
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```
Steps 2. Open the file `day2_nfet_idvds_L015_W039.spice` in design folder

```bash 
vim ./design/day2_nfet_idvds_L015_W039.spice
```

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc

.end
```
Steps 3. Use ngspice for simulating `day2_nfet_idvds_L015_W039.spice` 
``` 
ngspice day2_nfet_idvds_L015_W039.spice
```
``` 
gnuplot plot -vdd#branch
```

<div align="center" >
  <img src="./assets/day2_nfet_idvds_gnuplot.png" alt="day2_nfet_idvds_gnuplot" width="80%">
</div>

Similarly we do it for `day2_nfet_idvgs_L015_W039.spice`

<div align="center" >
  <img src="./assets/day2_nfet_idvgs_gnuplot.png" alt="day2_nfet_idvgs_gnuplot" width="80%">
</div>

The results from above simulation show the effect of velocity saturation as discussed above. Forming of Linear slope and Peak Current Reduction when we compare it with `day1_nfet_idvds_L2_W5.spice`.


**Threshold Voltage Extraction:**
1. Plot Id vs Vgs on linear scale
2. Find maximum slope (gm,max) point
3. Extrapolate linear portion to Id = 0 axis
4. Intersection gives Vt ≈ 0.4-0.5V for SKY130 typical corner

**Observations:**
- **Subthreshold Region**: Exponential Id increase below Vt
- **Strong Inversion**: Quadratic Id dependence above Vt
- **Body Effect**: Vt increases with positive Vsb

**Comparative Analysis:**

| Parameter | Long Channel (W=5µm, L=2µm) | Short Channel (W=0.39µm, L=0.15µm) |
|-----------|----------------------------|-------------------------------------|
| W/L Ratio | 2.5 | 2.6 |
| Peak Id | ≈ 410 μA | ≈ 210 μA |
| Saturation Mode | Classical pinch-off | Velocity saturated |
| Vds,sat | ≈ Vgs - Vt | << Vgs - Vt |
| Transconductance | Higher | Degraded (~50%) |

**Key Observations:**
- Despite similar W/L ratio, short-channel device has **~50% lower current**
- Saturation occurs at **lower Vds**
- Curves show **less pronounced saturation** region
- **Velocity saturation** is the dominant current-limiting mechanism


### MOS Characteristics

<div align="center" >
  <img src="./assets/MOS_Characteristics.png" alt="MOS_Characteristics" width="80%">
</div>

### CMOS Inverter

A CMOS inverter uses complementary PMOS and NMOS transistors:

**Structure:**
- PMOS source → VDD, drain → Vout
- NMOS source → GND, drain → Vout
- Gates tied together → Vin
- Drains tied together → Vout

**Logic Operation:**

| Vin | PMOS | NMOS | Vout |
|-----|------|------|------|
| 0V (Low) | ON | OFF | VDD (High) |
| VDD (High) | OFF | ON | 0V (Low) |

<div align="center" >
  <img src="./assets/Cmos_inverter_ckt.png" alt="Cmos_inverter_ckt" width="80%">
</div>

**PMOS and NMOS Ids vs Vds Curves**

<div align="center" >
  <img src="./assets/pmos_nmos.png" alt="pmos_nmos" width="80%">
</div>

**Changing_vgsP_in_terms_of_Vin**

<div align="center" >
  <img src="./assets/changing_vgsP_in_terms_of_Vin.png" alt="changing_vgsP_in_terms_of_Vin" width="80%">
</div>

**Creating Load Curves**

<div align="center" >
  <img src="./assets/LoadCurve_Pmos.png" alt="LoadCurve_Pmos" width="80%">
</div>

<div align="center" >
  <img src="./assets/LoadCurve_Nmos.png" alt="LoadCurve_Nmos" width="80%">
</div>

### CMOS_VTC(VoltageTransferCharacteristics)

<div align="center" >
  <img src="./assets/CMOS_VTC(VoltageTransferCharacteristics).png" alt="VoltageTransferCharacteristics" width="80%">
</div>

### SPICE Deck

<div align="center" >
  <img src="./assets/Spice_deck.png" alt="Spice_deck" width="80%">
</div>

<div align="center" >
  <img src="./assets/spice_netlist_cmos_inverter.png" alt="spice_netlist_cmos_inverter" width="80%">
</div>

### Analysis

<div align="center" >
  <img src="./assets/spice_sim_analysis_diff_wL_pmos_in_cmos_inverter.png" alt="spice_sim_analysis_diff_wL_pmos_in_cmos_inverter" width="80%">
</div>


### Lab 3

Use ngspice for simulating `day3_inv_vtc_Wp084_Wn036.spice` 
``` 
ngspice day3_inv_vtc_Wp084_Wn036.spice
```
``` 
gnuplot plot -vdd#branch
```

<div align="center" >
  <img src="./assets/day3_cmos_vtc_gnuplot.png" alt="day3_inv_vtc_Wp084_Wn036.spice" width="80%">
</div>

**Switching Threshold Extraction:**

Method 1: Graphical
- Plot Vout vs Vin
- Plot line y = x
- Intersection gives Vm

Method 2: SPICE Measurement
```spice
.measure dc vm when out=in
```

**Expected Vm ≈ 0.89-0.90V** (close to VDD/2 = 0.9V)

Similarly we do it for `day3_inv_tran_Wp084_Wn036.spice`

<div align="center" >
  <img src="./assets/day3_cmos_tran_gnuplot.png" alt="day3_cmos_tran_gnuplot" width="80%">
</div>


## **Delay Measurements:**

## **Transient Delay Extraction — Detailed Calculation**

**Context:** values from ngspice output (screenshot):
- First rising-crossing point: `x0 = 2.15167e-09` s, `y0 = 0.902174`
- Second rising-crossing point: `x0 = 2.485e-09` s, `y0 = 0.901087`
- First falling-crossing candidate: `x0 = 4.05e-09` s, `y0 = 0.9`
- Second falling-crossing candidate: `x0 = 4.33488e-09` s, `y0 = 0.898913`


---

## **Definitions**
- **Rise propagation delay (tpLH)** ≈ time difference between the two specified rising crossing points:
tp_rise = t2_rise − t1_rise


- **Fall propagation delay (tpHL)** ≈ time difference between the two specified falling crossing points:
tp_fall = t2_fall − t1_fall



---

## **Numeric calculation (SI units)**

### Rise delay
t1_rise = 2.15167e-09 s
t2_rise = 2.48500e-09 s

tp_rise = t2_rise - t1_rise
= 2.48500e-09 - 2.15167e-09
= 3.3333000000000013e-10 s



Convert to convenient units:
tp_rise = 3.3333e-10 s = 333.33 ps = 0.33333 ns


### Fall delay
t1_fall = 4.05000e-09 s
t2_fall = 4.33488e-09 s

tp_fall = t2_fall - t1_fall
= 4.33488e-09 - 4.05000e-09
= 2.8488000000000036e-10 s



Convert to convenient units:
tp_fall = 2.8488e-10 s = 284.88 ps = 0.28488 ns



---

## **Summary (final values)**
- Rise propagation delay (tp_rise) = 3.3333e-10 s = 333.33 ps

- Fall propagation delay (tp_fall) = 2.8488e-10 s = 284.88 ps


### **Threshold Voltage Shift**

<div align="center" >
  <img src="./assets/Switching_Threshold.png" alt="Switching_Threshold" width="80%">
</div>

**Deriving Threshold Voltage**

<div align="center" >
  <img src="./assets/Deriving_Vm.png" alt="Deriving_Vm" width="80%">
</div>

**Varying_wL_pmos_for_setting_Vm**

<div align="center" >
  <img src="./assets/Varying_wL_pmos_for_setting_Vm.png" alt="Varying_wL_pmos_for_setting_Vm" width="80%">
</div>

**Variation_in_Rise,Fall_delay_for_diff_wL**

<div align="center" >
  <img src="./assets/Variation_in_Rise,Fall_delay_for_diff_wL.png" alt="Variation_in_Rise,Fall_delay_for_diff_wL" width="80%">
</div>

**Understanding Clock Tree Buffer**

<div align="center" >
  <img src="./assets/ClockBuffer.png" alt="ClockBuffer" width="80%">
</div>

**Input Output Characteristics**

<div align="center" >
  <img src="./assets/IO_char_with_finiteSlope.png" alt="IO_char_with_finiteSlope" width="80%">
</div>

<div align="center" >
  <img src="./assets/Actual_IO_Char_inverter.png" alt="Actual_IO_Char_inverter" width="80%">
</div>

## Noise Margin

<div align="center" >
  <img src="./assets/NMH_NML.png" alt="NMH_NML" width="80%">
</div>

<div align="center" >
  <img src="./assets/Noise_Margin_summary.png" alt="Noise_Margin_summary" width="80%">
</div>

<div align="center" >
  <img src="./assets/effectOf_wL_on_noiseMargin.png" alt="effectOf_wL_on_noiseMargin" width="80%">
</div>

### Lab 4


Use ngspice for simulating `day4_inv_noisemargin_wp1_wn036.spice` 
``` 
ngspice day4_inv_noisemargin_wp1_wn036.spice
```
``` 
gnuplot plot -vdd#branch
```

<div align="center" >
  <img src="./assets/day4_cmos_noiseM_gnuplot.png" alt="day4_cmos_noiseM_gnuplot" width="80%">
</div>

<div align="center" >
  <img src="./assets/NoiseMargin_calc.png" alt="NoiseMargin_calc" width="80%">
</div>

## Power Supply Variation

<div align="center" >
  <img src="./assets/PowerSupplyVariation.png" alt="PowerSupplyVariation" width="80%">
</div>

<div align="center" >
  <img src="./assets/spice_sim_powerSupply_scaling.png" alt="spice_sim_powerSupply_scaling" width="80%">
</div>

<div align="center" >
  <img src="./assets/advantage_disadvantages_powerSupplyScaling.png" alt="advantage_disadvantages_powerSupplyScaling" width="80%">
</div>

### Lab 5 Part 1

Use ngspice for simulating `day5_inv_supplyvariation_Wp1_Wn036.spice` 
``` 
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice
```
``` 
gnuplot plot -vdd#branch
```

<div align="center" >
  <img src="./assets/characteristics_for_diff_supply_voltages.png" alt="characteristics_for_diff_supply_voltages" width="80%">
</div>

## Variation due to Etching Process

<div align="center" >
  <img src="./assets/Variation_due_to_etching_process.png" alt="Variation_due_to_etching_process" width="80%">
</div>

## Variation due to Oxide Thickness

<div align="center" >
  <img src="./assets/Variation_due_to_oxide_thickness.png" alt="Variation_due_to_oxide_thickness" width="80%">
</div>

## Device Variations

<div align="center" >
  <img src="./assets/spice_sim_device_variations.png" alt="spice_sim_device_variations" width="80%">
</div>

<div align="center" >
  <img src="./assets/static_behavior_analysis_spice_sim.png" alt="static_behavior_analysis_spice_sim" width="80%">
</div>

### Lab 5 Part 2

Use ngspice for simulating `day5_inv_devicevariation_wp7_wn042.spice` 
``` 
ngspice day5_inv_devicevariation_wp7_wn042.spice
```
``` 
gnuplot plot -vdd#branch
```

<div align="center" >
  <img src="./assets/day5_cmos_device_variation_gnuplot.png" alt="day5_cmos_device_variation_gnuplot" width="80%">
</div>

## Acknowledgement 👑
I am thankful to [**Kunal Ghosh**](https://github.com/kunalg123) and Team **[VLSI System Design (VSD)](https://vsdiat.vlsisystemdesign.com/)** for the opportunity to participate in the ongoing **RISC-V SoC Tapeout Program**.  

I also acknowledge the support of **RISC-V International**, **India Semiconductor Mission (ISM)**, **VLSI Society of India (VSI)**, and [**Efabless**](https://github.com/efabless) for making this initiative possible.  

**Let’s build the future of electronics together!** If you have questions or want to discuss SoC/VLSI design, open an issue or start a discussion.

---
