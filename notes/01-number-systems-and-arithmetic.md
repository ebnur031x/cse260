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
       1    ← this is 8¹
       4    7
     + 3    6
     --------
            5    ← this is 8⁰
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

**One position can contain exactly one digit**
One position = one digit. (LOCK IN)
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

---

# 15. Multiplication and division — the distinction I need to keep clear

This is where I had to slow down.

There are **two main paths** when doing arithmetic in another base.

### A. Direct

Stay inside the original base.

~~~text
base-r number
     ↓
arithmetic directly in base-r
     ↓
base-r answer
~~~

There is no middleman.

### B. Conversion / middleman

Use decimal as the bridge.

~~~text
base-r
  ↓
decimal
  ↓
arithmetic
  ↓
base-r
~~~

Decimal is the **middleman**.

Both are mathematically valid unless the question specifically restricts the method.

There is also a useful **hybrid/helper** approach:

~~~text
base-r divisor
     ↓
convert divisor to decimal
     ↓
calculate useful multiples in decimal
     ↓
convert those multiples back to base-r
     ↓
use them to choose the quotient digit
~~~

This is not pure direct arithmetic because decimal is helping in the middle.

---

# 16. Base-r multiplication — what finally clicked

For direct multiplication, the long-multiplication structure is normal.

The important rule is:

> **Calculate the quantity first, then write that quantity in the working base.**

For example:

~~~text
23₅ × 14₅
~~~

Start with the rightmost multiplier digit:

~~~text
3 × 4 = 12₁₀
~~~

I cannot just write 12 as a base-5 result because 12₅ means a different quantity.

Convert 12 decimal to base 5:

~~~text
12 ÷ 5 = 2 R 2
 2 ÷ 5 = 0 R 2
~~~

Read bottom → top:

~~~text
22₅
~~~

So:

~~~text
3 × 4 = 22₅
~~~

Write 2 and carry 2.
NOW HOW to know which one is WRITE and CARRY?

* rightmost digit → write
* remaining digit(s) → carry
Example in base 6:

5 * 2 = 10(10) = 14(60) <- 
So:
- Write 4 (rightmost)
- Carry 1 (carry, the next digit to the left)

Next:

~~~text
2 × 4 + 2 = 10₁₀
~~~

Convert 10 decimal to base 5:

~~~text
10 ÷ 5 = 2 R 0
 2 ÷ 5 = 0 R 2
~~~

So:

~~~text
10₁₀ = 20₅
~~~

Therefore:

~~~text
   23₅
×   4₅
-------
  202₅
~~~

Then multiply by the next digit:

~~~text
23₅ × 1₅ = 23₅
~~~

Shift left:

~~~text
230₅
~~~

Add:

~~~text
  202₅
+ 230₅
-------
  432₅
~~~

So:

~~~text
23₅ × 14₅ = 432₅
~~~

### The mistake I made at first

I saw:

~~~text
3 × 4 = 12
~~~

and wondered why I could not simply leave 12.

The reason:

~~~text
12₅ ≠ 12₁₀
~~~

The multiplication gives the quantity 12 decimal. I then have to represent that quantity in base 5:

~~~text
12₁₀ → 22₅
~~~

That distinction is important.

---

# 17. Division — the two main methods

For division I have the same two main paths.

### Method 1 — Conversion / middleman

~~~text
base-r dividend → decimal
base-r divisor  → decimal
        ↓
      divide
        ↓
decimal answer → base-r
~~~

I already understood this method fairly quickly.

### Method 2 — Direct base-r long division

~~~text
base-r dividend ÷ base-r divisor
          ↓
       long division
          ↓
     base-r answer
~~~

This is the method I was struggling with.

---

# 18. Direct division — the actual algorithm

The structure is:

~~~text
start from the left
        ↓
take enough digits so divisor fits
        ↓
find quotient digit
        ↓
multiply divisor × quotient digit
        ↓
subtract
        ↓
↓ bring down next dividend digit
        ↓
repeat
~~~

The crucial rule:

> **Do not decide a fixed number of digits to take.**

Start from the left and keep taking digits until the current chunk is at least as large as the divisor.

For binary, quotient digits are only 0 or 1.

For base 6, quotient digits are:

~~~text
0 1 2 3 4 5
~~~

---

# 19. Why direct division felt harder than it looked

I initially saw something like:

~~~text
54₆ × 4₆ = 344₆
~~~

and it looked like I was supposed to magically know that.

I am not.

That multiplication is another calculation.

~~~text
   54₆
×   4₆
------
~~~

Rightmost:

~~~text
4 × 4 = 16₁₀
~~~

Now convert 16 decimal to base 6:

~~~text
16 ÷ 6 = 2 R 4
 2 ÷ 6 = 0 R 2
~~~

Bottom → top:

~~~text
24₆
~~~

Write 4, carry 2.

Next:

~~~text
5 × 4 + 2 = 22₁₀
~~~

Convert:

~~~text
22 ÷ 6 = 3 R 4
 3 ÷ 6 = 0 R 3
~~~

So:

~~~text
22₁₀ = 34₆
~~~

Therefore:

~~~text
   54₆
×   4₆
------
  344₆
~~~

Nothing is magic.

> **If I do not know an intermediate result, I work it out.**

---

# 20. Complete direct base-6 division example

Example:

~~~text
1354₆ ÷ 54₆
~~~

No whole-problem conversion to decimal.

Step 1: Take the first two digits
Divisor = 54(6)
Start with = 13(6)
Can 54(6) fit into 13(6)?
No, because \(13_6 < 54_6\).
So take one more digit:

135(6)
Step 2: How many times does \(54_6\) fit into \(135_6\)?

54*2=144(6) [everything in base 6] 
144>135, so 54 * 1=54 is good.

So the first quotient digit is: 1

NOW subtract: 135(6)-54(60=41(6)

Step 3: Bring down the next digit, which 4 from 135[4] <-

bring it down, 414(6)

try possible combo, 
54(6)*5= 450
54 * 4 = 344 (also in base 6)

lets do this: 54 * 4 = ?
54
 4
----
4 * 4 = 16(10)
Convert 16 to base 6:

16/6 = 2 r 4
2/6 = 0 r 2

so, 24(6)

write 4, carry 2

54
 4
----
 4

so, 5 * 4 + 2 = 22(10)

convert 22 to base 6.
22 / 6 = 3 r 4
3 / 6 = 0 r 3

so, 34
write 4, carry 3

34(6)
54
 4
----
344(6)

414
344
-----
 30

now, 414-344(6)=30(6)
so:
1354/54=14(6) R 30(6)

     The whole process:
54₆ ) 1354₆(14
       54
      ---
       41
        ↓ 4
       414
       344
       ---
        30
~~~

---

# 21. The shortcut I discovered

I do **not** necessarily need to calculate:

~~~text
54₆ × 1
54₆ × 2
54₆ × 3
54₆ × 4
54₆ × 5
~~~

every time.

I can:

1. **Estimate** the quotient digit.
2. **Test** the likely candidate.
3. If it fits, check the next digit if necessary.
4. Choose the largest one that still fits.

For:

~~~text
414₆ ÷ 54₆
~~~

I expect something around 4–5.

So calculate:

~~~text
54₆ × 4₆ = 344₆
54₆ × 5₆ = 442₆
~~~

Then:

~~~text
344₆ < 414₆ < 442₆
~~~

So the quotient digit is 4.

This is the practical shortcut.

---

# 22. Another shortcut: decimal as a helper

There is another useful way to find those multiples.

First convert only the divisor:

~~~text
54₆
= 5(6) + 4
= 30 + 4
= 34₁₀
~~~

Then:

~~~text
34 × 3 = 102₁₀
34 × 4 = 136₁₀
34 × 5 = 170₁₀
~~~

Convert those results back to base 6:

~~~text
102₁₀ → 250₆
136₁₀ → 344₆
170₁₀ → 442₆
~~~

So:

~~~text
344₆ < 414₆ < 442₆
~~~

and the quotient digit is 4.

This works mathematically, but it is **not pure direct base-6 arithmetic**. Decimal is acting as a middleman/helper.

So I should keep the three approaches clearly separated:

~~~text
DIRECT
base-r → arithmetic directly in base-r

CONVERSION / MIDDLEMAN
base-r → decimal → arithmetic → base-r

HYBRID HELPER
base-r divisor → decimal
               → useful multiples
               → base-r
~~~

If the question says **"base 6 only"** or **"without converting to decimal"**, I cannot use the middleman/hybrid method.

---

# 23. The exam decision rule

Before starting a division question, ask:

> **Does the question restrict the method?**

### If it just says "divide"

The conversion/middleman method is valid:

~~~text
base-r → decimal → divide → base-r
~~~

### If it says "using base-r"

Use direct long division.

### If it says "without converting to decimal"

Use direct long division.

### If there is no restriction but I want speed

The conversion method is usually simpler.

### If I need to demonstrate base-r arithmetic

Use direct.

The course material explicitly lists **Base-R multiplication and Base-R division** as Lecture 1 topics. fileciteturn1file0L10-L25

---

# 24. What I was actually struggling with

The problem was not simply "I don't understand division."

I kept seeing intermediate answers appear without seeing **where they came from**.

For example:

~~~text
54₆ × 4₆ = 344₆
~~~

I needed to see:

~~~text
4 × 4 = 16₁₀
16₁₀ → 24₆

5 × 4 + 2 = 22₁₀
22₁₀ → 34₆

therefore 344₆
~~~

And even:

~~~text
16₁₀ → 24₆
~~~

is not magic:

~~~text
16 ÷ 6 = 2 R 4
2 ÷ 6 = 0 R 2
→ 24₆
~~~

That was the missing layer.

### My rule for future notes

> **Never let an intermediate result become "magic."**
>
> If I don't know where it came from, expand that step.

---

# 25. My clean mental map

~~~text
NUMBER SYSTEMS
│
├── CONVERSION
│   ├── Base-r → Decimal
│   │      → place value
│   │
│   ├── Decimal → Base-r
│   │      → repeated division by target base
│   │
│   └── Base-r → Base-r
│          → decimal as middleman
│
└── ARITHMETIC
    │
    ├── DIRECT
    │   └── stay in the original base
    │
    └── CONVERSION / MIDDLEMAN
        └── convert → decimal arithmetic → convert back
~~~

For division:

~~~text
DIVISION
│
├── CONVERSION
│   base-r → decimal
│          → divide
│          → base-r
│
├── DIRECT
│   long division
│   → choose quotient digit
│   → multiply
│   → subtract
│   → ↓ bring down
│   → repeat
│
└── HYBRID HELPER
    convert divisor to decimal
    → calculate useful multiples
    → convert multiples back
    → compare
~~~

---

# 26. Where I am now

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
- multiplication in another base
- direct vs conversion methods
- base-r direct division
- how to choose quotient digits
- why intermediate multiplication must actually be calculated
- how to convert an intermediate decimal result back to the working base
- the decimal-middleman shortcut
- when the shortcut is and isn't allowed

The biggest lesson from this part was:

> **Don't let an intermediate step become "magic."**

If I need:

~~~text
54₆ × 4₆
~~~

I calculate it.

If I get:

~~~text
16₁₀
~~~

and need base 6, I convert it.

If I use decimal as a helper, I know I'm using a **middleman/helper method**, not pretending it was direct.

That distinction keeps the whole topic organized in my head.

---

## Next

Move on after I have done a few more direct base-r division problems. I do not need to keep re-learning the concept; I need enough practice for the quotient-digit selection and bring-down process to become automatic.
