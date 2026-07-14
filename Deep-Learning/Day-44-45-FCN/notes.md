# Fully Convolutional Networks (FCN)

a normal CNN gives one label for the whole image (classification). an FCN gives a label for
every pixel (segmentation). the trick is it has no dense layer, its all convolutions, and it
upsamples back to the original size.

## CNN vs FCN

- **CNN** : conv + pooling, then flatten + dense, output is one class for the image
- **FCN** : conv + pooling, then upsample back up, output is a full size map, one label per pixel

so an FCN is basicaly a CNN with the dense head removed and an upsampling path added.

## The shape of an FCN

1. **encoder** : conv + pooling, pulls out features and shrinks the image
2. **bottleneck** : a conv in the middle on the smallest feature map
3. **decoder** : upsampling + conv, grows it back to the input size
4. **output** : a 1x1 conv that turns each pixels features into its label (sigmoid for digit vs background)

pooling shrinks the image on the way down, so upsampling is needed to get a full size mask
back out. upsampling here is nearest neighbour, just repeat each pixel into a block.

## The MNIST segmentation task

the label is a mask, `mask = (image > 0)`, so every pixel is either digit or background.
the FCN learns to color in the digit shape.

## What I did in this folder

1. FCN intuition, one label per image vs one label per pixel, and the encoder decoder idea (`01`)
2. the full thing on mnist (`02`), in three stages:
   - a manual forward pass from scratch (conv, relu, pool, conv, upsample, threshold to a mask)
   - a trainable FCN from scratch (Conv + a pixel classifier, pixel wise BCE loss, training loop)
   - the same thing built with keras (encoder, bottleneck, decoder, 1x1 sigmoid output), trained on mnist

note: `02` uses keras/tensorflow and was run with mnist downloaded, so re running it needs
tensorflow (it lives in the python.org 3.13, not the anaconda kernel). the saved outputs are
all there. `01` is offline and runs on any kernel with numpy + sklearn.
