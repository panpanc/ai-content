# Everything Is a Function in Higher Math

## Stage 0: The Suspicion — Why Does Everything Look Like a Function?

### Motivation

You start learning math and you encounter separate-looking objects: numbers, sets, logical propositions, geometric shapes, data types. Each has its own notation, its own rules, its own textbook chapter. But as you go deeper, a strange pattern emerges:

- A number $n \in \mathbb{N}$ can be viewed as a function $\mathbf{n} \to \Omega$ (a set with $n$ elements) or as a function $1 \to \mathbb{N}$ (picking an element).
- A subset $A \subseteq X$ is the same thing as a function $\chi_A : X \to \{0, 1\}$.
- A sequence $a_0, a_1, a_2, \ldots$ is literally a function $a : \mathbb{N} \to \mathbb{R}$.
- A probability distribution on a finite set is a function $p : X \to [0,1]$ with $\sum p(x) = 1$.
- A matrix is a function $M : \{1,\ldots,m\} \times \{1,\ldots,n\} \to \mathbb{R}$.

Is this a coincidence? A notational trick? Or is there something deeper going on — a reason why the function concept keeps swallowing everything else?

### Intuition

Think of functions as the **universal adapter**. Just as USB-C replaced a zoo of proprietary connectors, the function concept provides a single interface for relating mathematical objects. Instead of inventing a new type of relationship for each pair of mathematical structures, you ask: "Can I express this as a function?" Almost always, yes — and once you do, you inherit an enormous toolkit (composition, inverses, fixed points, universal properties) for free.

### The claim we'll investigate

The thesis of this document: **in higher mathematics, essentially every mathematical object can be productively viewed as a function (or a morphism in a category, which generalizes functions).** This isn't just an encoding trick — it's a shift in perspective that unifies disparate areas and reveals hidden structure.

---

## Stage 1: The Familiar Cases — Things You Already Know Are Functions

### Motivation

Before we get to the surprising cases, let's catalog the obvious ones to build the pattern.

### Sequences are functions

A sequence $a_0, a_1, a_2, \ldots$ in a set $S$ is a function $a : \mathbb{N} \to S$. This isn't a metaphor — it's the definition. When you write $a_n$, you're evaluating $a$ at $n$.

Why does this matter? Because "sequence" inherits all function concepts:
- A subsequence is a composition $a \circ \phi$ where $\phi : \mathbb{N} \to \mathbb{N}$ is strictly increasing.
- A sequence converges iff the function has a limit.
- You can ask whether the function is injective (no repeated terms), surjective (every element appears), etc.

### Vectors are functions

A vector $\mathbf{v} \in \mathbb{R}^n$ is a function $v : \{1, \ldots, n\} \to \mathbb{R}$. The $i$-th component $v_i$ is the value $v(i)$.

An infinite-dimensional vector in $\ell^2$ is a function $v : \mathbb{N} \to \mathbb{R}$ with $\sum |v(n)|^2 < \infty$.

A function $f : [0,1] \to \mathbb{R}$ in $L^2([0,1])$ is a "continuous-index vector" — a vector with uncountably many components, indexed by points in $[0,1]$ instead of $\{1, \ldots, n\}$.

```
Finite vector:     v : {1, 2, 3} → ℝ          [3.0, -1.5, 2.7]
Sequence:          v : ℕ → ℝ                   1, 1/2, 1/3, 1/4, ...
Function:          v : [0,1] → ℝ               f(x) = sin(πx)
                   ↑
           All the same pattern: domain → ℝ
```

### Matrices are functions of two arguments

An $m \times n$ matrix is a function $M : \{1,\ldots,m\} \times \{1,\ldots,n\} \to \mathbb{R}$. Entry $M_{ij}$ is $M(i, j)$.

But a matrix also *represents* a linear function $T : \mathbb{R}^n \to \mathbb{R}^m$. So a matrix is a function that encodes a function. Functions all the way down.

### Common misconception

> "Viewing a vector as a function is just a pedantic re-encoding — it doesn't help you do anything."

Wrong. This viewpoint is precisely what makes the jump to infinite dimensions possible. You can't draw an infinite-dimensional vector as an arrow. But you can define it as a function from an index set to $\mathbb{R}$, and suddenly $\ell^2$, $L^2$, Hilbert spaces, and functional analysis all make sense. The "encoding" is the bridge.

---

## Stage 2: Sets as Functions — The Characteristic Function Trick

### Motivation

Sets feel fundamentally different from functions. A set just *is* — it's a collection of objects. A function *does something* — it maps inputs to outputs. But the boundary dissolves completely once you see the trick.

### Subsets as indicator functions

Given a universal set $X$, every subset $A \subseteq X$ corresponds to a unique function:

$$\chi_A : X \to \{0, 1\}, \quad \chi_A(x) = \begin{cases} 1 & \text{if } x \in A \\ 0 & \text{if } x \notin A \end{cases}$$

And conversely, every function $f : X \to \{0, 1\}$ defines a unique subset $\{x \in X : f(x) = 1\}$.

This is a **bijection**: $\mathcal{P}(X) \cong \{0,1\}^X$, where $\{0,1\}^X$ denotes the set of all functions from $X$ to $\{0,1\}$.

### Concrete trace

Let $X = \{a, b, c, d\}$ and $A = \{a, c\}$.

```
x:       a    b    c    d
χ_A(x):  1    0    1    0
```

Now set operations become arithmetic on functions:

| Set operation | Function operation | Formula |
|---|---|---|
| $A \cup B$ | pointwise max | $\chi_{A \cup B}(x) = \max(\chi_A(x), \chi_B(x))$ |
| $A \cap B$ | pointwise product | $\chi_{A \cap B}(x) = \chi_A(x) \cdot \chi_B(x)$ |
| $A^c$ | complement | $\chi_{A^c}(x) = 1 - \chi_A(x)$ |
| $A \subseteq B$ | pointwise $\leq$ | $\chi_A(x) \leq \chi_B(x)$ for all $x$ |

### Why this matters

Once sets become functions, **set theory becomes a special case of function theory**. The power set $\mathcal{P}(X)$ is a function space. Cardinality arguments ($|\mathcal{P}(X)| = 2^{|X|}$) become statements about the size of function spaces. Cantor's diagonal argument — arguably the most important proof technique in foundations — is a statement about functions from $X$ to $\{0,1\}$.

### Intuition

Think of a subset as an **access list**. For each element, the function answers yes or no. The set *is* its access list. There's no information in the set that isn't captured by the function, and vice versa.

### The programmer's version

```python
# A "set" is just a predicate (function returning bool)
evens = lambda x: x % 2 == 0
primes = lambda x: x > 1 and all(x % d != 0 for d in range(2, x))

# Intersection is just "and"
even_primes = lambda x: evens(x) and primes(x)

print(even_primes(2))   # True  — the only even prime
print(even_primes(3))   # False
print(even_primes(4))   # False
```

### Common misconception

> "A set is a container that holds objects."

This "bag of objects" metaphor fails for infinite sets (you can't literally hold infinitely many things) and for sets defined by properties ("the set of all reals satisfying $x^2 < 2$"). The function view handles both naturally — the characteristic function just answers the membership question, regardless of whether the set is finite or infinite.

---

## Stage 3: Numbers as Functions — Points, Picks, and Iterations

### Motivation

Surely numbers aren't functions? A number like $3$ just... is. But in several precise senses, numbers are naturally viewed as functions.

### An element as a function from a one-point set

In category theory, picking an element $x \in X$ is the same as giving a function $x : 1 \to X$, where $1 = \{*\}$ is a one-element set. The function sends $*$ to $x$.

```
1 = {*} ---x---> X
  *  ↦  x
```

This looks trivial, but it's deeply useful: it means "elements" and "morphisms" are the same kind of thing. An element of $X$ is a morphism from the terminal object to $X$. This idea generalizes to any category — in a group $G$, "elements" are homomorphisms $\mathbb{Z} \to G$; in a topological space $X$, "points" are continuous maps $\{*\} \to X$.

### Natural numbers as functions (the von Neumann construction)

In set theory, the natural numbers are built from the empty set using functions:

```
0 = ∅                    (the empty set)
1 = {∅} = {0}            (the set containing 0)
2 = {∅, {∅}} = {0, 1}    (the set containing 0 and 1)
3 = {0, 1, 2}
n = {0, 1, ..., n-1}
```

Each natural number $n$ *is* a set with $n$ elements. And the successor function $S(n) = n \cup \{n\}$ is the engine that builds the whole number line. The natural numbers are the free algebra generated by $0$ and $S$ — that is, they're characterized by the functions on them.

### Church numerals: numbers AS iteration

In lambda calculus, a number $n$ is a function that composes another function $n$ times:

$$\mathbf{0} = \lambda f. \lambda x. \, x$$
$$\mathbf{1} = \lambda f. \lambda x. \, f(x)$$
$$\mathbf{2} = \lambda f. \lambda x. \, f(f(x))$$
$$\mathbf{3} = \lambda f. \lambda x. \, f(f(f(x)))$$

The number $n$ is "apply $f$ exactly $n$ times." Addition is function composition in disguise:

$$\text{add} = \lambda m. \lambda n. \lambda f. \lambda x. \, m\,f\,(n\,f\,x)$$

```python
# Church numerals in Python
zero  = lambda f: lambda x: x
one   = lambda f: lambda x: f(x)
two   = lambda f: lambda x: f(f(x))
three = lambda f: lambda x: f(f(f(x)))

# Convert to int: apply the numeral to (+1) starting from 0
to_int = lambda n: n(lambda x: x + 1)(0)

print(to_int(zero))   # 0
print(to_int(three))  # 3

# Addition
add = lambda m: lambda n: lambda f: lambda x: m(f)(n(f)(x))
five = add(two)(three)
print(to_int(five))   # 5

# Multiplication
mult = lambda m: lambda n: lambda f: m(n(f))
six = mult(two)(three)
print(to_int(six))    # 6
```

### The insight

Numbers don't have to be primitive objects. They can be *derived* from functions. In lambda calculus, there's literally nothing except functions — no numbers, no booleans, no data structures — and yet you can build all of computation.

---

## Stage 4: Logic as Functions — Propositions and Proofs

### Motivation

Logic feels even further from functions than numbers do. "It's raining" is a proposition — what could it mean for it to be a function? Two answers, at two levels of sophistication.

### Propositions as functions to truth values

A predicate like "is even" on the natural numbers is a function:

$$\text{even} : \mathbb{N} \to \{\text{true}, \text{false}\}$$

A proposition with no free variables (like "7 is prime") is a function from the one-point set: $1 \to \{\text{true}, \text{false}\}$ — i.e., just a truth value.

Logical connectives become operations on functions:

| Logic | Functions |
|---|---|
| $P \land Q$ | true iff both $P(x)$ and $Q(x)$ are true |
| $P \lor Q$ | true iff at least one is true |
| $P \Rightarrow Q$ | true iff $P(x) = F$ or $Q(x) = T$ |
| $\neg P$ | $1 - P$ (complement) |
| $\forall x.\, P(x)$ | $P$ is the constant function $\text{true}$ |
| $\exists x.\, P(x)$ | $P$ hits $\text{true}$ at least once |

This is the foundation of model theory and database query languages (SQL's `WHERE` clause is a predicate — a function from rows to booleans).

### Propositions as types, proofs as functions (Curry-Howard)

There's a much deeper connection. The **Curry-Howard correspondence** says:

| Logic | Type theory / Programming |
|---|---|
| Proposition $A$ | Type $A$ |
| Proof of $A$ | Term (value) of type $A$ |
| $A \Rightarrow B$ | Function type $A \to B$ |
| $A \land B$ | Product type $(A, B)$ |
| $A \lor B$ | Sum type $A + B$ |
| $\forall x : T.\, P(x)$ | Dependent function $\prod_{x:T} P(x)$ |
| $\exists x : T.\, P(x)$ | Dependent pair $\sum_{x:T} P(x)$ |

Under this correspondence, **a proof of "A implies B" is a function from proofs of A to proofs of B**. Proving an implication is literally writing a function.

### Concrete example

**Proposition**: If $n$ is even, then $n + 2$ is even.

**Proof as function**: Given evidence that $n$ is even (a witness $k$ such that $n = 2k$), produce evidence that $n + 2$ is even (a witness $k'$ such that $n + 2 = 2k'$). The function: $k \mapsto k + 1$.

```typescript
// "Even" is witnessed by a number k such that n = 2k
type EvenWitness = { half: number };

// The proof is a function
function evenPlusTwoIsEven(n: number, witness: EvenWitness): EvenWitness {
  // n = 2 * witness.half, so n + 2 = 2 * (witness.half + 1)
  return { half: witness.half + 1 };
}
```

### What goes wrong without this?

Without the Curry-Howard perspective, logic and computation are separate disciplines. Logicians prove theorems; programmers write code. But with this correspondence, a type-checking compiler is a **proof checker**, a well-typed program is a **constructive proof**, and an optimizing compiler is a **proof simplifier**. Languages like Coq, Lean, and Agda exploit this directly: you write proofs as programs and the type checker verifies correctness.

### Church booleans

Even classical true/false logic can be encoded purely as functions:

$$\text{true} = \lambda a. \lambda b.\, a \quad \text{("pick the first")}$$
$$\text{false} = \lambda a. \lambda b.\, b \quad \text{("pick the second")}$$

Verify: $\text{and}(\text{true}, \text{false})$:

```
and(p, q) = p(q)(false)

and(true, false)
= true(false)(false)
= (λa.λb. a)(false)(false)
= false  ✓
```

A boolean IS a choice function. There's no separate notion of "truth" — just picking.

### Common misconception

> "Curry-Howard is just a cute analogy between logic and programming."

It's not an analogy — it's an isomorphism. Proof assistants are literally programming languages where writing a program *is* constructing a proof. The correspondence is the foundation of formal verification.

---

## Stage 5: Relations as Functions — From Pairs to Morphisms

### Motivation

A relation $R \subseteq X \times Y$ looks fundamentally different from a function: a function maps each input to exactly one output, while a relation can map to zero, one, or many. But the function perspective absorbs relations too.

### Relations as functions to $\{0, 1\}$

A relation $R \subseteq X \times Y$ is the same data as a function:

$$\chi_R : X \times Y \to \{0, 1\}$$

This is just the characteristic function trick (Stage 2) applied to $X \times Y$. "Is $(x, y) \in R$?" becomes "what is $\chi_R(x, y)$?"

### Relations as set-valued functions (currying)

Alternatively, a relation $R \subseteq X \times Y$ defines a function:

$$R^* : X \to \mathcal{P}(Y), \quad R^*(x) = \{y \in Y : (x, y) \in R\}$$

Each $x$ maps to its set of related elements. A function is the special case where each $R^*(x)$ is a singleton.

### Concrete example

Let $X = \{1, 2, 3\}$, $Y = \{a, b, c\}$, and $R = \{(1,a), (1,b), (2,b), (3,c)\}$.

As $\chi_R$ (a matrix):

```
       a   b   c
  1  [ 1   1   0 ]
  2  [ 0   1   0 ]
  3  [ 0   0   1 ]
```

As set-valued function:

```
R*(1) = {a, b}
R*(2) = {b}
R*(3) = {c}
```

### The currying isomorphism

This decomposition is the **currying** principle:

$$(A \times B \to C) \;\cong\; (A \to (B \to C))$$

A function of two arguments is "the same" as a function of one argument that returns a function. Named after Haskell Curry (though Schönfinkel discovered it first).

```python
# Uncurried: takes a pair
def add_uncurried(pair):
    x, y = pair
    return x + y

# Curried: takes one arg, returns a function
def add_curried(x):
    return lambda y: x + y

add_uncurried((3, 4))    # 7
add_curried(3)(4)         # 7
```

Currying means we never *need* multi-argument functions or relations. Everything reduces to single-argument functions that may return other functions. This is why lambda calculus — which only has single-argument functions — can express all of computation.

### Equivalence relations as quotient maps

An equivalence relation $\sim$ on $X$ defines a **surjection** (quotient map):

$$q : X \to X/{\sim}, \quad q(x) = [x]$$

where $[x]$ is the equivalence class of $x$. The equivalence relation *is* the surjection. Partitions, equivalence relations, and surjections are three views of the same function.

---

## Stage 6: Spaces as Function Algebras — Geometry Becomes Algebra

### Motivation

Geometry is about shapes, distances, and topology. Algebra is about operations on symbols. These feel like different worlds. But one of the deepest ideas in modern mathematics is: **a space is determined by its algebra of functions.**

### The idea

Instead of studying a space $X$ directly, study the functions $X \to \mathbb{R}$ (or $X \to \mathbb{C}$). The collection of all such functions carries enough information to reconstruct $X$.

| Space | Functions on it | Algebra |
|---|---|---|
| Finite set $\{1,\ldots,n\}$ | $f : \{1,\ldots,n\} \to \mathbb{R}$ | $\mathbb{R}^n$ (vectors) |
| Compact Hausdorff space $X$ | $C(X)$ = continuous $f : X \to \mathbb{R}$ | Commutative C*-algebra |
| Smooth manifold $M$ | $C^\infty(M)$ = smooth functions | Commutative $\mathbb{R}$-algebra |
| Affine variety $V$ | Polynomial functions on $V$ | Coordinate ring $k[V]$ |

### The Gelfand-Naimark theorem

Every commutative C*-algebra $A$ is isomorphic to $C(X)$ for some compact Hausdorff space $X$, and $X$ can be recovered as the space of characters (algebra homomorphisms $A \to \mathbb{C}$).

In plain language: **compact spaces and commutative C*-algebras are the same thing, viewed from different angles.** The space is determined by its functions, and the functions are determined by the space.

### Points are evaluation functionals

Here's the punchline. A point $x \in X$ defines an evaluation map:

$$\text{ev}_x : C(X) \to \mathbb{R}, \quad \text{ev}_x(f) = f(x)$$

This map is a function on functions. And by Gelfand-Naimark, the points of $X$ are exactly the (nonzero) algebra homomorphisms $C(X) \to \mathbb{R}$.

**Points are functions too.** Everything is a function.

```
                   X ←———————→ {algebra homs C(X) → ℝ}
                   ↕                    ↕
           "a space"          "evaluation functionals"
              ↕                        ↕
       geometry/topology        algebra of functions
```

### Concrete example

Take $X = \{a, b, c\}$ (discrete topology). Then $C(X) = \mathbb{R}^3$ (a function on three points is just three real numbers).

```
f = (f(a), f(b), f(c)) = (2, 5, 3)   -- a function on X

ev_a(f) = f(a) = 2
ev_b(f) = f(b) = 5
ev_c(f) = f(c) = 3
```

The three evaluation maps $\text{ev}_a$, $\text{ev}_b$, $\text{ev}_c$ are *distinct* linear functionals on $\mathbb{R}^3$. They perfectly separate the points — they **are** the points.

### Why this matters in practice

This duality is the foundation of:
- **Algebraic geometry**: Study varieties through their coordinate rings
- **Noncommutative geometry** (Connes): Drop the commutativity requirement — a noncommutative C*-algebra is a "space" with no underlying point set, yet it still has geometry
- **Functional programming**: A data structure is characterized by its observations (accessor functions), not its internal representation

### Common misconception

> "Studying functions on a space is just one technique among many."

In many areas, it's not *a* technique — it's *the* technique. Algebraic geometers literally define spaces as rings (of functions). Scheme theory, the foundation of modern algebraic geometry, defines a "space" as a locally ringed space — a topological space equipped with a sheaf of function rings. The functions *are* the geometry.

---

## Stage 7: Categories — Where "Function" Gets Its Final Generalization

### Motivation

We've seen that numbers, sets, logical propositions, relations, and spaces can all be viewed as functions. But we've been using "function" loosely — sometimes meaning a set-theoretic map, sometimes a homomorphism, sometimes an evaluation. Category theory provides the umbrella.

### The definition

A **category** $\mathcal{C}$ consists of:
- **Objects**: $A, B, C, \ldots$ (the "things")
- **Morphisms**: For each pair $(A, B)$, a collection $\text{Hom}(A, B)$ of arrows $f : A \to B$ (the "functions between things")
- **Composition**: $g \circ f$ whenever the types match
- **Identity**: $\text{id}_A : A \to A$ for each object

That's it. The morphisms are the generalized functions.

### Examples

| Category | Objects | Morphisms |
|---|---|---|
| $\mathbf{Set}$ | Sets | Functions |
| $\mathbf{Grp}$ | Groups | Group homomorphisms |
| $\mathbf{Top}$ | Topological spaces | Continuous maps |
| $\mathbf{Vect}$ | Vector spaces | Linear maps |
| $\mathbf{Poset}$ | Elements of a poset | $a \to b$ iff $a \leq b$ |
| Any group $G$ | Single object $*$ | Elements of $G$ (composition = group operation) |

### The key insight: structure is in the morphisms

In category theory, you **never look inside objects**. You only look at morphisms (functions) between them. An object is completely determined by its relationships to other objects.

This is the ultimate "everything is a function" statement: even objects are determined by their morphisms.

### Universal properties — defining objects by their functions

The product $A \times B$ in a category is defined not by what it "is" but by what it "does" — by a function property:

> $A \times B$ is an object with projections $\pi_1 : A \times B \to A$ and $\pi_2 : A \times B \to B$, such that for any object $C$ with maps $f : C \to A$ and $g : C \to B$, there exists a unique morphism $\langle f, g \rangle : C \to A \times B$ making the diagram commute.

```
         C
        /|\
       / | \
    f /  |  \ g
     /   |   \
    ↓    ↓    ↓
    A ← A×B → B
       π₁   π₂
```

The product is defined entirely in terms of morphisms. No mention of "ordered pairs" or "elements." Any object that satisfies this function-property *is* a product, regardless of its internal structure.

### Functors and natural transformations: functions all the way up

A **functor** $F : \mathcal{C} \to \mathcal{D}$ maps objects to objects and morphisms to morphisms, preserving composition and identities. It's a "function between worlds of functions."

A **natural transformation** $\alpha : F \Rightarrow G$ between functors is a family of morphisms $\alpha_A : F(A) \to G(A)$ for each object $A$, satisfying a compatibility condition. Functions between functions between functions.

```
Functions → Functors → Natural transformations → ...
(morphisms between objects) → (morphisms between categories) → (morphisms between functors)
```

It's functions all the way up.

---

## Stage 8: The Yoneda Lemma — The Deepest "Everything Is a Function"

### Motivation

If the thesis "everything is a function" has a mathematical proof, the Yoneda lemma is it. It says: **an object is completely determined by the functions into it (or out of it).**

### Setup

Fix a category $\mathcal{C}$ and an object $A$. Define the **representable functor**:

$$h^A = \text{Hom}(A, -) : \mathcal{C} \to \mathbf{Set}$$

This sends each object $B$ to the set of morphisms $A \to B$ (all the functions *out of* $A$). It packages "everything $A$ can map to" into a single functor.

### The Yoneda lemma

For any functor $F : \mathcal{C} \to \mathbf{Set}$:

$$\text{Nat}(h^A, F) \cong F(A)$$

Natural transformations from $h^A$ to $F$ are in bijection with elements of $F(A)$.

### The Yoneda embedding (the punchline)

Taking $F = h^B$ (another representable functor):

$$\text{Nat}(h^A, h^B) \cong h^B(A) = \text{Hom}(A, B)$$

Natural transformations between representable functors are the same as morphisms. The functor:

$$y : \mathcal{C} \to [\mathcal{C}^{op}, \mathbf{Set}], \quad A \mapsto h^A$$

is **fully faithful**. This means: $\mathcal{C}$ embeds into a category of functors (which are functions) without losing any information. Every object becomes a function (its representable functor), and no structure is lost.

### What this means in plain language

**You can replace any mathematical object with "the collection of all functions out of it" and lose nothing.** The object *is* its functions. This is the mathematical justification for the "everything is a function" philosophy.

### Concrete example: Yoneda for a poset

Let $\mathcal{C}$ be the poset $(\{1, 2, 3\}, \leq)$, viewed as a category. For each element $a$, $h^a(b) = \text{Hom}(a, b)$ is either a singleton (if $a \leq b$) or empty (if $a \not\leq b$).

```
h¹(1) = {*}    h¹(2) = {*}    h¹(3) = {*}      (1 ≤ everything)
h²(1) = ∅      h²(2) = {*}    h²(3) = {*}      (2 ≤ 2, 3 only)
h³(1) = ∅      h³(2) = ∅      h³(3) = {*}      (3 ≤ 3 only)
```

Each element is determined by the pattern of "what it can reach" — its representable functor is like a fingerprint. No two elements have the same fingerprint (that's full faithfulness).

### Intuition

Think of it like an API: you don't need to know the internal state of an object if you can observe all of its behaviors (function calls). The Yoneda lemma says the behaviors determine the object completely.

---

## Stage 9: Lambda Calculus — A Universe of Pure Functions

### Motivation

We've seen that many mathematical objects can be *viewed* as functions. Lambda calculus goes further: it's a formal system where **literally everything is a function.** There are no primitive data types, no built-in numbers, no booleans. Just functions applied to functions.

### The syntax

Every expression in lambda calculus is one of three things:

1. **Variable**: $x$
2. **Abstraction** (define a function): $\lambda x.\, M$ — "the function that takes $x$ and returns $M$"
3. **Application** (call a function): $M\, N$ — "apply $M$ to $N$"

That's it. Three forms. Everything else is built from these.

### Church encoding: building the world from functions

**Booleans** (a boolean IS a choice function):

$$\text{true} = \lambda x. \lambda y.\, x \quad \text{(pick the first)}$$
$$\text{false} = \lambda x. \lambda y.\, y \quad \text{(pick the second)}$$

**Pairs** (a pair IS a function awaiting a selector):

$$\text{pair} = \lambda a. \lambda b. \lambda f.\, f\, a\, b$$
$$\text{fst} = \lambda p.\, p\, \text{true}$$
$$\text{snd} = \lambda p.\, p\, \text{false}$$

**Natural numbers** (a number IS iteration — from Stage 3):

$$\mathbf{n} = \lambda f. \lambda x.\, f^n(x)$$

**Lists** (a list IS its own fold function):

$$\text{nil} = \lambda c. \lambda n.\, n$$
$$\text{cons} = \lambda h. \lambda t. \lambda c. \lambda n.\, c\, h\, (t\, c\, n)$$

So `[1, 2, 3]` becomes $\lambda c. \lambda n.\, c\, 1\, (c\, 2\, (c\, 3\, n))$.

```python
# Lists as fold functions in Python
nil  = lambda c: lambda n: n
cons = lambda h: lambda t: lambda c: lambda n: c(h)(t(c)(n))

# Build [1, 2, 3]
lst = cons(1)(cons(2)(cons(3)(nil)))

# Sum the list: fold with addition
to_sum = lst(lambda h: lambda acc: h + acc)(0)
print(to_sum)  # 6

# Convert to Python list
to_list = lst(lambda h: lambda acc: [h] + acc)([])
print(to_list)  # [1, 2, 3]
```

### Recursion: the Y combinator

Even recursion is a function. The **Y combinator** gives you recursion without any built-in recursion primitive:

$$Y = \lambda f.\, (\lambda x.\, f\, (x\, x))\, (\lambda x.\, f\, (x\, x))$$

It satisfies $Y\, f = f\, (Y\, f)$: applying $Y$ to $f$ gives you $f$ applied to its own recursive call.

### The deeper point

Lambda calculus is **Turing-complete** — it can compute anything a computer can compute. And it does this with nothing but functions. This proves, in a formal sense, that functions alone are sufficient for all of computation. Everything else (numbers, booleans, data structures, control flow) is syntactic sugar over functions.

---

## Stage 10: Homotopy Type Theory — Functions as Paths

### Motivation

The "everything is a function" story doesn't end with classical math. In homotopy type theory (HoTT), even *equality* and *geometric paths* become functions.

### The identity type

In HoTT, given $a, b : A$, the type $a =_A b$ (the identity type) represents "proofs that $a$ equals $b$." An inhabitant of this type is a **path** from $a$ to $b$.

By Curry-Howard (Stage 4), a path is a term of a type — i.e., can be viewed function-like. Specifically, a path $p : a =_A b$ can be modeled as a continuous function:

$$p : [0, 1] \to A \quad \text{with } p(0) = a, \; p(1) = b$$

### Functions between paths

A **homotopy** between functions $f, g : A \to B$ is a family of paths:

$$H : \prod_{x : A} (f(x) =_B g(x))$$

For each input $x$, $H(x)$ is a path from $f(x)$ to $g(x)$. A homotopy is a function that outputs paths — which are themselves function-like objects.

### Higher structure

Paths between paths (2-paths) witness that two proofs of equality are "the same." This gives rise to an infinite tower:

```
Level 0:  Points (elements of A)
Level 1:  Paths (elements of a =_A b)           ← functions [0,1] → A
Level 2:  Paths between paths (homotopies)       ← functions between functions
Level 3:  Paths between homotopies               ← ...
  ⋮
```

All the way up, it's functions and functions between functions.

### Univalence: equivalence is equality

The **univalence axiom** (Voevodsky) says: for types $A$ and $B$,

$$(A =_{\mathcal{U}} B) \simeq (A \simeq B)$$

Two types are equal iff there's an equivalence (bijective function) between them. Equality of types *is* a function — specifically, an equivalence.

### Why this matters

HoTT provides a foundational system where:
- Logic is functions (Curry-Howard)
- Equality is paths (functions from an interval)
- Spaces are types (with function-like higher structure)
- Properties of spaces are function types

It's the most comprehensive realization of "everything is a function" as a foundational principle.

---

## Stage 11: End-to-End Walkthrough — One Object Through Every Lens

### Motivation

Let's take a single, simple object — the set $\{0, 1\}$ — and trace how it looks as a function through every lens we've discussed.

### The boolean set $\mathbf{2} = \{0, 1\}$

**As a classifier (Stage 2):**
Functions $X \to \{0,1\}$ classify subsets of $X$. In category-theory language, $\{0,1\}$ is the **subobject classifier** $\Omega$ such that:

$$\text{Sub}(X) \cong \text{Hom}(X, \Omega)$$

Subsets are functions. $\{0,1\}$ is the object that makes this work.

**As a number (Stage 3):**
$2 = \{0, 1\} = \{\emptyset, \{\emptyset\}\}$ in the von Neumann encoding. As a Church numeral: $\mathbf{2} = \lambda f. \lambda x. f(f(x))$. "Apply twice."

**As a logic (Stage 4):**
$\{0, 1\} = \{\text{false}, \text{true}\}$. Functions $X \to \{0,1\}$ are predicates. $\{0,1\}$ is the set of truth values — the codomain of all propositional logic.

**As a space (Stage 6):**
With the discrete topology, its function algebra $C(\{0,1\}) = \mathbb{R}^2$. Points are evaluation functionals: $\text{ev}_0(f,g) = f$, $\text{ev}_1(f,g) = g$.

**As a category (Stage 7):**
View $\{0 \leq 1\}$ as a poset category with two objects and one non-identity morphism $0 \to 1$. Functors from this category to another category $\mathcal{C}$ pick out a single arrow. So the "arrow category" $\mathcal{C}^{\mathbf{2}}$ has morphisms of $\mathcal{C}$ as objects.

**As a representable (Stage 8):**
In $\mathbf{FinSet}$:

$$h^{\mathbf{2}}(B) = \text{Hom}(\{0,1\}, B) = B \times B$$

The two-element set is "the thing that picks out pairs."

**As a type (Stage 10):**
In HoTT, the type $\mathbf{2}$ has exactly two terms. The identity types $0 =_{\mathbf{2}} 0$ and $1 =_{\mathbf{2}} 1$ are inhabited (reflexivity), while $0 =_{\mathbf{2}} 1$ is empty. $\mathbf{2}$ is a "discrete" type with no nontrivial path structure.

### The common thread

In every lens, $\{0,1\}$ plays a role defined by the functions *into it or out of it*:
- Functions *into* $\{0,1\}$: predicates, subsets, truth assignments
- Functions *out of* $\{0,1\}$: choosing pairs, picking morphisms

The object $\{0,1\}$ *is* its function signature.

---

## Stage 12: Common Pitfalls and When This Breaks Down

### Pitfall 1: Confusing "can be encoded as" with "is"

You *can* encode the number $3$ as a Church numeral $\lambda f. \lambda x. f(f(f(x)))$. That doesn't mean $3$ "really is" a function in some ontological sense. The encoding is useful because it gives you computational purchase. But in other contexts (number theory, algebra), viewing $3$ as an element of $\mathbb{Z}$ is more natural. The "everything is a function" viewpoint is a **lens**, not a metaphysical claim.

### Pitfall 2: Losing concreteness in abstraction

Category theory gives you maximal generality, but you can get lost in morphism-speak and lose sight of what you're computing. A matrix is "a morphism in $\mathbf{Vect}_{\mathbb{R}}$," but when you need to solve $Ax = b$, you need the actual entries. The function viewpoint is a scaffold, not a replacement for concrete calculation.

### Pitfall 3: Assuming all categories are Set-like

In $\mathbf{Set}$, morphisms are literal functions with elements you can evaluate. In other categories (homotopy category, derived category), morphisms are equivalence classes and "evaluation at a point" may not make sense. The slogan is most precise when interpreted as "everything is a morphism in some category."

### Pitfall 4: Circularity

If you define numbers as functions and functions as sets of ordered pairs and ordered pairs using numbers... you need a ground floor. Each foundational system (set theory, type theory, lambda calculus) picks different primitives and builds the rest. "Everything is a function" works within a chosen system, not as an absolute statement.

### When the viewpoint is less helpful

- **Combinatorics and enumeration**: Counting arguments often work better with direct set-theoretic reasoning than with morphism gymnastics.
- **Numerical computation**: When you're optimizing a matrix multiply for cache locality, the abstract function view adds nothing. You need bytes in memory.
- **Foundations debates**: Whether functions are "really" sets of ordered pairs (set theory), or sets are "really" special types (type theory), or both are "really" morphisms (category theory) is a philosophical question, not a mathematical one.

---

## Stage 13: The Transferable Pattern and Key Takeaways

### The generalizable pattern: Representable Thinking

The deepest version of "everything is a function" is the **representable thinking** pattern:

1. **Don't define objects by what they are — define them by what they do.** (= their morphisms, their API)
2. **If two objects do the same thing (have the same morphisms), they're the same.** (= Yoneda lemma, universal properties)
3. **To understand an object, study the functions out of it and into it.** (= representable functors, probing)

This pattern appears in:
- **Software engineering**: An interface is defined by its methods (functions), not its internal state. Dependency injection, duck typing, and protocol-oriented programming all embody this.
- **Physics**: An observable is a function from states to numbers. Quantum mechanics defines a system through its algebra of observables.
- **Machine learning**: A representation (embedding) is a function from raw data to a vector space. The representation *is* the model's understanding.

### Summary table

| Math object | Function view | Key insight |
|---|---|---|
| Sequence | $a : \mathbb{N} \to S$ | Enables infinite-dimensional thinking |
| Subset | $\chi_A : X \to \{0,1\}$ | Set theory $\subseteq$ function theory |
| Number | Church numeral / element picker | Can be derived, not primitive |
| Proposition | $P : X \to \{T, F\}$ or a type | Curry-Howard: proofs = functions |
| Relation | $R : X \times Y \to \{0,1\}$ or $X \to \mathcal{P}(Y)$ | Relations are multivalued functions |
| Space | Determined by $C(X)$ | Gelfand-Naimark: spaces = function algebras |
| Object in a category | Determined by $\text{Hom}(-, X)$ | Yoneda: objects = their morphisms |
| All of computation | Lambda calculus | Functions are Turing-complete |
| Equality / paths | Functions from $[0,1]$ | HoTT: geometry = function types |

### Key takeaways

1. **The function concept is universal**: Nearly every mathematical structure can be encoded, characterized, or reconstructed from functions.
2. **This isn't just encoding**: Viewing things as functions gives you compositionality (combine things), universality (one framework), and abstraction (right level of detail).
3. **The Yoneda lemma is the formal justification**: An object is determined by the morphisms into/out of it. Functions are all you need.
4. **Choose the right lens**: "Everything is a function" is maximally useful for structural reasoning, type theory, and categorical thinking. For concrete computation, you still need concrete representations.
5. **The engineering parallel**: In software, this is the "program to an interface, not an implementation" principle. In math, it's the Yoneda principle. Same idea, different languages.

### Practice exercises

1. **Characteristic functions**: Let $X = \{1,2,3,4,5\}$, $A = \{2,4\}$, $B = \{1,2,3\}$. Write out $\chi_A$, $\chi_B$, $\chi_{A \cap B}$, and $\chi_{A \cup B}$ as tables. Verify that $\chi_{A \cap B} = \chi_A \cdot \chi_B$ pointwise.

2. **Church arithmetic**: Implement Church exponentiation in Python: `exp = lambda m: lambda n: n(m)`. Verify that `exp(two)(three)` converts to $2^3 = 8$ using the `to_int` function from Stage 9. (Yes, exponentiation is the simplest Church operation — why?)

3. **Yoneda for a small category**: Consider the poset $\{a, b, c\}$ with $a \leq b \leq c$. Write out the representable functors $h^a$, $h^b$, $h^c$ as tables (for each object $x$, is $\text{Hom}(\_, x)$ empty or a singleton?). Verify that no two representable functors are the same.
