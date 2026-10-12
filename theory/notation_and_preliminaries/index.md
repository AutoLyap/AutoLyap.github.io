Canonical HTML: https://autolyap.github.io/theory/notation_and_preliminaries/
Documentation version: 0.2.1

<a id="notation-and-preliminaries"></a>



# 1. Notation and preliminaries

Let \(\naturals\) denote the set of nonnegative integers,
\(\mathbb{N}\) the set of positive integers, and \(\mathbb{Z}\)
the set of integers. For
\(n,m \in \mathbb{Z}\cup\set{\pm\infty}\), we define


\[\llbracket n,m \rrbracket = \set{l \in \mathbb{Z} \xmiddle\vert n \leq l \leq m}.\]

We use \(\reals\), \(\reals_+\), and \(\reals_{++}\) for
the real, nonnegative real, and positive real numbers, respectively. We
write \(\reals^n\) for the set of all \(n\)-tuples in
\(\reals\), \(\reals^{m\times n}\) for real-valued matrices of
size \(m\times n\), and \([M]_{i,j}\) for the \((i,j)\)-th
element of \(M\in\reals^{m\times n}\). We denote by
\(\sym^{n}\) the set of symmetric real-valued matrices of size
\(n\times n\), and by \(\sym_+^n\subseteq \sym^{n}\) the set of
positive semidefinite ones.

The matrix \(0_{n\times m} \in \reals^{n\times m}\) is the
\(n\times m\) all-zero matrix, \(I_{n} \in \reals^{n\times n}\)
is the identity matrix, \(e^n_{i}\in \reals^{n}\) is the
\(i\)th standard basis vector in \(\reals^{n}\), and
\(\mathbf{1}_{n}\in \reals^{n}\) is the all-ones vector. All
vectors in \(\reals^n\) are column vectors by convention.

Throughout this documentation, \((\calH,\inner{\cdot}{\cdot})\) will denote
a real Hilbert space. All norms \(\norm{\cdot}\) are canonical norms
where the inner product will be clear from the context. We denote the
identity mapping \(x \mapsto x\) on \(\calH\) by \(\Id\).

<a id="def-func-defs"></a>



Let
\(f\colon \calH \to \reals \cup \{\pm\infty\}\),
\(L\in\reals_{+}\) and
\(\mu,\widetilde{\mu},\mu_{\textup{gd}}\in\reals_{++}\). The
function \(f\) is said to be

1. *proper* if \(-\infty \notin f\p{\calH}\) and
   \(\dom f \neq \emptyset\), where


\[\dom f = \{x\in\calH \mid f(x) < + \infty\}\]

   is called the *effective domain* of \(f\),
2. *lower semicontinuous* if


\[\liminf_{y\to x} f\p{y} \geq f\p{x}\]

   for each \(x\in\calH\),
3. *convex* if


\[f\p{\p{1 - \lambda} x + \lambda y} \leq \p{1 - \lambda} f\p{x} + \lambda f\p{y}\]

   for each \(x, y \in \calH\) and
   \(0 \leq \lambda \leq 1\),
4. \(\mu\)-strongly convex if \(f\) is proper and
   \(f-(\mu/2)\norm{\cdot}^{2}\) is convex,
5. \(\widetilde{\mu}\)-weakly convex if
   \(f+(\widetilde{\mu}/2)\norm{\cdot}^{2}\) is convex,
6. \(L\)-smooth if \(f\) is Fréchet differentiable and
   the gradient \(\nabla f:\calH \to \calH\) is
   \(L\)-Lipschitz continuous, i.e.,


\[\norm{\nabla f(x) - \nabla f(y)} \leq L \norm{x - y}\]

   for each \(x,y\in\calH\), and
7. \(\mu_{\textup{gd}}\)-gradient dominated if \(f\) is
   Fréchet differentiable and

   <a id="equation-eq-pl"></a>

   <a id="eq-pl"></a>



(1.1)\[\begin{aligned}
            f\p{x} - \inf_{y\in\calH}f\p{y} \leq \frac{1}{2\mu_{\textup{gd}}}\norm{\nabla f(x)}^2
        \end{aligned}\]

   for each \(x \in \calH\). Inequality [(1.1)](https://autolyap.github.io/theory/notation_and_preliminaries/#equation-eq-pl)
   is sometimes called the *Polyak–Łojasiewicz* inequality or
   simply the *Łojasiewicz* inequality.

<a id="def-calf"></a>



Let \(-\infty < \mu \leq L \leq +\infty\) such that
\(L\geq 0\). We let \(\mathcal{F}_{\mu,L}\p{\mathcal{H}}\)
denote the class of all proper and lower semicontinuous functions
\(f:\calH\rightarrow\reals\cup\{\pm\infty\}\) such that

1. \((L/2)\norm{\cdot}^{2} - f\) is convex and \(f\) is
   Fréchet differentiable if \(L < +\infty\), and
2. \(f-(\mu/2)\norm{\cdot}^{2}\) is convex.

For example, \(\mathcal{F}_{-L,L}\p{\mathcal{H}}\) is equal to the
class of \(L\)-smooth functions with domain \(\calH\).

The *Fréchet subdifferential* of a function
\(f\colon \calH \to \reals \cup \{\pm\infty\}\) is the set-valued
operator \(\partial f:\calH \rightrightarrows \calH\) given by


\[\begin{split}\begin{aligned}
    \partial f \p{x} =
    \begin{cases}
        \Bigset{u\in\calH \xmiddle| \displaystyle{\liminf_{y\rightarrow x}} \frac{f\p{y}-f\p{x} - \inner{u}{y-x} }{\norm{y-x}}\geq 0 } & \text{if } \abs{f(x)} < + \infty, \\
        \emptyset & \text{otherwise}
    \end{cases}\end{aligned}\end{split}\]

for each \(x\in\calH\).

1. If \(f\) is Fréchet differentiable at a point
   \(x\in\calH\), then


\[\partial f \p{x} = \set{\nabla f(x)}\]

   at that point.
2. If \(f\) is proper and convex, the Fréchet subdifferential
   becomes the *convex subdifferential*, i.e.,


\[\partial f \p{x} = \set{u \in \calH \xmiddle\vert \forall y \in \calH,\, f(y) \geq f(x) + \inner{u}{y-x} }\]

   for each \(x \in \calH\).
3. If \(f\) is proper and \(\widetilde{\mu}\)-weakly convex
   for some \(\widetilde{\mu}\in\reals_{++}\), then


\[\partial f(x) = \partial \p{f + (\widetilde{\mu}/2)\norm{\cdot}^{2}}\p{x} - \widetilde{\mu} x\]

   for each \(x\in\mathcal{H}\),
   where \(\partial \p{f + (\widetilde{\mu}/2)\norm{\cdot}^{2}}\)
   reduces to the convex subdifferential.

<a id="def-prox"></a>



Suppose that
\(f\in\mathcal{F}_{0,\infty}\p{\mathcal{H}}\) and
\(\gamma\in\reals_{++}\). Then the *proximal operator* of
\(f\) with *step size* \(\gamma\), denoted
\(\prox_{\gamma f} : \calH \to \calH\), is defined as the
single-valued operator given by


\[\begin{aligned}
        \prox_{\gamma f}(x) = \argmin_{z\in\calH}\Bigp{f(z) + \frac{1}{2\gamma}\norm{x-z}^2}
    \end{aligned}\]

for each \(x\in\calH\).

Suppose that \(f\in\mathcal{F}_{0,\infty}\p{\mathcal{H}}\) and
\(\gamma\in\reals_{++}\). If \(x,p\in\calH\), then


\[p = \prox_{\gamma f}(x) \Leftrightarrow \gamma^{-1}\p{x-p} \in \partial f (p).\]

Moreover, the *conjugate* of \(f\), denoted
\(f^{*}:\calH\to \reals \cup \{+\infty\}\), is the proper, lower
semicontinuous and convex function given by


\[f^{*}(u) = \sup_{x\in\calH}\p{\inner{u}{x} - f(x)}\]

for each \(u\in\calH\). If \(x,u\in\calH\), then


\[u \in \partial f(x) \Leftrightarrow x \in \partial f^{*}(u).\]

Let \(G:\calH\rightrightarrows\calH\) be a set-valued operator. The
set of *zeros* of \(G\) is denoted by


\[\zer G =\set{x\in\calH \xmiddle| 0 \in G\p{x} }\]

and the *graph* of \(G\) is denoted by


\[\gra G = \set{(x,y)\in\calH\times\calH \xmiddle| y\in G(x)}.\]

<a id="def-set-op"></a>



Let \(G:\calH\rightrightarrows\calH\) and
\(\mu \in\reals_{++}\). The operator \(G\) is said to be

1. *monotone* if


\[\inner{u - v}{x-y}\geq 0\]

   for each \((x,u),(y,v)\in\gra G\),
2. *maximally monotone* if \(G\) is monotone and there does
   not exist a monotone operator
   \(H:\calH\rightrightarrows\calH\) such that
   \(\gra G \subsetneq \gra H\), and
3. \(\mu\)-*strongly monotone* if


\[\inner{u - v}{x-y}\geq \mu \norm{x-y}^{2}\]

   for each \((x,u),(y,v)\in\gra G\).

The *inverse* of a set-valued operator
\(G:\calH\rightrightarrows\calH\), denoted by
\(G^{-1}:\calH\rightrightarrows\calH\), is defined through its graph


\[\gra G^{-1} = \set{(y,x)\in\calH\times\calH \xmiddle| \p{x, y} \in \gra G}.\]

<a id="def-resolvent"></a>



Suppose that \(G:\calH \rightrightarrows\calH\)
is maximally monotone and \(\gamma \in \reals_{++}\). The
*resolvent* of \(G\) with *step size* \(\gamma\), denoted
\(J_{\gamma G}:\calH \to \calH\), is defined by


\[\begin{aligned}
        \p{\Id + \gamma G}^{-1}\p{x} = \set{J_{\gamma G} \p{x}}
    \end{aligned}\]

for each \(x\in \calH\), since \(\p{\Id + \gamma G}^{-1}\) is
singleton-valued in this case.

<a id="def-single-op"></a>



Let \(G:\calH \to \calH\),
\(L\in\reals_{+}\), and \(\beta\in\reals_{++}\). The operator
\(G\) is said to be

1. \(L\)-*Lipschitz continuous* if


\[\norm{G\p{x} - G\p{y} } \leq L \norm{x-y}\]

   for each \(x,y\in \calH\), and
2. \(\beta\)-*cocoercive* if


\[\inner{G\p{x} - G\p{y} }{x-y} \geq \beta \norm{G\p{x} - G\p{y}}^{2}\]

   for each \(x,y\in \calH\).

We introduce the following conventions that enable us to treat
single-valued and singleton-valued operators interchangeably.

1. For notational convenience (at the expense of a slight abuse of
   notation), we will sometimes identify the operator


\[G:\calH\to\calH\]

   with the set-valued mapping


\[\calH \ni x \mapsto \set{G\p{x}} \subseteq\calH,\]

   which will be clear from context. For example, if
   \(x,y \in \calH\), the inclusion/equality pair


\[y\in G\p{x}
\quad\Longleftrightarrow\quad
y = G\p{x}\]

   is used interchangeably.
2. Similarly, if


\[G:\calH\rightrightarrows \calH,
\qquad
T: \calH \to \calH,
\qquad
G\p{x} = \set{T\p{x}}\]

   for each \(x\in\calH\), i.e., \(G\) is a singleton-valued
   operator, we will sometimes identify \(G\) with the
   corresponding single-valued operator \(T\).

Given any positive integer \(n\), we let the inner-product
\(\inner{\cdot}{\cdot}\) on \(\calH^{n}\) be given by


\[\begin{aligned}
    \inner{\bz^{1}}{\bz^{2}}=\sum_{j=1}^{n}\inner{z^{1}_{j}}{z^{2}_{j}}    \end{aligned}\]

for each \(\bz^{i}=\p{z^{i}_{1},\ldots,z^{i}_{n}}\in\calH^{n}\) and
\(i\in\llbracket 1,2\rrbracket\). If
\(M\in \reals^{m\times n}\), we define the tensor product
\(M\kron \Id\) to be the mapping
\((M\kron \Id):\calH^{n}\rightarrow\calH^{m}\) such that


\[\begin{aligned}
  (M\kron\Id)\bz = \Bigp{\sum_{j=1}^n[M]_{1,j}z_{j},\ldots,\sum_{j=1}^n[M]_{m,j}z_{j}}
\end{aligned}\]

for each \(\bz=\p{z_{1},\ldots,z_{n}}\in\calH^n\). The adjoint and
composition formulas are


\[(M\kron\Id)^*=M^{\top}\kron\Id,
\qquad
(M\kron\Id)\circ(N\kron\Id)=(MN)\kron\Id\]

for each \(N\in\reals^{n\times l}\).

If we let \(M_{1}\in\reals^{m\times n_{1}}\) and
\(M_{2}\in\reals^{m\times n_{2}}\), the relations above imply that


\[\inner{(M_1\kron\Id)\bz^{1}}{(M_2\kron\Id)\bz^{2}}
=
\inner{\bz^{1}}{\p{\p{M_1^{\top}M_2}\kron\Id}\bz^{2}}\]

for each \(\bz^{1}\in\calH^{n_1}\) and \(\bz^{2}\in\calH^{n_2}\).
We define the mapping \(\mathcal{Q}:\sym^{n}\times \calH^{n}
\rightarrow \reals\) by


\[\quadform{M}{\bz}=\inner{\bz}{(M\kron\Id)\bz}\]

for each \(M\in\sym^{n}\) and \(\bz\in\calH^{n}\). Note that, if
\(M\in\sym^{n}\), \(N\in\reals^{n\times m}\) and
\(\bz\in\calH^{m}\), then


\[\quadform{M}{(N\kron\Id)\bz}=\quadform{N^{\top}MN}{\bz}.\]

We define the *Gramian function*
\(\gramFunc:\mathcal{H}^n \to \sym^{n}_{+}\) such that


\[[\gramFunc\p{\bz}]_{i,j} = \inner{z_{i}}{z_{j}}\]

for each \(\bz=\p{z_1,\ldots,z_n} \in \mathcal{H}^{n}\). If
\(M\in\sym^{n}\) and \(\bz \in\mathcal{H}^{n}\), it holds that


\[\mathcal{Q}\p{M,\bz} = \trace\p{M \gramFunc\p{\bz}}.\]

TeX macro definitions used in this page:

```latex
\newcommand{\Bigp}[1]{\mathord{\left( #1 \right)}}
\newcommand{\Bigset}[1]{\mathord{\left\{ #1 \right\}}}
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\argmin}{\operatorname*{argmin}}
\newcommand{\bz}{\mathbf{z}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\dom}{\operatorname*{dom}}
\newcommand{\gra}{\operatorname*{gra}}
\newcommand{\gramFunc}{\mathtt{G}}
\newcommand{\inner}[2]{\langle #1, #2 \rangle}
\newcommand{\kron}{\otimes}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\norm}[1]{\lVert #1 \rVert}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\prox}{{\rm{prox}}}
\newcommand{\quadform}[2]{\mathcal{Q}\p{#1,#2}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
\newcommand{\sym}{\mathbb{S}}
\newcommand{\trace}{\mathrm{trace}}
\newcommand{\xmiddle}[1]{\;\middle#1\;}
\newcommand{\zer}{\operatorname*{zer}}
```
