# Chapter 4: Bounded Linear Operators

**Learning objectives:**
- Understand why continuity and boundedness are equivalent for linear operators between normed spaces
- Be able to compute operator norms for concrete operators
- Know key examples: integral operators, multiplication operators, shift operators
- Understand that $B(X,Y)$ is itself a Banach space (when $Y$ is)

**Prerequisites:** Chapter 2 (Normed Spaces and Banach Spaces)

---

## Motivation

So far, our focus has been on the *spaces* — normed spaces, Banach spaces, Hilbert spaces. We've built the stages. Now it's time to study the *actors*: the linear maps between these spaces.

In finite dimensions, every linear map $T: \mathbb{R}^n \to \mathbb{R}^m$ is automatically continuous. You represent it as a matrix, matrix multiplication is continuous, and you're done. This is so automatic that most linear algebra courses don't even mention continuity of linear maps — it's free.

In infinite dimensions, this gift vanishes. A linear map between infinite-dimensional spaces can be spectacularly discontinuous. The differentiation operator — one of the most natural operations in all of mathematics — is a prime example. Understanding *which* linear maps are continuous, and *how* to measure their "size," is the subject of this chapter. The answers will be essential for everything that follows: the four great theorems (Chapters 5–7), compact operators (Chapter 8), and spectral theory (Chapter 9).

---

## Bounded Equals Continuous (For Linear Maps)

### The key equivalence

For a general function between metric spaces, boundedness and continuity are unrelated properties. The function $f(x) = \sin(x)$ is bounded but not constant; $f(x) = x$ is continuous but unbounded. For *linear* maps, the situation is completely different.

**Theorem.** Let $T: X \to Y$ be a linear map between normed spaces. The following are equivalent:
1. $T$ is continuous (at every point)
2. $T$ is continuous at $0$
3. $T$ is **bounded**: there exists $C \geq 0$ such that $\|Tx\| \leq C\|x\|$ for all $x \in X$

The equivalence of (1) and (2) is a consequence of linearity: if $T$ is continuous at $0$ and $x_n \to x$, then $x_n - x \to 0$, so $T(x_n - x) = Tx_n - Tx \to 0$, hence $Tx_n \to Tx$. Continuity at one point propagates everywhere.

The equivalence of (2) and (3) is the important one. If $T$ is bounded, then $\|Tx_n - Tx\| = \|T(x_n - x)\| \leq C\|x_n - x\| \to 0$, so $T$ is continuous. Conversely, if $T$ is continuous at $0$, then the preimage of the unit ball in $Y$ contains some ball $B(0, \delta)$ in $X$, and linearity stretches this into the bound $\|Tx\| \leq (1/\delta)\|x\|$.

> **Note:** The term "bounded" for a linear operator does **not** mean the operator has bounded range. The map $T: \mathbb{R} \to \mathbb{R}$ given by $Tx = 2x$ is "bounded" as a linear operator (with $C = 2$), even though its range is all of $\mathbb{R}$. "Bounded" means "bounded relative to the input size" — the output is at most a constant multiple of the input. A better name might be "Lipschitz," but the terminology is entrenched.

### What goes wrong: an unbounded linear operator

The differentiation operator $T: C^1[0,1] \to C[0,1]$ defined by $Tf = f'$ is linear. Is it bounded (using the sup norm on both spaces)?

Consider $f_n(x) = \sin(nx)/n$. Then:
- $\|f_n\|_\infty = 1/n \to 0$
- $\|Tf_n\|_\infty = \|f_n'\|_\infty = \|\cos(nx)\|_\infty = 1$

So $\|Tf_n\| / \|f_n\| = n \to \infty$. No constant $C$ can satisfy $\|Tf\| \leq C\|f\|$ for all $f$. The differentiation operator is **unbounded** — equivalently, **discontinuous**.

```
Input shrinks:     ||f_n||_∞ = 1/n → 0
                   ──────────────────→

Output stays put:  ||Tf_n||_∞ = 1 (constant!)
                   ═══════════════════

Differentiation amplifies high-frequency oscillations.
Small wiggles (small amplitude, high frequency) have large derivatives.
```

This is the fundamental issue: differentiation amplifies high-frequency content. A function can be small in sup norm but have large derivatives if it oscillates rapidly. In finite dimensions, there's no room for arbitrarily rapid oscillation — the space is too "rigid." In infinite dimensions, you have infinitely many independent directions to oscillate in.

> **Note:** This doesn't mean differentiation is useless in functional analysis — it means we need to be more careful about which spaces we use. If we equip $C^1[0,1]$ with the norm $\|f\|_{C^1} = \|f\|_\infty + \|f'\|_\infty$, then differentiation $T: (C^1, \|\cdot\|_{C^1}) \to (C, \|\cdot\|_\infty)$ *is* bounded with $\|T\| \leq 1$. The operator isn't intrinsically "bad" — it's bounded or unbounded depending on the choice of norms.

### Misconception: "unbounded operators don't arise in practice"

Quite the opposite. The most important operators in mathematical physics — the position operator, the momentum operator, the Laplacian, the Hamiltonian of quantum mechanics — are all unbounded. They're defined on dense subspaces of Hilbert spaces, not on the whole space, and their theory requires careful handling of domains. This is the theory of **unbounded operators**, which lies beyond our course but is the natural sequel (see Chapter 10).

---

## The Operator Norm

### Measuring the size of a linear map

If a bounded operator satisfies $\|Tx\| \leq C\|x\|$, the smallest such $C$ is the **operator norm**:

$$\|T\| = \sup_{\|x\| \leq 1} \|Tx\| = \sup_{x \neq 0} \frac{\|Tx\|}{\|x\|}$$

The operator norm measures the maximum "amplification factor" of $T$. It's the worst-case ratio of output size to input size.

Equivalent formulations:

$$\|T\| = \sup_{\|x\| = 1} \|Tx\| = \sup_{\|x\| \leq 1} \|Tx\| = \inf\{C \geq 0 : \|Tx\| \leq C\|x\| \text{ for all } x\}$$

> **Note:** The sup in the definition might or might not be attained. If $X$ is finite-dimensional, compactness of the unit sphere guarantees the sup is a max. In infinite dimensions, the sup might not be attained — it's a genuine supremum. For example, the identity operator $I: c_0 \to c_0$ has $\|I\| = 1$, attained at $e_1 = (1, 0, 0, \ldots)$. But some operators only approach their norm asymptotically.

### Worked examples

**Example 1: The left shift on $\ell^2$.**

Define $L: \ell^2 \to \ell^2$ by $L(x_1, x_2, x_3, \ldots) = (x_2, x_3, x_4, \ldots)$. Then:

$$\|Lx\|^2 = \sum_{n=2}^\infty |x_n|^2 \leq \sum_{n=1}^\infty |x_n|^2 = \|x\|^2$$

So $\|L\| \leq 1$. Is $\|L\| = 1$? Take $x = e_2 = (0, 1, 0, 0, \ldots)$. Then $Lx = (1, 0, 0, \ldots)$, so $\|Lx\| = 1 = \|x\|$. Hence $\|L\| = 1$.

**Example 2: An integral operator.**

Define $T: C[0,1] \to C[0,1]$ by $(Tf)(x) = \int_0^x f(t) \, dt$. What is $\|T\|$ with the sup norm?

For $\|f\|_\infty \leq 1$:

$$|(Tf)(x)| = \left|\int_0^x f(t) \, dt\right| \leq \int_0^x |f(t)| \, dt \leq x \leq 1$$

So $\|T\| \leq 1$. Is this achieved? Take $f(t) = 1$ (the constant function). Then $(Tf)(x) = x$, so $\|Tf\|_\infty = 1$. Hence $\|T\| = 1$.

**Example 3: A multiplication operator on $L^2$.**

Let $g \in L^\infty[0,1]$ and define $M_g: L^2[0,1] \to L^2[0,1]$ by $(M_g f)(x) = g(x)f(x)$. Then:

$$\|M_g f\|_2^2 = \int_0^1 |g(x)|^2 |f(x)|^2 \, dx \leq \|g\|_\infty^2 \int_0^1 |f(x)|^2 \, dx = \|g\|_\infty^2 \|f\|_2^2$$

So $\|M_g\| \leq \|g\|_\infty$. In fact, $\|M_g\| = \|g\|_\infty$ (equality is achieved by concentrating $f$ where $|g|$ is near its supremum).

| Operator | Space | Formula | Norm |
|----------|-------|---------|------|
| Left shift $L$ | $\ell^2 \to \ell^2$ | $(x_1,x_2,\ldots) \mapsto (x_2,x_3,\ldots)$ | 1 |
| Right shift $R$ | $\ell^2 \to \ell^2$ | $(x_1,x_2,\ldots) \mapsto (0,x_1,x_2,\ldots)$ | 1 |
| Integration | $C[0,1] \to C[0,1]$ | $f \mapsto \int_0^x f(t) \, dt$ | 1 |
| Multiplication by $g$ | $L^2 \to L^2$ | $f \mapsto gf$ | $\|g\|_\infty$ |
| Differentiation | $C^1[0,1] \to C[0,1]$ (sup norm on both) | $f \mapsto f'$ | $\infty$ (unbounded) |

---

## The Space $B(X, Y)$

### Operators form a vector space

The set of all bounded linear operators from $X$ to $Y$, denoted $B(X, Y)$ (or $\mathcal{L}(X, Y)$), is itself a vector space: you can add operators and multiply them by scalars, and the result is still a bounded linear operator.

With the operator norm, $B(X, Y)$ becomes a normed space. The operator norm satisfies all three norm axioms — positive definiteness, homogeneity, and the triangle inequality. (The triangle inequality for the operator norm follows from the triangle inequality in $Y$.)

### When $Y$ is Banach, so is $B(X, Y)$

**Theorem.** If $Y$ is a Banach space, then $B(X, Y)$ is a Banach space.

The proof follows the same pattern as the completeness proof for $\ell^p$ in Chapter 2:

1. Take a Cauchy sequence $(T_n)$ in $B(X, Y)$.
2. For each fixed $x$, $(T_n x)$ is Cauchy in $Y$ (since $\|T_n x - T_m x\| \leq \|T_n - T_m\| \cdot \|x\|$).
3. By completeness of $Y$, define $Tx = \lim_n T_n x$.
4. Verify $T$ is linear and bounded, and $\|T_n - T\| \to 0$.

The pattern: completeness of the target space gives pointwise limits, and the Cauchy condition in operator norm ensures these assemble into a genuine bounded operator.

> **Convince yourself:** Where does the proof use that $Y$ is complete? (In step 3: without completeness of $Y$, the limit $Tx$ might not exist.) What if $X$ is not complete — does the conclusion change? (No — the proof only uses completeness of $Y$.)

### The special case: dual spaces

When $Y = \mathbb{R}$ (or $\mathbb{C}$), the space $B(X, \mathbb{R})$ is the **dual space** $X^*$ — the space of all continuous linear functionals on $X$. Since $\mathbb{R}$ is complete, $X^*$ is always a Banach space, even if $X$ isn't!

This is a remarkable fact: the dual space of *any* normed space is automatically complete. We'll explore dual spaces in depth in Chapter 5, where the Hahn-Banach theorem guarantees they're "rich enough" to be useful.

---

## Invertibility and the Neumann Series

### When can we solve $Tx = y$?

Given $T \in B(X, Y)$, the equation $Tx = y$ asks: is $y$ in the range of $T$? And if so, is the solution unique and stable (does it depend continuously on $y$)?

- **Existence:** $T$ must be surjective.
- **Uniqueness:** $T$ must be injective.
- **Stability:** $T^{-1}$ must be bounded.

In finite dimensions, bijectivity automatically gives stability — the inverse of an invertible matrix is always a matrix (hence bounded). In infinite dimensions, a bijective bounded operator *could* in principle have an unbounded inverse. (Spoiler: the inverse mapping theorem in Chapter 7 will show this can't happen on Banach spaces. But we don't have that yet.)

### The Neumann series: an operator-valued geometric series

Think of the geometric series: if $|r| < 1$, then $\sum_{n=0}^\infty r^n = 1/(1-r)$. The Neumann series is the operator-valued version.

**Theorem.** Let $X$ be a Banach space and $T \in B(X)$ with $\|T\| < 1$. Then $I - T$ is invertible, and

$$(I - T)^{-1} = \sum_{n=0}^\infty T^n$$

where the series converges in operator norm.

*Proof sketch.* The partial sums $S_N = \sum_{n=0}^N T^n$ form a Cauchy sequence in $B(X)$, since $\|T^n\| \leq \|T\|^n$ and $\sum \|T\|^n < \infty$ (geometric series). By completeness of $B(X)$ (which holds because $X$ is Banach), the series converges to some $S \in B(X)$. A direct calculation shows $(I-T)S = S(I-T) = I$.

The bound on the inverse: $\|(I-T)^{-1}\| \leq \sum \|T\|^n = 1/(1-\|T\|)$.

> **Example:** Let $A \in B(X)$ be any bounded operator on a Banach space, and suppose we want to solve $(A + \epsilon B)x = y$ where $B$ is a bounded perturbation and $\epsilon$ is small. If $A$ is invertible, write:
>
> $$A + \epsilon B = A(I + \epsilon A^{-1}B).$$
>
> When $|\epsilon| \cdot \|A^{-1}\| \cdot \|B\| < 1$, the Neumann series tells us $I + \epsilon A^{-1}B$ is invertible, so $A + \epsilon B$ is invertible. Small perturbations of invertible operators are invertible — this is the **stability of invertibility**, and it's why the set of invertible operators in $B(X)$ is *open*.

### What the Neumann series doesn't do

The Neumann series requires $\|T\| < 1$. What if $\|T\| \geq 1$ but $T$ is still "small" in some other sense? The spectral radius formula (Chapter 9) will tell us that the real condition is $r(T) < 1$ (spectral radius less than 1), which can hold even when $\|T\| \geq 1$. But proving convergence of $\sum T^n$ then requires more delicate estimates on $\|T^n\|$.

---

## Key Takeaways

- **Bounded = continuous for linear operators.** This equivalence is the foundation of operator theory. Linearity means "bad behavior at one point spreads everywhere," so continuity at zero is enough.
- **The operator norm measures worst-case amplification.** $\|T\| = \sup_{\|x\| \leq 1} \|Tx\|$ captures the maximum stretching factor. Different operators on the same spaces can have vastly different norms.
- **Unbounded operators exist and matter.** The differentiation operator is the canonical example — it's unbounded because it amplifies high-frequency oscillations. Important operators in physics (Hamiltonian, momentum) are also unbounded.
- **$B(X,Y)$ is Banach when $Y$ is.** The space of bounded operators inherits completeness from the target space. The special case $B(X, \mathbb{R}) = X^*$ (the dual space) is always Banach.
- **The Neumann series gives invertibility.** If $\|T\| < 1$, then $(I-T)^{-1} = \sum T^n$ converges in operator norm. This gives stability of invertibility: small perturbations of invertible operators remain invertible.

---

## What's Next

We've studied arbitrary bounded linear operators. What happens when we study the special case where the target space is the scalars — i.e., linear *functionals*? In **Chapter 5: The Hahn-Banach Theorem and Dual Spaces**, we'll see that the dual space $X^*$ is a powerful tool for understanding $X$ itself. The Hahn-Banach theorem — the first of the "big four" theorems of functional analysis — guarantees that $X^*$ is rich enough to "see" every element of $X$, and it's the existence theorem that makes the entire duality machinery work.
