# Chapter 8: Compact Operators

**Learning objectives:**
- Understand compact operators as "almost finite-dimensional" maps
- Know the relationship between compact operators and finite-rank approximation
- State and understand the spectral theorem for compact self-adjoint operators
- Apply the Fredholm alternative to integral equations

**Prerequisites:** Chapters 3, 4

---

## Motivation

We've spent Chapters 4–7 building the general theory of bounded operators between Banach spaces: the operator norm, the four big theorems, dual spaces. That theory is powerful but also general — it applies to *all* bounded operators. Now we focus on a special class: **compact operators**.

Compact operators are the bridge between finite and infinite dimensions. They map bounded sets to relatively compact sets — sets whose closure is compact. Since the closed unit ball in an infinite-dimensional space is never compact (recall Chapter 1), compact operators must "compress" the image into something that behaves more like a finite-dimensional set. This "almost finite-dimensional" character means their spectral theory looks remarkably like the eigenvalue theory of matrices.

Why do we care? Because compact operators arise everywhere:
- **Integral operators** with continuous or square-integrable kernels are compact
- The **inverse of an elliptic differential operator** (like the Laplacian with boundary conditions) is compact
- Compact operators are the operators for which the equation $Tx = y$ can be analyzed by the **Fredholm alternative** — a clean dichotomy between existence and uniqueness

This chapter connects the abstract theory of Chapters 4–7 back to the concrete problems of Chapter 1.

---

## What Makes an Operator Compact?

### The definition

Let $X$ and $Y$ be normed spaces. A linear operator $T: X \to Y$ is **compact** if for every bounded sequence $(x_n)$ in $X$, the image sequence $(Tx_n)$ has a convergent subsequence in $Y$.

Equivalently: $T$ maps the closed unit ball $\overline{B}_X$ to a **relatively compact** set in $Y$ — a set whose closure is compact.

### Intuition: compressing infinite-dimensional information

Think of a compact operator as a "dimensional compressor." The input can wander freely in infinite-dimensional space, but the output is squeezed into a set that's "essentially finite-dimensional" — it has compact closure. The infinite-dimensional freedom of the input is tamed in the output.

In finite dimensions, every bounded operator is compact (because bounded closed sets are compact). Compactness is a nontrivial condition only in infinite dimensions — it's the condition that recovers finite-dimensional-like behavior.

### Key examples

**Example 1: Finite-rank operators.** An operator $T: X \to Y$ has **finite rank** if $\dim(\text{range}(T)) < \infty$. Every finite-rank operator is compact: the image is contained in a finite-dimensional subspace, and bounded sets in finite-dimensional spaces have compact closure. Finite-rank operators are the simplest compact operators — they *literally* reduce to finite dimensions.

**Example 2: Integral operators.** Define $T: C[0,1] \to C[0,1]$ by

$$(Tf)(x) = \int_0^1 K(x,t) f(t) \, dt$$

where $K: [0,1]^2 \to \mathbb{R}$ is continuous. This operator is compact. The proof uses the Arzelà-Ascoli theorem: the image of the unit ball consists of equicontinuous, uniformly bounded functions, which have convergent subsequences.

More generally, any integral operator on $L^2$ with a square-integrable kernel $K \in L^2([0,1]^2)$ (a **Hilbert-Schmidt operator**) is compact.

> **Example:** The Volterra operator $(Vf)(x) = \int_0^x f(t) \, dt$ on $L^2[0,1]$ is compact. It's an integral operator with kernel $K(x,t) = \mathbf{1}_{t \leq x}$, which is in $L^2([0,1]^2)$ (since $\int \int \mathbf{1}_{t \leq x}^2 \, dt \, dx = 1/2 < \infty$). The Volterra operator will be a running example throughout this chapter.

### The critical non-example

**The identity operator on an infinite-dimensional space is NOT compact.** Consider $I: \ell^2 \to \ell^2$. The standard basis vectors $e_n = (0, \ldots, 0, 1, 0, \ldots)$ form a bounded sequence ($\|e_n\| = 1$), but $\|e_n - e_m\| = \sqrt{2}$ for $n \neq m$ — no subsequence is Cauchy, let alone convergent.

This is the fundamental reason why compactness is special in infinite dimensions: the identity maps the unit ball to itself, and the unit ball is not compact. A compact operator must distort the unit ball enough to make its image compact.

```
Identity operator (NOT compact):

    e₁ •          • e₂
          \      /
           \    /
            \  /
  Unit ball: ●──── all ||eₙ - eₘ|| = √2, no convergent subsequence
            /  \
           /    \
          /      \
    e₃ •          • e₄


Compact operator T:

    Tx₁ •───• Tx₂
         \ /
    Tx₃ •─●──• Tx₄    ← images cluster, subsequence converges
         / \
    Tx₅ •───• Tx₆
```

### Properties of compact operators

- Every compact operator is bounded (compact sets are bounded). But not vice versa.
- The sum of two compact operators is compact.
- The composition of a compact operator with a bounded operator (in either order) is compact.
- The set of compact operators $K(X, Y)$ is a closed subspace of $B(X, Y)$ — limits of compact operators are compact.

> **Note:** The last property — closure — is crucial. It means you can approximate a compact operator by "simpler" operators and the limit is still compact. This connects to the next section.

---

## Approximation by Finite-Rank Operators

### Compact operators as limits of finite-rank operators

The intuition that compact operators are "almost finite-dimensional" is made precise by the following:

**Theorem.** On a separable Hilbert space $H$, every compact operator $T: H \to H$ is the limit (in operator norm) of finite-rank operators.

*Proof sketch.* Let $\{e_n\}$ be an orthonormal basis for $H$. Define $P_n$ as the orthogonal projection onto $\text{span}\{e_1, \ldots, e_n\}$. Then $P_n T$ is finite-rank (its range is at most $n$-dimensional). We claim $\|P_n T - T\| \to 0$.

If not, there exist $\epsilon > 0$ and unit vectors $x_n$ with $\|(P_n T - T)x_n\| \geq \epsilon$. Since $T$ is compact, $(Tx_n)$ has a convergent subsequence $Tx_{n_k} \to y$. Then $P_{n_k} y \to y$ (since $P_n \to I$ pointwise), and $\|(P_{n_k} T - T)x_{n_k}\| \leq \|P_{n_k}(Tx_{n_k} - y)\| + \|P_{n_k} y - y\| + \|y - Tx_{n_k}\| \to 0$. Contradiction.

### Why this matters

The approximation theorem has practical consequences:

1. **Numerical computation.** To compute with a compact operator, approximate it by a finite-rank operator (a matrix). The approximation converges in operator norm, so the error is controlled. This is the mathematical foundation of Galerkin methods and other discretization techniques for integral equations.

2. **Theoretical tool.** Many properties of compact operators can be proved by first proving them for finite-rank operators (which is essentially linear algebra) and then taking limits.

> **Example:** The Volterra operator $V$ on $L^2[0,1]$ can be approximated by the finite-rank operators $V_n = P_n V$, where $P_n$ projects onto the first $n$ Fourier modes (or onto piecewise constant functions on $n$ intervals). Each $V_n$ is represented by a matrix — a discretization of the integral — and $\|V_n - V\| \to 0$.

---

## The Spectral Theorem for Compact Self-Adjoint Operators

### Why this is special

In Chapter 9, we'll tackle spectral theory for general bounded operators — a rich subject. But for compact self-adjoint operators, the spectral picture is dramatically simpler: it looks almost exactly like finite-dimensional eigenvalue theory. This is the reward for working with compact operators.

### Statement

**Theorem (Spectral Theorem for Compact Self-Adjoint Operators).** Let $H$ be a Hilbert space and $T: H \to H$ a compact self-adjoint operator. Then:

1. The eigenvalues of $T$ are real and form a sequence $(\lambda_n)$ with $\lambda_n \to 0$ (or are finite in number).
2. Eigenvectors for distinct eigenvalues are orthogonal.
3. $H$ has an orthonormal basis consisting of eigenvectors of $T$.
4. $T$ can be written as:

$$Tx = \sum_{n} \lambda_n \langle x, e_n \rangle \, e_n$$

where $\{e_n\}$ are the orthonormal eigenvectors and $\lambda_n$ the corresponding eigenvalues.

### Comparison with finite dimensions

| Property | Matrices ($n \times n$ symmetric) | Compact self-adjoint on $H$ |
|---|---|---|
| Eigenvalues | $n$ real eigenvalues | Countably many, real, $\to 0$ |
| Eigenvectors | Orthonormal basis of $\mathbb{R}^n$ | Orthonormal basis of $H$ |
| Diagonalization | $T = PDP^T$ | $T = \sum \lambda_n \langle \cdot, e_n \rangle e_n$ |
| Nonzero eigenvalues | At most $n$ | At most countably many |

The only new feature in infinite dimensions is that the eigenvalues must accumulate at $0$. This is forced by compactness: if there were infinitely many eigenvalues bounded away from zero, the corresponding eigenvectors (orthonormal, hence at distance $\sqrt{2}$ from each other) would give a bounded sequence with no convergent subsequence — contradicting compactness.

### Worked example: an integral operator

Consider the operator $T: L^2[0, \pi] \to L^2[0, \pi]$ defined by

$$(Tf)(x) = \int_0^\pi K(x,t) f(t) \, dt \quad \text{where } K(x,t) = \min(x, t).$$

This kernel defines the Green's function for $-u'' = f$ on $[0, \pi]$ with $u(0) = u(\pi) = 0$ — exactly the differential equation from Chapter 1. The operator $T$ is compact (continuous kernel on a bounded domain) and self-adjoint ($K(x,t) = K(t,x)$).

**Finding eigenvalues:** $Tf = \lambda f$ means $\int_0^\pi \min(x,t) f(t) \, dt = \lambda f(x)$. Differentiating twice: $-f''(x) = \frac{1}{\lambda} f(x)$, with boundary conditions $f(0) = f(\pi) = 0$.

The eigenfunctions are $e_n(x) = \sqrt{2/\pi} \sin(nx)$ with eigenvalues $\lambda_n = 1/n^2$.

Check: $\lambda_n = 1/n^2 \to 0$ as $n \to \infty$ ✓. The eigenfunctions form an ONB for $L^2[0,\pi]$ ✓.

> **Note:** The eigenvalues $\lambda_n = 1/n^2$ of the Green's function are the *reciprocals* of the eigenvalues $n^2$ of the differential operator $-d^2/dx^2$. Compact operators have eigenvalues that go to zero; their "inverses" (the original differential operators, which are unbounded) have eigenvalues that go to infinity. Compactness and unboundedness are two sides of the same coin.

### Proof sketch: why eigenvalues exist

The key step is showing that $T$ has at least one nonzero eigenvalue. The argument: if $T \neq 0$, then either $\|T\| = \sup_{\|x\|=1} \langle Tx, x \rangle$ or $-\|T\| = \inf_{\|x\|=1} \langle Tx, x \rangle$ (using self-adjointness). Say $\|T\|$ is achieved or approached. By compactness, a maximizing sequence has a convergent subsequence, and the limit is an eigenvector with eigenvalue $\pm\|T\|$.

Once you have one eigenvalue, restrict $T$ to the orthogonal complement of the eigenspace (which is invariant since $T$ is self-adjoint) and repeat. By induction or Zorn's lemma, you get all eigenvalues.

---

## The Fredholm Alternative

### Motivation: solving equations with compact operators

The equation $(I - T)x = y$, where $T$ is compact, is the prototype for integral equations of the second kind:

$$f(x) - \lambda \int K(x,t) f(t) \, dt = g(x).$$

The **Fredholm alternative** gives a clean dichotomy for these equations — clean enough to fully settle existence and uniqueness.

### Statement

**Theorem (Fredholm Alternative).** Let $T: X \to X$ be a compact operator on a Banach space $X$, and consider $I - T$. Then exactly one of the following holds:

**(a)** $\ker(I - T) = \{0\}$ — the homogeneous equation $(I-T)x = 0$ has only the trivial solution. In this case, $(I-T)$ is bijective, and for every $y \in X$, the equation $(I-T)x = y$ has a unique solution.

**(b)** $\ker(I - T) \neq \{0\}$ — the homogeneous equation has nontrivial solutions. In this case, the equation $(I-T)x = y$ is solvable if and only if $y$ is orthogonal (in an appropriate sense) to $\ker(I - T^*)$.

**There is no middle ground:** either the equation is always uniquely solvable, or the kernel is nontrivial and solvability depends on a finite-dimensional compatibility condition.

### Why this matters

In finite dimensions, this is obvious: a square matrix is either invertible or singular, and in the singular case, the equation $Ax = b$ is solvable iff $b$ is in the column space. The Fredholm alternative says *the exact same dichotomy holds* when you replace the matrix with $I - T$ where $T$ is compact. The "almost finite-dimensional" nature of compact operators preserves this clean structure.

Without compactness, the dichotomy can fail: there exist bounded operators $T$ where $I - T$ is injective but not surjective, or surjective but not injective with infinite-dimensional kernel. Compactness rules out these intermediate cases.

### Worked example: a Fredholm integral equation

Consider:

$$f(x) - \int_0^1 e^{x+t} f(t) \, dt = g(x).$$

This is $(I - T)f = g$ where $(Tf)(x) = \int_0^1 e^{x+t} f(t) \, dt$.

The kernel $K(x,t) = e^{x+t} = e^x e^t$ has rank 1: $Tf = e^x \int_0^1 e^t f(t) \, dt = e^x \langle f, e^t \rangle_{L^2}$. So $Tf = c \cdot e^x$ where $c = \int_0^1 e^t f(t) \, dt$.

**Homogeneous equation:** $(I - T)f = 0$ means $f(x) = c \cdot e^x$ with $c = \int_0^1 e^t \cdot c \cdot e^t \, dt = c \int_0^1 e^{2t} \, dt = c \cdot \frac{e^2 - 1}{2}$.

So either $c = 0$ (giving $f = 0$) or $\frac{e^2 - 1}{2} = 1$. Since $\frac{e^2 - 1}{2} \approx 3.19 \neq 1$, the only solution is $c = 0$, $f = 0$.

By the Fredholm alternative (case (a)): for every $g \in C[0,1]$, the equation has a unique solution. The solution is:

$$f(x) = g(x) + \frac{\int_0^1 e^t g(t) \, dt}{1 - \frac{e^2-1}{2}} \cdot e^x.$$

> **Verification:** You can check by substituting this $f$ back into the integral equation. The Fredholm alternative told us a unique solution must exist; the rank-1 structure of $T$ lets us find it explicitly.

---

## Key Takeaways

- **Compact operators are "almost finite-dimensional."** They map bounded sets to relatively compact sets, meaning their images can be approximated by finite-dimensional objects. Every compact operator on a separable Hilbert space is a limit of finite-rank operators.
- **The identity is the dividing line.** On an infinite-dimensional space, the identity operator is not compact. Compactness requires genuine "compression" of the image.
- **The spectral theorem for compact self-adjoint operators** recovers the full diagonalization picture from finite-dimensional linear algebra: real eigenvalues converging to 0, orthonormal eigenvectors, and a diagonal expansion $T = \sum \lambda_n \langle \cdot, e_n \rangle e_n$.
- **The Fredholm alternative** gives a clean existence/uniqueness dichotomy for equations $(I - T)x = y$ with compact $T$: either unique solvability for all $y$, or solvability iff $y$ satisfies a finite-dimensional compatibility condition. No middle ground.
- **Compact operators arise naturally** from integral equations, Green's functions, and inverses of differential operators. They connect the abstract theory of Chapters 4–7 to the motivating problems of Chapter 1.

---

## What's Next

The spectral theorem for compact self-adjoint operators gave us eigenvalues and eigenvectors — familiar terrain from linear algebra. But what about operators that aren't compact? In **Chapter 9: Spectral Theory**, we'll see that the notion of "eigenvalue" must be generalized to the **spectrum** — a richer concept that includes eigenvalues but also "continuous spectrum" and "residual spectrum." We'll prove the spectral theorem for general bounded self-adjoint operators, introduce the functional calculus, and see how this machinery connects to quantum mechanics.
