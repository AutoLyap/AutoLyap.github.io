Canonical HTML: https://autolyap.github.io/theory/interpolation_conditions/
Documentation version: 0.2.1

<a id="interpolation-conditions"></a>



# 4. Interpolation conditions

This page is technical. It introduces a key assumption used to
formulate the Lyapunov search as a semidefinite program (SDP).

<a id="ass-interpolation"></a>



**Assumption 4.1 (Interpolation conditions).**

Consider
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
and, for notational convenience, let


\[\begin{split}\begin{aligned}
    \left(
        \begin{array}{@{}c@{}}
            \forall i \in \IndexFunc\cup\IndexOp \\
            \forall k \in \llbracket0,K\rrbracket
        \end{array}
    \right)
    &\quad \bu^{k}_{i} = \p{u^{k}_{i,1}, \ldots, u^{k}_{i,\NumEval_{i}}}\in \calH^{\NumEval_i},  \\
    \left(
        \begin{array}{@{}c@{}}
            \forall i \in \IndexFunc\cup\IndexOp \\
            \forall k \in \llbracket0,K\rrbracket
        \end{array}
    \right)
    &\quad \by^{k}_{i} = \p{y^{k}_{i,1}, \ldots, y^{k}_{i,\NumEval_{i}}}\in \calH^{\NumEval_i}, \\
    \left(
        \begin{array}{@{}c@{}}
            \forall i \in \IndexFunc \\
            \forall k \in \llbracket0,K\rrbracket
        \end{array}
    \right)
    &\quad \bfcn_{i}\p{\by_{i}^{k}} = \p{F^{k}_{i,1}, \ldots, F^{k}_{i,\NumEval_{i}}} \in \reals^{\NumEval_{i}}\end{aligned}\end{split}\]

and


\[\begin{split}\begin{aligned}
    \bu^{\star} &= \p{u^{\star}_{1},\ldots,u^{\star}_{m}} = \p{u^{\star}_{1,\star},\ldots,u^{\star}_{m,\star}} \in \calH^{m} \quad \text{ such that }\quad \sum_{i=1}^{m} u^{\star}_{i} = 0, \\
    y^{\star} &= y^{\star}_{1,\star}=\cdots=y^{\star}_{m,\star} \in \calH, \\
    \bFcn^{\star} &=\p{F^{\star}_{i,\star}}_{i\in\IndexFunc}\in\reals^{\NumFunc }.\end{aligned}\end{split}\]

1. For each \(i\in\IndexFunc\), suppose that there exist finite
   and disjoint sets \(\mathcal{O}_{i}^{\textup{func-ineq}}\)
   and \(\mathcal{O}_{i}^{\textup{func-eq}}\), vectors and
   matrices


\[\begin{split}\begin{aligned}
        \p{\forall o \in \mathcal{O}_{i}^{\textup{func-ineq}}}
        &\quad \p{ a_{\p{i,o}}^{\textup{func-ineq}}, M_{\p{i,o}}^{\textup{func-ineq}}} \in  \reals^{n_{i,o}} \times \sym^{2n_{i,o}}, \\
        \p{\forall o \in \mathcal{O}_{i}^{\textup{func-eq}}}
        &\quad \p{a_{\p{i,o}}^{\textup{func-eq}}, M_{\p{i,o}}^{\textup{func-eq}}} \in  \reals^{n_{i,o}} \times \sym^{2n_{i,o}},
    \end{aligned}\end{split}\]

   and, depending on
   \(\PEPMinIter,\PEPMaxIter\in\llbracket0,K\rrbracket\) such
   that \(\PEPMinIter\leq\PEPMaxIter\), index sets


\[\begin{split}\begin{gathered}
        \p{\forall o \in \mathcal{O}_{i}^{\textup{func-ineq}}\cup \mathcal{O}_{i}^{\textup{func-eq}} } \\
        \mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}\subseteq \p{ \p{\llbracket 1,\NumEval_{i} \rrbracket \times \llbracket\PEPMinIter,\PEPMaxIter\rrbracket } \cup \set{ \p{\star,\star} }   }^{n_{i,o}},
    \end{gathered}\end{split}\]

   such that *(i)* implies *(ii)* below:

   1. There exists a function \(f_{i}\in\mathcal{F}_{i}\) such
      that


\[\begin{split}\begin{aligned}
            \p{\forall (j,k)\in\p{\llbracket 1,\NumEval_{i} \rrbracket \times \llbracket\PEPMinIter,\PEPMaxIter\rrbracket } \cup \set{ \p{\star,\star} } } \quad
            \left[
            \begin{aligned}
                f_{i}\p{y_{i,j}^{k}} = F_{i,j}^{k}, \\
                u_{i,j}^{k} \in \partial f_{i} \p{y_{i,j}^{k} }.
            \end{aligned}
            \right.
        \end{aligned}\end{split}\]
   2. It holds that


\[\begin{split}\begin{aligned}
    \left(
        \begin{array}{@{}c@{}}
            \forall o \in \mathcal{O}_{i}^{\textup{func-ineq}} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
    \right)
    \quad
    \p{a_{\p{i,o}}^{\textup{func-ineq}}}^{\top} F
    + \quadform{M_{\p{i,o}}^{\textup{func-ineq}}}{z}
    \leq 0,
    \\
    \left(
        \begin{array}{@{}c@{}}
            \forall o \in \mathcal{O}_{i}^{\textup{func-eq}} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
    \right)
    \quad
    \p{a_{\p{i,o}}^{\textup{func-eq}}}^{\top} F
    + \quadform{M_{\p{i,o}}^{\textup{func-eq}}}{z}
    = 0.
\end{aligned}\end{split}\]

      where


\[\begin{split}z = \p{y_{i,j_{1}}^{k_{1}},\ldots,y_{i,j_{n_{i,o}}}^{k_{n_{i,o}}},u_{i,j_{1}}^{k_{1}},\ldots,u_{i,j_{n_{i,o}}}^{k_{n_{i,o}}}}
\quad \text{and} \quad
F = \begin{bmatrix}
    F_{i,j_{1}}^{k_{1}} \\
    \vdots \\
    F_{i,j_{n_{i,o}}}^{k_{n_{i,o}}}
\end{bmatrix}.\end{split}\]

   Moreover, if the converse holds, i.e., *(ii)* implies *(i)*, then
   we say that the function class \(\mathcal{F}_i\) has a *tight
   interpolation condition*.
2. Similarly, for each \(i\in\IndexOp\), suppose that there
   exists a finite set \(\mathcal{O}_{i}^{\textup{op}}\),
   matrices


\[\begin{aligned}
        \p{\forall o \in \mathcal{O}_{i}^{\textup{op}}} \quad M_{\p{i,o}}^{\textup{op}} \in \sym^{2 n_{i,o}},
    \end{aligned}\]

   and, depending on
   \(\PEPMinIter,\PEPMaxIter\in\llbracket0,K\rrbracket\) such
   that \(\PEPMinIter\leq\PEPMaxIter\), index sets


\[\begin{aligned}
        \p{\forall o \in \mathcal{O}_{i}^{\textup{op}}}
        &\quad \mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}\subseteq \p{ \p{\llbracket 1,\NumEval_{i} \rrbracket \times \llbracket\PEPMinIter,\PEPMaxIter\rrbracket } \cup \set{ \p{\star,\star} }   }^{n_{i,o}}
    \end{aligned}\]

   such that *(i)* implies *(ii)* below:

   1. There exists an operator \(G_{i}\in\mathcal{G}_{i}\)
      such that


\[\begin{aligned}
            \p{\forall (j,k)\in\p{\llbracket 1,\NumEval_{i} \rrbracket \times \llbracket\PEPMinIter,\PEPMaxIter\rrbracket } \cup \set{ \p{\star,\star} } } \quad u_{i,j}^{k} \in G_{i} \p{y_{i,j}^{k} }.
        \end{aligned}\]
   2. It holds that


\[\begin{split}\left(
    \begin{array}{@{}c@{}}
        \forall o \in \mathcal{O}_{i}^{\textup{op}} \\
        \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
    \end{array}
\right)
\quad
\quadform{M_{\p{i,o}}^{\textup{op}}}{z} \leq 0,\end{split}\]

      where


\[z = \p{y_{i,j_{1}}^{k_{1}},\ldots,y_{i,j_{n_{i,o}}}^{k_{n_{i,o}}},u_{i,j_{1}}^{k_{1}},\ldots,u_{i,j_{n_{i,o}}}^{k_{n_{i,o}}}}.\]

   Moreover, if the converse holds, i.e., *(ii)* implies *(i)*, then
   we say that the operator class \(\mathcal{G}_i\) has a *tight
   interpolation condition*.

For concrete interpolation-condition examples in API docstrings, see
[Function classes](https://autolyap.github.io/function_classes/) and
[Operator classes](https://autolyap.github.io/operator_classes/). In the setting of
[Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation),
the following shipped classes have tight interpolation
conditions:

- Function classes:

  - [`Convex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Convex "autolyap.problemclass.Convex")
  - [`StronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.StronglyConvex "autolyap.problemclass.StronglyConvex")
  - [`WeaklyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.WeaklyConvex "autolyap.problemclass.WeaklyConvex")
  - [`Smooth`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Smooth "autolyap.problemclass.Smooth")
  - [`SmoothConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothConvex "autolyap.problemclass.SmoothConvex")
  - [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex")
  - [`SmoothWeaklyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothWeaklyConvex "autolyap.problemclass.SmoothWeaklyConvex")
  - [`IndicatorFunctionOfClosedConvexSet`](https://autolyap.github.io/function_classes/#autolyap.problemclass.IndicatorFunctionOfClosedConvexSet "autolyap.problemclass.IndicatorFunctionOfClosedConvexSet")
  - [`SupportFunctionOfClosedConvexSet`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SupportFunctionOfClosedConvexSet "autolyap.problemclass.SupportFunctionOfClosedConvexSet")
- Operator classes:

  - [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone")
  - [`StronglyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.StronglyMonotone "autolyap.problemclass.StronglyMonotone")
  - [`LipschitzOperator`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.LipschitzOperator "autolyap.problemclass.LipschitzOperator")
  - [`Cocoercive`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.Cocoercive "autolyap.problemclass.Cocoercive")

Intersections of function classes are supported, and intersections of
operator classes are also supported. However, after taking such
intersections, tightness of the resulting class is not guaranteed in
general.

TeX macro definitions used in this page:

```latex
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\PEPMaxIter}{\bar{k}}
\newcommand{\PEPMinIter}{\underline{k}}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bfcn}{\mathbf{f}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\quadform}[2]{\mathcal{Q}\p{#1,#2}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
\newcommand{\sym}{\mathbb{S}}
```
