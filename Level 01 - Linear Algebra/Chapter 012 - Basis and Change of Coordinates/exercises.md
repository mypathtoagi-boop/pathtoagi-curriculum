# Exercises — Chapter 012: Basis and Change of Coordinates

## Level 1 — Explain It

1. Explain why a vector is not the same thing as its coordinate list.
2. Explain what a basis does.
3. Explain why the same geometric vector can have different coordinates.
4. Explain the difference between rotating a vector and rotating the coordinate system.
5. Explain why orthonormal bases are especially convenient.

## Level 2 — Calculate It

Use

$$
\mathbf{b}_1=\begin{bmatrix}1\\1\end{bmatrix},
\qquad
\mathbf{b}_2=\begin{bmatrix}1\\-1\end{bmatrix}.
$$

Find the coordinates of

$$
\mathbf{x}=\begin{bmatrix}6\\2\end{bmatrix}
$$

in basis $B$.

Then reconstruct $\mathbf{x}$ from those coordinates.

## Level 3 — Derive It

Let

$$
B=[\mathbf{b}_1\ \mathbf{b}_2].
$$

Show why

$$
B\mathbf{c}=c_1\mathbf{b}_1+c_2\mathbf{b}_2.
$$

Then derive

$$
\mathbf{c}=B^{-1}\mathbf{x}
$$

from

$$
B\mathbf{c}=\mathbf{x}.
$$

## Level 4 — Predict It

Without running code:

```python
import numpy as np

B = np.array([[1., 1.],
              [1., -1.]])
x = np.array([4., 2.])
```

Predict:

1. `B.shape`
2. `x.shape`
3. the basis coordinates of `x`
4. the shape of `np.linalg.solve(B, x)`
5. whether `B @ coords` reconstructs `x`

## Level 5 — Break It

### Experiment A — Dependent basis

Use

$$
B=\begin{bmatrix}1&2\\1&2\end{bmatrix}.
$$

Try to solve for coordinates of several vectors.

Explain geometrically why the failure occurs.

### Experiment B — Poorly conditioned basis

Choose two basis vectors that are almost parallel, such as

$$
[1,1]
$$

and

$$
[1.0001,1].
$$

Investigate how small changes in the target vector affect coordinates.

Explain why a valid basis can still be numerically awkward.

## Level 6 — Build It

Implement a function that converts basis coordinates to standard coordinates using only loops and arithmetic.

Then implement the reverse transformation for a 2D basis by solving the two equations manually.

Compare both against NumPy.

## Investigation — Rotate the Coordinate System

Choose several 2D points.

Create an orthonormal basis rotated by $45^\circ$.

For every point:

1. compute standard coordinates,
2. compute rotated-basis coordinates,
3. reconstruct the original point,
4. plot both coordinate systems,
5. explain which quantities changed and which geometric object stayed fixed.

## Shape Check

```text
B ∈ R^(5×5)           → shape ______
c ∈ R^5               → shape ______
B @ c                  → shape ______
B^-1 @ x               → shape ______
```

## Mastery Gate

Do not continue until you can answer:

- What is a basis?
- Why are coordinates basis-dependent?
- Why are basis vectors stored as columns?
- What does the inverse basis matrix do?
- What makes an orthonormal basis special?
- Why do dependent vectors fail as a basis?
- How does basis change prepare us for PCA and eigenvectors?
