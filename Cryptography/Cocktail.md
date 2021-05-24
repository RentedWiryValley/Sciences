## Cocktail construction

#### Introduction

Cocktail construction is a methodology for creating and improving CSPRNGs.

A cocktail construction requires at least two independent Non-Fixed Number Generators (NFNG), including CSPRNGs, Non-Cryptographic PRNGs (NCPRNG), and custom non-random NFNGs. The algorithm is as follows:

```
Rₙ = X1ₙ ⊕ X2ₙ ⊕ ...
```
, where `R` is the output pseudorandom number; `X1` is the number from the first NFNG, `X2` is the number from the second NFNG, and so on; `n` is the index/counter.

It is important that the NFNGs are independent of each other. A dependent NFNG may weaken the security. An independent NFNG always contributes to the security. As long as the NFNGs are independent of each other, the more NFNGs there are, the better security the construction will have.

Cocktail construction enables cryptographers to create CSPRNGs systematically. The unique value of cocktail construction is that even non-cryptographers can work on CSPRNGs and subsequently stream ciphers.

The main use cases of cocktail construction include:

- to construct a (potential) CSPRNG without using cryptographic primitives
- to enhance the security of a (potential) CSPRNG

#### Constructing CSPRNG

The quickest way to cocktail-construct a (potential) CSPRNG with no cryptographic primitives is by XORing two independent NCPRNGs. If two NCPRNGs have distinct internal mechanisms, they can be deemed independent. For example, xoshiro256++ and xoshiro512\*\* can be deemed independent, while xoshiro256++ and xoshiro512++ should be deemed dependent. Thorough research will be ideal.

It's also possible to cocktail-construct a (potential) CSPRNG with

- an NCPRNG and a good independent custom NFNG,
- an NCPRNG and a few average independent custom NFNGs,
- a few good independent custom NFNGs,
- some/many average independent custom NFNGs.

#### Enhancing security

The security of a (potential) CSPRNG can be enhanced when it is combined with an NFNG. Known and unknown weaknesses/vulnerabilities/backdoors can be overcome/mitigated/eliminated.

The quickest way to enhance the security of a (potential) CSPRNG is by combining it with a PRNG, which can be a CSPRNG or NCPRNG. Alternatively, a custom NFNG can be used.

If the (potential) CSPRNG is built with cocktail construction, it is crucial that the security-enhancing NFNG is independent of every component of the construction.

It is noteworthy that even a non-zero fixed number generator, i.e., a non-zero single-value many-time pad, can contribute to the security, albeit minimal.

#### Custom NFNG

This chapter contains a preliminary study on custom NFNGs.

A custom NFNG does not have to be random. It just needs to be variable.

It is suggested that the NFNG is made simple so that its independence from other components can be easily verified. Making the algorithm simple should automatically ensure its independence from PRNGs, especially CSPRNGs, because they tend to involve complicated calculations, although only research can be sure.

To achieve ideal variability, try to make it so that 

- every bit has an as equal as possible chance of changing;
- there are no apparent patterns.

For example, `Xₙ = n * 2` is not ideal because the least significant bit is always 0, and the most significant bits have a pattern of being 0. The following examples are much better, although their qualities may be different:

- `Xₙ = Xₙ₋₂ + Xₙ₋₁`, where `X₀` and `X₁` are random numbers.
- `Xₙ = Xₙ₋₂ + rot(Xₙ₋₁, n)`, where `rot` is bitwise circular shift.
- `Xₙ = (n + 1) * rot(P, n)`, where `P` is a random number.
- `Xₙ = (n + C) * rot(P, n)`, where `C` is a random number.

\* Random numbers are keys.

All arithmetic and bitwise operations can be explored.

Dependent NFNGs may not always be bad. For example, if `Yₙ` is the number from another NFNG,

- `Xₙ = rot(Yₙ + C₁, n) + rot(C₂, Yₙ)`, where `C₁` and `C₂` are random numbers.

will likely work well with `Yₙ`.

An algorithm with a XOR as the finishing operation is effectively two NFNGs.

NFNGs can be the cornerstones of cocktail-constructed CSPRNGs. It deserves extensive research. Ideally, a library of NFNGs is built and grouped (by their dependence) by researchers so that everyone can cocktail-construct their own CSPRNGs by picking one from each selected group.

#### Best practices

At this stage, the cryptographic security of a construction without a CSPRNG can only be validated by professional cryptographers and with extensive studies. If being sure of the security is a must, cocktail-construct one with at least one CSPRNG.

Different keys for different NFNGs are always preferable.

#### State compromise extensions

There are a few ways to make a construction withstand state compromise extensions, which is a requirement of a CSPRNG:

- Regularly replace the internal states of non-cryptographic components with their hashes by a cryptographic hash function.
- Regularly replace the internal states of non-cryptographic components with the pseudorandom numbers generated by the construction and removed from the export queue.

If a PRNG has no "dictability", i.e., predictability and retrodictability, replacing its internal state with the pseudorandom numbers generated by itself and removed from the export queue may be a generic way of making it withstand state compromise extensions.

#### Miscellaneous information

Cocktail construction is an immediate product of the realization of the *theorem of entropy combination* and *conjecture of randomness combination*, which suggest the entropy/randomness of a sequence can be improved by XORing it with an independent sequence with any non-zero level of entropy/randomness.

Cocktail construction plays a pivotal role in SoA projects because it establishes a trusted CSPRNG with guaranteed cryptographic security with no trust in any existing cryptographic primitives.

In SoA projects, a configurable CSPRNG is implemented in pseudorandom.c/.h with cocktail construction. The file names of cocktail.c/.h may be misleading. They contain a stream cipher that utilizes the CSPRNG.

No cryptographic primitive is used in the CSPRNG/cipher in alpha-encrypter. Instead, two NCPRNGs and a custom NFNG are used. It is on purpose. The implementation is intentionally weakened to welcome challengers in order to prove the validity of cocktail construction.
