## "Alpha" cipher and "Bravo" cipher

#### Notations

Alpha cipher and Bravo cipher use pseudorandom numbers, like stream ciphers. If we consider One-Time Pad the "original form" of stream ciphers, there exist the original forms of Alpha cipher and Bravo cipher, which, like One-Time Pad, use truly random numbers instead of pseudorandom numbers.

#### Claims

- Both Alpha cipher and Bravo cipher are novel.
- Neither Alpha cipher nor Bravo cipher is based on One-Time Pad.
- Alpha cipher and Bravo cipher use distinct mechanisms.
- Both Alpha cipher and Bravo cipher are secure if the provided pseudorandom number source is secure.
- Both Alpha cipher and Bravo cipher may have truly provable security if the provided pseudorandom number source is secure.
- The original forms of both Alpha cipher and Bravo cipher may have perfect secrecy.

The first claim is more of a speculative deduction due to the lack of knowledge in cryptography.

The claims that both ciphers are secure are made with the awareness of Schneier's law and with no reviews. It is about the theories/algorithms/mechanisms, not the implementations.

The raw mode of Alpha cipher can be insecure in some cases. Besides the raw mode, two secure modes have been developed. The claims about provable security and perfect secrecy apply to the two secure modes only.

#### Source code

Both Alpha cipher and Bravo cipher are used in SoA projects.

The source code of Alpha cipher is zipped and then encrypted with alpha-encrypter. The cipher data file is alpha.7z.cipher. The source code of alpha-encrypter is disclosed.

The source code of Bravo cipher is zipped and then encrypted with bravo-encrypter. The cipher data file is bravo.7z.cipher. The source code of bravo-encrypter is disclosed, except the Alpha cipher part.

The decryption of Alpha cipher and Bravo cipher has been scheduled. In the meanwhile, challengers are welcome.
