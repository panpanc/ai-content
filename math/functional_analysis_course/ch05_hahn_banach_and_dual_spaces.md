# Chapter 5: The Hahn-Banach Theorem and Dual Spaces

**Learning objectives:**
- Understand the Hahn-Banach theorem: you can extend functionals without increasing the norm
- Know what dual spaces are and why they matter
- Be able to identify the duals of $\ell^p$ and $L^p$
- Understand reflexivity and the bidual
- Get a first taste of weak and weak-* topologies

**Prerequisites:** Chapters 2, 4

---

## Motivation

In Chapter 4, we studied bounded linear operators — maps between normed spaces. Now we focus on a special and surprisingly powerful case: linear maps into the scalars. A **continuous linear functional** on a space $X$ is a bounded linear map $\varphi: X \to \mathbb{R}$ (or $\mathbb{C}$). The collection of all such functionals is the **dual space** $X^*$.

Why care about linear functionals? Because they're the simplest "measurements" you can make on vectors. A functional assigns a number to each vector in a way that respects the linear structure. The dual space $X^*$ encodes *all possible* such measurements, and it turns out that studying $X^*$ often reveals more about $X$ than studying $X$ directly.

The fundamental question is: is $X^*$ rich enough? Could it happen that $X^*$ is too small to distinguish different elements of $X$? The **Hahn-Banach theorem** answers: no. You can always extend a functional from a subspace to the whole space without increasing its norm, which guarantees that $X^*$ is large enough to "see" every element of $X$. This is the first of the "big four" theorems of functional analysis, and it has a fundamentally different character from the other three (Chapters 6–7): it's about *existence*, not structure, and it uses the axiom of choice rather than the Baire category theorem.

---

## Linear Functionals and the Dual Space

### Functionals as measurements

A **linear functional** on a vector space $X$ is a linear map $\varphi: X \to \mathbb{R}$. If $X$ is a normed space, we say $\varphi$ is **continuous** (or **bounded**) if $\|\varphi\| = \sup_{\|x\| \leq 1} |\varphi(x)| < \infty$.

Think of a functional as a "test" or "measurement." You feed it a vector and it returns a number. Different functionals measure different aspects of a vector.

> **Example:** On $C[0,1]$ with the sup norm:
> - **Evaluation functional:** $\varphi(f) = f(1/2)$. This measures the value at a specific point. It's bounded: $|\varphi(f)| \leq \|f\|_\infty$, so $\|\varphi\| = 1$.
> - **Integration functional:** $\varphi(f) = \int_0^1 f(t) \, dt$. This measures the average value. Bounded: $|\varphi(f)| \leq \|f\|_\infty$, so $\|\varphi\| \leq 1$. Taking $f = 1$ shows $\|\varphi\| = 1$.
> - **Weighted integration:** $\varphi(f) = \int_0^1 g(t) f(t) \, dt$ for a fixed $g \in L^1$. This measures the "correlation" of $f$ with $g$.

The **dual space** $X^*$ is the vector space of all continuous linear functionals on $X$, equipped with the operator norm. As we noted in Chapter 4, $X^*$ is always a Banach space — even if $X$ itself isn't complete. This is because $X^* = B(X, \mathbb{R})$, and $\mathbb{R}$ is complete.

### Connection to Riesz representation

In Chapter 3, we proved the Riesz representation theorem: on a Hilbert space $H$, every continuous linear functional has the form $\varphi(x) = \langle x, y \rangle$ for a unique $y \in H$. So $H^* \cong H$ — the dual space is a copy of the space itself.

For general Banach spaces, the picture is more complex. The dual space might look very different from the original space. That's why we need a general theorem — Hahn-Banach — to guarantee $X^*$ is useful.

---

## The Hahn-Banach Theorem

### The problem: extending functionals

Suppose $M$ is a subspace of a normed space $X$, and $\varphi: M \to \mathbb{R}$ is a bounded linear functional on $M$. Can we extend $\varphi$ to all of $X$ without increasing its norm?

In pictures:

```
X (whole space)
├── M (subspace)
│   └── φ: M → ℝ (bounded, ||φ|| = c)
│
└── Want: Φ: X → ℝ with Φ|_M = φ and ||Φ|| = c
```

The Hahn-Banach theorem says: yes, always.

### Statement (analytic form)

**Theorem (Hahn-Banach).** Let $M$ be a subspace of a real normed space $X$, and let $\varphi: M \to \mathbb{R}$ be a bounded linear functional. Then there exists a bounded linear functional $\Phi: X \to \mathbb{R}$ such that:
1. $\Phi(x) = \varphi(x)$ for all $x \in M$ (extension)
2. $\|\Phi\| = \|\varphi\|$ (norm preservation)

### Proof idea

The proof has two stages:

**Stage 1: Extend by one dimension.** Suppose $M$ is a proper subspace and $x_0 \in X \setminus M$. We want to extend $\varphi$ to $M_1 = M + \text{span}\{x_0\}$ by choosing $\Phi(x_0) = c$ for some real number $c$. The constraint is:

$$|\varphi(m) + c \cdot t| \leq \|\varphi\| \cdot \|m + t x_0\| \quad \text{for all } m \in M, \, t \in \mathbb{R}$$

This reduces to choosing $c$ in the interval $[\sup_m (\varphi(m) - \|\varphi\|\|m - x_0\|), \, \inf_m (-\varphi(m) + \|\varphi\|\|m + x_0\|)]$. The key: this interval is nonempty (by the triangle inequality), so a valid choice of $c$ exists.

**Stage 2: Extend to all of $X$.** If $X$ is finite-dimensional, iterate Stage 1 finitely many times. If $X$ is infinite-dimensional, use **Zorn's lemma** (a form of the axiom of choice): consider the partially ordered set of all norm-preserving extensions of $\varphi$ to subspaces containing $M$. Every chain has an upper bound (take the union), so by Zorn's lemma, a maximal element exists. If the maximal extension were defined on a proper subspace of $X$, we could extend it one more dimension — contradicting maximality. So the maximal extension is defined on all of $X$.

> **Note:** The use of Zorn's lemma is essential and cannot be avoided. There are models of set theory (without the axiom of choice) in which the Hahn-Banach theorem fails. This makes Hahn-Banach philosophically different from the Baire category theorems (Chapters 6–7), which use only completeness. Hahn-Banach is a result about *existence* guaranteed by the axiom of choice; the other big theorems are about *structure* guaranteed by completeness.

### The geometric form: separating hyperplanes

The Hahn-Banach theorem has an equivalent geometric formulation that's often more intuitive.

**Theorem (Geometric Hahn-Banach / Separation).** Let $C$ be a nonempty open convex set in a normed space $X$, and let $x_0 \notin C$. Then there exists a continuous linear functional $\varphi \in X^*$ and a constant $\alpha$ such that

$$\varphi(x) < \alpha \leq \varphi(x_0) \quad \text{for all } x \in C.$$

In words: you can always separate a point from a convex set by a **hyperplane** $\{x : \varphi(x) = \alpha\}$.

```
        ┌─────────────┐
        │             │        • x₀
        │     C       │
        │   (convex)  │    ─────── hyperplane: φ(x) = α
        │             │
        └─────────────┘

     φ(x) < α on C          φ(x₀) ≥ α
```

### The critical corollary: $X^*$ separates points

**Corollary.** For any $x \neq 0$ in a normed space $X$, there exists $\varphi \in X^*$ with $\|\varphi\| = 1$ and $\varphi(x) = \|x\|$.

*Proof.* Define $\varphi$ on $\text{span}\{x\}$ by $\varphi(\alpha x) = \alpha \|x\|$. This is a bounded linear functional with $\|\varphi\| = 1$. By Hahn-Banach, extend to all of $X$.

This corollary is what makes the dual space genuinely useful: for any two distinct points $x \neq y$, there exists a functional $\varphi \in X^*$ with $\varphi(x) \neq \varphi(y)$. The functionals in $X^*$ can "see" every point of $X$ — they separate points.

Without Hahn-Banach, we'd have no guarantee that $X^*$ is anything more than $\{0\}$.

---

## Identifying Dual Spaces

### The duality pairing for $\ell^p$

One of the most satisfying exercises in functional analysis is computing dual spaces concretely. The key result:

**Theorem.** For $1 \leq p < \infty$, $(\ell^p)^* \cong \ell^q$ where $\frac{1}{p} + \frac{1}{q} = 1$ (with $q = \infty$ when $p = 1$).

The isomorphism is given by: each $y \in \ell^q$ defines a functional $\varphi_y \in (\ell^p)^*$ via

$$\varphi_y(x) = \sum_{n=1}^\infty x_n y_n,$$

and every element of $(\ell^p)^*$ arises this way. Moreover, $\|\varphi_y\| = \|y\|_q$.

**Proof sketch for $(\ell^1)^* = \ell^\infty$:** Given $\varphi \in (\ell^1)^*$, define $y_n = \varphi(e_n)$ where $e_n$ is the $n$-th standard basis vector. Then $|y_n| = |\varphi(e_n)| \leq \|\varphi\| \|e_n\|_1 = \|\varphi\|$, so $\|y\|_\infty \leq \|\varphi\|$. Conversely, for any $x \in \ell^1$: $|\varphi(x)| = |\sum x_n \varphi(e_n)| = |\sum x_n y_n| \leq \|x\|_1 \|y\|_\infty$, so $\|\varphi\| \leq \|y\|_\infty$. Hence $\|\varphi\| = \|y\|_\infty$ and $\varphi = \varphi_y$.

### The table of dualities

| Space $X$ | Dual $X^*$ | Conjugate exponent |
|-----------|-----------|-------------------|
| $\ell^p$ ($1 < p < \infty$) | $\ell^q$ | $1/p + 1/q = 1$ |
| $\ell^1$ | $\ell^\infty$ | |
| $c_0$ | $\ell^1$ | |
| $L^p[a,b]$ ($1 < p < \infty$) | $L^q[a,b]$ | $1/p + 1/q = 1$ |
| $L^1[a,b]$ | $L^\infty[a,b]$ | |
| $C[a,b]$ | Signed Borel measures $\mathcal{M}[a,b]$ | (Riesz–Markov) |
| $H$ (Hilbert) | $H$ | (Riesz representation) |

### The anomaly: $(\ell^\infty)^*$

Notice the asymmetry: $(\ell^1)^* = \ell^\infty$, but $(\ell^\infty)^* \neq \ell^1$. The dual of $\ell^\infty$ is strictly larger than $\ell^1$ — it contains exotic objects called **Banach limits** and **finitely additive measures** that can't be represented as sequences. The space $(\ell^\infty)^*$ is so large and complicated that you can't write its elements down explicitly.

> **What goes wrong:** The natural map $\ell^1 \hookrightarrow (\ell^\infty)^*$ sends $y \in \ell^1$ to $\varphi_y(x) = \sum x_n y_n$. This is injective but not surjective. There exist continuous linear functionals on $\ell^\infty$ that *cannot* be represented as $\varphi_y$ for any $y \in \ell^1$. For example, a Banach limit $L$ assigns a "generalized limit" to every bounded sequence, extending the ordinary limit from convergent sequences to all bounded sequences. Such an $L$ exists by Hahn-Banach (extend the functional $\lim$ from the subspace $c$ of convergent sequences to all of $\ell^\infty$), but cannot be written as $\sum x_n y_n$ for any $y \in \ell^1$.

---

## Reflexivity and the Bidual

### The canonical embedding

For any normed space $X$, there is a natural map $J: X \to X^{**}$ (the **bidual** — the dual of the dual) defined by:

$$J(x)(\varphi) = \varphi(x) \quad \text{for all } \varphi \in X^*.$$

Each $x \in X$ defines a functional on $X^*$ by "evaluation at $x$." This map $J$ is:
- Linear
- Isometric: $\|J(x)\| = \|x\|$ (this uses Hahn-Banach to find $\varphi$ with $\varphi(x) = \|x\|$)
- Hence injective: $J(x) = 0$ implies $\|x\| = 0$ implies $x = 0$

So $X$ embeds isometrically into $X^{**}$. The question is: is this embedding surjective?

### Reflexive spaces

A Banach space $X$ is **reflexive** if $J: X \to X^{**}$ is surjective — i.e., every element of $X^{**}$ is evaluation at some $x \in X$.

| Space | Reflexive? | Why? |
|-------|-----------|------|
| $\ell^p$ ($1 < p < \infty$) | Yes | $(\ell^p)^{**} = (\ell^q)^* = \ell^p$ |
| $L^p$ ($1 < p < \infty$) | Yes | Same argument |
| Hilbert spaces | Yes | $H^* \cong H$, so $H^{**} \cong H$ |
| $\ell^1$ | **No** | $(\ell^1)^{**} = (\ell^\infty)^* \supsetneq \ell^1$ |
| $c_0$ | **No** | $(c_0)^{**} = (\ell^1)^* = \ell^\infty \supsetneq c_0$ |
| $L^1$ | **No** | $(L^1)^{**} = (L^\infty)^* \supsetneq L^1$ |

> **Note:** Reflexivity is a "Goldilocks" property. The spaces $\ell^1$ and $c_0$ are "too small" — their biduals are strictly larger. The space $\ell^\infty$ is "too large" — it's the dual of $\ell^1$ but its own dual is even larger. The spaces $\ell^p$ for $1 < p < \infty$ are "just right" — they equal their biduals.

### Why reflexivity matters

Reflexive spaces have better compactness properties. The key result:

**Theorem (Eberlein-Šmulian).** A Banach space is reflexive if and only if its closed unit ball is weakly compact.

This has concrete consequences for optimization: if you're minimizing a continuous functional over a bounded set in a reflexive space, weak compactness guarantees the minimum is achieved. In non-reflexive spaces (like $L^1$), minimizing sequences can "escape" — the minimum might not be attained. This is why PDE theory heavily uses $L^p$ spaces with $p > 1$ (reflexive) and why $L^1$ requires special techniques.

---

## Weak Topologies (Introduction)

### Why another topology?

The norm topology isn't the only way to define convergence in a Banach space. For some purposes — particularly compactness arguments — it's too strong. We need weaker notions of convergence.

### The weak topology on $X$

A sequence $(x_n)$ in $X$ converges **weakly** to $x$ (written $x_n \rightharpoonup x$) if

$$\varphi(x_n) \to \varphi(x) \quad \text{for every } \varphi \in X^*.$$

Instead of requiring $\|x_n - x\| \to 0$ (convergence "in all directions simultaneously"), we only require convergence when "tested" against each individual functional. This is a weaker requirement.

> **Example:** In $\ell^2$, the standard basis vectors $e_n = (0, \ldots, 0, 1, 0, \ldots)$ converge weakly to $0$: for any $y \in \ell^2 = (\ell^2)^*$, $\langle e_n, y \rangle = y_n \to 0$ (since $y \in \ell^2$ means $\sum |y_n|^2 < \infty$, so $y_n \to 0$). But $\|e_n\| = 1$ for all $n$, so $(e_n)$ does **not** converge in norm. Weak convergence is strictly weaker than norm convergence.

### The weak-* topology on $X^*$

A sequence $(\varphi_n)$ in $X^*$ converges **weak-*** to $\varphi$ (written $\varphi_n \overset{*}{\rightharpoonup} \varphi$) if

$$\varphi_n(x) \to \varphi(x) \quad \text{for every } x \in X.$$

This is convergence "tested" against elements of $X$ (not $X^{**}$). The weak-* topology is weaker than the weak topology on $X^*$, and it has a crucial compactness property:

**Theorem (Banach-Alaoglu).** The closed unit ball of $X^*$ is compact in the weak-* topology.

This is a profound result. In infinite dimensions, the closed unit ball is never compact in the norm topology (recall from Chapter 1). Banach-Alaoglu says: switch to the weak-* topology, and compactness is restored. This "recovery of compactness" is essential throughout PDE theory, calculus of variations, and probability.

| Topology | Convergence means | Unit ball compact? |
|----------|------------------|--------------------|
| Norm (strong) | $\|x_n - x\| \to 0$ | Only in finite dimensions |
| Weak | $\varphi(x_n) \to \varphi(x)$ for all $\varphi \in X^*$ | Iff $X$ is reflexive (Eberlein-Šmulian) |
| Weak-* (on $X^*$) | $\varphi_n(x) \to \varphi(x)$ for all $x \in X$ | **Always** (Banach-Alaoglu) |

### Misconception: weak convergence is not pointwise convergence

In spaces of functions, it's tempting to confuse weak convergence with pointwise convergence. They're different. Weak convergence in $L^2[0,1]$ means $\int f_n g \to \int f g$ for every $g \in L^2$, which is a statement about integrals against test functions, not about values at individual points.

However, there are connections. In $\ell^p$ ($1 < p < \infty$), weak convergence is equivalent to boundedness plus coordinate-wise convergence — which is a form of "pointwise" convergence for sequences.

---

## Key Takeaways

- **The Hahn-Banach theorem guarantees extension of functionals** without increasing the norm. It ensures $X^*$ separates points of $X$ — the dual space is "rich enough" to study $X$.
- **Hahn-Banach is an existence theorem** (using Zorn's lemma), fundamentally different from the structure theorems of Chapters 6–7 (using Baire category). The four big theorems have two different "engines."
- **Dual spaces vary in complexity.** For Hilbert spaces, $H^* \cong H$. For $\ell^p$ with $1 < p < \infty$, $(\ell^p)^* = \ell^q$. For $\ell^\infty$, the dual is intractably large. Reflexive spaces ($X = X^{**}$) are the "sweet spot."
- **Weak topologies recover compactness.** The norm topology on an infinite-dimensional space is never compact on bounded sets. Weak and weak-* topologies are weaker, and Banach-Alaoglu guarantees weak-* compactness of the unit ball.
- **Duality is a lens.** Studying $X$ through its dual $X^*$ is one of the most powerful techniques in functional analysis. The interaction between a space and its dual — through Hahn-Banach, reflexivity, and weak topologies — is a central theme of the subject.

---

## What's Next

We've now met the first of the "big four" theorems — Hahn-Banach, which is about existence and extension. In **Chapter 6: The Uniform Boundedness Principle**, we encounter a theorem with a completely different flavor. Where Hahn-Banach used the axiom of choice to guarantee existence, the uniform boundedness principle uses completeness (via the Baire category theorem) to upgrade pointwise bounds to uniform bounds. It's the first of three "structure" theorems that reveal deep consequences of working in Banach spaces, and its applications — including a stunning proof that there exist continuous functions with divergent Fourier series — showcase the power of the abstract machinery.
