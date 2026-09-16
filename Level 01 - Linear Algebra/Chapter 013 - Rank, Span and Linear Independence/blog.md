# Chapter 013 — Rank, Span and Linear Independence

> **The Big Question:** How many genuinely different directions does a set of vectors contain?

## Where We Are

Chapter 012 showed that a valid basis must do two things:

1. reach every vector in the space,
2. represent each vector uniquely.

Then we saw a failure case:

$$
\mathbf{b}_1=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{b}_2=\begin{bmatrix}2\\2\end{bmatrix}.
$$

These vectors do not form a basis of the plane.

Why not?

They look like two vectors, but they only contain **one independent direction**.

That observation leads to three connected ideas:

- span,
- linear independence,
- rank.

---

## 1. The Problem: Counting Vectors Is Not Counting Information

Suppose we have

$$
\mathbf{v}_1=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{v}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

These give two independent directions.

Now add

$$
\mathbf{v}_3=\begin{bmatrix}1\\1\end{bmatrix}.
$$

We now have three vectors.

But did we gain a new direction?

No.

Because

$$
\mathbf{v}_3=\mathbf{v}_1+\mathbf{v}_2.
$$

So the third vector contains no new directional information.

> 💡 **Intuition** — Three instructions do not necessarily mean three freedoms. One instruction may be completely predictable from the others.

---

## 2. Span: What Can These Vectors Build?

Take vectors

$$
\mathbf{v}_1,\mathbf{v}_2,\ldots,\mathbf{v}_k.
$$

Their **span** is the set of every linear combination:

$$
\boxed{
\operatorname{span}(\mathbf{v}_1,\ldots,\mathbf{v}_k)
=
\left\{
\sum_i c_i\mathbf{v}_i
\right\}
}
$$

Read that as:

> choose any coefficients, scale the vectors, add them; everything you can reach belongs to the span.

---

## 3. Span of One Vector

Let

$$
\mathbf{v}=\begin{bmatrix}1\\2\end{bmatrix}.
$$

Its span contains

$$
\ldots,-2\mathbf{v},-\mathbf{v},0,\mathbf{v},2\mathbf{v},\ldots
$$

All those points lie on one line through the origin.

So one nonzero vector spans a line.

---

## 4. Span of Two Nonparallel Vectors

Take

$$
\mathbf{v}_1=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{v}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

A general linear combination is

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2
=
\begin{bmatrix}c_1\\c_2\end{bmatrix}.
$$

Since $c_1$ and $c_2$ can be any real numbers, every point in the plane is reachable.

Therefore

$$
\operatorname{span}(\mathbf{v}_1,\mathbf{v}_2)=\mathbb{R}^2.
$$

---

## 5. A Tempting Wrong Idea: “Two Vectors Always Span 2D”

Take

$$
\mathbf{a}=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{b}=\begin{bmatrix}2\\2\end{bmatrix}.
$$

But

$$
\mathbf{b}=2\mathbf{a}.
$$

Any combination

$$
c_1\mathbf{a}+c_2\mathbf{b}
$$

becomes

$$
(c_1+2c_2)\mathbf{a}.
$$

So every result still lies on the same line.

> ⚠️ **A Tempting Wrong Idea**
>
> The number of vectors does not tell you the dimension of their span. What matters is how many independent directions they contribute.

---

## 6. Linear Independence

Vectors are **linearly independent** if none of them can be built from the others.

The formal test is:

$$
c_1\mathbf{v}_1+\cdots+c_k\mathbf{v}_k=\mathbf{0}
$$

must have only the trivial solution

$$
c_1=c_2=\cdots=c_k=0.
$$

If there is any nonzero combination that produces zero, the vectors are dependent.

---

## 7. Why the Zero-Combination Test Works

Suppose

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2+c_3\mathbf{v}_3=0
$$

with

$$
c_3\ne0.
$$

Then rearrange:

$$
\mathbf{v}_3
=
-\frac{c_1}{c_3}\mathbf{v}_1
-
\frac{c_2}{c_3}\mathbf{v}_2.
$$

So $\mathbf{v}_3$ is built from the others.

That is dependence.

The test is not arbitrary—it encodes redundancy.

---

## 8. Example by Hand

Take

$$
\mathbf{v}_1=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{v}_2=\begin{bmatrix}1\\1\end{bmatrix}.
$$

Suppose

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2=0.
$$

Then

$$
\begin{bmatrix}c_1+c_2\\c_2\end{bmatrix}
=
\begin{bmatrix}0\\0\end{bmatrix}.
$$

Second coordinate gives

$$
c_2=0.
$$

Then first gives

$$
c_1=0.
$$

Only trivial solution.

So the vectors are independent.

---

## 9. Matrices Turn This Into Rank

Put vectors into the columns of a matrix:

$$
A=
\begin{bmatrix}
|&|&&|\\
\mathbf{v}_1&\mathbf{v}_2&\cdots&\mathbf{v}_k\\
|&|&&|
\end{bmatrix}.
$$

The column space of $A$ is exactly the span of its columns.

The number of independent columns is the **rank**.

$$
\boxed{\operatorname{rank}(A)=\text{number of independent directions produced by }A}
$$

---

## 10. Rank as Dimension of the Output Space

Consider

$$
A=
\begin{bmatrix}
1&2\\
1&2
\end{bmatrix}.
$$

Its second column is twice the first.

So rank is 1.

Now apply it to

$$
\mathbf{x}=\begin{bmatrix}x_1\\x_2\end{bmatrix}.
$$

Then

$$
A\mathbf{x}
=
\begin{bmatrix}
x_1+2x_2\\
x_1+2x_2
\end{bmatrix}.
$$

Every output has equal coordinates.

So every output lies on the line

$$
y=x.
$$

A 2D input space has been crushed onto a 1D line.

Rank counts the dimension that survives.

---

## 11. Full Rank

A square $n\times n$ matrix has **full rank** if

$$
\operatorname{rank}(A)=n.
$$

Then its columns are independent.

For a square matrix, full rank implies the transformation does not collapse any direction completely.

This is exactly when an inverse exists.

So several ideas meet:

```text
full rank
  ⇔ independent columns
  ⇔ no lost dimension
  ⇔ unique coordinates
  ⇔ inverse exists   (for square matrices)
```

---

## 12. Why Information Loss Prevents Inversion

Take

$$
P=
\begin{bmatrix}
1&0\\
0&0
\end{bmatrix}.
$$

Then

$$
P\begin{bmatrix}x\\y\end{bmatrix}
=
\begin{bmatrix}x\\0\end{bmatrix}.
$$

All y-information disappears.

For example,

$$
\begin{bmatrix}3\\1\end{bmatrix},
\quad
\begin{bmatrix}3\\5\end{bmatrix},
\quad
\begin{bmatrix}3\\100\end{bmatrix}
$$

all map to

$$
\begin{bmatrix}3\\0\end{bmatrix}.
$$

From the output alone, there is no way to know which input was original.

No inverse can reconstruct destroyed information.

---

## 13. Null Space: Which Inputs Disappear?

The **null space** contains vectors sent to zero:

$$
\boxed{
\mathcal{N}(A)=\{\mathbf{x}:A\mathbf{x}=0\}
}
$$

For

$$
P=
\begin{bmatrix}1&0\\0&0\end{bmatrix},
$$

we need

$$
P\begin{bmatrix}x\\y\end{bmatrix}
=
\begin{bmatrix}x\\0\end{bmatrix}
=
\begin{bmatrix}0\\0\end{bmatrix}.
$$

So

$$
x=0
$$

while $y$ can be anything.

Thus the null space is the entire y-axis.

Those are exactly the directions the transformation destroys.

---

## 14. Rank–Nullity Intuition

For a transformation from an $n$-dimensional input space,

$$
\boxed{\operatorname{rank}(A)+\operatorname{nullity}(A)=n}
$$

Interpretation:

```text
input dimensions
= dimensions that survive
+ dimensions that disappear
```

For the projection matrix $P$ above:

- input dimension = 2,
- rank = 1,
- nullity = 1.

So

$$
1+1=2.
$$

This theorem makes information loss measurable.

---

## 15. Row Space and Column Space

A matrix contains two related geometric spaces.

### Column space

All possible outputs $A\mathbf{x}$.

### Row space

The span of its row vectors.

Both have the same dimension:

$$
\operatorname{rank}(A).
$$

That equality is one of linear algebra's central structural facts.

For machine learning, the most intuitive view is often:

> rank tells us how many independent output directions the matrix can express.

---

## 16. Rank and Data

Suppose a dataset has three features:

```text
height_cm
height_m
weight_kg
```

But

$$
\text{height\_m}=\frac{1}{100}\text{height\_cm}.
$$

The two height columns are redundant.

The dataset has three columns but fewer than three independent feature directions.

Rank exposes redundancy.

This matters for:

- regression,
- covariance matrices,
- PCA,
- numerical stability,
- compression.

---

## 17. Near Dependence: The Numerical Version of Trouble

Exact dependence is easy:

$$
\mathbf{v}_2=2\mathbf{v}_1.
$$

Real data is often almost dependent instead.

Example:

$$
\mathbf{v}_1=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{v}_2=\begin{bmatrix}1.000001\\1\end{bmatrix}.
$$

These are technically independent.

But they are nearly parallel.

Solving equations with such directions can be numerically unstable.

Later, singular values will quantify exactly how close a matrix is to losing a direction.

---

## 18. Shape Check

If

$$
A\in\mathbb{R}^{m\times n},
$$

then

$$
\operatorname{rank}(A)\le\min(m,n).
$$

Why?

A matrix cannot have more independent columns than columns, nor more independent output directions than output dimensions.

For a `(5×3)` matrix:

$$
\operatorname{rank}(A)\le3.
$$

For a `(2×100)` matrix:

$$
\operatorname{rank}(A)\le2.
$$

---

## 19. Code and Verification

```python
import numpy as np

A = np.array([
    [1., 2.],
    [1., 2.]
])

assert np.linalg.matrix_rank(A) == 1

B = np.array([
    [1., 0.],
    [0., 1.]
])

assert np.linalg.matrix_rank(B) == 2
```

Do not treat `matrix_rank` as magic.

The conceptual question is always:

> how many genuinely independent directions remain?

---

## 20. Break It

### Duplicate feature

Copy one dataset column. Rank should not increase.

### Linear combination feature

Add a new feature equal to `2*x1 - 3*x2`. The number of columns increases; rank may not.

### Projection

Use a matrix that maps 3D points onto a plane. Observe rank drop from at most 3 to at most 2.

### Almost duplicate feature

Add tiny noise to a duplicate column. Mathematically rank may become full, but numerical conditioning becomes poor.

This prepares us for SVD.

---

## 21. History Lens — Solving Equations Reveals Structure

Many ideas around rank emerged from studying systems of linear equations. Mathematicians needed to know when equations had unique solutions, infinitely many solutions, or contradictions.

The modern geometric interpretation is especially useful:

- independent directions carry new information,
- dependent directions repeat information,
- rank counts how much survives a transformation.

Machine learning repeatedly faces the same issue in datasets with redundant features and in low-dimensional representations.

---

## 22. Distinctions That Matter

| Pair | Difference |
|---|---|
| span vs independence | what vectors can build vs whether any vector is redundant |
| number of columns vs rank | stored vectors vs independent directions |
| rank vs nullity | surviving dimensions vs destroyed dimensions |
| column space vs null space | possible outputs vs inputs mapped to zero |
| exact dependence vs near dependence | algebraic redundancy vs numerical instability |

---

## 23. What We Discovered

1. Span is everything reachable by linear combinations.
2. Linear independence means no vector is redundant.
3. Rank counts independent directions in a matrix.
4. Column-space dimension equals rank.
5. Rank deficiency means some dimensions are collapsed.
6. Null space contains the directions destroyed by a transformation.
7. Rank + nullity equals input dimension.
8. Full-rank square matrices are invertible.
9. Redundant data features reduce effective dimension.
10. Near dependence motivates singular values and SVD.

---

## 24. One-Minute Explanation

Span tells us everything a set of vectors can build. Linear independence asks whether each vector adds a genuinely new direction. Rank counts how many independent directions a matrix contains or preserves. If rank drops, the matrix has collapsed some information; the directions that disappear live in the null space. A full-rank square matrix loses no dimension and can be inverted. These ideas explain why some coordinate systems fail and prepare us to study eigenvectors and singular values.

---

## 25. Mastery Check

1. What is the span of one nonzero vector in 2D?
2. Why can two vectors span only a line?
3. State the linear-independence test.
4. Why does a nontrivial zero combination imply dependence?
5. What does rank measure geometrically?
6. Why does rank deficiency imply information loss?
7. What is the null space?
8. Explain rank–nullity in plain English.
9. Why can duplicated features fail to increase rank?
10. What is the difference between exact and near dependence?

---

## 🔭 Bridge to Chapter 014

Rank tells us how many directions survive.

But some transformations have special directions that survive in an even stronger sense:

they do not turn at all—they only stretch, shrink, or flip.

> **Which directions does a transformation leave pointing along themselves?**

That question leads to **eigenvectors**.
