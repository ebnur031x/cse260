The range formula is actually simple. Let's use your `3 + (-5)` example.

## 1. The formula for 2's complement

For `n` bits:

`Range = -2^(n-1) to 2^(n-1)-1`

Here, `n` means the total number of bits.

## 2. Example: 4-bit 2's complement

You used 4 bits:

```text
0011  (+3)
1011  (-5)
```

So, `n = 4`.

Step 1: Calculate the minimum.

`-2^(4-1) = -2^3 = -8`

Step 2: Calculate the maximum.

`2^(4-1)-1 = 2^3-1 = 7`

**4-bit 2's complement range**

# −8 to +7

Minimum: −8

Maximum: +7

−8, −7, −6, …, −1, 0, 1, …, 6, 7

## 3. Now check your answer

You calculated:

```text
  0011
+ 1011
------
  1110
```

The mathematical answer is:

`3 + (-5) = -2`

Check whether `-2` fits in the range:

`-8 <= -2 <= 7`

Yes! So there is no overflow.

## 4. One more example: overflow

Suppose you calculate `5 + 4 = 9` using 4-bit 2's complement.

The range is still `-8` to `+7`.

Since `9 > 7`, overflow occurs.

### Remember these two formulas

| Bits | 2's complement range |
|---:|:---|
| 4 | −8 to +7 |
| 5 | −16 to +15 |
| 6 | −32 to +31 |
| 8 | −128 to +127 |

The rule: Find the range using the number of bits, calculate the true mathematical answer, and check whether that answer falls inside the range.
