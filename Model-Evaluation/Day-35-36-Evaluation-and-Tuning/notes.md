# Model Evaluation and Tuning

## Why this matters

Up till now i mostly judged models by accuracy on one train test split. Thats not enough.
Accuracy can lie, and a single split is noisy. This folder is about doing it properly:
good metrics, cross validation, and tuning.

---

## Classification Metrics

Everything starts from the **confusion matrix** (binary case):

- **TP** : actual positive, predicted positive
- **TN** : actual negative, predicted negative
- **FP** : actual negative, predicted positive (false alarm)
- **FN** : actual positive, predicted negative (a miss)

From those four:

- **precision** = TP / (TP + FP) : of everything we called positive, how much really was
- **recall** = TP / (TP + FN) : of all the actual positives, how many we caught
- **f1** = harmonic mean of precision and recall (one balanced number)

### Why accuracy lies

On imbalanced data (say 5% positives), a model that always predicts negative gets 95%
accuracy but catches zero positives (recall 0). So look at precision and recall, not just
accuracy.

### ROC and AUC

A classifier gives a probability, and the threshold turns it into a label. Sliding the
threshold trades true positives against false positives. The **ROC curve** draws that
whole tradeoff, and **AUC** (area under it) sums it up: 1.0 is perfect, 0.5 is random.

---

## Cross Validation

A single train test split depends on the random seed, so the score wobbles a few percent.

**k-fold cross validation:** split the data into k folds, train on k-1 and test on the
one left out, rotate so every fold is the test set once, then average. Every point is used
for testing exactly once, so the score is steadier and you also get a std across folds.

**Stratified k-fold:** keeps the same class balance in every fold. Default for
classification, important when classes are imbalanced.

---

## Hyperparameter Tuning

Hyperparameters are the knobs you set before training (number of trees, max depth, etc).
Tuning searches for a good combination, scoring each one with cross validation.

- **Grid search** : tries every combination in the grid. Thorough but slow on big grids.
- **Random search** : samples a fixed number of random combinations. Often just as good and much faster.

Always keep a **separate test set** that the search never sees, and do the final check
there, because the search already used the cv folds.

---

## Key Takeaways

- accuracy is a first glance, not the whole story
- precision vs recall tells you what kind of mistakes the model makes
- cross validation gives a trustworthy score instead of one noisy split
- grid and random search both use cv inside to tune fairly
- judge the final tuned model on held out data it never saw

---

## What I Did In This Folder

1. Confusion matrix, precision, recall, f1, the accuracy trap, and ROC/AUC (`01`)
2. Why one split is noisy, then k-fold from scratch, sklearn cross_val_score, and stratified k-fold (`02`)
3. Grid search and random search with cross validation, then evaluated the tuned model on a held out test set (`03`)
