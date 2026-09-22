# Exercises — Chapter 010: Matrix–Matrix Multiplication as Composition

## Level 1 — Explain It

1. Explain why matrix multiplication represents composition of transformations.
2. In $BA\mathbf{x}$, why does $A$ act first?
3. Explain why matrix multiplication is generally not commutative.
4. Explain the difference between associativity and commutativity.
5. Explain why two purely linear neural-network layers can collapse into one.

## Level 2 — Calculate It

Let

$$
A=\begin{bmatrix}2&0\\0&1\end{bmatrix},
\qquad
B=\begin{bmatrix}1&1\\0&1\end{bmatrix}.
$$

Calculate both

$$
BA
$$

and

$$
AB.
$$

Then apply both products to

$$
\mathbf{x}=\begin{bmatrix}1\\2\end{bmatrix}.
$$

Explain the geometric difference.

## Level 3 — Derive It

Starting from

$$
A=\begin{bmatrix}a&b\\c&d\end{bmatrix},
\qquad
B=\begin{bmatrix}e&f\\g&h\end{bmatrix},
$$

derive the full formula for $BA$ by transforming the columns of $A$ with $B$.

Then show that every entry can be written as a row–column dot product.

## Level 4 — Predict It

Without running code, predict output shapes:

```text
(4×3) @ (3×2)  → ______
(8×5) @ (5×7)  → ______
(2×6) @ (6×1)  → ______
(3×4) @ (5×2)  → valid / invalid?
```

Then explain the inner-dimension rule in plain English.

## Level 5 — Break It

### Experiment A — Reverse the order

Choose a rotation matrix and a non-uniform stretch matrix. Apply them in both orders to the same set of points. Plot the results and explain why they differ.

### Experiment B — Element-wise confusion

Compare `A * B` with `A @ B` in NumPy. Explain why identical shapes do not make the operations equivalent.

### Experiment C — Singular transformation

Use

$$
S=\begin{bmatrix}1&0\\0&0\end{bmatrix}.
$$

Apply it to several points. Explain what information is lost and why no inverse can recover the original y-coordinate.

## Level 6 — Build It

Implement matrix multiplication from scratch:

```python
def matmul(A, B):
    pass
```

Requirements:

- use explicit `i`, `j`, `k` loops
- validate inner dimensions
- compare against NumPy on at least 100 random compatible matrix pairs
- test a few incompatible shapes and raise a useful error

## Investigation — Linear Layers Collapse

Create two random matrices $W_1$ and $W_2$ and a random vector $x$.

Verify numerically:

$$
W_2(W_1x)=(W_2W_1)x.
$$

Then insert ReLU between them:

$$
W_2\operatorname{ReLU}(W_1x).
$$

Investigate whether a single fixed matrix can reproduce that mapping for many random inputs.

Explain what the experiment suggests about the role of nonlinearity.

## Shape Check

Fill in:

```text
A ∈ R^(5×3)
B ∈ R^(3×8)
A @ B → shape ______

W1 ∈ R^(4×6)
x  ∈ R^6
W1 @ x → shape ______

W2 ∈ R^(2×4)
W2 @ (W1 @ x) → shape ______
W2 @ W1 → shape ______
```

## Mastery Gate

Do not continue until you can answer:

- Why is matrix multiplication a composition rule?
- Why does the rightmost matrix act first?
- Why must inner dimensions match?
- Why is order generally important?
- Why is multiplication associative?
- What does an inverse mean geometrically?
- Why can linear layers without activations collapse into one layer?
