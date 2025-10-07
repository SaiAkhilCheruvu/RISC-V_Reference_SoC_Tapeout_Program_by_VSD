# Fundamentals Of Static Timing Analysis (STA)



## 🧠 Static Timing Analysis (STA)

Static Timing Analysis (**STA**) is a fundamental step in verifying the **timing performance of a digital circuit** without requiring simulation vectors.  
It checks whether all data paths meet setup and hold time requirements, ensuring correct functionality across all process, voltage, and temperature (PVT) corners.

---

## 📑 Table of Contents
1. [Timing Path](#1-timing-path)
2. [Arrival Time and Slack](#2-arrival-time-and-slack)
3. [Expected Time](#3-expected-time)
4. [Types of Timing Analysis](#4-types-of-timing-analysis)
5. [Slew / Transition Analysis](#5-slew--transition-analysis)
6. [Load Analysis](#6-load-analysis)
7. [Clock Analysis](#7-clock-analysis)
8. [Timing Graph Model](#8-timing-graph-model)
9. [Analysis Methods](#9-analysis-methods)
10. [Setup and Hold Verification](#10-setup-and-hold-verification)
11. [Latches and Flip-Flops](#11-latches-and-flip-flops)
12. [Jitter and Uncertainty](#12-jitter-and-uncertainty)
13. [On-Chip Variation (OCV)](#13-on-chip-variation-ocv)
14. [Clock Effects](#14-clock-effects)
15. [Summary](#15-summary)

---

## 1. Timing Path

A **timing path** is the logical route that a signal follows from a **start point** to an **end point**.

### ✅ Valid Timing Path
- **Start Points:** Flip-flop clock pins or input ports  
- **End Points:** Flip-flop data (D) pins or output ports

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 07_57_33" src="https://github.com/user-attachments/assets/f3e46f44-a7c7-4b6c-aa15-03ba5b3ff3bf" />

---

## 2. Arrival Time and Slack

- **Arrival Time (AAT):** The actual time taken by a signal to reach an endpoint  
- **Required Arrival Time (RAT):** The expected or allowable latest time by which the signal should arrive

  <img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 07_59_35" src="https://github.com/user-attachments/assets/5469feba-a377-4419-b6a5-92423b161b24" />


### 🧩 Slack Formula

```
Slack = RAT – AAT
```


- **Positive Slack →** Timing met  
- **Negative Slack →** Timing violation

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_00_44" src="https://github.com/user-attachments/assets/d362e2f8-0682-449f-a5b6-93a868297eed" />


---

## 3. Expected Time

The **expected time** (RAT) represents the target arrival time constraint at an endpoint.

- **Setup (Max delay):** Latest time by which data must arrive before the next active clock edge  
- **Hold (Min delay):** Earliest time after the clock edge that data must remain stable

### 💡 Example

```
Minimum expected time = 0.3 ns
Arrival time = 0.2 ns
Maximum expected time = 1 ns
```


**Results:**
- Setup slack = 0.2 – 1.0 = –0.8 ns ❌  
- Hold slack  = 0.2 – 0.3 = –0.1 ns ❌

---

## 4. Types of Timing Analysis

### 4.1 Setup & Hold Path Categories

| Type | Description |
|------|--------------|
| **reg2reg** | Between two flip-flops (launch → capture) |
| **in2reg** | Input port → Flip-flop D pin |
| **reg2out** | Flip-flop output → Output port |
| **in2out** | Input port → Output port |

**reg2reg:**



> IO timing includes *in2reg*, *reg2out*, and *in2out* paths.

### 4.2 Other Timing Analyses
1. **Clock Gating:** Ensures no glitches occur when enabling/disabling clock gates  
2. **Recovery/Removal:** For asynchronous reset/preset signals  
   - *Recovery time:* Reset inactive before clock edge  
   - *Removal time:* Reset active after clock edge  
3. **Data-to-Data:** Combinational-only interactions (no flops)  
4. **Latch Timing (Time Borrowing):** Latches can “borrow” time between paths when transparent

---

## 5. Slew / Transition Analysis

Measures how fast a signal transitions between logic levels.

- **Data Slew (max/min):** Transition rate of data signals  
- **Clock Slew (max/min):** Transition rate of clock signals  

> Excessive slew → slower logic, higher power, and degraded timing.

---

## 6. Load Analysis

Verifies that the driving cells meet electrical limits:

- **Fanout (max/min):** Number of loads driven  
- **Capacitance (max/min):** Total net capacitance

---

## 7. Clock Analysis

### 7.1 Clock Skew

```
Skew = Capture Clock Latency – Launch Clock Latency
```


### 7.2 Pulse Width
Duration of the clock’s high or low phase.  
Must meet minimum pulse width requirements for correct sequential operation.

---

## 8. Timing Graph Model

STA tools convert the circuit into a **Directed Acyclic Graph (DAG)** — also called a **timing graph**.

- **Nodes →** Pins or nets  
- **Edges →** Timing arcs with delay values

### 8.1 Calculation Flow

- **AAT (Actual Arrival Time):**  
  Computed *forward* from sources (flop outputs / inputs)
- **RAT (Required Arrival Time):**  
  Computed *backward* from sinks (flop inputs / outputs)

> Multiple paths →  
> - AAT = **max (latest)** arrival  
> - RAT = **min (earliest)** required  

**Slack:**  

```
Slack = RAT – AAT
```


---

## 9. Analysis Methods

- **PBA (Path-Based Analysis):** Follows exact physical paths — accurate, slower  
- **GBA (Graph-Based Analysis):** Uses worst-case path approximations — faster, more pessimistic

---

## 10. Setup and Hold Verification

### 10.1 Setup Analysis
- Data must **arrive before** the capture clock edge  
- `Combinational Delay < Clock Period – Setup Time`

### 10.2 Hold Analysis
- Data must **remain stable after** the clock edge  
- `Combinational Delay > Hold Time`

---

## 11. Latches and Flip-Flops

| Element | Function |
|----------|-----------|
| **Positive Latch** | Transparent when clock = HIGH |
| **Negative Latch** | Transparent when clock = LOW |
| **Flip-Flop** | Master (neg latch) + Slave (pos latch) |

- **Ideal Clock:** Rise/Fall transitions = 0  
- **Positive-edge FF:** Implemented via master–slave configuration


## 12. Jitter and Uncertainty

- **Jitter:** Random variation of clock edge timing  
  → measured using **eye diagrams**  
- **Noise Margin:** Tolerance to signal distortion  
- **Uncertainty:** Additional time margin added to account for jitter/noise

---

### 13. Power Supply Variations
- **Voltage Droop:** Temporary reduction in supply voltage due to switching current demand  
- **Ground Bounce:** Variation in ground potential caused by return current transients  
> Both affect delay and jitter and must be considered in STA margins.

---

## 14. On-Chip Variation (OCV)

Process variations can change delay characteristics across the chip.

### 14.1 Sources of Variation
1. **Etching Process:** Variations in etch depth/width alter resistance & capacitance  
2. **Oxide Thickness:** Changes threshold voltages → delay variations

### 14.2 OCV Derates
To model variation:
- **Max Derate:** Used for setup (adds delay)  
- **Min Derate:** Used for hold (reduces delay)

---

## 15. Clock Effects

- **Clock Pull-In:** Reduced delay → Clock arrives earlier  
- **Clock Push-Out:** Increased delay → Clock arrives later

### 14.1 Additional Pessimism
STA assumes worst-case scenarios (max skew, derates, jitter).  
**Pessimism removal** reduces over-conservative margins for realistic silicon correlation.

---

## 16. Summary

Static Timing Analysis ensures that a design **meets setup and hold constraints** across all PVT corners by evaluating:

- Arrival and Required times  
- Slack computation  
- Clock skew and jitter  
- OCV and load effects  

STA builds a **timing graph**, computes **AAT** and **RAT**, and verifies **Slack ≥ 0** for every node — guaranteeing timing closure before tape-out.

---

