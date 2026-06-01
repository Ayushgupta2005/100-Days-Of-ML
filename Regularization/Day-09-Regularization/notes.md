# Regularization

## Why Regularization?

While working with Polynomial Regression, I noticed that increasing the degree creates more features and makes the model more flexible. This helps fit non-linear data, but it can also lead to overfitting.

Regularization is used to control model complexity and prevent overfitting.

---

## Dataset Used

I generated a synthetic non-linear dataset:

python y = 0.5*x³ - 4*x² + 10*x + noise 

The noise was added to simulate real-world data.

---

## Why Polynomial Features?

Initially the dataset had only one feature:

text x 

A normal linear regression model can only learn:

text y = w0 + w1*x 

which is a straight line.

To capture the curve in the data, I used:

python PolynomialFeatures(degree=10) 

This transformed the single feature into:

text 1, x, x², x³, x⁴, ..., x¹⁰ 

Now the model has multiple polynomial features and can fit non-linear relationships.

---

## Problem After Polynomial Transformation

After converting the data into polynomial features, the number of coefficients increased.

text w0, w1, w2, ..., w10 

Some coefficients can become very large.

Large coefficients can lead to:

- Complex curves
- Overfitting
- Poor generalization on unseen data

This is where regularization helps.

---

# Ridge Regression (L2 Regularization)

### Formula

text Loss = MSE + λ Σ wi² 

### Idea

Ridge adds a penalty on squared coefficients.

### Effect

- Shrinks coefficients
- Reduces model complexity
- Helps prevent overfitting
- Keeps all features

### Observation

The learned curve becomes smoother because very large coefficients are penalized.

---

# Lasso Regression (L1 Regularization)

### Formula

text Loss = MSE + λ Σ |wi| 

### Idea

Lasso adds a penalty on the absolute value of coefficients.

### Effect

- Shrinks coefficients
- Can make coefficients exactly zero
- Performs feature selection

### Observation

Some polynomial terms become completely unused because their coefficients become zero.

---

# Elastic Net

### Formula

text Loss = MSE + λ₁ Σ |wi| + λ₂ Σ wi² 

### Idea

Elastic Net combines both Ridge and Lasso.

### Effect

- Shrinks coefficients
- Can remove unnecessary features
- More stable than pure Lasso
- Works well when features are correlated

### Observation

Elastic Net gives a balance between Ridge and Lasso.

---

# Comparison

| Method | Regularization | Feature Selection |
|----------|----------|----------|
| Ridge | L2 | No |
| Lasso | L1 | Yes |
| Elastic Net | L1 + L2 | Yes |

---

# Key Takeaways

- Polynomial Features increase model complexity.
- High complexity can cause overfitting.
- Regularization adds a penalty to large coefficients.
- Ridge shrinks coefficients.
- Lasso shrinks coefficients and can remove features.
- Elastic Net combines the advantages of both Ridge and Lasso.
- Regularization helps the model generalize better on unseen data.

---

## What I Did In This Folder

1. Created a non-linear dataset.
2. Converted the data into polynomial features.
3. Applied Ridge Regression.
4. Applied Lasso Regression.
5. Applied Elastic Net Regression.
6. Compared learned curves.
7. Compared coefficients.
8. Understood how regularization helps reduce overfitting.