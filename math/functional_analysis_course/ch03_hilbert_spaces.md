# Chapter 3: Inner Products and Hilbert Spaces

**Learning objectives:**
- Understand inner products as the tool that brings "angle" and "projection" to infinite-dimensional spaces
- Know the projection theorem and why it's the single most useful result in Hilbert space theory
- See Fourier series as a Hilbert space phenomenon — not just a calculus trick
- Understand orthonormal bases and how they differ from algebraic (Hamel) bases
- Know the Riesz representation theorem for Hilbert spaces

**Prerequisites:** Chapter 2 (Normed Spaces and Banach Spaces)

---

## Motivation

In Chapter 2, we equipped function spaces with norms — a way to measure "how big" a function is — and discovered that completeness (the Banach space property) is the right condition for analysis. But norms alone give us only *size*. They tell us the distance between two vectors, but they can't tell us about *angles*, *perpendicularity*, or *projection*.

Think about what you lose without angles. In $\mathbb{R}^n$, if you want the best approximation to a vector $v$ inside a subspace $W$, you drop a perpendicular from $v$ to $W$. This is the geometric heart of least-squares fitting, Fourier analysis, signal processing, and quantum mechanics. All of it depends on having a notion of "perpendicular" — which norms alone don't provide.

**Inner products** are the missing piece. They give infinite-dimensional spaces an almost Euclidean geometry: angles, orthogonality, projections, and the Pythagorean theorem. A complete inner product space is called a **Hilbert space**, and Hilbert spaces are the best-behaved infinite-dimensional spaces you'll encounter. This is the chapter where functional analysis becomes *geometric*.

---

## Inner Products: Angles in Function Space

### Why we need more than a norm

In $\mathbb{R}^n$, the dot product $\langle x, y \rangle = \sum x_i y_i$ does double duty: it defines both a norm ($\|x\| = \sqrt{\langle x, x \rangle}$) and a notion of angle ($\cos\theta = \langle x, y \rangle / \|x\|\|y\|$). Not every norm comes from an inner product. An inner product is strictly more structure than a norm — and it's exactly the structure we need for geometry.

An **inner product** on a (real) vector space $X$ is a function $\langle \cdot, \cdot \rangle: X \times X \to \mathbb{R}$ satisfying:

1. **Linearity in the first argument:** $\langle \alpha x + \beta y, z \rangle = \alpha\langle x, z \rangle + \beta\langle y, z \rangle$
2. **Symmetry:** $\langle x, y \rangle = \langle y, x \rangle$ (or conjugate symmetry for complex spaces)
3. **Positive definiteness:** $\langle x, x \rangle \geq 0$, with equality iff $x = 0$

Every inner product induces a norm: $\|x\| = \sqrt{\langle x, x \rangle}$. The fundamental inequality connecting them is **Cauchy-Schwarz**:

$$|\langle x, y \rangle| \leq \|x\| \cdot \|y\|$$

This is the inequality that makes the "angle" formula $\cos\theta = \langle x, y \rangle / (\|x\|\|y\|)$ well-defined.

### The key examples

**$\ell^2$:** Sequences $x = (x_1, x_2, \ldots)$ with $\sum |x_n|^2 < \infty$, with inner product $\langle x, y \rangle = \sum_{n=1}^\infty x_n y_n$. This is the infinite-dimensional Euclidean space.

**$L^2[a,b]$:** (Equivalence classes of) square-integrable functions, with inner product

$$\langle f, g \rangle = \int_a^b f(x) g(x) \, dx.$$

This is the most important function space in applied mathematics. The inner product computes a "continuous dot product" — summing products of values, weighted by $dx$.

### The parallelogram law: a diagnostic test

Not every norm comes from an inner product. The **parallelogram law** gives an exact characterization:

$$\|x + y\|^2 + \|x - y\|^2 = 2\|x\|^2 + 2\|y\|^2$$

A norm satisfies this identity if and only if it comes from an inner product (via the **polarization identity**, which recovers the inner product from the norm).

> **What goes wrong:** The $L^1$ and $L^\infty$ norms on $C[0,1]$ fail the parallelogram law. Take $f(x) = 1$ and $g(x) = x$ on $[0,1]$:
>
> Under $\|\cdot\|_\infty$:
> - $\|f + g\|_\infty = 2$, $\|f - g\|_\infty = 1$
> - Left side: $4 + 1 = 5$
> - Right side: $2(1) + 2(1) = 4$
> - $5 \neq 4$ — the parallelogram law fails.
>
> So $C[0,1]$ with the sup norm is a Banach space but **not** a Hilbert space. This means we can measure size but not angles in this space. The geometric toolkit of projections and orthogonality is unavailable — a real limitation.

Only $\ell^2$ and $L^2$ (among the $\ell^p$ and $L^p$ families) are Hilbert spaces. This makes $p = 2$ special in a way that goes beyond convenience.

---

## The Projection Theorem

### The most important single result

If you could only know one theorem about Hilbert spaces, this should be it. The **projection theorem** says: in a Hilbert space $H$, for any closed subspace $M$ and any point $x \in H$, there exists a unique closest point $\hat{x} \in M$ to $x$, and the "error" $x - \hat{x}$ is orthogonal to $M$.

$$x = \hat{x} + (x - \hat{x}), \quad \hat{x} \in M, \quad (x - \hat{x}) \perp M$$

Geometrically:

```
        x
        |  ⟂
        |
        |
   -----*̂x------------ M (closed subspace)

   x - x̂ is perpendicular to M
   x̂ is the unique closest point in M to x
```

This is exactly what happens in $\mathbb{R}^n$ — the closest point in a subspace is the perpendicular projection. The miracle is that it works in infinite dimensions too, as long as the space is a Hilbert space and the subspace is closed.

**Why completeness is essential.** Consider the subspace $M$ of polynomials inside $L^2[0,1]$. If we tried to do this in the *incomplete* inner product space of polynomials, the projection might not exist — the minimizing sequence might converge to a non-polynomial function. Completeness guarantees the limit stays in the space.

> **Example:** Let $H = L^2[-\pi, \pi]$ and let $M = \text{span}\{1, \cos x, \sin x\}$. Project $f(x) = x$ onto $M$.
>
> Since $\{1, \cos x, \sin x\}$ are orthogonal in $L^2[-\pi, \pi]$ (you can verify this by direct integration), the projection is:
>
> $$\hat{f} = \frac{\langle x, 1 \rangle}{\langle 1, 1 \rangle} \cdot 1 + \frac{\langle x, \cos x \rangle}{\langle \cos x, \cos x \rangle} \cos x + \frac{\langle x, \sin x \rangle}{\langle \sin x, \sin x \rangle} \sin x$$
>
> Computing: $\langle x, 1 \rangle = \int_{-\pi}^{\pi} x \, dx = 0$ (odd function). $\langle x, \cos x \rangle = \int_{-\pi}^{\pi} x \cos x \, dx = 0$ (odd times even is odd). $\langle x, \sin x \rangle = \int_{-\pi}^{\pi} x \sin x \, dx = 2\pi$ (by integration by parts).
>
> So $\hat{f}(x) = \frac{2\pi}{\pi} \sin x = 2\sin x$. The best approximation to $f(x) = x$ by a function in $\text{span}\{1, \cos x, \sin x\}$ is $2\sin x$.

> **Note:** The projection theorem also holds for closed *convex* sets, not just subspaces. This more general form is crucial for optimization: the closest point in a closed convex constraint set is always well-defined in a Hilbert space.

### Every Hilbert space decomposes

A consequence of the projection theorem: if $M$ is a closed subspace of $H$, then every $x \in H$ decomposes uniquely as $x = m + m^\perp$ with $m \in M$ and $m^\perp \in M^\perp$. We write $H = M \oplus M^\perp$ (orthogonal direct sum). This clean decomposition — every element splits into a "component in $M$" and a "component perpendicular to $M$" — is what makes Hilbert spaces so much more tractable than general Banach spaces.

---

## Orthonormal Bases and Fourier Series

### A new kind of basis

In finite-dimensional linear algebra, a basis is a linearly independent set that spans the space. In an $n$-dimensional space, a basis has exactly $n$ elements, and every vector is a *finite* linear combination of basis vectors.

In infinite-dimensional Hilbert spaces, the right notion of "basis" is different. An **orthonormal basis** (ONB) is a set $\{e_n\}$ that is:
- **Orthonormal:** $\langle e_m, e_n \rangle = \delta_{mn}$
- **Complete:** the only vector orthogonal to every $e_n$ is zero

Every $x \in H$ can be expanded as $x = \sum_{n=1}^\infty \langle x, e_n \rangle \, e_n$, where the sum converges in the norm of $H$. This is an *infinite* series, not a finite combination — the closure in the definition is essential.

> **Note:** This is a **Schauder basis**, not a **Hamel basis**. A Hamel basis for $L^2$ would be uncountable and impossible to write down explicitly. The ONB is countable (for separable Hilbert spaces) and the expansion involves limits. The distinction matters: when we say "basis" in Hilbert space theory, we always mean ONB.

### The Gram-Schmidt process

Given any countable linearly independent set in a Hilbert space, Gram-Schmidt produces an orthonormal set — exactly as in $\mathbb{R}^n$. Start with $v_1, v_2, \ldots$:

1. $e_1 = v_1 / \|v_1\|$
2. $\tilde{e}_2 = v_2 - \langle v_2, e_1 \rangle e_1$, then $e_2 = \tilde{e}_2 / \|\tilde{e}_2\|$
3. Continue: subtract off projections onto all previous $e_k$'s, then normalize.

Applying this to $\{1, x, x^2, \ldots\}$ in $L^2[-1,1]$ produces the **Legendre polynomials**. Different starting sets and intervals give different families of orthogonal polynomials — Chebyshev, Hermite, Laguerre — each adapted to a particular weight function.

### Parseval's identity and Bessel's inequality

Two fundamental quantitative results:

**Bessel's inequality:** For any orthonormal set (not necessarily a basis) and any $x \in H$:

$$\sum_n |\langle x, e_n \rangle|^2 \leq \|x\|^2$$

**Parseval's identity:** If $\{e_n\}$ is an ONB, this becomes an equality:

$$\sum_{n=1}^\infty |\langle x, e_n \rangle|^2 = \|x\|^2$$

Parseval's identity is the infinite-dimensional Pythagorean theorem. The squared norm of a vector equals the sum of the squares of its "components" — exactly as in $\mathbb{R}^n$, but now with infinitely many terms.

### Fourier series: the canonical example

The functions $e_n(x) = e^{inx}/\sqrt{2\pi}$ for $n \in \mathbb{Z}$ form an ONB for $L^2[-\pi, \pi]$. The expansion of $f$ in this basis is the **Fourier series**:

$$f(x) = \sum_{n=-\infty}^{\infty} c_n e^{inx}, \quad c_n = \frac{1}{2\pi}\int_{-\pi}^{\pi} f(x) e^{-inx} dx$$

Parseval's identity becomes: $\sum |c_n|^2 = \frac{1}{2\pi}\int_{-\pi}^{\pi} |f(x)|^2 dx$.

> **Example:** Take $f(x) = x$ on $[-\pi, \pi]$. The Fourier coefficients are $c_0 = 0$, $c_n = i(-1)^{n+1}/n$ for $n \neq 0$. Parseval gives:
>
> $$\sum_{n \neq 0} \frac{1}{n^2} = \frac{1}{2\pi} \int_{-\pi}^{\pi} x^2 \, dx = \frac{\pi^2}{3}$$
>
> So $2\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{3}$, which gives $\sum_{n=1}^\infty \frac{1}{n^2} = \frac{\pi^2}{6}$. This is the **Basel problem**, solved here as a *corollary* of Hilbert space theory!

### Convergence: $L^2$ vs. pointwise

The Fourier series converges to $f$ in $L^2$ — meaning $\|f - S_N f\|_2 \to 0$ where $S_N$ is the $N$-th partial sum. This does **not** mean pointwise convergence. The Fourier series of a continuous function can diverge at individual points (we'll see this dramatically in Chapter 6 via the uniform boundedness principle).

The distinction is important: $L^2$ convergence is a statement about "average agreement," not "point-by-point agreement." Hilbert space theory gives us $L^2$ convergence automatically; pointwise convergence requires additional hypotheses.

---

## The Riesz Representation Theorem

### Every functional is inner product with something

This is one of the most elegant results in functional analysis, and it's what makes Hilbert spaces truly special.

**Theorem (Riesz Representation):** Let $H$ be a Hilbert space and $\varphi: H \to \mathbb{R}$ (or $\mathbb{C}$) a continuous linear functional. Then there exists a unique $y \in H$ such that

$$\varphi(x) = \langle x, y \rangle \quad \text{for all } x \in H.$$

Moreover, $\|\varphi\| = \|y\|$.

In other words: the dual space $H^*$ is isometrically isomorphic to $H$ itself. Every "measurement" of vectors is secretly "inner product with some fixed vector."

### Proof idea

The proof uses the projection theorem directly. Given $\varphi \neq 0$, the kernel $M = \ker(\varphi)$ is a closed hyperplane. Take any $z \notin M$ and let $z_0$ be its component perpendicular to $M$ (via the projection theorem). Then $y$ is a scalar multiple of $z_0$, chosen so that the inner product reproduces $\varphi$.

### Why this matters

Compare the dual space situation across different Banach spaces:

| Space | Dual space | How "nice"? |
|-------|-----------|-------------|
| $\ell^2$ | $\ell^2$ (itself!) | Hilbert — maximally nice |
| $\ell^p$ ($1 < p < \infty$) | $\ell^q$ ($1/p + 1/q = 1$) | Reflexive — dual is similar |
| $\ell^1$ | $\ell^\infty$ | Dual is bigger and different |
| $c_0$ | $\ell^1$ | Dual is different |
| $\ell^\infty$ | Huge (finitely additive measures) | Intractable |

Hilbert spaces sit at the top: their dual is a copy of themselves. No identifications to memorize, no measure theory to invoke — just inner products. This self-duality is what makes Hilbert spaces the easiest infinite-dimensional spaces to work in, and why so much of applied mathematics (signal processing, quantum mechanics, statistics) lives in $L^2$.

> **Note:** In Chapter 5, we'll see the **Hahn-Banach theorem**, which guarantees that dual spaces are "rich enough" for general Banach spaces. But identifying the dual concretely is much harder outside Hilbert spaces. The Riesz representation theorem handles the Hilbert case completely and beautifully.

### Connection to quantum mechanics

In quantum mechanics, states are vectors in a Hilbert space $H$, and observables correspond to self-adjoint operators. The Riesz representation theorem underlies Dirac's **bra-ket notation**: $\langle \psi |$ (a "bra") is the functional $x \mapsto \langle x, \psi \rangle$, which by Riesz is just the vector $\psi$ in disguise. The notation treats vectors and functionals interchangeably — something that only works in Hilbert spaces.

---

## Key Takeaways

- **Inner products add geometry** — angles, orthogonality, and projection — to normed spaces. Among $L^p$ and $\ell^p$ spaces, only $p = 2$ gives an inner product (verified by the parallelogram law).
- **The projection theorem** says the closest point in a closed subspace exists, is unique, and the error is perpendicular. This requires completeness — incomplete spaces can have "missing" projections. It's the most practically useful result in Hilbert space theory.
- **Orthonormal bases** give infinite series expansions $x = \sum \langle x, e_n \rangle e_n$, with Parseval's identity as the infinite Pythagorean theorem. Fourier series are the canonical example.
- **The Riesz representation theorem** says $H^* \cong H$ — every continuous linear functional is inner product with a fixed vector. This self-duality makes Hilbert spaces uniquely tractable among Banach spaces.
- **Fourier convergence in $L^2$ is automatic** from the Hilbert space machinery. Pointwise convergence is a separate (and harder) question.

---

## What's Next

We've now built the two main types of "nice" infinite-dimensional spaces: Banach spaces (Chapter 2) for general analysis, and Hilbert spaces (this chapter) for geometric analysis. In **Chapter 4: Bounded Linear Operators**, we shift focus from spaces to the *maps between them*. Linear maps between finite-dimensional spaces are always continuous — in infinite dimensions, this fails dramatically. Understanding which linear maps are "well-behaved" (bounded/continuous), how to measure them (operator norm), and how operators compose and invert is essential preparation for the four great theorems of Chapters 5–7 and the spectral theory of Chapters 8–9.
