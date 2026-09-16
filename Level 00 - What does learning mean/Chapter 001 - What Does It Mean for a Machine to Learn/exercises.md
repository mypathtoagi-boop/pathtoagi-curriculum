# Chapter 001 Exercises — What Does It Mean for a Machine to Learn?

Work from explanation to calculation, then test the assumptions behind the model. Show your reasoning before checking code.

## Level 1 — Explain

1. Why does a lookup table that answers the four observed houses perfectly fail to demonstrate learning?
2. Explain the difference between prediction and learning without using equations.
3. Why can’t the average signed error be the dataset objective for the flat ₹6 lakh model?
4. A colleague proposes training the model on “percentage of quotes within ₹1,000 of the truth”. Explain why that measure cannot guide learning, even though it is a reasonable thing to report.
5. The input `x` and the parameter `w` both appear in `ŷ = wx + b`. Explain to someone with no mathematics why training is allowed to change one and not the other.

## Level 2 — Calculate

For the four houses `(x, y) = (1, 3), (2, 5), (3, 7), (4, 9)`, use `ŷ = wx + b` with `w = 3`, `b = 0`.

1. Write all four predictions and errors.
2. Compute the MSE.
3. Compare it with the model `w = 0`, `b = 6`.
4. Take one gradient-descent step with `η = 0.01` and verify that the loss falls.
5. Two models produce errors `2, 2, 2, 2` and `0, 0, 0, 8`. Compute the mean absolute error and the mean squared error of each. Which model would you deploy for Meera, and what does your answer say about the loss you should train on?
6. For the single house `y = 7`, tabulate `|ŷ − 7|` and `(ŷ − 7)²` at `ŷ = 6.9, 6.99, 7, 7.01, 7.1`. Measure the slope of each on both sides of 7 using `h = 0.01`, and state precisely what goes wrong for the absolute cost at the minimum.

## Level 3 — Derive

With `b = 0`, prove that the MSE is

$$
L(w) = 7.5w^2 - 35w + 41.
$$

Set the slope to zero. You should get `w = 7/3` and a minimum loss of `1/6`. Explain why the best weight is not exactly 2 when the plot cost is unavailable.

## Level 4 — Investigate

Find the largest learning rate `η` that still converges, to two decimal places, for the four-house data. Then change the inputs to `[10, 20, 30, 40]` while keeping the prices `[3, 5, 7, 9]`. Predict how the stability threshold changes and explain your result using

$$
\frac{1}{n}\sum_i x_i^2.
$$

## Level 5 — Design

Design a loss for house pricing that punishes overcharging more heavily than undercharging. Write the rule mathematically, explain why it matches the business goal, and describe how it would change the model’s behaviour.

Then design against yourself: one of the five houses in your training data was entered wrongly and is off by a factor of ten. Describe what squared error does to that training run, propose a loss that is harder to hijack by a single bad row, and say what you give up by adopting it.

## Mastery check

- Why is perfect performance on seen examples insufficient evidence of learning?
- What do `w` and `b` represent in the house model?
- Why must a loss discard the cancellation caused by signed errors?
- How does the slope tell the model which way to update a parameter?
- Why can a learning rate that is too small or too large both fail?
- Which quantities in the training loop come from the world, and which belong to the model?
- Why does giving every house its own private `w` and `b` destroy the point of learning?
- Why is the slope of the absolute cost undefined at zero, and why does that matter to an optimizer?
