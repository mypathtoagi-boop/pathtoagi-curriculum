# Chapter 012 — Basis and Change of Coordinates

> **The Big Question:** Are coordinates properties of a vector itself, or are they descriptions that depend on the coordinate system we choose?

## Where We Are

Chapter 009 showed that a matrix is determined by what it does to basis vectors.

Chapter 011 explored linear transformations more deeply.

But we have been quietly assuming one special coordinate system:

$$
\mathbf{e}_1=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{e}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

Why those two vectors?

Because they are convenient—not because nature demands them.

The same geometric arrow can have different coordinates in a different basis.

That idea unlocks PCA, eigenvectors, Fourier methods, embeddings, quantum mechanics and many numerical algorithms.

---

## 1. Coordinates Are Instructions

Take

$$
\mathbf{x}=\begin{bmatrix}3\\2\end{bmatrix}.
$$

In the standard basis, this means

$$
\mathbf{x}=3\mathbf{e}_1+2\mathbf{e}_2.
$$

So coordinates are not the vector itself.

They are the **instructions for building the vector from chosen basis directions**.

That distinction is subtle and essential.

> 💡 **Intuition** — A location can be described as “3 blocks east and 2 blocks north,” or using a rotated street grid. The location is unchanged. The instructions change.

---

## 2. What Makes a Basis?

A basis must do two jobs:

1. **reach every vector in the space**, and
2. **represent each vector uniquely**.

In 2D, choose

$$
\mathbf{b}_1=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{b}_2=\begin{bmatrix}1\\-1\end{bmatrix}.
$$

These are not horizontal and vertical.

Yet they point in different enough directions that together they can build every point in the plane.

That makes them a valid basis.

---

## 3. Express the Same Vector in a New Basis

Let

$$
\mathbf{x}=\begin{bmatrix}4\\2\end{bmatrix}.
$$

We want numbers $c_1,c_2$ such that

$$
\mathbf{x}=c_1\mathbf{b}_1+c_2\mathbf{b}_2.
$$

Substitute:

$$
\begin{bmatrix}4\\2\end{bmatrix}
=
c_1\begin{bmatrix}1\\1\end{bmatrix}
+
c_2\begin{bmatrix}1\\-1\end{bmatrix}.
$$

Coordinate-wise:

$$
c_1+c_2=4
$$

$$
c_1-c_2=2.
$$

Add the equations:

$$
2c_1=6
$$

so

$$
c_1=3.
$$

Then

$$
c_2=1.
$$

Therefore the same geometric vector has new coordinates

$$
[\mathbf{x}]_B=
\begin{bmatrix}3\\1\end{bmatrix}.
$$

Standard coordinates:

$$
\begin{bmatrix}4\\2\end{bmatrix}.
$$

Coordinates in basis $B$:

$$
\begin{bmatrix}3\\1\end{bmatrix}.
$$

Same arrow. Different description.

---

## 4. Turn Basis Vectors Into a Matrix

Put the new basis vectors into columns:

$$
B=
\begin{bmatrix}
1&1\\
1&-1
\end{bmatrix}.
$$

Then

$$
B
\begin{bmatrix}c_1\\c_2\end{bmatrix}
=
\mathbf{x}.
$$

Why?

Because matrix–vector multiplication forms a weighted combination of columns:

$$
c_1\mathbf{b}_1+c_2\mathbf{b}_2.
$$

So the basis matrix translates **basis coordinates → standard coordinates**.

---

## 5. Change Coordinates With an Inverse

We have

$$
B\mathbf{c}=\mathbf{x}.
$$

To recover basis coordinates $\mathbf{c}$ from standard coordinates $\mathbf{x}$, multiply by $B^{-1}$:

$$
\boxed{
\mathbf{c}=B^{-1}\mathbf{x}
}
$$

For our example,

$$
B^{-1}
=
\frac12
\begin{bmatrix}
1&1\\
1&-1
\end{bmatrix}.
$$

Then

$$
B^{-1}
\begin{bmatrix}4\\2\end{bmatrix}
=
\frac12
\begin{bmatrix}6\\2\end{bmatrix}
=
\begin{bmatrix}3\\1\end{bmatrix}.
$$

Exactly what we found by solving equations.

---

## 6. A Tempting Wrong Idea: “Coordinates Are the Vector”

Suppose someone says:

> The vector *is* `[4,2]`.

That is incomplete.

`[4,2]` only has geometric meaning after a basis is understood.

In the standard basis, it means

$$
4\mathbf{e}_1+2\mathbf{e}_2.
$$

In another basis, the same pair of numbers describes a different arrow.

> ⚠️ **A Tempting Wrong Idea**
>
> Coordinates are not intrinsic labels attached to a vector. They are coefficients relative to a chosen basis.

---

## 7. Why Change Basis at All?

Why replace a familiar coordinate system with another one?

Because some problems become dramatically simpler in the right basis.

Imagine a cloud of data stretched diagonally.

In the standard basis:

```text
 y
 ↑
 |        •
 |      •
 |    •
 |  •
 |•
 +------------→ x
```

The important direction is diagonal.

If we rotate the basis so one axis lies along that direction, the data may become easy to describe:

- one coordinate contains most variation,
- the other contains very little.

That is the seed of PCA.

---

## 8. Basis as a Language Choice

Consider the vector

$$
\mathbf{x}=3\mathbf{b}_1+1\mathbf{b}_2.
$$

In basis $B$, its coordinates are simply

$$
\begin{bmatrix}3\\1\end{bmatrix}.
$$

The basis was chosen so these coefficients are useful.

This is like choosing a vocabulary adapted to the problem.

A poor basis can make structure look complicated.

A good basis can make it obvious.

---

## 9. Orthonormal Bases

A particularly convenient basis has vectors that are:

1. mutually perpendicular,
2. each of length 1.

Such a basis is **orthonormal**.

If

$$
Q=[\mathbf{q}_1\ \mathbf{q}_2\ \cdots\ \mathbf{q}_n]
$$

has orthonormal columns, then

$$
Q^TQ=I.
$$

That implies

$$
Q^{-1}=Q^T.
$$

So changing coordinates becomes especially easy:

$$
\mathbf{c}=Q^T\mathbf{x}.
$$

Each coordinate is simply a dot product with a basis vector.

---

## 10. Why Dot Products Reappear

For an orthonormal basis,

$$
c_i=\mathbf{q}_i\cdot\mathbf{x}.
$$

That means coordinates are projections.

The first coordinate asks:

> how much of x points along $q_1$?

The second asks:

> how much points along $q_2$?

So Chapter 007’s dot product has become the machinery of coordinate systems.

---

## 11. Rotation as Change of Basis vs Rotation of the Vector

This distinction often confuses learners.

Two different stories can use similar matrices.

### Active transformation

The vector physically rotates while axes stay fixed.

### Passive change of basis

The vector stays fixed while we describe it using rotated axes.

These are related but conceptually different.

> 💡 **Intuition** — Either rotate the arrow on the paper, or rotate the ruler you use to measure the arrow.

Keeping these stories separate prevents sign and transpose confusion later.

---

## 12. Shape Check

If

$$
B\in\mathbb{R}^{n\times n}
$$

is a basis matrix and

$$
\mathbf{c}\in\mathbb{R}^n,
$$

then

$$
\mathbf{x}=B\mathbf{c}
$$

has shape

$$
(n\times n)(n)\rightarrow(n).
$$

And

$$
\mathbf{c}=B^{-1}\mathbf{x}
$$

also returns an $n$-vector.

A change of basis changes coordinates, not the dimension of the underlying space.

---

## 13. Neural-Network Connection

Representation learning can be viewed partly as learning useful coordinate systems.

Early features may be inconvenient for a task.

A learned matrix transforms them into new coordinates where important structure becomes easier to separate.

Of course deep networks add nonlinearities, so the story is richer than a simple basis change.

But the geometric instinct remains valuable:

> learned layers often try to place data into a representation where the next operation is easier.

---

## 14. PCA Preview

PCA will ask:

> Which orthonormal directions capture the most variation in the data?

Once those directions are found, we use them as a new basis.

Then each data point gets new coordinates:

$$
\mathbf{z}=Q^T\mathbf{x}.
$$

If later coordinates contain little information, we can drop them.

That becomes dimensionality reduction.

---

## 15. Code From Scratch

For a simple basis,

```python
import numpy as np

B = np.array([
    [1.,  1.],
    [1., -1.]
])

x = np.array([4., 2.])

coords = np.linalg.solve(B, x)
assert np.allclose(coords, [3., 1.])

reconstructed = B @ coords
assert np.allclose(reconstructed, x)
```

Notice the two directions:

```text
basis coordinates --B--> standard coordinates
standard coordinates --B^-1--> basis coordinates
```

---

## 16. Break It

### Basis vectors are dependent

Take

$$
\mathbf{b}_1=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{b}_2=\begin{bmatrix}2\\2\end{bmatrix}.
$$

They point in the same direction.

They cannot span the whole plane.

The matrix

$$
B=
\begin{bmatrix}1&2\\1&2\end{bmatrix}
$$

has no inverse.

So not every pair of vectors is a basis.

This failure leads directly to span, independence and rank.

---

## 17. History Lens — Coordinates Are a Choice

Descartes connected geometry and algebra by assigning coordinates to points. Later linear algebra generalized the idea: coordinates only make sense relative to a chosen basis.

The conceptual leap is powerful because it separates two things:

- the underlying geometric object,
- the numbers we use to describe it.

Modern machine learning relies on this separation constantly when it transforms data into new feature spaces.

---

## 18. Distinctions That Matter

| Pair | Difference |
|---|---|
| vector vs coordinates | geometric object vs coefficients in a basis |
| standard basis vs arbitrary basis | convenient default vs any valid spanning independent set |
| active transform vs passive basis change | move vector vs change description |
| basis matrix vs inverse basis matrix | coordinates→standard vs standard→coordinates |
| orthogonal vs orthonormal | perpendicular vs perpendicular and unit length |

---

## 19. What We Discovered

1. Coordinates depend on a chosen basis.
2. A basis must span the space and represent vectors uniquely.
3. Basis vectors placed as columns form a basis matrix.
4. $B\mathbf{c}$ converts basis coordinates to standard coordinates.
5. $B^{-1}\mathbf{x}$ converts standard coordinates to basis coordinates.
6. Orthonormal bases make inversion easy: $Q^{-1}=Q^T$.
7. Dot products with orthonormal basis vectors give coordinates directly.
8. Good bases can expose structure that is hidden in standard coordinates.
9. If basis vectors are dependent, the coordinate system fails.

---

## 20. One-Minute Explanation

A vector is not the same thing as its coordinates. Coordinates are instructions saying how much of each basis vector to combine. Change the basis and the coordinate numbers change even though the geometric vector stays the same. Put basis vectors into the columns of a matrix $B$: multiplying $B$ by basis coordinates reconstructs the vector, while $B^{-1}$ converts the vector back into those coordinates. Choosing a good basis can make a difficult problem simple, which is why basis changes appear throughout machine learning.

---

## 21. Mastery Check

1. Why are coordinates not intrinsic to a vector?
2. What two properties must a basis have?
3. Express `[4,2]` using basis `[1,1]`, `[1,-1]`.
4. Why do basis vectors become columns of $B$?
5. What does $B^{-1}$ do?
6. Why is an orthonormal basis convenient?
7. Why does $Q^TQ=I$ imply $Q^{-1}=Q^T$?
8. Explain active transformation vs passive basis change.
9. Why can dependent vectors not form a basis?
10. How does basis choice foreshadow PCA?

---

## 🔭 Bridge to Chapter 013

We now know that a basis must span the space and give unique coordinates.

But those words hide the next question:

> **How can we tell whether a set of vectors really contributes independent directions, and how many dimensions of space they actually cover?**

That question forces **span, linear independence and rank**.
