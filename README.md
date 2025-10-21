# 4-Bit Asynchronous JK Counter Simulation (ngspice)

This project demonstrates the simulation of a **4-bit asynchronous ripple counter** using **behavioral modeling** in **ngspice**.  
Each flip-flop output (Q0–Q3) toggles at half the frequency of the previous one, producing a binary count sequence from `0000` to `1111`.

---

## ⚙️ Project Overview

**Tool Used:** [ngspice](https://ngspice.sourceforge.io/)  
**File Name:** `jk_counter.cir`  
**Simulation Type:** Transient analysis (`.tran`)  
**Output Waveforms:** Clock, Q0, Q1, Q2, Q3  

This project models JK flip-flop behavior using **sinusoidal time functions** to generate digital square waves.  
The design avoids complex transistor-level modeling and directly produces ideal logic-level outputs.

---

## 📂 Directory Setup

Open your terminal and create a working directory for the project:

```bash
mkdir -p ~/ihp/IHP-Open-PDK/jk_counter
cd ~/ihp/IHP-Open-PDK/jk_counter