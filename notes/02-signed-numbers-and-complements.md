# CSE260 — Lecture 2: Signed Numbers & Complements

These are my revision notes from how I actually learned this lecture. Keep the order: understand the idea first, then the bit procedure. If something feels fuzzy, return to the small mental model instead of memorising a paragraph.

> **Current stopping point:** I understand what overflow means, and I have seen one example. I have **not** finished learning the binary sign-rule for detecting overflow yet.

---

## 1. BCD and Excess-3

### BCD: split the decimal number into digits

BCD does **not** convert the whole number into ordinary binary. I convert each decimal digit separately, using 4 bits per digit.

Example: (629_{10})

- 6 → `0110`
- 2 → `0010`
- 9 → `1001`

So:

`629 in BCD = 0110 0010 1001`

Only `0000` through `1001` are valid BCD digit codes.

### Excess-3: split, add 3, convert

For every decimal digit separately:

1. Add 3.
2. Write the result in 4-bit binary.

For (629_{10}):

- 6 + 3 = 9 → `1001`
- 2 + 3 = 5 → `0101`
- 9 + 3 = 12 → `1100`

So:

`629 in Excess-3 = 1001 0101 1100`

**The short memory hook:**

- BCD = **split → convert**
- Excess-3 = **split → add 3 → convert**

I did this one myself, and both answers were correct.

---

## 2. Why do we need signed-number representations?

Unsigned binary uses every bit for the value. But computers also need negative numbers, so we need a convention for representing the sign.

The three systems in this part of the lecture are:

1. Sign & Magnitude
2. 1's Complement
3. 2's Complement

The same number can have different bit patterns depending on which system the question asks for. **Always identify the representation first.**

---

## 3. Sign & Magnitude

### My mental model

**One bit for the sign; all remaining bits for the magnitude.**

- Sign bit `0` → positive
- Sign bit `1` → negative

For 8-bit Sign & Magnitude, there is 1 sign bit and 7 magnitude bits.

I worked through 25:

`25 in binary = 11001`

Pad the magnitude to 7 bits:

`0011001`

Then add the sign bit:

```text
+25 = 0 0011001 = 00011001
-25 = 1 0011001 = 10011001
```

### What clicked for me

I do **not** convert (-25) as if it were a completely different magnitude. First I find the magnitude 25, pad it to 7 bits, and then choose the sign bit. To change +25 into −25 in this system, **only the first bit changes**.

### Range and zero

With 8-bit Sign & Magnitude, the range is (-127) to (+127).

There are two zero patterns:

- `00000000` = +0
- `10000000` = −0

That duplicate zero is a drawback of this representation.

---

## 4. 1's Complement

### The procedure

For a negative number:

1. Write the positive number in the required number of bits.
2. Flip **every** bit: `0 → 1`, `1 → 0`.

I did (-37) using 8 bits:

```text
+37 = 00100101
flip  every bit
      11011010
```

Therefore:

`−37 = 11011010` in 8-bit 1's Complement.

No extra +1 here.

### Zero

1's Complement has two zeros:

- `00000000` = +0
- `11111111` = −0

For 8 bits, its range is also (-127) to (+127).

**Memory hook:** 1's Complement = **flip**.

---

## 5. 2's Complement

### The procedure

I should think of this as 1's Complement with one extra step:

1. Write the positive number using the required number of bits.
2. Flip every bit.
3. Add 1.

I did (-25) using 8 bits:

```text
+25        00011001
flip       11100110
add 1      11100111
```

Therefore:

`−25 = 11100111` in 8-bit 2's Complement.

**Memory hook:**

- Sign & Magnitude → change the sign bit.
- 1's Complement → flip every bit.
- 2's Complement → flip every bit, then add 1.

### Why only one zero?

The useful takeaway for now is simple: **1's Complement has +0 and −0; 2's Complement has only one zero.** I don't need to reopen the whole derivation every time I revise this.

---

## 6. Range of 2's Complement — the part that was fuzzy

For (N) bits, the range is:

[
-2^{N-1} 	ext{ to } 2^{N-1}-1
]

For 8 bits:

[
-128 	ext{ to } +127
]

### Why is the negative end one step farther?

This confused me at first. The thing that made it click was looking at the starting point and counting **zero only once**.

There are (2^8 = 256) different 8-bit patterns.

Patterns starting with `0` represent:

`0` through `+127` → 128 patterns.

Patterns starting with `1` represent:

`−128` through `−1` → 128 patterns.

So the negative side gets (-128), but there is no (+128). Zero already occupies one of the non-negative patterns.

**Lock-in:**

`8-bit 2's Complement = −128 to +127`

Don't just memorise the formula. If the endpoints feel uneven, remember the two groups of 128 patterns and that zero exists only once.

---

## 7. Overflow — what I understand so far

My current understanding:

> **Overflow means the true mathematical answer does not fit in the range representable with the chosen number of bits.**

For 8-bit 2's Complement, the range is (-128) to (+127). So (+128) cannot be represented.

Example:

```text
  01111111   = +127
+ 00000001   =   +1
-----------
  10000000
```

The mathematical result is (127+1=128), which is outside the range. The 8-bit pattern `10000000` represents (-128), not (+128), so this addition has overflowed.

**Important: this is where I stopped.** I understand the meaning of overflow and have seen this example. I still need to learn the quick binary rule for detecting overflow from the operand/result signs, and then practise addition and subtraction.

---

## My current quick-revision card

```text
BCD
Split into decimal digits → convert each digit to 4 bits.

EXCESS-3
Split into digits → add 3 to each → convert to 4 bits.

SIGN & MAGNITUDE
First bit is sign; remaining bits are magnitude.
+25 = 00011001
-25 = 10011001

1's COMPLEMENT
Negative = positive bits, then flip all bits.
-37 = 11011010  (8 bits)
Two zeros.

2's COMPLEMENT
Negative = flip all bits, then add 1.
-25 = 11100111  (8 bits)
One zero.

8-BIT 2's COMPLEMENT RANGE
-128 to +127

OVERFLOW — current understanding
True answer outside the representable range.
Next: learn the sign-based detection rule and practise.
```

## What to study next

Continue from exactly here:

1. Learn the 2's-complement overflow detection rule.
2. Practise a couple of additions and explain *why* each does or doesn't overflow.
3. Learn 2's-complement subtraction.
4. Then cover 1's-complement addition/subtraction, including its carry handling.

Don't jump ahead to all the arithmetic at once. The point of these notes is to preserve the understanding I have built so far, including where I paused.
