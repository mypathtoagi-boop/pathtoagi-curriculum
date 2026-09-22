# Chapter 010 — Matrix–Matrix Multiplication as Composition

> **The Big Question:** If one matrix transforms a vector and another matrix transforms the result, can both transformations be combined into one matrix?

## Where We Are

Chapter 009 showed that a matrix can act on a vector:

$$
\mathbf{x}\rightarrow A\mathbf{x}.
$$

A deep network does not stop after one transformation.

It repeatedly applies transformations:

$$
\mathbf{x}
\rightarrow A\mathbf{x}
\rightarrow B(A\mathbf{x}).
$$

Writing nested expressions works for two layers. It becomes painful for twenty.

We need a way to combine transformations themselves.

> **Today:** we discover matrix–matrix multiplication as function composition, many dot products, and “apply the right matrix first.”
>
> **Next:** once matrices can represent transformations, we will study what makes a transformation linear and what geometry it preserves.

---


## The Picture to Hold in Your Head

Matrix multiplication looks strange only if matrices are tables. If matrices are **actions on space**, multiplication means something natural: **do one action, then another**.

The product is the single matrix that has exactly the same final effect as the two-step journey. The order matters because the second transformation acts on a space that the first transformation has already changed. “Rotate then shear” and “shear then rotate” are different physical instructions, so their matrices must differ too.

Read products from right to left, just like nested functions: $AB\mathbf{x}$ means $B$ touches the vector first, then $A$.

> 🎛️ Play **First, then second** and immediately play **The other order**. Do not compare only the final numbers; watch which directions the second operation inherits from the first.

## 1. The Problem: Two Transformations in Sequence

Suppose we first stretch x-coordinates by 2:

$$
A=
\begin{bmatrix}
2&0\\
0&1
\end{bmatrix}.
$$

Then shear horizontally:

$$
B=
\begin{bmatrix}
1&1\\
0&1
\end{bmatrix}.
$$

Take

$$
\mathbf{x}=\begin{bmatrix}1\\2\end{bmatrix}.
$$

First apply $A$:

$$
A\mathbf{x} = \begin{bmatrix} 2\\2 \end{bmatrix}.
$$

Then apply $B$:

$$
B(A\mathbf{x}) = \begin{bmatrix} 1&1\\ 0&1 \end{bmatrix} \begin{bmatrix} 2\\2 \end{bmatrix} = \begin{bmatrix}4\\2\end{bmatrix}.
$$

Can one matrix $C$ do the whole job?

We want

$$
C\mathbf{x}=B(A\mathbf{x})
$$

for **every** vector $\mathbf{x}$, not only this one.

---

## 2. What Would the Combined Matrix Need to Do?

From Chapter 009, a matrix is determined by where it sends the basis vectors.

So to discover the combined matrix, ask what the two-step process does to

$$
\mathbf{e}_1=\begin{bmatrix}1\\0\end{bmatrix}
$$

and

$$
\mathbf{e}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

Whatever those final vectors are, they must become the columns of $C$.

This gives us a route to matrix multiplication without memorizing a rule.

---

## 3. Transform the First Basis Vector

Start with

$$
\mathbf{e}_1=\begin{bmatrix}1\\0\end{bmatrix}.
$$

Apply $A$:

$$
A\mathbf{e}_1 = \begin{bmatrix}2\\0\end{bmatrix}.
$$

Now apply $B$:

$$
B(A\mathbf{e}_1) = \begin{bmatrix} 1&1\\0&1 \end{bmatrix} \begin{bmatrix}2\\0\end{bmatrix} = \begin{bmatrix}2\\0\end{bmatrix}.
$$

So the first column of the combined matrix is

$$
\begin{bmatrix}2\\0\end{bmatrix}.
$$

---

## 4. Transform the Second Basis Vector

Now

$$
\mathbf{e}_2=\begin{bmatrix}0\\1\end{bmatrix}.
$$

Apply $A$:

$$
A\mathbf{e}_2 = \begin{bmatrix}0\\1\end{bmatrix}.
$$

Apply $B$:

$$
B(A\mathbf{e}_2) = \begin{bmatrix}1\\1\end{bmatrix}.
$$

So the second column is

$$
\begin{bmatrix}1\\1\end{bmatrix}.
$$

Therefore

$$
C=
\begin{bmatrix}
2&1\\
0&1
\end{bmatrix}.
$$

Check our original vector:

$$
C\begin{bmatrix}1\\2\end{bmatrix} = \begin{bmatrix} 2(1)+1(2)\\ 0(1)+1(2) \end{bmatrix} = \begin{bmatrix}4\\2\end{bmatrix}.
$$

It matches the two-step process.

---

## 5. The Discovery: Matrix Multiplication

We write the combined transformation as

$$
\boxed{C=BA}
$$

so that

$$
(BA)\mathbf{x}=B(A\mathbf{x}).
$$

This notation encodes order.

> **The matrix closest to the vector acts first.**

That is exactly like function composition:

$$
(g\circ f)(x)=g(f(x)).
$$

Matrix multiplication is transformation composition.

---

## 6. Why the Order Looks Backward

Suppose you read

$$
BA\mathbf{x}.
$$

Human language may tempt you to say “B then A.”

But parentheses reveal the truth:

$$
B(A\mathbf{x}).
$$

$A$ touches $\mathbf{x}$ first.

Then $B$ acts on the result.

This is not an arbitrary convention. It follows from how function composition works.

---

## 7. Derive the Entry Formula

Let

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix},
\qquad
B=
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}.
$$

The first column of $BA$ is $B$ applied to the first column of $A$:

$$
B\begin{bmatrix}a\\c\end{bmatrix} = \begin{bmatrix} ea+fc\\ga+hc \end{bmatrix}.
$$

The second column is

$$
B\begin{bmatrix}b\\d\end{bmatrix} = \begin{bmatrix} eb+fd\\gb+hd \end{bmatrix}.
$$

Therefore

$$
\boxed{
BA=
\begin{bmatrix}
ea+fc & eb+fd\\
ga+hc & gb+hd
\end{bmatrix}
}
$$

Each entry is a row–column dot product.

---

## 8. The Row–Column Rule

To find entry $(i,j)$ of $BA$:

1. take row $i$ of $B$,
2. take column $j$ of $A$,
3. dot them.

Symbolically:

$$
(BA)_{ij} = \sum_k B_{ik}A_{kj}.
$$

The repeated index $k$ is the inner dimension being summed over.

This formula looks abstract only until you remember:

> one output entry = one dot product.

---

## 9. Shape Rule

Suppose

$$
A\in\mathbb{R}^{m\times n}
$$

and

$$
B\in\mathbb{R}^{p\times m}.
$$

Then

$$
BA\in\mathbb{R}^{p\times n}.
$$

Shape mnemonic:

```text
(p × m)(m × n) = (p × n)
      ↑  ↑
      must match
```

The inner dimensions disappear because they are summed over in dot products.

The outer dimensions survive.

---

## 10. Why Matrix Multiplication Is Not Commutative

For ordinary numbers,

$$
2\times3=3\times2.
$$

For matrices, usually

$$
AB\ne BA.
$$

Why?

Because order of transformations matters.

Stretch then rotate is generally not the same as rotate then stretch.

That is a geometric fact, not a symbolic annoyance.

---

## 11. Concrete Example: Order Matters

Let

$$
S=
\begin{bmatrix}
2&0\\0&1
\end{bmatrix}
$$

and a 90° rotation

$$
R=
\begin{bmatrix}
0&-1\\1&0
\end{bmatrix}.
$$

Take

$$
\mathbf{x}=\begin{bmatrix}1\\1\end{bmatrix}.
$$

### Stretch then rotate

$$
S\mathbf{x}=\begin{bmatrix}2\\1\end{bmatrix}
$$

then

$$
R(S\mathbf{x})=\begin{bmatrix}-1\\2\end{bmatrix}.
$$

### Rotate then stretch

$$
R\mathbf{x}=\begin{bmatrix}-1\\1\end{bmatrix}
$$

then

$$
S(R\mathbf{x})=\begin{bmatrix}-2\\1\end{bmatrix}.
$$

Different answers.

Therefore

$$
RS\ne SR.
$$

---

## 12. A Tempting Wrong Idea: “Just Multiply Matching Entries”

Element-wise multiplication would give

$$
A\odot B.
$$

But composition requires that each output coordinate of one transformation be fed into every relevant input of the next.

That mixing is exactly what row–column dot products achieve.

> ⚠️ **A Tempting Wrong Idea**
>
> Element-wise multiplication combines corresponding entries. Matrix multiplication composes transformations. They solve different problems.

---

## 13. Associativity: Why Deep Composition Is Manageable

Matrix multiplication satisfies

$$
(AB)C=A(BC).
$$

So parentheses can move without changing the final transformation.

This matters computationally.

Suppose shapes are:

$$
A:(1000\times10),
\quad
B:(10\times1000),
\quad
C:(1000\times1).
$$

Different parenthesizations can require very different amounts of work.

The mathematics is the same; the computational cost may not be.

This becomes important in optimized numerical computing.

---

## 14. Identity Matrix

The identity matrix does nothing:

$$
I=
\begin{bmatrix}
1&0\\0&1
\end{bmatrix}.
$$

For every vector,

$$
I\mathbf{x}=\mathbf{x}.
$$

And for compatible matrices,

$$
IA=A,
$$

$$
AI=A.
$$

It plays the same role as the number 1 in ordinary multiplication.

---

## 15. Inverse Matrix as “Undo”

If a matrix $A$ has an inverse $A^{-1}$, then

$$
A^{-1}A=I.
$$

So if

$$
\mathbf{y}=A\mathbf{x},
$$

then

$$
A^{-1}\mathbf{y}=\mathbf{x}.
$$

The inverse undoes the transformation.

Not every matrix has one. Later, rank will explain why.

---

## 16. Neural-Network Connection

Ignoring nonlinearities for a moment, imagine two dense layers:

$$
\mathbf{h}=W_1\mathbf{x}
$$

and

$$
\mathbf{y}=W_2\mathbf{h}.
$$

Substitute:

$$
\mathbf{y}=W_2(W_1\mathbf{x}).
$$

Therefore

$$
\mathbf{y}=(W_2W_1)\mathbf{x}.
$$

Two purely linear layers collapse into one linear layer.

That observation will become crucial when we ask why neural networks need nonlinear activation functions.

---

## 17. Batch Computation Preview

Suppose each column of $X$ is one input vector.

Then

$$
Y=WX
$$

applies the same transformation to many examples at once.

Matrix–matrix multiplication therefore appears in two ways:

1. compose transformations,
2. process batches of vectors efficiently.

Modern deep-learning workloads are dominated by this operation.

---

## 18. Code From Scratch

```python
def matmul(A, B):
    rows_a = len(A)
    cols_a = len(A[0])
    rows_b = len(B)
    cols_b = len(B[0])

    if cols_a != rows_b:
        raise ValueError("inner dimensions must match")

    out = [[0.0 for _ in range(cols_b)] for _ in range(rows_a)]

    for i in range(rows_a):
        for j in range(cols_b):
            total = 0.0
            for k in range(cols_a):
                total += A[i][k] * B[k][j]
            out[i][j] = total

    return out
```

The three loops have clear meanings:

- `i`: output row,
- `j`: output column,
- `k`: dot-product accumulation.

NumPy compresses all of that into

```python
C = A @ B
```

but the mathematics underneath is unchanged.

---

## 19. Break It

### Shape mismatch

A `(3×4)` matrix cannot multiply a `(5×2)` matrix because 4 ≠ 5.

### Wrong order

Even when both $AB$ and $BA$ exist, they usually differ.

### Assuming every matrix has an inverse

A transformation that squashes a plane onto a line loses information. No inverse can recover what was destroyed.

### Forgetting nonlinearities

Two linear neural-network layers collapse into one. Adding nonlinear activations prevents this collapse and gives deep networks their expressive power.

---

## 20. History Lens — Composition as Algebra

One of linear algebra’s great achievements is turning sequences of geometric operations into algebraic objects that can themselves be multiplied.

Instead of separately describing “rotate, then stretch, then shear,” we can multiply matrices and obtain a single transformation.

That ability to compose operations is why matrices became fundamental across geometry, mechanics, graphics, control theory and neural networks.

---

## 21. Distinctions That Matter

| Pair | Difference |
|---|---|
| matrix–vector vs matrix–matrix | transform one vector vs compose/apply across many vectors |
| matrix multiplication vs element-wise multiplication | composition via dot products vs matching-entry products |
| $AB$ vs $BA$ | usually different transformation order |
| identity vs inverse | identity does nothing; inverse undoes a specific transformation |
| associativity vs commutativity | parentheses may move; order usually may not |

---

## 22. What We Discovered

1. Matrix multiplication composes transformations.
2. In $BA\mathbf{x}$, $A$ acts first.
3. Each matrix-product entry is a row–column dot product.
4. Shape rule: $(p\times m)(m\times n)=(p\times n)$.
5. Matrix multiplication is associative but generally not commutative.
6. The identity matrix leaves vectors unchanged.
7. Inverse matrices, when they exist, undo transformations.
8. Multiple purely linear neural-network layers collapse into one matrix multiplication.
9. Matrix–matrix multiplication also powers batch computation.

---

## 23. One-Minute Explanation

A matrix transforms vectors. If one matrix acts and then another acts, the two transformations can be combined by multiplying the matrices. The rightmost matrix acts first, just like nested functions. Each entry of the product is a dot product between a row from the left matrix and a column from the right matrix. Order matters because transformations do not generally commute: rotate-then-stretch is not the same as stretch-then-rotate. In neural networks, matrix multiplication composes layers and processes large batches efficiently.

---

## 24. Mastery Check

1. Why does $BA\mathbf{x}$ mean apply $A$ first?
2. Derive a 2×2 matrix product by hand.
3. Why are matrix-product entries row–column dot products?
4. What shape results from `(7×3) @ (3×5)`?
5. Why is `(7×3) @ (4×5)` invalid?
6. Give a geometric explanation for $AB\ne BA$.
7. What does the identity matrix do?
8. What does an inverse matrix mean geometrically?
9. Why can two linear neural-network layers collapse into one?
10. Why does adding a nonlinearity change that conclusion?

---

## 🔭 Bridge to Chapter 011

We can now represent transformations and compose them.

But we have been using the word **linear** without fully asking what it permits and forbids.

> **What exactly makes a transformation linear, and what geometric structure must it preserve?**

That is the next question.
