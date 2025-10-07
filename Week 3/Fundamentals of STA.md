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

<img width="1025" height="396" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_10_56" src="https://github.com/user-attachments/assets/7661f69f-a423-48b2-beea-80326f8226ce" />


**in2reg:**

<img width="1015" height="387" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_11_04" src="https://github.com/user-attachments/assets/bade8a57-33d0-4f02-af15-d39c89a95af3" />

**reg2out:**

<img width="1024" height="394" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_11_13" src="https://github.com/user-attachments/assets/c38cc86c-03f5-4aa0-a079-9d18b895989a" />

**in2out:**

<img width="1011" height="391" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_11_19" src="https://github.com/user-attachments/assets/da1f6b67-e9c1-4e0f-963d-77a6a134b400" />


> IO timing includes *in2reg*, *reg2out*, and *in2out* paths.

### 4.2 Other Timing Analyses
1. **Clock Gating:** Ensures no glitches occur when enabling/disabling clock gates

<img width="1003" height="371" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_12_15" src="https://github.com/user-attachments/assets/d61b977b-a127-407e-9f51-c5797cd7c890" />



2. **Recovery/Removal:** For asynchronous reset/preset signals  
   - *Recovery time:* Reset inactive before clock edge  
   - *Removal time:* Reset active after clock edge
  
<img width="1028" height="368" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_12_59" src="https://github.com/user-attachments/assets/069fb324-b02b-4806-9391-66a6ecc5bc27" />


3. **Data-to-Data:** Combinational-only interactions (no flops)

<img width="1003" height="378" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_14_54" src="https://github.com/user-attachments/assets/4cf50c88-6d02-4c8f-9787-60e89de81a96" />


5. **Latch Timing (Time Borrowing):** Latches can “borrow” time between paths when transparent

<img width="1003" height="448" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_16_42" src="https://github.com/user-attachments/assets/73132660-2c7e-4c0f-9f54-c50203a22743" />


---

## 5. Slew / Transition Analysis

Measures how fast a signal transitions between logic levels.

- **Data Slew (max/min):** Transition rate of data signals  
- **Clock Slew (max/min):** Transition rate of clock signals  

> Excessive slew → slower logic, higher power, and degraded timing.

<img width="1010" height="462" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_19_30" src="https://github.com/user-attachments/assets/b6e27cd2-861e-4aae-b6d9-da27539f2c80" />


---

## 6. Load Analysis

Verifies that the driving cells meet electrical limits:

- **Fanout (max/min):** Number of loads driven

<img width="1015" height="459" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_19_38" src="https://github.com/user-attachments/assets/c9c2b04f-7b39-4c1d-b31d-af163f24fcb7" />

- **Capacitance (max/min):** Total net capacitance

<img width="1021" height="473" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_19_45" src="https://github.com/user-attachments/assets/4b435134-b2a4-4c7d-b3de-186ae1701e29" />


---

## 7. Clock Analysis

### 7.1 Clock Skew

```
Skew = Capture Clock Latency – Launch Clock Latency
```

<img width="1020" height="543" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_21_31" src="https://github.com/user-attachments/assets/b4df96b1-8034-49d7-840a-823341f4cb5a" />


### 7.2 Pulse Width
Duration of the clock’s high or low phase.  
Must meet minimum pulse width requirements for correct sequential operation.


<img width="1046" height="511" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 20_00_33" src="https://github.com/user-attachments/assets/b93aa79b-7f57-424e-aae1-ad9838480202" />


---

## 8. Timing Graph Model

STA tools convert the circuit into a **Directed Acyclic Graph (DAG)** — also called a **timing graph**.

- **Nodes →** Pins or nets  
- **Edges →** Timing arcs with delay values

  <img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_28_08" src="https://github.com/user-attachments/assets/6a0bec20-5463-449b-a0b7-7c99b3ed75a1" />

**Example: Convert the following circuit to a DAG

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_50_49" src="https://github.com/user-attachments/assets/70ff795d-0377-4143-a4a7-06270140bfad" />


### 8.1 Calculation Flow

- **AAT (Actual Arrival Time):**  
  Computed *forward* from sources (flop outputs / inputs)

  <img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_41_14" src="https://github.com/user-attachments/assets/08a50a91-6deb-4b8a-ba6e-ade7b76a3220" />

  

- **RAT (Required Arrival Time):**  
  Computed *backward* from sinks (flop inputs / outputs)

  <img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_41_20" src="https://github.com/user-attachments/assets/a3c9f60b-d1e1-48e5-bc43-93776dc66f80" />



**Slack:**  

```
Slack = RAT – AAT
```

*Note:* AAT is expected to be lesser than RAT at every node for meeting design expectations


**Example:**

AAT is indicated using yellow, RAT is indicated using Blue and Slack is indicated using Red


<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_45_31" src="https://github.com/user-attachments/assets/d9a0d8e9-bc60-476c-9c3a-66dd2b009d70" />


---

## 9. Analysis Methods

- **PBA (Path-Based Analysis):** Follows exact physical paths — accurate, slower  
- **GBA (Graph-Based Analysis):** Uses worst-case path approximations — faster, more pessimistic

<img width="1213" height="325" alt="image" src="https://github.com/user-attachments/assets/bd8f0fba-78ac-462f-92f5-e8b13c55b66e" />


---

## 10. Setup and Hold Verification

### 10.1 Setup Analysis
- Data must **arrive before** the capture clock edge  
- `Combinational Delay < Clock Period – Setup Time`

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_56_44" src="https://github.com/user-attachments/assets/5bbe1a17-06e5-4cd4-abcc-becc90d2ea3e" />

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 08_57_30" src="https://github.com/user-attachments/assets/f8cdd93f-cff9-4666-aae2-dff25d55da11" />

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_23_19" src="https://github.com/user-attachments/assets/1b7e5959-f74c-4d12-902d-3bef341c7c03" />

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_30_11" src="https://github.com/user-attachments/assets/48ab1082-5244-493a-94c1-f476ae514412" />


### 10.2 Hold Analysis
- Data must **remain stable after** the clock edge  
- `Combinational Delay > Hold Time`

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_34_48" src="https://github.com/user-attachments/assets/5e9de554-d9ab-4f8b-9a0a-400de3c4522f" />

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_35_30" src="https://github.com/user-attachments/assets/13878013-5ae8-40b4-94d1-b8672cf50fe6" />

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_38_34" src="https://github.com/user-attachments/assets/82149382-7a20-40cc-8625-4d43175fae34" />

---

## 11. Latches and Flip-Flops

| Element | Function |
|----------|-----------|
| **Positive Latch** | A positive latch passes input to output when the clock is HIGH |
| **Negative Latch** | A negative latch passes input to output when the clock is LOW |
| **Flip-Flop** | Combination of Master (Negative latch) and Slave (Positive latch) |

**Positive Latch:**

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_02_24" src="https://github.com/user-attachments/assets/e7731e25-de64-4ea8-adc0-cfe745f4a219" />


**Negative Latch:**

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_01_13" src="https://github.com/user-attachments/assets/359ee62d-f9f8-4866-9f4b-64489db9a2fd" />

**Flip Flop:**

<img width="421" height="411" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_01_24" src="https://github.com/user-attachments/assets/5ce159d9-59a4-4426-b4a3-4478db71e20a" />


**Ideal Clock:** Slew is zero, ie, the Rise/Fall transitions are equal to zero

**Positive-edge FF:** Implemented via master–slave configuration

- **Setup time:** Time before the rising clock edge during which D must be stable for correct Q output.

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_07_14" src="https://github.com/user-attachments/assets/d9a023da-99bf-4571-825b-a948f666fd66" />

- **CLK-Q delay:** Time taken for Qm to propagate to Q includes transmission gate and inverter delay.
  
<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_10_41" src="https://github.com/user-attachments/assets/11ef696d-1669-462a-87ad-bdbd758890cb" />


- **Hold time:** Duration D must remain valid after the clock edge; here, hold time is zero since D can change immediately after rising edge.


<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_11_19" src="https://github.com/user-attachments/assets/8ea70e4f-a477-41ae-b8f2-a8dc1dafadf6" />


## 12. Jitter and Uncertainty

- **Jitter:** Random variation of clock edge timing. It is measured using **eye diagrams**
  
- **Noise Margin:** Tolerance to signal distortion  
- **Uncertainty:** Additional time margin added to account for jitter/noise


<img width="1128" height="523" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_18_51" src="https://github.com/user-attachments/assets/0dfe00fc-c781-463e-82e1-14b94b7ca788" />

---

### 13. Power Supply Variations
- **Voltage Droop:** Temporary reduction in supply voltage due to switching current demand  
- **Ground Bounce:** Variation in ground potential caused by return current transients  
> Both affect delay and jitter and must be considered in STA margins.

---

## 14. On-Chip Variation (OCV)

Process variations can change delay characteristics across the chip.

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_54_42" src="https://github.com/user-attachments/assets/162af09b-e83f-4ee7-b187-3e73e134c42a" />

### 14.1 Sources of Variation
1. **Etching Process:** Variations in etch depth/width alter resistance & capacitance
2. **Oxide Thickness:** Changes threshold voltages → delay variations

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_54_20" src="https://github.com/user-attachments/assets/0ad4c7e0-3e1f-4725-95cd-fbbfea31ebdb" />


### 14.2 OCV Derates
To model variation:
- **Max Derate:** Used for setup (adds delay)  
- **Min Derate:** Used for hold (reduces delay)


---

## 15. Clock Effects

- **Clock Pull-In:** Reduced delay → Clock arrives earlier  
- **Clock Push-Out:** Increased delay → Clock arrives later

<img width="1366" height="768" alt="Course_ VSD - Static Timing Analysis - I _ Udemy - Brave 07-10-2025 09_56_47" src="https://github.com/user-attachments/assets/4679b782-581d-415e-907e-412c049a7aac" />


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

