# 3. Padding

Padding makes the message length fit the required block structure of a hash function.

For **MD5, SHA-1, SHA-256 and SHA-512**, the basic idea is:

```text
Original message
      ↓
Append 1 bit
      ↓
Append enough 0 bits
      ↓
Append original message length L
      ↓
Complete block(s)
```

The first four use a **Merkle-Damgård-style construction**, where the final length field is part of the strengthening step.

---

# 1. MD5

MD5 processes data in:

* **512-bit blocks**
* 32-bit words
* 128-bit output

For a message of length **L bits**:

### Step 1 — Append `1`

Append a single `1` bit:

```text
M → M || 1
```

The new length is:

```text
L + 1
```

### Step 2 — Append `0` bits

Append `k` zero bits so that the length becomes **448 mod 512**:

```text
L + 1 + k ≡ 448 (mod 512)
```

Therefore:

```text
k = (448 - (L + 1)) mod 512
```

After this step:

```text
L + 1 + k ≡ 448 (mod 512)
```

### Step 3 — Append the length

MD5 appends the original message length **L** as a:

```text
64-bit integer
```

The length is encoded in **little-endian** order.

Therefore the final length is:

```text
L + 1 + k + 64
```

Since the first part is 448 bits:

```text
448 + 64 = 512 bits
```

So the final message is a multiple of **512 bits**.

### MD5 structure

```text
|     original message     | 1 | 0 ... 0 | 64-bit L |
|<--------- L ------------>|   |          |          |
                           ↑
                         padding

             total = multiple of 512 bits
```

### Example: L = 1000 bits

First:

```text
L = 1000
```

Append `1`:

```text
1000 + 1 = 1001
```

We need:

```text
1001 + k ≡ 448 (mod 512)
```

1001 mod 512:

```text
1001 - 512 = 489
```

We need to reach 448 in the **next** block:

```text
448 + 512 = 960
```

Therefore:

```text
k = 960 - 1001
  = -41
```

That is impossible, so we need another 512-bit block:

```text
448 + 1024 = 1472
```

Therefore:

```text
k = 1472 - 1001
  = 471 zero bits
```

Then append 64 bits for the length:

```text
1000 + 1 + 471 + 64
= 1536 bits
```

And:

```text
1536 / 512 = 3 blocks
```

---

# 2. SHA-1

SHA-1 uses essentially the same padding structure as MD5.

It processes:

```text
512-bit blocks
```

For a message of length **L bits**:

### Step 1 — Append `1`

```text
L → L + 1
```

### Step 2 — Append `k` zero bits

Choose `k` so that:

```text
L + 1 + k ≡ 448 (mod 512)
```

Therefore:

```text
k = (448 - (L + 1)) mod 512
```

### Step 3 — Append the length

Append the original length:

```text
L
```

encoded as a:

```text
64-bit big-endian integer
```

So:

```text
message
+ 1 bit
+ k zero bits
+ 64-bit length
```

The final length is a multiple of:

```text
512 bits
```

### Example: L = 1000

```text
L + 1 = 1001
```

We need to reach 448 modulo 512.

The next suitable value is:

```text
1472
```

Therefore:

```text
k = 1472 - 1001
  = 471
```

Then:

```text
1000 + 1 + 471 + 64
= 1536 bits
```

Thus:

```text
1536 / 512 = 3 blocks
```

---

# 3. SHA-256

SHA-256 uses the same general padding method as SHA-1.

It processes:

```text
512-bit blocks
```

For a message of length **L bits**:

### Step 1 — Append `1`

```text
L + 1
```

### Step 2 — Append zero bits

Choose `k` such that:

```text
L + 1 + k ≡ 448 (mod 512)
```

Therefore:

```text
k = (448 - (L + 1)) mod 512
```

### Step 3 — Append the length

Append:

```text
64-bit representation of L
```

in **big-endian** order.

Therefore:

```text
| message | 1 | 0...0 | 64-bit L |
```

with:

```text
message + padding + length
≡ 0 (mod 512)
```

### Example: L = 1000

```text
L = 1000

L + 1 = 1001
```

Next value satisfying:

```text
x ≡ 448 (mod 512)
```

is:

```text
1472
```

Therefore:

```text
k = 1472 - 1001
  = 471
```

Final length:

```text
1000 + 1 + 471 + 64
= 1536 bits
```

Number of blocks:

```text
1536 / 512
= 3 blocks
```

---

# 4. SHA-512

SHA-512 uses the same idea, but its block size and length field are larger.

SHA-512 processes:

```text
1024-bit blocks
```

For a message of length **L bits**:

### Step 1 — Append `1`

```text
L + 1
```

### Step 2 — Append zero bits

SHA-512 needs the message to reach:

```text
896 mod 1024
```

before the length field.

Therefore:

```text
L + 1 + k ≡ 896 (mod 1024)
```

So:

```text
k = (896 - (L + 1)) mod 1024
```

### Step 3 — Append the length

SHA-512 appends a:

```text
128-bit representation of L
```

in **big-endian** order.

Therefore:

```text
| message | 1 | 0...0 | 128-bit L |
```

The final length is a multiple of:

```text
1024 bits
```

### Example: L = 1000

First:

```text
L + 1 = 1001
```

We need:

```text
1001 + k ≡ 896 (mod 1024)
```

The next suitable value is:

```text
896 + 1024 = 1920
```

Therefore:

```text
k = 1920 - 1001
  = 919
```

Then add the 128-bit length:

```text
1000 + 1 + 919 + 128
= 2048 bits
```

Number of blocks:

```text
2048 / 1024
= 2 blocks
```

---

# 5. Comparison

| Hash        | Block size | `1` bit | Zero padding target | Length field | Length encoding |
| ----------- | ---------: | ------: | ------------------: | -----------: | --------------- |
| **MD5**     |   512 bits |       1 |         448 mod 512 |      64 bits | Little-endian   |
| **SHA-1**   |   512 bits |       1 |         448 mod 512 |      64 bits | Big-endian      |
| **SHA-256** |   512 bits |       1 |         448 mod 512 |      64 bits | Big-endian      |
| **SHA-512** |  1024 bits |       1 |        896 mod 1024 |     128 bits | Big-endian      |

The important calculation is:

### MD5 / SHA-1 / SHA-256

```text
L + 1 + k ≡ 448 (mod 512)

k = (448 - (L + 1)) mod 512

Final length = L + 1 + k + 64
```

### SHA-512

```text
L + 1 + k ≡ 896 (mod 1024)

k = (896 - (L + 1)) mod 1024

Final length = L + 1 + k + 128
```

---

# 6. What happens if L is already near the end of a block?

This is important.

Suppose SHA-256 has:

```text
L ≡ 500 (mod 512)
```

After adding the `1` bit:

```text
500 + 1 = 501
```

There are only:

```text
512 - 501 = 11 bits
```

left in the current block.

But we need:

```text
448 bits
```

before the length field.

So the padding cannot fit into the current block.

Instead, SHA-256 creates another block:

```text
Block 1:
| message | 1 | 0...0 |
                 ↑
          remaining space

Block 2:
| 0...0 | 64-bit L |
```

This is why padding can require **one or two additional blocks**.

---

# 7. SHA-3 Padding

SHA-3 is different.

SHA-3 uses a **sponge construction**, not the Merkle-Damgård-style construction used by MD5, SHA-1 and SHA-2.

Its padding is called:

```text
multi-rate padding
```

or:

```text
pad10*1
```

It can be written conceptually as:

```text
1 0 0 0 ... 0 1
```

The important difference is that SHA-3 does **not append the original message length**.

There is:

```text
NO 64-bit length field
NO 128-bit length field
```

Instead, the padding makes the message fit the sponge's **rate**.

---

# 8. SHA-3 pad10*1 construction

Suppose the remaining space in the current rate block is:

```text
r
```

bits.

SHA-3 adds:

```text
1
```

then enough zeros, and finally:

```text
1
```

so the padding exactly fills the block.

Conceptually:

```text
Message | 1 | 0 | 0 | ... | 0 | 1
         ↑                       ↑
       first 1                 final 1
```

The total padded message becomes a multiple of the **rate**.

For example, SHA3-256 has:

```text
capacity = 512 bits
state    = 1600 bits
```

Therefore:

```text
rate = 1600 - 512
     = 1088 bits
```

So SHA3-256 absorbs the message in:

```text
1088-bit blocks
```

---

# 9. Example of SHA-3 padding

Suppose the message occupies:

```text
1000 bits
```

and we are considering SHA3-256, where:

```text
rate = 1088 bits
```

Remaining space:

```text
1088 - 1000
= 88 bits
```

We need 88 bits of padding.

The padding has the form:

```text
1 000...000 1
```

There are:

```text
88 bits total
```

of padding.

So:

```text
1000-bit message
        +
88-bit pad10*1
        =
1088 bits
```

Exactly one rate block.

---

# 10. Why SHA-3 does not append L

This is the important conceptual difference.

### MD5 / SHA-1 / SHA-256 / SHA-512

They use:

```text
Message
   ↓
Padding
   ↓
Append original length L
   ↓
Fixed-size blocks
   ↓
Compression function
   ↓
Chaining state
```

The length field is part of **Merkle-Damgård strengthening**.

The idea is to make the final message explicitly contain its original length before processing the final blocks.

### SHA-3

SHA-3 instead uses:

```text
Message
   ↓
pad10*1
   ↓
Rate-sized blocks
   ↓
Absorb
   ↓
Keccak permutation
   ↓
Squeeze
   ↓
Hash
```

There is no need to put:

```text
L = original message length
```

into the padded message.

The sponge simply needs the input to be separated into complete **rate-sized blocks**.

---

# 11. Why SHA-3 padding belongs to a sponge construction

The distinction comes from how the underlying constructions work.

### Merkle-Damgård-style

```text
Message
   ↓
Block 1 ──→ Compression
                ↓
              State
                ↓
Block 2 ──→ Compression
                ↓
              State
                ↓
              ...
```

The padding includes:

```text
1 + zeros + message length
```

The length field is therefore part of the strengthening technique.

### Sponge

```text
              ┌───────────────┐
Message ───→  │     State     │
              │    1600 bit   │
              └───────────────┘
                     ↓
                 Permutation
                     ↓
              ┌───────────────┐
              │     State     │
              └───────────────┘
                     ↓
                  Squeeze
```

The sponge has two conceptual parts:

```text
1600-bit state = rate + capacity
```

For SHA3-256:

```text
1600 = 1088 + 512
       ↑       ↑
      rate   capacity
```

The message is XORed into the **rate** portion, and the Keccak permutation mixes the entire state.

Therefore, the padding only needs to ensure that the input fits the rate-sized absorption blocks.

---

# 12. Final comparison

```text
MD5 / SHA-1 / SHA-256 / SHA-512
─────────────────────────────────

Message
   ↓
Append 1
   ↓
Append zeros
   ↓
Append original length L
   ↓
Fixed-size blocks
   ↓
Compression function
   ↓
Hash
```

```text
SHA-3
─────────────────────────────────

Message
   ↓
pad10*1
   ↓
Rate-sized blocks
   ↓
Absorb into sponge
   ↓
Keccak permutation
   ↓
Squeeze
   ↓
Hash
```

## Key takeaway

The crucial difference is:

> **Merkle-Damgård strengthening encodes the original message length \(L\) in the padding, while SHA-3's sponge padding `pad10*1` does not encode \(L\); it simply makes the input fit the sponge's rate-sized blocks.**

### Formulas to remember

**MD5 / SHA-1 / SHA-256:**

```text
L + 1 + k ≡ 448 (mod 512)

Final = L + 1 + k + 64
```

**SHA-512:**

```text
L + 1 + k ≡ 896 (mod 1024)

Final = L + 1 + k + 128
```

**SHA-3:**

```text
Message + pad10*1
```

where the padding fills the remaining space of the **rate**.

For SHA3-256:

```text
State = 1600 bits
Capacity = 512 bits
Rate = 1088 bits
```

So SHA3-256 absorbs:

```text
1088-bit blocks
```

rather than 512-bit blocks like SHA-256.
