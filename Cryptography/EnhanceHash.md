## A generic approach to enhancing security of hash functions

#### Introduction

The following generic approach should be able to fix (partially) broken hash functions, such as MD5 and SHA2, and may be able to turn non-cryptographically secure ones into secure ones.

The basic form of the approach is as follows:

```
H' = H(f₁(m)) ⊕ H(f₂(m))
```
, where `H'` is the new hash function; `H` is the original hash function; `f₁` and `f₂` are different functions; `m` is the message.

Some options of `f₁` and `f₂` include

- `f(m) = m`
- `f(m) = m | k`
- `f(m) = k | m`
- `f(m) = k₁ | m | k₂`
- Alteration, such as bitwise circular shift, of the first few bytes of `m`
- Alteration, such as bitwise circular shift, of the last few bytes of `m`
- Prepending `m` with the first few bytes of `m`
- Appending `m` with the last or the first few bytes of `m`
- Combinations of the aforementioned options

, where `k`, `k₁` and `k₂` are salts.

The only requirement for the approach to work is that the hash function is self-independent. Self-independence refers to a property of an algorithm of which the outputs are independent as long as any bit in the inputs, including the parameters, is different. At this stage, it is assumed that a good avalanche effect provides good self-independence. Whether they are equivalent is to be studied.

#### Miscellaneous information

The approach is a product of the realization of the *theorem of entropy combination* and *conjecture of randomness combination*, which suggest the entropy/randomness of a sequence can be improved by XORing it with an independent sequence with any non-zero level of entropy/randomness.
