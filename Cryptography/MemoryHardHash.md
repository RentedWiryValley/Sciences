## A generic approach to creating memory-hard hash functions

#### Introduction

The basic algorithm of a generic approach to creating a memory-hard hash function out of any existing hash function can be outlined as follows:

```
// calculate and keep intermediate hash values
h₁ = H(x)
h₂ = H(h₁)
h₃ = H(h₂)
...
hₙ = H(hₙ₋₁)

// hash all intermediate hash values in backward direction
h' = H(hₙ | hₙ₋₁ | ... | h₁)
```

, where `h'` is the memory-hard hash output value, `H` is the existing hash function; `x` is the input; `n` is the number of intermediate hashes needed. The total memory size required is `S * n`, where `S` is the output size of `H`.

The approach works because the final step requires all the intermediate hash values to be available in the memory, which in turn is because the memory space of the previous hash values cannot be reused, which in turn is because the previous hash values cannot be discarded, which in turn is because they cannot be calculated from their descendants, which in turn is because hashing is one-way.

#### Exploits

Although the hash values cannot be calculated backward, they can be calculated from the input. Subsequently, it is possible to run the algorithm with a smaller than `S * n` memory space, as long as the hash algorithm supports `Init`/`Update`/`Final` functionality. For example, if `n` is an even number and the memory space is only big enough for `m` hash values, where `m = n / 2`, the memory-hard hash can be calculated as follows:

```
// initialize H
H_Init

// calculate and keep h₁ ~ hₘ
h₁ = H(x)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// calculate and keep hₘ₊₁ ~ hₙ in the memory space that holds h₁ ~ hₘ
h₁ = H(hₘ) // h₁ holds hₘ₊₁
h₂ = H(h₁) // h₂ holds hₘ₊₂
...
hₘ = H(hₘ₋₁) // hₘ holds hₙ

// update H with (hₙ | hₙ₋₁ | ... | hₘ₊₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

// calculate and keep h₁ ~ hₘ in the same memory space
h₁ = H(x)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// update H with (hₘ | hₘ₋₁ | ... | h₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

// finalize H
h' = H_Final
```

, where `H_Init`/`H_Update`/`H_Final` are the `Init`/`Update`/`Final` functions of the hash algorithm; `H` is the standalone function of the hash algorithm that uses a different state than `H_Init`/`H_Update`/`H_Final`.

The same technique can be applied if the memory space is even smaller. In addition, caching some intermediate hash values can greatly reduce the number of hashing. For example, if `n` can be divided by 4 and the memory space is only big enough for `m` hash values, where `m = n / 4`, the number of hashing can be maximally reduced by caching `hₘ` and `h₂ₘ` and applying the aforementioned technique, as shown below:

```
h₁ = H(x)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

c₁ = hₘ // c₁ holds hₘ

h₁ = H(hₘ)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

c₂ = hₘ // c₂ holds h₂ₘ

h₁ = H(hₘ)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁) // hₘ holds h₃ₘ

// initialize H
H_Init

h₁ = H(hₘ) // h₃ₘ is hashed; h₁ holds h₃ₘ₊₁
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// update H with (hₙ | hₙ₋₁ | ... | h₃ₘ₊₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

h₁ = H(c₂) // h₂ₘ is hashed; h₁ holds h₂ₘ₊₁
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// update H with (h₃ₘ | h₃ₘ₋₁ | ... | h₂ₘ₊₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

h₁ = H(c₁) // hₘ is hashed; h₁ holds hₘ₊₁
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// update H with (h₂ₘ | h₂ₘ₋₁ | ... | hₘ₊₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

h₁ = H(x)
h₂ = H(h₁)
...
hₘ = H(hₘ₋₁)

// update H with (hₘ | hₘ₋₁ | ... | h₁)
H_Update(hₘ | hₘ₋₁ | ... | h₁)

// finalize H
h' = H_Final
```

The number of hash values that need caching is `Ceiling(n / m) - 2`, no matter if `n` can be divided by `m`.

Using the exploits may be slower than using virtual memory or saving/loading intermediate hash values to/from the disk. Nonetheless, there may be potential use cases.

#### Countermeasures

Improvements can be made to counter the exploits. The most intuitive solution would be to let `hᵢ = H(hᵢ₋₁ | hᵢ₋₂ | ... | h₁)`, where `3 ≤ i ≤ n`. Since every intermediate hash requires all previous hash values to exist, it is likely the most secure solution possible. The caching exploit technique is still usable. However, it can only reduce the number of hashing by `Ceiling(n / m) - 1`, which is the number of hash values that need caching.

An imperfect alternative would be to let `hᵢ = H(hᵢ₋₁ | hᵣ)`, where `r` is a pseudorandom number made from `hᵢ₋₁` to be between `1` and `i - 2` inclusive. The caching exploit technique remains effective.

Other options that may help include:

- repetition of the intermediate hashing, as shown below:

	```
	hᵢ = hᵢ₋₁
	repeat many times:
		hᵢ = H(hᵢ)
	```
	, where `2 ≤ i ≤ n`.

- repetition of the overall hashing, as shown below:

	```
	H_Init
	repeat many times:
		H_Update(hₙ | hₙ₋₁ | ... | h₁)
	h' = H_Final
	```

#### Generalizations

To make the approach even more generic,

- The intermediate `H` does not have to be the same as the overall `H`.
- The intermediate `H` does not have to be the same (one) function.
- The intermediate `H` does not have to be a hash function. It just has to be a one-way function.

#### Miscellaneous information

Generic memory-hard hash function is implemented in mhh.c/.h in SoA projects. A decision was made to use an anti-cheating method similar to the aforementioned imperfect alternative.
