# 5. Chaining Values

Let the padded message be:

```text
M = M₁ || M₂ || ... || Mₜ
```

where:

* `M₁, M₂, ..., Mₜ` are the message blocks
* `||` means concatenation
* `t` is the total number of blocks

For example, if the algorithm uses 512-bit blocks:

```text
M₁ = 512 bits
M₂ = 512 bits
...
Mₜ = 512 bits
```

The hash function processes these blocks **sequentially**.

---

# 1. H⁽⁰⁾ — the initial value

Before processing the first message block, the algorithm needs an initial internal state.

This is the **Initialization Vector (IV)**:

```text
H⁽⁰⁾ = IV
```

The superscript `0` means:

> **Zero message blocks have been processed.**

For example, SHA-256 starts with an internal state of:

```text
8 × 32 bits = 256 bits
```

So:

```text
H⁽⁰⁾ =

6a09e667
bb67ae85
3c6ef372
a54ff53a
510e527f
9b05688c
1f83d9ab
5be0cd19
```

---

# 2. Processing the first block

The first message block is:

```text
M₁
```

The compression function takes:

```text
H⁽⁰⁾
M₁
```

as input:

```text
H⁽¹⁾ = f(H⁽⁰⁾, M₁)
```

The result is the new chaining value:

```text
H⁽¹⁾
```

So:

```text
H⁽⁰⁾ + M₁
       ↓
   compression
       ↓
     H⁽¹⁾
```

The important point is:

> **H⁽¹⁾ contains the accumulated internal state after processing the first block.**

---

# 3. Processing the second block

Now the algorithm does **not start again from the IV**.

Instead, it uses the result from the previous step:

```text
H⁽²⁾ = f(H⁽¹⁾, M₂)
```

So:

```text
H⁽¹⁾ + M₂
       ↓
   compression
       ↓
     H⁽²⁾
```

This is why it is called a **chaining value**.

The states are chained together:

```text
H⁽⁰⁾ → H⁽¹⁾ → H⁽²⁾ → H⁽³⁾ → ...
```

---

# 4. General iteration

For every block `Mᵢ`:

```text
H⁽ⁱ⁾ = f(H⁽ⁱ⁻¹⁾, Mᵢ)
```

This means:

```text
new state
    =
compression(
    previous state,
    current message block
)
```

For a message containing `t` blocks:

```text
H⁽⁰⁾ = IV

H⁽¹⁾ = f(H⁽⁰⁾, M₁)

H⁽²⁾ = f(H⁽¹⁾, M₂)

H⁽³⁾ = f(H⁽²⁾, M₃)

...

H⁽ᵗ⁾ = f(H⁽ᵗ⁻¹⁾, Mₜ)
```

---

# 5. Simple chaining diagram

```text
                         H⁽⁰⁾ = IV
                             │
                             │
                            M₁
                             │
                             ▼
                     ┌─────────────┐
                     │ Compression │
                     │      f      │
                     └─────────────┘
                             │
                             ▼
                           H⁽¹⁾
                             │
                            M₂
                             │
                             ▼
                     ┌─────────────┐
                     │ Compression │
                     │      f      │
                     └─────────────┘
                             │
                             ▼
                           H⁽²⁾
                             │
                            M₃
                             │
                             ▼
                     ┌─────────────┐
                     │ Compression │
                     │      f      │
                     └─────────────┘
                             │
                             ▼
                           H⁽³⁾
                             │
                            ...
                             │
                            Mₜ
                             │
                             ▼
                     ┌─────────────┐
                     │ Compression │
                     │      f      │
                     └─────────────┘
                             │
                             ▼
                           H⁽ᵗ⁾
                             │
                             ▼
                         DIGEST
```

The key visual idea is:

```text
H⁽⁰⁾ → H⁽¹⁾ → H⁽²⁾ → H⁽³⁾ → ... → H⁽ᵗ⁾
         ↑       ↑       ↑              ↑
        M₁      M₂      M₃             Mₜ
```

---

# 6. What does the compression function do?

The **compression function** is the core operation that mixes:

1. the previous internal state, and
2. the current message block.

We can represent it as:

```text
             Previous state
                  │
                  ▼
              ┌───────┐
Message ────→ │   f   │
block         └───────┘
                  │
                  ▼
              New state
```

Mathematically:

```text
H⁽ⁱ⁾ = f(H⁽ⁱ⁻¹⁾, Mᵢ)
```

The exact operations inside `f` depend on the hash function.

For example, SHA-256's compression function performs **64 rounds** of operations involving:

* addition modulo `2³²`
* XOR
* bit rotations
* bit shifts
* logical functions
* round constants
* the message schedule

The purpose is to create strong **mixing and diffusion**.

A small change in the message should eventually cause a very different internal state.

---

# 7. Why is it called "compression"?

It is called a **compression function** because it takes a relatively large amount of input and produces a fixed-size state.

For example, SHA-256 uses:

```text
Previous state = 256 bits
Message block  = 512 bits
```

Together:

```text
256 + 512 = 768 bits
```

The compression function produces:

```text
256 bits
```

So conceptually:

```text
768 bits
    ↓
compression function
    ↓
256 bits
```

The state size stays fixed after every block.

```text
H⁽⁰⁾ = 256 bits
H⁽¹⁾ = 256 bits
H⁽²⁾ = 256 bits
H⁽³⁾ = 256 bits
...
```

This allows the algorithm to process messages of arbitrary length without the internal state continuously growing.

---

# 8. How is the final digest obtained?

Suppose the padded message contains `t` blocks:

```text
M₁ || M₂ || ... || Mₜ
```

The algorithm performs:

```text
H⁽⁰⁾ = IV

H⁽¹⁾ = f(H⁽⁰⁾, M₁)

H⁽²⁾ = f(H⁽¹⁾, M₂)

...

H⁽ᵗ⁾ = f(H⁽ᵗ⁻¹⁾, Mₜ)
```

After the final block:

```text
H⁽ᵗ⁾
```

is the final internal state.

For MD5, SHA-1 and SHA-256, this final state corresponds directly to the digest, with the words serialized according to the algorithm's specified byte order.

Therefore:

```text
DIGEST = H⁽ᵗ⁾
```

For example, SHA-256 has:

```text
H⁽ᵗ⁾ = 8 × 32-bit words
     = 256 bits
     = 32 bytes
     = 64 hexadecimal characters
```

---

# 9. Complete example with three blocks

Suppose the padded message is:

```text
M = M₁ || M₂ || M₃
```

Start:

```text
H⁽⁰⁾ = IV
```

Process the first block:

```text
H⁽¹⁾ = f(H⁽⁰⁾, M₁)
```

Process the second:

```text
H⁽²⁾ = f(H⁽¹⁾, M₂)
```

Process the third:

```text
H⁽³⁾ = f(H⁽²⁾, M₃)
```

Therefore:

```text
                 M₁              M₂              M₃
                  ↓               ↓               ↓
               ┌─────┐         ┌─────┐         ┌─────┐
IV → H⁽⁰⁾ ───→ │  f  │ ───→ H⁽¹⁾│  f  │ ───→ H⁽²⁾│  f  │ ───→ H⁽³⁾
               └─────┘         └─────┘         └─────┘
                                                         │
                                                         ▼
                                                       DIGEST
```

So the final digest depends on **every block**, because every block changes the chaining value that is passed to the next block.

---

# 10. Why the chaining matters

Imagine:

```text
M = M₁ || M₂ || M₃
```

If we change even a small part of `M₁`:

```text
M₁ → M₁'
```

then:

```text
H⁽¹⁾ changes
```

which means:

```text
H⁽²⁾ = f(H⁽¹⁾, M₂)
```

also changes.

Then:

```text
H⁽³⁾ = f(H⁽²⁾, M₃)
```

also changes.

Therefore the change propagates through the entire chain:

```text
M₁ changes
   ↓
H⁽¹⁾ changes
   ↓
H⁽²⁾ changes
   ↓
H⁽³⁾ changes
   ↓
Final digest changes
```

This contributes to the **avalanche effect** expected from a cryptographic hash function.

---

# Key takeaway

The whole construction can be summarized as:

```text
H⁽⁰⁾ = IV

H⁽¹⁾ = f(H⁽⁰⁾, M₁)
H⁽²⁾ = f(H⁽¹⁾, M₂)
H⁽³⁾ = f(H⁽²⁾, M₃)
...
H⁽ᵗ⁾ = f(H⁽ᵗ⁻¹⁾, Mₜ)

DIGEST = H⁽ᵗ⁾
```

In simple words:

> **The IV gives the hash its starting state. Each compression step takes the previous state and the next message block, mixes them, and produces a new fixed-size state. That new state becomes the input for the next block. After the final block, the final chaining value \(H^{(t)}\) becomes the message digest.**

```text
        IV
         ↓
       H⁽⁰⁾
         ↓ + M₁
         ↓
       H⁽¹⁾
         ↓ + M₂
         ↓
       H⁽²⁾
         ↓ + M₃
         ↓
        ...
         ↓ + Mₜ
         ↓
       H⁽ᵗ⁾
         ↓
      DIGEST
```
