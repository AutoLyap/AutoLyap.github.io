Canonical HTML: https://autolyap.github.io/theory/algorithm_representation/
Documentation version: 0.2.1

<a id="algorithm-representation"></a>



# 3. Algorithm representation

We consider first-order algorithms that solve
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion) and can be
represented as a discrete-time linear time‐varying system in state-space
form in feedback interconnection with the potentially nonlinear and
set-valued operators \((\partial f_{i})_{i\in\IndexFunc}\) and
\((G_{i})_{i\in\IndexOp}\) that define the problem.

Before presenting the algorithm representation, we introduce some
notation:

1. \(\NumFunc = \abs{\IndexFunc}\) denotes the number of
   functional components in
   [(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion);
2. \(\NumOp = \abs{\IndexOp}\) denotes the number of operator
   components in
   [(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion);
3. \(\NumEval_{i}\in\mathbb{N}\) denotes the number of
   evaluations of \(\partial f_{i}\) and \(G_{i}\) per
   iteration, for \(i\in\IndexFunc\) and \(i\in\IndexOp\),
   respectively;
4. \(\NumEvalFunc = \sum_{i\in\IndexFunc} \NumEval_{i}\) denotes
   the total number of subdifferential evaluations per iteration;
5. \(\NumEvalOp = \sum_{i\in\IndexOp} \NumEval_{i}\) denotes the
   total number of operator evaluations per iteration; and
6. \(\NumEval = \NumEvalFunc + \NumEvalOp\) denotes the combined
   total number of evaluations per iteration.

Since we consider algorithms that allow for multiple evaluations of
\((\partial f_{i})_{i\in\IndexFunc}\) and
\((G_{i})_{i\in\IndexOp}\) per iteration, we define
\(\bfcn_{i}:\calH^{\NumEval_{i}}\to\p{\reals\cup\{\pm\infty\}}^{\NumEval_{i}}\)
such that


\[\begin{split}\begin{aligned}
    \Bigp{
    \begin{array}{@{}c@{}}
        \forall i\in\IndexFunc \\
        \forall \by_{i}=\p{y_{i,1},\ldots,y_{i,\NumEval_{i}}}\in \calH^{\NumEval_{i}}
    \end{array}
    }
    \quad
    \bfcn_{i}(\by_{i})&=\p{f_i\p{y_{i,1}},\ldots,f_{i}\p{y_{i,\NumEval_{i}}}},\end{aligned}\end{split}\]

\(\bm{\partial}\bfcn_{i}:\calH^{\NumEval_{i}}\rightrightarrows\calH^{\NumEval_{i}}\)
such that


\[\begin{split}\begin{aligned}
    \Bigp{
    \begin{array}{@{}c@{}}
        \forall i\in\IndexFunc \\
        \forall \by_{i}=\p{y_{i,1},\ldots,y_{i,\NumEval_{i}}}\in \calH^{\NumEval_{i}}
    \end{array}
    }
    \quad
    \bm{\partial}\bfcn_{i}(\by_{i})&= \prod_{j=1}^{\NumEval_{i}} \partial f_{i}\p{y_{i,j}},\end{aligned}\end{split}\]

and
\(\bm{G}_{i}: \calH^{\NumEval_{i}} \rightrightarrows \calH^{\NumEval_{i}}\)
such that


\[\begin{split}\begin{aligned}
    \Bigp{
    \begin{array}{@{}c@{}}
        \forall i\in\IndexOp \\
        \forall \by_{i}=\p{y_{i,1},\ldots,y_{i,\NumEval_{i}}}\in \calH^{\NumEval_{i}}
    \end{array}
    }
    \quad
    \bm{G}_{i}\p{\by_{i}}=\prod_{j=1}^{\NumEval_{i}} G_{i}\p{y_{i,j}}.\end{aligned}\end{split}\]

We are now ready to give the algorithm representation: Pick an initial
\(\bx_{0}\in\calH^{n}\), an iteration horizon
\(K\in\naturals\cup\set{\infty}\), and let

<a id="equation-eq-linear-system-with-nonlinearity"></a>

<a id="eq-linear-system-with-nonlinearity"></a>



(3.1)\[\begin{split}    \p{\forall k \in\llbracket0,K\rrbracket} \quad
    \left[
    \begin{aligned}
        &\bx^{k+1} = \p{A_{k} \kron \Id} \bx^{k} + \p{B_{k} \kron \Id} \bu^{k}, \\
        &\by^{k} =  \p{C_{k} \kron \Id} \bx^{k} + \p{D_{k} \kron \Id} \bu^{k}, \\
        &\p{\bu^{k}_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\bm{\partial}\bfcn_{i}\p{\by_{i}^{k}}, \\
        &\p{\bu^{k}_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}\bm{G}_{i}\p{\by_{i}^{k}}, \\
        &\bFcn^{k} =\p{\bfcn_{i}\p{\by_{i}^{k}} }_{i\in\IndexFunc},
    \end{aligned}
    \right.\end{split}\]

where


\[\begin{split}\begin{aligned}
   \bx^{k} &= \p{x_1^k,\ldots,x_n^k} \in \calH^n, \\
   \bu^{k} &= \p{\bu^{k}_{1},\ldots,\bu^{k}_{m}} \in \prod_{i=1}^{m}\calH^{\NumEval_{i}}, \\
   \by^{k} &= \p{\by^{k}_{1},\ldots,\by^{k}_{m}} \in \prod_{i=1}^{m}\calH^{\NumEval_{i}}, \\
   \bFcn^{k} &\in \reals^{\NumEvalFunc}.
\end{aligned}\end{split}\]

are the algorithm variables, and

<a id="equation-eq-abcd"></a>

<a id="eq-abcd"></a>



(3.2)\[\begin{aligned}
    A_{k}&\in\reals^{n\times n},& B_{k}&\in\reals^{n\times {\NumEval}},& C_{k}&\in\reals^{{\NumEval}\times n},& D_{k}&\in\reals^{{\NumEval}\times {\NumEval}}\end{aligned}\]

are matrices containing the parameters of the algorithm at hand.

We close this page with
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness). It is satisfied
by practical algorithms and ensures that the subsequent theoretical
results are well-posed.

<a id="ass-well-posedness"></a>



**Assumption 3.1 (Well-posedness).**

We assume that the parameter sequence


\[\p{A_{k},B_{k},C_{k},D_{k}}_{k=0}^{K}\]

is chosen such that, for each


\[\bx_{0}\in\calH^{n}, \qquad
\p{f_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc} \mathcal{F}_{i}, \qquad
\p{G_i}_{i\in\IndexOp} \in \prod_{i\in\IndexOp} \mathcal{G}_i,\]

there exists a sequence


\[\p{\bx^{k},\bu^{k},\by^{k},\bFcn^{k}}_{k=0}^{K}\]

satisfying [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity).

In this context, the symbol \(\Pi\) denotes Cartesian products.
For interface documentation in AutoLyap, see
[Algorithms](https://autolyap.github.io/algorithms/). In particular:

- *Base algorithms*: abstract interfaces for users to implement their
  own algorithms; see [Base algorithms](https://autolyap.github.io/base_algorithms/). For an
  example, see [The proximal gradient
  method](https://autolyap.github.io/examples/define_your_own_algorithm/proximal_gradient_method/).
- *Concrete algorithms*: built-in algorithm implementations; see
  [Concrete algorithms](https://autolyap.github.io/concrete_algorithms/) for the full list.

TeX macro definitions used in this page:

```latex
\newcommand{\Bigp}[1]{\mathord{\left( #1 \right)}}
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumEvalOp}{\bar{m}_{\textup{op}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\NumOp}{m_{\textup{op}}}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bfcn}{\mathbf{f}}
\newcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\kron}{\otimes}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
```
