## Theorem of entropy combination

***The entropy of the result of XOR of two independent numbers is no lower than the entropy of either number.***

#### Proof

To prove the original statement, we just need to prove the following one:

*The entropy of the result of XOR of two independent bits is no lower than the entropy of either bit.*

The matrix of all possibilities of XOR operations is as follows:

|Input Bit 1|Input Bit 2|Output Bit|
|-----------|-----------|----------|
|0          |0          |0         |
|0          |1          |1         |
|1          |0          |1         |
|1          |1          |0         |

Let `x` be the probability of input bit 1 being 0, `y` be the probability of input bit 2 being 0, and `z` be the probability of output bit being 0.

According to Shannon's entropy formula, the closer `P(0)` is to 0.5, the higher the entropy will be.

- To prove the entropy of output bit is no lower than the entropy of input bit 1, we just need to prove `|z - 0.5| ≤ |x - 0.5|`.
- To prove the entropy of output bit is no lower than the entropy of input bit 2, we just need to prove `|z - 0.5| ≤ |y - 0.5|`.

According to the matrix, for output bit to be 0, there are two possibilities:

1. Both input bit 1 and input bit 2 are 0.
2. Both input bit 1 and input bit 2 are 1.

If input bit 1 and input bit 2 are independent, the probability of both being 0 is `x * y`; the probability of both being 1 is `(1 - x) * (1 - y)`.

Then total probability `z = x * y + (1 - x) * (1 - y)`, where `0 ≤ x ≤ 1` and `0 ≤ y ≤ 1`.

```
∵ z = xy + (1 - x) (1 - y)
∴ z = xy + 1 - x - y + xy
∴ z = 2xy + 1 - x - y
```

Let `x = X + 0.5`, `y = Y + 0.5` and `z = Z + 0.5`. Then `-0.5 ≤ X ≤ 0.5` and `-0.5 ≤ Y ≤ 0.5`.

```
∵ z = 2xy + 1 - x - y
∴ Z + 0.5 = 2 (X + 0.5) (Y + 0.5) + 1 - (X + 0.5) - (Y + 0.5)
∴ Z + 0.5 = 2XY + X + Y + 0.5 + 1 - X - 0.5 - Y - 0.5
∴ Z + 0.5 = 2XY + 0.5
∴ Z = 2XY
∴ |Z| = |2XY|
∴ |Z| = |2X||Y| and |Z| = |2Y||X|
```

- `|Z| = |2X||Y|`:
	```
	∵ -0.5 ≤ X ≤ 0.5
	∴ 0 ≤ |X| ≤ 0.5
	∴ 0 ≤ |2X| ≤ 1
	∴ |Z| ≤ |Y|
	```
	∴ `|z - 0.5| ≤ |y - 0.5|`

- `|Z| = |2Y||X|`:
	```
	∵ -0.5 ≤ Y ≤ 0.5
	∴ 0 ≤ |Y| ≤ 0.5
	∴ 0 ≤ |2Y| ≤ 1
	∴ |Z| ≤ |X|
	```
	∴ `|z - 0.5| ≤ |x - 0.5|`

At this point, the theorem has been proved.

#### Alternative forms

- *The entropy of any number never decreases when it is XORed with any independent number.*
- *The entropy of any number, if not maximum, always increases when it is XORed with any independent number that has non-zero entropy.*

#### Conjectures of randomness combination

It is fairly intuitive that the statements remain true if "entropy" is replaced by "randomness". However, since randomness is not equivalent to Shannon entropy, according to my understanding, they are conjectures at this stage. The conjectures are as follows:

- *The randomness of the result of XOR of two independent numbers is no lower than the randomness of either number.*
- *The randomness of any number never decreases when it is XORed with any independent number.*
- *The randomness of any number, if not maximum, always increases when it is XORed with any independent number that has non-zero randomness.*

There are two special cases:

- *The result of XOR of a random number and any non-random number is random.*
- *The result of XOR of two random numbers is random.*

Unlike the general forms, the special cases have evidently been proved therefore are not conjectures.

#### Miscellaneous information

XOR is considered a "combination" operation. There may be other combination operations.

The theorems and conjectures play an important role in most of my cryptography, theories and implementations. Cocktail construction is an immediate product of them. Everything applicable is XORed with something else in order to improve entropy/randomness/security. XOR is used in the implementations of FSRNG, CSPRNG with Cocktail construction, PBKDF3/PBKDF3a/PBKDF3b, MHKDF and so on for this purpose.
