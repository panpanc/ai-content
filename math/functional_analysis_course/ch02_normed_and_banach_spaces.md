# Chapter 2: Normed Spaces and Banach Spaces

**Learning objectives:**
- Understand what a norm is and why we need one on function spaces
- See how different norms capture different notions of "closeness" for functions
- Understand completeness and why it matters — Cauchy sequences that don't converge
- Know the key examples: $\ell^p$, $L^p$, $C[a,b]$

**Prerequisites:** Chapter 1

---

## Motivation

In Chapter 1, we saw that problems in differential equations, Fourier analysis, and optimization force us into infinite-dimensional vector spaces of functions. But a bare vector space — just addition and scalar multiplication — doesn't give us enough to work with. We can't talk about convergence, approximation, or continuity without some way of measuring *distance*.

This chapter builds the first essential layer of structure: **norms**. A norm tells us "how big" a vector is, which in turn tells us how far apart two vectors are. Different norms on the same space capture different notions of closeness — and in infinite dimensions, these can lead to genuinely different theories.

But having a norm isn't enough. We also need the space to be **complete** — meaning Cauchy sequences actually converge to something *in the space*. Without completeness, the deep theorems of functional analysis (Chapters 5–7) simply don't hold. Complete normed spaces are called **Banach spaces**, and they're the backbone of the entire subject.

---

## Norms: Measuring Size

### Why we need to measure size

Think of a norm as a ruler for a vector space. In $\mathbb{R}^n$, the Euclidean norm $\|x\| = \sqrt{x_1^2 + \cdots + x_n^2}$ is so natural that you might not think of it as a choice. But it *is* a choice — and in infinite-dimensional spaces, different choices lead to different mathematics.

A **norm** on a vector space $X$ is a function $\|\cdot\|: X \to [0, \infty)$ satisfying three axioms:

1. **Positive definiteness:** $\|x\| = 0$ if and only if $x = 0$
2. **Homogeneity:** $\|\alpha x\| = |\alpha| \|x\|$ for all scalars $\alpha$
3. **Triangle inequality:** $\|x + y\| \leq \|x\| + \|y\|$

Each axiom earns its place. Positive definiteness means different vectors are distinguishable. Homogeneity means scaling a vector scales its size proportionally. The triangle inequality means distances behave sensibly — the direct route is never longer than a detour.

A vector space equipped with a norm is called a **normed space**, and the norm induces a metric: $d(x, y) = \|x - y\|$.

### Multiple norms on the same space

Here's where infinite dimensions get interesting. Consider $C[0,1]$, the continuous functions on $[0,1]$. Three natural norms:

$$\|f\|_\infty = \sup_{x \in [0,1]} |f(x)| \quad \text{(sup norm — worst-case size)}$$

$$\|f\|_1 = \int_0^1 |f(x)| \, dx \quad \text{($L^1$ norm — average size)}$$

$$\|f\|_2 = \left(\int_0^1 |f(x)|^2 \, dx\right)^{1/2} \quad \text{($L^2$ norm — root-mean-square size)}$$

> **Example:** Let $f_n(x) = x^n$ on $[0,1]$. Then:
> - $\|f_n\|_\infty = 1$ for all $n$ (the sup is always 1, attained at $x = 1$)
> - $\|f_n\|_1 = \frac{1}{n+1} \to 0$
> - $\|f_n\|_2 = \frac{1}{\sqrt{2n+1}} \to 0$
>
> So $f_n \to 0$ in $L^1$ and $L^2$, but **not** in the sup norm. The three norms genuinely disagree about convergence.

This isn't a quirk — it reflects real mathematical content. The sup norm says "$f_n$ is close to $g$ if $f_n(x) \approx g(x)$ for *every* $x$." The $L^1$ norm says "$f_n$ is close to $g$ if $f_n(x) \approx g(x)$ *on average*." These are different requirements, and neither is "right" in absolute terms — which one you need depends on your problem.

### What goes wrong: the wrong norm for the job

Choosing the wrong norm can make well-posed problems ill-posed. Consider approximating the step function

$$g(x) = \begin{cases} 0 & x < 1/2 \\ 1 & x \geq 1/2 \end{cases}$$

by continuous functions. In the $L^1$ norm, continuous functions *can* approximate $g$ arbitrarily well (just make the transition steeper and steeper). In the sup norm, they *cannot* — $g$ is discontinuous, so $\|f - g\|_\infty \geq 1/2$ for any continuous $f$ near the jump. The norm determines what you can approximate.

---

## The Zoo of Normed Spaces

### Sequence spaces: $\ell^p$

For $1 \leq p < \infty$, the **$\ell^p$ space** consists of all sequences $x = (x_1, x_2, \ldots)$ with finite $p$-norm:

$$\|x\|_p = \left(\sum_{n=1}^\infty |x_n|^p\right)^{1/p} < \infty.$$

For $p = \infty$: $\|x\|_\infty = \sup_n |x_n|$.

These spaces are the infinite-dimensional analogues of $\mathbb{R}^n$ with different norms. Some key members:

- **$\ell^1$:** absolutely summable sequences. The "thriftiest" space — vectors must decay fast.
- **$\ell^2$:** square-summable sequences. The "Goldilocks" space — an inner product makes it a Hilbert space (Chapter 3).
- **$\ell^\infty$:** bounded sequences. The "most generous" space — any bounded sequence belongs.

> **Note:** There's a strict nesting: $\ell^1 \subset \ell^2 \subset \ell^p \subset \ell^\infty$ for $1 \leq p \leq \infty$. The smaller $p$ is, the more restrictive the membership requirement. Think of it as: smaller $p$ demands faster decay.

### Function spaces: $L^p$ and $C[a,b]$

The **$L^p$ spaces** on a measure space $(X, \mu)$ consist of (equivalence classes of) measurable functions with finite $p$-norm:

$$\|f\|_p = \left(\int_X |f|^p \, d\mu\right)^{1/p}.$$

These are the continuous analogues of $\ell^p$. The space $C[a,b]$ with the sup norm is another workhorse.

| Space | Norm | Complete? | Inner product? | Dual space |
|-------|------|-----------|----------------|------------|
| $\ell^p$ ($1 \leq p \leq \infty$) | $\|x\|_p$ | Yes | Only $\ell^2$ | $\ell^q$ ($1/p + 1/q = 1$) |
| $L^p[a,b]$ ($1 \leq p \leq \infty$) | $\|f\|_p$ | Yes | Only $L^2$ | $L^q$ ($1 \leq p < \infty$) |
| $C[a,b]$ | $\|f\|_\infty$ | Yes | No | Signed Borel measures |
| $C[a,b]$ | $\|f\|_1$ | **No** | No | — |

The last row is crucial — and it's the subject of the next section.

---

## Completeness and Banach Spaces

### Why completeness matters

**Completeness** means every Cauchy sequence converges. In a complete space, you can form infinite series, take limits of approximating sequences, and use fixed-point arguments — the core moves of analysis. Without it, you're working on quicksand.

Think of completeness as a safety net. You set up a sequence of approximations, prove they form a Cauchy sequence, and completeness guarantees a limit exists in your space. Without that guarantee, your approximations might converge to a "phantom" — something outside the space.

### The disaster of incompleteness

Here's a concrete example of what goes wrong. Consider $C[0,1]$ with the $L^1$ norm $\|f\|_1 = \int_0^1 |f(x)| dx$.

Define a sequence of continuous functions:

```
f_n(x):

    1 |         ___________
      |        /
      |       /  <- transition over [1/2 - 1/n, 1/2 + 1/n]
      |      /
    0 |_____/
      +-----|---|----|------
      0    1/2-1/n  1/2+1/n  1
```

Each $f_n$ is continuous, ramps linearly from 0 to 1 near $x = 1/2$, and satisfies:

$$\|f_n - f_m\|_1 = \int_0^1 |f_n(x) - f_m(x)| \, dx \leq \frac{2}{\min(n,m)} \to 0$$

So $(f_n)$ is Cauchy in $(C[0,1], \|\cdot\|_1)$. But the "limit" is the step function

$$g(x) = \begin{cases} 0 & x < 1/2 \\ 1 & x \geq 1/2, \end{cases}$$

which is **not continuous**. The Cauchy sequence has no limit in $C[0,1]$. The space is incomplete under this norm.

> **Note:** The *same* set $C[0,1]$ with the *sup norm* is complete. Completeness is a property of the space **together with its norm**, not of the set of functions alone. This is why the choice of norm matters so much.

The fix is to **complete** $C[0,1]$ under $\|\cdot\|_1$ — the resulting space is $L^1[0,1]$, which includes all (Lebesgue) integrable functions, not just continuous ones.

### Banach spaces: the definition

A **Banach space** is a normed vector space that is complete — every Cauchy sequence converges in the space.

All the heavy machinery of functional analysis — the Hahn-Banach theorem (Chapter 5), the uniform boundedness principle (Chapter 6), the open mapping and closed graph theorems (Chapter 7) — requires Banach spaces. Completeness is the hypothesis that makes everything work.

### Verification: why $\ell^p$ is complete

Let's sketch why $\ell^p$ (for $1 \leq p < \infty$) is a Banach space. Take a Cauchy sequence $(x^{(k)})$ in $\ell^p$, where $x^{(k)} = (x_1^{(k)}, x_2^{(k)}, \ldots)$.

**Step 1:** For each fixed $n$, the sequence $(x_n^{(k)})_{k=1}^\infty$ is Cauchy in $\mathbb{R}$ (since $|x_n^{(k)} - x_n^{(j)}| \leq \|x^{(k)} - x^{(j)}\|_p$). By completeness of $\mathbb{R}$, it converges to some $x_n$.

**Step 2:** Define $x = (x_1, x_2, \ldots)$. We need to show $x \in \ell^p$ and $\|x^{(k)} - x\|_p \to 0$.

**Step 3:** For any finite $N$, we have $\sum_{n=1}^N |x_n^{(k)} - x_n|^p \leq \|x^{(k)} - x^{(j)}\|_p^p$ for large enough $j$ (by letting $j \to \infty$, using continuity of $|\cdot|^p$). Taking $N \to \infty$ gives $\|x^{(k)} - x\|_p \to 0$.

The key move: completeness of $\mathbb{R}$ gives us coordinate-by-coordinate limits, and the Cauchy condition in $\ell^p$ ensures these assemble into a legitimate $\ell^p$ element. This pattern — "limits exist coordinatewise, norm convergence follows from the Cauchy property" — recurs throughout functional analysis.

> **Convince yourself:** Where did we use the assumption $p < \infty$? (Hint: the argument for $\ell^\infty$ is similar but uses sup instead of sum.)

---

## Equivalent Norms

### When do different norms agree?

Two norms $\|\cdot\|_a$ and $\|\cdot\|_b$ on a vector space $X$ are **equivalent** if there exist constants $c, C > 0$ such that

$$c\|x\|_a \leq \|x\|_b \leq C\|x\|_a \quad \text{for all } x \in X.$$

Equivalent norms produce the same open sets, the same convergent sequences, and the same continuous linear maps. For most purposes, equivalent norms are interchangeable.

### The finite-dimensional miracle

**Theorem:** On a finite-dimensional vector space, *all* norms are equivalent.

This is why finite-dimensional linear algebra doesn't worry about which norm to use. Whether you use $\|\cdot\|_1$, $\|\cdot\|_2$, or $\|\cdot\|_\infty$ on $\mathbb{R}^n$, you get the same topology. Convergence, continuity, compactness — all norm-independent.

The intuition: in $\mathbb{R}^n$, the unit sphere (in any norm) is compact. A continuous positive function on a compact set has a positive minimum and a finite maximum. The ratio between any two norms is such a function.

### The infinite-dimensional failure

In infinite dimensions, this miracle fails completely. The norms $\|\cdot\|_1$ and $\|\cdot\|_\infty$ on $C[0,1]$ are *not* equivalent.

> **Example:** Consider the "spike" functions $f_n(x) = \max(0, n - n^2|x - 1/2|)$. Each $f_n$ is a triangle of height $n$ and base width $2/n$, centered at $x = 1/2$.
>
> ```
> f_3:    height 3
>          /\
>         /  \
>        /    \
> ______/      \______
> 0          1/2          1
>        |--2/3--|
> ```
>
> - $\|f_n\|_\infty = n \to \infty$
> - $\|f_n\|_1 = \frac{1}{2} \cdot \frac{2}{n} \cdot n = 1$ for all $n$
>
> So $\|f_n\|_\infty / \|f_n\|_1 = n \to \infty$. No constant $C$ can satisfy $\|f\|_\infty \leq C\|f\|_1$ for all continuous $f$.

This is a fundamental feature of infinite dimensions: you can concentrate a function's values in a smaller and smaller region, keeping its $L^1$ norm fixed while blowing up its sup norm. In finite dimensions, there's nowhere to hide — the compactness of the unit sphere prevents this kind of escape.

> **Note:** The question of when two norms on a Banach space must be equivalent connects to the **open mapping theorem** (Chapter 7). If a Banach space is complete under two norms and the identity map between them is bounded, the inverse mapping theorem tells us the norms are equivalent. This is one of the most elegant applications of the "big three" theorems.

---

## Key Takeaways

- **Norms measure size and distance** in a vector space. Different norms capture different notions of closeness, and in infinite dimensions, these genuinely disagree.
- **Completeness is non-negotiable.** Incomplete spaces have "holes" — Cauchy sequences that don't converge — and the major theorems of functional analysis fail without it. Complete normed spaces are **Banach spaces**.
- **The key examples** are $\ell^p$, $L^p$, and $C[a,b]$ with the sup norm. These appear throughout the subject as test cases, counterexamples, and workhorses.
- **Norm equivalence fails in infinite dimensions.** Unlike finite dimensions, different norms on an infinite-dimensional space can produce fundamentally different topologies. The choice of norm is a real mathematical decision.
- **Completeness depends on the norm.** The same set of functions ($C[0,1]$) can be complete under one norm (sup) and incomplete under another ($L^1$). The space and the norm are inseparable.

---

## What's Next

We now have normed spaces and the crucial concept of completeness. But norms only give us "size" — they tell us how far apart two functions are, but they can't tell us about **angles** or **projections**. In **Chapter 3: Inner Products and Hilbert Spaces**, we add this geometric structure. Inner products let us talk about orthogonality and perpendicular projection, which is the mathematical foundation of Fourier analysis, least-squares approximation, and quantum mechanics. Hilbert spaces — complete inner product spaces — are the most geometrically rich infinite-dimensional spaces, and the place where functional analysis feels most like the linear algebra you already know.
