# Chapter 014 — Eigenvectors: What a Transformation Leaves Alone

> **The Big Question:** Which directions does a matrix leave pointing where they started, and what can we do once we know?

## Where We Are

A matrix can transform space.

But most vectors change direction when a matrix acts on them.

What if we could find a special direction that **doesn't turn** — it only gets stretched or shrunk?

> **Are there directions a transformation leaves pointing the same way?**

**Next → Eigenvectors.**


## The Picture to Hold in Your Head

Most vectors change both length **and direction** when a matrix acts on them. An eigenvector is special because the matrix does not turn it. The vector may stretch, shrink or reverse, but it stays on the same line. The eigenvalue tells you the scale factor along that line.

That turns a complicated transformation into a simpler question: **are there directions in which this matrix behaves like ordinary multiplication by a number?** When there are enough such directions, changing into the eigenvector basis makes the matrix diagonal — the transformation becomes independent stretches along special axes.

Repeated application is where this matters most. After many steps, directions with larger eigenvalue magnitude dominate. Eigenvectors therefore explain long-run behavior of dynamical systems, iterative algorithms and deep repeated linear transformations.

> 🎛️ Use **Try every direction** to hunt for an eigenvector by eye, **When there is none** to see the limit of the idea, and **Again and again** to see why the dominant eigenvector eventually takes over.

## 1. The Problem: One Direction Refused to Move

Chapter 4 left an observation dangling. Apply the shear $S = \begin{bmatrix} 1&1\\0&1\end{bmatrix}$ to a few vectors and watch what happens to their **directions**:

$$
S\begin{bmatrix} 0\\1 \end{bmatrix} = \begin{bmatrix} 1\\1 \end{bmatrix}
\quad(\text{swung } 45°)
\qquad
S\begin{bmatrix} 1\\2 \end{bmatrix} = \begin{bmatrix} 3\\2 \end{bmatrix}
\quad(\text{swung too})
$$

Almost everything tilts. But:

$$
S\begin{bmatrix} 1\\0 \end{bmatrix} = \begin{bmatrix} 1\\0 \end{bmatrix}
$$

Nothing. That direction came out exactly as it went in.

The scaling matrix $\begin{bmatrix} 2&0\\0&3\end{bmatrix}$ does the same thing twice over — it leaves both axes pointing where they were and merely changes their lengths, one by a factor of 2 and the other by 3. Meanwhile a 90° rotation leaves *nothing* pointing where it started.

So matrices differ in a way Chapter 4 never captured. Some directions are **special to a particular matrix**: it acts on them by pure stretching, with no turning at all.

Why care? Because a transformation described as *"stretch by 2 along this direction and by 3 along that one"* is about as simple as a transformation can get. If every matrix could be described that way, matrices would stop being mysterious.

---

## 2. What Would an Answer Need?

We want a method that, given any matrix:

1. **Finds the special directions**, if there are any.
2. **Reports the stretch factor** for each one.
3. **Tells us honestly when there are none** — the rotation must not produce a fake answer.
4. **Lets us rebuild the matrix** from those directions, so the description is complete rather than partial.

---

## 3. First Attempt: Try Every Direction

The obvious method: take a vector, apply the matrix, check whether the output points the same way. Rotate the input a little and repeat.

$$
\begin{bmatrix} 1\\0 \end{bmatrix} \to \text{unchanged}\;\checkmark
\qquad
\begin{bmatrix} 1\\0.1 \end{bmatrix} \to \begin{bmatrix} 1.1\\0.1 \end{bmatrix} \;(\text{tilted})
\qquad
\begin{bmatrix} 1\\0.2 \end{bmatrix} \to \begin{bmatrix} 1.2\\0.2 \end{bmatrix} \;(\text{tilted})
$$

This is Chapter 4 §3's mistake again: there are infinitely many directions, and sampling them can only ever suggest an answer, never prove one. Worse, it would silently miss a special direction that happens to fall between our samples.

> ⚠️ **A Tempting Wrong Idea**
>
> *"Search numerically until the output is close enough to parallel."*
>
> "Close to parallel" is not a property a direction either has or lacks — it depends on your tolerance. And the question is exact: either $A\mathbf{v}$ is a multiple of $\mathbf{v}$ or it is not. **Stop searching and write the condition down as an equation.**

---

## 4. The Discovery: Write the Question as an Equation

State the requirement in symbols. We want a vector $\mathbf{v}$ whose image is a scalar multiple of itself:

$$
\boxed{\;A\mathbf{v} = \lambda\mathbf{v}\;}
$$

That is the whole definition. $\mathbf{v}$ is an **eigenvector** — a direction the matrix does not turn. $\lambda$ (lambda) is its **eigenvalue** — the factor it gets stretched by. *Eigen* is German for "own" or "characteristic": these are the matrix's own directions.

One exclusion: $\mathbf{v} = \mathbf{0}$ satisfies the equation for every $\lambda$ and tells us nothing, so eigenvectors are required to be non-zero.

Now solve it. Move everything to one side — but carefully, because $A$ is a matrix and $\lambda$ is a number, so $A - \lambda$ is nonsense. Insert the identity matrix $I$ to fix the types:

$$
A\mathbf{v} - \lambda I\mathbf{v} = \mathbf{0}
\qquad\Longrightarrow\qquad
(A - \lambda I)\,\mathbf{v} = \mathbf{0}
$$

Read what this says: the matrix $(A - \lambda I)$ sends the non-zero vector $\mathbf{v}$ to the origin. Chapter 4 §8 gave that a name — $\mathbf{v}$ is in the **null space** — and told us exactly when a non-trivial null space exists:

$$
\det(A - \lambda I) = 0
$$

This is the **characteristic equation**, and it is a polynomial in $\lambda$ alone. The infinite search of §3 has become finding the roots of a polynomial.

> 💡 The whole move was refusing to hunt and instead writing the requirement as an equation. Chapter 4's determinant then converted "a direction survives" into "this number is zero."

> 📜 **History Lens — Euler, Cauchy, and a German Prefix**
>
> The idea arrived through physics, not algebra. In the 1750s Leonhard Euler, studying how rigid bodies rotate, found that every rotation of a solid body has an **axis** — a line that stays put while everything else turns. That is an eigenvector, three-quarters of a century before anyone wrote $A\mathbf{v} = \lambda\mathbf{v}$.
>
> In the 1820s Augustin-Louis Cauchy, working on quadratic forms and the "principal axes" of surfaces, produced the characteristic equation in essentially the form above and proved that symmetric matrices always have real roots — the fact that makes §10's PCA work at all.
>
> The name came last. Mathematicians called them "proper values" or "characteristic values" for decades; David Hilbert used *Eigenwert* in the early 1900s, and when the German literature was translated the prefix survived untranslated. So "eigenvalue" is a half-translated word — and its meaning, *the matrix's own value*, is exactly right.

---

## 5. Doing It by Hand

Take a matrix small enough to solve completely:

$$
A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}
$$

**Step 1 — build $A - \lambda I$.**

$$
A - \lambda I = \begin{bmatrix} 2-\lambda & 1 \\ 1 & 2-\lambda \end{bmatrix}
$$

**Step 2 — set its determinant to zero.** Using $ad - bc$ from Chapter 4 §7:

$$
(2-\lambda)(2-\lambda) - (1)(1) = 0
$$

**Step 3 — expand and solve.**

$$
\lambda^2 - 4\lambda + 4 - 1 = 0
\quad\Longrightarrow\quad
\lambda^2 - 4\lambda + 3 = 0
\quad\Longrightarrow\quad
(\lambda-3)(\lambda-1) = 0
$$

$$
\lambda_1 = 3, \qquad \lambda_2 = 1
$$

**Step 4 — find each direction.** For $\lambda_1 = 3$, solve $(A - 3I)\mathbf{v} = \mathbf{0}$:

$$
\begin{bmatrix} -1 & 1 \\ 1 & -1 \end{bmatrix}\begin{bmatrix} v_1 \\ v_2\end{bmatrix} = \begin{bmatrix} 0\\0\end{bmatrix}
\quad\Longrightarrow\quad
-v_1 + v_2 = 0
\quad\Longrightarrow\quad
v_1 = v_2
$$

So any vector along $(1,1)$ works. Both rows gave the same equation — that is the determinant being zero, showing up as a redundant row.

For $\lambda_2 = 1$: $v_1 + v_2 = 0$, giving the direction $(1,-1)$.

**Step 5 — check.**

$$
A\begin{bmatrix} 1\\1\end{bmatrix} = \begin{bmatrix} 3\\3\end{bmatrix} = 3\begin{bmatrix} 1\\1\end{bmatrix} \;\checkmark
\qquad
A\begin{bmatrix} 1\\-1\end{bmatrix} = \begin{bmatrix} 1\\-1\end{bmatrix} = 1\begin{bmatrix} 1\\-1\end{bmatrix} \;\checkmark
$$

The entire behaviour of $A$ on the whole plane is now one sentence: **stretch by 3 along the diagonal $(1,1)$, and leave the anti-diagonal $(1,-1)$ alone.**

Note that eigenvectors have no natural length — $(2,2)$ and $(-5,-5)$ are equally valid. Only the **direction** is determined, so we usually normalize to length 1.

---

## 6. When There Is No Answer

Requirement 3 said the method must fail honestly. Test it on the two awkward cases from §1.

**The shear** $S = \begin{bmatrix} 1&1\\0&1\end{bmatrix}$:

$$
\det\begin{bmatrix} 1-\lambda & 1 \\ 0 & 1-\lambda\end{bmatrix} = (1-\lambda)^2 = 0
\quad\Longrightarrow\quad
\lambda = 1 \text{ (twice)}
$$

Solving $(S-I)\mathbf{v} = \mathbf{0}$ gives $v_2 = 0$ — only the direction $(1,0)$. A repeated root, but just **one** eigenvector direction instead of two. Such matrices are called **defective**, and they cannot be rebuilt from their eigenvectors alone. Our §1 observation was right: the shear has exactly one special direction.

**The rotation** $R = \begin{bmatrix} 0&-1\\1&0\end{bmatrix}$:

$$
\det\begin{bmatrix} -\lambda & -1 \\ 1 & -\lambda \end{bmatrix} = \lambda^2 + 1 = 0
\quad\Longrightarrow\quad
\lambda^2 = -1
$$

No real solutions. And that is exactly correct — a 90° rotation turns *every* direction, so there is no real direction it leaves alone. The mathematics did not invent a fake answer; it reported the truth.

> ⚠️ **Precision, not false simplicity.** Over the complex numbers $\lambda = \pm i$, and those complex eigenvalues encode the rotation angle — genuinely useful in signal processing and in analysing recurrent networks (Chapter 40). But over the real plane, where our houses and pixels live, the honest answer is: none.

---

## 7. Diagonalization: Rebuilding the Matrix

Requirement 4 said the description should let us rebuild $A$. Put the eigenvectors in the columns of a matrix $P$ and the eigenvalues down the diagonal of $D$:

$$
P = \begin{bmatrix} 1 & 1 \\ 1 & -1\end{bmatrix},
\qquad
D = \begin{bmatrix} 3 & 0 \\ 0 & 1\end{bmatrix}
$$

Then

$$
\boxed{\;A = PDP^{-1}\;}
$$

For our matrix, $P^{-1} = \begin{bmatrix} 0.5 & 0.5\\ 0.5 & -0.5\end{bmatrix}$, and multiplying out gives back $\begin{bmatrix} 2&1\\1&2\end{bmatrix}$ exactly.

Read it right to left, as Chapter 4 §9 taught, and it tells a story in three acts:

| | | |
|---|---|---|
| $P^{-1}$ | **translate** | rewrite the vector in eigenvector coordinates |
| $D$ | **stretch** | scale each of those coordinates by its eigenvalue — the easy part |
| $P$ | **translate back** | express the result in ordinary coordinates again |

> 💡 **Intuition** — Every diagonalizable matrix is secretly just a scaling. It only *looks* complicated because we insist on describing it in the wrong coordinate system. Change to the matrix's own axes and all it does is stretch.

---

## 8. Why This Is Powerful: Repeated Application

Here is where eigenvalues stop being a curiosity. Suppose you need $A^{10}$ — apply the transformation ten times. Direct multiplication is nine matrix products. With diagonalization:

$$
A^2 = (PDP^{-1})(PDP^{-1}) = PD\underbrace{(P^{-1}P)}_{I}DP^{-1} = PD^2P^{-1}
$$

The inner $P^{-1}P$ cancels, and the pattern continues:

$$
A^{n} = PD^{n}P^{-1}
$$

And $D^n$ is trivial, because powering a diagonal matrix just powers each diagonal entry. For our $A$:

$$
D^{10} = \begin{bmatrix} 3^{10} & 0 \\ 0 & 1^{10}\end{bmatrix} = \begin{bmatrix} 59049 & 0 \\ 0 & 1 \end{bmatrix}
\quad\Longrightarrow\quad
A^{10} = \begin{bmatrix} 29525 & 29524 \\ 29524 & 29525\end{bmatrix}
$$

Now look at what happened to the two eigenvalues. $3^{10} = 59049$; $1^{10} = 1$. After ten applications, the $\lambda = 3$ direction is **59,049 times more important** than the other. Apply it a hundred times and the second direction is numerically invisible.

> 🎯 This single observation explains a startling amount of deep learning:
>
> - Repeatedly applying a matrix drives everything toward its **dominant eigenvector**. That is why the power-iteration experiment in the notebook converges.
> - If eigenvalues exceed 1, repeated application **explodes**; if they are below 1, it **vanishes**. That is precisely the exploding/vanishing gradient problem of Chapters 39–40, and the reason LSTMs were invented.
> - Chapter 4's Level-4 exercise asked what happens to the determinant of a long chain of matrices. This is the answer: products of eigenvalues, compounding.

---

## 9. Where Eigenvectors Stop Being Enough

The eigenvector picture is powerful, but it has boundaries.

A quarter-turn rotation has no real direction that stays on its own line. A rectangular matrix maps between spaces of different dimensions, so the equation $A\mathbf{v}=\lambda\mathbf{v}$ does not even make sense: the left and right sides live in different spaces. Some square matrices also fail to provide enough independent eigenvectors to form a basis.

This is an important mathematical moment. We should not force the tool past the point where its question makes sense.

What we actually want is slightly different:

> **Can we find special input directions that a matrix sends to perpendicular output directions, and measure how strongly each one survives?**

That broader question does not require the input and output spaces to be the same. It also does not require a direction to come back pointing along itself.

The answer is the next chapter: **Singular Value Decomposition**.

---

## 10. 🔬 The Experiment: Why One Direction Takes Over

Before leaving eigenvectors, use them for the job they are exceptionally good at: repeated application.

Take

$$
A = \begin{bmatrix}2&1\\1&2\end{bmatrix}.
$$

Start from almost any non-zero vector, apply $A$, normalize the result, and repeat. The direction converges toward $[1,1]$.

Why? The two eigen-directions are $[1,1]$ with eigenvalue $3$, and $[1,-1]$ with eigenvalue $1$. Any starting vector can be written as a mixture of those two directions. After $n$ applications:

$$
A^n\mathbf{x} = c_1 3^n \mathbf{v}_1 + c_2 1^n \mathbf{v}_2.
$$

The first term grows $3^n$ while the second does not. Eventually the first direction overwhelms the other.

That is the intuition behind the **power method**, one of the simplest eigenvector algorithms: repeatedly apply the matrix and normalize. It works because repeated transformation amplifies the dominant eigen-direction.

> 🎛️ The **Again and again** studio scene is this experiment with your hands. Change the matrix so the two eigenvalues become nearly equal and notice how much slower the direction settles.

## 11. How It Breaks

| Failure | What it looks like | Why |
|---|---|---|
| **Defective matrix** | fewer independent eigenvectors than dimensions | A repeated eigenvalue does not guarantee enough independent directions. The matrix cannot be diagonalized. §6 |
| **Complex eigenvalues** | no real eigenvector direction | A rotation can turn every real direction. That is a real geometric fact, not an arithmetic failure. §6 |
| **Nearly equal dominant magnitudes** | power iteration converges painfully slowly | Neither eigen-direction quickly overwhelms the other. §10 |
| **$|\lambda|>1$ under repetition** | values grow rapidly | The same stretch is multiplied again and again: $|\lambda|^n$. §8 |
| **$|\lambda|<1$ under repetition** | values shrink toward zero | Repeated contraction compounds in exactly the same way. §8 |
| **Poorly conditioned eigenvector basis** | diagonalization is numerically fragile | If the columns of $P$ are nearly dependent, converting into and out of eigen-coordinates magnifies numerical error. |

## 12. 🎯 Machine Learning Connection

Eigenvalues matter whenever the **same local transformation is applied repeatedly** or whenever we care about **curvature along special directions**.

| Eigen-idea | Where it shows up |
|---|---|
| dominant eigenvalue of a repeated map | long-run direction of iterative algorithms and linear recurrent dynamics |
| spectral radius $\max_i |\lambda_i|$ | whether repeated linear dynamics tend to grow, shrink, or remain bounded |
| eigenvalues of a loss Hessian | curvature: steep directions versus flat directions |
| largest Hessian eigenvalue | why an overly large learning rate can become unstable |
| eigenvectors of a symmetric covariance matrix | a preview of the principal directions used later in PCA |

The key pattern is the same in every row: a high-dimensional object becomes easier to reason about when we find directions along which its action reduces to multiplication by one number.

> **Do not jump ahead to SVD yet.** Eigenvectors are about a square map acting on its own space. The next chapter starts exactly where that assumption breaks.

## 13. Distinctions That Matter

| Pair | Difference |
|---|---|
| **eigenvector** vs **eigenvalue** | direction left on its own line vs scalar stretch along that direction |
| **diagonalizable** vs **defective** | enough independent eigenvectors to form a basis vs not enough |
| **real** vs **complex eigenvalues** | visible invariant directions in real space vs rotation-like behavior requiring complex coordinates |
| **largest eigenvalue** vs **largest magnitude** | for repeated powers, the magnitude $|\lambda|$ controls dominance |
| **eigendecomposition** vs **SVD** | eigendecomposition asks for directions preserved by a square self-map; SVD, next chapter, relaxes that requirement and works for every real matrix |

## 14. What We Discovered

1. An eigenvector is a non-zero direction a square matrix does not turn.
2. Its eigenvalue is the factor by which the matrix stretches, shrinks or reverses that direction.
3. The equation $A\mathbf{v}=\lambda\mathbf{v}$ becomes $(A-\lambda I)\mathbf{v}=0$, so eigenvalues occur when $A-\lambda I$ becomes singular.
4. A matrix with enough independent eigenvectors can be diagonalized as $A=PDP^{-1}$.
5. In the eigenvector basis, a complicated transformation becomes independent scalar stretches.
6. Repeated powers become easy: $A^n=PD^nP^{-1}$.
7. The eigenvalue with largest magnitude controls the long-run direction of repeated application for most starting vectors.
8. Some matrices have no real eigenvectors or not enough eigenvectors to form a basis; that limitation motivates SVD rather than being a failure of eigen-analysis.

## 15. Mathematics We Built

$$
A\mathbf{v} = \lambda\mathbf{v}
\qquad
(A - \lambda I)\mathbf{v} = \mathbf{0}
\qquad
\det(A - \lambda I) = 0
$$

$$
A = PDP^{-1}
\qquad
A^{n} = PD^{n}P^{-1}
$$

For a $2\times2$ matrix, the characteristic equation is a quadratic in $\lambda$. Its roots are the eigenvalues; each root makes $A-\lambda I$ singular, revealing a non-zero null-space direction that becomes the corresponding eigenvector.

## 16. What Each Symbol Means

| Symbol | English | In code |
|---|---|---|
| $A$ | the square transformation we are studying | `A` |
| $\lambda$ | eigenvalue — stretch factor along a special direction | `eigvals[i]` |
| $\mathbf{v}$ | eigenvector — a direction that is not turned | `eigvecs[:, i]` |
| $I$ | identity matrix | `np.eye(n)` |
| $\det(A-\lambda I)=0$ | condition that makes a non-zero eigenvector possible | `np.linalg.eig(A)` |
| $P$ | eigenvectors stacked as columns | `eigvecs` |
| $D$ | eigenvalues placed on a diagonal | `np.diag(eigvals)` |
| $A^n$ | applying the same transformation $n$ times | `np.linalg.matrix_power(A, n)` |

## 17. One-Minute Explanation

With no equations:

> A transformation turns almost every arrow. What is special about the few it does not turn, and why does that make applying the transformation a thousand times easy?

---

## 18. Exercises

**Level 1 — Observe.** Chapter 4's four transformations were scale, rotate, shear and project. Without calculating, say how many real eigenvector directions each one has, and what the eigenvalues should be. Then check one prediction: what must the eigenvalues of a projection be, given that applying it twice is the same as applying it once?

**Level 2 — Calculate (by hand).** Find the eigenvalues and eigenvectors of $B = \begin{bmatrix} 4&1\\2&3\end{bmatrix}$. Verify each by computing $B\mathbf{v}$ and checking it is $\lambda\mathbf{v}$. Then confirm two shortcuts on your answers: the eigenvalues should sum to the trace $4+3=7$, and multiply to $\det B = 10$. Why must those hold?

**Level 3 — Derive.** Prove that if $A\mathbf{v} = \lambda\mathbf{v}$ then $A^2\mathbf{v} = \lambda^2\mathbf{v}$, then extend it to $A^n$. Use it to prove that if every $|\lambda_i| < 1$, then $A^n\mathbf{x} \to \mathbf{0}$ for *every* starting $\mathbf{x}$ — and explain in one sentence what that means for a 50-layer network whose weight matrices all have small eigenvalues.

**Level 4 — Investigate** (notebook Steps 8–11). Start from a random vector and apply $A = \begin{bmatrix}2&1\\1&2\end{bmatrix}$ repeatedly, normalizing each time. Which direction does it converge to, and how many steps does it take? Then rerun with $A = \begin{bmatrix}2&0\\0&1.999\end{bmatrix}$. Convergence becomes dramatically slower — explain why using the ratio $\lambda_2/\lambda_1$.

**Level 5 — Design.** Build a $2\times2$ matrix whose eigenvectors are the diagonal directions $(1,1)$ and $(1,-1)$, with eigenvalues $5$ and $\tfrac12$. Construct it from $A=PDP^{-1}$ rather than guessing its entries. Then predict, without multiplying $A$ twenty times, what $A^{20}$ will do to a vector that has even a tiny component along $(1,1)$. Finally change the two eigenvalues so repeated application is stable instead of explosive.

---

## 19. Common Mistakes

| Mistake | Why it is wrong |
|---|---|
| "Every matrix has $n$ eigenvectors." | Defective matrices do not. §6 |
| "No real eigenvalues means I made an arithmetic error." | It usually means the matrix rotates. §6 |
| "Eigenvectors have a specific length." | Only the direction is determined; any non-zero multiple is the same eigenvector. §5 |

## 20. Socratic Questions

1. The determinant equals the product of eigenvalues. What must be true about the determinant if one eigenvalue is zero, and what does that say geometrically?
2. A symmetric matrix has perpendicular real eigenvectors. Why would that make symmetric matrices unusually easy to understand geometrically?
3. If two eigenvalues have almost the same magnitude, why does the power method converge slowly?
4. A 90° rotation has no real eigenvector. What exactly fails in the equation $A\mathbf{v}=\lambda\mathbf{v}$ over the real numbers?
5. If a matrix maps $\mathbb{R}^{100}$ to $\mathbb{R}^{20}$, why can ordinary eigendecomposition not even be posed for that matrix?
6. We want a decomposition that still tells us the important input and output directions for rotations, rectangular matrices and rank-deficient maps. What should such a decomposition preserve from the eigenvector story, and what must it relax?

## 21. 🔭 Bridge to Chapter 015

Eigenvectors gave us a beautiful simplification: find directions a matrix does not turn, change coordinates into those directions, and the transformation becomes a collection of scalar stretches.

But the quarter-turn exposed the problem. Some matrices turn **every** real direction. Rectangular matrices are even more stubborn: their inputs and outputs do not live in the same space, so “comes back along the same direction” is the wrong question from the start.

We do not want to abandon the geometric insight. We want to generalize it.

> **For any matrix whatsoever, can we find perpendicular input directions, measure how strongly each survives, and see where those directions land in the output space?**

Yes. The answer is **Singular Value Decomposition** — and unlike eigendecomposition, it works for every real matrix.
