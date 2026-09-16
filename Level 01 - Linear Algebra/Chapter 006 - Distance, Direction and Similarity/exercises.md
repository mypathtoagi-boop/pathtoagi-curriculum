# Exercises — Chapter 006: Distance, Direction and Similarity

These exercises follow the PathToAGI six-level ladder. Do them in order. The point is not to memorize formulas; it is to make geometry feel inevitable.

## Level 1 — Explain It

1. Explain in plain English why the vector $[3,-3]$ does **not** represent zero movement even though its coordinates sum to zero.
2. Explain the difference between a vector's **length** and its **direction**.
3. Explain why two vectors can be very far apart but still point in exactly the same direction.
4. Explain why recording area in square feet versus hundreds of square feet can change Euclidean distance.
5. Explain why a neighborhood ID such as `1`, `2`, `3` usually should not be treated as geometric distance.

## Level 2 — Calculate It

For each pair, calculate the Euclidean distance completely by hand.

1. $(0,0)$ and $(3,4)$
2. $(1,2)$ and $(4,6)$
3. $(-1,2)$ and $(2,-2)$
4. $[1,1,1]$ and $[4,5,1]$

Then calculate both L1 and L2 distance between

$$
\mathbf{a}=\begin{bmatrix}1\\2\end{bmatrix},
\qquad
\mathbf{b}=\begin{bmatrix}4\\6\end{bmatrix}.
$$

Explain why the answers differ.

## Level 3 — Predict It

Without running code, predict the result or shape.

```python
import numpy as np

x = np.array([3.0, 4.0])
y = np.array([6.0, 8.0])
```

Predict:

1. `x.shape`
2. `np.linalg.norm(x)`
3. `np.linalg.norm(y)`
4. `np.linalg.norm(y - x)`
5. `(x / np.linalg.norm(x)).shape`
6. Whether `x / ||x||` and `y / ||y||` are equal.

Only then run the code.

## Level 4 — Break It

### Experiment A — Scale destroys nearest-neighbour intuition

Create three houses using `[rooms, area]`.

First represent area in square feet.

Then divide area by 100 and calculate the same distances again.

Question:

> Can the nearest neighbour change even though the real houses did not change?

If yes, explain exactly why.

### Experiment B — Normalize the zero vector

Try:

```python
z = np.array([0.0, 0.0])
z / np.linalg.norm(z)
```

Predict what should happen before running it.

Explain why this is a mathematical edge case rather than merely a NumPy problem.

### Experiment C — Fake category geometry

Encode three cities as

```text
Hyderabad = 1
Ranchi    = 2
Delhi     = 10
```

Compute numeric distances.

Then explain why those distances say nothing reliable about geographic, cultural or semantic similarity.

## Level 5 — Build It

Implement these functions **without** `np.linalg.norm`:

```python
def l1_distance(a, b):
    pass


def l2_distance(a, b):
    pass


def normalize(v):
    pass
```

Requirements:

- `l1_distance([1,2], [4,6]) == 7`
- `l2_distance([1,2], [4,6]) == 5`
- `normalize([3,4])` should be approximately `[0.6, 0.8]`
- your `normalize` function must explicitly handle the zero vector

Then verify your L2 implementation against NumPy on at least 100 randomly generated vector pairs.

## Level 6 — Investigate It

### Investigation: Which metric changes the neighbourhood?

Generate 30 random 2D points.

Choose one query point.

For every other point, compute:

- L1 distance
- L2 distance
- L∞ distance

Find the five nearest neighbours under each metric.

Then answer:

1. Are the neighbour sets identical?
2. Which points change rank most dramatically?
3. Draw the points and explain geometrically why the metric changes the result.
4. Which metric would make sense for a taxi moving on a grid?
5. Which metric would make sense for straight-line physical distance?
6. Can you invent a real problem where L∞ is the natural choice?

## Shape Check

Fill in the missing shapes before touching Python.

```text
x ∈ R^8                      → shape ______
y ∈ R^8                      → shape ______
x - y                        → shape ______
||x - y||                    → shape ______
x / ||x||                    → shape ______
```

Explain why the norm changes the type of object from vector to scalar.

## Mastery Gate

Do not move to Chapter 007 until you can answer all of these without notes:

- What is the geometric meaning of subtraction between two vectors?
- How does Pythagoras become the L2 norm?
- Why is Euclidean distance not the only valid metric?
- Why can feature units distort geometry?
- What does unit normalization preserve and what does it destroy?
- Why is distance not the same thing as directional similarity?
- Why do we now need the dot product?
