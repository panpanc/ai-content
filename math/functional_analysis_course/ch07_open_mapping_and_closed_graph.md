# Chapter 7: The Open Mapping and Closed Graph Theorems

**Learning objectives:**
- State and understand the open mapping theorem
- Derive the inverse mapping theorem as a corollary
- Understand the closed graph theorem as a shortcut for proving boundedness
- Apply these theorems to concrete problems

**Prerequisites:** Chapters 2, 4, 6

---

## Motivation

In Chapter 6, we saw the first of the "big three" structure theorems: the uniform boundedness principle. It used the Baire category theorem to upgrade pointwise bounds to uniform bounds. Now we meet the other two: the **open mapping theorem** and the **closed graph theorem**. Both are also powered by Baire category, but they extract fundamentally different structural conclusions.

The open mapping theorem says: if a bounded linear operator between Banach spaces is surjective, it maps open sets to open sets. This sounds technical, but its corollary — the **inverse mapping theorem** — is one of the most useful results in analysis: if a bounded linear operator is bijective, its inverse is *automatically* bounded. You never have to check continuity of the inverse separately. That's another "free lunch" from completeness.

The closed graph theorem provides a different kind of gift: to prove an operator is bounded, you only need to verify that its graph is closed — often a much weaker condition.

Together with the Hahn-Banach theorem (Chapter 5) and the uniform boundedness principle (Chapter 6), these form the "big four" theorems of functional analysis — the foundational pillars that support the entire theory.

---

## The Open Mapping Theorem

### What "open mapping" means

A function $f: X \to Y$ between topological spaces is called **open** if it maps open sets to open sets: whenever $U \subset X$ is open, $f(U) \subset Y$ is open.

Not every continuous function is open. Consider $f: \mathbb{R} \to \mathbb{R}$ given by $f(x) = x^2$. The open interval $(-1, 1)$ maps to $[0, 1)$, which is not open. Continuous maps preserve preimages of open sets; open maps preserve images of open sets. These are different conditions.

For linear maps, being open has a concrete meaning: if $T$ maps open balls to sets containing open balls, then the equation $Tx = y$ is "uniformly solvable" — for any $y$ in a neighborhood of $Ty_0$, there's a solution $x$ close to $y_0$. Open-ness is a quantitative surjectivity condition.

### Statement

**Theorem (Open Mapping Theorem / Banach-Schauder).** Let $X$ and $Y$ be Banach spaces, and let $T \in B(X, Y)$ be surjective. Then $T$ is an open map.

Equivalently: there exists $\delta > 0$ such that $B_Y(0, \delta) \subset T(B_X(0, 1))$. The image of the unit ball contains a ball around the origin.

### Proof sketch

The proof has two stages, both using Baire category.

**Stage 1: The image of the unit ball is "almost" a neighborhood of zero.**

Since $T$ is surjective, $Y = \bigcup_{n=1}^\infty T(\overline{B}_X(0, n)) = \bigcup_{n=1}^\infty n \cdot T(\overline{B}_X(0, 1))$. By Baire (applied to $Y$, which is Banach), some $\overline{T(\overline{B}_X(0, n))}$ has nonempty interior. By scaling, $\overline{T(\overline{B}_X(0, 1))}$ contains a ball $B_Y(y_0, 2\epsilon)$. Translating (using the fact that $T$ is linear), $\overline{T(\overline{B}_X(0, 2))}$ contains $B_Y(0, 2\epsilon)$, so

$$B_Y(0, \epsilon) \subset \overline{T(\overline{B}_X(0, 1))}.$$

**Stage 2: The closure is unnecessary — the image itself contains a ball.**

This is a "gluing" argument. Given $y \in B_Y(0, \epsilon)$, Stage 1 gives $x_1$ with $\|x_1\| < 1$ and $\|y - Tx_1\| < \epsilon/2$. Apply Stage 1 again to find $x_2$ with $\|x_2\| < 1/2$ and $\|y - Tx_1 - Tx_2\| < \epsilon/4$. Iterate: $x = \sum x_n$ converges (the series is absolutely convergent, and $X$ is complete), and $Tx = y$. Moreover, $\|x\| < 1 + 1/2 + 1/4 + \cdots = 2$.

So $B_Y(0, \epsilon) \subset T(B_X(0, 2))$, which means $B_Y(0, \epsilon/2) \subset T(B_X(0, 1))$. $\square$

> **Note:** Both stages are essential. Stage 1 uses Baire to show the image is "almost" a neighborhood (up to closure). Stage 2 uses completeness of $X$ to remove the closure. The theorem requires *both* spaces to be Banach.

### What goes wrong without the hypotheses

**Without surjectivity:** Consider the inclusion $T: \ell^1 \hookrightarrow \ell^2$ (every $\ell^1$ sequence is in $\ell^2$). This is bounded and injective, but the image $T(\ell^1)$ is a proper dense subspace of $\ell^2$, and $T$ is not open — the image of the open unit ball of $\ell^1$ doesn't contain any open ball of $\ell^2$.

**Without completeness of $Y$:** Let $X = Y_0$ be a Banach space equipped with two norms $\|\cdot\|_1$ and $\|\cdot\|_2$, where $\|\cdot\|_1$ is strictly stronger (finer topology) but $Y_0$ is only complete under $\|\cdot\|_1$. The identity map $(Y_0, \|\cdot\|_1) \to (Y_0, \|\cdot\|_2)$ is bounded and bijective, but if $Y_0$ isn't complete under $\|\cdot\|_2$, the open mapping theorem doesn't apply, and the inverse might be unbounded.

---

## The Inverse Mapping Theorem

### The corollary that changes everything

**Theorem (Inverse Mapping Theorem).** Let $X$ and $Y$ be Banach spaces and $T \in B(X, Y)$ a bijection. Then $T^{-1} \in B(Y, X)$ — the inverse is automatically bounded.

*Proof.* By the open mapping theorem, $T$ is open. So $T^{-1}$ maps open sets (which are images of open sets under $T$) back to open sets — i.e., $T^{-1}$ is continuous. A continuous linear map is bounded. $\square$

This is remarkable. You define a bijective bounded operator, and you get continuity of the inverse *for free*. In finite dimensions, this is obvious (every bijective linear map is bicontinuous). In infinite dimensions, it's not obvious at all — and it requires both spaces to be complete.

### Application: equivalence of norms

**Corollary.** If a vector space $X$ is a Banach space under two norms $\|\cdot\|_a$ and $\|\cdot\|_b$, and $\|\cdot\|_a$ is stronger ($\|x\|_b \leq C\|x\|_a$ for some constant $C$), then the norms are equivalent.

*Proof.* The identity map $I: (X, \|\cdot\|_a) \to (X, \|\cdot\|_b)$ is bounded (with $\|I\| \leq C$) and bijective. Both spaces are Banach. By the inverse mapping theorem, $I^{-1}$ is bounded — meaning $\|x\|_a \leq C'\|x\|_b$ for some $C'$. The norms are equivalent.

> **Example:** Suppose you have a space of functions that is complete under two norms, and one norm dominates the other. You automatically get equivalence. This is useful when one norm is easy to compute (like the sup norm) and the other is natural for the problem (like a Sobolev norm). If both make the space complete and one dominates, they're secretly the same topology.

This connects back to Chapter 2, where we noted that norm equivalence fails in infinite dimensions in general. The inverse mapping theorem tells us *when* it must hold: when both norms make the space complete.

---

## The Closed Graph Theorem

### Motivation: a shortcut for proving boundedness

Proving that a linear operator is bounded means showing $\|Tx\| \leq C\|x\|$ for all $x$ — you need a quantitative estimate. Sometimes this estimate is hard to prove directly. The closed graph theorem offers an alternative: instead of bounding $\|Tx\|$, just show that the *graph* of $T$ is closed.

### The graph of an operator

The **graph** of a linear operator $T: X \to Y$ is the set

$$\text{Graph}(T) = \{(x, Tx) : x \in X\} \subset X \times Y.$$

We equip $X \times Y$ with the product norm $\|(x, y)\| = \|x\| + \|y\|$. The graph is always a subspace of $X \times Y$. It's closed if: whenever $(x_n, Tx_n) \to (x, y)$ in $X \times Y$, we have $y = Tx$.

In other words, the graph is closed if: $x_n \to x$ and $Tx_n \to y$ together imply $y = Tx$.

### Statement

**Theorem (Closed Graph Theorem).** Let $X$ and $Y$ be Banach spaces and $T: X \to Y$ a linear operator. If $\text{Graph}(T)$ is closed in $X \times Y$, then $T$ is bounded.

### Proof via the open mapping theorem

The graph $G = \text{Graph}(T)$ is a closed subspace of $X \times Y$. Since $X$ and $Y$ are Banach and the graph is closed, $G$ is itself a Banach space.

Consider the projection $\pi_1: G \to X$ defined by $\pi_1(x, Tx) = x$. This is bounded ($\|\pi_1(x, Tx)\| = \|x\| \leq \|x\| + \|Tx\| = \|(x, Tx)\|$), linear, and bijective (since each $x$ determines $(x, Tx)$ uniquely).

By the inverse mapping theorem, $\pi_1^{-1}: X \to G$ is bounded. That is, there exists $C$ such that $\|(x, Tx)\| \leq C\|x\|$ for all $x$. Since $\|(x, Tx)\| = \|x\| + \|Tx\| \geq \|Tx\|$, we get $\|Tx\| \leq C\|x\|$. So $T$ is bounded. $\square$

### When to use it

The closed graph theorem is most useful when:
- You know $T$ is linear and you know what $Tx$ "should be" (from some construction), but a direct norm estimate is hard.
- You can verify the "sequential" condition: if $x_n \to x$ and $Tx_n \to y$, then $y = Tx$.

> **Example:** Consider the operator $T: X \to X$ where $X$ is a Banach space and $T$ is defined by some limit process: $Tx = \lim_{n \to \infty} S_n x$ where $(S_n)$ are bounded operators. Suppose you know the limit exists for all $x$. Then $T$ is linear (limits preserve linearity). Is $T$ bounded?
>
> **Using the closed graph theorem:** Suppose $x_k \to x$ and $Tx_k \to y$. We need $y = Tx$. For each $n$, $S_n x_k \to S_n x$ (since $S_n$ is continuous). Taking $k \to \infty$: $Tx_k \to y$, and also $S_n x_k \to S_n x$ for each $n$. Taking $n \to \infty$: $S_n x \to Tx$. So $y = Tx$. The graph is closed, so $T$ is bounded.
>
> Without the closed graph theorem, you'd need to find a uniform bound on the limit operator — much harder.

### What goes wrong: unbounded operators have non-closed graphs

Conversely, if $T$ is unbounded, its graph is not closed. This gives a concrete way to see unboundedness.

> **What goes wrong:** The differentiation operator $T: C^1[0,1] \to C[0,1]$ with $Tf = f'$ (both spaces with the sup norm). This is unbounded (Chapter 4). Its graph is not closed *in $C[0,1] \times C[0,1]$*: take $f_n(x) = x^n/n$. Then $f_n \to 0$ in sup norm, and $f_n'(x) = x^{n-1} \to g$ where $g(x) = 0$ for $x < 1$ and $g(1) = 1$ — but $g$ is discontinuous, and the limit pair $(0, g)$ doesn't have $g = 0' = 0$. (More precisely, the issue is that $C^1[0,1]$ with the sup norm is not complete, so the closed graph theorem's hypothesis on the domain being Banach isn't satisfied.)

---

## The Big Four Together

### How they relate

We now have all four major theorems. Let's see the architecture:

| Theorem | Input | Output | Engine | Chapter |
|---------|-------|--------|--------|---------|
| **Hahn-Banach** | Functional on subspace | Extension to whole space | Zorn's lemma (axiom of choice) | 5 |
| **Uniform Boundedness** | Pointwise bounded family | Uniformly bounded | Baire category | 6 |
| **Open Mapping** | Surjective bounded operator | Open map (bounded inverse) | Baire category | 7 |
| **Closed Graph** | Closed graph | Bounded operator | Open mapping (Baire) | 7 |

The philosophical split:
- **Hahn-Banach** is about *existence*. It uses the axiom of choice, doesn't require completeness of the whole space, and works in a very general setting.
- **The Big Three** (UBP, Open Mapping, Closed Graph) are about *structure*. They all require completeness and are powered by the Baire category theorem. They tell you that Banach spaces have rigidity — certain "nice" behaviors are forced by the algebraic-topological structure.

### Application: solving equations $Tx = y$

All four theorems contribute to the fundamental problem: given $T \in B(X, Y)$, solve $Tx = y$.

```
Solving Tx = y:

1. Does a solution exist?
   → Surjectivity of T
   → Hahn-Banach helps: T is surjective iff
     "φ(Tx) = 0 for all x" implies φ = 0  (Chapter 5)

2. Is the solution unique?
   → Injectivity of T
   → ker(T) = {0}

3. Does the solution depend continuously on y?
   → Boundedness of T⁻¹
   → Open Mapping Theorem: automatic if T is bijective
     and both spaces are Banach!  (Chapter 7)

4. Is the problem well-posed under perturbations?
   → Neumann series: if ||T - T₀|| is small and
     T₀ is invertible, T is invertible  (Chapter 4)
```

> **Example:** Consider the integral equation $f(x) - \lambda \int_0^1 K(x,t)f(t) \, dt = g(x)$, which we can write as $(I - \lambda T_K)f = g$ where $T_K$ is the integral operator with kernel $K$. If $T_K$ is compact (Chapter 8) and $\lambda$ is not an eigenvalue of $T_K$, the Fredholm alternative guarantees $(I - \lambda T_K)$ is bijective. The open mapping theorem then guarantees the solution depends continuously on $g$.

---

## Key Takeaways

- **The open mapping theorem** says surjective bounded operators between Banach spaces are open maps. This requires completeness of *both* spaces.
- **The inverse mapping theorem** is the killer corollary: bijective bounded operators between Banach spaces have bounded inverses — automatically. This is the "free lunch" of functional analysis.
- **The closed graph theorem** gives a shortcut for proving boundedness: verify the graph is closed instead of finding a norm estimate. This is a reduction from a quantitative condition to a qualitative one.
- **The big four theorems** form two families: Hahn-Banach (existence, via axiom of choice) and the big three (structure, via Baire category). Together they're the foundation of the entire subject.
- **Completeness is the recurring theme.** Every one of the big three requires Banach spaces. Without completeness, none of the structural conclusions hold.

---

## What's Next

We've now built the abstract theory: spaces (Chapters 1–3), operators (Chapter 4), and the four fundamental theorems (Chapters 5–7). In **Chapter 8: Compact Operators**, we turn to a special class of operators that bridges the gap between finite and infinite dimensions. Compact operators map bounded sets to sets with compact closure — they're "almost finite-dimensional." Their spectral theory looks remarkably like finite-dimensional linear algebra, and they're the natural setting for solving integral equations and the Fredholm alternative. This sets the stage for the general spectral theory in Chapter 9.
