# SHA-256 Structure

SHA-256 processes a message in **512-bit blocks**. Each block is expanded and processed through **64 rounds** to produce a 256-bit chaining value.

## 1. Message-block size

SHA-256 processes the padded message in blocks of:

**512 bits = 64 bytes**

Each 512-bit block is processed separately.

```text
512-bit message block
        ↓
   split into 32-bit words
        ↓
W₀ W₁ W₂ ... W₁₅
```

Since:

$$
512 / 32 = 16
$$

each block initially contains **16 words of 32 bits**.

---

## 2. Initial 32-bit message words

The 512-bit block is divided into:

$$
W_0,W_1,\ldots,W_{15}
$$

Each `Wᵢ` is **32 bits**.

So:

$$
16 \times 32 = 512\text{ bits}
$$

For example:

```text
512-bit block

| W₀ | W₁ | W₂ | ... | W₁₄ | W₁₅ |
 32   32   32          32    32 bits
```

### What do W₀,...,W₁₅ mean?

They are simply the **16 original 32-bit pieces of the current message block**.

They are not hash values. They are parts of the input message.

---

## 3. Expansion to W₀,...,W₆₃

SHA-256 needs **64 words** for its 64 rounds.

The first 16 words come directly from the message:

$$
W_0,\ldots,W_{15}
$$

The remaining words are calculated from earlier words:

$$
W_t =
\sigma_1(W_{t-2})
+ W_{t-7}
+ \sigma_0(W_{t-15})
+ W_{t-16}
\pmod{2^{32}}
$$

for:

$$
16 \leq t \leq 63
$$

where:

$$
\sigma_0(x)=ROTR^7(x)\oplus ROTR^{18}(x)\oplus SHR^3(x)
$$

and:

$$
\sigma_1(x)=ROTR^{17}(x)\oplus ROTR^{19}(x)\oplus SHR^{10}(x)
$$

So the structure is:

```text
Original message block
        ↓
W₀ ... W₁₅
        ↓
   message schedule
        ↓
W₁₆ ... W₆₃
```

Therefore, one 512-bit block produces a **64-word message schedule**.

---

## 4. Number of rounds

SHA-256 performs:

**64 rounds**

Each round uses:

* one message-schedule word `Wₜ`
* one round constant `Kₜ`
* the current working variables `a,b,c,d,e,f,g,h`

```text
Round 0  → W₀,  K₀
Round 1  → W₁,  K₁
Round 2  → W₂,  K₂
...
Round 63 → W₆₃, K₆₃
```

---

## 5. Meaning of a, b, c, d, e, f, g, h

SHA-256 maintains an internal state of:

$$
8 \times 32 = 256\text{ bits}
$$

These eight 32-bit words are represented by:

```text
a  b  c  d  e  f  g  h
```

At the beginning of processing a block, they are initialized from the current **256-bit chaining value**.

For the first block:

```text
a b c d e f g h
↓
SHA-256 initial hash values (IV)
```

During every round, these eight values are updated.

A simplified view is:

```text
a b c d e f g h
        ↓
      Round
        ↓
a' b' c' d' e' f' g' h'
```

After 64 rounds, the resulting values are combined with the previous chaining value to produce the next chaining value.

---

## 6. Role of K₀,...,K₆₃

SHA-256 has **64 fixed 32-bit constants**:

$$
K_0,K_1,\ldots,K_{63}
$$

Each round uses one constant:

```text
Round 0  → K₀
Round 1  → K₁
...
Round 63 → K₆₃
```

These constants are part of the SHA-256 algorithm and are the same for every message.

They are derived from the **fractional parts of the cube roots of the first 64 prime numbers**.

Their purpose is to introduce fixed, nonlinear-looking variation into each round and prevent the round operations from having overly simple symmetry.

---

## 7. Ch (Choose)

`Ch` means **Choose**.

It is defined as:

$$
Ch(x,y,z)=(x\land y)\oplus(\neg x\land z)
$$

The idea is:

> For each bit, choose the bit from `y` or `z` depending on the corresponding bit of `x`.

If a bit of `x` is:

```text
x = 1 → choose y
x = 0 → choose z
```

Example for one bit:

```text
x = 1, y = 0, z = 1
       ↓
     choose y
       ↓
     result = 0
```

So `Ch` acts like a **bit-by-bit selector**.

---

## 8. Maj (Majority)

`Maj` means **Majority**.

It is defined as:

$$
Maj(x,y,z)
=
(x\land y)\oplus(x\land z)\oplus(y\land z)
$$

For each bit, it gives the value that appears in the **majority of x, y, and z**.

Example:

```text
x = 1
y = 1
z = 0

majority = 1
```

Another example:

```text
x = 0
y = 1
z = 1

majority = 1
```

So:

```text
Ch → chooses between two inputs
Maj → takes the majority bit
```

---

## 9. How Ch and Maj are used

SHA-256 also uses two larger functions:

$$
\Sigma_0(a)
=
ROTR^2(a)\oplus ROTR^{13}(a)\oplus ROTR^{22}(a)
$$

$$
\Sigma_1(e)
=
ROTR^6(e)\oplus ROTR^{11}(e)\oplus ROTR^{25}(e)
$$

Then each round calculates:

$$
T_1 =
h+\Sigma_1(e)+Ch(e,f,g)+K_t+W_t
\pmod{2^{32}}
$$

and:

$$
T_2 =
\Sigma_0(a)+Maj(a,b,c)
\pmod{2^{32}}
$$

These values are then used to update `a` through `h`.

Simplified:

```text
          a b c d e f g h
                  │
        ┌─────────┴─────────┐
        │                   │
     Maj(a,b,c)          Ch(e,f,g)
        │                   │
      Σ₀(a)               Σ₁(e)
        │                   │
        └───────┬───────────┘
                │
       Kₜ and Wₜ are added
                │
             T₁, T₂
                │
                ↓
       new a,b,c,d,e,f,g,h
```

---

## 10. Addition modulo 2³²

This is very important.

SHA-256 works with **32-bit words**, so additions are performed modulo:

$$
2^{32}
$$

That means if the result is larger than the largest 32-bit value, the overflow is discarded.

A 32-bit unsigned value can represent:

$$
0 \text{ to } 2^{32}-1
$$

For example:

```text
2³² - 1
+    1
---------
2³²
```

But SHA-256 keeps only the lower 32 bits:

$$
2^{32}\mod 2^{32}=0
$$

So:

```text
FFFFFFFF
+       1
---------
00000000
```

This is essentially **32-bit wraparound arithmetic**.

The same rule is used in the formulas for `T₁`, `T₂`, and the message schedule.

---

# Complete SHA-256 structure

Putting everything together:

```text
Padded message
      ↓
512-bit block Mᵢ
      ↓
Split into 16 × 32-bit words
      ↓
W₀ ... W₁₅
      ↓
Message schedule expansion
      ↓
W₀ ... W₆₃
      ↓
Initialize a,b,c,d,e,f,g,h
from current 256-bit state
      ↓
┌─────────────────────────────┐
│       64 SHA-256 rounds     │
│                             │
│ Wₜ + Kₜ                     │
│ Σ₀ + Maj                    │
│ Σ₁ + Ch                     │
│ 32-bit modular additions    │
│                             │
└─────────────────────────────┘
      ↓
New 256-bit chaining value
      ↓
Next 512-bit block
      ↓
...
      ↓
Final 256-bit digest
```

## Summary

| Component             | SHA-256                 |
| --------------------- | ----------------------- |
| Message block         | **512 bits**            |
| Initial message words | **16 × 32 bits**        |
| Initial words         | `W₀ ... W₁₅`            |
| Expanded words        | `W₀ ... W₆₃`            |
| Total schedule words  | **64**                  |
| Number of rounds      | **64**                  |
| Working variables     | `a,b,c,d,e,f,g,h`       |
| Working variable size | **32 bits each**        |
| Internal state        | **256 bits**            |
| Round constants       | `K₀ ... K₆₃`            |
| `Ch`                  | Bitwise choice/selector |
| `Maj`                 | Bitwise majority        |
| Arithmetic            | Addition modulo **2³²** |
| Final digest          | **256 bits**            |
