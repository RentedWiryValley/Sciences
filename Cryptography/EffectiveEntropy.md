## Effective entropy

#### Introduction

One of the uses of a key derivation function (KDF) is key stretching. The purpose of key stretching is to create keys that are hard(er) to brute-force. However, there does not seem to be a quantifiable method to measure the increased security. Effective entropy is an attempt to address the topic.

Effective entropy can be calculated as follows:

```
Eₑ = Eₒ + log₂((Tₖ + Tₜ) / Tₜ)
```

, where `Eₑ` is the effective entropy of the KDF-generated keys when the scheme is brute-forced from the original key or password/passphrase; `Tₖ` is the execution time of the KDF; `Tₜ` is the execution time of the target operation; `Eₒ` is the entropy of the original key or password/passphrase.

The target operation is the algorithm that the generated key is used for. For example, if the KDF-generated key is for AES encryption, the target operation will be AES encryption; if the KDF's output is passed to another KDF, of which the output is used as the key for AES encryption, the target operation will be the second KDF and AES encryption combined.

The increased entropy is `ΔE = log₂((Tₖ + Tₜ) / Tₜ)`.

#### Deduction

Entropy is an approximate indicator of security. Time complexity is the real determinant of security (against brute-forcing). For example, 128-bit entropy, though considered secure, is insecure if the target operation can somehow be executed `2⁶⁴` per second.

If a key has `n` bit of entropy, it effectively means it will take `Tₜ * 2ⁿ` time to brute-force all possibilities with the target operation. Subsequently, effective entropy can be calculated as follows:

```
Eₑ = log₂(Tₐ / Tₜ)
```
, where `Tₐ` is the time it takes to brute-force all possibilities with the target operation.

In the case of a KDF, since the unit execution time is `Tₖ + Tₜ` and the number of all possibilities is `2^Eₒ`, `Tₐ = (Tₖ + Tₜ) * 2^Eₒ`.

```
∵ Eₑ = log₂(Tₐ / Tₜ) and Tₐ = (Tₖ + Tₜ) * 2^Eₒ
∴ Eₑ = log₂((Tₖ + Tₜ) * 2^Eₒ / Tₜ)
∴ Eₑ = log₂(2^Eₒ) + log₂((Tₖ + Tₜ) / Tₜ)
∴ Eₑ = Eₒ + log₂((Tₖ + Tₜ) / Tₜ)
```

#### Discussions

Effective entropy is directly related to some parameters of the KDF, e.g., the number of iterations, because they determine `Tₖ`. Subsequently, as far as effective entropy is concerned, when a KDF is referred to, its core parameters must be included. In addition, the target operation needs to be specified.

`Tₖ` measured on a typical computer can be used as a reference only. It should be estimated based on the threat model, which may need to consider large-scale brute-force operations, [custom hardware attacks](https://en.wikipedia.org/wiki/Custom_hardware_attack), and efficiency improvements by optimization over time.

As the formula suggests, a higher `Tₖ` results in higher effective entropy; a lower `Tₜ` results in higher effective entropy, too.

If an adversary can (secretly) reduce `Tₜ`, the security is weakened because of a shorter verification time, although the effective entropy is higher. Subsequently, they may work on the efficiencies of both the KDF and the target operation.

#### Performance

For effective communication without a specified target operation, the performance of a KDF (`P`) can be defined as

```
P = log₂((Tₖ + Tₒ) / Tₒ)
```
, where `Tₒ` is the execution time of a representative cryptographic operation, such as AES encryption of one block.
