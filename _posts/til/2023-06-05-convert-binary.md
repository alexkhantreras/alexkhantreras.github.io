---
layout: post
title: "bin to dec"
author: "alex"
categories: til
tags: [til]
# image: anti-flash-white.jpg
---

### A quick snippet to convert binary numbers to their decimal values

<br/> <br/>

In in the decimal number system each digit has a `10^n` value. In the number `123` the `3` is in the `10^0` place, which gives it a value of `3 * 10^0` = `3 * 1` = `3`. Or said another way, there are `3` `ones`.

Similarly, there are `2` `tens`, or `2 * 10^1` = `2 * 10` = `20`. And `1` `one-hundreds`, or `1 * 10^2` = `1 * 100` = `100`. The sum of these is of course `123`

Binary is analogous, but it uses powers of `2` instead. So reading from right to left the place values of each digit are `2^0`, `2^1`, `2^2`, …, `2^n`

<br/> <br/>

So here’s a quick way to convert a string representation of a binary number into its decimal value:

```python
def convert_binary_to_decimal(binary_str: str) -> int:
    decimal = 0
    for char_digit in binary_str:
        one_or_zero = int(char_digit)
        # neat trick on below line
        decimal = decimal * 2 + one_or_zero

    return decimal
```

The reason it works is that each iteration of the loop retroactively accounts for the “true” value of of the _previously_ processed binary digit. 

Because we’re parsing from left to right, the first digit is the most significant. But we assume it’s in the `2^0` place (least significant) and only change that assumption if and when we process additional digits.

For example, if the input were `1`

```python
decimal = decimal * 2 + one_or_zero
decimal = 0 * 2 + 1
```

Since the current value of decimal is `0` and the digit being processed is `1`.

If the input were `11` then we’d move to the next digit, the second `1`. At the start of this iteration `decimal` is equal to `1` (from previous iteration)

```python
decimal = decimal * 2 + one_or_zero
decimal = 1 * 2 + 1
```

Which equals `3`, which is the correct decimal value of input binary value `11`.

Here’s the trick - multiplying `decimal` by `2` each additional iteration is like increasing the power of `2` that we multiplied the _previous_ binary digit by in the previous iteration. 

That is,

$$ (x * 2^3) * 2 = x * 2^4 $$

<br/> <br/>

In the last iteration, `one_or_zero` will be in the `2^0` place, which always evaluates to `1`. So the last `one_or_zero` will always have a _decimal_ value of `1` for binary digit `1` or a _decimal_ value of `0` for binary digit `0`. Meaning that we can just add it to `decimal`.

<br/> <br/>

The advantages of this method is that we can parse the input in left-to-right order without needing to first find the place-values (`2^n`) of each digit in each unique input.

<br/> <br/>

Of course, you could use the built-in `int()` function instead,

```python
# second parameter indicates which base, 2 for binary/base-2
decimal = int(binary_num, 2)
```

Also, note that I used a string representation of a binary sequence but most languages have binary types or some better way to represent them.

<br/> <br/>
