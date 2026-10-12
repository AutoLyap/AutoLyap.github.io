Canonical HTML: https://autolyap.github.io/theory/lyapunov_analyses/
Documentation version: 0.2.1

<a id="lyapunov-analyses"></a>



# 5. Lyapunov analyses

This page explains how AutoLyap uses the algorithm representation and
interpolation assumptions to cast the existence of a Lyapunov analysis
as the feasibility of a particular SDP.
Moreover, this formulation is constructive, in the sense that any feasible
solution directly yields an explicit Lyapunov function and associated
convergence certificate.

For later convenience across pages
[5.1](https://autolyap.github.io/theory/performance_estimation_via_sdps/),
[5.2](https://autolyap.github.io/theory/iteration_independent_analyses/), and
[5.3](https://autolyap.github.io/theory/iteration_dependent_analyses/), we introduce the
solution variables used in the Lyapunov analyses.

We are interested in analyses that may depend on a solution to the inclusion
problem [(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion). Without loss of generality and for
computational efficiency, we use

<a id="equation-eq-solution"></a>

<a id="eq-solution"></a>



(5.1)\[\begin{split}\begin{aligned}
    \p{y^{\star}, \hat{\bu}^{\star}, \bFcn^{\star}} \in
    \Bigset{
    \underbracket{\p{y,\hat{\bu}, \bFcn}}_{ \in \calH \times\calH^{m-1} \times \reals^{\NumFunc}  } \xmiddle|
    \begin{aligned}
        &\p{u_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\partial f_{i}\p{y}, \\
        &\p{u_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}G_{i}\p{y}, \\
        &\sum_{i=1}^{m} u_{i} = 0, \\
        &\hat{\bu} = \p{u_{1},\ldots,u_{m-1}}, \\
        &\bFcn=\p{f_{i}\p{y}}_{i\in\IndexFunc}
    \end{aligned}
    },\end{aligned}\end{split}\]

where \(\hat{\bu}^{\star}\) is void when \(m=1\). In particular,
\(y^{\star}\) in [(5.1)](https://autolyap.github.io/theory/lyapunov_analyses/#equation-eq-solution) is a solution to
[(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion).

The three subpages below are organized as follows. Page
[5.1](https://autolyap.github.io/theory/performance_estimation_via_sdps/) introduces a technical
SDP primitive. Pages [5.2](https://autolyap.github.io/theory/iteration_independent_analyses/) and
[5.3](https://autolyap.github.io/theory/iteration_dependent_analyses/) then build on that
primitive. The actual Lyapunov analyses, together with their corresponding
convergence conclusions, are presented on pages
[5.2](https://autolyap.github.io/theory/iteration_independent_analyses/) and
[5.3](https://autolyap.github.io/theory/iteration_dependent_analyses/).

- [5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
- [5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
- [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/)

TeX macro definitions used in this page:

```latex
\newcommand{\Bigset}[1]{\mathord{\left\{ #1 \right\}}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\underbracket}[1]{\underbrace{#1}}
\newcommand{\xmiddle}[1]{\;\middle#1\;}
```
