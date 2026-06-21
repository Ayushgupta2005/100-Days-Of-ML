# DBSCAN (Density Based Spatial Clustering)

## Why DBSCAN?

K-Means worked but it had some problems:

- we must choose k beforehand
- it assumes the clusters are round blobs
- it forces every point into a cluster, even the clear outliers

DBSCAN fixes a lot of this by looking at density instead of distance to a center.

---

## Main Idea

A cluster is just a dense region of points, and clusters are separated by low density
(empty) regions. Lonely points sitting in the empty areas are treated as noise.

---

## Two Settings

- **eps** : the radius around a point that decides how close counts as close
- **min_pts** : the minimum number of points inside eps for an area to count as dense

Everything in DBSCAN is built from these two numbers.

---

## Three Types of Points

- **Core point** : has at least min_pts points within eps (including itself). Sits in the dense middle.
- **Border point** : not enough neighbors to be core, but lies within eps of a core point. Sits on the edge.
- **Noise point** : neither core nor border. Sits alone in a low density area.

---

## How It Works (Steps)

1. Pick an unvisited point and find its neighbors within eps.
2. If it has fewer than min_pts neighbors, mark it as noise (for now).
3. If it has enough neighbors, start a new cluster and keep adding all the connected dense points.
4. A point marked noise earlier can later turn into a border point of a cluster.
5. Repeat until all points are visited.

---

## DBSCAN vs K-Means

| Thing | K-Means | DBSCAN |
|-------|---------|--------|
| Number of clusters | you choose k | found automatically |
| Cluster shape | round blobs only | any shape |
| Outliers | forced into a cluster | marked as noise |
| Speed and high dimensions | faster, handles it better | slower, sensitive to eps |

---

## Strengths

- finds clusters of any shape
- detects outliers as noise
- no need to know the number of clusters beforehand

## Weaknesses

- one eps does not fit clusters that have very different density
- choosing a good eps is tricky
- struggles in very high dimensions where all distances start to look the same

---

## Key Takeaways

- DBSCAN groups points by density, not by distance to a center
- eps and min_pts control everything
- it is great for non convex shapes and noisy data
- K-Means is still better for round, similar sized clusters

---

## What I Did In This Folder

1. Built the intuition behind density based clustering (`01`)
2. Implemented DBSCAN from scratch using simple functions, no class (`02`)
3. Compared DBSCAN with K-Means on the moons and circles datasets (`03`)
4. Used DBSCAN on the real Mall Customers data to segment customers and flag the outliers as noise (`04`)
