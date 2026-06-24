# Singular Value Decomposition (SVD)

## What is it

Any matrix A (any shape, m by n) can be broken into three pieces:

**A = U Σ Vᵀ**

- **U** (m by m) : left singular vectors
- **Σ** : diagonal matrix of singular values, sorted biggest to smallest
- **Vᵀ** (n by n) : right singular vectors

The singular values are the important part. A big one means that direction carries a lot
of the matrix, a tiny one means it barely matters.

---

## Geometric Idea

Any matrix is just a linear transformation, and SVD says that transformation is really
just three simple steps:

1. rotate (Vᵀ)
2. stretch along the axes (Σ)
3. rotate again (U)

So even a messy matrix is secretly just rotate, stretch, rotate.

---

## Low Rank Approximation

This is the part that makes SVD useful for compression.

Each singular value (with its U and V columns) adds one layer of detail to the matrix.
The big singular values carry most of the matrix, the small ones are tiny tweaks. So if
we keep only the top k and drop the rest, we get a close approximation using far fewer
numbers. That is a **rank k approximation**.

---

## Building It From Scratch

SVD comes out of eigen decomposition:

- the eigenvectors of AᵀA are the right singular vectors V
- the eigenvalues of AᵀA are the singular values squared (so take the square root)
- each column of U is then A v / sigma

numpy's `np.linalg.svd` is faster and more stable, but the eigen route shows where it all
comes from. The singular values match numpy, the U and V signs can flip (a direction and
its opposite are the same axis).

---

## Image Compression

An image is just a grid of numbers, so it is a matrix and SVD works straight on it.

- the singular values drop off fast, so the first few hold most of the picture
- keep the top k and the image is slightly blurry but still clearly recognisable
- storage for a rank k version is k (m + n + 1) numbers instead of m n
- the cumulative energy curve (cumsum of singular values squared) is a nice way to pick k

On the sample photo, around k = 50 the image already looks fine while using a small
fraction of the original storage.

---

## Connection to PCA

PCA is basically SVD on the centered data. The right singular vectors are the principal
components and the singular values are tied to how much variance each component explains.
