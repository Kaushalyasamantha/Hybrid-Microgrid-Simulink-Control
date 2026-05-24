# Closed-Loop Hybrid Microgrid Simulation with Single-Phase DQ Inverter Control

A comprehensive MATLAB/Simulink model of a multi-source hybrid DC/AC microgrid. This project integrates renewable energy generation, energetic energy storage, automated supervisory logic, and vector-controlled power inversion to power a standalone AC residential load.

## 📌 System Architecture Overview
![System Architecture](Images/system_architecture.png)

The system consists of three distinct generation/storage links tied across a shared 400V DC common bus:
1. **Solar PV String:** Equipped with an **Incremental Conductance MPPT** algorithm driving a high-frequency boost converter.
2. **Battery Energy Storage (BESS):** A Lithium-Ion battery pack regulated via a bidirectional half-bridge converter utilizing **cascade dual-loop control** (outer voltage tracking / inner current regulation).
3. **Emergency Backup Fuel Cell:** A Proton Exchange Membrane Fuel Cell (PEMFC) tied to automated rule-based supervisory Energy Management System (EMS) gates.
4. **AC Power Stage:** A single-phase 2-level H-Bridge inverter executing **$dq$ Synchronous Reference Frame control** via an **Orthogonal Signal Generator (OSG)** and a Discrete PLL.

---

## ⚡ Supervisory EMS Logic Matrix
The emergency backup fuel cell activates automatically based on multi-variable conditional constraints:
* **Condition True:** If $P_{pv} < 600\text{W}$ **AND** $\text{Battery SOC} < 30\%$, the EMS switches from idle to active mode, commanding a nominal 20A injection from the Fuel Cell to support the link and prevent battery deep discharge.

---

## 📊 Simulation Results & Waveforms
![Simulation Results](Images/Battery_Bank.png)

* **PV Subsystem Stability:** Demonstrates the high-frequency oscillation around the maximum power point ($V_{mpp} \approx 220\text{V}$), confirming accurate tracking performance.
* **DC Bus Regulation:** Showcases the battery buck-boost stage maintaining the central link at 400V while successfully absorbing the 100Hz single-phase back-propagating power ripple.
* **AC Output Waveform:** Displays the performance of the inner loop current PI dampening parameters ($P=0.4, I=40$), resulting in a clean 230V_rms (325.2V peak) 50Hz sine wave across the load.

---

## 🛠️ How to Run the Simulation
1. Clone this repository to your local directory.
2. Open MATLAB (version R2022a or later recommended) and initialize the Simscape Specialized Power Systems environment.
3. Open `Model/hybrid_microgrid_inverter_dq.slx`.
4. Ensure the `powergui` block is set to **Discrete** with a sample time of `1e-6` s.
5. Set the solver to `ode23tb (stiff/TR-BDF2)` with a maximum step size of `1e-5`.
6. Click **Run** and open the scopes to observe the transients.

## 🏷️ Keywords
`MATLAB` `Simulink` `Power Electronics` `Control Systems` `MPPT` `Inverter Design` `Microgrid` `Simscape`