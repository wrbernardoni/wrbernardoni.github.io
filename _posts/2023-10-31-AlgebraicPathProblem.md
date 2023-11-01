---
title: The Algebraic Path Problem
subtitle: Why Idempotent Semirings?
layout: post
topic: Semiring Geometry
---

Many problems are a hidden instance of what is called the <em>Algebraic Path Problem</em>, a method of turning combinatorial problems into solving linear equations over different "number systems". In this post we give motivating examples of the algebraic path problem, how to solve it, what an idempotent semiring is, and why we would want to talk about idempotent semirings.

---

# Motivating Examples: The Shortest Path Problem and Markov Process Terminal Probabilities

Consider the following two problems:

 * **The All-Pairs Shortest Path Problem**: Given a weighted graph $$G$$, what are the weights of the shortest paths between any two nodes $$i$$ and $$j$$
 * **Markov Process Terminal Probabilities**: Given a Markov chain, consider the stochastic process where at any time step you have an $$\alpha$$ probability to take a step as determined by the Markov chain, or a $$(1 - \alpha)$$ probability to terminate the process. What is the probability that if you begin at node $$i$$ you will terminate at node $$j$$?

Both of these problems can be represented as labelled directed graphs, such as in [Figure 1](#Fig1) and [Figure 2](#Fig2).

<div class="floatDiv">
<figure>
<a id="Fig1"></a>
<!-- https://q.uiver.app/#q=WzAsNCxbMCwxLCJcXHRleHRjaXJjbGVkezF9Il0sWzEsMCwiXFx0ZXh0Y2lyY2xlZHsyfSJdLFsyLDEsIlxcdGV4dGNpcmNsZWR7M30iXSxbMSwyLCJcXHRleHRjaXJjbGVkezR9Il0sWzMsMiwiMSIsMSx7InN0eWxlIjp7InRhaWwiOnsibmFtZSI6ImFycm93aGVhZCJ9fX1dLFswLDEsIjIiLDFdLFsxLDIsIjMiLDEseyJzdHlsZSI6eyJ0YWlsIjp7Im5hbWUiOiJhcnJvd2hlYWQifX19XSxbMywwLCIxIiwxXV0= -->
<iframe class="quiver-embed" src="https://q.uiver.app/#q=WzAsNCxbMCwxLCJcXHRleHRjaXJjbGVkezF9Il0sWzEsMCwiXFx0ZXh0Y2lyY2xlZHsyfSJdLFsyLDEsIlxcdGV4dGNpcmNsZWR7M30iXSxbMSwyLCJcXHRleHRjaXJjbGVkezR9Il0sWzMsMiwiMSIsMSx7InN0eWxlIjp7InRhaWwiOnsibmFtZSI6ImFycm93aGVhZCJ9fX1dLFswLDEsIjIiLDFdLFsxLDIsIjMiLDEseyJzdHlsZSI6eyJ0YWlsIjp7Im5hbWUiOiJhcnJvd2hlYWQifX19XSxbMywwLCIxIiwxXV0=&embed" width="200" height="200" style="border-radius: 8px; border: none;"></iframe>
<figcaption>Figure 1: A weighted graph</figcaption>
</figure>

<figure>
<a id="Fig2"></a>
<!-- https://q.uiver.app/#q=WzAsNCxbMCwxLCJcXHRleHRjaXJjbGVkezF9Il0sWzEsMCwiXFx0ZXh0Y2lyY2xlZHsyfSJdLFsyLDEsIlxcdGV4dGNpcmNsZWR7M30iXSxbMSwyLCJcXHRleHRjaXJjbGVkezR9Il0sWzMsMiwiMC4yIiwxLHsic3R5bGUiOnsidGFpbCI6eyJuYW1lIjoiYXJyb3doZWFkIn19fV0sWzAsMSwiMC40IiwxXSxbMSwyLCIwLjUiLDEseyJzdHlsZSI6eyJ0YWlsIjp7Im5hbWUiOiJhcnJvd2hlYWQifX19XSxbMywwLCIwLjEiLDFdXQ== -->
<iframe class="quiver-embed" src="https://q.uiver.app/#q=WzAsNCxbMCwxLCJcXHRleHRjaXJjbGVkezF9Il0sWzEsMCwiXFx0ZXh0Y2lyY2xlZHsyfSJdLFsyLDEsIlxcdGV4dGNpcmNsZWR7M30iXSxbMSwyLCJcXHRleHRjaXJjbGVkezR9Il0sWzMsMiwiMC4yIiwxLHsic3R5bGUiOnsidGFpbCI6eyJuYW1lIjoiYXJyb3doZWFkIn19fV0sWzAsMSwiMC40IiwxXSxbMSwyLCIwLjUiLDEseyJzdHlsZSI6eyJ0YWlsIjp7Im5hbWUiOiJhcnJvd2hlYWQifX19XSxbMywwLCIwLjEiLDFdXQ==&embed" width="200" height="200" style="border-radius: 8px; border: none;"></iframe>
<figcaption>Figure 2: A Markov chain</figcaption>
</figure>
</div>

One problem is the combinatorial problem of searching through potential paths in a network, and the other is finding some sort of steady state in a stochastic process, however we will show that both of these problems are a hidden instance of the *Algebraic Path Problem*.

## Terminal Probabilities in a Markov Chain

We can represent a Markov chain as a matrix $$A$$, with the entry $$A_{ij}$$ being the probability that we transition from state $$i$$ to state $$j$$, i.e.: $$A_{ij} = P(j \mid i, \text{1 step})$$. For the Markov chain in [Figure 2](#Fig2) that results in the matrix:

$$ A = \begin{bmatrix}
        0.6 & 0.4 & 0 & 0\\
        0 & 0.5 & 0.5 & 0\\
        0 & 0.5 & 0.3 & 0.2\\
        0.1 & 0 & 0.2 & 0.7
    \end{bmatrix} $$

The values on the diagonal come from the missing probability mass in the diagram. Node 1 has a 40% chance to move to node 2, and no other transition probabilities, so it must have a 60% chance of remaining stationary.

If we square this matrix, we get our "2 step probabilities":

$$
\begin{align}
    A^2_{ij} &= \sum_{k} A_{ik} * A_{kj}\\
    &= \sum_{k} P(k|i, \text{1 step}) * P(j | k, \text{1 step})\\
    &= P(j|i, \text{2 steps}) \label{eq: Markov 2 Step}
\end{align}
$$

So $$A^2_{ij}$$ is the probability of moving from node $$i$$ to node $$j$$ in *exactly* two steps.

For our example Markov chain we get:

$$
A^2 = \begin{bmatrix}
    0.36 & 0.44 & 0.2 & 0\\
    0 & 0.5 & 0.4 & 0.1\\
    0.02 & 0.4 & 0.38 & 0.2\\
    0.13 & 0.14 & 0.2 & 0.53
\end{bmatrix}
$$

We can induct on this, and we get that in general:

$$
A^n_{ij} = P(j|i, \text{n steps})
$$

If we want to find the probability that in our stochastic process we move from node $$i$$ to node $$j$$, we need to compute the following sum:

$$
P(j | i) = \sum_{n = 0}^\infty P(j | i, \text{n steps}) * P(\text{n steps})
$$

Our given process follows a binomial distribution for the number of steps, so we have:

$$
P(\text{n steps}) = \alpha^n * (1 - \alpha)
$$

Putting these pieces together we get:

$$
P(j|i) = \sum_{n = 0}^\infty A^n_{ij} * \alpha^n * (1 - \alpha)
$$

We can rewrite this to say that the probability that we will move from node $$i$$ to node $$j$$ will be the $$ij$$ component of the following matrix:

$$
(1 - \alpha)\sum_{n = 0}^\infty\left(\alpha A\right)^n
$$

For $$\alpha = 0.5$$ we get the following matrix for the Markov chain in [Figure 2](#Fig2):

$$
\begin{bmatrix}
         0.714983 & 0.211811 & 0.0634456 & 0.00976086 \\
 0.00244021 & 0.741337 & 0.22206 & 0.034163 \\
 0.00732064 & 0.224012 & 0.666179 & 0.102489 \\
 0.0561249 & 0.0507565 & 0.107369 & 0.785749
\end{bmatrix}
$$

This was an *explicit* process to solve the problem, but we could have taken another approach. We could have taken an *implicit* approach and noted that a matrix of probabilities $$X$$ is a solution to our problem if and only if it satisfies the implicit equation:

$$
X = \alpha A X + (1 - \alpha)I
$$

The above equation is the defining relation of a binomial distribution applied to our Markov chain.

We can "solve" this equation, and we get that if $$(I - \alpha A)$$ is an invertible matrix then the solution to oour equation is of the form

$$
X = (1 - \alpha)(I - \alpha A)^{-1}
$$

For the Markov chain in [Figure 2](#Fig2) and an arbitrary $$\alpha$$, one can verify as a lengthy linear algebra practice, that we get the following matrix:

<div class="floatDiv">
<figure class="nbPaper">
$$
\begin{bmatrix}
         \frac{(1-\alpha ) \left(0.09 \alpha ^3+0.42 \alpha ^2-1.5 \alpha +1\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.068 \alpha ^3-0.4 \alpha ^2+0.4 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.2 \alpha ^2-0.14 \alpha ^3\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{0.04 (1-\alpha ) \alpha ^3}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} \\
 \frac{0.01 (1-\alpha ) \alpha ^3}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(-0.102 \alpha ^3+0.77 \alpha ^2-1.6 \alpha +1\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.21 \alpha ^3-0.65 \alpha ^2+0.5 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.1 \alpha ^2-0.06 \alpha ^3\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} \\
 \frac{(1-\alpha ) \left(0.02 \alpha ^2-0.01 \alpha ^3\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.218 \alpha ^3-0.65 \alpha ^2+0.5 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(-0.21 \alpha ^3+1.07 \alpha ^2-1.8 \alpha +1\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.06 \alpha ^3-0.22 \alpha ^2+0.2 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} \\
 \frac{(1-\alpha ) \left(-0.01 \alpha ^3-0.08 \alpha ^2+0.1 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.14 \alpha ^2-0.072 \alpha ^3\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.08 \alpha ^3-0.22 \alpha ^2+0.2 \alpha \right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1} & \frac{(1-\alpha ) \left(0.06 \alpha ^3+0.38 \alpha ^2-1.4 \alpha +1\right)}{-0.058 \alpha ^4-0.162 \alpha ^3+1.32 \alpha ^2-2.1 \alpha +1}
    \end{bmatrix}
$$
</figure>
</div>

The key observation is that in order to solve this problem we can either compute the infinite sum:

$$
(1 - \alpha)\sum_{n = 0}^\infty\left(\alpha A\right)^n
$$

Or we can solve the linear relation:

$$
X = (1 - \alpha)(I - \alpha A)^{-1}
$$

## All Pairs Shortest Path Problem

We can create a similar adjacency matrix for the shortest path problem in [Figure 1](#Fig1):

$$
\begin{bmatrix}
    & 2 &  & \\
     &  & 3 & \\
     & 3 &  & 1\\
    1 &  & 1 & 
\end{bmatrix}
$$

We have a lot of missing edges we need to fill in in order to make a matrix. If we fill the missing edges with infinite weight edges then we won't change the length of our shortest paths -- the only paths that have infinite weight will be the ones that leave our original graph. Doing so we get the matrix:

$$
A = \begin{bmatrix}
        \infty & 2 & \infty & \infty\\
        \infty & \infty & 3 & \infty\\
        \infty & 3 & \infty & 1\\
        1 & \infty & 1 & \infty
    \end{bmatrix}
$$

For any graph $$G = (V,E)$$, if we have a weight function $$w : E \to \mathbb {R}$$, we can form the weighted adjacency matric $$A$$ where:

$$
A_{ij} = \begin{cases}
        w(i,j) & (i,j) \in E\\
        \infty & (i,j) \not\in E
    \end{cases}
$$

Using the convention that $$\min(\emptyset) = \infty$$, we can also see $$A$$ as the matrix where $$A^1_{ij}$$ is the minimum weights of the paths of length exactly $$1$$.

The minimum weights of the paths of exactly length $$2$$ between nodes $$i$$ and $$j$$ can be found as:

$$
\min_{k} A_{ik} + A_{kj}
$$

If we write $$a \oplus b = \min(a,b)$$ and $$a \otimes b = a + b$$ we can rewrite this equation as:

$$
\bigoplus_{k} A_{ik} \otimes A_{kj}
$$

This looks a lot like matrix multiplication! We will use this new "addition" $$\oplus$$ and "multiplication" $$\otimes$$ to define a new "matrix algebra".

We will define the matrix with the $$ij$$th entry being the lengths of the shortest paths in exactly 2 steps $$A^{\otimes 2}$$ and in general call $$A^{\otimes n}$$ the matrix:

$$
A^{\otimes n}_{ij} = \bigoplus_{k_1,k_2,...,k_{n-1}}A_{ik_1}\otimes A_{k_1k_2} \otimes...\otimes A_{k_{n-2}k_{n-1}}\otimes A_{k_{n-1}j}
$$

We define $$A^{\otimes 1} = A$$ and $$A^{\otimes 0}$$ to be the matrix:

$$
A^{\otimes 0}_{ij} = \begin{cases}
        0 & i = j\\
        \infty & i \ne j
    \end{cases}
$$

That is

$$
A^{\otimes 0} = \begin{bmatrix}
        0 & \infty & ... & \infty\\
        \infty & 0 & ... & \infty \\
        \vdots & & \ddots & \\
        \infty & & & 0
    \end{bmatrix}
$$

We will call $$A^{\otimes 0} = I_{\otimes}$$ in analogy with standard matrix multiplication.

We define $$\otimes$$ matrix multiplication as:

$$
(A \otimes B)_{ij} = \bigoplus_k A_{ik} \otimes B_{kj}
$$

Using this, one can see $$A^{\otimes n} \otimes A^{\otimes m} = A^{\otimes (n + m)}$$.

We can also define the $$\oplus$$ matrix addition as:

$$
(A \oplus B)_{ij} = A_{ij} \oplus B_{ij}
$$

We can see that $$A^{\otimes n}_{ij}$$ is the length of the shortest path in *exactly* $$n$$ steps between nodes $$i$$ and $$j$$. If we want the minimum weight path over any number of steps we want:

$$
\min_{n} A^{\otimes n}_{ij} = \bigoplus_{n = 0}^\infty A^{\otimes n}_{ij}
$$

Here we also get an implicit version of this equation, where a matrix $$X$$ solves the shortest path problem if and only if it satisfies the equation:

$$
X = (A \otimes X) \oplus I_{\otimes}
$$

For our example in [Figure 1](#Fig1) we get:

$$
\begin{align*}
    I_\otimes = A^{\otimes 0} &= \begin{bmatrix}
        0 & \infty & \infty & \infty\\
        \infty & 0 & \infty & \infty \\
        \infty & \infty & 0 & \infty\\
        \infty & \infty & \infty & 0
    \end{bmatrix}\\
    A = A^{\otimes 1} &= \begin{bmatrix}
        \infty & 2 & \infty & \infty\\
        \infty & \infty & 3 & \infty\\
        \infty & 3 & \infty & 1\\
        1 & \infty & 1 & \infty
    \end{bmatrix}\\
    A^{\otimes 2} &= \begin{bmatrix}
        \infty & \infty & 5 & \infty\\
        \infty & 6 & \infty & 4\\
        2 & \infty & 2 & \infty\\
        \infty & 3 & \infty & 2
    \end{bmatrix}\\
    \vdots\\
    \bigoplus_{n = 0}^\infty A^{\otimes n} &= \begin{bmatrix}
        0 & 2 & 5 & 6\\
        5 & 0 & 3 & 4\\
        2 & 3 & 0 & 1\\
        1 & 3 & 1 & 0
    \end{bmatrix}
\end{align*}
$$

# $$ X = AX + B $$ and the Kleene Star

Side by side the solutions for our two problems were:

$$
(1 - \alpha)\sum_{n = 0}^\infty \left(\alpha A\right)^n \qquad \bigoplus_{n = 0}^\infty A^{\otimes n}
$$

They both look like [geometric series](https://en.wikipedia.org/wiki/Geometric_series), and the implicit relation that they solved were of the form:

$$
\alpha A * X + (1 - \alpha)I \qquad (A \otimes X) \oplus I_{\otimes}
$$

If we let our idea of what $$*$$ and $$+$$ represent change, both problems could be reduced to solving a linear equation of the form

$$
X = A*X + B
$$

This is the **Algebraic Path Problem**. The algebraic path problem gives a common formulation to *many* types of problems, including finding:

 * Connected components in a graph
 * Shortest paths
 * Markov chain stationary distributions
 * Path enumeration
 * BGP routing
 * Expected cost of paths
 * Bridge and cut vertices
 * Minimum spanning trees
 * Widest path
 * Information propagation in trust networks
 * Traffic assignments
 * $$k$$-shortest paths
 * Approximate shortest paths
 * Time varying shortest paths
 * **[Contact Graph Routing/Routing in Space](https://arxiv.org/abs/2304.01150)**
 * and many more problems which can be found in the book [Path Problems in Networks, by John S. Baras and George Theodorakopoulos](https://link.springer.com/book/10.1007/978-3-031-79983-9).

To solve each of these problems we just need to find the right notion of multiplying and adding, and find a way to solve the equation $$X = AX + B$$.

If we were to naively try to solve this equation we could do the following:

$$
\begin{align*}
    X &= AX + B\\
    X - AX &= B\\
    (1 - A)X &= B\\
    X &= \frac{B}{1 - A}
\end{align*}
$$

But we run into a number of problems. We might not be able to compute $$\dfrac{B}{1 - A}$$. In our Markov chain example both $$A$$ and $$B$$ were matrices, and not all matrices are invertible, so we may not be able to dvide by $$1 - A$$. Even if we can divide by $$1- A$$ we would need to decide if we are *left* or *right* dividing by $$1 - A$$, because matrix multiplication is not commutative. Even worse, we might not even be able to *subtract*. In our shortest path example $$a \oplus b$$ was the minimum of $$a$$ and $$b$$ -- there is no inverse operation to finding the minimum of two numbers.

We need to find a method of solving $$X = AX + B$$ using *only addition and multiplication*. Our above solution is not worthless though. If we stare at it long enough we can note that it is the solution to the geometric series:

$$
\sum_{n = 0}^\infty A^n B
$$

We can see that an expression of the above form satisfies the equation

$$
X = AX + B
$$

To make writing our solutions easier we define the **Kleene Star**.

<div class="definition">
<strong>Definition:</strong> The <strong>Kleene Star</strong> of an element \(A\) is the following sum:

$$
A^* = 1 + A + A^2 + A^3 + ... = \sum_{n = 0}^\infty A^n
$$
</div>

When the Kleene star exists (which it does not always exist), assuming suitable axioms on our number system, we get that $$A^* B$$ is a solution to the equation:

$$
X = AX + B
$$

And $$B A^*$$ is a solution to the equation:

$$
X = XA + B
$$

If our number system has a non-commutative multiplication then these could represent distinct problems.

## "Number System"

What do we need in a "number system" to make our equation $$X = AX + B$$ make sense?

* We need to be able to even *write* $$X = AX + B$$, so we need a notion of a multiplication and an addition.
* We want $$A^* = \sum_{i = 0}^{\infty} A^i$$ to be a solution to $$X = AX + 1$$ which means:
	1. We need a $$1$$ -- we need a multiplicative identity.
	2. Our multiplication needs to distribute over addition - otherwise $$A^*$$ is not actually a solution to $$X = AX + 1$$.  
	\\\[ A A^* + 1 = A\left(\sum A^i\right) + 1 \\\]
	We need $$A\left(\sum A^i\right) = \sum A^{i + 1}$$ to make $$A^*$$ a solution. Strictly speaking to solve $$X = AX + B$$ we only need *left* distributivity and to solve $$X = XA + B$$ we only need *right* distributivity.
	3. We want our addition and our multiplication to be associative. We didn't use any brackets when defining the Kleene star, and non-associative operations are quite a hassle.
	4. We want our addition to be commutative. If it is not then the following four equations all could represent distinct problems with four distinct solutions instead of the two that we currently have:
	\\\[X = AX + 1 \qquad X = 1 + AX \qquad X = XA + 1 \qquad X = 1 + XA\\\]

Taken together these are almost the axioms of a [**semiring**](#SemiringDefinition) -- the only missing axioms are the existence of an additive identity $$0$$, and the annihilation axiom: $$0a = 0 = a0$$.

It is important to note that we don't *need* to make these assumptions. There are other settings in which we can solve the algebraic path problem. Semirings are a broad class of algebraic systems and have a somewhat developed theory and classification, which make them a good setting to work in.

# Semirings

{% include semiringDef.html %}

Semirings are often called **rigs** as they are "ri**n**gs without the **n**egatives." I avoid this term in general as I find it is too easy to misread as *ring*, and in the context I work in the difference between a semiring and a ring is very critical. There is a valid critique of the term semiring, as it implies that these number systems are "half-rings", however most of the semirings I work with cannot be realized as "half of a ring".

In the context of networking problems we can give specific meanings to our semirings. The underlying set $$R$$ represents the processes or actions that can occur in our network. Adding two elements $$a$$ and $$b$$ is akin to doing the processes in parallel -- hence the commutativity: doing $$a$$ at the same time as $$b$$ is the same as doing $$b$$ at the same time as $$a$$. Multiplying two elements is akin to doing them in series.

Semirings are a *broad* class of algebraic objects encompassing many different behaviors.

All rings are semirings.

<div class="definition">
<strong>Example 1.</strong> The real numbers \(\mathbb R\) under their regular operations \(+\) and \(*\) form a semiring \((\mathbb R, +, *, 0, 1)\):

$$
\begin{align*}
a +_{\mathbb R} b &= a + b\\
a *_{\mathbb R} b &= a * b
\end{align*}
$$
</div>

There are also semirings that behave in profoundly "unringly" ways.

<div class="definition">
<strong>Example 2.</strong> We can form another semiring from the real numbers, the <strong>tropical semiring</strong>.

We take as our set \(\mathbb T = \mathbb R \cup \{\infty\}\) and define

$$
\begin{align*}
a +_{\mathbb T} b &= \min(a, b)\\
a *_{\mathbb T} b &= a +_{\mathbb R} b
\end{align*}
$$

The tuple \((\mathbb R, +_{\mathbb T}, *_{\mathbb T}, 0_{\mathbb T} = \infty, 1_{\mathbb T} = 0_{\mathbb R})\) forms a semiring.

The tropical semiring has <a href="https://en.wikipedia.org/wiki/Tropical_geometry">a well developed algebro-geometric theory</a>, which is introduced in the texts: <a href="https://bookstore.ams.org/gsm-161">Introduction to Tropical Geometry</a>, <a href="https://link.springer.com/book/10.1007/978-3-0346-0048-4">Tropical Algebraic Geometry</a>, and <a href="https://press.princeton.edu/books/hardcover/9780691117638/max-plus-at-work">Max Plus at Work</a>.
</div>

There are multiple isomorphic formulations of the tropical semiring, the one above is also called the $$(\min,+)$$ tropical semiring, as its two operations are min and plus.

Semirings may also look *nothing* like numbers. Let's denote the power set of a set $$X$$ as $$P(X)$$, and $$P_{fin}(X)$$ to be the set of finite subsets of $$X$$.

$$
P_{fin}(X) = \{Y \subseteq X, |Y| \in \mathbb N\}
$$

<div class="definition">
<strong>Example 3.</strong> Let \(X\) be a set. We can form a semiring on \(P(X)\), where:

$$
\begin{align*}
A +_{P(X)} B &= A \cup B\\
A *_{P(X)} B &= A \cap B\\
0_{P(X)} &= \emptyset\\
1_{P(X)} &= X
\end{align*}
$$
</div>

<div class="definition">
<strong>Example 4.</strong> Let \(M\) be a monoid. We can form a semiring on \(P(M)\), where:

$$
\begin{align*}
A +_{P(M)} B &= A \cup B\\
A *_{P(M)} B &= \{a *_M b : a \in A, b \in B\}\\
0_{P(X)} &= \emptyset\\
1_{P(X)} &= \{1_M\}
\end{align*}
$$
</div>

For both of the previous examples we can also form the **finite power set semiring** where our base set is $$P_{fin}(X) \cup \{X\}$$, and our operations are identical. In this case our elements are either finite subsets of our set/monoid or the whole set/monoid.

<div class="definition">
<strong>Example 5.</strong> Let \(\mathbb B = \{\bot, \top\}\) where \(\bot\) represents the logical false and \(\top\) is the logical true. We can form the <strong>boolean semiring</strong> as follows

$$
\begin{align*}
a +_{\mathbb B} b &= a \textbf{ OR } b\\
a *_{\mathbb B} b &= a \textbf{ AND } b\\
0_{\mathbb B} &= \bot\\
1_{\mathbb B} &= \top
\end{align*}
$$
</div>

In the same sense that $$\mathbb Z$$ is the foundational ring, $$\mathbb B$$ is the foundational *idempotent semiring*.

Semirings have a rich hierarchy. One primary division is between *dioids* and *rings*. If we require that we also have a subtraction to go along with our addition we end up in the realm of rings. However if we require that addition gives us a natural notion of a number being "bigger" than another then we are in the real of dioids. Dioids give a natural setting to talk about optimization; as routing and networking decisions are optimization problems we will find that we naturally stray into the realm of dioids.

We can get more specific. For most of our networking problems we can require that we work with a specific and very nice subclass of dioids called *idempotent semirings*.

A great resource on the theory of semirings and dioids is the text [Graphs, Dioids and Semirings by Michel Gondran and Michel Minoux](https://link.springer.com/book/10.1007/978-0-387-75450-5).

<div class="floatDiv">
<figure>
<!-- https://q.uiver.app/#q=WzAsNSxbMiwwLCJcXHRleHR7RG91Ymx5IE1vbm9pZCdkIFNldH0iXSxbMiwxLCJcXHRleHR7U2VtaXJpbmd9Il0sWzAsMiwiXFx0ZXh0e0Rpb2lkfSJdLFs0LDIsIlxcdGV4dHtSaW5nfSJdLFswLDMsIlxcdGV4dHtJZGVtcG90ZW50IFNlbWlyaW5nfSJdLFswLDEsIlxcdGV4dHtEaXN0cmlidXRpdmUgTGF3c30iLDFdLFsxLDIsIlxcdGV4dHtOYXR1cmFsIG9yZGVyIGlzIGEgcGFydGlhbCBvcmRlcn0iLDFdLFsxLDMsIlxcdGV4dHtBZGRpdGlvbiBpcyBhIGdyb3VwfSIsMV0sWzIsNCwiYSArIGEgPSBhIiwxXV0= -->
<iframe class="quiver-embed" src="https://q.uiver.app/#q=WzAsNSxbMiwwLCJcXHRleHR7RG91Ymx5IE1vbm9pZCdkIFNldH0iXSxbMiwxLCJcXHRleHR7U2VtaXJpbmd9Il0sWzAsMiwiXFx0ZXh0e0Rpb2lkfSJdLFs0LDIsIlxcdGV4dHtSaW5nfSJdLFswLDMsIlxcdGV4dHtJZGVtcG90ZW50IFNlbWlyaW5nfSJdLFswLDEsIlxcdGV4dHtEaXN0cmlidXRpdmUgTGF3c30iLDFdLFsxLDIsIlxcdGV4dHtOYXR1cmFsIG9yZGVyIGlzIGEgcGFydGlhbCBvcmRlcn0iLDFdLFsxLDMsIlxcdGV4dHtBZGRpdGlvbiBpcyBhIGdyb3VwfSIsMV0sWzIsNCwiYSArIGEgPSBhIiwxXV0=&embed" width="600" height="230" style="border-radius: 8px; border: none;"></iframe>
<figcaption>Figure 3. Varieties of Doubly Monoidal Sets</figcaption>
</figure>
</div>

## Idempotency

<div class="definition">
<strong>Definition:</strong> A semiring \(R\) is called <strong>idempotent</strong> if for all \(a \in R: a + a = a\).
</div>

Idempotent semirings are closely related to [quantales](https://en.wikipedia.org/wiki/Quantale), which are important constructions in analysis, category theory, and ring theory.

Idempotency is a natural restriction to impose for routing probllems. If addition represents objects being in parallel, then "being able to send a message from node \(a\) to node \(b\)" at the same time as "being able to send a message from node \(a\) to node \(b\)" is just "being able to send a message from node \(a\) to node \(b\)".

Examples 2, 3, 4, and 5 are canonical examples of idempotent semirings.

Idempotent semirings have a natural partial order given by their addition.

<div class="definition">
<strong>Definition:</strong> The <strong>canonical ordering</strong> on an idempotent semiring \(S\) is given by:

\[a \le b \iff a + b = a\]
</div>

This forms a partial order for any idempotent semiring. In fact it forms a lower semi-lattice. Under this ordering, if $$X$$ is a finite subset of our semiring we get:

$$
\inf(X) = \sum_{x \in X} x
$$

This can be remembered by the phrase: "The crocodile eats the bigger element." If you picture $$\le$$ as a little crocodile, then $$a \le b$$ if when we add $$a$$ and $$b$$, $$b$$ is "eaten": $$a + b = a$$.

Some authors prefer the flipped order where crocodiles prefer smaller prey. The choice of order only really affects if you write $$\inf$$s or $$\sup$$s in your equations. Our ordering lines up well with the tropical min-plus semiring (example 2), and makes the corresponding path problems "minimum path problems". When dealing with a class of objects called dioids, or with idempotent semirings which behave more like the power set semiring, the reverse choice of ordering can be more natural.

<div class="definition">
<strong>Definition:</strong> If the canonical ordering on an idempotent semiring is a total order, we say that the semiring is a <strong>totally ordered semiring.</strong>
</div>

<div class="definition">
<strong>Example 6.</strong>
<ul>
	<li>The tropical semiring \(\mathbb T\) is a totally ordered semiring as \(a + b \in \{a,b\}\). Here we get
	$$
	a \le b \iff a = a + b = \min(a,b)
	$$
	</li>
	<li>
		If \(X\) is a set or monoid and we examine the induced order on \(P(X)\) we get:
		$$
		a \le b \iff a = a + b = a \cup b
		$$
		That is, \(a \le b\) if and only if \(a \supseteq b\).  
		If \(|X| > 1\) then \(P(X)\) is <em>not</em> totally ordered.
	</li>
	<li>
		\(\mathbb B\) is totally ordered with \(\top < \bot\).
	</li>
</ul>
</div>

Equipped with this ordering, you can view addition in an idempotent semiring in the context of a decision problem. $$a + b$$ is the question of what route do you take if you have options $$a$$ and $$b$$. If you want the minimal path you take the smaller one, so you get $$a + b = \inf\{a,b\}$$. In the context of a non-total order, this is taking the common refinement of $$a$$ and $$b$$ -- depending on the context in $$a + b$$ you may use resources available in $$a$$ or resources available in $$b$$ or both.

## Matrix Semirings

<div class="definition">
<strong>Definition:</strong> Let \(X\) be a finite set and \((S, +, *, 0_S, 1_S)\) a semiring. We say that the <strong>matrix semiring over \(X\) with coefficients in \(S\)</strong>, denoted \(M_X(S)\) is the set of functions:

$$
X \times X \to S
$$

equipped with the binary operations:

$$
\begin{align}
\left(A +_{M_X(S)} B\right)(i,j) &= A(i,j) +_S B(i,j)\\
\left(A *_{M_X(S)} B\right)(i,j) &= \sum_{k \in X}A(i,k) *_S B(k,j)\\
\end{align}
$$

and distinguished elements:

$$
\begin{align}
0_{M_X(S)}(i,j) &= 0_S\\
1_{M_X(S)}(i,j) &= \begin{cases}1_S & i = j\\ 0_S & i \ne j\end{cases}
\end{align}
$$
</div>

A key thing to note here is that if $$S$$ is a **complete** semiring, meaning that it is closed under infinite sums, then you do not need to require that $$X$$ is finite. Completeness is impossible in many ring-like settings, but it is common among idempotent semirings - in fact any non-complete idempotent semiring can be extended to be complete. So while infinite matrices are rare and require additional assumptions in the context of rings, in the context of idempotent semirings infinite matrices can be dealt with freely.

The structure of $$M_X(S)$$ is highly dependent on the structure of $$S$$.

<div class="definition">
<strong>Proposition.</strong>
<ul>
	<li> If \(S\) is a semiring then \(M_X(S)\) is a semiring.
	</li>
	<li>
		If \(S\) is an idempotent semiring then \(M_X(S)\) is an idempotent semiring.
	</li>
</ul>
</div>

Elements of $$M_X(S)$$ can be referred to using matrix notation if we fix an indexing on $$X$$. For $$A \in M_X(S)$$ we write $$A_{ij}$$ to refer to $$A(i,j)$$ and we can write out elements in matrix notation:

$$
A = \begin{bmatrix}
        A_{00} & A_{01} & A_{02} & ...\\
        A_{10} & A_{11} & A_{12} & ...\\
        A_{20} & A_{21} & A_{22} & ...\\
        \vdots & \vdots & \vdots &\ddots
    \end{bmatrix}
$$

In this form we get

$$
\begin{align*}
0_{M_X(S)} &= \begin{bmatrix}
        0_S & 0_S & 0_S & ...\\
        0_S & 0_S & 0_S & ...\\
        0_S & 0_S & 0_S & ...\\
        \vdots & \vdots & \vdots &\ddots
    \end{bmatrix}\\
1_{M_X(S)} &= \begin{bmatrix}
        1_S & 0_S & 0_S & ...\\
        0_S & 1_S & 0_S & ...\\
        0_S & 0_S & 1_S & ...\\
        \vdots & \vdots & \vdots &\ddots
    \end{bmatrix} 
\end{align*}
$$

Matrix multiplication is the usual dot product of row and column that you may have seen in a linear algebra class.

A matrix $$A \in M_X(S)$$ can be seen as a directed graph weighted in $$S$$, with a set of nodes $$X$$ and where the edge from node $$i$$ to node $$j$$ has weight $$A_{ij}$$. We say that $$A$$ is the **weighted adjacency matrix** of the corresponding directed graph.

When interpreted as a directed graph we can get an important description of the Kleene star of a matrix.

<div class="definition">
<p><strong>Proposition.</strong> Let \(A \in M_X(S)\), and let \(S\) be a complete semiring (closed under infinite sums).</p>

<p>Let \(P_{ij}\) be the set of paths between nodes \(i\) and \(j\) in the graph described by \(A\).</p>

<p>For a path \(p = (a_1 a_2)(a_2 a_3)...(a_{n-1} a_n)\) with \(a_1 = i, a_n = j\) we define the weight \(\omega(p)\) to be the product in \(S\) of its edge weights:

$$
\omega(p) = \prod_{i = 1}^n A_{a_i a_{i+1}}
$$</p>

<p>
	The Kleene star of \(A\), \(A^*\), can be written as follows:

$$
A^*_{ij} = \sum_{p \in P_{ij}} \omega(p)
$$
</p>

<p>
	<strong>i.e. the \(ij\)th entry of the Kleene star of a matrix is the sum of the weights of all paths between \(i\) and \(j\)</strong>
</p>
</div>

If $$S$$ is an idempotent semiring, this proposition tells us

$$
A^*_{ij} = \inf(\omega(P_{ij}))
$$

Where the infinum is with respect to the canonical ordering on $$S$$.

**i.e. the $$ij$$th element of the Kleene star over an idempotent semiring is the infinum of the weights of all paths between $$i$$ and $$j$$.**


## Idempotent Semirings and the Kleene Star

From this corollary we get that solving these minimal weight path problems is the same as finding the Kleene star of a matrix. So we want some tricks to make the computation easier.

<div class="definition">
<strong>Definition.</strong> Let \(S\) be an idempotent semiring and let \(s \in S\). We define the <strong>\(n\)th cumulant of \(s\)</strong>, denoted \(s^{(n)}\) to be the element

$$
s^{(n)} = 1 + s + s^2 + ... + s^n
$$
</div>

There is a handy trick to computing cumulants. In an idempotent semiring we have the identity

$$
(1 + s)^n = 1 + s + s^2 + ... + s^n = s^{(n)}
$$

So as a hand-wavey statement we can say that computing $$s^*$$ is the "same" as computing $$\lim_{n \to \infty}(1 + s)^n$$. In many cases this limit converges in finite time, we call that the Kleene star "stabilizing".

<div class="definition"><strong>Definition.</strong> Let \(S\) be an idempotent semiring and let \(s \in S\). We say that <strong>\(s\) stabilizes at \(n\)</strong> if \(n\) is the least natural number such that \(s^n = s^{n+1}\).
</div>

<div class="definition"><strong>Proposition.</strong> If \(s\) stabilizes at \(n\) then \(s^* = s^{(n)}\).
</div>

So to solve these path problems in an idempotent semiring, we define the adjacency matrix $$A$$, we compute $$1_{M_X(S)} + A$$, and then we keep exponentiating $$1_{M_X(S)} + A$$ until it stops changing. The entries of the resulting matrix will be the "shortest path weights attainable" for our given problem.

---

Now that we know about the algebraic path problem, and what a semiring is, let's see how to use this idea to [mathematically model deep space satellite networks and create algorithms to analyse and determine routing protocols](/2023/11/01/AlgebraicContactGraph.html).