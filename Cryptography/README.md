## My cryptography research findings

#### Introduction

My journey into cryptography, which was mostly centered on securing SoA, began in early 2020. It turned out to be unexpectedly fruitful.

Because of the value of SoA, a unique threat model was established. It included the following assumptions:

- No existing cryptographic primitives, except One-Time Pad, could be fully trusted.
- No physical security of the plaintext, the One-Time Pad, and so on could be guaranteed.

The threat model dictated that custom cryptographic primitives that I could be sure were secure had to be created, and the cipher data of the plaintext would be the commitment.

#### Researches

The following list contains my research topics in chronicle order of discovery/creation, according to my memory:

- [Theorems/conjectures of entropy/randomness combination](EntropyCombination.md)
- [Cocktail construction](Cocktail.md)
- [A generic approach to enhancing security of hash functions](EnhanceHash.md)
- [Finite-State Random Number Generator](FSRNG.md)
- ["Alpha" cipher](AlphaBravo.md)
- [Cascade encryption](Cascade.md)
- ["Bravo" cipher](AlphaBravo.md)
- [Generic memory-hard hash function](MemoryHardHash.md)
- [How to enter sensitive information on a PC with a keylogger or key-logging keyboard](AntiKeylogger.md)
- [Effective entropy](EffectiveEntropy.md)
- [Finite-State (Open) One-Time Combined Pad](FSOOTCP.md)

There are two researches that are unfinished:
- A cryptographic hash function
- A One-Time Pad variant

A research that was marginally related to cryptography was done in the same period:

- [A homemade mask/helmet design for COVID-19](CovidMask.md)

No systematic education in cryptography, including systematic self-learning, such as textbook reading, has been received before or after the endeavor of securing SoA. Sporadic internet research has been the primary means of knowledge acquisition. Even to this day, my knowledge of cryptography is exceedingly limited and superficial. Misunderstandings and lack of understanding should be expected.

All researches and coding were done alone in spare time. None of the papers has been reviewed by anyone at the time of publication. Errors should be expected.

#### Prior arts

Unless explicitly specified, all the core knowledge and most of the accessory knowledge in the papers, except the fundamental, are independently created. It was pre-known that preexisting knowledge would be rediscovered, and some has been identified. Efforts have been made to exclude preexisting knowledge that is either basic or trivial. Known preexisting knowledge that is noteworthy include:

- *Cocktail construction*: Santha and Vazirani's finding, [several bit streams with weak randomness can be combined to produce a higher-quality quasi-random bit stream](https://cryptography.fandom.com/wiki/Cryptographically_secure_pseudorandom_number_generator), may overlap with Cocktail construction.
- *Cascade encryption*: Magic number is listed in the [cascade encryption entry on Wikipedia](https://en.wikipedia.org/wiki/Multiple_encryption).
- *How to enter sensitive information on a PC with a keylogger or key-logging keyboard*: The basics of the idea have been found on the internet.

There is a good chance there is more preexisting knowledge that I'm not aware of at this stage.

#### Credits

- PBKDF3/PBKDF3a/PBKDF3b were inspired by Skein's document.

---

[Other science fields](../README.md)
