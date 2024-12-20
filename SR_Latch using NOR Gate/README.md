# CMOS SR Latch Using NOR Gate in LTspice  

## Overview  
This project implements and simulates a **CMOS SR Latch** using a hierarchical **NOR gate** design in LTspice. The SR latch is a fundamental bistable circuit used as a memory element in sequential logic. It has two stable states and operates based on the inputs **Set (S)** and **Reset (R)**.  

This design uses CMOS technology to achieve high efficiency, low power consumption, and scalability, making it ideal for VLSI applications.  

## Repository Structure  

- **`SR_latch.asc`**: LTspice schematic file for the CMOS SR Latch.  
- **`SR_latch_ckt.png`**: Circuit diagram of the CMOS SR Latch.  
- **`SR_latch_op.png`**: Simulation output waveform demonstrating the latch's transient response.  
- **`NOR_Gate.subckt`**: Subcircuit file for the CMOS NOR gate.  
- **`SR_Latch.txt`**: The netlist of the SR Latch circuit generated by LTspice.
  
## VLSI Design Details  

### SR Latch Functionality  
The SR latch is composed of two cross-coupled NOR gates.  
- **Set (S)**: Drives the latch to the **Set** state (`Q = HIGH`, `Q̅ = LOW`).  
- **Reset (R)**: Drives the latch to the **Reset** state (`Q = LOW`, `Q̅ = HIGH`).  
- **Hold (H)**: Maintains the previous state when both inputs are LOW.  
- **Invalid (I)**: When both inputs are HIGH, the state is undefined, which is generally avoided in proper usage.  

### Truth Table  
The truth table for the SR Latch is as follows:  

| **Set (S)** | **Reset (R)** | **Q (Output)** | **Q̅ (Complement)** | Description          |  
|-------------|---------------|----------------|---------------------|----------------------|  
| 0           | 0             | Hold Previous  | Hold Previous       | Hold state           |  
| 0           | 1             | 0              | 1                   | Reset state          |  
| 1           | 0             | 1              | 0                   | Set state            |  
| 1           | 1             | Undefined      | Undefined           | Invalid state        |  

### 1. **NOR Gate Subcircuit (NOR_gate.asy)**

The NOR gate is the building block of the SR latch. Here is the subcircuit definition for a CMOS NOR gate:

```plaintext
* CMOS NOR gate
.subckt NOR_Gate A B Out Vdd Vss
M1 N1 A Vdd Vdd PMOS L=2u W=10u
M2 N2 B Vdd Vdd PMOS L=2u W=10u
M3 Out N1 Vss Vss NMOS L=2u W=10u
M4 Out N2 Vss Vss NMOS L=2u W=10u
.ends NOR_gate
```
### 2. **SR Latch Schematic**

The SR Latch consists of two instances of the NOR gate. These gates are cross-coupled to form the memory element:

```plaintext
* CMOS SR Latch using NOR Gates
.subckt SR_Latch S R Q Qbar Vdd Vss
* Set Input (S) connected to the first NOR gate
X1 S R Q N1 NOR_Gate Vdd Vss
* Reset Input (R) connected to the second NOR gate
X2 R N1 Qbar Qbar NOR_Gate Vdd Vss
.ends SR_Latch
```

