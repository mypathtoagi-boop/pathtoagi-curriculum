# Exercises — Chapter 016: PCA: Compressing Data by Finding Important Directions

## Level 1 — Explain It

1. Explain PCA without using the phrase “eigenvector.”
2. Why is PCA a change of basis rather than simple feature selection?
3. Why do we usually center data first?
4. Explain what “explained variance” means.
5. Why can a low-variance direction still matter for prediction?

## Level 2 — Calculate It

Consider centered points that lie exactly on the line

$$
y=x.
$$

Use these examples:

$$
(-2,-2),\ (-1,-1),\ (0,0),\ (1,1),\ (2,2).
$$

1. Compute the covariance matrix.
2. Identify the direction of maximum variance.
3. Identify the perpendicular direction.
4. Predict the second eigenvalue.
5. Explain the intrinsic dimension of the dataset.

## Level 3 — Derive It

Start with projected coordinates

$$
z=X_c\mathbf{v}.
$$

Show that squared projected length is

$$
\|X_c\mathbf{v}\|^2
=
\mathbf{v}^TX_c^TX_c\mathbf{v}.
$$

Then explain why maximizing projected variance under $\|v\|=1$ leads to an eigenvector problem.

Also derive the SVD relationship

$$
X_c^TX_c=V\Sigma^2V^T.
$$

## Level 4 — Predict It

For

```text
X shape = (1000, 50)
k = 5
```

predict:

```text
mean                 → ______
X_centered           → ______
covariance matrix    → ______
V_k                  → ______
Z = X_centered @ V_k → ______
reconstruction       → ______
```

Explain every dimension.

## Level 5 — Break It

### Experiment A — No centering

Create a diagonal cloud and add a large constant offset. Run PCA with and without centering. Compare first principal directions.

### Experiment B — Incompatible units

Create one feature around `0–1` and another around `0–100000`. Run PCA before and after standardization.

Explain why the dominant component changes.

### Experiment C — Throw away useful information

Construct a classification dataset where the label depends on a low-variance feature while another irrelevant feature has huge variance. Apply PCA to one component and inspect classification information.

Explain why PCA cannot know what the label needs.

## Level 6 — Build It

Implement PCA without scikit-learn:

```python
def pca_fit_transform(X, k):
    # center
    # covariance or SVD
    # select top directions
    # project
    return Z, components, mean
```

Requirements:

- return components ordered by explained variance
- reconstruct the original data approximately
- compute explained-variance ratios
- compare your results with `sklearn.decomposition.PCA` if available
- account for sign ambiguity in eigenvectors

## Investigation — Compression Curve

Take a dataset with at least 20 features.

For each $k=1,2,...,20$:

1. project to k components,
2. reconstruct,
3. measure squared reconstruction error,
4. compute cumulative explained variance.

Plot:

- reconstruction error vs k,
- cumulative explained variance vs k.

Then answer:

- where is the “elbow”?
- how many dimensions would you keep for 95% variance?
- is 95% necessarily right for every task?

## Investigation — PCA From SVD vs Covariance

Compute PCA in two ways:

1. eigenvectors of covariance matrix,
2. SVD of centered data.

Compare principal directions and explained variances.

Remember that component signs may flip.

Explain why a sign flip does not change the principal axis.

## Mastery Gate

Do not continue until you can answer:

- What optimization problem does PCA solve?
- Why does covariance appear?
- What do principal-component eigenvalues represent?
- How does PCA reduce dimension?
- How is reconstruction performed?
- How does SVD produce PCA directions?
- Why does scaling matter?
- Why is PCA unsupervised?
- What is one major limitation of linear PCA?
