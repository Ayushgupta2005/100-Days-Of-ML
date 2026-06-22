# Hierarchical Clustering

## What is it

Instead of giving you a flat set of clusters like K-Means does, hierarchical clustering
builds a whole tree of clusters. At the bottom every point is its own cluster, at the top
everything is one big cluster, and we cut the tree somewhere in between to get our groups.

The big plus is we dont have to choose the number of clusters before running it.

---

## Two Types

- **Agglomerative (bottom up)** : start with every point as its own cluster and keep
  merging the two closest clusters until there is one left. This is the common one and
  the one sklearn gives you.
- **Divisive (top down)** : start with everything in one cluster and keep splitting it.
  More expensive, so its rarely used.

---

## Linkage Methods

When a cluster has more than one point, "distance between two clusters" needs a rule.
That rule is the linkage:

- **Single** : distance between the two closest points (can chain into long stringy clusters)
- **Complete** : distance between the two farthest points (tight compact clusters)
- **Average** : average distance between all pairs of points
- **Ward** : merges the clusters that increase the total variance the least (nice balanced clusters)

Ward is a good default for blob like data. On the mall data single linkage was clearly the
worst, it chained almost everything into one cluster.

---

## Dendrogram

The dendrogram is the tree diagram of the whole merging history.

- bottom = each point is a leaf
- height of a join = the distance at which those two clusters merged
- a tall jump (long vertical line) means those groups are far apart, a natural place to cut
- to pick the number of clusters, draw a horizontal line and count how many vertical lines it crosses

In scipy, `linkage()` builds it, `dendrogram()` draws it, and `fcluster()` cuts it to give labels.


## What I Did In This Folder

1. Built the intuition, the two types, linkage methods, and the dendrogram idea (`01`)
2. Ran sklearn AgglomerativeClustering on the mall data and compared all four linkages (`02`)
3. Built and read the dendrogram with scipy and cut it to get the clusters (`03`)
