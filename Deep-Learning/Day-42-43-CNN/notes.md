# Convolutional Neural Networks (CNN)

a normal neural net flattens the image into one long vector and loses all the 2d structure.
a CNN keeps the structure by sliding small filters over the image. thats the whole idea.

## Convolution

- a **kernel** (filter) is a small grid of numbers
- put it on a patch of the image, multiply elementwise, sum, thats one output pixel
- slide it across the whole image and you get a **feature map**
- output is smaller than the input (`h - k + 1`) unless you pad
- diffrent kernels detect diffrent things, sobel_x finds vertical edges, sobel_y finds horizontal ones

## The full CNN pipeline

1. **convolution** : slide learned filters, make feature maps
2. **relu** : clip negatives to zero
3. **max pooling** : take the max in each 2x2 block, shrinks the map and adds a little shift tolerance
4. **flatten** : stack the maps into one vector
5. **dense + softmax** : normal layer to the 10 classes, softmax for probabilities

did this whole thing by hand on one digit with random weights just to see every step. the
prediction was a guess becuase nothing was trained yet.

## Why CNN over a plain dense net

- flattening throws away which pixels are neighbours, conv keeps locality
- the same small kernel slides everywhere, so way fewer weights and it can find a feature
  anywhere in the image, not just where it saw it in training

## Training one (pytorch)

- built it with `nn.Sequential` : Conv2d, ReLU, MaxPool2d, Flatten, Linear
- trained on the digits dataset (8x8) with Adam + cross entropy
- **CNN test acc ~97.8%**, plain MLP baseline ~97.1%, so on tiny 8x8 they are close
- the cool part, the conv layer **learned its own filters** from the data, no sobel by hand
- on real 28x28 mnist the CNN advantage is much bigger, but that needs a download wich the
  sandbox cant do, so digits is the offline stand in

## Notebooks

1. convolution intuition, kernels and edge detection on a real image (`01`)
2. full forward pass from scratch on one digit, conv relu pool flatten dense softmax (`02`)
3. a real trainable CNN in pytorch, compared to a dense net, and its learned filters (`03`)
