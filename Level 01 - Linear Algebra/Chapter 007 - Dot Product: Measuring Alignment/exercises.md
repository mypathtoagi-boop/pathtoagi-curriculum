# Exercises — Chapter 007: Dot Product: Measuring Alignment

## Level 1 — Explain It

1. Explain the dot product without using the words “formula” or “cosine.”
2. Why does element-wise multiplication produce a vector while a dot product produces a scalar?
3. Explain why perpendicular vectors have dot product zero.
4. Explain why raw dot product is not the same as cosine similarity.
5. Explain why the zero vector has no meaningful cosine similarity.

## Level 2 — Calculate It

Compute by hand:

1. $[1,0]\cdot[1,0]$
2. $[1,0]\cdot[0,1]$
3. $[1,0]\cdot[-1,0]$
4. $[2,3]\cdot[4,5]$
5. $[2,3,4]\cdot[5,1,2]$

Then calculate cosine similarity for:

$$
\mathbf{a}=\begin{bmatrix}1\\2\end{bmatrix},
\qquad
\mathbf{b}=\begin{bmatrix}2\\4\end{bmatrix}.
$$

Predict the answer before calculating.

## Level 3 — Derive It

Starting from the law of cosines,

$$
\|\mathbf{a}-\mathbf{b}\|^2
=
\|\mathbf{a}\|^2+
\|\mathbf{b}\|^2-
2\|\mathbf{a}\|\|\mathbf{b}\|\cos\theta,
$$

derive

$$
\mathbf{a}\cdot\mathbf{b}
=
\|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

Do not skip the expansion of $(\mathbf{a}-\mathbf{b})\cdot(\mathbf{a}-\mathbf{b})$.

## Level 4 — Predict It

Without running code:

```python
import numpy as np

a = np.array([1.0, 2.0, 3.0])
b = np.array([4.0, 5.0, 6.0])
```

Predict:

1. `a.shape`
2. `(a * b).shape`
3. `np.dot(a, b)`
4. the shape of `np.dot(a, b)`
5. whether cosine similarity changes if `b` is replaced by `10*b`

Then run and explain.

## Level 5 — Break It

### Experiment A — Inflate magnitude

Take two pairs of vectors with the same angle but very different magnitudes. Show that raw dot products differ even though cosine similarity is the same.

### Experiment B — Opposite direction

Create vectors with cosine similarity `-1`. Explain what this means geometrically.

### Experiment C — Zero vector

Try to compute cosine similarity with `[0,0]`. Your implementation must fail deliberately with a useful message rather than silently returning nonsense.

### Experiment D — Shape mismatch

Try to dot `[1,2,3]` with `[4,5]`. Explain why this is not merely a library limitation.

## Level 6 — Build It

Implement from scratch:

```python
def dot(a, b):
    pass


def norm(v):
    pass


def cosine_similarity(a, b):
    pass
```

Requirements:

- verify `dot([2,3,4], [5,1,2]) == 21`
- compare your `dot` against `np.dot` on 100 random pairs
- compare your cosine similarity against a NumPy implementation
- handle the zero-vector case explicitly

## Investigation — Embedding Similarity

Create five small 3D vectors representing mock documents. Make two vectors point in nearly the same direction but have very different magnitudes.

For a chosen query vector, rank all documents by:

1. Euclidean distance
2. raw dot product
3. cosine similarity

Then answer:

- Which ranking changes the most?
- Which metric cares about magnitude?
- Which metric mostly cares about direction?
- Can the nearest vector by Euclidean distance fail to have the highest cosine similarity?
- Which metric would you choose for normalized text embeddings, and why?

## Shape Check

Fill in before running anything:

```text
x ∈ R^7              → shape ______
w ∈ R^7              → shape ______
x * w                 → shape ______
sum(x * w)            → shape ______
x · w                 → shape ______
```

## Mastery Gate

Do not move on until you can answer without notes:

- What are the coordinate and geometric definitions of the dot product?
- What does its sign tell you?
- Why does magnitude affect raw dot product?
- How does cosine similarity remove magnitude?
- Why can a linear neuron be written as `w · x + b`?
- Why is matrix-vector multiplication naturally “many dot products at once”?
