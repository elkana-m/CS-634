# KNN and Radius Neighbors Classification Lab

**Course:** MSCS-634-M50 – Advanced Big Data and Data Mining  
**Student:** Elkana Munganga

## Purpose

This lab compares two distance-based classifiers on the scikit-learn Wine dataset (178 samples, 13 chemical features, 3 cultivar classes):

- **K-Nearest Neighbors (KNN)** — classify a sample using a fixed number of nearest neighbors (`k`)
- **Radius Neighbors (RNN)** — classify a sample using all neighbors within a chosen distance (`radius`)

The work uses an 80/20 stratified train/test split, tests several parameter values for each model, plots accuracy trends, and compares the best settings. The notebook is `lab2.ipynb`.

## Key Insights from Accuracy Trends

**KNN**

- `k = 1` was the weakest setting (accuracy 0.778). Using only the single nearest neighbor is more sensitive to noise.
- Accuracy rose to 0.806 at `k = 5` and stayed there for `k = 11`, `15`, and `21`. After a small neighborhood, adding more neighbors did not help on this test set.
- Best KNN result: **k = 5, accuracy 0.8056**.

**RNN**

- The smallest radius performed best. Accuracy was 0.722 at radius 350, fell to 0.694 for 400–500, and dropped to 0.667 at 550–600.
- Larger radii pulled in more distant, less relevant wines and made the decision boundary less precise.
- Best RNN result: **radius = 350, accuracy 0.7222**.

**Comparison**

- KNN outperformed RNN at every tested setting. A fixed number of neighbors was more stable here than a fixed distance.
- Parameter choice mattered more for RNN: accuracy declined steadily as the radius grew, while KNN plateaued after `k = 5`.
- Testing a range of values was necessary. The first or largest neighborhood was not automatically the best.

## Challenges and Decisions

- **Large radii.** Wine features are on very different scales (for example, proline is in the hundreds to thousands). Euclidean distances are therefore large, so radii in the 350–600 range were needed before most test points had neighbors. Feature scaling was not applied, which makes this more visible and is a limitation of the current setup.
- **Empty neighborhoods.** Some test points can fall outside every training point’s radius. I set `outlier_label="most_frequent"` so RNN could still assign a class instead of failing.
- **Stratified split.** Classes are uneven (59 / 71 / 48). I used `stratify=y` and `random_state=42` so the 142/36 split keeps class proportions and stays reproducible.
- **Odd k values.** I used 1, 5, 11, 15, and 21 to reduce majority-vote ties among three classes.
- **How to pick the “best” model.** When several k values tied at 0.806, I reported the smallest of those values (`k = 5`) as the best KNN setting. That keeps the neighborhood as local as possible among the top results.
