# Exercises — Chapter 009: Matrix–Vector Multiplication as Transformation

## Level 1 — Explain It

1. Explain matrix–vector multiplication using the **row view**.
2. Explain the same multiplication using the **column view**.
3. Why do the columns of a matrix reveal where the basis vectors go?
4. Why is matrix multiplication not the same as element-wise multiplication?
5. Explain why a dense neural-network layer naturally uses a matrix.

## Level 2 — Calculate It

Compute by hand:

$$
\begin{bmatrix}
2 & 1\\
-1 & 2
\end{bmatrix}
\begin{bmatrix}
2\\3
\end{bmatrix}.
$$

Then compute:

$$
\begin{bmatrix}
1 & 0\\
0 & 2
\end{bmatrix}
\begin{bmatrix}
3\\4
\end{bmatrix}.
$$

Explain the geometry of the second transformation.

## Level 3 — Derive It

Let

$$
W=
\begin{bmatrix}
| & |\\
\mathbf{c}_1 & \mathbf{c}_2\\
| & |
\end{bmatrix}
$$

and

$$
\mathbf{x}=\begin{bmatrix}x_1\\x_2\end{bmatrix}.
$$

Derive

$$
W\mathbf{x}=x_1\mathbf{c}_1+x_2\mathbf{c}_2.
$$

Then explain why this means a matrix transformation is completely determined by the transformed basis vectors.

## Level 4 — Predict It

Without running code, predict the result or shape:

```python
import numpy as np

W = np.array([
    [1., 2., 3.],
    [4., 5., 6.]
])
x = np.array([10., 20., 30.])
```

Predict:

1. `W.shape`
2. `x.shape`
3. `(W @ x).shape`
4. the numeric value of `W @ x`
5. whether `W * x` has the same meaning as `W @ x`

## Level 5 — Break It

### Experiment A — Shape mismatch

Try multiplying a `(4,3)` matrix by a vector of shape `(4,)`.

Explain the error in terms of dot products, not library rules.

### Experiment B — `*` versus `@`

Use the same matrix and vector with both operators. Inspect shapes and values. Explain why the operations answer different questions.

### Experiment C — Bad bias shape

Create a dense layer with 5 outputs. Intentionally create a bias vector of length 3. Explain why the addition is conceptually wrong.

## Level 6 — Build It

Implement matrix–vector multiplication from scratch:

```python
def matvec(W, x):
    pass
```

Requirements:

- do not use NumPy inside the implementation
- verify the result against `np.array(W) @ np.array(x)`
- test at least 100 random compatible shapes
- detect incompatible shapes and raise a useful error

## Investigation — Transform a Grid

Create a 2D grid of points and apply these matrices:

### Stretch

$$
\begin{bmatrix}2&0\\0&0.5\end{bmatrix}
$$

### Reflection

$$
\begin{bmatrix}-1&0\\0&1\end{bmatrix}
$$

### Shear

$$
\begin{bmatrix}1&1\\0&1\end{bmatrix}
$$

### Rotation by 90°

$$
\begin{bmatrix}0&-1\\1&0\end{bmatrix}
$$

For each:

1. predict where $e_1$ and $e_2$ will go
2. predict what happens to a square
3. plot the transformed grid
4. compare prediction with result

## Shape Check

Fill in before running:

```text
W ∈ R^(6×4)          → shape ______
x ∈ R^4              → shape ______
W @ x                → shape ______
b                    → natural shape ______
W @ x + b            → shape ______
```

## Mastery Gate

Do not continue until you can answer:

- Why is each output coordinate a dot product?
- Why do columns encode transformed basis vectors?
- What does linearity mean algebraically?
- What is the difference between stretch, shear and rotation?
- Why do inner dimensions have to match?
- Why is a dense neural-network layer naturally written as `W @ x + b`?
