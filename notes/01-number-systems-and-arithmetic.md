# CSE260 — Digital Logic Design
## My Notes: Number Systems, Base Conversion & Arithmetic

These are **my understanding notes**, not textbook notes.

The goal is not to collect definitions. The goal is that when I come back later, I can look at a small section and immediately remember **what confused me, what I was actually doing, and what finally made it click.**

---

# 1. Number systems — the basic idea

If I see:

**(101101)₂**

the 2 is the **base**.

It is NOT another digit.

The general idea is:

> **digit × base^(position)**

Positions start from the **right**, and the rightmost position is always 0.

Example:

~~~text
1    0    1    1
↑    ↑    ↑    ↑
2³   2²   2¹   2⁰
~~~

So:

1(2³) + 0(2²) + 1(2¹) + 1(2⁰) = 11₁₀

The important mental model:

- **base** tells me what gets raised to a power.
- **position** tells me the exponent.
- The exponent decreases as I move from left to right because the rightmost position is 0.

Decimal works exactly the same way.

---

# 2. Binary → decimal

Use place value.

Example:

**101110001001₂**

I assign positions from right to left, then multiply each bit by 2^position and add.

I practiced this and got:

**101110001001₂ = 2953₁₀**

---

# 3. Decimal → binary (whole number)

Here I use repeated division by the **base I'm converting TO**.

For decimal → binary, divide by 2.

Keep dividing the quotient until it becomes 0.

Then read the remainders **bottom → top**.

I used this for:

**4195₁₀ = 1000001100011₂**

### The thing I need to remember

> **Divide by the base I'm converting to.**
>
> Then read remainders from **bottom → top**.

---

# 4. Decimal fraction → binary

This is slightly different.

## Whole part

Same as before: repeated division by 2.

## Fractional part

Now repeatedly **multiply the fractional part by 2**.

Take the whole-number part produced each time as the next binary digit.

Example:

**0.625₁₀**

~~~text
0.625 × 2 = 1.25   → 1
0.25  × 2 = 0.50   → 0
0.50  × 2 = 1.00   → 1
~~~

So:

**0.625₁₀ = 0.101₂**

### Why do I stop at 0.50 × 2 = 1?

Because the remaining fractional part becomes **0**.

Once the remaining fraction is 0, every future fractional digit would just be 0.

So stop.

---

# 5. Non-terminating binary fractions

Sometimes the fractional part never reaches 0.

Example:

**4785.150263₁₀**

The whole part is:

**4785₁₀ = 1001010110001₂**

For the fraction:

~~~text
0.150263 × 2 = 0.300526 → 0
0.300526 × 2 = 0.601052 → 0
0.601052 × 2 = 1.202104 → 1
0.202104 × 2 = 0.404208 → 0
0.404208 × 2 = 0.808416 → 0
0.808416 × 2 = 1.616832 → 1
...
~~~

So I get approximately:

**1001010110001.001001...₂**

The CSE260 practice sheet specifically says that for an infinite fractional part, I should do **6–7 steps and use dots for the rest**. fileciteturn1file0L17-L21

---

# 6. Converting between arbitrary bases

There are two ideas here.

## Any base → decimal

Use place value.

## Decimal → another base

Use repeated division by the **target base**.

## Base A → Base B

If I don't have a direct shortcut, I can use:

~~~text
Base A → Decimal → Base B
~~~

This is the general bridge.

Important: this does NOT mean every conversion must always be done through decimal. It is just the general method when I don't have a direct shortcut.

---

# 7. Arithmetic in another base

This is where I initially had to slow down.

The arithmetic itself is familiar.

The difference is:

> **The carry/borrow is controlled by the base.**

For base 8, the valid digits are:

~~~text
0 1 2 3 4 5 6 7
~~~

There is no single base-8 digit called 8.

---

# 8. Addition in base 8 — the carry finally clicked

Example:

~~~text
  47₈
+ 36₈
-----
~~~

## Step 1 — align the positions

~~~text
  4 7
+ 3 6
-----
~~~

The rightmost digits are in the 8⁰ position.

So I calculate:

~~~text
7 + 6 = 13
~~~

Here 13 is the **actual quantity 13 in decimal**.

Now I need to express that quantity in base 8.

So I use the method I already know: divide by 8.

~~~text
13 ÷ 8 = 1 R 5
 1 ÷ 8 = 0 R 1
~~~

Read bottom → top:

~~~text
15₈
~~~

So:

**13₁₀ = 15₈**

This means:

- 5 stays in the current 8⁰ position.
- 1 is carried to the next position, 8¹.

Visual:

~~~text
  1
  4 7
+ 3 6
-----
    5
~~~

## Step 2 — the next column

Now the 8¹ column has:

- the original 4
- the original 3
- the carry 1

So:

~~~text
4 + 3 + 1 = 8
~~~

Again, 8 here is the actual quantity.

Convert:

~~~text
8 ÷ 8 = 1 R 0
1 ÷ 8 = 0 R 1
~~~

So:

**8₁₀ = 10₈**

This was the part I had to understand carefully.

The 10₈ is NOT going to sit entirely in the 8¹ position.

A single base-8 position can only contain one digit, and that digit can only be 0–7.

So:

~~~text
10₈
││
│└── 0 stays in the current position (8¹)
└─── 1 carries to the next position (8²)
~~~

There is nothing else in the 8² column, so the 1 simply stays there.

Final:

~~~text
8²   8¹   8⁰
 1    0    5
~~~

Therefore:

**47₈ + 36₈ = 105₈**

### The mental model that finally clicked

A position doesn't literally have a "capacity of 8".

Rather:

> **One base-8 position can hold only one digit, and the valid digits are 0–7.**

So when a calculation produces 8 or more, I represent that quantity using the next position.

---

# 9. Addition — what carry really means

If I am working in base 8:

~~~text
8⁰ → 8¹ → 8² → 8³
~~~

A carry always moves **one position to the left**.

So:

- carry from 8⁰ → 8¹
- carry from 8¹ → 8²
- carry from 8² → 8³

The carry is not some mysterious extra digit.

It represents a quantity that belongs to the **next place value**.

---

# 10. Subtraction in base 8

The normal-looking case is easy.

Example:

~~~text
  56₈
- 23₈
-----
~~~

Rightmost:

~~~text
6 - 3 = 3
~~~

Left:

~~~text
5 - 2 = 3
~~~

So:

**56₈ - 23₈ = 33₈**

No borrowing needed.

---

# 11. Borrowing in base 8 — the important part

Example:

~~~text
  52₈
- 27₈
-----
~~~

Start from the rightmost 8⁰ column:

~~~text
2 - 7
~~~

I can't do this because:

**2 < 7**

So I borrow 1 from the next column.

The next column is the 8¹ position.

And:

**1 × 8¹ = 8 × 8⁰**

So the borrowed 1 from the next position becomes **8 units in the current position**.

The 5 becomes 4.

The 2 effectively becomes:

~~~text
2 + 8 = 10
~~~

Then:

~~~text
10 - 7 = 3
~~~

No conversion is needed here.

The 10 is just the temporary quantity I am calculating with. I am NOT writing 10 as a base-8 digit in the answer.

Then the left column:

~~~text
4 - 2 = 2
~~~

Final:

~~~text
  52₈
- 27₈
-----
  23₈
~~~

---

# 12. The key difference: carry vs borrow

### Addition

If the column result reaches the base:

**base 8 → result reaches 8+**

I need to represent that result using the next position.

That creates a **carry**.

### Subtraction

If:

**top digit < bottom digit**

I **borrow the base** from the next position.

For base 8:

~~~text
borrow 1 from next position
→ receive 8 units in current position
~~~

That's it.

---

# 13. One thing I don't need to overthink

When I see something like:

~~~text
8 + 2 = 10
10 - 7 = 3
~~~

during borrowing, I don't need to stop and convert everything.

I'm simply working with the actual quantities.

The 7 is a valid base-8 digit and represents the quantity seven.

The temporary 10 is just the arithmetic result of 8 + 2.

I only need to worry about **how something is written in the base** when it becomes part of the written result.

---

# 14. My current mental model

If I forget everything else, remember this:

~~~text
BASE 8

valid digit:
0–7

ADDITION:
column result ≥ 8
→ keep the valid digit here
→ carry to the left

SUBTRACTION:
top < bottom
→ borrow from the left
→ borrowed 1 = 8 in the current column
~~~

And the place values are always:

~~~text
8²   8¹   8⁰
 ↑    ↑    ↑
left        right
~~~

---

## Where I am now

I understand:

- place value
- binary → decimal
- decimal → binary
- fractional decimal → binary
- arbitrary-base conversion
- addition in another base
- carrying
- subtraction in another base
- borrowing

The part that took the most time was **not the arithmetic itself**. It was understanding exactly what the carry/borrow represents and which place value it belongs to.

Once that clicked, the operations started looking much more normal.

---

## Next

**Multiplication in different bases.**

I should not move on until the carry/borrow mechanism feels natural, but I also don't need to keep re-explaining it once it does.
