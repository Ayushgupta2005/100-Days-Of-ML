## Gradient Descent Variants (Day 07)

### What is Gradient Descent?

Gradient Descent is an optimization algorithm used to find the best values of parameters (weights) by minimizing the loss function.

We update weights using:

w = w - learning_rate × gradient

---

### Loss Function (MSE)

MSE = (1/n) Σ (y_pred - y)²  

- Measures error between actual and predicted values  
- Goal → minimize this  

---

### Gradient (Important)

Gradient tells the direction in which the loss increases the most.  
We move in the opposite direction to reduce error.

Gradient formula:

∂MSE/∂w = (1/n) Xᵀ (Xw - y)

---

### Types of Gradient Descent

#### 1. Batch Gradient Descent

- Uses entire dataset for one update  
- Smooth convergence  
- Slow for large datasets  
Very Stable But Computationally expensive  

---

#### 2. Stochastic Gradient Descent (SGD)

- Uses one data point at a time  
- Very fast updates  
- Noisy convergence (zig-zag)  

Fast But Unstable  

---

#### 3. Mini-Batch Gradient Descent

- Uses small batches of data  
- Balance between batch and SGD  
- Most commonly used  

Efficient & Stable  

---

### Observations from Graph

- Batch GD → smooth curve  
- SGD → fluctuating (noise may be hidden if averaged)  
- Mini-Batch → smoother than SGD  

---

### Key Understanding

- All methods use same formula  
- Difference is how gradient is computed  
---

### Real World Usage

- Mini-Batch GD is used in most ML/DL models  
- SGD is useful for large datasets  
- Batch GD is mostly for small datasets  

---
### Important Takeaways

- Learning rate controls step size  
- Too high → diverges  
- Too low → slow learning  

- Gradient Descent is the base of:
  - Linear Regression  
  - Neural Networks  
  - Deep Learning  
---