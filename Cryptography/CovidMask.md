## A homemade mask/helmet design for COVID-19

This article may seem irrelevant to cryptography at first glance. It was actually a by-product of my cryptography research.

The idea was conceived at the early stage of the COVID-19 pandemic, when there was a shortage of masks, also when I was finalizing my understanding of entropy. Although it was designed for COVID-19, it should apply to any infectious diseases that are transmitted by respiratory droplets/aerosols.

#### How to make a medical degree mask/helmet with home-available materials

1. Use a cover of an air-tight material, such as plastic, on the nose/mouth area or the entire head. Seal it to the skin.
2. Plug a (long) tube of an air-tight material into the cover. Seal the connection.
3. Fill the tube with home-available nontoxic filtering materials, such as cotton.

#### Math

It is intuitive that thicker filtering materials have better filtering effects. Nonetheless, how can we be sure the approach can achieve medical degree effects?

No matter how inefficient a filtering material is, there is a chance for it to absorb a droplet/aerosol that is passing through. For a droplet/aerosol to pass the entirety of the material, it needs to beat the chance in every section.

Let's assume, for a given length (`L`) of the material, there is a 50% chance that the droplet/aerosol is absorbed.

In cryptography, 50% probability means 1 bit of entropy.

An N95 mask has a 95% filtration rate, or 5% passing probability. To beat the probability, only 5 bits of entropy is needed because 5 bits of entropy means 1 in 32, or 3.125% probability.

In other words, a homemade mask/helmet better than an N95 mask can be had with a filling length is `L * 5`.

Without any data, `L` of cotton is speculated to be somewhere between 5 mm and 5 cm. It means a tube of somewhere between 25 mm and 25 cm long cotton provides a similar level of protection as an N95 mask.

For comparison:

- To achieve the effectiveness of an N99 mask, which has a 99% filtration rate, or 1% passing rate, 7 bits of entropy is needed because 7 bits of entropy means 1 in 128 or ~0.78% probability. It means a tube of somewhere between 35 mm and 35 cm long cotton can be as effective as an N99 mask.

- To achieve the effectiveness of an N100 mask, which has a 99.97% filtration rate, or 0.03% passing rate, 12 bits of entropy is needed because 12 bits of entropy means 1 in 4096 or ~0.024% probability. It means a tube of somewhere between 60 mm and 60 cm long cotton can be as effective as an N100 mask.

The aforementioned model is apparently simplified. The real world is probably more complicated, e.g., the absorbing probability may vary according to the density of the droplets/aerosols or the level of saturation of the filtering material, and so on. However, the principle of the math can still be applicable, and the design should still work.
