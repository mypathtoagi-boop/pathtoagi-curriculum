# Exercises — Chapter 013: Rank, Span and Linear Independence

## Level 1 — Explain It

1. Explain the difference between **number of vectors** and **number of independent directions**.
2. Explain span using a physical movement analogy.
3. Explain why `[1,1]` and `[2,2]` do not span the whole plane.
4. Explain what rank measures geometrically.
5. Explain why losing rank means losing information.

## Level 2 — Calculate It

For each pair, decide whether the vectors are independent and describe their span.

1. `[1,0]`, `[0,1]`
2. `[1,1]`, `[2,2]`
3. `[1,2]`, `[2,-1]`
4. `[1,2,3]`, `[2,4,6]`

Then compute the rank by reasoning—not software—of

$$
A=\begin{bmatrix}
1&2\\
1&2
\end{bmatrix}
$$

and

$$
B=\begin{bmatrix}
1&0\\
0&1
\end{bmatrix}.
$$

## Level 3 — Derive It

Starting from

$$
c_1v_1+c_2v_2+c_3v_3=0
$$

with $c_3\ne0$, rearrange to show that $v_3$ can be written as a combination of the first two vectors.

Then explain why that proves dependence.

## Level 4 — Predict It

Without running code, predict ranks:

```text
[[1,0],[0,1]]                     → rank ___
[[1,2],[2,4]]                     → rank ___
[[1,0,1],[0,1,1]]                 → rank ___
[[1,2,3],[2,4,6],[3,6,9]]         → rank ___
```

Then verify with NumPy.

## Level 5 — Break It

### Duplicate a feature

Create a matrix with two independent columns. Add a duplicate of one column. Compare number of columns and rank.

### Add a linear-combination feature

Add `x3 = 2*x1 - x2`. Explain why storage increases but independent information does not.

### Destroy a dimension

Apply

$$
P=\begin{bmatrix}1&0\\0&0\end{bmatrix}
$$

to many 2D points. Show that all y-information disappears.

### Near dependence

Compare condition behavior of columns `[1,1]` and `[1.000001,1]`. Investigate how coordinate solutions react to tiny input perturbations.

## Level 6 — Build It

Write a simple 2D independence checker without using `matrix_rank`.

Then write code that:

1. accepts a small matrix,
2. identifies obviously duplicate/proportional columns,
3. reports how many unique independent directions remain for simple cases.

Compare your reasoning against `np.linalg.matrix_rank`.

## Investigation — Rank and Compression

Generate a random column vector `u` and row vector `v.T`. Construct

$$
A=uv^T.
$$

Investigate:

- matrix shape,
- matrix rank,
- what its columns look like relative to one another,
- what happens to arbitrary input vectors under `A @ x`.

Then add small random noise to `A` and observe what numerical rank routines report under different tolerances.

Explain the difference between exact algebraic rank and approximate low-rank structure.

## Shape Check

```text
A ∈ R^(8×3)           → maximum rank ______
B ∈ R^(2×100)         → maximum rank ______
C ∈ R^(5×5) full rank → rank ______
```

## Mastery Gate

Do not continue until you can answer:

- What is span?
- What is linear independence?
- Why does a nontrivial zero combination prove dependence?
- What does rank count?
- What does the null space contain?
- Why does full rank matter for inversion?
- What does rank–nullity mean in plain English?
- Why does near dependence matter numerically even when exact rank is full?
