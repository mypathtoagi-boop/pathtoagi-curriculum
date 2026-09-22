# Chapter 007 — Dot Product: Measuring Alignment

> **The Big Question:** How can multiplying coordinates tell us whether two vectors point in the same direction?

## Where We Are

Chapter 006 left a debt.

It measured with lengths, compared with directions, and then leaned on one fact it never proved — that in an orthonormal basis, a coordinate is simply $\mathbf{x}\cdot\mathbf{u}_i$. Chapter 005 leaned on the same fact to build cosine similarity.

Both times we said: *take it on trust, we will prove it later.* This is later.

Two questions have been running side by side:

Distance asks:

> How far apart are two points?

Direction asks:

> How similarly do two arrows point?

We saw that

$$
\begin{bmatrix}1\\2\end{bmatrix}
\quad\text{and}\quad
\begin{bmatrix}10\\20\end{bmatrix}
$$

are far apart but point exactly the same way.

So Euclidean distance cannot answer every similarity question.

We need an operation that turns two vectors into one number and somehow captures **alignment**.

That operation is the dot product.

But rather than memorize it, we will force it to appear.

---


## The Picture to Hold in Your Head

The dot product turns two arrows into **one score of agreement**. Positive means they lean together, zero means they are perpendicular, and negative means they lean against each other. That already explains a linear classifier: it asks whether an input lies with or against a learned direction.

There are three equally useful pictures of the same number. Algebraically it is **pair, multiply, add**. Geometrically it is a **projection or shadow**. After dividing out both lengths it becomes **cosine similarity**, a pure measure of angle.

This is why the dot product appears everywhere in AI. A neuron scores an input against a weight vector. Retrieval compares embeddings. Attention scores a query against keys. The surface application changes; the underlying question stays the same: **how aligned are these two directions?**

> 🎛️ Use **The sign**, **The shadow**, and **Only the angle** as three views of one operation. If those scenes feel like three unrelated tricks, stay here until you can explain why they must agree.

## 1. Start With the Easiest Directions Possible

Take the unit vector pointing right:

$$
\mathbf{e}_x=\begin{bmatrix}1\\0\end{bmatrix}.
$$

Compare it with itself.

Whatever our alignment score is, this should be strongly positive.

Now compare it with the unit vector pointing up:

$$
\mathbf{e}_y=\begin{bmatrix}0\\1\end{bmatrix}.
$$

These directions are perpendicular.

A natural alignment score should be neutral.

Finally compare right with left:

$$
-\mathbf{e}_x=\begin{bmatrix}-1\\0\end{bmatrix}.
$$

These point opposite ways.

A useful alignment score should become negative.

So we want something like:

```text
same direction       → positive
perpendicular        → zero
opposite direction   → negative
```

---

## 2. First Attempt: Add Coordinates

For

$$
\mathbf{a}=\begin{bmatrix}1\\0\end{bmatrix},
\qquad
\mathbf{b}=\begin{bmatrix}0\\1\end{bmatrix},
$$

adding coordinates gives

$$
(1+0)+(0+1)=2.
$$

But the vectors are perpendicular.

The score should be neutral, not strongly positive.

So addition does not capture alignment.

What does?

Try pairing corresponding coordinates and multiplying.

---

## 3. The Discovery: Pair, Multiply, Add

For two vectors

$$
\mathbf{a}=\begin{bmatrix}a_1\\a_2\end{bmatrix},
\qquad
\mathbf{b}=\begin{bmatrix}b_1\\b_2\end{bmatrix},
$$

compute

$$
a_1b_1+a_2b_2.
$$

Test the three direction cases.

Same direction:

$$
\begin{bmatrix}1\\0\end{bmatrix} \cdot \begin{bmatrix}1\\0\end{bmatrix} = 1(1)+0(0)=1.
$$

Perpendicular:

$$
\begin{bmatrix}1\\0\end{bmatrix} \cdot \begin{bmatrix}0\\1\end{bmatrix} = 1(0)+0(1)=0.
$$

Opposite:

$$
\begin{bmatrix}1\\0\end{bmatrix} \cdot \begin{bmatrix}-1\\0\end{bmatrix} = 1(-1)+0(0)=-1.
$$

Exactly the behavior we wanted.

This operation is the **dot product**:

$$
\boxed{ \mathbf{a}\cdot\mathbf{b} = \sum_{i=1}^{d} a_i b_i }
$$

Read it as:

> multiply matching coordinates, then add all the products.

---

## 4. Why “Dot” Product?

The notation

$$
\mathbf{a}\cdot\mathbf{b}
$$

uses a centered dot between the vectors.

It is not ordinary scalar multiplication, because each object contains several numbers.

It is also not element-wise multiplication.

Element-wise multiplication produces another vector:

$$
\begin{bmatrix}2\\3\end{bmatrix} \odot \begin{bmatrix}4\\5\end{bmatrix} = \begin{bmatrix}8\\15\end{bmatrix}.
$$

The dot product goes one step further and adds those results:

$$
2(4)+3(5)=8+15=23.
$$

So:

```text
vector + vector  → scalar
```

That shape change is part of the meaning.

---

## 5. Tiny Worked Example

Let

$$
\mathbf{x}=\begin{bmatrix}2\\3\\4\end{bmatrix},
\qquad
\mathbf{w}=\begin{bmatrix}5\\1\\2\end{bmatrix}.
$$

Then

$$
\mathbf{w}\cdot\mathbf{x} = 5(2)+1(3)+2(4).
$$

Compute term by term:

$$
10+3+8=21.
$$

One scalar comes out.

This exact operation is why a neuron can take many inputs and produce one pre-activation value.

---

## 6. The Machine-Learning Meaning: A Weighted Vote

Suppose a house is represented by

$$
\mathbf{x}=\begin{bmatrix} \text{rooms}\\ \text{area in hundreds of sq ft} \end{bmatrix} = \begin{bmatrix}3\\10\end{bmatrix}.
$$

Suppose weights are

$$
\mathbf{w}=\begin{bmatrix}2\\0.5\end{bmatrix}.
$$

Then

$$
\mathbf{w}\cdot\mathbf{x} = 2(3)+0.5(10) =6+5 =11.
$$

Each feature contributes a vote:

```text
rooms contribution = 6
area contribution  = 5
---------------------
total               = 11
```

> 💡 **Intuition** — A dot product is a weighted vote. Each input contributes according to both its value and its weight.

Add a bias $b$ and the familiar linear model appears:

$$
\hat y=\mathbf{w}\cdot\mathbf{x}+b.
$$

The model from Chapter 001 was already hiding a dot product in one dimension.

---

## 7. But Why Does This Measure an Angle?

So far the dot product behaves correctly in special cases.

Now we connect it to geometry.

Take two vectors $\mathbf{a}$ and $\mathbf{b}$ with angle $\theta$ between them.

Consider the triangle formed by

$$
\mathbf{a},\quad \mathbf{b},\quad \mathbf{a}-\mathbf{b}.
$$

The law of cosines says

$$
\|\mathbf{a}-\mathbf{b}\|^2 = \|\mathbf{a}\|^2+ \|\mathbf{b}\|^2- 2\|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

Now expand the left side using coordinates.

---

## 8. Derive the Geometric Dot-Product Formula

Start with

$$
\|\mathbf{a}-\mathbf{b}\|^2.
$$

Because norm squared is a vector dotted with itself,

$$
\|\mathbf{a}-\mathbf{b}\|^2 = (\mathbf{a}-\mathbf{b})\cdot(\mathbf{a}-\mathbf{b}).
$$

Expand:

$$
=\mathbf{a}\cdot\mathbf{a}
-2\mathbf{a}\cdot\mathbf{b}
+\mathbf{b}\cdot\mathbf{b}.
$$

But

$$
\mathbf{a}\cdot\mathbf{a}=\|\mathbf{a}\|^2
$$

and

$$
\mathbf{b}\cdot\mathbf{b}=\|\mathbf{b}\|^2.
$$

So

$$
\|\mathbf{a}-\mathbf{b}\|^2 = \|\mathbf{a}\|^2+ \|\mathbf{b}\|^2- 2\mathbf{a}\cdot\mathbf{b}.
$$

Compare with the law of cosines:

$$
\|\mathbf{a}-\mathbf{b}\|^2 = \|\mathbf{a}\|^2+ \|\mathbf{b}\|^2- 2\|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

The first two terms match.

Therefore the remaining terms must match:

$$
2\mathbf{a}\cdot\mathbf{b} = 2\|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

Cancel 2:

$$
\boxed{ \mathbf{a}\cdot\mathbf{b} = \|\mathbf{a}\|\|\mathbf{b}\|\cos\theta }
$$

This is the deep connection.

The coordinate formula and the geometric formula are the same operation viewed from two worlds.

---

## 9. What the Sign Means

Since vector lengths are non-negative, the sign of the dot product comes from

$$
\cos\theta.
$$

Recall:

$$
\cos 0^\circ=1,
$$

$$
\cos 90^\circ=0,
$$

$$
\cos 180^\circ=-1.
$$

Therefore:

| Angle | Dot product | Meaning |
|---:|---:|---|
| less than $90^\circ$ | positive | generally aligned |
| exactly $90^\circ$ | zero | perpendicular |
| greater than $90^\circ$ | negative | generally opposed |

The sign is geometric information.

---

## 10. Magnitude Still Matters

A common misunderstanding is:

> “The dot product is only angle.”

Not quite.

The formula is

$$
\mathbf{a}\cdot\mathbf{b} = \|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

So it depends on three things:

1. length of $\mathbf{a}$,
2. length of $\mathbf{b}$,
3. angle between them.

Take

$$
\mathbf{a}=\begin{bmatrix}1\\0\end{bmatrix}
$$

and

$$
\mathbf{b}=\begin{bmatrix}1\\0\end{bmatrix}.
$$

Dot product is 1.

Now scale both by 10:

$$
\mathbf{a}'=\begin{bmatrix}10\\0\end{bmatrix},
\qquad
\mathbf{b}'=\begin{bmatrix}10\\0\end{bmatrix}.
$$

Same angle, but

$$
\mathbf{a}'\cdot\mathbf{b}'=100.
$$

The direction did not change. Magnitude did.

So raw dot product is **alignment weighted by magnitude**.

---

## 11. Cosine Similarity Removes Magnitude

If we want direction only, divide out the lengths:

$$
\cos\theta = \frac{\mathbf{a}\cdot\mathbf{b}} {\|\mathbf{a}\|\|\mathbf{b}\|}.
$$

This is **cosine similarity**:

$$
\boxed{ \text{cosine similarity} = \frac{\mathbf{a}\cdot\mathbf{b}} {\|\mathbf{a}\|\|\mathbf{b}\|} }
$$

For nonzero vectors its value lies between -1 and 1.

```text
+1  → same direction
 0  → perpendicular
-1  → opposite direction
```

This is why embeddings are often compared with cosine similarity.

---

## 12. Example: Two Documents

Pretend two numbers measure how strongly a document talks about:

1. machine learning,
2. cooking.

Document A:

$$
\mathbf{a}=\begin{bmatrix}8\\1\end{bmatrix}.
$$

Document B:

$$
\mathbf{b}=\begin{bmatrix}4\\0.5\end{bmatrix}.
$$

Document C:

$$
\mathbf{c}=\begin{bmatrix}1\\8\end{bmatrix}.
$$

B is exactly half of A.

So A and B point in the same direction even though their magnitudes differ.

Their cosine similarity is 1.

C points mostly toward cooking, so its angle with A is much larger.

A recommendation system may care more about this direction than about raw vector size.

---

## 13. Projection: How Much of One Vector Lies Along Another?

Suppose $\mathbf{u}$ is a unit vector.

Then

$$
\mathbf{x}\cdot\mathbf{u}
$$

measures the signed amount of $\mathbf{x}$ in direction $\mathbf{u}$.

Why?

Because

$$
\mathbf{x}\cdot\mathbf{u} = \|\mathbf{x}\|\|\mathbf{u}\|\cos\theta.
$$

Since

$$
\|\mathbf{u}\|=1,
$$

we get

$$
\mathbf{x}\cdot\mathbf{u} = \|\mathbf{x}\|\cos\theta.
$$

That is exactly the scalar projection.

This idea will later reappear in:

- PCA,
- attention,
- linear layers,
- embeddings,
- orthogonal decompositions.

---

## 14. Shape Check

Let

$$
\mathbf{x}\in\mathbb{R}^d,
\qquad
\mathbf{w}\in\mathbb{R}^d.
$$

Then

```text
x                shape (d,)
w                shape (d,)
w * x elementwise shape (d,)
sum(w * x)       scalar
w · x             scalar
```

This is why a single neuron can map many features to one number.

The dot product reduces dimension.

---

## 15. From One Neuron to Many

Suppose one neuron has weights

$$
\mathbf{w}_1
$$

and another has

$$
\mathbf{w}_2.
$$

Each computes

$$
\mathbf{w}_i\cdot\mathbf{x}.
$$

Stack the weight vectors as rows:

$$
W=
\begin{bmatrix}
---\mathbf{w}_1^T---\\
---\mathbf{w}_2^T---
\end{bmatrix}.
$$

Then both dot products can be computed together as

$$
W\mathbf{x}.
$$

That is our bridge to matrices.

A matrix-vector multiplication is many dot products at once.

---

## 16. Code From Scratch

```python
def dot(a, b):
    total = 0.0
    for ai, bi in zip(a, b):
        total += ai * bi
    return total

assert dot([2, 3, 4], [5, 1, 2]) == 21
```

NumPy expresses the same operation directly:

```python
import numpy as np

a = np.array([2.0, 3.0, 4.0])
b = np.array([5.0, 1.0, 2.0])

assert np.isclose(np.dot(a, b), 21.0)
```

Again: library convenience comes **after** mathematical understanding.

---

## 17. Cosine Similarity From Scratch

```python
import math

def norm(v):
    return math.sqrt(sum(x*x for x in v))


def cosine_similarity(a, b):
    denom = norm(a) * norm(b)
    if denom == 0:
        raise ValueError("cosine similarity is undefined for a zero vector")
    return dot(a, b) / denom
```

The zero-vector check matters.

A zero vector has no direction.

So asking for its angle with another vector is not meaningful.

---

## 18. Break It

### Failure 1 — Different shapes

A dot product requires matching dimensions.

$$
[1,2,3]\cdot[4,5]
$$

has no ordinary dot-product meaning.

### Failure 2 — Forgetting magnitude

A large raw dot product does not necessarily mean a smaller angle. Large vector norms can inflate it.

### Failure 3 — Zero vector cosine similarity

Cosine similarity divides by

$$
\|\mathbf{a}\|\|\mathbf{b}\|.
$$

If either norm is zero, the denominator is zero.

### Failure 4 — Assuming cosine similarity is always non-negative

If vectors point in opposing directions, cosine similarity is negative.

---

## 19. 🎯 Machine-Learning Connection — Attention

Much later, Transformers will compare a **query** vector with **key** vectors.

At the heart of that comparison is a dot product:

$$
\text{score}=\mathbf{q}\cdot\mathbf{k}.
$$

Why?

Because dot products measure compatibility / alignment.

Attention is not a disconnected trick.

It is this chapter, scaled up.

---

## 20. 🎯 Machine-Learning Connection — Linear Classifiers

A classifier may compute

$$
z=\mathbf{w}\cdot\mathbf{x}+b.
$$

Geometrically, $\mathbf{w}$ defines a direction.

The dot product tells us how much $\mathbf{x}$ points along that learned direction.

So weights are not merely arbitrary coefficients.

They define geometry in feature space.

---

## 21. History Lens — From Geometry to Vector Analysis

The modern dot product emerged from the development of vector analysis in the nineteenth century, particularly through work associated with Gibbs and Heaviside, building on earlier geometric and algebraic traditions.

Its power comes from unifying two descriptions:

- coordinate arithmetic: multiply matching entries and add,
- geometry: lengths times cosine of the angle.

That bridge is exactly why the dot product became foundational in physics, engineering and machine learning.

---

## 22. Distinctions That Matter

| Pair | Difference |
|---|---|
| element-wise product vs dot product | vector out vs scalar out |
| dot product vs cosine similarity | dot depends on magnitude and angle; cosine removes magnitude |
| distance vs dot product | distance measures separation; dot measures alignment weighted by size |
| norm vs dot product | norm is length; dot combines two vectors |
| projection vs cosine | projection keeps magnitude along a direction; cosine is normalized alignment |

---

## 23. What We Discovered

1. Multiplying matching coordinates and summing gives the dot product.
2. The dot product maps two equal-length vectors to one scalar.
3. Its sign reveals broad directional relationship.
4. It satisfies

$$
\mathbf{a}\cdot\mathbf{b} = \|\mathbf{a}\|\|\mathbf{b}\|\cos\theta.
$$

5. Raw dot product depends on magnitude and direction.
6. Cosine similarity divides out magnitude.
7. Dot products are weighted sums, projections and alignment scores.
8. Matrix-vector multiplication will turn out to be many dot products at once.

---

## 24. One-Minute Explanation

The dot product takes two vectors, multiplies matching coordinates, and adds the results. That simple computation has a geometric meaning: it equals the product of the vector lengths times the cosine of the angle between them. So positive dot products usually mean the vectors point generally together, zero means perpendicular, and negative means generally opposite. Because magnitude also affects the raw dot product, cosine similarity divides by both lengths to isolate direction. In machine learning, dot products appear everywhere: linear models, neural-network layers, embeddings and Transformer attention.

---

## 25. Mastery Check

1. Compute $[2,3,4]\cdot[5,1,2]$ by hand.
2. Why does a dot product produce a scalar?
3. Why is the dot product zero for perpendicular vectors?
4. What does a negative dot product mean geometrically?
5. Why can two pairs with the same angle have different dot products?
6. How does cosine similarity remove magnitude?
7. Why is cosine similarity undefined for the zero vector?
8. Why can a neuron be understood as a dot product plus bias?
9. How does this chapter foreshadow attention?
10. How does stacking weight vectors lead naturally to matrices?

---

## 🔭 Bridge to Chapter 008

One vector of weights can compute one weighted sum.

But real datasets contain many examples and neural networks contain many neurons.

Writing every dot product separately would become unbearable.

> **How can we organize many vectors so that many dot products happen together?**

That question forces us into **matrices**.
