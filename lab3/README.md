# K-Means and K-Medoids Clustering Lab

**Course:** MSCS-634-M50 – Advanced Big Data and Data Mining  
**Student:** Elkana Munganga

## Purpose

This lab compares two partitioning clustering algorithms on the scikit-learn Wine dataset (178 samples, 13 chemical features, 3 cultivar classes):

- **K-Means** — each cluster center is the mean of its assigned points
- **K-Medoids** — each cluster center is an actual observation (a medoid)

The work standardizes the features, clusters the wines into three groups, evaluates the results with Silhouette Score and Adjusted Rand Index (ARI), and plots the clusters on alcohol vs. malic acid. The notebook is `lab3.ipynb`.

## Key Insights from Clustering Results

**K-Means (k = 3)**

- Silhouette Score: **0.2849**
- ARI vs. true wine classes: **0.8975**
- Cluster sizes: 65 / 51 / 62

**K-Medoids (k = 3)**

- Silhouette Score: **0.2676**
- ARI vs. true wine classes: **0.7411**
- Cluster sizes: 55 / 74 / 49
- Medoids were observations 106, 35, and 148

**Observations**

- Both algorithms recovered three groups, but K-Means produced slightly tighter clusters and matched the true labels much more closely.
- The Silhouette Scores are moderate (around 0.27–0.28). The clusters are usable, but they are not cleanly separated in feature space.
- ARI tells a different story from silhouette: K-Means is only a little better on compactness, but much better at recovering the actual cultivars (0.90 vs. 0.74).
- The scatter plots of alcohol vs. malic acid show overlap. That is expected: clustering used all 13 features, while the plots show only two of them.
- K-Means cluster sizes were closer to one another. K-Medoids put more wines into one group (74), which is closer to the true class counts (59 / 71 / 48) but still aligned less well overall.

## Challenges and Decisions

- **Feature scaling.** Wine features are on very different scales (proline is in the hundreds to thousands). I standardized all 13 features with `StandardScaler` so distance-based clustering would not be dominated by a few large-magnitude columns.
- **Choosing k.** I set `k = 3` from the known number of wine classes rather than using an elbow or silhouette search. That makes the ARI comparison fair, because both algorithms are asked to find the same number of groups as the labels.
- **K-Medoids from scratch.** scikit-learn does not include K-Medoids, so I implemented a simple PAM-style loop: assign points to the nearest medoid, then replace each medoid with the point in its cluster that has the lowest total distance.
- **Two evaluation metrics.** Silhouette measures cluster quality without labels. ARI measures agreement with the true classes. I reported both because a compact clustering is not automatically the one that matches cultivars.
- **2D plots vs. 13D clusters.** The visualizations use alcohol and malic acid only. Overlap in those plots does not mean the 13-feature clusters failed; it means two features are not enough to show the full separation.
- **Reproducibility.** K-Means used `random_state=42` and `n_init=10`. The custom K-Medoids used the same seed for the initial medoid draw so the comparison can be rerun.
