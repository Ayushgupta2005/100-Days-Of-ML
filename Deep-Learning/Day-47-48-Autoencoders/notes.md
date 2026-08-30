# Autoencoders

an autoencoder squeezes the input into a small code and rebuilds the input from it. there is no
label, the target is the input itself, so its unsupervised. the FCN folder already had this
encoder decoder shape, here the point is compression instead of segmentation.

## the shape

```
input  ->  encoder  ->  code (bottleneck)  ->  decoder  ->  output (same size as input)
```

the bottleneck is the whole trick. if the code is smaller than the input then the network
cannot copy, it has to drop the useless stuff and keep only what is needed to rebuild.

## PCA is the linear autoencoder

`fit_transform` is the encoder and `inverse_transform` is the decoder. on the 8x8 digits:

- code 2  : mse 0.052, barely a digit
- code 16 : mse 0.011, almost perfect
- code 64 : mse 0, no compression at all

so 64 pixels of a digit hold much less than 64 numbers of information.

## PCA vs a real autoencoder

| | PCA | neural autoencoder |
|---|---|---|
| mapping | linear only | non linear |
| training | one shot | gradient descent |
| speed | very fast | slow |

with the same 16 size code my numpy autoencoder got 0.0095 against PCA 0.0110, so bending
with a sigmoid does help.

## Denoising

feed a noisy image but keep the clean one as the target. now copying is useless, the only way
to get the loss down is to actually learn what a digit looks like. output error 0.0297 against
0.084 of the noisy input.

## What I did in this folder

1. intuition with PCA as a linear autoencoder, bottleneck size vs reconstruction error (`01`)
2. the full thing in numpy, forward + backprop by hand, 64 to 16 to 64, compared against PCA (`02`)
3. denoising autoencoder in pytorch, tested at 6 diffrent noise levels (`03`)

everything runs offline on sklearn digits, `03` needs torch. no downloads needed.
