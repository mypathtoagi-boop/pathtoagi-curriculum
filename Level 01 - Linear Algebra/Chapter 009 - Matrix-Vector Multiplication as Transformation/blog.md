# Chapter 009 — Matrix–Vector Multiplication as Transformation

> **The Big Question:** What does a matrix actually *do* to a vector?

## Where We Are

Chapter 008 organized many numbers into a matrix.

That gave us a compact way to store rows of related values.

But matrices are more than spreadsheets.

A matrix can act on a vector and produce a new vector.

That action is one of the deepest ideas in linear algebra and one of the most common operations in neural networks.

> **Today:** we discover matrix–vector multiplication as many dot products and as a transformation of space.
>
> **Next:** once one matrix transforms a vector, what happens when we apply one matrix after another?

---

## 1. The Problem: One Neuron Is Not Enough

In Chapter 007 we saw a single neuron-like calculation:

$$
z=\mathbf{w}\cdot\mathbf{x}+b.
$$

Suppose

$$
\mathbf{x}=\begin{bmatrix}2\\3\end{bmatrix}.
$$

One weight vector might detect “large house”:

$$
\mathbf{w}_1=\begin{bmatrix}2\\1\end{bmatrix}.
$$

Another might detect “high area relative to rooms”:

$$
\mathbf{w}_2=\begin{bmatrix}-1\\2\end{bmatrix}.
$$

We could compute separately:

$$
z_1=\mathbf{w}_1\cdot\mathbf{x}
$$

and

$$
z_2=\mathbf{w}_2\cdot\mathbf{x}.
$$

But a real layer may have hundreds or thousands of neurons.

Writing each dot product separately is not a scalable mathematical language.

We need one object that stores all those weight vectors and one operation that computes all outputs together.

---

## 2. Stack the Weight Vectors

Put each weight vector as a row of a matrix:

$$
W=
\begin{bmatrix}
2 & 1\\
-1 & 2
\end{bmatrix}.
$$

Input:

$$
\mathbf{x}=\begin{bmatrix}2\\3\end{bmatrix}.
$$

Now calculate

$$
W\mathbf{x}.
$$

The first row dots with $\mathbf{x}$:

$$
2(2)+1(3)=7.
$$

The second row dots with $\mathbf{x}$:

$$
-1(2)+2(3)=4.
$$

So

$$
\boxed{
W\mathbf{x}
=
\begin{bmatrix}7\\4\end{bmatrix}
}
$$

One matrix–vector multiplication computed two dot products at once.

---

## 3. The Row View

Suppose

$$
W\in\mathbb{R}^{m\times n}
$$

and

$$
\mathbf{x}\in\mathbb{R}^{n}.
$$

Each row of $W$ contains $n$ numbers, exactly matching the $n$ entries of $\mathbf{x}$.

So every row can take a dot product with $\mathbf{x}$.

That produces one number per row.

Therefore

$$
W\mathbf{x}\in\mathbb{R}^{m}.
$$

Shape rule:

```text
(m × n)  times  (n,)
              ↓
             (m,)
```

The inner dimension $n$ must match.

The outer dimension $m$ becomes the output size.

> 💡 **Intuition** — Each row is one question asked of the same input vector. The output contains one answer per question.

---

## 4. A Tempting Wrong Idea: Multiply Matching Slots Only

A beginner may try to multiply

$$
\begin{bmatrix}
2 & 1\\
-1 & 2
\end{bmatrix}
$$

with

$$
\begin{bmatrix}2\\3\end{bmatrix}
$$

entry by entry.

But the shapes do not even match as two grids.

More importantly, element-wise multiplication would not produce the weighted sums we need.

> ⚠️ **A Tempting Wrong Idea**
>
> Matrix multiplication is not “multiply whatever numbers happen to line up visually.” It is a structured collection of dot products.

This distinction will prevent many future bugs.

---

## 5. The Column View: A Different Mental Model

There is another equally important way to understand the same multiplication.

Write the matrix by columns:

$$
W=
\begin{bmatrix}
| & |\\
\mathbf{c}_1 & \mathbf{c}_2\\
| & |
\end{bmatrix}.
$$

If

$$
\mathbf{x}=\begin{bmatrix}x_1\\x_2\end{bmatrix},
$$

then

$$
\boxed{
W\mathbf{x}=x_1\mathbf{c}_1+x_2\mathbf{c}_2
}
$$

For our matrix,

$$
\mathbf{c}_1=\begin{bmatrix}2\\-1\end{bmatrix},
\qquad
\mathbf{c}_2=\begin{bmatrix}1\\2\end{bmatrix}.
$$

With

$$
\mathbf{x}=\begin{bmatrix}2\\3\end{bmatrix},
$$

we get

$$
W\mathbf{x}
=2\begin{bmatrix}2\\-1\end{bmatrix}
+3\begin{bmatrix}1\\2\end{bmatrix}.
$$

So

$$
=\begin{bmatrix}4\\-2\end{bmatrix}
+\begin{bmatrix}3\\6\end{bmatrix}
=\begin{bmatrix}7\\4\end{bmatrix}.
$$

Same answer.

Two interpretations:

- **row view:** many dot products,
- **column view:** weighted combination of basis directions.

Both matter.

---

## 6. A Matrix Moves the Basis Vectors

Take the standard basis vectors:

$$
\mathbf{e}_1=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{e}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

Now multiply:

$$
W\mathbf{e}_1
=
\begin{bmatrix}2\\-1\end{bmatrix}
=\mathbf{c}_1
$$

and

$$
W\mathbf{e}_2
=
\begin{bmatrix}1\\2\end{bmatrix}
=\mathbf{c}_2.
$$

The columns of the matrix tell us exactly where the basis vectors go.

That means the entire transformation is encoded by where it sends the basis.

This is a profound compression of information.

---

## 7. Geometry: Transform the Whole Grid

Imagine the usual coordinate grid.

Before transformation:

```text
          e2 ↑
             |
             |
-------------+----→ e1
```

After applying $W$:

- $e_1$ moves to $(2,-1)$,
- $e_2$ moves to $(1,2)$.

Every other vector is built from these basis vectors, so every other vector follows automatically.

If

$$
\mathbf{x}=2\mathbf{e}_1+3\mathbf{e}_2,
$$

then linearity gives

$$
W\mathbf{x}
=2W\mathbf{e}_1+3W\mathbf{e}_2.
$$

This is why seeing the transformed basis tells us the transformed space.

---

## 8. Discover Linearity

A matrix transformation satisfies two key properties.

### Additivity

$$
W(\mathbf{u}+\mathbf{v})=W\mathbf{u}+W\mathbf{v}
$$

### Scaling

$$
W(c\mathbf{u})=cW\mathbf{u}
$$

Together:

$$
\boxed{
W(a\mathbf{u}+b\mathbf{v})
=aW\mathbf{u}+bW\mathbf{v}
}
$$

That property is why the operation is called **linear**.

A matrix does not arbitrarily bend space. It preserves linear combinations.

---

## 9. Example: Scaling

Take

$$
S=
\begin{bmatrix}
2 & 0\\
0 & 3
\end{bmatrix}.
$$

Then

$$
S\begin{bmatrix}x\\y\end{bmatrix}
=
\begin{bmatrix}2x\\3y\end{bmatrix}.
$$

So x-coordinates double and y-coordinates triple.

For

$$
\begin{bmatrix}1\\1\end{bmatrix}
$$

we get

$$
\begin{bmatrix}2\\3\end{bmatrix}.
$$

The matrix stretches space differently along different directions.

---

## 10. Example: Reflection

Take

$$
R=
\begin{bmatrix}
-1 & 0\\
0 & 1
\end{bmatrix}.
$$

Then

$$
R\begin{bmatrix}x\\y\end{bmatrix}
=
\begin{bmatrix}-x\\y\end{bmatrix}.
$$

The x-coordinate flips sign while y stays unchanged.

That reflects the plane across the y-axis.

---

## 11. Example: Shear

Take

$$
H=
\begin{bmatrix}
1 & 1\\
0 & 1
\end{bmatrix}.
$$

Then

$$
H\begin{bmatrix}x\\y\end{bmatrix}
=
\begin{bmatrix}x+y\\y\end{bmatrix}.
$$

The vertical coordinate stays fixed, while x shifts according to y.

A square becomes a slanted parallelogram.

This is a **shear**.

Stretch and shear are not the same:

- stretch changes size along a direction,
- shear slides one layer relative to another.

---

## 12. Example: Rotation

A 2D rotation by angle $\theta$ uses

$$
R(\theta)=
\begin{bmatrix}
\cos\theta & -\sin\theta\\
\sin\theta & \cos\theta
\end{bmatrix}.
$$

For $90^\circ$,

$$
\cos90^\circ=0,
\qquad
\sin90^\circ=1.
$$

So

$$
R=
\begin{bmatrix}
0 & -1\\
1 & 0
\end{bmatrix}.
$$

Apply it to

$$
\begin{bmatrix}1\\0\end{bmatrix}:
$$

$$
R\begin{bmatrix}1\\0\end{bmatrix}
=
\begin{bmatrix}0\\1\end{bmatrix}.
$$

The right-pointing basis vector becomes the up-pointing basis vector.

That is exactly a 90° counter-clockwise rotation.

---

## 13. Shape Reasoning Before Arithmetic

Suppose

$$
W\in\mathbb{R}^{4\times3}
$$

and

$$
\mathbf{x}\in\mathbb{R}^{3}.
$$

Then

$$
W\mathbf{x}\in\mathbb{R}^{4}.
$$

We know the output shape before multiplying a single number.

But if

$$
\mathbf{x}\in\mathbb{R}^{5},
$$

then

$$
(4\times3)(5)
$$

is invalid.

Why?

Each matrix row expects 3 numbers for its dot product, but x provides 5.

Shapes are not bookkeeping. They encode whether the mathematical operation exists.

---

## 14. Neural-Network Connection

A dense layer computes

$$
\mathbf{z}=W\mathbf{x}+\mathbf{b}.
$$

Suppose

$$
\mathbf{x}\in\mathbb{R}^{3}
$$

and we want 5 neurons.

Each neuron needs 3 weights.

So

$$
W\in\mathbb{R}^{5\times3}.
$$

Then

$$
W\mathbf{x}\in\mathbb{R}^{5}.
$$

Bias must also have 5 entries:

$$
\mathbf{b}\in\mathbb{R}^{5}.
$$

This is the matrix form of five neurons working at once.

---

## 15. Code From Scratch

```python
def matvec(W, x):
    out = []
    for row in W:
        total = 0.0
        for wi, xi in zip(row, x):
            total += wi * xi
        out.append(total)
    return out

W = [[2, 1], [-1, 2]]
x = [2, 3]

assert matvec(W, x) == [7, 4]
```

This explicit loop should be understood before using a library shortcut.

NumPy:

```python
import numpy as np

W = np.array([[2., 1.], [-1., 2.]])
x = np.array([2., 3.])

y = W @ x
assert np.allclose(y, [7., 4.])
```

The `@` operator means matrix multiplication.

---

## 16. Break It

### Wrong input dimension

A $(2\times3)$ matrix cannot multiply a 4-vector.

### Confusing rows and columns

If you store neuron weights as columns instead of rows, the expected multiplication changes.

### Treating `*` as matrix multiplication

In NumPy,

```python
W * x
```

means broadcasting / element-wise multiplication, not the same thing as

```python
W @ x
```

### Forgetting bias shape

If $W\mathbf{x}$ has shape `(5,)`, a bias intended to add one value per neuron should also align with `(5,)`.

---

## 17. History Lens — Linear Maps Before Neural Networks

Linear transformations were studied long before computers because they capture structured changes: rotations, projections, changes of coordinates, systems of equations and physical transformations.

Machine learning inherited this language because a neural-network layer faces the same structural problem: take one vector space, transform it into another, and do so efficiently.

The modern notation is new compared with ancient geometry, but the underlying question is old:

> How can we describe a transformation once and apply it everywhere consistently?

Matrices are the answer.

---

## 18. Distinctions That Matter

| Pair | Difference |
|---|---|
| matrix as data table vs matrix as transformation | storage view vs action view |
| row view vs column view | many dot products vs weighted combination of columns |
| element-wise multiply vs matrix multiply | local pairwise products vs structured dot products |
| stretch vs shear | scale along directions vs slide coordinates relative to each other |
| shape compatibility vs equal shape | matrix multiplication needs matching inner dimensions, not identical shapes |

---

## 19. What We Discovered

1. Matrix–vector multiplication computes many dot products at once.
2. Rows determine output coordinates.
3. Columns show where basis vectors go.
4. A matrix therefore encodes a transformation of space.
5. Linear transformations preserve addition and scalar multiplication.
6. Scaling, reflection, shear and rotation can all be expressed as matrices.
7. Neural-network dense layers are matrix transformations plus bias.
8. Shape reasoning can validate an operation before arithmetic begins.

---

## 20. One-Minute Explanation

A matrix multiplying a vector can be understood in two ways. Row by row, each matrix row takes a dot product with the input, producing one output number. Column by column, the input coordinates tell us how much of each matrix column to combine. Geometrically, the matrix tells us where the basis vectors move, which determines how the whole space transforms. This is why matrix multiplication powers dense neural-network layers: many neurons are simply many learned dot products computed together.

---

## 21. Mastery Check

1. Compute $[[2,1],[-1,2]] [2,3]^T$ by hand.
2. Explain the row interpretation.
3. Explain the column interpretation.
4. Why do matrix columns reveal transformed basis vectors?
5. What does it mean for a transformation to be linear?
6. What is the output shape of `(7×3) @ (3,)`?
7. Why is `(7×3) @ (4,)` invalid?
8. What geometric transformation does `diag(2,3)` perform?
9. Why is `W * x` not generally the same as `W @ x`?
10. How is a dense neural-network layer a matrix–vector multiplication?

---

## 🔭 Bridge to Chapter 010

A matrix can transform a vector.

But deep networks apply transformation after transformation:

$$
\mathbf{x}
\rightarrow A\mathbf{x}
\rightarrow B(A\mathbf{x}).
$$

Writing nested transformations works, but we want to know whether the sequence itself can be represented by one matrix.

> **Can two transformations be composed into one transformation?**

That question forces **matrix–matrix multiplication**.
