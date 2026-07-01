# Recommender Systems

## The job

Guess how much a user would like something they havent seen, then show them the items they
would probably rate highest. Netflix, amazon, spotify, all the same idea.

Everything is built around the **user item matrix**: users as rows, items as columns,
ratings inside, and most of it empty. The goal is to fill in the blanks.

---

## Two Main Approaches

- **Content based** : use the features/tags of the items. Liked a sci-fi action movie, get
  more sci-fi action movies. Needs good item features.
- **Collaborative filtering** : ignore item features, use the crowd. People who rate things
  like you also loved X, so you probably will too. The popular one.

Collaborative filtering has two flavours:

- **user based** : find similar users, recommend what they liked
- **item based** : find items similar to ones you already liked

---

## Similarity

To decide who (or what) is similar, a common choice is **cosine similarity**, the angle
between two rating vectors. Same direction means similar (close to 1), right angle means
unrelated (0).

## Matrix Factorization

The scalable version, and basically SVD again. Split the ratings matrix:

**R ≈ P Qᵀ**

- **P** : a few hidden factors per user (e.g. how much they like action vs romance)
- **Q** : the same hidden factors per movie

A rating is how well a users taste lines up with a movies factors. We learn P and Q by
gradient descent, only using the ratings that actually exist (a mask handles the blanks),
with a small reg term.

On the toy movie data it predicted the hidden ratings within about half a star, and the
learned factors lined up with genres on their own, the action fans got action movies and
the romance fans got romance ones, without ever being told the genres.

## What I Did In This Folder

1. The user item matrix, content based vs collaborative filtering, and cosine similarity (`01`)
2. Built user based collaborative filtering from scratch and made recommendations (`02`)
3. Matrix factorization with gradient descent on a movie dataset, which rediscovered the genres (`03`)
