# Adder/Subtractor Module Documentation

## Overview

**Adder/Subtractor unit** using a **Ripple Carry Adder (RCA)**. The module supports both **addition and subtraction**, depending on the control signal (`alufn_signal[0]`). Additionally, it computes **status flags** such as:

- **Zero (Z) flag** – Indicates if the result is zero.
- **Overflow (V) flag** – Detects signed overflow.
- **Negative (N) flag** – Indicates if the result is negative.

## Module Parameters

```
module adder #(
    SIZE ~ 32 : SIZE > 1
)(
    input a[SIZE],
    input b[SIZE],
    input alufn_signal[6],
    output out[SIZE],
    output z,
    output v,
    output n
)
```

SIZE - determines the bit-width of the adder. Default is 32 but must be greater than 1

## Inputs & Outputs

Inputs:
a - First operand
b - Second operand
alufn_signal - Control signal that determines whether operation is addition or subtraction

Outputs:
out - The result of the computation
z - Zero flag
v - Overflow flag
n - Negative flag

## Internal Signals

```
sig xb[SIZE]
sig temp_out[SIZE]
```

xb - Stores b modified by XOR gate logic depending on addition or subtraction
temp_out - Stores the output from the RCA

```
xb = b ^ ( SIZEx{alufn_signal[0]} )
```

if alufn_signal[0] == 1, then: xb = b
if alufn_signal[0] == 0, then: xb = ~b

## Carry input for Subtraction

```
rca_unit.cin = alufn_signal[0]
```

In subtraction mode, cin is set to "1" to complete two's complement of xb

## Signed overflow detection

```
v = (a[SIZE-1] & xb[SIZE-1] & ~temp_out[SIZE-1]) | (~a[SIZE-1] & ~xb[SIZE-1] & temp_out[SIZE-1])
```

Overflow (v) occurs when:

1. Adding two POSITIVE numbers results in a negative
2. Adding two NEGATIVE numbers results in a positive

## Negative flag

```
n = temp_out[SIZE-1]
```

Directly takes the most significant bit (MSB) of the output to determine negativity

## Zero Flag

```
z = ~|temp_out
```

- Uses a NOR reduction to check if temp_out contains all zeros
