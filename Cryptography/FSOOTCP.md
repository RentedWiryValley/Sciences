## Finite-State (Open) One-Time Combined Pad

#### Introduction

A One-Time Combined Pad (OTCP) is a One-Time Pad (OTP) that's constructed by the combination (XOR) of multiple OTPs. Like OTP, OTCP has perfect secrecy if the pads are kept secret (and the other conditions are met).

Finite-State One-Time Combined Pad (FSOTCP) and Finite-State Open One-Time Combined Pad (FSOOTCP) are a construction of OTCPs that cannot practically be reestablished by an adversary even if the raw OTPs are not kept secret.

The basic form of FSOTCP/FSOOTCP is as follows:

1. Generate `N` OTPs.
2. Index the pads, i.e., `0`, `1`, ..., `N-1`.
3. Determine the target entropy (`T`), e.g, `128`, `256`, and so on.
4. Find the minimum number of pads needed (`K`), which is the minimum number that satisfies `log₂(N! / K! / (N - K)!) ≥ T`.
5. Generate `K` unique random numbers that range from `0` to `N-1` inclusive.
6. Select the pads of which the indexes are equal to the unique random numbers.
7. Combine (XOR) all selected pads to get the final OTP.

\* The `K` unique random numbers are the secret key.

FSOTCP is when the raw OTPs are kept secret. FSOTCP has perfect secrecy.

FSOOTCP is when the raw OTPs are disclosed. FSOOTCP has truly provable security.

The secret key can be reused with new raw OTPs in both FSOTCP and FSOOTCP.

Since the secret key can be reused and the pads can be shared publicly in FSOOTCP, it can be said that FSOOTCP has solved the key distribution problem of OTP, at the cost of perfect secrecy.

The resulting pad of an FSOTCP/FSOOTCP is random, not pseudorandom.

#### filexor

The project filexor in SoA is an implementation of FSOTCP/FSOOTCP in a slightly tweaked form. It performs a buffer-wide 
bitwise circular shift on each selected pad with the number of bits shifted being the index of the pad within the selection. The shift has two immediate effects:

1. It makes the order matter, which effectively switches the selection from combination to permutation.
2. It makes reused pads not cancel each other out, of which the complication is to be studied.

When a pad is reused in filexor, it is XORed by a bitwise-shifted version of itself. It is uncertain at this stage whether there is any loss of entropy/randomness. Two extreme possibilities are as follows:

- The loss of entropy/randomness is disastrous. If this is the case, pads cannot be reused. The overall result is permutation without repetitions. Subsequently, the minimum number of pads needed must satisfy `log₂(N! / (N - K)!) ≥ T`.
- There is no loss of entropy/randomness. If this is the case, pads can be reused, which effectively switches the selection from permutation without repetitions to permutation with repetitions. Subsequently, the minimum number of pads needed can be calculated as follows: `K = Ceiling(T / n)`, where `n = log₂(N)`.

Either way, the number of pads needed, i.e., the size of the secret key, is reduced.

#### Endless FSOOTCP

Although FSOOTCP has solved the key distribution problem of OTP, sharing files of gigabytes or more, even publicly, is most likely not ideal. A practically endless sequence of random numbers with pads of limited lengths can be created with the following algorithm:

1. Export the resulting pad's numbers as usual up till the size of the remaining numbers at the end of the pad is the same as or close to the size of the secret key.
2. Use the remaining numbers as the new secret key on the same pads to form a new FSOOTCP.
3. Repeat 1 and 2.

Similar to filexor, a tweak can be introduced to reduce the size of the secret key. Besides buffer-wide bitwise circular shift, there are other options, such as integer-wide bitwise circular shift and byte-level shift based on the index of the pad within the selection.

Although it is clear that the resulting pad of the first round of an Endless FSOOTCP is random, whether those of the remaining rounds are random or pseudorandom is to be decided. Regardless, an Endless FSOOTCP can withstand state compromise extensions.
