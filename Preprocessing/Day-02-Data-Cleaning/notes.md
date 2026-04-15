#  Day 02 - Data Cleaning & Preprocessing
## 1. Missing Values

### What is Missing Values?
Missing data (NaN / null values)

###  Why?
ML models cannot handle missing values

###  Methods
- Drop : `df.dropna()`
- Fill:
  - Mean (numerical)  
  - Median (skewed data)  
  - Mode (categorical )

## 2. Encoding

### What is Encoding?
Convert categorical data → numerical

### Why?
ML models only understand numbers

### Types
- Label Encoding → Binary data  
  Example: (Male=1, Female=0)

- One-Hot Encoding → Multiple categories  

### Example
Urban → [1,0,0]  
Rural → [0,1,0]

##  3. Feature Scaling

###  What is Scaling?
Bring all features to same scale

###  Why?
Prevents large values dominating small ones

### 🛠 Types

#### StandardScaler
z = (x - μ) / σ  
Scales data to have:
- Mean = 0  
- Std = 1  


####  MinMaxScaler
x' = (x - min) / (max - min)  
 Range = [0,1]

## Important Rules in Preprocessing

- Handle missing values first  
- Encoding before scaling  
- Don’t scale categorical data  

##  What I Learned

- How to clean real dataset  
- Convert categorical data  
- Scale features for better performance  