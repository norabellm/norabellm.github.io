---
title: "Binary Hex Octal Math : A weird number system in computer"
date: 2023-09-26T18:14:46+07:00
draft: false
---

Counting from one to ten is quite obivious for us, since most human have ten finger. 
Even tho, it not stopping math from counting in another weird form. Our limit to counting from
one to ten is called **radix** or **base**, for precise radix 10 / base 10. I will use base for the rest of this post.

For the clarity what radix job is, let me give you an example. Think you doing math, nine plus one (`9 + 1`). Nine in here is the last number in base 10. So, when we add it with one it shifting to left, The first digit return back to zero (`9 -> 0`) and adding new digit at left start from 1 (`9 -> 10`). 

Note, 0 already there in left digit actually (`09`) but unwritten for convinience, so basicly when shifting we add 1 to least left digit. So that why ten is written `10` (one zero).

This rule goes on until it reach 9 in second digit and grow another digit endlessly. To simply put, the radix job is rule to determine when new digit add to the left.

### Base 16 or Hexadecimal

The base 10 is largely acceptable because our physiology. How about sixteen ? your could use your feet toes or grow tentacle from your back like Cthulhu (pun intended). Base 16 are quite similar with base 10 with extra six member. The six member is A, B, C, D, E and F. This math quite drunk, huh? This not algebra class so do not worry for finding equations.

Speaking for equation here a table both base 10 and base 16 comparison.

| Base 10 | Base 16|
|--|--|
| 00 | 00 |
| 01 | 01 |
| 02 | 02 |
| 03 | 03 |
| 04 | 04 |
| 05 | 05 |
| 06 | 06 |
| 07 | 07 |
| 08 | 08 |
| 09 | 09 |
| 10 | 0A | 
| 11 | 0B |
| 12 | 0C |
| 13 | 0D |
| 14 | 0E |
| 15 | 0F |
| 16 | 10 |

For example of doing Base 16, let take look to these operation:

- `10 - 0F`
- `06 + 0A`

There two way to approach this math, first by convert it to decimal for convinience or directly step in base 16 way.

- Conversion to decimal method. We need convert `10` and `0F` to decimal which is `10 -> 16` and `0F -> 15`. Do the operation, substract it by arithmatic, `16 - 15 = 1` then convert back to Base 16, `01 -> 01`.

- Stepping the Base 16 method. We just need to step up based our operation, in this case `06 + 0A` so we will step up. Format `<number steped>(<the step number>)` : 

    - `06(00), 07(01), 08(02), 09(03), 0A(04), 0B(05)`
    - `0C(06), 0D(07), 0E(08), 0F(09), 10(0A)`
    - So, there result is `06 + 0A = 10`.

Little exercise for you, what result of `2F + 30` and what the decimal of it ?

### Base 8 or Octal

Base 8 is clearly half of Base 16. It only count from one to eight. So when you sum `7` with `1` in turn to `10`. Yet another drunk math, huh? I admit this base could confuse you with base 10. In octal you can not have number `8` or `9`, which the ratio again is still `2/10` against base 10 to distiguish.

| Base 8 | Base 10|
|--|--|
| 00 | 00 |
| 01 | 01 |
| 02 | 02 |
| 03 | 03 |
| 04 | 04 |
| 05 | 05 |
| 06 | 06 |
| 07 | 07 |
| 10 | 08 |
| 11 | 09 |
| 12 | 10 | 

Here some example to do arithmatic with base 8,

- `15 + 6`

Conversion to base 10 method is less feasible since we do number less than 10. So, the point just shift digit after the number step to `7`.

- `15(00), 16(01), 17(02), 20(03), 21(04), 22(05), 23(06)`, so the result is `15 + 6 = 23`.
- If in base 10, we could get result `21`.

You might be think so to get base 10 result just subtract 2 from base 8 result since the base just have 2 range different ? Maybe, lets check the base 2.

Let some exercise:

- `12 + 7`
- `30 - 21`

### Base 2 or Binary

Base 2 just consisted of 2 member : `0` and `1`. Since this base 2 is quite small, there are many range of method to utilize binary, but in here we focus talk about it simple arithmatic operation. 

Please bear with me in this part, binary is extermly useful if you could master in it conversion so you will getting used with many binary rule that imposed in computer such signed bit, so for that well graps in these part.

When writing 0 and 1 in binary we just simple do 0 and 1, but when writting 2 it got `10` and 3 got `11`. Drunkest way to count ? Yes for human but this simple approach help human to utilize transistor for serving human need in computation. 

This is because base 2 is lowest base and the least divisor of all base mention above, so this base could simplify higher bases.

Comparison

| Base 2 | Base 10|
|--|--|
| 0000 | 00 |
| 0001 | 01 |
| 0010 | 02 |
| 0011 | 03 |
| 0100 | 04 |
| 0101 | 05 |
| 0110 | 06 |
| 0111 | 07 |
| 1000 | 08 |

Since binary heavly depend in shifting, when it come to number that power of 2 it shift. Like `1 + 1` it become `10` and `11 + 1` it become `100`. These one trick for map binary by it number of digits. One unit that consist of 4 zero in binary called byte that could hold value from 0 to 16, all range of base 16.

Pratice time:

- 0100 + 0110 
- 0010 - 0101

To sum two binary, we need put it the position accordingly.

```txt
0100
0110
---- +
????

Then we perform addition from left digit to right digit.

 # 1, we sum the left most from both number, 0 + 0 = 0.
0100
0110 
---- +
??? 0 (0+0) 

 # 2, then move one digit to the right
0100
0110
----+
?? 1 (0+1) 0

 # 3, again move one digit, 1 + 1 = 10. Because this only one digit operation,
 # we took the 0 because it first digit and same digit position then the
 # 1 we store for next digit operation. So the result, is 0 with debt 1.
0100
0110
----+
? 0 (1+1) 1 0

 # 4, then move one digit to the right and add 1 from previous, 0 + 0 + 1 = 1
0100
0110
1
----+
1 (0+0+1) 0 1 0

So, the result is 1010. If translated to decimal:

1*(2^3) + 0(2^2) + 1(2^1) + 0(2^0) =
8 + 0 + 2 + 0 = 10
```


## Conversion by Induction

To convert all base respectively, we need a method. The method is convert it to another base 
by using multiply all number with base and it power of by their digits index (`rbase ^ index`) 
and sum it all. This method inducing base power digit index with target number.

```txt
element * (base ^ digit index) + element * (base ^ digit index) 
+ ... (until all element) 

```

For example, `45` is base 8. There are two number, `4` at second (2) and `5` at first (1), this is the position so we need to it to index by subtract by `1`. 

| Number | Position | Index |
|--|--|--|
| 4 | 2 | 1 |
| 5 | 1 | 0 |

`5` at index 0 and `4` at index 1.

After we got the index, we need to multiple all number with it base power with it index (8 ^ index) then sum all result, so here the result

```txt
(4*(8^1)) + (5*(8^0)) =
32_b10 + 5_b10 = 37_b10
```

Note, `_b10` is meaning I do numbering in base 10.

This is base 8 example, and the general idea would same. The key is what number system you do in multiplication and sum when convertion. Converting it to base 10 is most intuitive method to us. You could convert it to other base by stepping number using their rule, try another number system.

For example, `(4*(8^1))` is result `32` because I step it in base 10 but you could step it in base 16 so your got `1F` or `100000` in base 2. Its fine if you multiple in decimal then convert it to base 16, in case you not fond with multiplication in non base 10.

Example convertion to base 16

```txt
(4*(8^1)) + (5*(8^0)) =
1F_b16 + 5_b16 = 24_b16
```

Example convertion to base 2

```txt
(4*(8^1)) + (5*(8^0)) =
10000_b2 + 101_b2 = 10101_b2
```

Another example:

- `2F_b16` into base 2.

```
2 

```

- `1011000_b2` into base 16


### Exercise

- `69420_b10`, convert it to base 16 and base 8.
- `420_b16`, convert it to base 2 and base 10.


## Convertion by Base Round Value Deduction

Induction is good convertion for you that fast in math. Fortunately, there are way 
for people who slow at math. This method likely work well in base 2 only.

Base round value is when value that round in base such `100_b2` or `1000_b2`. These
method depend on pattern matching of these base round value and digit. To make this 
method effective we need remember some rainbow table and it pattern.

| Base 2 | Base 10 | Base 16 |
|--|--|--|
| 1 | 1 | 1 |
| 10 | 2 | 2 |
| 100 | 4 | 4 |
| 1000 | 8 | 8 |
| 10000 | 16 | 10 |
| 100000 | 32 | 20 |
| 1000000 | 64 | 40 |

- Pattern for base 10 multiply 2 when add new 0 digit.
- Pattern for base 16 multiply 2 when add new 0 digit and the value 10 or higher.

For example a `10110_b2` and I try to convert it to base 10, now let puzzle it out from rainbow table above.

```txt
10110

# 1. Diassamble the binary into round value, 
then sum it with the matched pattern.

10000_b2 = 16_b10 = 10_b16
00100_b2 = 4_b10  = 4_b16
00010_b2 = 2_b10  = 2_b16
----- +
10110_b2 = 22_b10 = 16_b16
```
