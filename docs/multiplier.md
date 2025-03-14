# 32 Bit Multiplier V1

Below is the given schematic for a 4 bit multiplier. We will be implementing a 32 bit multiplier using the same logic.

![schematic](https://natalieagus.github.io/50002/assets/contentimage/lab3-fpga/2024-50002-MUL.drawio.png)

## Implementation

We create 32\*32(1024) grid of full adders to implement the 32 bit multiplier. To index this multiplier, we use the following formula:

```
index = i * 32 + j
```

where `i` and `j` are the row and column index of the multiplier respectively.

Next, we need to map the inputs a, b and cin to the correct signals. We can use the following formulas:

For the `a` input, we simply perform a bitwise AND operation with the jth bit of `a` and the i+1th bit of `b`:

```
fa.a[i * 32 + j] = a[j] & b[i + 1]
```

For the `b` input, we take the output of the full adder above the current full adder. However, if the current full adder is in the first row, we take the jth bit of `a` and the 0th bit of `b`:

```
fa.b[i * 32 + j] = (i > 0) ? fa.s[(i * 32 + j) - 32 + 1] : a[j+1] & b[0]
```

For the `cin` input, we take the output of the full adder to the left of the current full adder. However, if the current full adder is in the first column, we take 0 as the input:

```
fa.cin[i * 32 + j] = (j > 0) ? fa.cout[(i * 32 + j) - 1] : 0
```

Finally, we can calculate the output of the multiplier by taking the output of the first full adder in each row:

```
mul[0] = a[0] & b[0];
repeat (i, 31) {
  mul[i+1] = fa.s[(i * 32)];
}
```

## Future Improvements

This implementation is extremely inefficient as it requires 1024 full adders and performs redundant calculations.

We can improve this by using a more efficient algorithm such as Booth's algorithm or Wallace tree multiplier.
