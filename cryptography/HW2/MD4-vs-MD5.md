# MD4 vs MD5

## Overview

**MD4** and **MD5** are cryptographic hash functions designed by Ronald Rivest. MD5 was introduced as a strengthened successor to MD4 after weaknesses were identified in the MD4 design.

Both algorithms process messages in **512-bit blocks** using **32-bit words** and produce a **128-bit message digest**.

---

## MD4 vs MD5

| Feature                |      MD4 |        MD5 |
| ---------------------- | -------: | ---------: |
| **Digest length**      | 128 bits |   128 bits |
| **Message-block size** | 512 bits |   512 bits |
| **Word size**          |  32 bits |    32 bits |
| **Number of rounds**   |        3 |          4 |
| **Steps per round**    |       16 |         16 |
| **Total steps**        |       48 |         64 |
| **Boolean functions**  |  F, G, H | F, G, H, I |
| **Security status**    |   Broken |     Broken |

---

## Boolean Functions

The algorithms use Boolean functions that operate on 32-bit words.

### MD4

MD4 uses three functions:

```text
F(X,Y,Z) = (X AND Y) OR (NOT X AND Z)

G(X,Y,Z) = (X AND Y) OR (X AND Z) OR (Y AND Z)

H(X,Y,Z) = X XOR Y XOR Z
```

These functions are used across the three rounds.

### MD5

MD5 keeps the same general structure but adds a fourth function:

```text
F(X,Y,Z) = (X AND Y) OR (NOT X AND Z)

G(X,Y,Z) = (X AND Z) OR (Y AND NOT Z)

H(X,Y,Z) = X XOR Y XOR Z

I(X,Y,Z) = Y XOR (X OR NOT Z)
```

The new **I function** is used in MD5's fourth round.

---

## Why Was MD5 Designed?

MD5 was created as a **strengthened successor to MD4**.

MD4 was designed to be very fast, but its relatively simple structure provided less security margin than desired. MD5 introduced additional complexity and mixing to make attacks more difficult.

### Main changes from MD4 to MD5

1. **Added a fourth round**

   * MD4: 3 rounds / 48 steps
   * MD5: 4 rounds / 64 steps

2. **Added a fourth Boolean function**

   * MD4: F, G, H
   * MD5: F, G, H, I

3. **Changed the G function**

   * MD5 uses a less symmetric version of the function.

4. **Changed the order of message words**

   * The message words are processed in different orders in some rounds.

5. **Changed the rotation amounts**

   * Different left-rotation values were used to improve mixing.

6. **Introduced different constants**

   * MD5 uses a different constant for each of its 64 steps.

These changes were intended to improve the algorithm's **diffusion, mixing, and security margin** compared with MD4.

---

## What Is a Collision?

A **collision** occurs when two different messages produce the same hash:

```text
Message A ≠ Message B

but

Hash(Message A) = Hash(Message B)
```

For a secure cryptographic hash function, finding such a pair should be computationally infeasible.

---

## Security Status

### MD4

MD4 is **cryptographically broken**.

Researchers developed collision attacks that can find collisions much more efficiently than would be expected for a secure 128-bit hash function.

MD4 should not be used for cryptographic security.

### MD5

MD5 is also **cryptographically broken with respect to collision resistance**.

Practical collision attacks have been demonstrated against MD5. As a result, MD5 should not be used for:

* Digital signatures
* Certificates
* Cryptographic authentication
* Other applications requiring collision resistance

MD5 may still appear as a **non-security checksum** for detecting accidental data corruption, but this is different from using it as a cryptographic hash.

---

## Why Are Both No Longer Collision-Resistant?

MD5 improved upon MD4, but the improvements were not enough to prevent later cryptanalysis.

The progression can be summarized as:

```text
MD4
 ↓
Weaknesses discovered
 ↓
MD5 designed as a strengthened successor
 ↓
More rounds + more mixing + additional function
 ↓
Later cryptanalysis
 ↓
Practical collision attacks
 ↓
MD4 and MD5 considered cryptographically broken
```

The important lesson is:

> **Adding complexity to a hash function does not guarantee long-term collision resistance.**

Modern cryptographic applications generally use stronger hash functions such as **SHA-256** or members of the **SHA-3** family.

---

## Quick Summary

```text
MD4:
128-bit digest
512-bit blocks
32-bit words
3 rounds
48 steps
F, G, H
Broken

MD5:
128-bit digest
512-bit blocks
32-bit words
4 rounds
64 steps
F, G, H, I
Broken for collision resistance
```

### Key Takeaway

**MD5 was designed to strengthen MD4 by adding another round, a new Boolean function, different constants, message ordering, and rotation amounts. However, both MD4 and MD5 were eventually shown to be vulnerable to practical collision attacks and are no longer considered collision-resistant.**
