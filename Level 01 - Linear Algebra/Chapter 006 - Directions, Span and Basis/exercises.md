# Chapter 006 Exercises — Directions, Span and Basis

Work from seeing to counting to proving, then test the ideas against real data. Everything here needs only Chapter 005's two operations: stretch an arrow, add two arrows.

## Level 1 — Explain

1. Ramesh's sheet has six columns and five facts. Explain to him, without using the word "dimension", why the sixth column made the model *worse* rather than better — and why training it twice gave two different answers.
2. Two arrows point the same way, one longer than the other. Why does turning both knobs still leave you on a single line? Answer once in words and once in one line of algebra.
3. In your own words: what is the difference between the arrows you were handed and their span?
4. Someone writes "the house is [3, 2]". What question do you have to ask before that sentence means anything?

## Level 2 — Calculate

Take $\mathbf{v} = [2, 1]$, $\mathbf{w} = [-1, 2]$ and $\mathbf{u} = [4, 2]$.

1. Compute $3\mathbf{v} - 2\mathbf{w}$ and $\tfrac{1}{2}\mathbf{v} + \mathbf{w}$.
2. Find $a$ and $b$ with $a\mathbf{v} + b\mathbf{w} = [7, 4]$. Check your answer by substituting it back.
3. Show $\{\mathbf{v}, \mathbf{u}\}$ is dependent by finding coefficients, not both zero, that combine them to $\mathbf{0}$.
4. Describe $\operatorname{span}(\mathbf{v}, \mathbf{u})$ in words, then state its dimension.
5. Find the coordinates of $[3, 2]$ in the basis $\{[1,1], [-1,1]\}$, and verify them.
6. Find the coordinates of the *same* point in the basis $\{[1,0], [1,1]\}$. Two different pairs of numbers, one unmoved point — say which part of §17 that demonstrates.

## Level 3 — Derive

1. Prove from the zero-combination test that if $\mathbf{w} = c\,\mathbf{v}$ then $\{\mathbf{v}, \mathbf{w}\}$ is dependent.
2. Prove the converse for two non-zero vectors: if they are dependent, one is a multiple of the other.
3. Show that any set containing $\mathbf{0}$ is never independent, and say what that implies about an all-zero column in a spreadsheet.
4. Reproduce §15's uniqueness argument without looking: if $\{\mathbf{v}, \mathbf{w}\}$ is a basis, every vector has exactly one recipe in it.
5. Let $\mathbf{u} = p\mathbf{v} + q\mathbf{w}$ in three dimensions. Show that $\operatorname{span}(\mathbf{v}, \mathbf{w}, \mathbf{u}) = \operatorname{span}(\mathbf{v}, \mathbf{w})$, and identify exactly where the third knob disappears.

## Level 4 — Predict

1. In the studio's **Line, plane, space**, predict the readout's direction count at each of the four beats *before* sliding to it. Where were you wrong, and why?
2. In **Everywhere you can reach**, predict what you must do to the two arrows to make the span collapse to a line. Then do it, and write down the relationship the two coordinate pairs ended up in.
3. Predict the coordinates of the muted arrow in **A different language** when you rotate the basis so the blue arrow lies along the muted one. Check it.
4. Can three arrows in $\mathbb{R}^3$ span only a line? Predict, then construct an example or explain why none exists.

## Level 5 — Build it

No linear-algebra library beyond array arithmetic.

1. `combine(vectors, coefficients)` — return the linear combination. Test it against your Level 2 answers.
2. `is_dependent(vectors, tol)` — return whether a set is dependent. Test on `[[1,2],[2,4]]`, on `[[1,2],[2,4.001]]`, and on a set containing a zero vector.
3. `coordinates(x, basis)` — return a vector's coordinates in a given basis, and raise a clear error when the basis is not one.
4. Say what value of `tol` you chose in (2) and what it is really deciding. Then find a pair of columns where two defensible choices of `tol` disagree.

## Level 6 — Investigate

Build the four-house table with all six of Ramesh's columns.

1. For every pair of columns, compute the cosine similarity from Chapter 007's preview, and find the pair that scores $1.000$.
2. Round the square-metre column to one decimal place and score that pair again. What does it read now?
3. Form a hypothesis about what threshold would catch a *nearly* redundant column without flagging honest ones. Test it on the sheet.
4. Train the model twice from different random starts, once on six columns and once on five, and measure how far the learned weights drift between runs in each case. Relate what you see to §15's uniqueness argument.
5. Say what it would cost the office if the threshold were wrong in each direction — too strict, and too loose.

## Mastery check

- Why is counting columns not the same as counting information?
- What question does each of linear combination, span, independence and basis answer?
- Why can stretching an arrow never give you a new direction?
- Why do two arrows sometimes span a plane and sometimes only a line?
- In three dimensions, what has to be true of a third arrow for the span to become the whole space?
- Why does independence make a vector's coordinates unique, and what breaks in training when it fails?
- What has to be named before a list of numbers identifies a vector?
