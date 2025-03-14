# Comparator Module Documentation

## Overview

**Comparator Unit** that performs relational operations between two input values (`A` and `B`). It determines the following conditions:

- **Equality (`A == B`)**
- **Less Than (`A < B`)**
- **Less Than or Equal To (`A <= B`)**

The module utilizes an **Adder/Subtractor Unit** (`adder`) to compute these comparisons, leveraging subtraction to determine relative ordering.

---

## Module Parameters

```
module compare #(
    SIZE ~ 32 // Can be reduced to 8-bit if necessary
)
(
    input a[SIZE],
    input b[SIZE],
    input alufn[6],
    output cmp
)
```

SIZE - Defines the bit width of the compare unit. Default is 32

## Inputs

a - First operand for comparison
b - Second operand for comparison
alufn - Determines the comparison operation

## Outputs

cmp = Output result of the comparison (1 = true, 0 = false)

## Internal Signals

```
sig et        // Equality flag (A == B)
sig lt        // Less than flag (A < B)
sig lte       // Less than or equal flag (A <= B)
sig conditions[4] // Stores the possible comparison results
```

et - Stores the equality condition
lt - Stores the less than condition
lte - Stores the less than or equal to condition
conditions[4] - Array to hold various comparison results for selection

## Logic Implementation

1. Why is subtraction needed for comparison?
   if A - B = 0, then A = B
   A - B < 0, then A < B
   A - B <= 0, then A <= B

2. Equality Check

```
et = adder.z
```

Logic:
if A - B = 0, meaning A == B, thus, z = "1"

3. Less Than Check

```
lt = adder.n ^ adder.v
```

Logic:
if A & B are both positive and A - B is negative, then A < B
if A & B are both negative and A - B flips the sign, then A < B

4. Less than or Equal To Check

```
lte = adder.z | (adder.n ^ adder.v)
```

Logic:
Combines the logic of Equality and Less Than checks
If any of the conditions == "1", then lte = "1"

5. MUX Selection for Comparison Type

```
conditions[0] = 0         // Default: Always false
conditions[1] = et        // Equality Condition (A == B)
conditions[2] = lt        // Less Than Condition (A < B)
conditions[3] = lte       // Less Than or Equal Condition (A <= B)
```

Logic:
Stores possible comparison results for multiplexer selection

```
muxcompare.s0 = alufn[1]
muxcompare.s1 = alufn[2]
muxcompare.in = conditions
cmp = muxcompare.out
```

Logic:
Uses a 4 to 1 multiplexer to select the correct comparison result based on alufn signal
