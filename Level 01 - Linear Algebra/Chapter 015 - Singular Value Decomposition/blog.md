# Chapter 015 — Singular Value Decomposition

> **The Big Question:** If a matrix stretches space in complicated ways—or is not even square—can we still find the fundamental directions of that transformation?

## Where We Are

Chapter 014 introduced eigenvectors: special directions that a square transformation does not rotate away from themselves.

For an eigenvector $\mathbf{v}$,

$$
A\mathbf{v}=\lambda\mathbf{v}.
$$

That is beautiful, but it has limits.

What if $A$ is rectangular?

What if its eigenvectors are difficult to use?

What if we want to know not merely which directions remain on themselves, but which input directions are stretched most strongly into which output directions?

This is the problem Singular Value Decomposition solves.

SVD is one of the most important constructions in numerical linear algebra, machine learning, compression, recommendation systems and dimensionality reduction.

---


## The Picture to Hold in Your Head

Eigendecomposition is beautiful, but it is picky: the matrix must be square, and the directions you want may not exist as real eigenvectors. SVD removes those restrictions.

The geometric story is universal: **turn the input axes, stretch along perpendicular directions, then turn again into the output space**. The stretch amounts are the singular values. A large singular value means that direction survives strongly; a tiny one means the matrix nearly erases it; zero means it is lost completely.

This is why one decomposition explains rank, conditioning, pseudoinverses, compression and latent factors. SVD is not a bag of applications. They are all consequences of knowing which directions a matrix preserves strongly and which it barely preserves at all.

> 🎛️ Play **The three steps** slowly enough to name $V^T$, $\Sigma$, and $U$ while the animation is moving. Then use **Keeping one axis** and try to make the second singular value almost disappear.

## 1. Start With a Simple Stretch

Consider

$$
A=
\begin{bmatrix}
3&0\\
0&1
\end{bmatrix}.
$$

A unit circle under this transformation becomes an ellipse.

```text
Before:                 After A:

    ***                    ******
  *     *                *        *
 *   •   *      →       *    •     *
  *     *                *        *
    ***                    ******
```

The x-direction is stretched by 3.

The y-direction is stretched by 1.

So the transformation has two natural input directions and two associated stretch amounts.

For this diagonal matrix, the structure is obvious.

For a general matrix it is hidden.

SVD finds it.

---

## 2. What Would a Useful Decomposition Need?

Imagine a complicated linear transformation.

We want to describe it as a sequence of simple geometric actions:

1. rotate or re-express the input in useful orthogonal directions,
2. stretch or shrink each direction independently,
3. rotate into the output space.

That suggests a factorization of the form

$$
A=U\Sigma V^T.
$$

Do not memorize the letters yet.

First understand the story:

```text
input
  ↓
choose special input directions
  ↓
stretch each direction independently
  ↓
choose special output directions
  ↓
output
```

That is SVD.

---

## 3. The SVD Statement

For any real matrix

$$
A\in\mathbb{R}^{m\times n},
$$

there exist matrices

$$
U\in\mathbb{R}^{m\times m},
$$

$$
V\in\mathbb{R}^{n\times n},
$$

and a rectangular diagonal matrix

$$
\Sigma\in\mathbb{R}^{m\times n}
$$

such that

$$
\boxed{A=U\Sigma V^T}.
$$

The columns of $U$ and $V$ are orthonormal.

The non-negative diagonal entries of $\Sigma$ are the **singular values**:

$$
\sigma_1\ge\sigma_2\ge\cdots\ge0.
$$

---

## 4. Read the Formula Right to Left

Apply $A$ to a vector:

$$
A\mathbf{x}=U\Sigma V^T\mathbf{x}.
$$

Since the rightmost operation happens first:

### Step 1 — $V^T$

Express the input in the special orthonormal directions stored in $V$.

### Step 2 — $\Sigma$

Stretch or shrink each of those coordinates by a singular value.

### Step 3 — $U$

Rotate/re-express the result in the output singular-vector directions.

So SVD says:

> Every linear transformation can be understood as orthogonal change of coordinates → axis-aligned scaling → orthogonal change of coordinates.

For rectangular matrices, dimensions may also expand or contract.

---

## 5. Why the Singular Values Are Non-Negative

Unlike eigenvalues, singular values are defined as square roots of eigenvalues of

$$
A^TA.
$$

Why $A^TA$?

Because for any vector $\mathbf{x}$,

$$
\mathbf{x}^TA^TA\mathbf{x} = (A\mathbf{x})^T(A\mathbf{x}) = \|A\mathbf{x}\|^2 \ge0.
$$

So $A^TA$ is positive semidefinite.

Its eigenvalues cannot be negative.

If

$$
A^TA\mathbf{v}_i=\lambda_i\mathbf{v}_i,
$$

then

$$
\sigma_i=\sqrt{\lambda_i}.
$$

This is the bridge between eigenvectors and SVD.

---

## 6. Derive the Right Singular Vectors

Suppose

$$
A=U\Sigma V^T.
$$

Then

$$
A^TA = (U\Sigma V^T)^T(U\Sigma V^T).
$$

Reverse order under transpose:

$$
A^TA = V\Sigma^TU^TU\Sigma V^T.
$$

Since $U$ is orthogonal,

$$
U^TU=I.
$$

Therefore

$$
A^TA = V\Sigma^T\Sigma V^T.
$$

So columns of $V$ are eigenvectors of $A^TA$.

And diagonal entries of $\Sigma^T\Sigma$ are

$$
\sigma_i^2.
$$

Thus:

```text
right singular vectors = eigenvectors of A^T A
singular values         = sqrt(eigenvalues of A^T A)
```

---

## 7. Left Singular Vectors

Similarly,

$$
AA^T=U\Sigma\Sigma^TU^T.
$$

So columns of $U$ are eigenvectors of

$$
AA^T.
$$

This gives the paired geometry:

- $V$: important directions in input space,
- $U$: corresponding directions in output space,
- $\Sigma$: stretch factors linking them.

---

## 8. Tiny Numeric Example

Take

$$
A=
\begin{bmatrix}
3&0\\
0&1
\end{bmatrix}.
$$

Then

$$
A^TA=
\begin{bmatrix}
9&0\\
0&1
\end{bmatrix}.
$$

Eigenvalues are

$$
9,1.
$$

So singular values are

$$
\sigma_1=3,
\qquad
\sigma_2=1.
$$

The right singular vectors are the standard basis directions.

The left singular vectors are also the standard basis directions.

Therefore

$$
U=I,
\qquad
V=I,
\qquad
\Sigma=
\begin{bmatrix}3&0\\0&1\end{bmatrix}.
$$

The SVD simply reveals the stretch already visible in the matrix.

---

## 9. A Less Trivial Example: Rotated Stretch

Suppose we rotate space, stretch strongly along one hidden direction, then rotate again.

The raw matrix entries may look arbitrary.

But SVD recovers:

- the hidden input axis that gets stretched most,
- how much it stretches,
- the corresponding output direction.

This is why SVD is often described as finding the **principal axes of a linear transformation**.

The unit circle picture is especially powerful:

```text
unit circle
   ↓ V^T
rotate basis
   ↓ Σ
axis-aligned ellipse
   ↓ U
rotated ellipse
```

The ellipse axes are the singular directions.

Their lengths are the singular values.

---

## 10. Rectangular Matrices: Where SVD Beats Eigenvectors

Consider

$$
A\in\mathbb{R}^{3\times2}.
$$

It maps 2D input vectors into 3D output vectors.

Eigenvectors of $A$ are not even defined in the ordinary sense because input and output live in spaces of different dimensions.

But SVD works perfectly.

$V$ describes directions in $\mathbb{R}^2$.

$U$ describes directions in $\mathbb{R}^3$.

$\Sigma$ describes how strongly each input direction maps into its output partner.

This is one reason SVD is so general.

---

## 11. Rank Appears Inside SVD

Suppose singular values are

$$
[7,\ 2,\ 0,\ 0].
$$

Only two directions survive with nonzero stretch.

Therefore

$$
\operatorname{rank}(A)=2.
$$

In general:

$$
\boxed{
\operatorname{rank}(A)=\text{number of nonzero singular values}
}
$$

This gives a quantitative version of Chapter 006.

A zero singular value means a direction is completely destroyed.

A tiny singular value means a direction is almost destroyed.

---

## 12. Near Dependence Becomes Visible

Chapter 006 introduced almost-dependent columns.

SVD gives us a ruler for that problem.

If

$$
\sigma_{\min}
$$

is extremely small, then one direction is being squashed almost to zero.

That means the transformation is close to losing information.

This causes numerical sensitivity.

The ratio

$$
\kappa(A)=\frac{\sigma_{\max}}{\sigma_{\min}}
$$

for a full-rank matrix is the 2-norm **condition number**.

Large condition number means some directions are stretched much more than others, making inversion sensitive to noise.

---

## 13. Low-Rank Approximation

Suppose singular values are

$$
100,\ 20,\ 1,\ 0.1,\ 0.01.
$$

The first two directions dominate.

Could we keep only them?

SVD writes

$$
A=
\sum_i \sigma_i\mathbf{u}_i\mathbf{v}_i^T.
$$

A rank-$k$ approximation keeps only the first $k$ terms:

$$
\boxed{
A_k=
\sum_{i=1}^{k}\sigma_i\mathbf{u}_i\mathbf{v}_i^T
}
$$

This is not just a heuristic.

The Eckart–Young theorem says truncated SVD gives the best rank-$k$ approximation under common matrix norms.

That is why SVD is so important for compression.

---

## 14. See One Rank-1 Piece

Each term

$$
\sigma_i\mathbf{u}_i\mathbf{v}_i^T
$$

has rank 1.

It says:

1. measure how much the input points along $\mathbf{v}_i$,
2. scale by $\sigma_i$,
3. output along $\mathbf{u}_i$.

So a matrix can be understood as a sum of simple one-direction channels.

That is an extraordinarily useful mental model.

---

## 15. Image Compression Intuition

A grayscale image is a matrix.

Suppose an image is $1000\times1000$.

Raw storage needs about one million pixel values.

If the image has strong low-rank structure, we might approximate it using only $k$ singular components.

For each component we store:

- one left singular vector,
- one right singular vector,
- one singular value.

With small $k$, storage can be dramatically reduced while preserving major visual structure.

The discarded singular directions often correspond to fine detail or noise.

---

## 16. Recommendation Systems

Imagine a user-item rating matrix:

```text
             movie1 movie2 movie3 ...
user1          5      4      ?
user2          1      2      5
user3          ?      5      1
...
```

A low-rank factorization can uncover latent directions such as rough preference axes.

SVD-like ideas help express a huge sparse interaction matrix through a smaller latent representation.

Modern recommender systems use richer methods, but low-rank factorization remains foundational.

---

## 17. Pseudoinverse

If $A$ is rectangular or not invertible, ordinary $A^{-1}$ may not exist.

SVD lets us build the Moore–Penrose pseudoinverse:

$$
A^+=V\Sigma^+U^T.
$$

$\Sigma^+$ replaces each nonzero singular value $\sigma_i$ with

$$
\frac{1}{\sigma_i}.
$$

This provides a principled way to solve least-squares problems and under/overdetermined systems.

Tiny singular values also explain why pseudoinverse solutions may need regularization.

---

## 18. Shape Check

For

$$
A\in\mathbb{R}^{m\times n},
$$

full SVD has

```text
U      (m × m)
Σ      (m × n)
V^T    (n × n)
```

Multiplication:

$$
(m\times m)(m\times n)(n\times n)
\rightarrow
(m\times n).
$$

Reduced/economy SVD stores only necessary dimensions, which is often what numerical libraries return or can return.

---

## 19. Code Experiment

```python
import numpy as np

A = np.array([
    [3., 0.],
    [0., 1.]
])

U, s, Vt = np.linalg.svd(A)

Sigma = np.diag(s)
reconstructed = U @ Sigma @ Vt

assert np.allclose(reconstructed, A)
assert np.allclose(s, [3., 1.])
```

The important experiment is not merely reconstruction.

Change the matrix and inspect:

- singular values,
- columns of `V`,
- columns of `U`,
- how the unit circle transforms.

Predict before running.

---

## 20. A Tempting Wrong Idea: “SVD Is Just Eigenvectors With Extra Steps”

SVD is related to eigendecomposition, but it solves a broader geometric problem.

Eigenvectors require a square transformation from a space to itself.

SVD works for any rectangular matrix and distinguishes input directions from output directions.

> ⚠️ **A Tempting Wrong Idea**
>
> Treating SVD as merely a computational trick hides its geometry. It is a description of how a matrix maps orthogonal input directions into orthogonal output directions with specific stretch factors.

---

## 21. Distinctions That Matter

| Pair | Difference |
|---|---|
| eigenvalue vs singular value | eigenvalue may be negative/complex; singular value is non-negative stretch magnitude |
| eigenvector vs right singular vector | eigenvector stays in same direction under square $A$; right singular vector is special input direction |
| right vs left singular vector | input direction vs corresponding output direction |
| rank vs singular values | rank counts nonzero singular values; values quantify strength |
| inverse vs pseudoinverse | exact undo for invertible square matrix vs generalized least-squares inverse |
| full SVD vs truncated SVD | exact decomposition vs low-rank approximation |

---

## 22. History Lens — Numerical Stability and Data Compression

SVD grew out of nineteenth-century linear algebra and later became central to numerical analysis because it exposes the geometry and conditioning of matrices more robustly than many direct methods.

Its importance exploded in data analysis because the same decomposition answers several practical questions:

- Which directions matter most?
- How close is this matrix to losing rank?
- Can we compress it?
- Can we solve a least-squares system safely?

That is a rare combination of deep theory and immediate usefulness.

---

## 23. What We Discovered

1. Any real matrix can be decomposed as $A=U\Sigma V^T$.
2. $V$ contains orthonormal input directions.
3. $U$ contains orthonormal output directions.
4. Singular values are non-negative stretch magnitudes.
5. Singular values are square roots of eigenvalues of $A^TA$.
6. Rank equals the number of nonzero singular values.
7. Tiny singular values reveal near-lost directions and poor conditioning.
8. Truncated SVD gives principled low-rank approximation.
9. SVD enables compression, latent-factor models and pseudoinverses.
10. PCA will use closely related singular directions to find important variation in data.

---

## 24. One-Minute Explanation

SVD takes any matrix—even a rectangular one—and rewrites it as three simple stages: rotate/re-express the input using $V^T$, stretch independent directions using $\Sigma$, and rotate/re-express into output space using $U$. The singular values tell us how strongly each special direction survives. Zero singular values mean lost dimensions; tiny ones mean nearly lost dimensions. Keeping only the largest singular values gives an optimal low-rank approximation, which is why SVD is fundamental to compression and dimensionality reduction.

---

## 25. Mastery Check

1. What geometric story does $U\Sigma V^T$ tell?
2. Why does SVD work for rectangular matrices?
3. Why are singular values non-negative?
4. How are right singular vectors related to $A^TA$?
5. What do left singular vectors represent?
6. Why does rank equal the number of nonzero singular values?
7. What does a tiny singular value mean geometrically?
8. What does condition number measure?
9. Why does truncated SVD compress matrices?
10. What is the role of the pseudoinverse?

---

## 🔭 Bridge to Chapter 016

SVD tells us which directions a matrix stretches most.

But our next problem begins with a dataset, not a transformation.

A cloud of data may vary strongly in one direction and weakly in another.

> **Can we rotate the coordinate system so that the first few axes capture most of the information in the data?**

That question leads to **Principal Component Analysis**.
