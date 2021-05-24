## Finite-State Random Number Generator

#### Introduction

Finite-State Random Number Generator (FSRNG) is a way of generating (nearly) truly random numbers without True Random Number Generator (TRNG) hardware. All that is needed is High-Resolution Time (HRT) hardware.

The algorithm of FSRNG is as follows:

```
let N = Ceiling(T / S)

macro Fill_Pool_With_HRTs:
	Do_Something
	Acquire_HRT
	Add_HRT_To_Pool
    
function Generate_Random_Buffer:
	Clear_Pool
	repeat N times:
		Fill_Pool_With_HRTs
	Map_Pool_To_Random_Buffer

function Get_Random_Number:
	if Random_Buffer_Is_Empty:
		Generate_Random_Buffer
	R = Extract_From_Random_Buffer
	return R
```

, where `T` is the target entropy; `S` is entropy per sample.

`S` is a measurement of the unpredictability of how much time it takes for `Fill_Pool_With_HRTs` and the loop to run. Be conservative in `S`.

`Do_Something` determines `S`. It can potentially be any operation. `Sleep`  with the shortest possible interval is likely a good option. To achieve optimal performance, it is justified to do systematic research on potential operations in the target system. For each operation, measure the average execution time (`t`) and the average entropy (`S`) with the HRTs in the pool. The goal is to find the operation that has the highest `S / t`. Faster operations may yield better results as long as the entropy is measurable, but only experiments can tell. Some candidates of `Do_Something` include:

- Not having `Do_Something`
- `nop` and other operations within CPU
- Memory operations
- Disk operations
- Operations involving other hardware
- `Sleep` with different intervals

Random number services, such as /dev/random, can also fill the pool in interrupt handlers.

`Map_Pool_To_Random_Buffer` can be done with a cryptographic hash function that has the output size (in bits) of `T` or larger. The hash function effectively compresses the pool, which has a larger size and lower entropy density, thus can be considered an entropy condenser.

#### Discussions

The numbers that an FSRNG generates are (nearly) truly random because:
- All samples are truly random (within their entropy range).
- The total entropy of all samples exceeds the target entropy.
- The hash function merely compresses the samples and loads every bit of the output numbers with maximal entropy.

The hash function may have to be "perfect", of which the definition is to be studied, for FSRNG to be fully (not nearly) truly random. Modern cryptographic hash functions, such as SHA series, Blake series, and Skein, are likely nearly perfect. It is speculated that the combination (XOR) of hash values by more than one hash function can improve on perfection.

Although FSRNG may not be strictly perfect, it should be good enough to be considered a truly random number generator.

FSRNG can be used when there is no hardware TRNG or when the hardware TRNG cannot be trusted. If the quality of FSRNG is a concern and a hardware TRNG is present, XOR the outputs of FSRNG with the outputs from the hardware TRNG.

#### Improvements

There are some weaknesses in the original design:

- An adversary can brute-force the coming random numbers at any point without the success of brute-forcing previous internal states.
- The random numbers in some rounds may not have maximal entropy if the short-term average entropy of the samples is lower than `S` for some reasons.

Both weaknesses can be overcome with the following change:

Before the pool is cleared, calculate the hash value of the pool with a different cryptographic hash function and add it to the pool after it is cleared.

#### Miscellaneous information

The inspiration that led to FSRNG was an example of insecure code that utilized Unix timestamps. It was realized that Unix timestamps were insecure only because the entropy they carry was low, and the use of Unix timestamps could be made secure if the entropy, or the timestamps, could be accumulated. The precursor idea of FSRNG used Unix timestamps (with human-induced long delays).

FSRNG gets its name because every input has a finite number of states, which can theoretically be brute-forced to get the output.

FSRNG is used in some projects about SoA (random.c/.h).
