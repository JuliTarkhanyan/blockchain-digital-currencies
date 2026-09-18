# Initialization Vector (IV) and H₀

## 1. What is an IV?

An **Initialization Vector (IV)** is the fixed initial value of the internal state of a hash function.

An iterative hash function processes the message **block by block**. The first block needs some starting state before the compression function can operate.

So the construction looks like:

```text
                 IV
                  ↓
Message block 1 → Compression
                  ↓
                H(1)
                  ↓
Message block 2 → Compression
                  ↓
                H(2)
                  ↓
Message block 3 → Compression
                  ↓
                H(3)
                  ↓
                 ...
                  ↓
                Hash
```

The IV provides the **starting state**.

Without it, the compression function would not have an initial chaining value for the first message block.

---

# 2. Why is it called an Initialization Vector?

It is called an **initialization** vector because it initializes the internal state before processing the first block.

For an iterative hash function:

```text
H(i) = Compression(H(i-1), M(i))
```

where:

* `M(i)` = the current message block
* `H(i-1)` = state produced by the previous block
* `H(i)` = new state

For the **first block**, there is no previous message block.

So we need:

```text
H(0) = IV
```

Then:

```text
H(1) = Compression(H(0), M(1))
```

and:

```text
H(2) = Compression(H(1), M(2))
```

and so on.

Therefore:

```text
H(0) = IV
```

is the starting point of the whole computation.

---

# 3. Why is the initial state called H⁽⁰⁾?

The superscript does **not** mean that the hash function has already processed the first message block.

It means:

```text
H(0) = state after processing 0 message blocks
```

Therefore:

```text
Before processing anything:

H(0) = IV
```

After the first block:

```text
H(1) = C(H(0), M(1))
```

After the second block:

```text
H(2) = C(H(1), M(2))
```

After the third block:

```text
H(3) = C(H(2), M(3))
```

So the notation is simply counting how many blocks have been processed:

```text
       0 blocks        1 block        2 blocks        3 blocks

          ↓               ↓              ↓              ↓

        H(0) ──→        H(1) ──→       H(2) ──→       H(3)
          ↑
         IV
```

The final state becomes the hash output (possibly after a final formatting step depending on the algorithm).

---

# 4. MD5 IV

MD5 has a **128-bit internal state**.

It consists of:

```text
4 words × 32 bits
= 128 bits
```

The four initial words are:

```text
A = 67452301
B = efcdab89
C = 98badcfe
D = 10325476
```

Therefore:

```text
H(0) =

67452301
efcdab89
98badcfe
10325476
```

### Calculation of the state size

```text
4 words × 32 bits
= 128 bits
```

So:

```text
MD5 IV = 128 bits
```

The processing starts as:

```text
H(0)
 ↓
┌──────────┬──────────┬──────────┬──────────┐
│67452301  │efcdab89  │98badcfe  │10325476  │
│    A     │    B     │    C     │    D     │
└──────────┴──────────┴──────────┴──────────┘
                 ↓
        process first 512-bit block
                 ↓
                H(1)
```

---

# 5. SHA-1 IV

SHA-1 has a **160-bit internal state**.

It consists of:

```text
5 words × 32 bits
= 160 bits
```

The initial words are:

```text
H0 = 67452301
H1 = efcdab89
H2 = 98badcfe
H3 = 10325476
H4 = c3d2e1f0
```

Therefore:

```text
H(0) =

67452301
efcdab89
98badcfe
10325476
c3d2e1f0
```

### Calculation

```text
5 × 32
= 160 bits
```

So:

```text
SHA-1 IV = 160 bits
```

The construction begins:

```text
H(0)
 ↓
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│67452301  │efcdab89  │98badcfe  │10325476  │c3d2e1f0  │
│    A     │    B     │    C     │    D     │    E     │
└──────────┴──────────┴──────────┴──────────┴──────────┘
                         ↓
                  first message block
                         ↓
                        H(1)
```

Notice something interesting:

The first four SHA-1 words are the same as the MD5 IV words, but SHA-1 has a fifth word:

```text
c3d2e1f0
```

So:

```text
MD5:    4 × 32 = 128 bits
SHA-1:  5 × 32 = 160 bits
```

---

# 6. SHA-256 IV

SHA-256 has a **256-bit internal state**.

It consists of:

```text
8 words × 32 bits
= 256 bits
```

The initial words are:

```text
H0 = 6a09e667
H1 = bb67ae85
H2 = 3c6ef372
H3 = a54ff53a
H4 = 510e527f
H5 = 9b05688c
H6 = 1f83d9ab
H7 = 5be0cd19
```

Therefore:

```text
H(0) =

6a09e667
bb67ae85
3c6ef372
a54ff53a
510e527f
9b05688c
1f83d9ab
5be0cd19
```

### Calculation

```text
8 words × 32 bits
= 256 bits
```

Therefore:

```text
SHA-256 IV = 256 bits
```

The construction begins:

```text
H(0)
 ↓
┌──────────┬──────────┬──────────┬──────────┐
│6a09e667  │bb67ae85  │3c6ef372  │a54ff53a  │
├──────────┼──────────┼──────────┼──────────┤
│510e527f  │9b05688c  │1f83d9ab  │5be0cd19  │
└──────────┴──────────┴──────────┴──────────┘
                    ↓
             first 512-bit block
                    ↓
                   H(1)
```

---

# 7. Comparison

| Hash        | Number of words | Word size | Total state / IV size |
| ----------- | --------------: | --------: | --------------------: |
| **MD5**     |               4 |   32 bits |              128 bits |
| **SHA-1**   |               5 |   32 bits |              160 bits |
| **SHA-256** |               8 |   32 bits |              256 bits |

### IV values

**MD5:**

```text
67452301 efcdab89 98badcfe 10325476
```

**SHA-1:**

```text
67452301 efcdab89 98badcfe 10325476 c3d2e1f0
```

**SHA-256:**

```text
6a09e667 bb67ae85 3c6ef372 a54ff53a
510e527f 9b05688c 1f83d9ab 5be0cd19
```

---

# 8. How the IV is used

Suppose the padded message contains three blocks:

```text
M(1) M(2) M(3)
```

The calculation is:

```text
H(0) = IV

H(1) = C(H(0), M(1))

H(2) = C(H(1), M(2))

H(3) = C(H(2), M(3))
```

Graphically:

```text
                IV
                 │
                 ▼
             H(0)
                 │
                 │ M(1)
                 ▼
          ┌─────────────┐
          │ Compression │
          └─────────────┘
                 │
                 ▼
               H(1)
                 │
                 │ M(2)
                 ▼
          ┌─────────────┐
          │ Compression │
          └─────────────┘
                 │
                 ▼
               H(2)
                 │
                 │ M(3)
                 ▼
          ┌─────────────┐
          │ Compression │
          └─────────────┘
                 │
                 ▼
               H(3)
                 │
                 ▼
               HASH
```

This is why these algorithms are called **iterative hash functions**: the output state from one step becomes the input state for the next step.

---

# 9. Why is the IV fixed?

The IV is normally a **fixed, publicly known constant**.

For example, SHA-256 always starts with:

```text
6a09e667
bb67ae85
3c6ef372
a54ff53a
510e527f
9b05688c
1f83d9ab
5be0cd19
```

It is not a secret key.

Everyone computing SHA-256 starts from the same state.

This is necessary because otherwise two people could hash the same message and get different results.

For example:

```text
Person A:

Message
  ↓
SHA-256 IV
  ↓
Hash A


Person B:

Message
  ↓
SHA-256 IV
  ↓
Hash B
```

Because the IV is the same:

```text
Hash A = Hash B
```

for the same message and same algorithm.

---

# 10. Where do the IV constants come from?

The IV values are not simply random numbers.

For SHA-256, the initial values are derived from the **fractional parts of the square roots of the first eight prime numbers**.

The first eight primes are:

```text
2, 3, 5, 7, 11, 13, 17, 19
```

For each prime:

```text
sqrt(prime)
```

Take the fractional part and multiply by:

```text
2^32
```

Then take the integer part.

For example, for prime 2:

```text
sqrt(2) ≈ 1.41421356237...
```

Fractional part:

```text
0.41421356237...
```

Multiply by:

```text
2^32
```

which gives approximately:

```text
1779033703...
```

Convert that integer to hexadecimal:

```text
1779033703 = 6a09e667
```

So the first SHA-256 IV word is:

```text
H0 = 6a09e667
```

The same process is used for:

```text
2 → 6a09e667
3 → bb67ae85
5 → 3c6ef372
7 → a54ff53a
11 → 510e527f
13 → 9b05688c
17 → 1f83d9ab
19 → 5be0cd19
```

This gives the complete SHA-256 initial state.

---

# 11. Why use such constants?

The purpose is to provide a **standard, fixed starting point** without making the IV look like it was specially chosen to create a weakness.

The values are:

* publicly known
* deterministic
* reproducible
* fixed by the specification

This allows everyone to implement the same hash function.

---

# 12. Final construction

The complete iterative structure can therefore be written as:

```text
H(0) = IV

H(i) = C(H(i-1), M(i))
```

For `n` message blocks:

```text
H(0) = IV

H(1) = C(H(0), M(1))
H(2) = C(H(1), M(2))
H(3) = C(H(2), M(3))
...
H(n) = C(H(n-1), M(n))
```

Then:

```text
Hash = H(n)
```

### Key idea

> **The IV is the initial internal state before any message block is processed. It is denoted \(H^{(0)}\) because zero message blocks have been processed. After each block, the state becomes \(H^{(1)}, H^{(2)}, \ldots\), until the final state \(H^{(n)}\) becomes the hash output.**

```text
          IV
           ↓
         H(0)
           ↓
     + Message 1
           ↓
         H(1)
           ↓
     + Message 2
           ↓
         H(2)
           ↓
          ...
           ↓
         H(n)
           ↓
         HASH
```
