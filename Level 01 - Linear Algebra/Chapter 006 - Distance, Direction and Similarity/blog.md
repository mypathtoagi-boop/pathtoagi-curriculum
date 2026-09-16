# Chapter 006 — Distance, Direction and Similarity

> **The Big Question:** Once data becomes vectors, how can a machine tell whether two examples are close, far apart, moving in the same direction, or fundamentally different?

## Where We Are

Chapter 005 gave us a new way to represent one thing using many numbers at once.

A house with rooms and area could become

$$
\mathbf{x}=\begin{bmatrix}2\\800\end{bmatrix}.
$$

That solved a representation problem. But representation alone does not tell us how two vectors relate.

Suppose we have three houses:

$$
\mathbf{a}=\begin{bmatrix}2\\800\end{bmatrix},\qquad
\mathbf{b}=\begin{bmatrix}2\\900\end{bmatrix},\qquad
\mathbf{c}=\begin{bmatrix}5\\2200\end{bmatrix}.
$$

Which pair is most alike?

A human immediately says A and B. A machine needs a rule.

> **Today:** we discover length, distance, direction and similarity from geometry.
>
> **Next:** we need one operation that converts two vectors into one number measuring alignment: the dot product.

---

## 1. The Problem: “These Two Look Similar” Is Not Mathematics

Imagine a property website recommending similar houses.

The user opens House A:

| Feature | Value |
|---|---:|
| Rooms | 2 |
| Area | 800 sq ft |

The system must choose between B = (2 rooms, 900 sq ft) and C = (5 rooms, 2200 sq ft).

What did we do mentally when we said B is closer?

We compared differences.

$$
\mathbf{b}-\mathbf{a}
=
\begin{bmatrix}0\\100\end{bmatrix}
$$

while

$$
\mathbf{c}-\mathbf{a}
=
\begin{bmatrix}3\\1400\end{bmatrix}.
$$

The second change is much larger. So maybe “similar” means “the difference vector is small.”

But what does **small** mean for a vector?

---

## 2. What Would a Useful Distance Need?

A useful measure should satisfy common sense:

1. A point is zero distance from itself.
2. Distance is never negative.
3. Distance from A to B equals distance from B to A.
4. Moving farther away should increase the distance.
5. The rule should work in 2 dimensions, 200 dimensions or 2 million dimensions.

We already know the seed of the answer from school geometry: Pythagoras.

---

## 3. First Attempt: Add Coordinate Differences

Take

$$
\mathbf{p}=\begin{bmatrix}1\\4\end{bmatrix},\qquad
\mathbf{q}=\begin{bmatrix}4\\1\end{bmatrix}.
$$

Difference:

$$
\mathbf{q}-\mathbf{p}=\begin{bmatrix}3\\-3\end{bmatrix}.
$$

A tempting idea is to add the differences:

$$
3+(-3)=0.
$$

That would say the points are zero distance apart. They are not.

> ⚠️ **A Tempting Wrong Idea**
>
> Signed coordinate differences cancel for the same reason signed prediction errors cancelled in Chapter 001. A positive change in one coordinate can erase a negative change in another even though both contribute to separation.

So every coordinate must contribute by **size**, not by sign.

---

## 4. The Discovery: Pythagoras Becomes Vector Length

Consider

$$
\Delta=\begin{bmatrix}3\\4\end{bmatrix}.
$$

This means move 3 units horizontally and 4 vertically. The straight-line distance is the hypotenuse:

$$
3^2+4^2=5^2.
$$

Therefore

$$
\sqrt{3^2+4^2}=5.
$$

For a 2D vector

$$
\mathbf{v}=\begin{bmatrix}v_1\\v_2\end{bmatrix},
$$

its length is

$$
\|\mathbf{v}\|_2=\sqrt{v_1^2+v_2^2}.
$$

In $d$ dimensions nothing changes except the number of terms:

$$
\boxed{\|\mathbf{v}\|_2=\sqrt{\sum_{i=1}^{d}v_i^2}}
$$

Read it as:

> square every coordinate, add them, then take the square root.

This is the **Euclidean norm** or **L2 norm**.

| Level | Same idea |
|---|---|
| 💡 Intuition | length of an arrow |
| ✏️ Tiny numbers | $[3,4]$ has length 5 |
| 🎓 Abstraction | $\|\mathbf{v}\|_2=\sqrt{\sum_i v_i^2}$ |

---

## 5. Distance Is the Length of a Difference

The arrow from point $\mathbf{u}$ to point $\mathbf{v}$ is

$$
\mathbf{v}-\mathbf{u}.
$$

So Euclidean distance is

$$
\boxed{d(\mathbf{u},\mathbf{v})=\|\mathbf{u}-\mathbf{v}\|_2}
$$

Take

$$
\mathbf{u}=\begin{bmatrix}1\\2\end{bmatrix},\qquad
\mathbf{v}=\begin{bmatrix}4\\6\end{bmatrix}.
$$

Then

$$
\mathbf{v}-\mathbf{u}=\begin{bmatrix}3\\4\end{bmatrix}
$$

and

$$
d(\mathbf{u},\mathbf{v})=\sqrt{3^2+4^2}=5.
$$

Subtraction converts a two-point problem into a one-vector problem. This pattern will return everywhere in ML.

---

## 6. Geometry Before Formula Memorization

```text
 y
 ↑
 6|                 • v=(4,6)
 5|               / |
 4|             /   | 4
 3|           /     |
 2|   • u=(1,2)-----+
 1|          3
 0+--------------------------→ x
```

The direct path is 5.

Walking only along coordinate axes is 3 + 4 = 7.

That reveals something important: there is more than one reasonable notion of distance.

---

## 7. Euclidean Distance Is a Choice, Not a Law

If a taxi can only move along city blocks, the relevant distance is

$$
|4-1|+|6-2|=3+4=7.
$$

That is **Manhattan distance**:

$$
\boxed{\|\mathbf{x}\|_1=\sum_i |x_i|}
$$

Another metric, the L∞ norm, keeps only the largest coordinate difference:

$$
\boxed{\|\mathbf{x}\|_\infty=\max_i |x_i|}
$$

| Metric | Geometry | Formula |
|---|---|---|
| L1 | city blocks | $\sum_i \lvert x_i \rvert$ |
| L2 | straight line | $\sqrt{\sum_i x_i^2}$ |
| L∞ | largest single-axis change | $\max_i \lvert x_i \rvert$ |

> 🎯 **ML Connection** — choosing a distance metric is choosing what “near” means to the model.

---

## 8. The Hidden Trap: Units Change Geometry

Return to houses.

$$
\mathbf{a}=\begin{bmatrix}2\\800\end{bmatrix},\qquad
\mathbf{b}=\begin{bmatrix}3\\810\end{bmatrix}.
$$

Difference:

$$
\begin{bmatrix}1\\10\end{bmatrix}.
$$

Distance:

$$
\sqrt{1^2+10^2}=\sqrt{101}\approx10.05.
$$

Area contributes $100$ inside the square root; rooms contributes $1$.

But now record area in hundreds of square feet:

$$
\mathbf{a}=\begin{bmatrix}2\\8\end{bmatrix},\qquad
\mathbf{b}=\begin{bmatrix}3\\8.1\end{bmatrix}.
$$

Distance becomes

$$
\sqrt{1^2+0.1^2}\approx1.005.
$$

Same houses. Different geometry.

> ⚠️ **Common Mistake** — a mathematically valid distance can still be semantically misleading when feature scales are arbitrary.

This is why feature scaling matters.

---

## 9. Length Tells Size, Not Direction

Consider

$$
\mathbf{a}=\begin{bmatrix}3\\4\end{bmatrix},\qquad
\mathbf{b}=\begin{bmatrix}-3\\-4\end{bmatrix}.
$$

Both have length 5.

Yet they point in opposite directions.

So norm cannot answer every similarity question.

Sometimes we care not about **how large** a vector is, but **where it points**.

---

## 10. Unit Vectors: Remove Magnitude, Keep Direction

For

$$
\mathbf{v}=\begin{bmatrix}3\\4\end{bmatrix},
$$

we know

$$
\|\mathbf{v}\|=5.
$$

Divide by its own length:

$$
\hat{\mathbf{v}}=\frac{\mathbf{v}}{\|\mathbf{v}\|}
=
\begin{bmatrix}0.6\\0.8\end{bmatrix}.
$$

Now

$$
\|\hat{\mathbf{v}}\|
=
\sqrt{0.6^2+0.8^2}=1.
$$

A vector with length 1 is a **unit vector**.

Normalization removes magnitude and keeps direction.

> 💡 Think of shrinking or stretching every arrow until they all have equal length. What remains different is only where they point.

---

## 11. Direction Becomes an Angle

Two arrows can relate in three especially important ways:

```text
same direction       perpendicular        opposite direction
→ →                  ↑                    ← →
0°                   90°                  180°
```

If two vectors point the same way, their angle is $0^\circ$.

If they are perpendicular, it is $90^\circ$.

If they point opposite ways, it is $180^\circ$.

This lets similarity ignore overall scale.

For example,

$$
\begin{bmatrix}1\\2\end{bmatrix}
\quad\text{and}\quad
\begin{bmatrix}10\\20\end{bmatrix}
$$

are far apart in Euclidean distance but point in exactly the same direction.

---

## 12. Distance and Direction Answer Different Questions

Let

$$
\mathbf{a}=\begin{bmatrix}1\\1\end{bmatrix},\quad
\mathbf{b}=\begin{bmatrix}100\\100\end{bmatrix},\quad
\mathbf{c}=\begin{bmatrix}1\\2\end{bmatrix}.
$$

Distance says

$$
d(\mathbf{a},\mathbf{b})\approx140
$$

while

$$
d(\mathbf{a},\mathbf{c})=1.
$$

So C is much closer.

But A and B point in exactly the same direction, while A and C do not.

No contradiction exists.

> **Distance asks:** how far apart are the endpoints?
>
> **Directional similarity asks:** how similarly do the arrows point?

Modern embedding systems often care deeply about the second question.

---

## 13. The Operation We Are Missing

We want a single number that behaves like this:

```text
same direction     → high alignment
perpendicular      → zero alignment
opposite direction → negative alignment
```

Angles could express this, but computing angles directly is awkward.

Try multiplying matching coordinates and adding:

$$
\begin{bmatrix}1\\0\end{bmatrix}
\cdot
\begin{bmatrix}1\\0\end{bmatrix}=1
$$

while

$$
\begin{bmatrix}1\\0\end{bmatrix}
\cdot
\begin{bmatrix}0\\1\end{bmatrix}=0.
$$

And

$$
\begin{bmatrix}1\\0\end{bmatrix}
\cdot
\begin{bmatrix}-1\\0\end{bmatrix}=-1.
$$

Something remarkable is happening.

Multiplication of coordinates is recovering geometry.

That operation is the **dot product**.

We derive it properly in the next chapter.

---

## 14. Shape Check

If

$$
\mathbf{x},\mathbf{y}\in\mathbb{R}^5,
$$

then

```text
x                 shape (5,)
y                 shape (5,)
x - y             shape (5,)
||x - y||         scalar
x / ||x||         shape (5,)
```

The difference preserves dimension.

The norm collapses the vector into one scalar magnitude.

Normalization returns a vector of the same shape.

Shape reasoning will become one of our strongest debugging tools.

---

## 15. Code From Scratch

```python
import math

def euclidean_distance(a, b):
    squared_sum = 0.0
    for ai, bi in zip(a, b):
        squared_sum += (ai - bi) ** 2
    return math.sqrt(squared_sum)

assert euclidean_distance([1, 2], [4, 6]) == 5.0
```

Every code line mirrors a mathematical step:

1. subtract,
2. square,
3. add,
4. square root.

With NumPy:

```python
import numpy as np

a = np.array([1.0, 2.0])
b = np.array([4.0, 6.0])

assert np.isclose(np.linalg.norm(a - b), 5.0)
```

Shorter code, same mathematics.

---

## 16. Break It

### Failure 1 — Wrong feature scales
One coordinate can dominate simply because its units are numerically larger.

### Failure 2 — Missing values
A vector such as $[2,\ ?,\ 800]$ needs an explicit missing-data strategy.

### Failure 3 — Fake geometry from category IDs
Encoding neighborhoods as 1, 2, 3 does not make neighborhood 3 twice as far from 1 as neighborhood 2 is.

### Failure 4 — Zero vector normalization
For

$$
\mathbf{0}=\begin{bmatrix}0\\0\end{bmatrix},
$$

we have

$$
\|\mathbf{0}\|=0.
$$

So

$$
\frac{\mathbf{0}}{\|\mathbf{0}\|}
$$

would divide by zero.

Even a simple geometric operation has edge cases.

---

## 17. History Lens — Ancient Geometry, Modern Data

Pythagorean relationships were known long before modern machine learning. Euclid later organized geometry into a systematic deductive framework.

The modern shift is not that AI invented distance. It is that we now represent almost anything as a point in a high-dimensional space.

A sentence can be a point.

An image can be a point.

A user can be a point.

A molecule can be a point.

Once data becomes vectors, ancient geometry becomes a computational language for similarity.

---

## 18. Distinctions That Matter

| Pair | Difference |
|---|---|
| vector vs norm | vector has coordinates; norm is one scalar length |
| norm vs distance | norm measures one vector from origin; distance measures separation between two points |
| magnitude vs direction | magnitude is size; direction is orientation |
| distance vs similarity | distance asks closeness; similarity may ask alignment |
| normalization vs standardization | normalization here makes one vector length 1; standardization later uses dataset statistics |

---

## 19. What We Discovered

1. A difference vector tells us how to move from one point to another.
2. Euclidean norm generalizes Pythagorean length.
3. Euclidean distance is the norm of a difference vector.
4. Metrics are choices that encode geometry.
5. Feature scales can distort geometry.
6. Length captures magnitude but not direction.
7. Unit vectors remove magnitude while preserving direction.
8. Distance and directional similarity answer different questions.
9. Dot products are the bridge from coordinate arithmetic to alignment.

---

## 20. One-Minute Explanation

A vector can be viewed as a point or an arrow. To measure how far two points are apart, subtract their vectors and measure the length of the resulting arrow. Euclidean length comes from Pythagoras: square each coordinate, add them, and take the square root. But distance is only one notion of similarity. Two vectors can be far apart yet point in exactly the same direction. That is why machine learning also cares about angle and alignment. The next chapter discovers the dot product, which turns two vectors into one number that measures that alignment.

---

## 21. Mastery Check

1. Why can signed coordinate differences cancel incorrectly?
2. Derive the distance between $(1,2)$ and $(4,6)$.
3. What does $\|\mathbf{x}\|_2$ mean geometrically?
4. Why can feature scale distort nearest-neighbour search?
5. What information is removed by unit normalization?
6. Give two vectors that are far apart but point in the same direction.
7. Why are category IDs usually poor Euclidean coordinates?
8. What is special about normalizing the zero vector?

---

## 🔭 Bridge to Chapter 007

We can measure how long an arrow is and how far two points are apart.

We can even describe alignment using angles.

But we still need a computational shortcut that turns two vectors into one number and reveals their directional relationship.

> **How can coordinate multiplication reveal an angle?**

That question forces the **dot product**.
