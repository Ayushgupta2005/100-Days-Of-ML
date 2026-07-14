# Fashion-MNIST with a CNN

one more image dataset, but this time not from scratch, just using pytorch. did the CNN by
hand already in the CNN folder, so here the point is to use the library on harder data.

## The dataset

- Fashion-MNIST, 28x28 grayscale clothes, 10 classes (t-shirt, trouser, pullover, dress,
  coat, sandal, shirt, sneaker, bag, ankle boot)
- 60k train, 10k test
- same shape as mnist digits but harder, the clothes look alike
- loaded from the kaggle csv in the archive folder, so no download (each row is label + 784 pixels)

## The model

built with `nn.Sequential`, no class:

- conv(1, 16) relu maxpool   28 -> 14
- conv(16, 32) relu maxpool  14 -> 7
- flatten
- dense 1568 -> 128 relu -> 10

adam + cross entropy, trained in mini batches of 128 with a DataLoader for 5 epochs.

## Result

- **test accuracy about 90%**
- per class accuracy shows the model is great on trousers, sandals, bags, sneakers, boots
  (the very distinct shapes) and weakest on **shirt (~0.66)** and **pullover (~0.79)**, becuase
  shirts, pullovers and t-shirts all look similiar at 28x28
- the actual vs predicted grid shows most mistakes are exactly those top vs top mixups

## Note on running it

uses pytorch, so re running needs a torch kernel (torch is in the python.org 3.13, not the
anaconda one). the data is fully local so nothing downloads.
