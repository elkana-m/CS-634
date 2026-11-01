# Hierarchical and DBSCAN Clustering Lab

**Course:** MSCS-634-M50 – Advanced Big Data and Data Mining  
**Student:** Elkana Munganga

## Purpose

This lab compares two clustering methods on the scikit-learn Wine dataset (178 samples, 13 chemical features, 3 cultivar classes):

- **Agglomerative Hierarchical Clustering** (Ward linkage) — merge nearest groups until a chosen number of clusters remains
- **DBSCAN** — form dense neighborhoods from `eps` and `min_samples`, and label isolated points as noise

Features are standardized before clustering. PCA is used only to plot the data in two dimensions. Both methods are scored with Silhouette, Homogeneity, and Completeness. The notebook is `lab5.ipynb`.

## Key Insights from Clustering Results and Visualizations

**Hierarchical (Ward, k = 3)**

- Silhouette: **0.2774**
- Homogeneity: **0.7904**
- Completeness: **0.7825**
- All 178 wines are assigned a cluster. Three groups match the known class count and agree well with the labels.
- Among k = 2–6, k = 3 had the highest silhouette. More clusters raised homogeneity a little (up to 0.83 at k = 6) but completeness fell as true classes were split.

**DBSCAN (best by silhouette: eps = 1.5, min_samples = 3)**

- 8 clusters, **147 noise points** (about 83% of the data)
- Silhouette: **0.3311** (computed on non-noise points only)
- Homogeneity: **0.1892**
- Completeness: **0.2510**
- The higher silhouette is misleading: it describes a few tight leftover groups, not a useful partition of the full dataset.

**Visualizations**

- The 2D PCA scatter shows three overlapping clouds. That is why a distance method that must pick a single neighborhood size struggles, while a merge-based method can still recover three groups in 13 dimensions.
- The Ward dendrogram supports cutting at three clusters: the last merges jump in height after three branches remain.
- DBSCAN plots with noise in red show the tradeoff clearly. Small `eps` leaves most points red. Large `eps` absorbs almost everyone into one or two blobs and loses the cultivar structure.

**Comparison**

- Hierarchical clustering is the better method here. It uses every observation, finds three groups, and matches the true classes much more closely.
- DBSCAN’s strength is labeling noise without a preset k, but on this standardized 13-feature wine data the density is too even for a single `eps` to recover the three cultivars.

## Challenges and Decisions

- **Scale first, then cluster.** Wine features have very different ranges. I standardized all 13 columns so distance and density are not dominated by proline or magnesium.
- **Cluster in 13D, plot in 2D.** PCA was for visualization only. Overlap in the PCA plot does not mean the 13-feature hierarchical clusters failed.
- **How to pick k.** I chose the hierarchical k with the highest silhouette (k = 3). That choice also lined up with the known class count and the dendrogram cut.
- **How to pick DBSCAN settings.** I searched `eps` in {1.5, 2.0, 2.5, 3.0, 3.5} and `min_samples` in {3, 5, 10}, then kept the valid run with the highest silhouette. That rule selected eps = 1.5 and min_samples = 3, which is compact but discards most of the data. A setting such as eps = 2.5 and min_samples = 10 agrees better with the labels (homogeneity 0.50) but still trails hierarchical clustering.
- **Silhouette for DBSCAN.** I computed silhouette only when at least two non-noise clusters existed, and only on those points. Comparing that number directly to hierarchical silhouette would ignore the 147 unused wines.
- **Multiple metrics.** Homogeneity and completeness were needed because silhouette alone favored a DBSCAN solution that is not a good overall segmentation.
