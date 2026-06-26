# Boosting

## What is boosting

Boosting is an ensemble method that trains many weak models one after another, where each
new model focuses on the mistakes of the ones before it. A bunch of weak learners stacked
this way turns into one strong model.

A weak learner is usually a **decision stump** (a tree with depth 1, one split).

---

## Bagging vs Boosting

| | Bagging (Random Forest) | Boosting |
|---|---|---|
| How models train | in parallel, independently | one after another, in sequence |
| Each model | a deep strong tree | a small weak learner |
| Focus | random subsets of data | the points the last models got wrong |
| Mainly reduces | variance | bias |

---

## AdaBoost (Adaptive Boosting)

Works by reweighting the training points each round. Uses labels as -1 and +1.

1. start with all points weighted equally (1/N)
2. train a weak stump using the weights
3. compute the weighted error
4. give the stump a say `alpha = 0.5 * ln((1 - err) / err)`, accurate stumps get a bigger say
5. multiply each point weight by `exp(-alpha * y * pred)`, so misclassified points get more weight, then normalise
6. repeat, and predict with the weighted vote `sign(sum of alpha * stump prediction)`

My from scratch version matched sklearn, and on the ring data a single stump was about
63% while 50 boosted stumps got into the 90s.

---

## Gradient Boosting

Boosts in a different way. Instead of reweighting points, each new tree is trained to
predict the **residuals** (the leftover errors) of the current model. You keep adding
small trees, each scaled by a **learning rate**, so the prediction slowly creeps towards
the target.

- small learning rate = smaller, safer steps
- lots of shallow trees adding up = a strong model

---

## XGBoost

A fast, regularised version of gradient boosting. It is what wins a lot of competitions on
tabular data. On the breast cancer data it did really well with barely any tuning.

---

## Key Takeaways

- boosting is sequential, bagging is parallel
- adaboost reweights the hard points, gradient boosting fits the residuals
- both turn weak stumps into a strong model
- xgboost is the go to for tabular data
- tree boosting also gives you feature importances for free

---

## What I Did In This Folder

1. Built the intuition, bagging vs boosting, weak learners, and how reweighting works (`01`)
2. Implemented AdaBoost from scratch with plain functions and matched sklearn (`02`)
3. Showed the gradient boosting residual idea, then compared AdaBoost, Gradient Boosting, Random Forest and XGBoost on breast cancer (`03`)
