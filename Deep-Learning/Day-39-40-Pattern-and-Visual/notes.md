# Pattern and Visual Recognition (image basics with OpenCV)

all of this is just cv2 + numpy on one photo (image.jpeg). the whole point is that an image
is only a grid of numbers, so everything is math on those numbers.

## 1. What is an image

- a pixel is a number 0 (black) to 255 (white), these are 8 bit images
- grayscale = one grid (h, w), colour = three grids R G B (h, w, 3), binary = only 0 or 255
- zoomed into an 8x8 patch and printed the actual pixel numbers
- split the colour image into R, G, B channels and into red/green/blue only images

## 2. Image enhancement

**Part A, histogram (contrast):**
- made a low contrast image on purpose, then fixed it
- **histogram equalization** spreads the bunched up intensities across 0..255
- **CLAHE** does it tile by tile so one bright area doesnt wash out the rest

**Part B, removing noise:**
- added gaussian + salt and pepper noise, then cleaned it 4 ways
- **gaussian blur** (smooths everything), **median blur** (best for salt and pepper),
  **bilateral** (keeps edges), **non local means**
- median wins on salt and pepper, bilateral keeps edges sharpest

**Part C, frequency domain (Fourier):**
- FFT moves the image to frequencies, low = smooth areas, high = edges/detail/noise
- **low pass mask** (white circle in the centre) keeps low freq, smooths the image
- **high pass mask** (black circle in the centre) keeps high freq, pulls out edges
- showed every mask + the kept spectrum + the result

## 3. Image segmentation

- **thresholding**: fixed cutoff, **Otsu** (auto cutoff), **adaptive** (local, handles uneven light)
- **edge based**: Canny
- **region based**: Otsu + morphology + distance transform to get solid region cores
- **colour based**: k means on the pixels, posterises into K colour regions

## 4. Image augmentation

- **geometric**: translation, rotation, scaling, shear, flip (h and v)
- **photometric**: brightness, contrast, gamma, noise, blur
- **colour** (done in HSV): hue shift, saturation up/down, value (brightness), channel shuffle
- each one shown before vs after, augmentation grows the dataset and makes a model robust to
  position, lighting and colour changes

## Notebooks

1. what is an image, types, channels, pixels as numbers (`01`)
2. enhancement: histogram, denoising (4 methods), frequency domain low/high pass (`02`)
3. segmentation: threshold, Otsu, adaptive, Canny, region, k means (`03`)
4. augmentation: geometric, photometric, colour, every technique (`04`)
