# SHA-256 versus SHA3-256

SHA-256 and SHA3-256 both produce a **256-bit hash**, but they use fundamentally different constructions.

* **SHA-256:** iterative **compression-function** construction
* **SHA3-256:** **sponge** construction based on the Keccak permutation

---

# 1. SHA-256 Construction

SHA-256 processes the message in **512-bit blocks**.

For each block, the algorithm takes:

* the previous **256-bit chaining value**
* the current **512-bit message block**

and applies the compression function.

### Basic construction

```text
Message
   ↓
Padding
   ↓
M₁       M₂       M₃       ...       Mₜ
 ↓        ↓        ↓                  ↓
┌────┐  ┌────┐  ┌────┐             ┌────┐
│ C  │  │ C  │  │ C  │     ...     │ C  │
└────┘  └────┘  └────┘             └────┘
  ↑        ↑        ↑                  ↑
 H⁽⁰⁾     H⁽¹⁾     H⁽²⁾              H⁽ᵗ⁻¹⁾
  │        │        │                  │
 IV       H⁽¹⁾     H⁽²⁾              H⁽ᵗ⁾
                                           ↓
                                      256-bit
                                       digest
```

The iteration is:

$$
H^{(0)}=IV
$$

and:

$$
H^{(i)}=C(H^{(i-1)},M_i)
$$

where:

* \(H^{(i)}\) = chaining value after block \(i\)
* \(M_i\) = message block
* \(C\) = compression function
* \(IV\) = initial value

---

## 2. What happens inside the SHA-256 compression function?

Each message block has:

$$
512\text{ bits}
$$

The chaining value has:

$$
256\text{ bits}
$$

The 512-bit message block is divided into:

$$
16\times32\text{-bit words}
$$

and expanded into:

$$
W_0,\ldots,W_{63}
$$

The compression function then performs:

$$
64\text{ rounds}
$$

using eight 32-bit working variables:

$$
a,b,c,d,e,f,g,h
$$

Each round uses:

* \(W_t\)
* \(K_t\)
* `Ch`
* `Maj`
* rotations and shifts
* addition modulo \(2^{32}\)

A simplified round is:

$$
T_1 =
h+\Sigma_1(e)+Ch(e,f,g)+K_t+W_t
\pmod{2^{32}}
$$

$$
T_2 =
\Sigma_0(a)+Maj(a,b,c)
\pmod{2^{32}}
$$

After 64 rounds, the result is combined with the previous chaining value to produce the next 256-bit state.

```text
       256-bit H⁽ⁱ⁻¹⁾
              │
              │
              ▼
        ┌─────────────┐
        │             │
512-bit │ Compression │
Mᵢ ────►│  function   │
        │             │
        │ 64 rounds   │
        └──────┬──────┘
               │
               ▼
          256-bit H⁽ⁱ⁾
```

### Important point

SHA-256 has a **fixed-size chaining state of 256 bits**.

The message is processed block by block, and each compression function produces the next chaining value.

---

# 3. SHA3-256 Construction

SHA3-256 does **not** use the same type of chaining-value compression construction.

It uses a **sponge construction**.

The main internal state has:

$$
1600\text{ bits}
$$

The state is divided into two parts:

$$
1600=r+c
$$

where:

* \(r\) = **rate**
* \(c\) = **capacity**

For SHA3-256:

$$
c=512\text{ bits}
$$

Therefore:

$$
r=1600-512
$$

$$
\boxed{r=1088\text{ bits}}
$$

So:

$$
\boxed{r=1088,\quad c=512}
$$

---

# 4. Why is the capacity 512 bits?

SHA3-256 needs a 256-bit output.

For SHA-3, the capacity is chosen as:

$$
c=2\times256
$$

Therefore:

$$
c=512
$$

Then the 1600-bit Keccak state gives:

$$
r=1600-512=1088
$$

So the state is:

```text
1600 bits
┌──────────────────────────────────────────────────────┐
│                    Rate r                            │
│                  1088 bits                           │
├──────────────────────────────────────────────────────┤
│                 Capacity c                           │
│                  512 bits                            │
└──────────────────────────────────────────────────────┘
```

---

# 5. What is the rate \(r\)?

The **rate** is the part of the state that directly interacts with the input and output.

For SHA3-256:

$$
r=1088\text{ bits}
$$

Since:

$$
1088/8=136
$$

the rate corresponds to:

$$
\boxed{136\text{ bytes}}
$$

Therefore, SHA3-256 absorbs the message **136 bytes at a time**.

---

# 6. What is the capacity \(c\)?

The capacity is the remaining part of the state:

$$
c=512\text{ bits}
$$

It is not directly filled with message data.

The capacity provides part of the security strength of the sponge.

For SHA3-256:

$$
c=512
$$

and the security level against generic attacks is related to:

$$
c/2=512/2=256
$$

which corresponds to the 256-bit hash output/security target.

---

# 7. The Permutation \(p\)

SHA3-256 uses the **Keccak-f[1600] permutation**.

The internal state is:

$$
1600\text{ bits}
$$

and the permutation keeps the state size unchanged:

$$
1600 \rightarrow 1600
$$

It consists of:

$$
\boxed{24\text{ rounds}}
$$

Each round transforms the entire 1600-bit state.

The five operations inside a Keccak round are:

$$
\theta,\rho,\pi,\chi,\iota
$$

So:

```text
1600-bit state
      ↓
   θ (Theta)
      ↓
   ρ (Rho)
      ↓
   π (Pi)
      ↓
   χ (Chi)
      ↓
   ι (Iota)
      ↓
1600-bit state
```

This complete 24-round permutation is commonly written as:

$$
p = Keccak\text{-}f[1600]
$$

---

# 8. Absorbing Phase

Before absorption, SHA3 applies its padding.

The padded message is divided into blocks of the rate:

$$
r=1088\text{ bits}
$$

Suppose the padded message is:

$$
P=P_1||P_2||\cdots||P_n
$$

where each:

$$
P_i=1088\text{ bits}
$$

The initial state is:

$$
S_0=0^{1600}
$$

The first message block is XORed into the **rate portion** of the state:

$$
S_0[0:1088]\oplus P_1
$$

The capacity remains unchanged at this point.

Then the permutation is applied:

$$
S_1=p(S_0\oplus P_1)
$$

The second block is absorbed:

$$
S_2=p(S_1\oplus P_2)
$$

and so on.

Therefore:

$$
S_i=p(S_{i-1}\oplus P_i)
$$

This is the **absorbing phase**.

### Schematic

```text
                    Absorbing phase

P₁ ───────┐
          XOR
           ↓
       ┌───────────────┐
       │ 1600-bit      │
       │    State      │
       └───────┬───────┘
               ↓
          permutation p
               ↓
       ┌───────────────┐
P₂ ──► │     XOR       │
       └───────┬───────┘
               ↓
          permutation p
               ↓
              ...
               ↓
          permutation p
               ↓
          Final state
```

The important point is that **each message block is XORed into the rate portion before applying the permutation**.

---

# 9. Why XOR?

Suppose a rate portion contains:

```text
101100...
```

and the message block contains:

```text
110010...
```

XOR gives:

```text
101100
XOR
110010
------
011110
```

The result becomes part of the state.

Then the permutation mixes the entire 1600-bit state.

This means the message affects the whole internal state after the permutation.

---

# 10. Squeezing Phase

After all message blocks have been absorbed, SHA3 enters the **squeezing phase**.

For SHA3-256, we need:

$$
256\text{ bits}
$$

The rate is:

$$
1088\text{ bits}
$$

Since:

$$
256<1088
$$

we can obtain the entire 256-bit digest from the first rate portion.

Conceptually:

```text
Final 1600-bit state
        ↓
┌─────────────────────────────┐
│ Rate: 1088 │ Capacity: 512 │
└─────────────────────────────┘
      ↓
take first 256 bits
      ↓
SHA3-256 digest
```

So for SHA3-256:

$$
\boxed{\text{output}=256\text{ bits}}
$$

No second permutation is required for a normal SHA3-256 output because 256 bits fit inside the 1088-bit rate.

For longer outputs, additional permutations can be performed to continue squeezing output.

---

# 11. Complete SHA3-256 Sponge

The entire construction can be represented as:

```text
                 MESSAGE
                    ↓
                  PAD
                    ↓
          P₁     P₂     P₃
           ↓      ↓      ↓
          XOR    XOR    XOR
           ↓      ↓      ↓
        ┌──────┐
        │ 1600 │
        │ bits │
        └──┬───┘
           ↓
        p (24 rounds)
           ↓
        ┌──────┐
        │ 1600 │◄── XOR P₂
        └──┬───┘
           ↓
        p (24 rounds)
           ↓
        ┌──────┐
        │ 1600 │◄── XOR P₃
        └──┬───┘
           ↓
        p (24 rounds)
           ↓
       Final state
           ↓
     ┌───────────────┐
     │ Rate 1088 bit │
     └───────┬───────┘
             ↓
       take 256 bits
             ↓
       SHA3-256 digest
```

---

# 12. SHA-256 vs SHA3-256

| Feature             | SHA-256                             | SHA3-256                           |
| ------------------- | ----------------------------------- | ---------------------------------- |
| Construction        | Compression function                | Sponge                             |
| Basic state         | 256 bits                            | 1600 bits                          |
| Message block/rate  | 512 bits                            | 1088-bit rate                      |
| Capacity            | Not defined as sponge parameter     | 512 bits                           |
| Initial state       | Fixed IV                            | 1600-bit state initialized to zero |
| Main transformation | Compression function                | Keccak-f[1600] permutation         |
| Rounds              | 64 rounds per block                 | 24 rounds per permutation          |
| Output              | 256 bits                            | 256 bits                           |
| Message mixing      | Compression function                | XOR into rate + permutation        |
| Padding             | Merkle-Damgård-style length padding | Sponge multi-rate padding          |
| Internal variables  | `a,b,c,d,e,f,g,h`                   | 1600-bit Keccak state              |
| Final phase         | Final chaining value                | Squeezing                          |

---

# 13. Main Difference

The biggest conceptual difference is:

### SHA-256

The message is processed **block by block**, and every block updates a **256-bit chaining value**.

$$
H_i=C(H_{i-1},M_i)
$$

```text
H₀ → C + M₁ → H₁ → C + M₂ → H₂ → ... → Hₜ
```

### SHA3-256

The message is **absorbed into a large 1600-bit state**, and the permutation repeatedly mixes the state.

$$
S_i=p(S_{i-1}\oplus P_i)
$$

Then the required number of output bits are **squeezed out**.

```text
Message
   ↓
Absorb
   ↓
1600-bit state
   ↓
Permutation
   ↓
1600-bit state
   ↓
Squeeze
   ↓
256-bit digest
```

---

# 14. Construction Calculation Summary

For **SHA-256**:

$$
512\text{-bit block}
\rightarrow
16\times32\text{-bit words}
\rightarrow
64\text{ words}
\rightarrow
64\text{ rounds}
\rightarrow
256\text{-bit chaining value}
$$

For **SHA3-256**:

$$
1600=r+c
$$

$$
c=2\times256=512
$$

$$
r=1600-512=1088
$$

Therefore:

$$
\boxed{r=1088\text{ bits}=136\text{ bytes}}
$$

$$
\boxed{c=512\text{ bits}=64\text{ bytes}}
$$

Then:

$$
\text{Absorb 1088-bit blocks}
\rightarrow
\text{XOR into rate}
\rightarrow
p=Keccak\text{-}f[1600]
\rightarrow
\text{Squeeze 256 bits}
$$

---

## Final Concept

```text
SHA-256

Message
   ↓
512-bit blocks
   ↓
Compression function
   ↓
256-bit chaining value
   ↓
256-bit digest
```

```text
SHA3-256

Message
   ↓
1088-bit rate blocks
   ↓
XOR into 1600-bit state
   ↓
Keccak-f[1600]
   ↓
Absorb all blocks
   ↓
Squeeze
   ↓
256-bit digest
```

**In one sentence:** SHA-256 repeatedly **compresses a message block together with a 256-bit chaining value**, while SHA3-256 **absorbs message blocks into the rate portion of a 1600-bit state and uses a permutation to mix the state before squeezing out the hash.**
