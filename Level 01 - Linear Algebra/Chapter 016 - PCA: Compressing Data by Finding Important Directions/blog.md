# Chapter 016 — PCA: Compressing Data by Finding Important Directions

> **The Big Question:** If a dataset lives in many dimensions, can we find a smaller set of directions that preserves most of what varies?

## Where We Are

Chapter 012 taught us that coordinates depend on basis.

Chapter 013 taught us that some directions may be redundant.

Chapter 015 taught us that singular values reveal which directions a matrix stretches strongly or weakly.

Now imagine a dataset instead of a transformation.

Perhaps each example has 100 features.

Do all 100 directions carry equal information?

Usually not.

Some directions may explain most of the variation, while others contain little more than noise.

Principal Component Analysis asks:

> Can we rotate the coordinate system so the first few axes capture as much variation as possible?

---

## 1. The Problem: A Diagonal Cloud

Suppose our data points are

$$
(1,1.1),
(2,1.9),
(3,3.2),
(4,3.9),
(5,5.1).
$$

Plot them mentally:

```text
 y
 ↑
 5|             •
 4|          •
 3|       •
 2|    •
 1| •
 0+----------------→ x
```

The data uses two coordinates, but most variation lies along one diagonal direction.

If we choose an axis along that diagonal, one coordinate may describe most of the structure.

That is dimensionality reduction.

---

## 2. First Attempt: Keep the Feature With Larger Variance

Maybe we simply compare x-variance and y-variance and keep the larger one.

But that fails when important variation is diagonal.

Both original axes can miss the best direction.

> ⚠️ **A Tempting Wrong Idea**
>
> PCA does not merely select original features. It discovers **new directions that are linear combinations of features**.

So we need to search over directions, not columns.

---

## 3. Step Zero: Center the Data

Before asking which direction varies most, move the cloud so its mean sits at the origin.

If data matrix is

$$
X=
\begin{bmatrix}
---\mathbf{x}_1^T---\\
---\mathbf{x}_2^T---\\
\vdots
\end{bmatrix},
$$

compute feature mean

$$
\boldsymbol{\mu}
=
\frac1n\sum_i\mathbf{x}_i.
$$

Then centered data is

$$
X_c=X-\boldsymbol{\mu}.
$$

Why center?

Because PCA is about variation **around the center**, not distance from an arbitrary origin.

Without centering, the mean offset can dominate the result.

---

## 4. Variance Along a Direction

Choose a unit vector

$$
\mathbf{v}.
$$

Project each centered point onto that direction:

$$
z_i=\mathbf{x}_i\cdot\mathbf{v}.
$$

Now we have one number per point.

The variance of those projected numbers tells us how spread out the data is along $\mathbf{v}$.

So PCA's first question becomes:

$$
\boxed{
\text{Which unit direction }\mathbf{v}\text{ maximizes projected variance?}
}
$$

That direction is the first principal component.

---

## 5. Build the Covariance Matrix

For centered data $X_c$ with examples as rows, covariance matrix is

$$
C=
\frac{1}{n-1}X_c^TX_c.
$$

For two features,

$$
C=
\begin{bmatrix}
\operatorname{Var}(x) & \operatorname{Cov}(x,y)\\
\operatorname{Cov}(x,y) & \operatorname{Var}(y)
\end{bmatrix}.
$$

Diagonal terms measure feature variance.

Off-diagonal terms measure whether features move together.

If covariance is strongly positive, the cloud tends to tilt diagonally upward.

If negative, it tends to tilt downward.

---

## 6. Why Eigenvectors Appear

Projected coordinate is

$$
z=X_c\mathbf{v}.
$$

Projected variance is proportional to

$$
\|X_c\mathbf{v}\|^2.
$$

Expand:

$$
\|X_c\mathbf{v}\|^2
=
\mathbf{v}^TX_c^TX_c\mathbf{v}.
$$

Ignoring the constant factor $1/(n-1)$,

$$
\text{variance along }\mathbf{v}
=
\mathbf{v}^TC\mathbf{v}.
$$

We want to maximize this subject to

$$
\|\mathbf{v}\|=1.
$$

The maximizing direction is the eigenvector of $C$ with largest eigenvalue.

So eigenvectors appear because PCA is an optimization problem over directions.

---

## 7. The First Principal Component

Let

$$
C\mathbf{v}_1=\lambda_1\mathbf{v}_1
$$

with largest eigenvalue

$$
\lambda_1.
$$

Then

$$
\mathbf{v}_1
$$

is the direction of maximum variance.

And

$$
\lambda_1
$$

is the amount of variance captured along that direction.

This gives a beautiful interpretation of eigenvalues:

> in PCA, eigenvalue = variance captured by that principal direction.

---

## 8. The Second Principal Component

After finding the first direction, we want the next direction that captures as much remaining variance as possible while staying perpendicular to the first.

That is the eigenvector associated with the second-largest eigenvalue.

Continue similarly.

Because covariance matrix is symmetric, its eigenvectors can be chosen orthonormal.

So PCA naturally constructs an orthonormal basis ordered by importance.

---

## 9. A New Coordinate System

Stack principal directions as columns:

$$
V=
\begin{bmatrix}
|&|&&|\\
\mathbf{v}_1&\mathbf{v}_2&\cdots&\mathbf{v}_d\\
|&|&&|
\end{bmatrix}.
$$

Then new coordinates are

$$
Z=X_cV.
$$

Each row of $Z$ describes one example in the principal-component basis.

This is Chapter 012's change of basis, now chosen from data.

---

## 10. Dimensionality Reduction

If eigenvalues are

$$
[9.0,\ 1.0,\ 0.05,\ 0.01],
$$

most variation lives in the first two directions.

So keep only

$$
V_k=[\mathbf{v}_1\ \mathbf{v}_2].
$$

Then

$$
Z_k=X_cV_k.
$$

Original data had 4 features.

Compressed representation has 2 principal coordinates.

We discarded directions with little variance.

---

## 11. Explained Variance Ratio

Total variance is sum of eigenvalues:

$$
\sum_i\lambda_i.
$$

The proportion captured by component $i$ is

$$
\boxed{
\text{explained variance ratio}_i
=
\frac{\lambda_i}{\sum_j\lambda_j}
}
$$

For eigenvalues

$$
[9,1],
$$

the first component captures

$$
\frac9{10}=90\%.
$$

The second captures 10%.

That gives a principled way to choose how many dimensions to keep.

---

## 12. Reconstruction

Compression should not only reduce coordinates; we should understand what information is lost.

If

$$
Z_k=X_cV_k,
$$

then reconstruct approximately:

$$
\hat X_c=Z_kV_k^T.
$$

Add the mean back:

$$
\boxed{
\hat X=Z_kV_k^T+\boldsymbol{\mu}
}
$$

If $k=d$, reconstruction is exact (up to numerical precision).

If $k<d$, reconstruction is approximate.

Compression error comes from discarded directions.

---

## 13. PCA Through SVD

Instead of explicitly building covariance matrix, perform SVD of centered data:

$$
X_c=U\Sigma V^T.
$$

Then columns of $V$ are principal directions.

Why?

Because

$$
X_c^TX_c
=
V\Sigma^T\Sigma V^T.
$$

So $V$ diagonalizes the covariance structure.

Eigenvalues of covariance are related to squared singular values:

$$
\lambda_i
=
\frac{\sigma_i^2}{n-1}.
$$

Thus PCA and SVD are tightly connected.

---

## 14. Why SVD Is Often Preferred Numerically

Computing covariance first squares the condition number and can magnify numerical issues.

Direct SVD of centered data is often more numerically stable.

So there are two conceptual routes:

### Covariance route

```text
center X
  ↓
C = X^T X/(n-1)
  ↓
eigenvectors of C
```

### SVD route

```text
center X
  ↓
X = UΣV^T
  ↓
principal directions = columns of V
```

Same geometry, different computation.

---

## 15. Tiny Numeric Intuition

Suppose centered points lie exactly on

$$
y=x.
$$

Then every point is a multiple of

$$
\begin{bmatrix}1\\1\end{bmatrix}.
$$

Normalized direction is

$$
\frac1{\sqrt2}
\begin{bmatrix}1\\1\end{bmatrix}.
$$

Variance perpendicular to that direction is zero.

So PCA finds:

- first component: diagonal direction,
- second component: perpendicular direction with zero variance.

The dataset is stored in 2D but intrinsically 1D.

---

## 16. A Tempting Wrong Idea: “Low Variance Means Unimportant”

PCA preserves directions of high variance.

But high variance is not the same as high predictive usefulness.

Imagine a classification problem where the label depends on a subtle low-variance feature.

PCA might discard it.

> ⚠️ **A Tempting Wrong Idea**
>
> PCA is unsupervised. It does not know your target labels. It preserves variance, not task relevance.

That distinction matters enormously.

---

## 17. Scaling Before PCA

Suppose features are:

```text
age:      18–80
income:   20,000–5,000,000
```

Raw variance of income may dominate simply because units are larger.

PCA on unscaled features may mostly discover “income direction.”

If features have incomparable units, standardization is often appropriate:

$$
z=
\frac{x-\mu}{\sigma}.
$$

But scaling changes the question.

PCA on covariance and PCA on standardized/correlation data are not identical analyses.

---

## 18. Geometry of Projection Error

When projecting a point onto a principal subspace, the reconstruction error is the perpendicular distance from the point to that subspace.

PCA chooses the rank-$k$ linear subspace minimizing total squared reconstruction error.

This is deeply connected to truncated SVD and the Eckart–Young theorem.

So PCA can be understood in two equivalent ways:

1. maximize captured variance,
2. minimize squared reconstruction error.

---

## 19. Shape Check

Suppose

$$
X\in\mathbb{R}^{1000\times50}.
$$

That means:

- 1000 examples,
- 50 features.

After centering:

$$
X_c:(1000\times50).
$$

Covariance:

$$
C=X_c^TX_c/(999)
$$

has shape

$$
(50\times50).
$$

If we keep $k=5$ principal directions:

$$
V_k:(50\times5).
$$

Compressed data:

$$
Z=X_cV_k:(1000\times5).
$$

Reconstruction:

$$
ZV_k^T:(1000\times50).
$$

Shapes tell the compression story.

---

## 20. Code From First Principles

```python
import numpy as np

X = np.array([
    [1.0, 1.1],
    [2.0, 1.9],
    [3.0, 3.2],
    [4.0, 3.9],
    [5.0, 5.1],
])

mean = X.mean(axis=0)
Xc = X - mean

C = Xc.T @ Xc / (len(X) - 1)

eigvals, eigvecs = np.linalg.eigh(C)
order = np.argsort(eigvals)[::-1]
eigvals = eigvals[order]
eigvecs = eigvecs[:, order]

pc1 = eigvecs[:, 0]
z = Xc @ pc1
```

The library call is small because the mathematics has already done the conceptual work.

---

## 21. Verify PCA With SVD

```python
U, s, Vt = np.linalg.svd(Xc, full_matrices=False)

pc1_svd = Vt[0]

# Direction can flip sign and still represent the same axis.
assert np.isclose(abs(np.dot(pc1, pc1_svd)), 1.0)
```

Why absolute value?

Because if $\mathbf{v}$ is a principal direction, then $-\mathbf{v}$ describes the same axis.

Eigenvector sign is arbitrary.

---

## 22. Break It

### Forget to center

Run PCA on data with a large offset. Compare directions before and after centering.

### Mix incompatible units

Use one feature measured in millions and another in decimals. Observe domination.

### Keep too few components

Compress curved or multi-directional data to 1D and inspect reconstruction loss.

### Use PCA for nonlinear structure

Points arranged on a curved manifold may require many linear components even though intrinsic structure is simple.

PCA is linear.

---

## 23. Machine-Learning Connections

PCA appears in:

- visualization,
- noise reduction,
- preprocessing,
- exploratory data analysis,
- compression,
- decorrelation,
- speeding up downstream models,
- understanding representation geometry.

It also trains an important habit:

> do not confuse the original coordinate system with the underlying structure of the data.

---

## 24. History Lens — Pearson and Hotelling

Ideas underlying PCA trace to Karl Pearson's early twentieth-century work on fitting lower-dimensional linear structures to data, with later formal development by Harold Hotelling.

The historical problem was recognizable even before modern computers:

> many measured variables may reflect a smaller number of dominant patterns.

PCA turns that intuition into linear algebra.

---

## 25. Distinctions That Matter

| Pair | Difference |
|---|---|
| feature selection vs PCA | keep original columns vs create new linear-combination directions |
| variance vs predictive importance | unsupervised spread vs usefulness for target |
| covariance PCA vs standardized PCA | raw-unit variation vs scale-normalized variation |
| projection vs reconstruction | compress into components vs map back to feature space |
| PCA vs nonlinear manifold learning | linear subspace vs potentially curved structure |

---

## 26. What We Discovered

1. PCA finds orthogonal directions of maximum data variance.
2. Data must usually be centered first.
3. Covariance matrix summarizes joint variation.
4. Principal directions are covariance eigenvectors.
5. Eigenvalues measure variance captured by components.
6. Keeping top components reduces dimension.
7. Reconstruction maps compressed points back approximately.
8. SVD of centered data yields principal directions directly.
9. PCA maximizes variance and equivalently minimizes squared reconstruction error.
10. PCA preserves variance, not necessarily task-relevant information.
11. Feature scaling can dramatically change PCA.

---

## 27. One-Minute Explanation

PCA looks at a cloud of centered data and asks which direction has the largest spread. That direction becomes the first principal component. The next component captures the largest remaining spread while staying perpendicular, and so on. These directions are eigenvectors of the covariance matrix, or equivalently right singular vectors of the centered data matrix. By keeping only the first few components, we compress data while preserving as much variance as possible. PCA is therefore a learned change of basis plus truncation.

---

## 28. Mastery Check

1. Why must PCA generally center data?
2. Why is PCA not simply feature selection?
3. What does covariance measure?
4. Why do covariance eigenvectors become principal directions?
5. What does each PCA eigenvalue mean?
6. How do you compute explained variance ratio?
7. How do you reconstruct from $k$ components?
8. Why is PCA closely connected to SVD?
9. Why can standardization change PCA dramatically?
10. Give a case where low variance could still be predictive.
11. Why is eigenvector sign arbitrary?
12. What kind of structure can linear PCA fail to capture?

---

## 🔭 Bridge to Level 2

We now have a language for vectors, matrices, transformations, bases, rank, eigenvectors, SVD and PCA.

But all of these tools describe **structure**.

Learning requires something else:

> **How does a quantity change when we change its input by a tiny amount?**

That question moves us from the mathematics of space to the mathematics of **change**.

Next: functions and slopes.
