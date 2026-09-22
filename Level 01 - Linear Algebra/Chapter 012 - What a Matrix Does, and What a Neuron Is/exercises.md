# Chapter 012 Exercises — What a Matrix Does, and What a Neuron Is

## Level 1 — Explain

1. A matrix has 10,000 entries. How many arrows do you actually need to understand to know what it does? Explain why.
2. In your own words: what is the difference between what a matrix *is* and what a matrix *does*?
3. Why can a transformation that destroys a direction never be undone — not "is difficult to undo", but cannot?

## Level 2 — Calculate

For `A = [[2, 1], [1, 3]]` and `B = [[1, 2], [2, 4]]`:

1. Compute `Ae₁`, `Ae₂`, `Be₁`, `Be₂`, and confirm each is a column.
2. Compute `A[1, -1]` row by row, then as `1(Ae₁) + (−1)(Ae₂)`. Confirm they agree.
3. Find a non-zero `n` with `Bn = 0`.
4. State the rank and nullity of each matrix and check both sum to 2.
5. A neuron has `w = [0.8, 0.6]`. Compute `w·x` for `x = [3, 4]`, `x = [-3, 4]` and `x = [-0.8, -0.6]`, and say in words what each answer means.

## Level 3 — Derive

1. Prove a linear map from `ℝ³` to `ℝ²` must have a non-trivial null space, whatever its entries.
2. Prove that if `A` has a non-trivial null space, no `C` satisfies `CA = I`.
3. Show the composition of two linear maps is linear — the fact §7 leans on.
4. Show that if every `‖wᵢ‖ = 1` and the `wᵢ` are mutually at right angles, then `z = Wx` is exactly the coordinates of `x` in that basis.

## Level 4 — Investigate

1. Multiply a hundred random 4×4 matrices into one `W`. Confirm that pushing a vector through the stack layer by layer equals `Wx` to machine precision.
2. Insert a ReLU between each pair and show it no longer does.
3. Take the tangled one-dimensional data from §7 and find weights for a two-neuron ReLU layer that makes the two colours linearly separable.
4. Build a random 8×3 weight matrix, compute its rank, then set one row to the sum of two others and recompute. Explain what changed about the layer's output space.

## Level 5 — Design

You are designing a layer that maps 512 numbers to 32. By §3 you are deliberately destroying at least 480 directions.

Decide what that layer should keep and what you are content to lose. Then say how you would test — *after* training — whether it threw away something it needed. Finally: if it did, what would that look like in the model's behaviour, and how would you distinguish it from simply not having trained long enough?

## Mastery check

- What do the columns of a matrix record, and why does that determine everything else?
- What is the relationship between rank, nullity and the input dimension?
- Why can a transformation with a non-trivial null space never be inverted?
- In what sense is a neuron a direction, and a layer a basis?
- Why does a stack of linear layers collapse, and what exactly does the activation add?
