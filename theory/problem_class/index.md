Canonical HTML: https://autolyap.github.io/theory/problem_class/
Documentation version: 0.2.1

<a id="problem-classes"></a>



# 2. Problem classes

To cover both structured optimization and inclusion problems, we
introduce two disjoint index sets
\(\IndexFunc, \IndexOp \subseteq \llbracket1,m\rrbracket\), where
\(m\in\mathbb{N}\), such that
\(\IndexFunc\cup \IndexOp = \llbracket1,m\rrbracket\), and consider
inclusion problems of the form

<a id="equation-eq-the-problem-inclusion"></a>

<a id="eq-the-problem-inclusion"></a>



(2.1)\[\begin{aligned}
    \text{find}\  y\in\calH\ \text{ such that }\ 0\in\sum_{i\in\IndexFunc} \partial f_{i}\p{y} + \sum_{i\in\IndexOp} G_{i}\p{y},\end{aligned}\]

where the functions \(f_i:\calH \to \reals \cup \set{\pm \infty}\)
and operators \(G_i : \calH \rightrightarrows \calH\) are chosen
from some user-specified function class \(\mathcal{F}_i\) and
operator class \(\mathcal{G}_{i}\), respectively, i.e.,


\[\begin{split}\begin{aligned}
    \p{\forall i \in \IndexFunc}& \quad f_{i} \in \mathcal{F}_{i} \subseteq \set{ f: \calH \to \reals\cup \set{\pm \infty}}, \\
    \p{\forall i \in \IndexOp}& \quad G_{i} \in \mathcal{G}_{i} \subseteq \set{ G: \calH \rightrightarrows \calH }.\end{aligned}\end{split}\]

For example, if \(\IndexOp=\emptyset\), then
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion) is a
first-order optimality condition for minimizing


\[\sum_{i\in\IndexFunc} f_{i}.\]

Moreover,
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion) provides a
formalism that covers monotone inclusion problems,
certain equilibrium problems,
so-called (mixed) variational inequalities,
and beyond.

The API for these objects is documented in
[API reference](https://autolyap.github.io/api_reference/). In particular,
[Problem classes](https://autolyap.github.io/problem_class/) documents the
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")
interface for formulating
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion), while
[Function classes](https://autolyap.github.io/function_classes/) and
[Operator classes](https://autolyap.github.io/operator_classes/) document the shipped
function- and operator-class interfaces corresponding to the
classes \((\mathcal{F}_i)_{i\in\IndexFunc}\) and
\((\mathcal{G}_i)_{i\in\IndexOp}\), respectively, in
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion). The API also supports intersections of
function classes and intersections of operator classes (but not mixed
function-operator intersections).

TeX macro definitions used in this page:

```latex
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
```
