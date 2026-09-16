# Variational Autoencoder (VAE)

the Day 47 autoencoder rebuilds well but cannot invent. a VAE changes the **shape of the latent
space** so that sampling works, the encoder and decoder stay basicaly the same.

## Why the plain autoencoder fails

trained a normal AE with a 2d code and looked at where the codes landed:

- the scale is arbitrary, mine ran from -8 to +24 on one axis. so to sample i first have to go
  and inspect the trained model to find the range. another run gives another range
- about 25% of the space has no training image anywhere near it, decoding there gives blobs
- no control, my 8 random samples came out as four 9s, a 6 and a 1

so it is not that generation is impossible, it is that there is no scale to sample from, no
coverage guarantee and no variety.

## The two changes

1. the encoder outputs **mu and logvar** instead of a point, and training samples from that
   cloud. so the decoder must give a sensible digit for a whole neighbourhood, wich fills the gaps
2. a **KL term** pulls every cloud toward `N(0,1)`, so the codes end up at a known scale

```
z = mu + exp(0.5*logvar) * eps        eps ~ N(0,1)      the reparameterization trick
KL = -0.5 * sum(1 + logvar - mu^2 - exp(logvar))
```

the trick matters because you cannot backprop through a random draw. moving the randomness into
`eps` leaves mu and sigma reachable by plain multiply and add.

i checked the KL formula before training: 0 when mu=0 sigma=1, 4.5 when mu=3, 1.8 when
sigma=0.1. so it punishes drifting away and collapsing, both.

## Results

trained on 10000 mnist images, latent 16, 60 epochs, a few seconds on cpu.

- final recon 86.5, kl 22.7
- codes came out mean 0.009, std 1.04, so the space really is standard normal
- sampling `z ~ N(0,1)` gives varied digits, no model inspection needed
- KL **rises** early then flattens, the model only spends KL budget when reconstruction pays for it

samples are blurry, and thats a real VAE property not a bug. the per pixel loss makes the model
average the options it cannot decide between. GANs attack exactly this.

## Latent space

- interpolating between two digits stays on real digits the whole way
- the 18x18 decoded grid of the 2d latent is the picture worth keeping, digit regions are
  connected and lookalikes are neighbours (4s next to 9s, 3s next to 8s), with no labels ever used
- average code per digit decodes to a clean prototype
- `mean(1) - mean(7)` as a direction turned both a 7 and a 9 into a 1, better than i expected,
  though the 9 passed through a blob at half strength

## beta

| beta | recon | kl |
|---|---|---|
| 0.5 | 82.4 | 29.0 |
| 1.0 | 87.2 | 22.1 |
| 4.0 | 124.0 | 8.3 |

exactly the tradeoff you would predict, tidier space costs reconstruction.

## What I did in this folder

1. plain autoencoder on mnist and why its latent space cannot be sampled (`01`)
2. the VAE written out, reparameterization, KL by hand, sampling (`02`)
3. interpolation, the full 2d latent map, code arithmetic, the beta knob (`03`)

needs torch. mnist comes from `fetch_openml`, downloads once into `~/scikit_learn_data` then
works offline.
