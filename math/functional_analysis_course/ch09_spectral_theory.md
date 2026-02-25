# Chapter 9: Spectral Theory

**Learning objectives:**
- Understand the spectrum of a bounded operator (point, continuous, residual)
- Compute spectra for concrete operators (shifts, multiplication operators)
- Know the spectral radius formula
- Understand the spectral theorem for bounded self-adjoint operators (statement and significance)
- See connections to quantum mechanics

**Prerequisites:** Chapters 4, 8

---

## Motivation

Eigenvalues are the single most important concept in finite-dimensional linear algebra. They tell you how a matrix stretches, rotates, and compresses. They determine stability of dynamical systems, principal components of data, and energy levels of quantum systems. Diagonalizing a matrix — writing it as $PDP^{-1}$ — is the ultimate act of understanding it.

In infinite dimensions, the picture is richer. An operator can fail to be invertible *without* having an eigenvalue. The **spectrum** includes eigenvalues but also new phenomena: **continuous spectrum** (where approximate eigenvectors exist but exact ones don't) and **residual spectrum** (where the range isn't even dense). Understanding these is essential for differential equations, quantum mechanics, and signal processing.

The **spectral theorem** — the infinite-dimensional version of "every symmetric matrix is diagonalizable" — is the capstone of this course. It says that every bounded self-adjoint operator on a Hilbert space is, in a precise sense, secretly a multiplication operator. This is the theorem that makes quantum mechanics mathematically rigorous.

---

## The Spectrum and Resolvent

### Beyond eigenvalues

In finite dimensions, $A - \lambda I$ fails to be invertible iff $\det(A - \lambda I) = 0$, iff $\lambda$ is an eigenvalue. These conditions are equivalent because a square matrix is injective iff it's surjective.

In infinite dimensions, injectivity and surjectivity diverge. An operator $T - \lambda I$ can be injective (no eigenvalue) and yet fail to be invertible — its range might not be all of $X$. This forces a broader concept.

### Definition

Let $X$ be a complex Banach space and $T \in B(X)$. The **spectrum** of $T$ is

$$\sigma(T) = \{\lambda \in \mathbb{C} : T - \lambda I \text{ is not invertible in } B(X)\}.$$

The complement $\rho(T) = \mathbb{C} \setminus \sigma(T)$ is the **resolvent set**, and for $\lambda \in \rho(T)$, the **resolvent operator** is $R(\lambda, T) = (T - \lambda I)^{-1}$.

> **Note:** "Invertible" means a bounded inverse exists. By the inverse mapping theorem (Chapter 7), if $T - \lambda I$ is bijective between Banach spaces, the inverse is automatically bounded. So "invertible" and "bijective" coincide on Banach spaces — a consequence of completeness.

### Three types of spectrum

The spectrum decomposes based on *how* $T - \lambda I$ fails to be invertible:

| Type | Notation | Condition | Finite-dim analogue |
|------|----------|-----------|---------------------|
| **Point spectrum** | $\sigma_p(T)$ | $T - \lambda I$ not injective | Eigenvalues |
| **Continuous spectrum** | $\sigma_c(T)$ | Injective, dense range, not surjective | No analogue |
| **Residual spectrum** | $\sigma_r(T)$ | Injective, range not dense | No analogue |

```
σ(T): T - λI is not invertible
├── σ_p(T): not injective (eigenvalues)
├── σ_c(T): injective, dense range, not surjective
└── σ_r(T): injective, range not dense
```

In finite dimensions, only $\sigma_p$ exists. The continuous and residual spectra are genuinely infinite-dimensional phenomena.

### Example: the right shift

Let $S: \ell^2 \to \ell^2$ be the **right shift**: $S(x_1, x_2, \ldots) = (0, x_1, x_2, \ldots)$.

**Point spectrum:** Suppose $Sx = \lambda x$. Then $(0, x_1, x_2, \ldots) = (\lambda x_1, \lambda x_2, \ldots)$. The first component gives $\lambda x_1 = 0$. If $\lambda \neq 0$, then $x_1 = 0$, forcing $x_2 = 0, \ldots$, so $x = 0$. If $\lambda = 0$, then $Sx = 0$ gives $x = 0$. So $\sigma_p(S) = \emptyset$.

**Full spectrum:** Despite having no eigenvalues, $\sigma(S) = \overline{\mathbb{D}} = \{|\lambda| \leq 1\}$, the closed unit disk. For $|\lambda| < 1$, there are **approximate eigenvectors** — sequences $x^{(n)}$ with $\|x^{(n)}\| = 1$ and $\|(S - \lambda I)x^{(n)}\| \to 0$ — so $S - \lambda I$ can't have a bounded inverse. The open disk is continuous spectrum; the boundary circle is also in the spectrum.

> **Example:** The right shift has spectrum equal to the entire closed unit disk, but *no eigenvalues at all*. In finite dimensions, this is impossible — the spectrum of a matrix consists entirely of eigenvalues. This illustrates that infinite-dimensional spectral theory is genuinely richer.

### Example: multiplication operators

Let $g \in L^\infty[0,1]$ and define $M_g: L^2[0,1] \to L^2[0,1]$ by $(M_g f)(x) = g(x) f(x)$.

The spectrum is: $\sigma(M_g) = \text{ess ran}(g)$, the **essential range** of $g$ — the set of values $\lambda$ such that $\{x : |g(x) - \lambda| < \epsilon\}$ has positive measure for every $\epsilon > 0$.

**Why:** If $\lambda \notin \text{ess ran}(g)$, then $|g(x) - \lambda| \geq \epsilon$ a.e., so $1/(g(x) - \lambda)$ is bounded a.e., and $M_{1/(g-\lambda)}$ is a bounded inverse. If $\lambda \in \text{ess ran}(g)$, approximate eigenvectors are built by concentrating on the set where $g \approx \lambda$.

> **Note:** Multiplication operators are the "model" for spectral theory. The spectral theorem says *every* bounded self-adjoint operator is secretly a multiplication operator. Keep this in mind — it's the destination.

### Structural properties

For $T \in B(X)$ on a complex Banach space:

1. **$\sigma(T) \subset \{|\lambda| \leq \|T\|\}$** — by the Neumann series (Chapter 4), $|\lambda| > \|T\|$ implies $T - \lambda I$ is invertible.
2. **$\sigma(T)$ is closed** — the resolvent set is open (small perturbations of invertible operators are invertible).
3. **$\sigma(T) \neq \emptyset$** — proved using the resolvent as a $B(X)$-valued analytic function and Liouville's theorem.

Combining: **the spectrum is a nonempty compact subset of $\mathbb{C}$**.

> **Note:** Nonemptiness requires the complex scalars. Over $\mathbb{R}$, a rotation matrix has empty real spectrum. Spectral theory is fundamentally a complex-variable story.

---

## The Spectral Radius Formula

### Connecting algebra to analysis

The **spectral radius** is $r(T) = \sup\{|\lambda| : \lambda \in \sigma(T)\}$.

The **Gelfand formula** connects this algebraic quantity to operator norms:

$$\boxed{r(T) = \lim_{n \to \infty} \|T^n\|^{1/n} = \inf_{n \geq 1} \|T^n\|^{1/n}}$$

### Why it matters

The spectral radius controls long-term behavior: if $r(T) < 1$, then $\|T^n\| \to 0$ (the powers die out). If $r(T) > 1$, the powers blow up. This is stability theory for $x_{n+1} = Tx_n$.

### The spectral radius can be strictly less than $\|T\|$

> **Example:** Consider $T = \begin{pmatrix} 0 & 4 \\ 0 & 0 \end{pmatrix}$. Then $\|T\| = 4$, but $T^2 = 0$, so $r(T) = 0$. The norm measures single-step amplification; the spectral radius measures asymptotic growth.
>
> For the right shift $S$ on $\ell^2$: $\|S^n\| = 1$ for all $n$, so $r(S) = 1 = \|S\|$. Here they coincide.

---

## Spectral Theory for Self-Adjoint Operators

### Why self-adjointness changes everything

An operator $T \in B(H)$ on a Hilbert space is **self-adjoint** if $\langle Tx, y \rangle = \langle x, Ty \rangle$ for all $x, y$. This is the infinite-dimensional analogue of a symmetric matrix.

Three simplifications for self-adjoint $T$:

**1. The spectrum is real.** If $\lambda = \alpha + i\beta$ with $\beta \neq 0$, then $\|(T - \lambda I)x\|^2 \geq \beta^2\|x\|^2$ for all $x$ (the cross terms vanish by self-adjointness). So $T - \lambda I$ is bounded below and has dense range, hence is invertible.

**2. No residual spectrum.** If $T - \lambda I$ is injective but its range isn't dense, there exists $y \neq 0$ perpendicular to the range. Then $\langle (T - \lambda I)x, y \rangle = 0$ for all $x$, which gives $(T - \bar{\lambda})y = 0$. Since $\lambda$ is real (by point 1), this means $(T - \lambda)y = 0$, contradicting injectivity.

**3. The spectral theorem applies.**

For self-adjoint operators: $\sigma(T) \subseteq \mathbb{R}$ and $\sigma(T) = \sigma_p(T) \cup \sigma_c(T)$.

### The compact case (Chapter 8 recap)

For compact self-adjoint $T$: eigenvalues $\lambda_n \to 0$, orthonormal eigenvectors, and $T = \sum \lambda_n \langle \cdot, e_n \rangle e_n$. This is "diagonal" representation.

But what about non-compact self-adjoint operators? The multiplication operator $M_x$ on $L^2[0,1]$ ($(M_x f)(t) = t f(t)$) has spectrum $[0,1]$, entirely continuous — no eigenvalues. It can't be diagonalized via eigenvectors. Yet it *is* already a multiplication operator, so it's already as "diagonal" as possible.

### The spectral theorem (general case)

**Spectral Theorem (Multiplication Operator Form).** Let $T = T^* \in B(H)$. There exists a measure space $(\Omega, \mu)$, a bounded measurable function $g: \Omega \to \mathbb{R}$, and a unitary operator $U: H \to L^2(\Omega, \mu)$ such that

$$UTU^{-1} = M_g$$

where $M_g$ is multiplication by $g$. Moreover, $\sigma(T) = \text{ess ran}(g)$.

**Every bounded self-adjoint operator is unitarily equivalent to a multiplication operator.**

```
Finite dimensions:                 Infinite dimensions:

  T = PDP⁻¹                        T = U⁻¹ M_g U

  D = diagonal matrix               M_g = multiplication operator
  P = eigenvector matrix             U = unitary equivalence
  diagonal entries = eigenvalues     g = "eigenvalue function"
  eigenvalues are discrete           spectrum can be continuous
```

### The spectral measure form

Equivalently, there exists a **spectral measure** — a family of projection operators $\{E(\lambda)\}_{\lambda \in \mathbb{R}}$ such that:

$$T = \int_{\sigma(T)} \lambda \, dE(\lambda).$$

For compact self-adjoint $T$ with eigenvalues $\lambda_n$ and eigenprojections $P_n$, this is $T = \sum \lambda_n P_n$ — the spectral measure is discrete. For general self-adjoint $T$, $E(\lambda)$ varies continuously.

---

## Functional Calculus

### Applying functions to operators

If $T = PDP^{-1}$ is a diagonal matrix with eigenvalues $\lambda_1, \ldots, \lambda_n$, then $f(T) = P \text{diag}(f(\lambda_1), \ldots, f(\lambda_n)) P^{-1}$ for any function $f$.

The spectral theorem lets us do the same in infinite dimensions: if $T = U^{-1} M_g U$, define

$$f(T) = U^{-1} M_{f \circ g} U.$$

**Theorem (Continuous Functional Calculus).** For $T = T^* \in B(H)$ and continuous $f: \sigma(T) \to \mathbb{C}$, there exists a unique $f(T) \in B(H)$ satisfying:
- If $f(\lambda) = \lambda$, then $f(T) = T$
- $(fg)(T) = f(T)g(T)$ (multiplicative)
- $\bar{f}(T) = f(T)^*$
- $\|f(T)\| = \sup_{\lambda \in \sigma(T)} |f(\lambda)|$

> **Example:** If $T \geq 0$ (positive self-adjoint), define $\sqrt{T}$ via $f(\lambda) = \sqrt{\lambda}$. Then $(\sqrt{T})^2 = T$, $\sqrt{T}$ is self-adjoint and positive, and $\|\sqrt{T}\| = \sqrt{\|T\|}$. This can't be done algebraically — the functional calculus produces $\sqrt{T}$ effortlessly.
>
> The **polar decomposition** $T = U|T|$, where $|T| = \sqrt{T^*T}$, uses this construction. It's the operator analogue of writing a complex number as $re^{i\theta}$.

### What goes wrong without self-adjointness

For general bounded operators, you can only apply *analytic* functions via the Cauchy integral:

$$f(T) = \frac{1}{2\pi i} \oint_\Gamma f(\lambda)(T - \lambda I)^{-1} d\lambda.$$

This **holomorphic functional calculus** is more restrictive — it requires analyticity on a neighborhood of $\sigma(T)$, not just continuity. The full continuous functional calculus requires self-adjointness (or more generally, normality: $TT^* = T^*T$).

---

## Connection to Quantum Mechanics

### The framework

Quantum mechanics is spectral theory in a Hilbert space:

| Physics | Mathematics |
|---|---|
| State of a system | Unit vector $\psi \in H$ |
| Observable quantity | Self-adjoint operator $T$ |
| Possible measurement outcomes | Spectrum $\sigma(T)$ |
| Probability of outcome in $S$ | $\langle E(S)\psi, \psi \rangle$ |
| Expected value | $\langle T\psi, \psi \rangle$ |

The spectral theorem is what makes this consistent: measurement outcomes are real (spectrum of self-adjoint operators is real), and probabilities sum to 1 (the spectral measure is a probability measure on $\sigma(T)$).

### Position and momentum

For a quantum particle on $\mathbb{R}$:
- **Position operator:** $(Q\psi)(x) = x\psi(x)$ — a multiplication operator. Spectrum: all of $\mathbb{R}$ (continuous spectrum, no eigenvalues).
- **Momentum operator:** $(P\psi)(x) = -i\hbar \psi'(x)$ — under Fourier transform, becomes multiplication by $\xi$. The "eigenbasis" of momentum is the Fourier basis.

> **Note:** Both $Q$ and $P$ are *unbounded* — they're not in $B(H)$. The spectral theorem extends to unbounded self-adjoint operators (with domain considerations), but that is beyond this course. The conceptual point stands: self-adjoint operators represent observables, spectra are measurement outcomes.

### The uncertainty principle

Position and momentum don't commute: $[Q, P] = i\hbar I$. Non-commuting operators can't be simultaneously diagonalized, so there's no state in which both are sharply defined. This is the Heisenberg uncertainty principle — a direct consequence of the non-commutativity, expressed in the language of spectral theory.

---

## Key Takeaways

- **The spectrum generalizes eigenvalues.** $\sigma(T)$ is always a nonempty compact subset of $\mathbb{C}$. It decomposes into point, continuous, and residual spectrum. Finite-dimensional eigenvalue theory is a special case.
- **The spectral radius formula** $r(T) = \lim \|T^n\|^{1/n}$ bridges algebra (spectrum) and analysis (norms). It controls asymptotic behavior of iterates.
- **Self-adjointness simplifies radically.** Spectrum is real, no residual spectrum, and the full spectral theorem applies: every bounded self-adjoint operator is secretly a multiplication operator.
- **The functional calculus** lets you apply continuous functions to self-adjoint operators, producing constructions (like $\sqrt{T}$) that are impossible algebraically.
- **Spectral theory is the language of quantum mechanics.** Observables are self-adjoint operators, measurement outcomes are spectra, probabilities come from spectral measures.

---

## What's Next

We now have all the pieces in place. In **Chapter 10: Synthesis**, we put it all together — revisit the motivating problems from Chapter 1 with the full toolkit, see how the major theorems interconnect, survey the landscape beyond this course, and reflect on the unifying principles of functional analysis.
