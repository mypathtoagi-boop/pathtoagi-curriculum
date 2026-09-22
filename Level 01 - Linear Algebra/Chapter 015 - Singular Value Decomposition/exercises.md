# Exercises — Chapter 015: Singular Value Decomposition

## Level 1 — Explain It

1. Explain SVD as three geometric steps without using matrix notation.
2. What is the difference between a right singular vector and a left singular vector?
3. Why are singular values always non-negative?
4. Why does SVD work for rectangular matrices while ordinary eigendecomposition does not?
5. Explain what a zero singular value means geometrically.

## Level 2 — Calculate It

For

$$
A=\begin{bmatrix}3&0\\0&1\end{bmatrix},
$$

calculate

$$
A^TA.
$$

Find its eigenvalues and use them to obtain the singular values.

Then explain why the standard basis vectors are both right and left singular vectors in this case.

## Level 3 — Derive It

Starting from

$$
A=U\Sigma V^T,
$$

derive

$$
A^TA=V\Sigma^T\Sigma V^T.
$$

Then explain why this proves that columns of $V$ are eigenvectors of $A^TA$ and singular values are square roots of its eigenvalues.

## Level 4 — Predict It

For a matrix

$$
A\in\mathbb{R}^{5\times3},
$$

predict the full-SVD shapes:

```text
U      → ______
Σ      → ______
V^T    → ______
```

Then predict the maximum possible rank and maximum number of nonzero singular values.

## Level 5 — Break It

### Experiment A — Rank deficiency

Create a matrix with one column exactly twice another. Compute its singular values. Identify the zero or near-zero singular value.

### Experiment B — Near dependence

Add tiny random noise to the dependent column. Observe how the smallest singular value changes.

### Experiment C — Condition number

Construct diagonal matrices with entries `[1,1]`, `[1,0.1]`, `[1,0.001]`. Compare condition numbers and solve small linear systems with slightly perturbed targets.

Explain why tiny singular values amplify noise.

## Level 6 — Build It

For a small 2×2 matrix:

1. compute $A^TA$,
2. find its eigenvalues/eigenvectors using NumPy eigendecomposition,
3. turn eigenvalues into singular values,
4. construct right singular vectors,
5. derive left singular vectors using $u_i = Av_i/\sigma_i$ for nonzero $\sigma_i$,
6. reconstruct $A$.

Do this before calling `np.linalg.svd`.

## Investigation — Image Compression

Use a grayscale image matrix.

1. Compute its SVD.
2. Reconstruct it using ranks 1, 5, 10, 20, 50.
3. Compare visual quality.
4. Plot singular values on a log scale.
5. Estimate storage required for each rank-k representation.
6. Explain why early singular components preserve coarse structure.

## Investigation — Low-Rank Data

Construct

$$
A=UV^T
$$

with

$$
U\in\mathbb{R}^{100\times3},
\qquad
V\in\mathbb{R}^{50\times3}.
$$

Predict the maximum rank before computing it.

Then add noise and inspect the singular-value spectrum.

Explain the difference between exact rank and effective rank.

## Shape Check

```text
A ∈ R^(8×3)
full U      → ______
full Σ      → ______
full V^T    → ______
max rank    → ______
```

## Mastery Gate

Do not continue until you can answer:

- What does each factor in $UΣV^T$ do geometrically?
- How is SVD connected to eigenvectors?
- Why are singular values useful measures of rank strength?
- What does a tiny singular value mean?
- Why does truncated SVD compress well?
- What is a condition number?
- Why does SVD prepare us naturally for PCA?
