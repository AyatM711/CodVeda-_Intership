# Task 3: Clustering Analysis (K-Means) — Analysis Summary
**Level:** 2 (Intermediate)
**Dataset:** `iris.csv` (150 records, 4 numeric features, 1 species label)
**Tools:** Python, pandas, scikit-learn, matplotlib, seaborn

---

## Objective

Use K-Means clustering to group similar flowers based on their physical measurements alone, without using the known species label, then evaluate whether the resulting groups align with real biological categories.

## Data Preparation

The dataset had no missing values across its 150 records. Four numeric features were used for clustering:

- `sepal_length`
- `sepal_width`
- `petal_length`
- `petal_width`

The `species` column (setosa, versicolor, virginica) was deliberately excluded from the clustering process it was only used afterward, to check how well the unsupervised clusters matched reality.

## Methodology

1. **Standardized** all four features using `StandardScaler` so no single measurement (e.g., sepal length, which has larger raw values) would dominate the distance calculations used by K-Means.
2. **Determined the optimal number of clusters** using the elbow method: K-Means was run for k = 1 through 10, recording inertia (within-cluster sum of squared distances) at each step.
3. **Fitted the final K-Means model** with the chosen k.
4. **Visualized** the resulting clusters and cross-checked them against the real species labels.

## Choosing k: The Elbow Method

<img width="522" height="297" alt="image" src="https://github.com/user-attachments/assets/ad33d30d-16f3-44d8-ab36-cec6fe556d2d" />


| k | Inertia |
|---|---|
| 1 | 600.00 |
| 2 | 223.73 |
| **3** | **140.97** |
| 4 | 114.62 |
| 5 | 91.30 |
| ... | ... |
| 10 | 50.62 |

Inertia drops sharply from k=1 to k=3, then levels off  each additional cluster beyond 3 gives a much smaller improvement. This "elbow" at **k = 3** was selected as the optimal number of clusters, which conveniently matches the fact that the dataset contains exactly 3 iris species.

## Results

| Cluster | Size |
|---|---|
| 0 | 53 |
| 1 | 50 |
| 2 | 47 |

**Cluster vs. actual species (validation only):**

| Cluster | setosa | versicolor | virginica |
|---|---|---|---|
| 0 | 0 | 39 | 14 |
| 1 | 50 | 0 | 0 |
| 2 | 0 | 11 | 36 |

**Interpretation:**

- **Cluster 1 is a perfect match for setosa**  all 50 setosa flowers were grouped together, and no other species leaked in. Setosa is measurably distinct from the other two species in petal size.
- **Clusters 0 and 2 mostly separate versicolor and virginica**, but with some overlap: 14 virginica flowers were grouped with versicolor (cluster 0), and 11 versicolor flowers were grouped with virginica (cluster 2). This reflects real biological similarity  versicolor and virginica are closer to each other in petal/sepal dimensions than either is to setosa.

## Visual Findings

<img width="853" height="302" alt="image" src="https://github.com/user-attachments/assets/3e09bb0f-1690-4e44-81d4-63b42111f232" />


- **Petal length vs. petal width (left):** Clusters are cleanly separated petal measurements are the strongest distinguishing feature between species.
- **Sepal length vs. sepal width (right):** Clusters overlap more heavily here sepal measurements alone are less reliable for telling species apart.

## Conclusion

K-Means, using only unlabeled measurements, successfully rediscovered a grouping structure that closely mirrors the real iris species with perfect separation for setosa and reasonable (though imperfect) separation between versicolor and virginica. This demonstrates that petal measurements carry more discriminative signal than sepal measurements for this dataset.

