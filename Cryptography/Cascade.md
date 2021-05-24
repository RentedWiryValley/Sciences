## Cascade encryption

#### Introduction

One of the principles of cascade encryption is that there is no identifiable information in the outputs of any encryption layer, except the first.

Common identifiable information includes:

- Magic number
- Padding
- Checksum
- Authentication

There is likely more. Even randomness bias may be indicative.

The principle is true no matter how the keys are produced. However, the impacts are different when it is violated.

- If the keys are independent, the revelation of any identifiable information does not compromise the encryption as a whole. It just confirms that the outer layers are decrypted successfully and only the inner layers are to be decrypted.
- If the keys are dependent, i.e., when they are derived from the same key, the revelation of any identifiable information can compromise the encryption as a whole. It is because it confirms the correctness of the decryption keys of the outer layers, and subsequently, the key derivation, which is most likely weaker than the encryption algorithms, becomes the target.

#### Miscellaneous information

The cascade encryption in SoA (cascade.c/.h) was designed as follows:

- AES with PKCS padding was the first layer.
- ChaCha20 without Poly1035 authentication was the second layer.
- All the other layers did not come with any identifiable information.

The design was based on two facts:

- PKCS padding in AES is unavoidable.
- Poly1305 authentication in ChaCha20 is avoidable.

Although Poly1035 was included in the ChaCha20 implementation used, it was intentionally avoided.
