# Quantum Information and Computation: Single Systems

**Complete, self-contained study notes**

> [!TIP]
> This file uses **GitHub Flavored Markdown (GFM)** and GitHub-compatible LaTeX math: inline expressions use `$...$`, and display equations use `$$...$$`.

> [!NOTE]
> **Scope:** These notes explain the mathematical description of classical and quantum information for a **single isolated system**. They begin with classical information because quantum information is best understood as an extension of it. The central objects are vectors, matrices, measurements, and operations.

---

## Table of Contents

1. [Learning objectives](#1-learning-objectives)
2. [Two mathematical descriptions of quantum information](#2-two-mathematical-descriptions-of-quantum-information)
3. [Mathematical prerequisites and notation](#3-mathematical-prerequisites-and-notation)
4. [Classical information and classical states](#4-classical-information-and-classical-states)
5. [Probability vectors and probabilistic states](#5-probability-vectors-and-probabilistic-states)
6. [Dirac notation: kets, bras, inner products, and outer products](#6-dirac-notation-kets-bras-inner-products-and-outer-products)
7. [Measurement of classical probabilistic states](#7-measurement-of-classical-probabilistic-states)
8. [Classical deterministic operations](#8-classical-deterministic-operations)
9. [Classical probabilistic operations and stochastic matrices](#9-classical-probabilistic-operations-and-stochastic-matrices)
10. [Composition of classical operations](#10-composition-of-classical-operations)
11. [Quantum states of a single system](#11-quantum-states-of-a-single-system)
12. [Complex amplitudes, phase, and normalization](#12-complex-amplitudes-phase-and-normalization)
13. [Standard-basis quantum measurement](#13-standard-basis-quantum-measurement)
14. [Unitary operations](#14-unitary-operations)
15. [Important single-qubit gates](#15-important-single-qubit-gates)
16. [Computing gate actions](#16-computing-gate-actions)
17. [Composition of unitary operations](#17-composition-of-unitary-operations)
18. [The square root of NOT](#18-the-square-root-of-not)
19. [Classical–quantum comparison](#19-classicalquantum-comparison)
20. [Common misconceptions and pitfalls](#20-common-misconceptions-and-pitfalls)
21. [Key results to memorize](#21-key-results-to-memorize)
22. [Worked examples](#22-worked-examples)
23. [Practice problems](#23-practice-problems)
24. [Solutions](#24-solutions)
25. [Where this leads next](#25-where-this-leads-next)

---

## 1. Learning Objectives

After studying these notes, you should be able to:

- Define a finite classical state set for an information-bearing system.
- Represent uncertainty about a classical state using a probability vector.
- Read and use Dirac notation, including kets, bras, inner products, and outer products.
- Represent deterministic classical operations as matrices.
- Recognize and use column-stochastic matrices.
- Explain why operation composition corresponds to matrix multiplication in reverse chronological order.
- Define a pure quantum state as a normalized complex vector.
- Apply the Born rule for a standard-basis measurement.
- Explain measurement-induced state update, often called collapse.
- Define unitary matrices and explain why they preserve valid quantum states.
- Use the Pauli, Hadamard, phase, S, and T gates.
- Distinguish absolute/global phase from physically meaningful relative phase.
- Compute the result of a sequence of quantum operations.
- Explain why quantum operations can exhibit behavior that has no classical stochastic analogue, such as a square root of NOT.

---

## 2. Two Mathematical Descriptions of Quantum Information

Quantum information is commonly introduced through two compatible descriptions.

### 2.1 Simplified description

The simplified description uses:

- **State vectors** to describe quantum states.
- **Unitary matrices** to describe closed-system evolution.
- Projective or basis measurements to extract classical outcomes.

This is the natural starting point and is sufficient for understanding a large portion of quantum algorithms and quantum circuits.

A state is written as a normalized vector such as

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle,
\qquad |\alpha|^2+|\beta|^2=1.
$$

An operation is represented by a unitary matrix $U$, and the state changes according to

$$
|\psi\rangle \longmapsto U|\psi\rangle.
$$

### 2.2 General description

The general description uses:

- **Density matrices** for states.
- **Quantum channels**, usually completely positive trace-preserving maps, for operations.
- **POVMs**—positive operator-valued measures—for general measurements.

This broader framework can describe:

- Classical uncertainty about quantum states.
- Entanglement with an unobserved environment.
- Decoherence.
- Noise.
- State preparation and measurement errors.
- Open quantum systems.

The simplified pure-state/unitary description is not wrong. It is a special case of the general description. A pure state $|\psi\rangle$ corresponds to the density matrix

$$
\rho = |\psi\rangle\langle\psi|.
$$

These notes focus on the simplified description, while occasionally pointing toward the general framework.

---

## 3. Mathematical Prerequisites and Notation

### 3.1 Finite sets

A set $\Sigma$ is **finite** when it contains a finite number of elements. Its size is written

$$
|\Sigma|.
$$

For example,

$$
\Sigma=\{0,1\}, \qquad |\Sigma|=2.
$$

The symbol $\Sigma$ will represent the possible classical states of a system.

### 3.2 Column and row vectors

A column vector has the form

$$
v=
\begin{pmatrix}
v_1\\
v_2\\
\vdots\\
v_n
\end{pmatrix}.
$$

A row vector has the form

$$
w=
\begin{pmatrix}
w_1 & w_2 & \cdots & w_n
\end{pmatrix}.
$$

### 3.3 Complex numbers

A complex number has the form

$$
z=a+bi,
$$

where $a,b\in\mathbb{R}$ and $i^2=-1$.

Its complex conjugate is

$$
z^*=a-bi.
$$

Its magnitude is

$$
|z|=\sqrt{a^2+b^2},
$$

so

$$
|z|^2=z^*z=a^2+b^2.
$$

For example,

$$
|1+2i|^2=1^2+2^2=5.
$$

### 3.4 Transpose and conjugate transpose

The transpose of a matrix exchanges rows and columns:

$$
A^T.
$$

For a complex matrix, the **conjugate transpose**, also called the **adjoint**, is

$$
A^\dagger=(A^*)^T.
$$

That means:

1. Take the complex conjugate of every entry.
2. Exchange rows and columns.

For a vector

$$
|\psi\rangle=
\begin{pmatrix}
\alpha\\
\beta
\end{pmatrix},
$$

its conjugate transpose is

$$
\langle\psi|=
\begin{pmatrix}
\alpha^* & \beta^*
\end{pmatrix}.
$$

### 3.5 Identity matrix

The $n\times n$ identity matrix is

$$
I_n=
\begin{pmatrix}
1&0&\cdots&0\\
0&1&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&1
\end{pmatrix}.
$$

It satisfies

$$
I_nv=v
$$

for every $n$-dimensional vector $v$.

### 3.6 Matrix multiplication and order

If $A$ and $B$ are matrices and $v$ is a vector, then

$$
A(Bv)=(AB)v.
$$

Matrix multiplication is **associative**, but generally not commutative:

$$
A(BC)=(AB)C,
$$

but usually

$$
AB\ne BA.
$$

This fact is essential because the order of information-processing operations matters.

---

## 4. Classical Information and Classical States

Consider a physical system or information-bearing device called $X$.

We assume that $X$ has a finite, nonempty set of recognizable classical states:

$$
\Sigma=\{a_1,a_2,\ldots,a_n\}.
$$

A **classical state** is an unambiguous configuration of the system within the abstraction being used.

### 4.1 Examples

#### A bit

$$
\Sigma=\{0,1\}.
$$

This set is often called the **binary alphabet**.

#### A six-sided die

$$
\Sigma=\{1,2,3,4,5,6\}.
$$

A real die also has position, orientation, temperature, material, and many other physical properties. In the information-processing abstraction, only the upward-facing number is treated as the state.

#### A fan switch

$$
\Sigma=\{\text{off},\text{low},\text{medium},\text{high}\}.
$$

### 4.2 Why the state set must be nonempty

A system must have at least one possible state. To store nontrivial information, it must have at least two distinguishable states.

### 4.3 Abstraction is deliberate

The state set is chosen according to what matters for the problem. A physical object has enormously many microscopic properties, but an information model keeps only the distinctions relevant to the task.

---

## 5. Probability Vectors and Probabilistic States

Sometimes the exact classical state is known. Often it is not.

Suppose $X$ is a bit and

$$
\Pr(X=0)=\frac34,
\qquad
\Pr(X=1)=\frac14.
$$

This uncertainty is described by the probability vector

$$
p=
\begin{pmatrix}
3/4\\
1/4
\end{pmatrix}.
$$

The ordering of entries follows the chosen ordering of the classical states, here $0$ first and $1$ second.

### 5.1 Definition: probability vector

A vector

$$
p=
\begin{pmatrix}
p_1\\p_2\\\vdots\\p_n
\end{pmatrix}
$$

is a probability vector when

$$
p_j\ge 0
\quad\text{for every }j,
$$

and

$$
\sum_{j=1}^{n}p_j=1.
$$

Each entry represents the probability of the corresponding classical state.

### 5.2 Certain classical states as probability vectors

If the system is certainly in state $a$, then its probability vector has a $1$ in the position corresponding to $a$ and $0$ elsewhere.

For a bit,

$$
|0\rangle=
\begin{pmatrix}
1\\0
\end{pmatrix},
\qquad
|1\rangle=
\begin{pmatrix}
0\\1
\end{pmatrix}.
$$

Thus, definite classical states are special probability vectors.

---

## 6. Dirac Notation: Kets, Bras, Inner Products, and Outer Products

Dirac notation is a compact language for vectors and matrices. It is heavily used in quantum information, but it is not inherently quantum mechanical.

### 6.1 Kets

A **ket** is a column vector. For a classical state $a\in\Sigma$,

$$
|a\rangle
$$

is the standard basis vector containing $1$ in the coordinate corresponding to $a$ and $0$ elsewhere.

For a bit,

$$
|0\rangle=
\begin{pmatrix}1\\0\end{pmatrix},
\qquad
|1\rangle=
\begin{pmatrix}0\\1\end{pmatrix}.
$$

These are called **standard basis vectors** or **computational-basis vectors**.

### 6.2 Example with four card suits

Choose the order

$$
(\clubsuit,\diamondsuit,\heartsuit,\spadesuit).
$$

Then

$$
|\clubsuit\rangle=
\begin{pmatrix}1\\0\\0\\0\end{pmatrix},
\quad
|\diamondsuit\rangle=
\begin{pmatrix}0\\1\\0\\0\end{pmatrix},
$$

$$
|\heartsuit\rangle=
\begin{pmatrix}0\\0\\1\\0\end{pmatrix},
\quad
|\spadesuit\rangle=
\begin{pmatrix}0\\0\\0\\1\end{pmatrix}.
$$

There is no universally required ordering. The important point is to choose an ordering and use it consistently.

### 6.3 Expanding a vector in the standard basis

Every vector can be written uniquely as a linear combination of standard basis vectors.

For the probability vector

$$
p=
\begin{pmatrix}
3/4\\1/4
\end{pmatrix},
$$

we can write

$$
p=\frac34|0\rangle+\frac14|1\rangle.
$$

More generally,

$$
v=\sum_{a\in\Sigma}v_a|a\rangle.
$$

### 6.4 Bras

A **bra** is a row vector. For a standard basis state $a$,

$$
\langle a|
$$

has a $1$ in the coordinate corresponding to $a$ and $0$ elsewhere.

For a bit,

$$
\langle0|=
\begin{pmatrix}1&0\end{pmatrix},
\qquad
\langle1|=
\begin{pmatrix}0&1\end{pmatrix}.
$$

The words **bra** and **ket** are the two halves of the word **bracket**.

### 6.5 Inner products

Multiplying a bra by a ket produces a scalar:

$$
\langle a|b\rangle.
$$

For standard basis vectors,

$$
\langle a|b\rangle=
\begin{cases}
1,&a=b,\\
0,&a\ne b.
\end{cases}
$$

This is also written using the Kronecker delta:

$$
\langle a|b\rangle=\delta_{ab}.
$$

For a general complex vector

$$
|\psi\rangle=
\begin{pmatrix}
\alpha\\\beta
\end{pmatrix},
$$

we define

$$
\langle\psi|=(|\psi\rangle)^\dagger
=
\begin{pmatrix}
\alpha^*&\beta^*
\end{pmatrix}.
$$

Then

$$
\langle\psi|\psi\rangle=|\alpha|^2+|\beta|^2.
$$

### 6.6 Outer products

Multiplying a ket by a bra produces a matrix:

$$
|a\rangle\langle b|.
$$

For a bit,

$$
|0\rangle\langle0|
=
\begin{pmatrix}
1&0\\0&0
\end{pmatrix},
$$

$$
|0\rangle\langle1|
=
\begin{pmatrix}
0&1\\0&0
\end{pmatrix},
$$

$$
|1\rangle\langle0|
=
\begin{pmatrix}
0&0\\1&0
\end{pmatrix},
$$

$$
|1\rangle\langle1|
=
\begin{pmatrix}
0&0\\0&1
\end{pmatrix}.
$$

In general, $|a\rangle\langle b|$ is the matrix containing a single $1$ in row $a$, column $b$, and zeros elsewhere.

### 6.7 Completeness relation

The standard basis satisfies

$$
\sum_{a\in\Sigma}|a\rangle\langle a|=I.
$$

For a qubit,

$$
|0\rangle\langle0|+|1\rangle\langle1|=I_2.
$$

This relation is useful throughout quantum mechanics and linear algebra.

---

## 7. Measurement of Classical Probabilistic States

Suppose a classical system has probabilistic state

$$
p=\sum_{a\in\Sigma}p(a)|a\rangle.
$$

A classical measurement means inspecting the system and learning its classical state.

### 7.1 Outcome probabilities

The outcome $a$ occurs with probability

$$
p(a).
$$

### 7.2 State update after observation

Once the outcome $a$ is known, the observer’s new probabilistic description is

$$
|a\rangle.
$$

For example,

$$
p=\frac34|0\rangle+\frac14|1\rangle
$$

leads to:

- Outcome $0$ with probability $3/4$, after which the state description becomes $|0\rangle$.
- Outcome $1$ with probability $1/4$, after which the state description becomes $|1\rangle$.

In a classical setting, this update can be interpreted as a change in knowledge rather than a physical change in the object.

### 7.3 Observer dependence of information

An observer who sees the measurement result may assign the definite state $|a\rangle$. Another observer who does not learn the result may continue using the original probability vector.

This is not a contradiction. Probability distributions encode an observer’s available information.

---

## 8. Classical Deterministic Operations

A deterministic operation changes the classical state without introducing randomness.

Mathematically, it is described by a function

$$
f:\Sigma\to\Sigma.
$$

If the input is $a$, the output is

$$
f(a).
$$

### 8.1 Matrix representation

Every such function has a unique matrix $M_f$ satisfying

$$
M_f|a\rangle=|f(a)\rangle
$$

for every $a\in\Sigma$.

The entry in row $b$, column $a$ is

$$
(M_f)_{b,a}=
\begin{cases}
1,&b=f(a),\\
0,&b\ne f(a).
\end{cases}
$$

Each column contains exactly one $1$, because every input has exactly one output.

### 8.2 Dirac-notation formula

The matrix can be written compactly as

$$
M_f=\sum_{a\in\Sigma}|f(a)\rangle\langle a|.
$$

Verification:

$$
M_f|b\rangle
=
\sum_{a\in\Sigma}|f(a)\rangle\langle a|b\rangle.
$$

Since $\langle a|b\rangle=\delta_{ab}$, every term vanishes except the term $a=b$:

$$
M_f|b\rangle=|f(b)\rangle.
$$

### 8.3 All deterministic operations on one bit

There are exactly four functions from $\{0,1\}$ to itself.

#### 1. Constant-zero function

$$
f_1(0)=0,\qquad f_1(1)=0.
$$

$$
M_1=
\begin{pmatrix}
1&1\\
0&0
\end{pmatrix}.
$$

#### 2. Identity function

$$
f_2(0)=0,\qquad f_2(1)=1.
$$

$$
M_2=
\begin{pmatrix}
1&0\\
0&1
\end{pmatrix}=I.
$$

#### 3. NOT or bit-flip function

$$
f_3(0)=1,\qquad f_3(1)=0.
$$

$$
M_3=
\begin{pmatrix}
0&1\\
1&0
\end{pmatrix}.
$$

#### 4. Constant-one function

$$
f_4(0)=1,\qquad f_4(1)=1.
$$

$$
M_4=
\begin{pmatrix}
0&0\\
1&1
\end{pmatrix}.
$$

### 8.4 Acting on probabilistic states

If $p$ is a probability vector, then the state after applying the deterministic operation is

$$
p'=M_fp.
$$

Example: apply NOT to

$$
p=
\begin{pmatrix}
3/4\\1/4
\end{pmatrix}.
$$

Then

$$
M_3p=
\begin{pmatrix}
0&1\\1&0
\end{pmatrix}
\begin{pmatrix}
3/4\\1/4
\end{pmatrix}
=
\begin{pmatrix}
1/4\\3/4
\end{pmatrix}.
$$

The probabilities are exchanged, exactly as expected from a bit flip.

---

## 9. Classical Probabilistic Operations and Stochastic Matrices

A probabilistic operation may introduce randomness. Deterministic operations are special cases with no introduced randomness.

### 9.1 Example operation

Consider the following operation on a bit:

- If the input is $0$, leave it as $0$.
- If the input is $1$, output $0$ with probability $1/2$ and $1$ with probability $1/2$.

Its matrix is

$$
M=
\begin{pmatrix}
1&1/2\\
0&1/2
\end{pmatrix}.
$$

The first column gives the output probability vector for input $0$:

$$
M|0\rangle=
\begin{pmatrix}1\\0\end{pmatrix}=|0\rangle.
$$

The second column gives the output probability vector for input $1$:

$$
M|1\rangle=
\begin{pmatrix}1/2\\1/2\end{pmatrix}.
$$

### 9.2 Definition: column-stochastic matrix

A matrix $M$ is column-stochastic when:

1. Every entry is a nonnegative real number.
2. The entries in each column sum to $1$.

Equivalently, every column is a probability vector.

> **Convention warning:** Some fields use row-stochastic matrices instead. In these notes, states are column vectors, so operations are represented by **column-stochastic** matrices.

### 9.3 Why stochastic matrices preserve probability vectors

Let $p$ be a probability vector. Then

$$
Mp
$$

is a weighted combination of the columns of $M$, with weights given by the entries of $p$. Since the columns are probability vectors and the weights are nonnegative and sum to $1$, the result is also a probability vector.

More explicitly, if $\mathbf{1}^T=(1,1,\ldots,1)$, column stochasticity means

$$
\mathbf{1}^TM=\mathbf{1}^T.
$$

Therefore,

$$
\mathbf{1}^T(Mp)=(\mathbf{1}^TM)p=\mathbf{1}^Tp=1.
$$

Nonnegativity is also preserved because both $M$ and $p$ have nonnegative entries.

### 9.4 Probabilistic operations as random deterministic operations

The example matrix can be written as

$$
M=\frac12
\begin{pmatrix}
1&1\\0&0
\end{pmatrix}
+
\frac12
\begin{pmatrix}
1&0\\0&1
\end{pmatrix}.
$$

Thus, it is equivalent to:

1. Flip a fair coin.
2. With probability $1/2$, apply the constant-zero operation.
3. With probability $1/2$, apply the identity operation.

More generally, any finite classical probabilistic operation can be viewed as a random choice among deterministic operations. This follows because all required random choices can, conceptually, be made in advance and encoded into a deterministic rule.

---

## 10. Composition of Classical Operations

Suppose a system starts in probability vector $p$.

Apply $M_1$ first:

$$
p_1=M_1p.
$$

Then apply $M_2$:

$$
p_2=M_2p_1=M_2(M_1p).
$$

By associativity,

$$
p_2=(M_2M_1)p.
$$

Therefore, the combined operation is represented by

$$
M_2M_1.
$$

### 10.1 Rightmost operation acts first

If operations are performed in chronological order

$$
M_1, M_2,\ldots,M_n,
$$

then the total matrix is

$$
M_nM_{n-1}\cdots M_2M_1.
$$

The first operation is on the far right because it acts directly on the original state vector.

### 10.2 Closure under multiplication

The product of column-stochastic matrices is also column-stochastic. Therefore, stochastic matrices are **closed under multiplication**.

This is exactly what we expect: performing valid probabilistic operations one after another should still produce a valid probabilistic operation.

### 10.3 Order matters

Let

$$
M_1=
\begin{pmatrix}
1&1\\0&0
\end{pmatrix}
$$

be constant zero, and

$$
M_2=
\begin{pmatrix}
0&1\\1&0
\end{pmatrix}
$$

be NOT.

Apply constant zero first, then NOT:

$$
M_2M_1=
\begin{pmatrix}
0&0\\1&1
\end{pmatrix},
$$

which is constant one.

Apply NOT first, then constant zero:

$$
M_1M_2=
\begin{pmatrix}
1&1\\0&0
\end{pmatrix},
$$

which is constant zero.

Hence,

$$
M_2M_1\ne M_1M_2.
$$

This is a concrete example of noncommutativity.

---

## 11. Quantum States of a Single System

Quantum information begins with a different definition of state.

Let a system have classical state set

$$
\Sigma=\{a_1,\ldots,a_n\}.
$$

A pure quantum state is a complex column vector

$$
|\psi\rangle
=
\sum_{a\in\Sigma}\alpha_a|a\rangle,
$$

where each $\alpha_a\in\mathbb{C}$ and

$$
\sum_{a\in\Sigma}|\alpha_a|^2=1.
$$

The coefficients $\alpha_a$ are called **amplitudes**.

### 11.1 Amplitudes are not probabilities

The amplitude $\alpha_a$ is generally complex and may be negative or imaginary. It is not itself a probability.

The corresponding standard-basis measurement probability is

$$
|\alpha_a|^2.
$$

Because probabilities are obtained after taking absolute values squared, information contained in signs and complex phases can be hidden from one measurement basis and revealed by later interference.

### 11.2 Euclidean norm

For

$$
v=
\begin{pmatrix}
\alpha_1\\\vdots\\\alpha_n
\end{pmatrix},
$$

the Euclidean norm is

$$
\|v\|_2
=
\sqrt{\sum_{j=1}^{n}|\alpha_j|^2}.
$$

A quantum state vector must satisfy

$$
\||\psi\rangle\|_2=1.
$$

Equivalently,

$$
\langle\psi|\psi\rangle=1.
$$

Thus, pure quantum states are unit vectors in a complex vector space.

### 11.3 Qubits

A **qubit**, short for quantum bit, is a quantum system with classical basis states $0$ and $1$.

A general qubit pure state is

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
$$

with

$$
|\alpha|^2+|\beta|^2=1.
$$

In column-vector form,

$$
|\psi\rangle=
\begin{pmatrix}
\alpha\\\beta
\end{pmatrix}.
$$

### 11.4 Standard-basis states

$$
|0\rangle=
\begin{pmatrix}1\\0\end{pmatrix},
\qquad
|1\rangle=
\begin{pmatrix}0\\1\end{pmatrix}.
$$

These are valid quantum states because their norm is $1$.

### 11.5 Plus and minus states

$$
|+\rangle=\frac{|0\rangle+|1\rangle}{\sqrt2}
=
\frac1{\sqrt2}
\begin{pmatrix}1\\1\end{pmatrix},
$$

$$
|-\rangle=\frac{|0\rangle-|1\rangle}{\sqrt2}
=
\frac1{\sqrt2}
\begin{pmatrix}1\\-1\end{pmatrix}.
$$

Normalization check:

$$
\left|\frac1{\sqrt2}\right|^2
+
\left|\frac{\pm1}{\sqrt2}\right|^2
=
\frac12+\frac12=1.
$$

### 11.6 A complex-amplitude example

Consider

$$
|\psi\rangle=
\frac{1+2i}{3}|0\rangle-
\frac23|1\rangle.
$$

The squared magnitudes are

$$
\left|\frac{1+2i}{3}\right|^2
=
\frac{1^2+2^2}{9}
=
\frac59,
$$

and

$$
\left|-\frac23\right|^2=\frac49.
$$

Therefore,

$$
\frac59+\frac49=1,
$$

so the vector is normalized.

Its bra is

$$
\langle\psi|
=
\frac{1-2i}{3}\langle0|-
\frac23\langle1|.
$$

### 11.7 A four-dimensional example

For the ordered basis

$$
(\clubsuit,\diamondsuit,\heartsuit,\spadesuit),
$$

consider

$$
|\phi\rangle=
\frac12|\clubsuit\rangle
-
\frac{i}{2}|\diamondsuit\rangle
+
0|\heartsuit\rangle
+
\frac1{\sqrt2}|\spadesuit\rangle.
$$

Its normalization is

$$
\frac14+\frac14+0+\frac12=1.
$$

This illustrates that the quantum-state formalism is not limited to qubits.

---

## 12. Complex Amplitudes, Phase, and Normalization

Complex phases are responsible for much of the distinctive behavior of quantum information.

### 12.1 Polar form

Every nonzero complex number can be written as

$$
z=re^{i\theta},
$$

where $r=|z|\ge0$ and $\theta$ is its phase angle.

Euler’s formula is

$$
e^{i\theta}=\cos\theta+i\sin\theta.
$$

The magnitude is

$$
|re^{i\theta}|=r.
$$

So changing the phase does not change the squared magnitude directly.

### 12.2 Global phase

The states

$$
|\psi\rangle
\quad\text{and}\quad
 e^{i\gamma}|\psi\rangle
$$

represent the same physical pure state when the entire vector is multiplied by one common phase factor.

For any standard-basis outcome $a$,

$$
|\langle a|e^{i\gamma}\psi\rangle|^2
=
|e^{i\gamma}|^2|\langle a|\psi\rangle|^2
=
|\langle a|\psi\rangle|^2.
$$

A global phase cannot affect any measurement probability.

### 12.3 Relative phase

A relative phase between components is physically meaningful.

Compare

$$
|+\rangle=\frac{|0\rangle+|1\rangle}{\sqrt2}
$$

and

$$
|-\rangle=\frac{|0\rangle-|1\rangle}{\sqrt2}.
$$

Both produce $0$ and $1$ with equal probability in a standard-basis measurement. However, they are distinct states because the relative sign changes interference under later operations.

For example,

$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle.
$$

Thus, the Hadamard gate converts the hidden relative-phase distinction into an observable standard-basis distinction.

### 12.4 Qubit parameterization

Ignoring global phase, every pure qubit state can be written as

$$
|\psi\rangle
=
\cos\frac{\theta}{2}|0\rangle
+
e^{i\phi}\sin\frac{\theta}{2}|1\rangle,
$$

with

$$
0\le\theta\le\pi,
\qquad
0\le\phi<2\pi.
$$

This is the basis of the Bloch-sphere representation. The two angles encode the physically meaningful degrees of freedom of a pure qubit after global phase has been removed.

---

## 13. Standard-Basis Quantum Measurement

Measurement is the interface through which classical information is extracted from a quantum system.

Suppose

$$
|\psi\rangle=
\sum_{a\in\Sigma}\alpha_a|a\rangle.
$$

A standard-basis measurement has possible outcomes $a\in\Sigma$.

### 13.1 Born rule

The probability of outcome $a$ is

$$
\Pr(a)=|\alpha_a|^2.
$$

Equivalently,

$$
\Pr(a)=|\langle a|\psi\rangle|^2.
$$

Another equivalent form is

$$
\Pr(a)=
\langle\psi|
\bigl(|a\rangle\langle a|\bigr)
|\psi\rangle.
$$

The normalization condition guarantees

$$
\sum_{a\in\Sigma}\Pr(a)=1.
$$

### 13.2 State update

If the observed outcome is $a$, the post-measurement state becomes

$$
|a\rangle.
$$

This update is often called **collapse of the state vector**.

### 13.3 Examples

#### Measuring $|+\rangle$

$$
|+\rangle=\frac1{\sqrt2}|0\rangle+\frac1{\sqrt2}|1\rangle.
$$

Therefore,

$$
\Pr(0)=\frac12,
\qquad
\Pr(1)=\frac12.
$$

#### Measuring $|-\rangle$

$$
|-\rangle=\frac1{\sqrt2}|0\rangle-\frac1{\sqrt2}|1\rangle.
$$

Again,

$$
\Pr(0)=\frac12,
\qquad
\Pr(1)=\frac12.
$$

The minus sign does not affect this particular measurement because probabilities use absolute values squared.

#### Measuring the complex-amplitude state

For

$$
|\psi\rangle=
\frac{1+2i}{3}|0\rangle-\frac23|1\rangle,
$$

$$
\Pr(0)=\frac59,
\qquad
\Pr(1)=\frac49.
$$

If the outcome is $0$, the post-measurement state is $|0\rangle$. If the outcome is $1$, the post-measurement state is $|1\rangle$.

#### Measuring basis states

$$
|0\rangle\longrightarrow 0
\quad\text{with probability }1,
$$

$$
|1\rangle\longrightarrow 1
\quad\text{with probability }1.
$$

### 13.4 Repeated measurement

Suppose a standard-basis measurement gives outcome $a$. The state becomes $|a\rangle$. A second immediate measurement in the same basis gives $a$ again with probability $1$.

This does not mean unlimited information can be extracted. After the first measurement, the original superposition has generally been replaced by a basis state.

### 13.5 Measurement in another basis

A measurement in a different orthonormal basis can often be implemented by applying a unitary transformation and then performing a standard-basis measurement.

For example, to distinguish $|+\rangle$ from $|-\rangle$:

1. Apply $H$.
2. Measure in the standard basis.

Since

$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle,
$$

the states are perfectly distinguishable.

This example shows that two states can look identical in one measurement basis and different in another.

---

## 14. Unitary Operations

Closed-system quantum operations in the simplified model are represented by unitary matrices.

### 14.1 Definition

A square complex matrix $U$ is unitary when

$$
U^\dagger U=I
$$

and equivalently

$$
UU^\dagger=I.
$$

For square matrices, either equality implies the other.

Therefore,

$$
U^{-1}=U^\dagger.
$$

### 14.2 Norm preservation

A matrix is unitary if and only if it preserves the Euclidean norm of every vector:

$$
\|Uv\|_2=\|v\|_2.
$$

Proof:

$$
\|Uv\|_2^2
=(Uv)^\dagger(Uv)
=v^\dagger U^\dagger Uv
=v^\dagger Iv
=v^\dagger v
=\|v\|_2^2.
$$

Thus, if $|\psi\rangle$ is normalized, then $U|\psi\rangle$ is also normalized.

### 14.3 Inner-product preservation

Unitary matrices preserve inner products:

$$
\langle U\phi|U\psi\rangle
=
\langle\phi|U^\dagger U|\psi\rangle
=
\langle\phi|\psi\rangle.
$$

Consequences include preservation of:

- Norms.
- Angles in the complex inner-product sense.
- Orthogonality.
- Distinguishability of orthogonal pure states.

### 14.4 Reversibility

Every unitary evolution is reversible because

$$
U^\dagger U=I.
$$

To undo $U$, apply $U^\dagger$.

This is an important contrast with irreversible classical operations such as constant-zero, which destroy information and cannot be inverted.

### 14.5 Why unitaries are the correct linear operations

Quantum state vectors must remain normalized. Once we require state transformations to be linear and reversible while preserving all norms, unitary matrices arise naturally.

Similarly:

- Probability vectors are preserved by stochastic matrices.
- Quantum pure-state vectors are preserved by unitary matrices.

---

## 15. Important Single-Qubit Gates

### 15.1 Identity gate

$$
I=
\begin{pmatrix}
1&0\\0&1
\end{pmatrix}.
$$

It leaves every state unchanged:

$$
I|\psi\rangle=|\psi\rangle.
$$

### 15.2 Pauli-X gate

$$
X=
\begin{pmatrix}
0&1\\1&0
\end{pmatrix}.
$$

Actions:

$$
X|0\rangle=|1\rangle,
\qquad
X|1\rangle=|0\rangle.
$$

For a general qubit,

$$
X(\alpha|0\rangle+\beta|1\rangle)
=
\beta|0\rangle+\alpha|1\rangle.
$$

It is called:

- NOT gate.
- Bit-flip gate.
- Pauli-X operation.

### 15.3 Pauli-Y gate

$$
Y=
\begin{pmatrix}
0&-i\\i&0
\end{pmatrix}.
$$

Actions:

$$
Y|0\rangle=i|1\rangle,
\qquad
Y|1\rangle=-i|0\rangle.
$$

It performs a bit flip together with phase changes.

### 15.4 Pauli-Z gate

$$
Z=
\begin{pmatrix}
1&0\\0&-1
\end{pmatrix}.
$$

Actions:

$$
Z|0\rangle=|0\rangle,
\qquad
Z|1\rangle=-|1\rangle.
$$

For a general qubit,

$$
Z(\alpha|0\rangle+\beta|1\rangle)
=
\alpha|0\rangle-\beta|1\rangle.
$$

It is called a **phase flip** because it changes the relative phase of the $|1\rangle$ component by $\pi$.

For the plus and minus states,

$$
Z|+\rangle=|-\rangle,
\qquad
Z|-\rangle=|+\rangle.
$$

### 15.5 Hermitian and self-inverse properties of Pauli gates

Each Pauli matrix is Hermitian:

$$
X^\dagger=X,
\qquad
Y^\dagger=Y,
\qquad
Z^\dagger=Z.
$$

Each squares to identity:

$$
X^2=Y^2=Z^2=I.
$$

Therefore, each is unitary and each is its own inverse.

### 15.6 Hadamard gate

$$
H=
\frac1{\sqrt2}
\begin{pmatrix}
1&1\\1&-1
\end{pmatrix}.
$$

Its most important actions are

$$
H|0\rangle=|+\rangle,
\qquad
H|1\rangle=|-\rangle,
$$

and

$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle.
$$

Also,

$$
H^\dagger=H,
\qquad
H^2=I.
$$

The Hadamard gate changes between the computational basis

$$
\{|0\rangle,|1\rangle\}
$$

and the Hadamard or $X$-basis

$$
\{|+\rangle,|-\rangle\}.
$$

### 15.7 General phase gate

For any real $\theta$, define

$$
P(\theta)=
\begin{pmatrix}
1&0\\0&e^{i\theta}
\end{pmatrix}.
$$

Then

$$
P(\theta)|0\rangle=|0\rangle,
\qquad
P(\theta)|1\rangle=e^{i\theta}|1\rangle.
$$

The conjugate transpose is

$$
P(\theta)^\dagger=
\begin{pmatrix}
1&0\\0&e^{-i\theta}
\end{pmatrix}=P(-\theta).
$$

Therefore,

$$
P(\theta)^\dagger P(\theta)=I,
$$

so every phase gate is unitary.

### 15.8 S gate

Set $\theta=\pi/2$:

$$
S=P\left(\frac\pi2\right)
=
\begin{pmatrix}
1&0\\0&i
\end{pmatrix}.
$$

Important identities:

$$
S^2=Z,
\qquad
S^4=I.
$$

Thus, $S$ is a square root of $Z$.

### 15.9 T gate

Set $\theta=\pi/4$:

$$
T=P\left(\frac\pi4\right)
=
\begin{pmatrix}
1&0\\0&e^{i\pi/4}
\end{pmatrix}
=
\begin{pmatrix}
1&0\\0&\frac{1+i}{\sqrt2}
\end{pmatrix}.
$$

Important identities:

$$
T^2=S,
\qquad
T^4=Z,
\qquad
T^8=I.
$$

The T gate is especially important in fault-tolerant quantum computing and universal gate sets.

---

## 16. Computing Gate Actions

There are two standard methods.

### 16.1 Method 1: explicit matrix multiplication

Suppose

$$
|\psi\rangle=
\begin{pmatrix}
\alpha\\\beta
\end{pmatrix}.
$$

Then

$$
H|\psi\rangle
=
\frac1{\sqrt2}
\begin{pmatrix}
1&1\\1&-1
\end{pmatrix}
\begin{pmatrix}
\alpha\\\beta
\end{pmatrix}
=
\frac1{\sqrt2}
\begin{pmatrix}
\alpha+\beta\\
\alpha-\beta
\end{pmatrix}.
$$

In Dirac form,

$$
H|\psi\rangle
=
\frac{\alpha+\beta}{\sqrt2}|0\rangle
+
\frac{\alpha-\beta}{\sqrt2}|1\rangle.
$$

#### Example

For

$$
|\psi\rangle=
\frac{1+2i}{3}|0\rangle-\frac23|1\rangle,
$$

let

$$
\alpha=\frac{1+2i}{3},
\qquad
\beta=-\frac23.
$$

Then

$$
\alpha+\beta=\frac{-1+2i}{3},
$$

and

$$
\alpha-\beta=\frac{3+2i}{3}.
$$

Therefore,

$$
H|\psi\rangle
=
\frac{-1+2i}{3\sqrt2}|0\rangle
+
\frac{3+2i}{3\sqrt2}|1\rangle.
$$

Normalization is automatically preserved because $H$ is unitary.

### 16.2 Method 2: linearity in Dirac notation

Suppose

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle.
$$

For any linear operator $U$,

$$
U|\psi\rangle
=U(\alpha|0\rangle+\beta|1\rangle)
=\alpha U|0\rangle+\beta U|1\rangle.
$$

If the action of $U$ on basis states is known, the full action follows immediately.

#### Example: applying T to $|+\rangle$

$$
T|+\rangle
=
T\left(\frac{|0\rangle+|1\rangle}{\sqrt2}\right)
=
\frac1{\sqrt2}\left(T|0\rangle+T|1\rangle\right).
$$

Since

$$
T|0\rangle=|0\rangle,
\qquad
T|1\rangle=e^{i\pi/4}|1\rangle,
$$

we obtain

$$
T|+\rangle
=
\frac{|0\rangle+e^{i\pi/4}|1\rangle}{\sqrt2}.
$$

Now apply $H$:

$$
HT|+\rangle
=
\frac1{\sqrt2}
\left(
H|0\rangle+e^{i\pi/4}H|1\rangle
\right).
$$

Using

$$
H|0\rangle=\frac{|0\rangle+|1\rangle}{\sqrt2},
\qquad
H|1\rangle=\frac{|0\rangle-|1\rangle}{\sqrt2},
$$

we get

$$
HT|+\rangle
=
\frac12
\left[
(1+e^{i\pi/4})|0\rangle
+
(1-e^{i\pi/4})|1\rangle
\right].
$$

This example illustrates how relative phase later affects amplitudes through interference.

---

## 17. Composition of Unitary Operations

Suppose $U_1$ is applied first and $U_2$ second.

Starting from $|\psi\rangle$,

$$
|\psi_1\rangle=U_1|\psi\rangle,
$$

then

$$
|\psi_2\rangle=U_2|\psi_1\rangle
=U_2U_1|\psi\rangle.
$$

Thus, the combined operation is

$$
U_2U_1.
$$

For a sequence $U_1,U_2,\ldots,U_n$ applied in that chronological order,

$$
U_{\text{total}}=U_nU_{n-1}\cdots U_2U_1.
$$

### 17.1 Closure of unitary matrices

If $U$ and $V$ are unitary, then $VU$ is unitary:

$$
(VU)^\dagger(VU)
=U^\dagger V^\dagger VU
=U^\dagger IU
=U^\dagger U
=I.
$$

Therefore, any sequence of valid unitary operations is itself represented by a unitary matrix.

### 17.2 Noncommutativity

In general,

$$
UV\ne VU.
$$

Example:

$$
HZ\ne ZH.
$$

Indeed,

$$
HZ|0\rangle=H|0\rangle=|+\rangle,
$$

whereas

$$
ZH|0\rangle=Z|+\rangle=|-\rangle.
$$

Because $|+\rangle\ne|-\rangle$, the two operation orders differ.

---

## 18. The Square Root of NOT

Apply the following operations in order:

1. Hadamard $H$.
2. Phase gate $S$.
3. Hadamard $H$.

Because the first operation acts on the state first and therefore appears on the right, the total unitary is

$$
V=HSH.
$$

Using

$$
H=\frac1{\sqrt2}
\begin{pmatrix}
1&1\\1&-1
\end{pmatrix},
\qquad
S=
\begin{pmatrix}
1&0\\0&i
\end{pmatrix},
$$

we find

$$
V
=
\frac12
\begin{pmatrix}
1+i&1-i\\
1-i&1+i
\end{pmatrix}.
$$

Now square it:

$$
V^2=(HSH)(HSH).
$$

Since $H^2=I$,

$$
V^2=HS^2H.
$$

Because $S^2=Z$,

$$
V^2=HZH.
$$

And

$$
HZH=X.
$$

Therefore,

$$
V^2=X.
$$

So $V$ is a **square root of NOT**.

### 18.1 Why this is surprising classically

Could a real stochastic matrix $M$ satisfy

$$
M^2=X?
$$

No. A simple determinant argument proves this.

The classical NOT matrix has determinant

$$
\det(X)=-1.
$$

If $M$ were a real matrix satisfying $M^2=X$, then

$$
\det(M^2)=\det(X).
$$

But

$$
\det(M^2)=\det(M)^2\ge0
$$

for real $\det(M)$, while

$$
\det(X)=-1.
$$

This is impossible. Therefore, no real stochastic matrix—and in fact no real matrix at all—can square to the classical NOT matrix.

Complex amplitudes allow quantum theory to realize such an intermediate operation.

---

## 19. Classical–Quantum Comparison

| Feature | Classical probabilistic information | Pure-state quantum information |
|---|---|---|
| State space | Probability vectors | Normalized complex vectors |
| Entries | Nonnegative real probabilities | Complex amplitudes |
| Normalization | $\sum_a p(a)=1$ | $\sum_a\lvert\alpha_a\rvert^2=1$ |
| Definite basis state | $\lvert a\rangle$ | $\lvert a\rangle$ |
| Standard measurement outcome | $a$ with probability $p(a)$ | $a$ with probability $\lvert\alpha_a\rvert^2$ |
| Post-measurement state | $\lvert a\rangle$ after seeing $a$ | $\lvert a\rangle$ after obtaining $a$ |
| Allowed operation matrix | Column-stochastic matrix | Unitary matrix |
| Composition | Matrix multiplication | Matrix multiplication |
| First operation appears | Rightmost | Rightmost |
| Reversible operations | Permutation matrices | All unitaries |
| Interference | No amplitude interference | Yes, due to complex amplitudes |
| Relative phase | Not present | Physically meaningful |
| Noise in basic framework | Naturally included by stochasticity | Requires density matrices/channels for full treatment |

### 19.1 Similarities

Both theories use:

- Vectors to represent states.
- Matrices to represent operations.
- Matrix-vector multiplication to update states.
- Matrix multiplication to compose operations.
- Basis vectors for definite classical states.
- Probabilistic measurement outcomes.

### 19.2 Crucial differences

The key differences are:

- Classical uncertainty uses probabilities directly.
- Quantum states use amplitudes whose squared magnitudes become probabilities.
- Quantum amplitudes may be negative or complex.
- Relative phases allow constructive and destructive interference.
- Unitary evolution is reversible.
- A measurement generally disturbs a quantum state.

---

## 20. Common Misconceptions and Pitfalls

### 20.1 “An amplitude is a probability”

Incorrect. An amplitude may be negative or complex. The probability is its squared magnitude.

$$
\alpha_a \ne \Pr(a),
\qquad
\Pr(a)=|\alpha_a|^2.
$$

### 20.2 “A minus sign never matters because it disappears when squared”

A minus sign may disappear in one direct basis measurement, but it can affect later interference.

$$
|+\rangle
\quad\text{and}\quad
|-\rangle
$$

have identical standard-basis probabilities yet are perfectly distinguishable after a Hadamard gate.

### 20.3 “Every normalized vector is a probability vector”

No. Quantum normalization uses the sum of squared magnitudes. Classical normalization uses the sum of entries.

For example,

$$
\frac1{\sqrt2}
\begin{pmatrix}1\\-1\end{pmatrix}
$$

is a valid quantum state but not a probability vector because it has a negative entry and its entries do not sum to $1$.

### 20.4 “Matrix multiplication follows left-to-right time order”

No. If $U_1$ happens first and $U_2$ second, the total matrix is

$$
U_2U_1.
$$

Always apply the rightmost matrix first.

### 20.5 “All deterministic classical operations are reversible”

No. Constant functions are deterministic but irreversible. Only one-to-one deterministic functions correspond to permutation matrices and are reversible.

### 20.6 “Unitary means Hermitian”

Not necessarily.

- Unitary: $U^\dagger U=I$.
- Hermitian: $A^\dagger=A$.

Some matrices, such as $X,Y,Z,H$, are both. A general phase gate $P(\theta)$ is unitary but not Hermitian for arbitrary $\theta$.

### 20.7 “Measurement reveals the full state vector”

A single measurement returns one classical outcome, not the complete vector of amplitudes. Reconstructing an unknown state requires many identically prepared systems and multiple measurement settings; this process is called quantum-state tomography.

### 20.8 “The post-measurement state is always the original state”

Only if the original state was already the observed basis state. A superposition generally changes after measurement.

### 20.9 “Global and relative phase are equally observable”

No. Global phase is physically irrelevant, while relative phase affects interference and can be observed indirectly.

---

## 21. Key Results to Memorize

### Classical information

$$
p=\sum_a p(a)|a\rangle,
\qquad
p(a)\ge0,
\qquad
\sum_a p(a)=1.
$$

$$
M_f=\sum_a|f(a)\rangle\langle a|.
$$

A valid probabilistic operation is represented by a column-stochastic matrix.

If $M_1$ is applied first and $M_2$ second:

$$
M_{\text{total}}=M_2M_1.
$$

### Quantum information

$$
|\psi\rangle=\sum_a\alpha_a|a\rangle,
\qquad
\sum_a|\alpha_a|^2=1.
$$

$$
\Pr(a)=|\alpha_a|^2=|\langle a|\psi\rangle|^2.
$$

After observing $a$:

$$
|\psi\rangle\longmapsto|a\rangle.
$$

A matrix is unitary when

$$
U^\dagger U=UU^\dagger=I.
$$

Unitary evolution:

$$
|\psi\rangle\longmapsto U|\psi\rangle.
$$

Composition:

$$
U_{\text{total}}=U_n\cdots U_2U_1.
$$

Important gates:

$$
X=
\begin{pmatrix}0&1\\1&0\end{pmatrix},
\quad
Y=
\begin{pmatrix}0&-i\\i&0\end{pmatrix},
\quad
Z=
\begin{pmatrix}1&0\\0&-1\end{pmatrix},
$$

$$
H=\frac1{\sqrt2}
\begin{pmatrix}1&1\\1&-1\end{pmatrix},
$$

$$
S=
\begin{pmatrix}1&0\\0&i\end{pmatrix},
\qquad
T=
\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}.
$$

---

## 22. Worked Examples

### Example 1: Is a vector a valid probability vector?

Consider

$$
p=
\begin{pmatrix}
0.2\\0.5\\0.3
\end{pmatrix}.
$$

All entries are nonnegative and

$$
0.2+0.5+0.3=1.
$$

Therefore, it is a valid probability vector.

Now consider

$$
q=
\begin{pmatrix}
0.6\\0.6\\-0.2
\end{pmatrix}.
$$

Although the entries sum to $1$, one entry is negative. Therefore, it is not a probability vector.

### Example 2: Is a vector a valid quantum state?

Consider

$$
|\psi\rangle=
\frac12|0\rangle+
\frac{\sqrt3}{2}|1\rangle.
$$

Check normalization:

$$
\left|\frac12\right|^2+
\left|\frac{\sqrt3}{2}\right|^2
=
\frac14+\frac34=1.
$$

Therefore, it is a valid quantum state.

### Example 3: Normalize a vector

Consider

$$
v=
\begin{pmatrix}
1+i\\2
\end{pmatrix}.
$$

Its squared norm is

$$
\|v\|^2=|1+i|^2+|2|^2=2+4=6.
$$

Thus,

$$
\|v\|=\sqrt6.
$$

The normalized state is

$$
|\psi\rangle
=
\frac1{\sqrt6}
\begin{pmatrix}
1+i\\2
\end{pmatrix}.
$$

### Example 4: Measurement probabilities

For

$$
|\psi\rangle=
\frac1{\sqrt6}(1+i)|0\rangle+
\frac2{\sqrt6}|1\rangle,
$$

$$
\Pr(0)=\frac{|1+i|^2}{6}=\frac26=\frac13,
$$

$$
\Pr(1)=\frac{|2|^2}{6}=\frac46=\frac23.
$$

### Example 5: Classical deterministic operation in Dirac notation

Let $\Sigma=\{0,1,2\}$, and define

$$
f(0)=1,
\qquad
f(1)=1,
\qquad
f(2)=0.
$$

Then

$$
M_f
=|1\rangle\langle0|
+|1\rangle\langle1|
+|0\rangle\langle2|.
$$

In the ordered basis $(0,1,2)$,

$$
M_f=
\begin{pmatrix}
0&0&1\\
1&1&0\\
0&0&0
\end{pmatrix}.
$$

Each column contains exactly one $1$.

### Example 6: Verify unitarity of H

Because $H$ is real and symmetric,

$$
H^\dagger=H.
$$

Then

$$
H^\dagger H=H^2
=
\frac12
\begin{pmatrix}
1&1\\1&-1
\end{pmatrix}
\begin{pmatrix}
1&1\\1&-1
\end{pmatrix}
$$

$$
=
\frac12
\begin{pmatrix}
2&0\\0&2
\end{pmatrix}
=I.
$$

Therefore, $H$ is unitary.

### Example 7: Distinguishing plus and minus

A standard-basis measurement cannot distinguish $|+\rangle$ from $|-\rangle$, because both produce $0$ and $1$ with probability $1/2$.

Apply $H$:

$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle.
$$

Now a standard-basis measurement distinguishes them with certainty.

### Example 8: Order of gates

Apply $H$ then $Z$ to $|0\rangle$:

$$
ZH|0\rangle=Z|+\rangle=|-\rangle.
$$

Apply $Z$ then $H$:

$$
HZ|0\rangle=H|0\rangle=|+\rangle.
$$

The outputs differ, so

$$
ZH\ne HZ.
$$

---

## 23. Practice Problems

### Part A: Classical states and probability vectors

1. Let $\Sigma=\{a,b,c\}$. Write the standard basis vectors in the order $(a,b,c)$.
2. Determine whether each vector is a probability vector:
   - $(0.1,0.2,0.7)^T$
   - $(0.5,0.6,-0.1)^T$
   - $(1/2,1/4,1/8)^T$
3. A bit equals $0$ with probability $0.35$. Write its probability vector.
4. Apply the NOT matrix to the vector from Problem 3.

### Part B: Dirac notation

5. Compute $\langle0|1\rangle$, $\langle1|1\rangle$, and $|1\rangle\langle0|$.
6. Show that

   $$
   |0\rangle\langle0|+|1\rangle\langle1|=I.
   $$

7. Let

   $$
   |\psi\rangle=(2+i)|0\rangle-3i|1\rangle.
   $$

   Write $\langle\psi|$.

### Part C: Classical operations

8. Write the matrix for a function $f:\{0,1,2\}\to\{0,1,2\}$ satisfying

   $$
   f(0)=2,\quad f(1)=0,\quad f(2)=2.
   $$

9. Is the matrix

   $$
   M=
   \begin{pmatrix}
   0.6&0.2\\
   0.4&0.8
   \end{pmatrix}
   $$

   column-stochastic?

10. Let $C_0$ be constant zero and $X$ be NOT. Compute $XC_0$ and $C_0X$, and interpret both.

### Part D: Quantum states and measurement

11. Determine whether each vector is a valid qubit state:

   $$
   \frac1{\sqrt3}
   \begin{pmatrix}1\\1\end{pmatrix},
   \qquad
   \frac1{\sqrt5}
   \begin{pmatrix}1+ i\\\sqrt3\end{pmatrix}.
   $$

12. Normalize

   $$
   \begin{pmatrix}2\\1-i\end{pmatrix}.
   $$

13. For

   $$
   |\psi\rangle=\frac{i}{2}|0\rangle+\frac{\sqrt3}{2}|1\rangle,
   $$

   find the standard-basis measurement probabilities.

14. If the first measurement in Problem 13 gives outcome $0$, what is the state immediately afterward? What is the probability that an immediate repeated measurement gives $0$?

15. Explain why $|+\rangle$ and $|-\rangle$ cannot be distinguished by a single standard-basis measurement but can be distinguished after applying $H$.

### Part E: Gates and composition

16. Compute $X|+\rangle$.
17. Compute $Z|+\rangle$ and $Z|-\rangle$.
18. Verify that $S^2=Z$.
19. Compute $T^2$ and identify the resulting gate.
20. Starting from $|0\rangle$, apply $H$, then $T$, then $H$. Write the total operator in correct order and find the final state.
21. Prove that the product of two unitary matrices is unitary.
22. Show that $H X H=Z$ and $H Z H=X$.
23. Explain why no real matrix can satisfy $M^2=X$, where $X$ is the NOT matrix.

---

## 24. Solutions

### 1.

$$
|a\rangle=
\begin{pmatrix}1\\0\\0\end{pmatrix},
\quad
|b\rangle=
\begin{pmatrix}0\\1\\0\end{pmatrix},
\quad
|c\rangle=
\begin{pmatrix}0\\0\\1\end{pmatrix}.
$$

### 2.

- $(0.1,0.2,0.7)^T$: valid.
- $(0.5,0.6,-0.1)^T$: invalid because of the negative entry.
- $(1/2,1/4,1/8)^T$: invalid because the entries sum to $7/8$, not $1$.

### 3.

$$
p=
\begin{pmatrix}
0.35\\0.65
\end{pmatrix}.
$$

### 4.

$$
Xp=
\begin{pmatrix}
0&1\\1&0
\end{pmatrix}
\begin{pmatrix}
0.35\\0.65
\end{pmatrix}
=
\begin{pmatrix}
0.65\\0.35
\end{pmatrix}.
$$

### 5.

$$
\langle0|1\rangle=0,
\qquad
\langle1|1\rangle=1,
$$

$$
|1\rangle\langle0|
=
\begin{pmatrix}
0&0\\1&0
\end{pmatrix}.
$$

### 6.

$$
|0\rangle\langle0|
=
\begin{pmatrix}1&0\\0&0\end{pmatrix},
\quad
|1\rangle\langle1|
=
\begin{pmatrix}0&0\\0&1\end{pmatrix}.
$$

Adding gives $I$.

### 7.

Complex conjugate the coefficients and convert kets to bras:

$$
\langle\psi|=(2-i)\langle0|+3i\langle1|.
$$

### 8.

The columns are $|f(0)\rangle=|2\rangle$, $|f(1)\rangle=|0\rangle$, and $|f(2)\rangle=|2\rangle$:

$$
M_f=
\begin{pmatrix}
0&1&0\\
0&0&0\\
1&0&1
\end{pmatrix}.
$$

### 9.

Yes. All entries are nonnegative and each column sums to $1$.

### 10.

$$
XC_0=C_1,
$$

because setting a bit to zero and then flipping it always produces one.

$$
C_0X=C_0,
$$

because flipping first is irrelevant if the bit is then overwritten with zero.

### 11.

First vector:

$$
\frac13+\frac13=\frac23\ne1,
$$

so it is invalid.

Second vector:

$$
\frac{|1+i|^2}{5}+\frac{|\sqrt3|^2}{5}
=
\frac25+\frac35=1,
$$

so it is valid.

### 12.

The squared norm is

$$
|2|^2+|1-i|^2=4+2=6.
$$

The normalized state is

$$
\frac1{\sqrt6}
\begin{pmatrix}2\\1-i\end{pmatrix}.
$$

### 13.

$$
\Pr(0)=\left|\frac i2\right|^2=\frac14,
$$

$$
\Pr(1)=\left|\frac{\sqrt3}{2}\right|^2=\frac34.
$$

### 14.

The post-measurement state is $|0\rangle$. An immediate repeated standard-basis measurement gives $0$ with probability $1$.

### 15.

Both states have amplitudes of squared magnitude $1/2$ for $0$ and $1$, so a direct standard-basis measurement has the same distribution. However,

$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle,
$$

so applying $H$ converts the relative phase difference into a deterministic outcome difference.

### 16.

$$
X|+\rangle
=
\frac{X|0\rangle+X|1\rangle}{\sqrt2}
=
\frac{|1\rangle+|0\rangle}{\sqrt2}
=|+\rangle.
$$

### 17.

$$
Z|+\rangle=|-\rangle,
\qquad
Z|-\rangle=|+\rangle.
$$

### 18.

$$
S^2=
\begin{pmatrix}1&0\\0&i\end{pmatrix}^2
=
\begin{pmatrix}1&0\\0&i^2\end{pmatrix}
=
\begin{pmatrix}1&0\\0&-1\end{pmatrix}
=Z.
$$

### 19.

$$
T^2=
\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}^2
=
\begin{pmatrix}1&0\\0&e^{i\pi/2}\end{pmatrix}
=
S.
$$

### 20.

The total operator is

$$
HTH.
$$

Starting from $|0\rangle$:

$$
H|0\rangle=|+\rangle,
$$

$$
T|+\rangle=
\frac{|0\rangle+e^{i\pi/4}|1\rangle}{\sqrt2},
$$

so

$$
HTH|0\rangle
=
\frac12
\left[
(1+e^{i\pi/4})|0\rangle
+
(1-e^{i\pi/4})|1\rangle
\right].
$$

### 21.

Let $U$ and $V$ be unitary. Then

$$
(VU)^\dagger(VU)
=U^\dagger V^\dagger VU
=U^\dagger IU
=U^\dagger U
=I.
$$

Therefore, $VU$ is unitary.

### 22.

One method is direct multiplication. Another uses basis actions.

For $HXH$:

$$
HXH|0\rangle
=HX|+\rangle
=H|+\rangle
=|0\rangle,
$$

$$
HXH|1\rangle
=HX|-\rangle
=H(-|-\rangle)
=-|1\rangle.
$$

Thus, $HXH=Z$.

Similarly, $HZH=X$.

### 23.

Assume a real matrix $M$ satisfies $M^2=X$. Then

$$
\det(M)^2=\det(X)=-1.
$$

But $\det(M)^2\ge0$ for any real determinant, a contradiction. Therefore, no real matrix can square to $X$.

---

## 25. Where This Leads Next

The single-system framework develops naturally into the theory of multiple quantum systems.

The next major ideas are:

1. **Tensor products** for combining systems.
2. **Multi-qubit basis states**, such as $|00\rangle,|01\rangle,|10\rangle,|11\rangle$.
3. **Entanglement**, where the joint state cannot be decomposed into independent states of the components.
4. **Controlled gates**, such as CNOT.
5. **Quantum circuits** built from sequences of single- and multi-qubit gates.
6. **Quantum protocols**, involving separated individuals who exchange quantum and classical information.
7. **General measurements** and projective measurements.
8. **Density matrices and quantum channels** for noise and open-system dynamics.

The central pattern remains the same:

- Define valid states.
- Define valid operations.
- Define measurement rules.
- Use linear algebra to predict observable outcomes.

---

## Final Conceptual Summary

Classical and quantum information share a common linear-algebraic structure, but they differ in what their vectors mean.

A classical probability vector contains probabilities directly. A quantum state vector contains complex amplitudes. Squared amplitude magnitudes produce probabilities only when a measurement is performed. The complex signs and phases that are invisible in one measurement basis can influence future operations through interference.

Classical probabilistic operations are represented by stochastic matrices. Quantum closed-system operations are represented by unitary matrices. In both settings, operations act by matrix-vector multiplication and compose through matrix multiplication, with the earliest operation appearing on the right.

The quantum framework is more than a replacement of probabilities by complex numbers. Those complex amplitudes, together with unitary evolution and measurement, make possible phase-sensitive interference, reversible transformations, and operations such as the square root of NOT—phenomena that have no equivalent within ordinary real stochastic dynamics.
