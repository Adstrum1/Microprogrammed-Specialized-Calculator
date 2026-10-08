# 8-bit Microprogrammed Specialized Calculator

A custom 8-bit specialized computing device designed and implemented as a university project in computer electronics.

The project demonstrates the design of a dedicated hardware calculator based on a **microprogrammed Mealy finite-state machine**, parallel data transfer, TTL-compatible logic, registers, counters, arithmetic units, and EPROM-based control logic.

Unlike a general-purpose processor, the device is designed to perform a **specific arithmetic algorithm** rather than execute an arbitrary set of instructions.

## Project Overview

The calculator processes **8-bit signed values represented in two's complement format**.

The control unit is implemented as a microprogrammed Mealy automaton. The microprogram is stored in EPROM, while registers and counters are used for temporary data storage and intermediate results during computation.

The device communicates with external circuitry through an **8-bit bidirectional parallel data bus** with additional synchronization signals.

### Main Characteristics

* 8-bit data processing
* Two's complement signed integer representation
* Microprogrammed Mealy finite-state machine
* EPROM-based microprogram storage
* Register-based state and data storage
* 8-bit bidirectional data bus
* TTL-compatible signal levels
* External power supply
* External clock source
* Local reset circuit
* Dedicated arithmetic algorithm
* Hardware implementation using 74xx-series TTL logic

## Mathematical Function

The specialized calculator implements the following arithmetic expression:

$$
Y_i = - \frac{5X_i}{8} + \frac{2X_{i+1}}{4}
$$

where:

* $X_i$ — current input value
* $X_{i+1}$ — next input value
* $Y_i$ — calculated result

All values are represented as 8-bit signed integers in two's complement format.

## System Architecture

The calculator consists of several functional parts:

* Control unit
* Data registers
* Arithmetic units
* Counters
* Bidirectional bus interface
* EPROM containing the microprogram
* Synchronization and reset circuits

The control unit coordinates the data path and determines which operation is performed at each stage of the calculation.

The detailed architecture is documented in the structural and functional electrical schematics included in the repository.

## Microprogrammed Control

The control system is implemented as a **Mealy-type microprogrammed finite-state machine**.

The current state is stored in hardware registers. The EPROM contains the microprogram that defines control signals and state transitions.

The control logic reacts to both the current state and input conditions.

The general sequence of operation is:

1. Wait for the `START` signal.
2. Load input data.
3. Perform the first arithmetic stage.
4. Divide the intermediate result by 8.
5. Read the next input value.
6. Store the intermediate result.
7. Perform the second arithmetic stage.
8. Divide the intermediate result.
9. Store the calculated result.
10. Output the result.
11. Return to the required state and continue processing.

## Microprogram States

The original microprogram contains the following states:

| State | Operation                | Transition                     |
| ----- | ------------------------ | ------------------------------ |
| A0    | Wait for start           | `START = 1` → A1, otherwise A0 |
| A1    | Load input number        | `X < 9` → A1, otherwise A2     |
| A2    | Add number 5 times       | → A3                           |
| A3    | Divide by 8              | → A4                           |
| A4    | Read number              | `X < 9` → A2, otherwise A5     |
| A5    | Store intermediate value | `X < 9` → A5, otherwise A6     |
| A6    | Add number 2 times       | → A7                           |
| A7    | Divide by 8              | → A8                           |
| A8    | Store intermediate value | `X < 9` → A8, otherwise A9     |
| A9    | Output result and store  | → A10                          |
| A10   | Program loop             | `X < 9` → A9, otherwise A0     |

The state sequence provides the control required to implement the specified arithmetic algorithm.

## Data Representation

The calculator uses an **8-bit two's complement representation** for signed data.

The representable range is:

$$
-128 \leq X \leq 127
$$

Two's complement representation allows the same arithmetic hardware to process both positive and negative values.

## Data Bus

Communication with external circuitry is performed through an **8-bit bidirectional parallel data bus**.

The interface includes additional synchronization and control signals required for reliable data exchange.

The signal levels are compatible with **TTL logic**.

The bidirectional bus interface is implemented using 74LS245 bus transceivers.

## Memory and Data Storage

The project uses two different types of storage functions.

### Microprogram Storage

Two **2732 EPROM** devices are used for storing the microprogram and control information.

The EPROMs provide the control information required by the microprogrammed automaton during operation.

### Temporary Data Storage

Registers and counters are used to store:

* Input values
* Intermediate arithmetic results
* Current state information
* Counters and control values

Therefore, the EPROM is used for **program/control storage**, while the registers and counters provide the working storage required during calculations.

## Hardware

The calculator is implemented using standard 74xx-series TTL logic and memory components.

### Main Components

| Component  | Quantity | Purpose                         |
| ---------- | -------: | ------------------------------- |
| 74LS193    |        2 | Binary counters                 |
| 2732 EPROM |        2 | Microprogram storage            |
| 74198      |        1 | Universal shift register        |
| 74LS163    |        2 | Synchronous binary counters     |
| 74273      |        2 | Octal registers                 |
| 74LS245    |        2 | Bidirectional bus transceivers  |
| 74LS83     |        2 | 4-bit binary adders             |
| 74LS74     |        1 | Dual D-type flip-flop           |
| XOR gates  |       10 | Logic and arithmetic operations |
| OR gate    |        1 | Control logic                   |

### Passive Components

| Component                          | Quantity |
| ---------------------------------- | -------: |
| SMD 4.7 µF, 25 V, ±10% capacitor   |        1 |
| SMD 0.1 µF, 100 V, ±10% capacitors |        7 |

## Arithmetic Data Path

The arithmetic part of the calculator uses dedicated digital logic rather than software instructions.

The required operations are implemented using:

* 74LS83 binary adders
* XOR logic
* Registers
* Counters
* Shift-register logic
* Control signals generated by the microprogrammed control unit

This allows the device to perform the required calculation directly at the hardware level.

## Calculation Process

The calculation is divided into several hardware-controlled stages.

### First Stage

The current input value $X_i$ is processed by repeated addition:

$$
X_i + X_i + X_i + X_i + X_i = 5X_i
$$

The resulting value is then divided by 8:

$$
\frac{5X_i}{8}
$$

### Second Stage

The next input value $X_{i+1}$ is processed according to the second part of the algorithm:

$$
X_{i+1} + X_{i+1} = 2X_{i+1}
$$

The intermediate result is then divided according to the implemented algorithm.

### Final Result

The partial results are combined to obtain:

$$
Y_i = - \frac{5X_i}{8} + \frac{2X_{i+1}}{4}
$$

The resulting value is stored and subsequently transferred to the output interface.

## Input and Output

The calculator uses an external parallel interface for data exchange.

### Input

Input data is transferred through the 8-bit bidirectional bus.

The input process is controlled by synchronization signals and the microprogrammed control unit.

### Output

After the calculation is completed, the resulting 8-bit value is placed on the data bus.

The output timing is controlled by the appropriate microprogram state and control signals.

## What This Project Demonstrates

This project demonstrates practical aspects of digital computer architecture and hardware design, including:

* Specialized computer architecture
* Microprogrammed control
* Mealy finite-state machines
* EPROM-based control logic
* Register-transfer operations
* Digital arithmetic
* Two's complement representation
* Parallel data buses
* TTL logic
* Hardware counters
* Hardware registers
* Timing and synchronization
* Implementation of an algorithm using discrete digital logic

## Project Context

This project was originally developed as a university course project in **Computer Electronics**.

Although it can be described as a calculator, it is more accurately a **specialized hardware computing device**. Instead of using a general-purpose microcontroller or processor, the required algorithm is implemented directly using digital logic, registers, counters, adders, and a microprogrammed control unit.

The project therefore demonstrates the principles behind simple dedicated processors and domain-specific computing hardware.

## Limitations

The device is intentionally specialized.

It does not provide:

* General-purpose arithmetic operations
* A programmable instruction set
* General-purpose software execution
* Floating-point arithmetic
* Arbitrary mathematical expressions

The hardware is optimized specifically for the predefined calculation described above.

## Project Status

**Completed — University Course Project**

The repository contains the original design and documentation created during the course project.

The project is preserved as a hardware-oriented portfolio project demonstrating digital system design, microprogrammed control, and implementation of an arithmetic algorithm using discrete TTL logic.

## License

This project is provided for educational and portfolio purposes.
