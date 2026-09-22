# Chapter 005 — Numbers Become Vectors

> **The Big Question:** What changes when one number is no longer enough to describe one thing?

---

## The Morning After the Fifth House

Yesterday, in Chapter 001, you taught a machine something.

Four houses had sold:

| Rooms | Sold for (₹ lakh) |
|---:|---:|
| 1 | 3 |
| 2 | 5 |
| 3 | 7 |
| 4 | 9 |

The machine discovered the compact rule $\hat{y}=wx+b$.

Training pushed the two dials close to $w=2$ and $b=1$, so the machine learned $\hat{y}=2x+1$.

Five rooms? $\hat{y}=2(5)+1=11$.

Meera was happy.

Ramesh was suspicious.

You went home feeling that machine learning might actually make sense.

Then the next morning arrives.

Meera puts two files on your desk.

Both houses have **three rooms**.

One is a compact 800-square-foot flat facing the road.

The other is a 1,600-square-foot flat facing a park.

They did not sell for the same price.

You already know what your model will do.

For both houses, $x=3$.

Therefore, for both houses, $\hat{y}=2(3)+1=7$.

Same input.

Same prediction.

Every time.

Meera looks at you.

> “The houses are different. Why does the machine think they are the same?”

That question is the beginning of linear algebra.

---

## The Model Is Not Stupid. It Is Blind.

The problem is not gradient descent.

It is not the learning rate.

It is not that you need more training.

No amount of training can fix this:

$$
x_{\text{small flat}} = 3, \qquad x_{\text{large flat}} = 3.
$$

The machine receives the same number for both houses.

So any rule of the form $\hat{y}=f(x)$ must give the same answer to both.

The missing information is sitting right in the files:

- number of rooms,
- floor area,
- perhaps age,
- perhaps floor number,
- perhaps distance from the station.

But your model sees only one of them.

> 💡 **The failure is not in learning. The failure is in representation.**
>
> The world contains more information than the input can currently hold.

Chapter 001 taught us how a machine can learn a rule.

Chapter 005 asks a more basic question:

> **What exactly are we going to give the machine as its input?**

---

## The First New Word: Feature

A **feature** is one measurable or encoded fact that we allow the machine to see.

For a house:

| Feature | Example value |
|---|---:|
| rooms | 3 |
| area | 800 sq ft |
| age | 6 years |
| floor | 4 |
| distance to station | 1.2 km |

Yesterday, the model had one feature: $x = \text{rooms}$.

Now we want at least two: $x_1 = \text{rooms}$ and $x_2 = \text{area}$.

For the smaller flat, $x_1 = 3$ and $x_2 = 800$.

For the larger flat, $x_1 = 3$ and $x_2 = 1600$.

At last the machine can see that they are different.

But we have created a new problem.

Yesterday the input was one thing: $x$.

Today the input is becoming $x_1, x_2, x_3, \ldots$

A photograph may have thousands or millions of measurements.

A language model may represent one token using thousands of learned numbers.

Are we really going to carry them around one by one?

We need a single mathematical object that can hold many numbers **without crushing them together**.

---

## 🧑‍💼 Ramesh's First Fix — “Just Add Them”

Ramesh has been waiting for this moment.

“Easy,” he says. “You still want one input. Make one input.”

For a two-room, 800-square-foot house, his score is $2+800=802$.

Problem solved.

Except it is not.

![Two bars showing that when rooms and area are added into one score, the area dominates while the room count becomes almost invisible.](assets/chapter-005-visual-3.svg)

What does the number 802 mean?

Is one room worth one square foot?

According to this encoding, yes.

If the house gains one room, the score changes $802\rightarrow803$.

If the house gains one square foot, the score also changes $802\rightarrow803$.

The two completely different changes become identical.

Worse, area is numerically hundreds of times larger than room count. The smaller feature almost disappears inside the larger one.

> ⚠️ **A Tempting Wrong Idea**
>
> “If the model accepts one number, compress all the features into one number.”
>
> The compression silently decides how the features should be combined **before the machine gets a chance to learn that combination**.

The whole point of learning is to let the machine discover how much each feature should matter.

So the features must travel together while remaining separate.

---

## What Would a Real Representation Need?

Before inventing the solution, write down what it must do.

A useful representation should:

1. **Hold several features together** as one mathematical object.
2. **Keep their identities separate.** Rooms must not blur into area.
3. **Preserve order.** Slot 1 cannot mean rooms today and age tomorrow.
4. **Scale to many features.** Two features now; thousands later.
5. **Work with arithmetic.** We should be able to add changes, scale them, compare them, and eventually learn with them.

That list forces the next idea into existence.

---

## 🔍 The Discovery — Put the Features in One Ordered Box

Instead of carrying two separate symbols, $x_1=3$ and $x_2=800$, put them together:

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 800 \end{bmatrix}.
$$

This is a **vector**.

Read it as:

$$
\mathbf{x} = \begin{bmatrix} \text{rooms} \\ \text{area} \end{bmatrix}.
$$

For the compact flat:

$$
\mathbf{x}_{A} = \begin{bmatrix} 3 \\ 800 \end{bmatrix}.
$$

For the larger flat:

$$
\mathbf{x}_{B} = \begin{bmatrix} 3 \\ 1600 \end{bmatrix}.
$$

Same room count.

Different vector.

The machine can finally tell the two houses apart.

![A house's rooms, area and age flow into labelled slots of a feature vector before the vector is passed to a model.](assets/chapter-005-visual-7.svg)

The vector is not the house.

It is not even everything we know about the house.

It is the **numerical description we chose to give the machine**.

That distinction matters.

> ### Mental model
>
> **A feature is one fact.**
>
> **A feature vector is the collection of facts the machine sees about one example.**

---

## The Smallest Important Change From Chapter 001

Yesterday: $x=3$.

One input number.

Today:

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 800 \end{bmatrix}.
$$

Several input numbers.

That is the entire conceptual jump.

A scalar became a vector.

We usually write scalars with ordinary letters such as $x$, $y$, $w$, and $b$.

We usually write vectors in bold, such as $\mathbf{x}$, $\mathbf{w}$, and $\mathbf{v}$.

The boldface is not decoration.

It tells your brain:

> “This symbol contains several coordinates.”

If the vector contains $d$ features, we write $\mathbf{x} \in \mathbb{R}^{d}$.

Do not let that notation sound grander than it is.

It simply means:

> $\mathbf{x}$ contains $d$ real numbers.

For our two-feature house, $\mathbf{x} \in \mathbb{R}^{2}$.

For a 784-pixel image flattened into one list, $\mathbf{x} \in \mathbb{R}^{784}$.

Same idea.

More slots.

---

## The Order Is Part of the Meaning

Suppose we agree that

$$
\mathbf{x} = \begin{bmatrix} \text{rooms} \\ \text{area} \end{bmatrix}.
$$

Then

$$
\begin{bmatrix} 3 \\ 800 \end{bmatrix}
$$

means three rooms and 800 square feet.

But

$$
\begin{bmatrix} 800 \\ 3 \end{bmatrix}
$$

does **not** mean the same thing.

The numbers are the same.

The meaning is different because the slots changed.

A vector is an **ordered** collection.

Think of a school bag with labelled pockets.

You may change what is inside a pocket.

You may not randomly change what the pocket means.

That rule becomes extremely important later when vectors have thousands of coordinates.

---

## From a List of Numbers to a Point in Space

So far a vector is useful bookkeeping.

Now comes the reason linear algebra becomes visual.

Take a simpler vector:

$$
\mathbf{x} = \begin{bmatrix} 2 \\ 8 \end{bmatrix}.
$$

Suppose the first coordinate is rooms and the second is area measured in hundreds of square feet.

We can draw those two numbers as the point $(2,8)$.

Two coordinates become one location.

And if we draw an arrow from the origin to that point, the same vector becomes an arrow.

![House feature vectors plotted as points and arrows in a two-dimensional feature space.](assets/chapter-005-visual-1.svg)

So one vector now has three readings:

| View | What $\mathbf{x}$ means |
|---|---|
| **Data** | the features describing one house |
| **Point** | where that house sits in feature space |
| **Arrow** | a direction and amount from the origin |

Nothing changed in the numbers.

Only our interpretation changed.

This is the first deep idea of linear algebra:

> **Numbers can become geometry.**

That is why this level exists.

---

## Why the Arrow Picture Is More Than Decoration

Suppose a house currently has

$$
\mathbf{x} = \begin{bmatrix} 2 \\ 800 \end{bmatrix}.
$$

Now the owner adds one room and 200 square feet.

That change is

$$
\Delta \mathbf{x} = \begin{bmatrix} 1 \\ 200 \end{bmatrix}.
$$

The new house is

$$
\mathbf{x}_{\text{new}} = \mathbf{x} + \Delta \mathbf{x}.
$$

Numerically,

$$
\begin{bmatrix} 2 \\ 800 \end{bmatrix} + \begin{bmatrix} 1 \\ 200 \end{bmatrix} = \begin{bmatrix} 3 \\ 1000 \end{bmatrix}.
$$

Geometrically, vector addition means: walk one arrow, then continue with the other.

![Vector addition shown tip-to-tail, alongside scalar multiplication stretching and reversing an arrow.](assets/chapter-005-visual-4.svg)

Now suppose we double the **change**:

$$
2\Delta \mathbf{x} = 2 \begin{bmatrix} 1 \\ 200 \end{bmatrix} = \begin{bmatrix} 2 \\ 400 \end{bmatrix}.
$$

Multiplying a vector by one number stretches every coordinate by the same factor.

That number is called a **scalar**.

So we have two basic moves: vector addition, $\mathbf{u}+\mathbf{v}$, and scalar multiplication, $c\mathbf{u}$.

Add vectors.

Scale vectors.

Those two moves look almost too simple to deserve names.

But Chapter 006 will ask what entire spaces can be built using nothing but those two operations.

---

## The House Still Needs a Price

We have solved the representation problem.

We have **not** solved the prediction problem.

The machine now sees

$$
\mathbf{x} = \begin{bmatrix} \text{rooms} \\ \text{area} \end{bmatrix},
$$

but Chapter 001 taught us that a model needs adjustable dials.

Yesterday there was one input feature, so there was one main weight: $\hat{y}=wx+b$.

If there are two input features, each needs its own weight:

$$
\hat{y} = w_1x_1 + w_2x_2 + b.
$$

For house pricing, we can read that as

$$
\hat{y} = w_{\text{rooms}}x_{\text{rooms}} + w_{\text{area}}x_{\text{area}} + b.
$$

One dial controls how much room count matters.

Another controls how much area matters.

The bias still shifts the whole prediction.

Nothing from Chapter 001 was discarded.

We simply gave the model more information and therefore more dials.

---

## A Small Numerical Example

Take

$$
\mathbf{x}_{A} = \begin{bmatrix} 3 \\ 800 \end{bmatrix}.
$$

Choose, just for illustration,

$$
w_{\text{rooms}} = 1, \qquad w_{\text{area}} = 0.005, \qquad b=0.
$$

Then

$$
\hat{y}_{A} = 1(3) + 0.005(800) = 3+4 = 7.
$$

Now use the larger flat:

$$
\mathbf{x}_{B} = \begin{bmatrix} 3 \\ 1600 \end{bmatrix}.
$$

The same rule gives

$$
\hat{y}_{B} = 1(3) + 0.005(1600) = 3+8 = 11.
$$

The two houses have the same room count.

But area has its own weight, so the model can finally give different predictions.

This is exactly what the one-number model could not do.

---

## What the Weights Really Mean

Notice the units.

If price is measured in lakh rupees, then $w_{\text{rooms}}$ has units of $\frac{\text{lakh}}{\text{room}}$, while $w_{\text{area}}$ has units of $\frac{\text{lakh}}{\text{square foot}}$.

The weights are allowed to be very different numbers because the features mean different things.

That is why Ramesh's “just add the features” trick was wrong.

He combined the measurements before giving the machine a chance to learn separate weights.

A useful mental model is:

> **The feature vector says what the example contains.**
>
> **The weight vector says how strongly the model currently cares about each feature.**

So let us put the weights into a vector too.

$$
\mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \end{bmatrix}.
$$

Now both sides of the model have the same shape:

$$
\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \end{bmatrix}.
$$

One feature.

One matching weight.

Slot by slot.

---

## 🔍 The Next Compression — Pair, Multiply, Add

The prediction still looks like this:

$$
\hat{y} = w_1x_1 + w_2x_2 + b.
$$

With five features:

$$
\hat{y} = w_1x_1 + w_2x_2 + w_3x_3 + w_4x_4 + w_5x_5 + b.
$$

With a million:

$$
\hat{y} = w_1x_1 + w_2x_2 + \cdots + w_{1000000}x_{1000000} + b.
$$

The idea is simple.

The notation is becoming ridiculous.

Look at the repeated instruction:

1. pair each feature with its weight,
2. multiply each pair,
3. add the products.

![Features pair with weights, each pair is multiplied, and the products are added into one score.](assets/chapter-005-visual-8.svg)

Mathematics gives that repeated operation a name:

**dot product**.

We write

$$
\mathbf{w}\cdot\mathbf{x} = \sum_{i=1}^{d}w_i x_i.
$$

So the whole model becomes

$$
\boxed{ \hat{y} = \mathbf{w}\cdot\mathbf{x} + b }
$$

That is Chapter 001's model grown up.

When $d=1$, $\mathbf{w}\cdot\mathbf{x}=wx$, so we recover $\hat{y}=wx+b$.

When $d=2$, $\mathbf{w}\cdot\mathbf{x}=w_1x_1+w_2x_2$.

When $d=1000000$, the notation does not change.

Only the number of coordinates changes.

> ### Mental model
>
> When you see
>
> $$
> \mathbf{w}\cdot\mathbf{x},
> $$
>
> hear:
>
> **pair, multiply, add.**

That is all we need from the dot product today.

Chapter 007 will return to it and ask a much deeper question:

> Why does this same arithmetic also measure how two directions align?

Do not steal that discovery from future-you.

---

## This Is Already the Beginning of a Neuron

There is one connection worth seeing now.

A basic artificial neuron begins with $z=\mathbf{w}\cdot\mathbf{x}+b$.

That is the same weighted sum we just built.

Later the neuron applies another function to $z$, called an activation function.

For now, stop before that step.

The important connection is simply this:

> The arithmetic inside a neuron starts with the same feature-vector and weight-vector story we just discovered from house prices.

Deep learning did not invent a completely different mathematics.

It builds on this one.

---

## Shapes — Start Reading Them Now

Chapter 001 ended by asking you to notice shapes.

Keep that habit.

For one house with two features:

```text
x = [rooms, area]
shape: (2,)
```

For the matching weights:

```text
w = [room_weight, area_weight]
shape: (2,)
```

The dot product

```text
w · x
```

returns one number.

Why?

Because every feature contributes one weighted vote, and the votes are added together.

So:

```text
(d,) · (d,)  →  scalar
```

If the two vectors do not have the same number of slots, the operation has no sensible pairings.

That shape check will save you from an absurd number of bugs later.

---

## One Warning About Scale

There is a subtle problem hiding in our house vector:

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 800 \end{bmatrix}.
$$

The number 800 is much larger than 3.

But that does not mean area is automatically “more important” than rooms.

It may simply be measured in different units.

Write the same area in square metres and the coordinate changes again, even though the house does not.

So numerical size and real-world importance are not the same thing.

We will handle feature scaling properly later.

For now remember:

> **A vector contains coordinates, and coordinates inherit the units we choose.**

That choice affects the geometry.

---

## ⚠️ Five Ways a Feature Vector Can Lie

A vector is useful only if its coordinates mean what we think they mean.

| Failure | What went wrong |
|---|---|
| **Wrong order** | The model thinks slot 1 means rooms, but you put area there. |
| **Missing feature** | Price depends strongly on location, but location never enters $\mathbf{x}$. |
| **Meaningless numeric code** | Encoding Delhi $=1$, Mumbai $=2$, Chennai $=3$ accidentally invents distances and order that may have no meaning. |
| **Bad scale** | One coordinate dominates geometry mainly because of its units. |
| **Duplicate information** | Area in square feet and area in square metres are the same fact written twice. |

The last one is especially interesting.

If two coordinates are different numbers but contain the same information, how can we tell?

That is exactly where Chapter 006 begins.

---

## A Glimpse of Matrices — Do Not Learn Them Yet

One weight vector asks one weighted question of one input: $\mathbf{w}\cdot\mathbf{x}$.

A neural-network layer may want many such weighted questions.

So later we will stack many weight vectors together.

That stack is a matrix:

$$
W = \begin{bmatrix} \mathbf{w}_1^{\mathsf{T}} \\ \mathbf{w}_2^{\mathsf{T}} \\ \vdots \\ \mathbf{w}_m^{\mathsf{T}} \end{bmatrix}.
$$

Then $W\mathbf{x}$ computes many weighted combinations of the same input.

![Several weight-vector rows are stacked into a matrix; each row meets the same input vector and produces one output.](assets/chapter-005-visual-9.svg)

Do not memorize matrix multiplication here.

Chapter 008 will earn matrices properly, and Chapter 009 will show what matrix-vector multiplication does geometrically.

For today keep one preview:

> **vector = one collection of coordinates**
>
> **matrix = a structured collection of vectors**

That is enough.

---

## 🎯 Where This Goes

Yesterday's model, $\hat{y}=wx+b$, was not thrown away.

It expanded.

Today it is $\hat{y}=\mathbf{w}\cdot\mathbf{x}+b$.

The same pattern will appear again and again:

| Today | Later |
|---|---|
| house features | model input |
| $\mathbf{x}$ | feature vector / representation |
| $\mathbf{w}$ | learned weights |
| $\mathbf{w}\cdot\mathbf{x}$ | weighted score |
| many weight vectors | matrix |
| repeated vector transformations | neural-network layers |
| learned vectors | embeddings |

A photograph, a sound segment, a customer, a token, or a hidden state can all be represented by collections of numbers.

The meaning changes.

The mathematics survives.

---

## Distinctions That Matter

| | |
|---|---|
| **Scalar** — one number | **Vector** — several ordered coordinates |
| **Feature** — one fact | **Feature vector** — the facts describing one example |
| **World** — the actual house | **Representation** — the numbers the model is allowed to see |
| **Coordinate** — one slot in a vector | **Dimension** — how many slots the vector has |
| **$\mathbf{x}$** — what the example contains | **$\mathbf{w}$** — how the model weights those contents |
| **Element-wise multiplication** — keeps separate products | **Dot product** — pair, multiply, then add to one number |
| **Point view** — where the example is | **Arrow view** — direction and amount from the origin |

---

## What We Discovered

1. Chapter 001's learning loop did not fail. Its **input representation** did.
2. A model cannot use information that never appears in its input.
3. One measurable or encoded fact is a **feature**.
4. Several features can travel together as one ordered object: a **vector**.
5. The same vector can be read as data, as a point, or as an arrow.
6. Vector addition means combining changes; scalar multiplication stretches a vector.
7. Each feature can have its own learned weight.
8. The repeated operation “pair each feature with its weight, multiply, add” is the **dot product**.
9. Therefore the one-feature model $\hat{y}=wx+b$ becomes $\hat{y}=\mathbf{w}\cdot\mathbf{x}+b$.
10. That weighted sum is also the arithmetic at the entrance of a neuron.
11. Some features can be redundant even when their numbers look different. That unanswered problem leads directly to Chapter 006.

---

## The Mathematics We Built

A $d$-dimensional feature vector:

$$
\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_d \end{bmatrix} \in \mathbb{R}^{d}.
$$

A matching weight vector:

$$
\mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \\ \vdots \\ w_d \end{bmatrix} \in \mathbb{R}^{d}.
$$

Vector addition:

$$
\mathbf{u}+\mathbf{v} = \begin{bmatrix} u_1+v_1 \\ u_2+v_2 \\ \vdots \\ u_d+v_d \end{bmatrix}.
$$

Scalar multiplication:

$$
c\mathbf{u} = \begin{bmatrix} cu_1 \\ cu_2 \\ \vdots \\ cu_d \end{bmatrix}.
$$

Dot product:

$$
\mathbf{w}\cdot\mathbf{x} = \sum_{i=1}^{d} w_i x_i = w_1x_1+w_2x_2+\cdots+w_dx_d.
$$

The multi-feature linear model:

$$
\boxed{ \hat{y} = \mathbf{w}\cdot\mathbf{x} + b }
$$

One line replaced a formula that could have contained a million repeated terms.

That compression is what good mathematical notation is for.

---

## What Each Symbol Means

| Symbol | Read it as | Meaning here |
|---|---|---|
| $x_i$ | “x sub i” | one feature value |
| $\mathbf{x}$ | “vector x” | all features for one example |
| $d$ | “dimension” | number of coordinates/features |
| $w_i$ | “weight i” | learned importance attached to feature $i$ |
| $\mathbf{w}$ | “weight vector” | all feature weights |
| $b$ | “bias” | adjustable offset |
| $\mathbf{w}\cdot\mathbf{x}$ | “w dot x” | pair, multiply, add |
| $\hat{y}$ | “y-hat” | model prediction |

---

## The One-Minute Version

Chapter 001 gave the machine one number per house: the number of rooms.

Then two houses appeared with the same room count but different floor areas.

The machine could not distinguish them because both entered the model as the same input.

So we introduced **features**.

Instead of one number, a house became

$$
\mathbf{x} = \begin{bmatrix} \text{rooms} \\ \text{area} \end{bmatrix}.
$$

That ordered collection is a vector.

Each feature gets its own weight, so the old rule $\hat{y}=wx+b$ becomes $\hat{y}=\mathbf{w}\cdot\mathbf{x}+b$.

The vector tells the machine what the house contains.

The weight vector tells the model how to combine those facts.

The dot product means pair, multiply, add.

That is the doorway from one-variable machine learning into linear algebra.

---

## 🔭 Bridge to Chapter 006 — When Is a New Feature Actually New?

Ramesh likes the new spreadsheet.

Too much.

The next morning he adds more columns:

- rooms,
- area in square feet,
- area in square metres,
- age,
- floor,
- distance to station.

“Six features,” he says proudly.

But area in square feet and area in square metres are not two independent facts.

If you know one, you can calculate the other exactly.

So the vector has six coordinates but perhaps fewer than six genuinely new directions of information.

Now the question changes.

Not:

> “How many numbers are in the vector?”

But:

> **“How many genuinely different directions does this vector space contain?”**

To answer that, we need to understand what vectors can build when we add and scale them.

That gives us three ideas:

**span, linear independence, and basis.**

**Next → Chapter 006: How Many Directions Do You Actually Need?**
