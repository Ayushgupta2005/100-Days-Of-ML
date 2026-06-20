# Principal Component Analysis (PCA)

## Why PCA?

Real datasets often have many features (dozens or even hundreds). This causes problems:

- We can't visualize anything beyond 3 dimensions.
- Many features are correlated and carry overlapping information.
- More features → more computation and higher risk of overfitting (curse of dimensionality).
- Some features are mostly noise.

PCA reduces the number of features while keeping as much information as possible.

---

## Feature Selection vs Feature Extraction

- **Feature Selection** → keep some original columns, drop the rest.
- **Feature Extraction (PCA)** → create brand new features that are combinations of the original ones.

So a principal component is not a single original feature — it is a mix of all of them.

---

## Key Idea: Variance = Information

- The spread of data along a direction is its variance.
- High variance → a lot of information (points differ a lot along that direction).
- Low variance → little information (points look the same along that direction).

PCA finds the directions of maximum variance and describes the data using those.

---

## The PCA Algorithm (Steps)

1. **Standardize** the data (mean 0, std 1).
2. Compute the **covariance matrix**.
3. Compute **eigenvalues** and **eigenvectors** of the covariance matrix.
4. **Sort** eigenvectors by eigenvalue (largest variance first).
5. Pick the **top k** eigenvectors → these are the principal components.
6. **Project** the data onto these k components.

---

## Important Math

- **Covariance matrix** → how features vary together.
- **Eigenvectors** → the principal component directions.
- **Eigenvalues** → the variance along each component.
- **Explained Variance Ratio** = λᵢ / Σλ → fraction of information kept by each component.
- **Projection** → Z = X_std · W, where W holds the top k eigenvectors.

---

## Why Standardization Matters

PCA is based on variance, so a feature with a large scale (e.g. income in lakhs) would
dominate a feature with a small scale (e.g. age) just because of its units.
Standardizing puts every feature on equal footing before computing the covariance.

---

## Choosing the Number of Components

- Look at the **explained variance ratio** of each component.
- Use the **cumulative** explained variance (e.g. keep enough components for ~95%).
- A **scree plot** (variance vs component number) shows the "elbow" where adding more
  components stops helping much — similar idea to the Elbow Method in K-Means.

---

## Limitations

- PCA is **linear** — it cannot capture curved / non-linear structure.
- Components are combinations of features, so they are **harder to interpret**.
- High variance does **not always** mean important for prediction.

---

## Key Takeaways

- PCA is an **unsupervised** dimensionality reduction technique.
- It creates new features (principal components) ordered by how much variance they capture.
- The first few components usually keep most of the information.
- Always standardize the data first.
- Great for **visualization**, **speeding up models**, and **reducing noise**.

---

## What I Did In This Folder

1. Built the **intuition** behind PCA — variance, principal components, projection (`01`).
2. Implemented **PCA from scratch** as a reusable class and verified it against sklearn (`02`).
3. Applied PCA to the **Iris dataset**, reducing 4 features to 2 and visualizing the
   three species in a single 2D scatter plot (`03`).
