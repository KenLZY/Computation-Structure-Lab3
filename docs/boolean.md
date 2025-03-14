# Boolean Module Documentation

## Overview

The **Boolean Unit** is responsible for performing **bitwise logical operations** between two input values (`A` and `B`). It utilizes a **4-to-1 multiplexer (`mux_4`)** to select the operation based on the **ALUFN control signal**.

This module supports **common bitwise operations**, such as:

- **AND** (`A & B`)
- **OR** (`A | B`)
- **XOR** (`A ^ B`)
- **"A"** (`A`)

---

## Module Parameters

```
module boolean #(
    SIZE ~ 32 : SIZE > 0
)(
    input a[SIZE],
    input b[SIZE],
    input alufn[6],
    output bool[SIZE]
)
```

SIZE - Defines the bit width of boolean unit. Default 32 but must be greater than 0

## Inputs

- a: First Operand
- b: Second Operand
- alufn: Control signal to determine which Boolean operation to execute

## Outputs

- bool: Output result of the Boolean Operation

## Logic Implementation

1. Multiplexer Setup

```
mux_4 mux_4_8[SIZE];
```

- An array of SIZE (32 by default) 4 to 1 multiplexers used to perform bitwise Boolean operations
- Each bit of the result is computed independently

2. Boolean Operation Selection

```
mux_4_8.in = SIZEx{{alufn[3:0]}};
mux_4_8.s0 = a;
mux_4_8.s1 = b;
bool = mux_4_8.out;
```

- alufn[3:0] determines the Boolean operation
- The multiplexer transforms into a "truth table"
- output is then stored in bool
