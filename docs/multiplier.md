# 32 Bit Multiplier V1

Below is the given schematic for a 4 bit multiplier. We will be implementing a 32 bit multiplier using the same logic.

![schematic](https://natalieagus.github.io/50002/assets/contentimage/lab3-fpga/2024-50002-MUL.drawio.png)

## Implementation

We create 31 \* 31 (total of 496) grid of full adders in a staircase pattern as illustrated above to implement the 32 bit multiplier.

```
fa fa[496]
```

To index the full adders, we maintain two signals, a signal representing first fa in the current row, and the first fa in the previous row. We will use and maintain these signals to handle logic row by row.

```
sig current_row_fa_index[$clog2(496)]
sig previous_row_fa_index[$clog2(496)]
```

where `i` and `j` are the row and column index of the fa respectively.

Next, we need to map the inputs a, b and cin to the correct signals. We can use the following formulas:

For the `a` input, we simply perform a bitwise AND operation with the jth bit of `a` and the ith bit of `b`:

```
fa.a[current_row_fa_index+j] = a[j] & b[i]
```

For the `b` input, we take the output of the full adder above the current full adder.

```
fa.b[current_row_fa_index+j] = fa.s[previous_row_fa_index+1+j]
```

For the `cin` input, we take the output of the full adder to the left of the current full adder. However, if the current full adder is in the first column, we take 0 as the input:

```
if (j == 0){ // set the cin input of the leading FA of the ith row to be 0
		fa.cin[current_row_fa_index+j] = 0
}
else{
		fa.cin[current_row_fa_index+j] = fa.cout[current_row_fa_index+j-1]
}
```

Finally, we can calculate the output of the multiplier by taking the output of the first full adder in each row, and updating the current row and previous row indices for the next loop iteration:

```
previous_row_fa_index = current_row_fa_index
current_row_fa_index = current_row_fa_index + 32-i
mul[i] = fa.s[previous_row_fa_index]
```

## Future Improvements

This implementation is extremely inefficient as it requires 496 full adders and performs redundant calculations.

We can improve this by using a more efficient algorithm such as Booth's algorithm or Wallace tree multiplier.
