---
layout: post
title: "how to quickly convert binary to decimal"
author: "alex"
categories: til
tags: [til]
# image: picton-blue.jpg
---

In decimal number systems each digit has a `10^n` value. In the number `123` the `3` is in the `10^0` place, which gives it a value of `3 * 10^0` = `3 * 1` = `3`. Or said another way, there are `3` `ones`.

Similarly, there are `2` `tens`, or `2 * 10^1` = `2 * 10` = `20`.

Binary is analogous, but it uses powers of `2` instead of powers of `10`. So reading from right to left the place values are `2^0`, `2^1`, `2^2`, …, `2^n`

——

So here’s a quick way to convert a string representation of a binary number into its decimal value:

```python
def convert_binary_to_decimal(input_binary_str: str) -> int:
    decimal = 0
    for digit in input_binary_str:
        # input is string so convert to int
        one_or_zero = int(digit)

        # neat trick here
        decimal = decimal * 2 + one_or_zero

    return decimal
```

The reason it works is that each iteration of the loop retroactively accounts for the “true” value of of the _previous_ binary digit. 

Because we’re moving from left to right, the first digit is the most significant, but we assume it’s in the `2^0` place (least significant) and only change that assumption if and when we process more digits.

For example, if the input were `1`

    decimal = decimal * 2 + one_or_zero
    decimal = 0 * 2 + 1

Since the current value of decimal is `0` and the digit being processed is `1`.

If the input were `11` then we’d move to the next digit, the second `1`. At the start of this iteration `decimal` is equal to `1` (from previous iteration)

    decimal = decimal * 2 + one_or_zero
    decimal = 1 * 2 + 1

Which equals `3`, which is the correct decimal value of binary value `11`.

Here’s the trick - multiplying `decimal` by `2` each additional iteration is like increasing the power of `2` that we multiplied the _previous_ binary digit by in the previous iteration. 

That is, 

$$
(x * 2^3) * 2 = x * 2^4
$$


In the last iteration, `one_or_zero` will be in the `2^0` place, which always evaluates to `1`, and so the last `one_or_zero` will always have a _decimal_ value of `1` for binary digit `1` or a _decimal_ value of `0` for binary digit `0`. So we can just add it.

—— 

The advantage of this method is that we can parse the input in left-to-right order. And we don’t need to find the place-values (`2^n`) of each digit in each unique input first.

Of course, you could use the built-in `int()` function instead,

    # second parameter indicates which base, 2 for binary/base-2
    decimal = int(binary_num, 2)

Also, note that I used a string representation of a binary sequence but most languages have binary types or some better way to represent them.
