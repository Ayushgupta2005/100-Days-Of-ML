# Regularization

## Why Regularization?

When a model becomes too complex, it starts memorizing the training data instead of learning the actual pattern.

This is called Overfitting.

Regularization helps control model complexity by penalizing large coefficients.

---

## Dataset Used

We generated a synthetic non-linear dataset:

python y = 0.5*x³ - 4*x² + 10*x + noise 

The data follows a curved relationship and contains random noise to simulate real-world data.

---

## Why Polynomial Features?

Original data contained only one feature:

text x 

A linear model can only learn:

text y = w₀ + w₁x 

which produces a straight line.

To capture the curve, we transformed the data using:

python PolynomialFeatures(degree=10) 

This converted:

text x 

into:

text 1, x, x², x³, x⁴, ..., x¹⁰ 

Now the model can learn a polynomial relationship.

The model is still Linear Regression, but it is trained on transformed polynomial features.

---

## Problem After Polynomial Features

After transformation, the model has many coefficients:

text w₀, w₁, w₂, ..., w₁₀ 

Some coefficients can become very large.

Large coefficients lead to:

- Complex curves
- High variance
- Overfitting

Regularization solves this problem.

---

# Ridge Regression (L2)

Ridge adds a penalty on squared coefficients.

Loss = MSE + \lambda \sum w_i^2

### Effect

- Shrinks coefficients
- Reduces model complexity
- Helps prevent overfitting
- Keeps all features

### Observation

Large coefficients become smaller, resulting in smoother curves.

---

# Lasso Regression (L1)

Lasso adds a penalty on absolute coefficients.

Loss = MSE + \lambda \sum |w_i|

### Effect

- Shrinks coefficients
- Can make coefficients exactly zero
- Performs feature selection

### Observation

Some polynomial terms become completely unused.

---

# Elastic Net

Elastic Net combines Ridge and Lasso.

Loss = MSE + \lambda_1 \sum |w_i| + \lambda_2 \sum w_i^2

### Effect

- Shrinks coefficients
- Can remove features
- More stable than pure Lasso
- Useful when many features are correlated

---

# Comparison

| Method | Penalty | Feature Selection |
|----------|----------|----------|
| Ridge | L2 | No |
| Lasso | L1 | Yes |
| Elastic Net | L1 + L2 | Yes |

---

# Key Learnings

- Polynomial Features increase model complexity.
- High complexity can cause overfitting.
- Regularization controls coefficient size.
- Ridge shrinks coefficients.
- Lasso shrinks and removes coefficients.
- Elastic Net combines both approaches.
- Regularization improves generalization on unseen data.

---

## Folder Summary

In this folder we:

1. Created a non-linear dataset.
2. Converted it into polynomial features.
3. Trained Ridge Regression.
4. Trained Lasso Regression.
5. Trained Elastic Net Regression.
6. Compared coefficients and learned curves.
7. Understood how regularization reduces overfitting.
```