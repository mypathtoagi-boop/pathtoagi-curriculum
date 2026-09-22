# Chapter 006 — Directions, Span and Basis

> **The Big Question:** When is a new measurement genuinely new — and what does a machine do with the ones that are not?

## Where We Are

Chapter 005 turned a house into a vector: several measurements, kept apart, travelling together. One dot product prices it.

It also handed you two operations and then moved on rather quickly. You can **stretch** a vector, and you can **add** two of them. That is the entire toolkit. It looks like almost nothing.

This chapter does one thing: it takes those two operations and asks how far they go.

> **How much of a space can I build using only a few vectors?**

That question is not idle. Answering it forces four ideas into existence — **linear combination**, **span**, **independence** and **basis** — and those four are the vocabulary for everything that follows. By Chapter 009 they are how a matrix is read. By Chapter 012 they are how a neuron is described. By Chapter 016 they are how a model compresses a dataset.

> **Today:** how many genuinely different directions a set of arrows contains, and how to describe a thing once you have them.
>
> **Next:** why pairing numbers, multiplying them and adding them up has anything to do with an angle.

---

## The Picture to Hold in Your Head

One question, in four disguises.

**Linear combination** asks: *what can I make out of these arrows?* **Span** asks: *where can I get to?* **Independence** asks: *is any of these arrows unnecessary?* A **basis** is the answer that is neither too little nor too much — enough directions to reach everything, with nothing spare. And then **coordinates** stop being "the numbers" and become what they always were: instructions for a walk along arrows somebody chose.

Hold one image for the whole chapter: **two knobs.** One controls how much of the first arrow you use, the other how much of the second. Turn them and watch what your arrowhead can touch. Everything below is that picture, counted carefully.

> 🧪 **The studio is the chapter.** Six scenes, in the order the argument runs: **One knob**, **Two knobs**, **Everywhere you can reach**, **A copy is not a direction**, **Line, plane, space**, **A different language**. Open the first one now and leave it open. When the prose says the reachable set collapses to a line, you should be watching it happen.

---

## 1. The Problem: Ramesh Improves the Spreadsheet

Chapter 005 ended well. Meera stopped asking why two-room houses got the same quote, and Ramesh stopped calling it "your computer thing."

Which, for Ramesh, was fatal. A man with nineteen years of filing instincts and a newly working system will improve it.

On Thursday he hands you the sheet.

![Ramesh's six-column sheet: rooms, area in square feet, area in square metres, age, floor, and minutes to the station, with the two area columns bracketed together as one fact recorded twice](assets/chapter-006-visual-1.svg)

"Six columns now," he says. "More information, better chart."

You run it. And the model gets *worse* — not dramatically, but in a way that makes your skin crawl: **train it twice and you get two different answers.**

| Run | lakh per room | lakh per sq ft | lakh per sq m | Loss on the four houses |
|---|---:|---:|---:|---:|
| first | 1.9 | 0.0031 | 0.0209 | 0.0004 |
| second | 0.4 | 0.0049 | 0.0015 | 0.0004 |

Both fit the four houses equally well. Both cannot be right about what a room is worth. And nothing in the output says which one to trust, because as far as the loss is concerned there is nothing to choose between them.

Meera has no interest in your debugging, only in the question underneath it:

> "Which of these columns is actually earning its place?"

Look at the sheet again. Area in square feet. Area in square metres. Those are not two measurements. That is **one measurement, written down twice**, because 800 sq ft *is* 74.3 sq m — always, for every house, forever. The conversion never varies.

The machine cannot know that. It sees two columns of numbers and dutifully hunts for a dial for each. And because the second column says nothing the first did not already say, there are infinitely many pairs of dials that fit exactly as well — which is precisely why two runs disagree and neither is wrong.

> 💡 **This is not a spreadsheet problem. It is a geometry problem** — and geometry has an exact answer for it, which is what this chapter is about.

---

## 2. What Would an Answer Need?

Before inventing anything, write down what a real answer has to do. Four things.

1. **Spot a column that adds nothing**, even when its numbers look nothing like any other column's.
2. **Count the directions that are genuinely there** — not the columns. The directions.
3. **Say what the set can and cannot reach.** If the columns cannot express something, we should be able to name what.
4. **Keep working at 768 columns**, where nobody can eyeball anything at all.

Requirement 1 is the one that kills the obvious approach, so let us go and kill it.

---

## 3. First Attempt: Count the Columns

The sheet has six columns, so the machine has six measurements. Six dials, six directions. Obvious.

Except we have just established that it has **five facts**. The count says six, the truth says five, and no amount of staring at the numbers fixes it, because the square-metre column looks nothing like the square-foot column. `800` and `74.3` share no digits.

Worse, the same trap hides in shapes you would never spot. Suppose the office also records `total rooms` and, separately, `bedrooms` and `other rooms`. Three columns. Two facts — the third is always the sum of the other two, and the sum is invisible unless you go looking for it.

> ⚠️ **A Tempting Wrong Idea** — *"Count the columns and you know how much information you have."*
>
> Counting is not measuring. A column earns its place by **pointing somewhere the others cannot reach** — and that sentence is about geometry, not arithmetic.

So we need the geometric question. Chapter 005 gave us arrows. Let us ask what a set of arrows can actually get to.

---

## 4. The Two Operations You Already Have

Everything in this chapter is built from Chapter 005's two operations, so put them back on the table.

Take a vector:

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}
$$

**Stretch it.** Multiply by a number and every slot is multiplied:

$$
3\mathbf{v} = \begin{bmatrix} 6 \\ 3 \end{bmatrix}, \qquad -\mathbf{v} = \begin{bmatrix} -2 \\ -1 \end{bmatrix}, \qquad 0\mathbf{v} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

The direction survives. The length changes, and a negative number turns the arrow round.

**Add two of them.** Match the slots:

$$
\begin{bmatrix} 2 \\ 1 \end{bmatrix} + \begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 1 \\ 3 \end{bmatrix}
$$

Geometrically: walk along the first arrow, then walk along the second from wherever you ended up. Where you stop is the sum.

That is all we have. Two verbs. The rest of the chapter is a long answer to *what can you build with two verbs.*

---

## 5. The Discovery: A Linear Combination Is Two Knobs

Take two vectors and do both operations at once:

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} -1 \\ 2 \end{bmatrix}
$$

and build

$$
a\mathbf{v} + b\mathbf{w}
$$

where $a$ and $b$ are any numbers you like.

That expression has a name — a **linear combination** — and the name is the least interesting thing about it. Do not memorise it yet. Think about it like this instead:

> 💡 **Two arrows, two knobs.** One knob says how much of $\mathbf{v}$ to use. The other says how much of $\mathbf{w}$. Turn them and the tip of the result moves.

Work one out by hand. Take $a = 2$ and $b = 3$:

$$
2\mathbf{v} + 3\mathbf{w} = 2\begin{bmatrix} 2 \\ 1 \end{bmatrix} + 3\begin{bmatrix} -1 \\ 2 \end{bmatrix}
$$

Stretch each one first:

$$
= \begin{bmatrix} 4 \\ 2 \end{bmatrix} + \begin{bmatrix} -3 \\ 6 \end{bmatrix}
$$

Then add the slots:

$$
= \begin{bmatrix} 1 \\ 8 \end{bmatrix}
$$

You have just constructed a new vector out of old ones. Nothing was needed but stretching and adding.

The general form, for any number of arrows, is the same sentence written longer:

$$
c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k
$$

> 🧪 **Studio — "Two knobs."** Set $c_1$ and $c_2$ and watch the dashed walk: along the blue arrow first, then the pink one, ending at the gold arrowhead. Then try to land the gold arrowhead exactly on a point you choose. Notice that you are solving two equations by feel.

---

## 6. Why the Word "Linear"?

The word looks like decoration. It is not — it is the geometry of the simplest case.

Throw away the second arrow. Keep one:

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}
$$

Now try every possible multiple:

$$
\ldots,\; -2\mathbf{v},\; -\mathbf{v},\; \mathbf{0},\; \mathbf{v},\; 2\mathbf{v},\; 3\mathbf{v},\; \ldots
$$

Every one of those arrowheads lands on the same straight line through the origin.

```text
                     2v
                   /
                 v
               /
   ----------O--------------------
           /
        -v
      /
   -2v
```

One arrow, stretched every possible way, gives you a **line**. That is the geometric seed inside the word *linear*.

And notice what stretching cannot do, because the whole chapter leans on it: multiplying by a number can make an arrow longer, shorter, or point backwards. It can flatten it to nothing. **It can never rotate it.** No multiple of $[2,1]$ will ever be $[1,2]$.

> 🧪 **Studio — "One knob."** One arrow, one slider. Sweep it from $-2.5$ to $2.5$ and watch the reachable set: a line, and only a line. Then drag the arrowhead to a new direction. The line turns with it — but it is still a line.

---

## 7. The Discovery: Span — Everywhere the Knobs Can Reach

Now the second arrow comes back, pointing somewhere genuinely different:

```text
          w
          |
          |
          |
          O---------------> v
```

Travel some amount along $\mathbf{v}$. Travel some amount along $\mathbf{w}$. Add them:

$$
a\mathbf{v} + b\mathbf{w}
$$

Turn $a$ and $b$ continuously and the arrowhead sweeps out — potentially — every point of the plane.

```text
   .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .
   .  .  .  .  O  .  .  .  .
   .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .
```

The set of everything you can reach has a name: the **span**.

$$
\operatorname{span}(\mathbf{v}, \mathbf{w}) = \{\, a\mathbf{v} + b\mathbf{w} \;\mid\; a, b \in \mathbb{R} \,\}
$$

Do not learn the set notation first. Learn this:

> 💡 **Span = everywhere these arrows let me get to.**
>
> Not what you have. What you can reach.

The general version is the same idea with more arrows:

$$
\operatorname{span}\{\mathbf{v}_1, \ldots, \mathbf{v}_k\} = \{\, c_1\mathbf{v}_1 + \cdots + c_k\mathbf{v}_k \;\mid\; c_i \in \mathbb{R} \,\}
$$

Read it aloud as: *every place I can get to by taking some amount of each arrow and adding them up.*

![Three panels: one arrow reaches a line; two arrows pointing differently reach the whole plane; two arrows pointing the same way still reach only the line](assets/chapter-006-visual-2.svg)

> 🎥 **Watch this one now.** [*Linear combinations, span, and basis vectors*](https://youtu.be/k7RM-ot2NWY) — 3Blue1Brown, Chapter 2 of *Essence of Linear Algebra* (10 min), with the companion page at [3blue1brown.com/lessons/span](https://www.3blue1brown.com/lessons/span). The animation of two arrows sweeping out a plane is worth more than a page of this text, and you have now hit exactly the wall it was built for.

---

## 8. One Arrow in the Plane Spans a Line

Make the first claim precise, because the exceptions are where the chapter's real content lives.

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}
$$

What is $\operatorname{span}(\mathbf{v})$?

```text
                 /
               /
             /
   --------O--------
         /
       /
```

A line. Why can it be nothing else? Because $a\mathbf{v}$ can only make $\mathbf{v}$ longer, shorter, reverse it, or collapse it to zero. It cannot rotate it into a different direction, so no point off that line is ever reachable.

$$
\boxed{\text{one non-zero arrow spans one dimension}}
$$

The hedge in that sentence matters. *Non-zero.* The zero vector spans nothing but itself:

$$
a\mathbf{0} = \mathbf{0} \quad \text{for every } a
$$

A column of zeros in a spreadsheet is a column that reaches nowhere. It is not merely useless; §11 will show it makes a set dependent all by itself.

---

## 9. Two Arrows Usually Span the Whole Plane

Now the cleanest possible pair:

$$
\mathbf{v} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

Combine them:

$$
a\mathbf{v} + b\mathbf{w} = a\begin{bmatrix} 1 \\ 0 \end{bmatrix} + b\begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} a \\ b \end{bmatrix}
$$

Look at what came out. The knob settings *are* the answer's slots. Since $a$ and $b$ can be anything, every vector in the plane is reachable:

$$
\begin{bmatrix} 3 \\ 7 \end{bmatrix}, \qquad \begin{bmatrix} -2 \\ 4 \end{bmatrix}, \qquad \begin{bmatrix} 100 \\ -50 \end{bmatrix}
$$

Everything. So

$$
\operatorname{span}(\mathbf{v}, \mathbf{w}) = \mathbb{R}^2
$$

And it is not special to those two. Take the pair we started with, $\mathbf{v} = [2,1]$ and $\mathbf{w} = [-1,2]$, and ask for a specific target — say $[1, 8]$. We already know $2\mathbf{v} + 3\mathbf{w}$ lands there. Ask for $[5, 0]$ instead and you can solve it:

$$
a\begin{bmatrix} 2 \\ 1 \end{bmatrix} + b\begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 5 \\ 0 \end{bmatrix} \qquad\Longrightarrow\qquad \begin{aligned} 2a - b &= 5 \\ a + 2b &= 0 \end{aligned}
$$

which gives $a = 2$, $b = -1$. Check it: $2[2,1] - [-1,2] = [4,2] + [1,-2] = [5,0]$.

The same works for any target at all, **provided the two arrows point in genuinely different directions.**

That proviso is doing an enormous amount of work, and it is time to look at what happens when it fails.

> 🧪 **Studio — "Everywhere you can reach."** The faint cloud is the span: every $c_1\mathbf{v}_1 + c_2\mathbf{v}_2$ for a lattice of knob settings. Drag either arrow anywhere you like. The cloud keeps filling the plane — right up until the moment the two arrows line up, and then it does not.

---

## 10. A Tempting Wrong Idea: "Two Arrows Always Reach the Plane"

Take:

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} 4 \\ 2 \end{bmatrix}
$$

and notice:

$$
\mathbf{w} = 2\mathbf{v}
$$

The second arrow introduces no new direction. It is the first one, wearing a larger coat.

```text
                        w
                      /
                   /
                /
              v
            /
          /
   O---------------------------
```

Now consider any combination at all:

$$
a\mathbf{v} + b\mathbf{w}
$$

Substitute $\mathbf{w} = 2\mathbf{v}$ and the illusion collapses in one line:

$$
a\mathbf{v} + b(2\mathbf{v}) = (a + 2b)\,\mathbf{v}
$$

Whatever you do with two knobs, the result is *some single multiple of $\mathbf{v}$*. Two knobs, one direction. You are still trapped on the same line you were on with one arrow.

$$
\operatorname{span}(\mathbf{v}, \mathbf{w}) = \operatorname{span}(\mathbf{v}) = \text{a line}
$$

And there it is — **that is square feet and square metres**, in geometry. The square-metre column is $0.0929 \times$ the square-foot column: a second arrow lying exactly along the first. Ramesh added a column and added no direction, and the two knobs collapsed into one.

> ⚠️ **Two arrows do not guarantee two directions.** Neither do six columns guarantee six. The number of arrows is an upper bound on the number of directions, never a promise.

> 🧪 **Studio — "A copy is not a direction."** This scene opens with $\mathbf{v}_2 = 2\mathbf{v}_1$ deliberately. Sweep both knobs through their whole range and watch every recipe land on one line. Then nudge $\mathbf{v}_2$ a few degrees off and watch the plane snap back into existence. That snap is the whole chapter.

---

## 11. Linear Dependence: Somebody in the Group Is Unnecessary

Give the failure its name.

If one arrow in a set can already be built out of the others, it adds nothing. The set is **linearly dependent**.

$$
\mathbf{w} = 2\mathbf{v} \qquad\Longrightarrow\qquad \boxed{\{\mathbf{v}, \mathbf{w}\} \text{ is linearly dependent}}
$$

More generally: a set is dependent when at least one of its arrows lies in the span of the others. It is not adding reach; it is repeating reach that was already there.

> 💡 **Dependent = somebody in the group is unnecessary.**

That definition is fine for two arrows you can see. It is useless for 768 columns you cannot. So we need a test that does not require looking.

### The test that works when you cannot see

Here is the question that does it. **Can you combine the arrows, using at least one amount that is not zero, and land back exactly where you started?**

$$
c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k = \mathbf{0}
$$

Setting every $c$ to zero always works, and says nothing at all. The real question is whether there is *any other* way.

- If the **only** way to reach zero is all-zeros, the arrows are **linearly independent**.
- If there is **another** way, they are **dependent** — and that combination is a receipt proving one of them was redundant.

Why does the test work? Because a non-zero combination summing to zero can be rearranged. Suppose $c_1 \ne 0$:

$$
c_1\mathbf{v}_1 = -c_2\mathbf{v}_2 - \cdots - c_k\mathbf{v}_k
$$

Divide by $c_1$:

$$
\mathbf{v}_1 = -\frac{c_2}{c_1}\mathbf{v}_2 - \cdots - \frac{c_k}{c_1}\mathbf{v}_k
$$

Which says, in symbols: **$\mathbf{v}_1$ was already reachable from the others.** It never added anything. The receipt is the proof.

Try it on Ramesh's sheet. Write the square-foot column as $\mathbf{a}$ and the square-metre column as $\mathbf{m}$. We know $\mathbf{m} = 0.0929\,\mathbf{a}$, so

$$
0.0929\,\mathbf{a} - 1 \cdot \mathbf{m} = \mathbf{0}
$$

Not all zeros. Dependent. The test found in one line what the spreadsheet could never show — and it never needed to know that one column was feet and the other metres.

### Two consequences worth keeping

**Any set containing $\mathbf{0}$ is dependent.** Take the coefficient on the zero vector to be $1$ and every other coefficient to be zero:

$$
1 \cdot \mathbf{0} + 0\,\mathbf{v}_2 + \cdots + 0\,\mathbf{v}_k = \mathbf{0}
$$

A non-zero coefficient, a total of zero. Dependent, immediately. An all-zero column in a spreadsheet is not neutral — it is redundancy with a certificate.

**More arrows than dimensions is always dependent.** Three arrows in the plane cannot be independent, ever, no matter how carefully chosen. Two of them already reach everywhere the third can be, so the third is in their span.

![The arrows e1 and e2 from the origin, and a third arrow to one-one shown as a dashed walk along the first two, so it adds no new direction](assets/chapter-006-visual-3.svg)

The dashed walk is the receipt: $[1,1] = \hat{\mathbf{i}} + \hat{\mathbf{j}}$, so the third arrow was a place you could already get to. You will prove the general statement in Chapter 012, where it acquires the name **rank**.

---

## 12. Linear Independence: Every Arrow Earns Its Place

The good case, stated plainly.

$$
\mathbf{v} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

Can you build $\mathbf{w}$ by scaling $\mathbf{v}$? No — every multiple of $[1,0]$ has a second slot of zero.

Can you build $\mathbf{v}$ by scaling $\mathbf{w}$? No, for the mirror reason.

Each one contributes a direction the other cannot produce. Therefore:

$$
\boxed{\{\mathbf{v}, \mathbf{w}\} \text{ is linearly independent}}
$$

> 💡 **Independent = every arrow contributes a direction we did not already have.**

Run the zero test on them, to see it agree:

$$
c_1\begin{bmatrix} 1 \\ 0 \end{bmatrix} + c_2\begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} c_1 \\ c_2 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

forces $c_1 = 0$ and $c_2 = 0$. Only the useless solution. Independent.

> 🧠 **Predict before you read on.** Are $[1,2]$ and $[2,4.001]$ independent? Run the test in your head, then ask a second question: if those were two columns of a spreadsheet, would you *want* the answer the test gives you? Hold that thought — §20 is about exactly the gap between them.

---

## 13. The Same Story in Three Dimensions

This is the most important picture in the chapter, and it is where the ideas stop being about two arrows on a page and start being about dimension.

### One arrow

Take a single vector $\mathbf{v}$ in three-dimensional space. Its span is still

$$
\boxed{\text{a line}}
$$

Nothing changed. Stretching one arrow cannot rotate it, whatever room it lives in.

### Two arrows

Add $\mathbf{w}$, pointing somewhere off that line. The combinations

$$
a\mathbf{v} + b\mathbf{w}
$$

now have two knobs to turn, and as they vary the arrowhead sweeps out

$$
\boxed{\text{a flat sheet through the origin — a plane}}
$$

```text
                    /
              _____/_____
            /     /     /
          /______O_____/
               /
              /
```

Two knobs, two dimensions of freedom, one flat sheet. Note that it passes through the origin: setting both knobs to zero is always allowed, and always lands you at $\mathbf{0}$.

### A third arrow, and two completely different endings

Introduce $\mathbf{u}$ and consider

$$
a\mathbf{v} + b\mathbf{w} + c\mathbf{u}
$$

**Ending one: $\mathbf{u}$ lies in the plane already.** Then $\mathbf{u}$ is itself some recipe of the first two:

$$
\mathbf{u} = p\mathbf{v} + q\mathbf{w}
$$

Substitute it and the third knob dissolves:

$$
a\mathbf{v} + b\mathbf{w} + c(p\mathbf{v} + q\mathbf{w}) = (a + cp)\mathbf{v} + (b + cq)\mathbf{w}
$$

Still a combination of two arrows. Still that same plane. You gained a knob and no new reach.

```text
   v + w         ->  a plane
   add u inside  ->  the same plane
```

The three arrows are linearly dependent. This is §10's collapse, one dimension up.

**Ending two: $\mathbf{u}$ sticks out of the plane.** Now something genuinely new happens.

```text
                 u
                 |
                 |
        _________|______
      /          |     /
     /___________O____/
            v, w plane
```

Adding $c\mathbf{u}$ takes the whole plane and slides it — up for positive $c$, down for negative. And $c$ is continuous, so you do not get a few shifted copies. You get all of them:

```text
   ================
   ================
   =======O========
   ================
   ================
```

Stacked copies of a plane, sliding continuously, sweep out everything. Therefore

$$
\operatorname{span}(\mathbf{v}, \mathbf{w}, \mathbf{u}) = \mathbb{R}^3
$$

provided $\mathbf{u}$ is not already in the plane of $\mathbf{v}$ and $\mathbf{w}$.

> 💡 **The pattern is now unmistakable.** Each genuinely new arrow takes what you could already reach and sweeps it through one more direction. A point sweeps into a line. A line sweeps into a plane. A plane sweeps into space. The arrows that do nothing are the ones that sweep a set through itself.

> 🧪 **Studio — "Line, plane, space."** Drag the **Play** slider through the four beats: one arrow and its line; a second arrow and the sheet it opens; a third arrow lying *inside* that sheet, which changes nothing; and the same third arrow lifted out, which fills the space. Watch the readout count independent directions — 1, 2, 2, 3 — and notice that the count drops back when the third arrow lies flat.

---

## 14. Dimension Has a Better Meaning Now

Before this chapter you probably held something like:

> 2-D means $x$ and $y$.

That is not wrong so much as incomplete — it describes a convention rather than a fact. Here is the deeper meaning, and it is one you can now justify:

> 💡 **Dimension = how many independent directions you need to reach everything in the space.**

| Space | Independent directions needed | Dimension |
|---|---:|---:|
| a line through the origin | 1 | 1 |
| a plane through the origin | 2 | 2 |
| ordinary space | 3 | 3 |
| a small handwritten digit's pixels | 784 | 784 |
| a word-embedding space | 768 | 768 |

$$
\mathbb{R}^{1000} \quad \text{means: up to 1000 independent directions are needed.}
$$

You do not have to picture a thousand directions. Nobody can. The algebra keeps working regardless — which is Chapter 005's habit again: **reason in 2-D, compute in 1000-D.**

And notice what the table quietly settles. Ramesh's sheet has six columns and spans a **five**-dimensional space. Not because someone counted wrong, but because one of his arrows lies along another.

> 🧠 **Think** — How would you tell Ramesh that his six columns span five dimensions, without using the word "dimension"?

---

## 15. The Discovery: A Basis Is Enough and No More

Two questions are now on the table, and they pull in opposite directions.

- **Span** asks: *do I have enough arrows to reach everything?* Too few and you cannot describe the world.
- **Independence** asks: *do I have any spare arrows?* Too many and your description is ambiguous — which is exactly what made your two training runs disagree.

Ask for both at once and you get the answer this chapter has been walking towards:

> **What is the smallest clean set of directions that lets me construct every vector in the space?**

You need enough arrows to span it, and no redundant ones. A set with both properties is called a **basis**.

$$
\boxed{\text{Basis} = \text{independent arrows that span the space}}
$$

Read that definition slowly, because each half is load-bearing:

| The words | What they rule out |
|---|---|
| **"spans the space"** | too few directions — there are places you cannot describe |
| **"linearly independent"** | unnecessary directions — there are descriptions that are not unique |

Which is worth compressing into something you will actually remember:

$$
\boxed{\text{Basis} = \text{Enough} + \text{No redundancy}}
$$

### Why "no redundancy" buys uniqueness

This is the part that repays the effort, because it is the exact reason your two training runs disagreed.

If $\{\mathbf{v}, \mathbf{w}\}$ is a basis and some vector $\mathbf{x}$ can be written two ways,

$$
\mathbf{x} = a_1\mathbf{v} + b_1\mathbf{w} = a_2\mathbf{v} + b_2\mathbf{w}
$$

then subtract one from the other:

$$
(a_1 - a_2)\mathbf{v} + (b_1 - b_2)\mathbf{w} = \mathbf{0}
$$

Independence says the only way to reach zero is with zero coefficients, so $a_1 = a_2$ and $b_1 = b_2$. **The two ways were the same way.**

$$
\boxed{\text{a basis gives every vector exactly one recipe}}
$$

Drop independence and that guarantee goes with it. Ramesh's six columns are not a basis, so a house has *infinitely many* recipes, and gradient descent picks whichever one it happens to walk into. Two runs, two answers, same loss. The mystery from §1 was a missing word all along.

### Dimension, settled

And out of this falls the definition we used informally in §14: **dimension is how many arrows a basis needs.** Every basis of a given space turns out to have the same size — you will prove that in Chapter 012 — which is what makes dimension a property of the space rather than of your choice of arrows.

---

## 16. The Standard Basis, and Why It Is Only a Default

The familiar pair:

$$
\hat{\mathbf{i}} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \qquad \hat{\mathbf{j}} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

They are independent (§12) and they span $\mathbb{R}^2$ (§9), so they are a basis — the **standard basis**.

$$
\left\{ \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \begin{bmatrix} 0 \\ 1 \end{bmatrix} \right\}
$$

Now the insight that matters more than the definition.

**A basis does not have to be horizontal and vertical.** Take:

$$
\mathbf{v} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} -1 \\ 1 \end{bmatrix}
$$

```text
        w \     / v
            \ /
             O
```

Neither is an axis. Is either a multiple of the other? No — so they are independent. Do they span the plane? Take any target $[x, y]$ and solve:

$$
a\begin{bmatrix} 1 \\ 1 \end{bmatrix} + b\begin{bmatrix} -1 \\ 1 \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} \qquad\Longrightarrow\qquad a = \frac{x + y}{2}, \quad b = \frac{y - x}{2}
$$

A recipe exists for every target, so yes. Independent and spanning: **a perfectly valid basis.** The plane does not know which pair of arrows you prefer.

> 💡 **This is the profound bit.** There is nothing special about the axes you were taught. They are a default, not a law — and once you accept that, a sentence becomes available that unlocks most of the rest of this level: **coordinates only mean something relative to the basis somebody chose.**

---

## 17. Coordinates Are Instructions, Not the Vector

Now cash that in, because it changes what Chapter 005's notation was saying all along.

In Chapter 005 we wrote a house as

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 2 \end{bmatrix}
$$

and read it as "three of the first thing, two of the second." Which is closer to the truth than it looked, because what those numbers *are* is

$$
\mathbf{x} = 3\hat{\mathbf{i}} + 2\hat{\mathbf{j}}
$$

The pair $(3, 2)$ is not the vector. It is a pair of **instructions**:

> use 3 copies of basis direction one
>
> **+**
>
> use 2 copies of basis direction two

We had been using $\hat{\mathbf{i}}$ and $\hat{\mathbf{j}}$ without ever mentioning them — the way you use your own language without noticing it is a language.

![The same point read against the ordinary horizontal and vertical arrows gives one pair of numbers, and read against a different pair of arrows chosen to suit the data gives another, although the house has not moved](assets/chapter-006-visual-5.svg)

Choose different arrows and the same house gets different numbers. **The house did not move. The description changed.**

> ⚠️ **A Tempting Wrong Idea** — *"The vector is its coordinates."*
>
> The vector is the arrow. The coordinates are how you described it, given a basis somebody chose. Forget that and change-of-basis, PCA, embeddings and half of interpretability become impossible to think about clearly.

> 🧪 **Studio — "A different language."** The blue and pink arrows are a basis you can drag; the muted arrow is a fixed house. The readout gives that house's coordinates **in your basis**. Turn the two arrows and watch the pair of numbers change while the house stays exactly where it is.

---

## 18. 🔬 The Experiment: Read One House in Two Languages

Do this one by hand. It takes three minutes and it makes §17 permanent.

A house sits at

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 2 \end{bmatrix}
$$

in the ordinary basis. Slot one is rooms, slot two is area in hundreds of square feet.

Now Meera proposes a different pair of directions, because she thinks about houses differently:

$$
\mathbf{s} = \begin{bmatrix} 1 \\ 1 \end{bmatrix} \;\;\text{("bigger overall")}, \qquad \mathbf{d} = \begin{bmatrix} -1 \\ 1 \end{bmatrix} \;\;\text{("more space per room")}
$$

> 🧠 **Predict first.** Before computing: will the house's two new numbers be bigger or smaller than 3 and 2? Will their *sum* mean anything? Write your guess down.

Find $a$ and $b$ with $a\mathbf{s} + b\mathbf{d} = \mathbf{x}$. From §16 the recipe is $a = \frac{x+y}{2}$ and $b = \frac{y-x}{2}$:

$$
a = \frac{3 + 2}{2} = 2.5, \qquad b = \frac{2 - 3}{2} = -0.5
$$

Check it, always:

$$
2.5\begin{bmatrix} 1 \\ 1 \end{bmatrix} - 0.5\begin{bmatrix} -1 \\ 1 \end{bmatrix} = \begin{bmatrix} 2.5 \\ 2.5 \end{bmatrix} + \begin{bmatrix} 0.5 \\ -0.5 \end{bmatrix} = \begin{bmatrix} 3 \\ 2 \end{bmatrix}
$$

So the same house is

$$
\begin{bmatrix} 3 \\ 2 \end{bmatrix}_{\text{rooms, area}} \qquad\text{and}\qquad \begin{bmatrix} 2.5 \\ -0.5 \end{bmatrix}_{\mathbf{s}, \mathbf{d}}
$$

Two descriptions. One house. And the second one says something the first does not: *this home is a fair size, and slightly cramped for its size* — because $b$ came out negative.

Now do three more, and watch a pattern appear:

| House (rooms, area) | In Meera's basis $(a, b)$ | Reads as |
|---|---|---|
| $[3, 2]$ | $[2.5,\; -0.5]$ | average size, a little cramped |
| $[2, 2]$ | $[2,\; 0]$ | average size, exactly typical |
| $[2, 6]$ | $[4,\; 2]$ | large, and unusually spacious |
| $[6, 2]$ | $[4,\; -2]$ | large, and unusually cramped |

Look at the second column. The first number is now "how big", the second is "how cramped" — and the second is **zero** for the house that sits exactly on the average trend. Nothing was learned, no model was trained. The numbers became more interesting because somebody chose better arrows.

> 💡 **That is the entire idea behind learned representations**, met a hundred chapters early and with the arithmetic small enough to do on paper. PCA in Chapter 016 is this experiment with the arrows chosen by the data instead of by Meera.

---

## 19. 🎯 Machine Learning Connection

Four places these ideas show up, in the order you will meet them.

### A feature vector lives in a space with directions

A house recorded three ways:

$$
\mathbf{x} = \begin{bmatrix} 1500 \\ 3 \\ 8 \end{bmatrix} \qquad \begin{aligned} 1500 &\to \text{area} \\ 3 &\to \text{bedrooms} \\ 8 &\to \text{age} \end{aligned}
$$

Loosely, the axes are an *area direction*, a *bedroom direction* and an *age direction*, so the house is one point in a three-dimensional **feature space**:

$$
\text{three features} \;\Longrightarrow\; \mathbf{x} \in \mathbb{R}^3
$$

A richer model uses a hundred features, and then $\mathbf{x} \in \mathbb{R}^{100}$ and you are working in a hundred-dimensional feature space. Nothing in this chapter changes; there are simply more knobs.

### Redundant features are linear dependence

Suppose your dataset carries:

```text
height in centimetres
height in metres
weight
age
```

But

$$
\text{height}_{\text{m}} = \tfrac{1}{100}\,\text{height}_{\text{cm}}
$$

Those two columns carry the same information. One is redundant, and the receipt is easy to write:

$$
\tfrac{1}{100}\,\text{height}_{\text{cm}} - 1 \cdot \text{height}_{\text{m}} = \mathbf{0}
$$

Numerically the data sits in $\mathbb{R}^4$. Effectively it uses three independent directions. Ramesh's square metres, wearing a lab coat.

This one idea reappears under a great many names: **rank** (Chapter 012), **multicollinearity** (Chapter 038), **dimensionality reduction** and **PCA** (Chapter 016), **embeddings** and **representation learning** (Chapter 132).

### A neuron scales and adds — that is all it does

A neuron receives

$$
\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}, \qquad \mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \\ w_3 \end{bmatrix}
$$

and computes

$$
w_1x_1 + w_2x_2 + w_3x_3
$$

Look at the shape of that expression. Scale each thing, add the results.

$$
\boxed{\text{scale things and add them}}
$$

That is the operation this whole chapter has been about. A neuron is a linear combination with a learned set of knobs — which is why the geometry you have just built is not a detour on the way to neural networks. It is the same subject.

### A layer moves the basis

A whole layer computes

$$
\mathbf{y} = W\mathbf{x}
$$

and by Chapter 009 you will read that matrix two ways: as a stack of dot-product questions, and as a record of **where the basis vectors land**. The columns of $W$ are the new homes of $\hat{\mathbf{i}}$ and $\hat{\mathbf{j}}$, and the whole space follows them.

That is why this chapter had to come before matrices. A matrix is about to stop being a rectangular pile of numbers.

---

## 20. How It Breaks

| Failure | What it looks like | Why |
|---|---|---|
| **The same fact twice** | weights differ wildly between runs, loss identical | §10 and §15. Two columns, one direction, so infinitely many dial settings fit equally well and training picks one at random. |
| **Almost the same fact twice** | unstable weights, and no warning at all | §12's prediction. $[1,2]$ and $[2,4.001]$ pass the independence test and still wreck the model — the span is technically the plane and practically a line. Chapter 012 §18 names it. |
| **A column of zeros** | a dial that never moves | §11. The zero vector makes any set dependent, and its weight has nothing to push against. |
| **Too few directions** | error that no training can remove | §7. If what matters lies outside the span of what you recorded, nothing you do reaches it. Chapter 005's blind model, in general form. |
| **More columns than examples** | fits perfectly, predicts nothing | §11's last consequence. With more arrows than dimensions the set is certainly dependent, so there is always a recipe that fits the data exactly and means nothing. |
| **Forgetting which basis you are in** | numbers compared that were never comparable | §17. Coordinates only mean something relative to arrows somebody chose. |

---

## 21. Shapes: What to Check

The habit from Chapter 005, extended to this chapter's objects.

```text
   one feature vector       x : (d,)
   one basis of that space  d arrows, each (d,)
   a set of k arrows        (k, d)  -- k rows, d slots each
   their span               a subspace of R^d, dimension <= k
   coordinates in a basis   c : (d,)   one number per basis arrow
```

Two shape facts worth saying out loud, because they catch real bugs:

1. **A set of $k$ arrows in $\mathbb{R}^d$ spans at most $\min(k, d)$ dimensions.** More arrows than $d$ cannot help; fewer than $d$ cannot reach everything.
2. **Coordinates have one number per basis arrow, not per slot of the original.** They happen to coincide for the standard basis, which is exactly why the distinction is easy to miss.

---

## 22. Distinctions That Matter

| | |
|---|---|
| **Linear combination** — one recipe | **Span** — every recipe's result |
| **Span** — everything reachable | **Basis** — the smallest set that reaches it |
| **Dependent** — one arrow is spare | **Independent** — every arrow earns its place |
| **Number of columns** — what you recorded | **Dimension** — how much you actually have |
| **The vector** — the arrow itself | **Coordinates** — its description in a chosen basis |
| **The standard basis** — a convenient default | **A basis** — any independent spanning set |
| **Spans the space** — enough directions | **Independent** — no unnecessary directions |

---

## 23. What We Discovered

1. Two operations — stretch and add — are enough to build everything in this chapter.
2. A **linear combination** is those two operations run together with a knob for each arrow.
3. Scaling one arrow can lengthen, shorten or reverse it, but never rotate it, so one arrow reaches only a **line**.
4. **Span** is everywhere a set of arrows can reach. Not what you have — what you can get to.
5. Two arrows reach the whole plane **only if they point in genuinely different directions**. Two arrows are not two directions.
6. A set is **dependent** when one arrow already lies in the span of the others, and the zero-combination test detects that without your having to see anything.
7. In three dimensions the same story gives a line, then a plane, then all of space — and a third arrow lying inside the plane adds a knob and no reach.
8. **Dimension** is how many independent directions a space needs, which is the same as how many arrows a basis has.
9. A **basis** is enough plus no redundancy, and independence is exactly what makes a vector's recipe unique.
10. **Coordinates are instructions** relative to a basis somebody chose. Change the arrows and the numbers change while the thing described does not.
11. Counting columns is not counting information — which is where the chapter started, and now it is a theorem rather than a suspicion.

---

## 24. Mathematics We Built

$$
\begin{aligned} \text{linear combination:} \quad & c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k \\[6pt] \operatorname{span}\{\mathbf{v}_1,\ldots,\mathbf{v}_k\} \;&=\; \{\, c_1\mathbf{v}_1 + \cdots + c_k\mathbf{v}_k \;\mid\; c_i \in \mathbb{R} \,\} \end{aligned}
$$

$$
c_1\mathbf{v}_1 + \cdots + c_k\mathbf{v}_k = \mathbf{0} \;\Longrightarrow\; \text{every } c_i = 0 \qquad \text{(independence)}
$$

$$
\boxed{\text{Basis} = \text{independent} + \text{spanning}} \qquad \dim(\text{space}) = \text{size of any basis}
$$

$$
\mathbf{x} = c_1\mathbf{b}_1 + \cdots + c_d\mathbf{b}_d \qquad \text{(the } c_i \text{ are } \mathbf{x}\text{'s coordinates in the basis } \{\mathbf{b}_i\}\text{)}
$$

---

## 25. What Each Symbol Means

| Symbol | In English | In code |
|---|---|---|
| $a\mathbf{v} + b\mathbf{w}$ | a recipe: this much of one arrow, that much of the other | `a*v + b*w` |
| $\operatorname{span}\{\cdot\}$ | everything those arrows can reach | — |
| $c_i$ | how much of arrow $i$ the recipe uses | — |
| $\mathbf{0}$ | the arrow with no length at all | `np.zeros(d)` |
| $\hat{\mathbf{i}}, \hat{\mathbf{j}}$ | the two standard basis arrows of the plane | `np.eye(2)` |
| $\mathbf{b}_i$ | one arrow of whichever basis you chose | — |
| $\mathbb{R}^d$ | all lists of $d$ ordinary numbers | shape `(d,)` |
| $\dim$ | how many independent directions | `np.linalg.matrix_rank(A)` |

---

## 26. One-Minute Explanation

Explain to someone with no mathematics, using no equations:

> Ramesh added a column to the spreadsheet and the model got worse. Why — and what would have made the new column worth having?

If you need the word "independence" to get through it, you have not finished understanding it.

---

## 27. One Picture for the Whole Chapter

```text
                      VECTOR
                        |
                        v
           +--------------------------+
           |  scale it                |
           |  add it to other vectors |
           +------------+-------------+
                        |
                        v
                LINEAR COMBINATION
                        |
                        v
         What can all combinations reach?
                        |
                        v
                      SPAN
                        |
               +--------+--------+
               |                 |
               v                 v
         new direction?      redundant?
               |                 |
              YES               YES
               |                 |
       LINEARLY INDEPENDENT   LINEARLY DEPENDENT
               |
               v
   enough independent arrows to span the space
               |
               v
                     BASIS
```

And the chain to carry forward:

$$
\boxed{\text{Vector} \rightarrow \text{Linear Combination} \rightarrow \text{Span} \rightarrow \text{Independence} \rightarrow \text{Basis}}
$$

Chapter 005 gave you vectors and two operations. This chapter asked what those two operations can construct, and the answer produced four words that the rest of linear algebra is written in.

---

## 28. Exercises

**Level 1 — Observe.** Look at the three panels in §7. Write one sentence for each: what happens to the *reachable set* when you add a third arrow that (a) points the same way as one you already have, (b) points at a right angle to both. Then name which of Ramesh's six columns is case (a), and say what a case-(b) column would have to measure.

**Level 2 — Calculate (by hand).** Take $\mathbf{v} = [2, 1]$, $\mathbf{w} = [-1, 2]$ and $\mathbf{u} = [4, 2]$.

1. Compute $3\mathbf{v} - 2\mathbf{w}$.
2. Find $a$ and $b$ with $a\mathbf{v} + b\mathbf{w} = [7, 4]$, and check your answer.
3. Show $\{\mathbf{v}, \mathbf{u}\}$ is dependent by finding coefficients, not both zero, that combine them to $\mathbf{0}$.
4. Write $\operatorname{span}(\mathbf{v}, \mathbf{u})$ in words, then state its dimension.
5. Find the coordinates of $[3, 2]$ in the basis $\{[1,1], [-1,1]\}$ and verify them.

**Level 3 — Derive.** Prove each of these from the definitions, not from a picture.

1. If $\mathbf{w} = c\,\mathbf{v}$ for some number $c$, then $\{\mathbf{v}, \mathbf{w}\}$ is dependent.
2. Conversely, if two non-zero vectors are dependent, one is a multiple of the other.
3. Any set containing $\mathbf{0}$ is dependent.
4. If $\{\mathbf{v}, \mathbf{w}\}$ is a basis of $\mathbb{R}^2$, every vector has exactly one recipe in it. (This is §15's argument — reproduce it without looking.)

**Level 4 — Predict, then break it.** Open the studio scene **"Line, plane, space"** and predict the readout's direction count at each of the four beats before you slide to it. Then, in **"Everywhere you can reach"**, place the two arrows so the span is a line, and describe in one sentence what you had to do to the numbers. Finally: in $\mathbb{R}^3$, can three arrows span only a line? Construct an example or explain why not.

**Level 5 — Build it.** Write two functions from scratch, no linear-algebra library beyond array arithmetic. `is_dependent(vectors, tol)` returns whether a set is dependent; `coordinates(x, basis)` returns a vector's coordinates in a given basis. Test the first on `[[1,2],[2,4]]`, on `[[1,2],[2,4.001]]`, and on a set containing a zero vector. Then explain what value of `tol` you chose and what it is really deciding.

**Level 6 — Investigate.** Build the four-house table with all six of Ramesh's columns. For every pair of columns, compute the cosine similarity from Chapter 007's preview, and find the pair that scores $1.000$. Then round the square-metre column to one decimal place and score it again. Form a hypothesis about what threshold would catch a *nearly* redundant column without flagging honest ones, test it on the sheet, and say what it would cost you if the threshold were wrong in each direction.

---

## 29. Common Mistakes

| Mistake | Why it is wrong |
|---|---|
| "More columns means more information." | §1. Six columns, five facts. A column earns its place by pointing somewhere new. |
| "These two columns look nothing alike, so they are independent." | `800` and `74.3` share no digits and are the same measurement. Independence is about direction, not appearance. |
| "Two vectors always span a plane." | Only if they point differently. Two parallel arrows span a line, and the algebra of §10 shows why in one step. |
| "Three vectors always span space." | Only if none of them lies in the plane of the other two. §13, ending one. |
| "Independence is something you can see." | Past three dimensions nobody can see anything. That is why §11 gives you a test rather than a picture. |
| "The span is the arrows." | The span is everywhere the arrows can *reach*. The arrows are a handful of vectors; the span is usually infinite. |
| "The vector *is* its coordinates." | Coordinates describe the arrow relative to arrows somebody chose. §17. |
| "A basis must be perpendicular." | Perpendicular is convenient, not required. $\{[1,1], [-1,1]\}$ happens to be; $\{[1,0], [1,1]\}$ is a basis and is not. |
| "Passing the independence test means the features are fine." | $[1,2]$ and $[2,4.001]$ pass, and will still ruin a model. §20, row two. |

---

## 30. Socratic Questions

1. Five arrows are handed to you in $\mathbb{R}^3$. Without computing anything, what can you already say about whether they are independent — and which section proves it?
2. Ramesh's sheet spans five dimensions using six columns. Is there a single right set of five columns to keep, or many? What would decide between them?
3. A basis must be independent *and* spanning. If a dataset forced you to give up one of the two, which would you sacrifice, and what exactly would go wrong?
4. We keep saying scaling cannot rotate an arrow. Which operation *can*? What would it have to look like as a machine — and which chapter do you think introduces it?
5. Two word-embedding models each describe "river" as a vector in $\mathbb{R}^{768}$. Given §17, what would it even mean to say the two vectors are similar?
6. §18 found a basis in which the second coordinate meant "cramped for its size". Who chose those arrows, and what would it take for the *data* to choose them instead?
7. If a set of arrows is dependent, there are infinitely many recipes for a given target. Is that ever useful, rather than merely ambiguous?

---

## 31. 🔭 Bridge to Chapter 007

We now have directions, and we have leaned on one tool all chapter without ever earning it.

Twice in Chapter 005, and twice more here, the question was really *how much of this arrow lies along that one?* — whether a column is a copy of another, whether a third vector lies in a plane, how cramped a house is for its size. Each time we answered it with algebra: solve for the coefficients and look.

There is a single number that answers it directly, and you have already met it. Chapter 005 built the dot product as a weighted vote, and claimed, with a promise to come back:

$$
\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\|\,\|\mathbf{v}\|\cos\theta
$$

So:

> **Why should pairing numbers, multiplying them and adding them up have anything whatsoever to do with an angle?**

Chapter 007 proves it from the law of cosines, reads the *sign* of a dot product geometrically, and turns the whole thing into **projection** — the exact tool for "how much of this arrow lies along that one". It also gives you length and distance properly, and the unit vectors this chapter deliberately did without.

Then Chapters 008 to 011 give matrices their algebra, and Chapter 012 puts it all together and tells you what a neuron is.

---

## What You Will Need, and When

This chapter used arrows, arithmetic and nothing taken on trust. Here is where each thread goes.

### Linear algebra — you are standing in it

There is no "later" for this one. Linear combinations, span, independence, basis and dimension **are** linear algebra, and you have just built them out of a spreadsheet argument rather than being handed them as definitions.

🎥 **See it drawn** — [*Linear combinations, span, and basis vectors*](https://youtu.be/k7RM-ot2NWY), 3Blue1Brown, Chapter 2 of *Essence of Linear Algebra* (10 min), with the companion page at [3blue1brown.com/lessons/span](https://www.3blue1brown.com/lessons/span). Watch it **now**, not before — you have hit exactly the wall it was made for. The rest of the playlist, [*Essence of Linear Algebra*](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab), tracks the rest of this level closely.

### Statistics — when you ask which directions the data itself prefers

§18 chose a better basis by hand, and left the obvious question hanging: who picks the arrows? You could guess, as Meera did. Or you could ask the data.

Doing that properly needs variance and covariance — the machinery for saying *"the houses vary mostly along this direction"* in numbers rather than by eye.

→ **Level 03**, from Chapter 026, and it comes back as PCA in Chapter 016.

### Numerical computing — when "nearly dependent" starts to matter

§20's second row is the row that bites in practice, and the tool for it is the **condition number**, which Chapter 005 §10 already met under a different disguise. Rank and near-rank-deficiency get their proper treatment in Chapter 012, and the SVD in Chapter 015 turns "how nearly dependent is this set?" into a number you can read off.

---

## References

**Linear combinations, span and basis**

3Blue1Brown, *Linear combinations, span, and basis vectors* — Chapter 2 of *Essence of Linear Algebra*.
<https://www.3blue1brown.com/lessons/span> · <https://youtu.be/k7RM-ot2NWY>

*The "two knobs" framing, the collapse to a line when one arrow is a copy, and the sliding-plane picture of a third dimension are Grant Sanderson's, and they are the clearest way into this material that exists.*

**Span asks whether you have enough; independence asks whether you have spare**

Gilbert Strang, *Introduction to Linear Algebra*, chapters on vector spaces and independence, for the same ideas stated as theorems once the pictures have done their work.
