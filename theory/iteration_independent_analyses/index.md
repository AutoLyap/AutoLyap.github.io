Canonical HTML: https://autolyap.github.io/theory/iteration_independent_analyses/
Documentation version: 0.2.1

<a id="iteration-independent-analyses"></a>



# 5.2. Iteration-independent analyses

For iteration-independent analyses, we consider algorithms that continue to iterate
indefinitely and have iteration-independent parameters. I.e.,
in [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity),
we assume that \(K=\infty\) and that there exist fixed matrices


\[\begin{aligned}
    \p{A,B,C,D}\in\reals^{n\times n}\times\reals^{n\times \NumEval}\times\reals^{\NumEval\times n}\times\reals^{\NumEval \times \NumEval}\end{aligned}\]

such that

<a id="equation-eq-constant-abcd"></a>

<a id="eq-constant-abcd"></a>



(5.24)\[\begin{aligned}
    \p{\forall k \in \naturals }\quad \p{A_{k},B_{k},C_{k},D_{k}} = \p{A,B,C,D}.\end{aligned}\]

<a id="def-iteration-independent-lyapunov"></a>



**Definition 5.2.1 (Iteration-independent quadratic Lyapunov inequality).**

Suppose that
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness)
and [(5.24)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-constant-abcd) hold. Let


\[\bx_{0}\in\calH^{n}, \qquad
\p{ ( \bx^{k}, \bu^{k}, \by^{k}, \bFcn^{k} ) }_{k\in\naturals},\]

be an initial point and a sequence of iterates satisfying
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity), and let


\[\p{y^{\star}, \hat{\bu}^{\star}, \bFcn^{\star}}\]

be a point satisfying [(5.1)](https://autolyap.github.io/theory/lyapunov_analyses/#equation-eq-solution). Also let


\[\rho \in [0,1], \qquad h\in\naturals, \qquad \alpha\in\naturals.\]

Define

<a id="equation-eq-iteration-independent-lyapunov-v"></a>

<a id="eq-iteration-independent-lyapunov-v"></a>



(5.25)\[\begin{split}\begin{aligned}
        \p{\forall k \in \naturals}
        \quad
            \mathcal{V}(W,w,k) {}={}&{} \quadform{W}{\p{\bx^{k},\bu^{k},\ldots,\bu^{k+h},\hat{\bu}^{\star},y^{\star}}} \\
            & + w^{\top}\p{\bFcn^{k},\ldots,\bFcn^{k+h},\bFcn^{\star}},
    \end{aligned}\end{split}\]

for each \(\p{W,w}\in\set{\p{Q,q},\p{P,p}}\), where


\[Q,P \in \sym^{n+\p{h+1}\NumEval+m}, \qquad
q,p\in\mathbb{R}^{\p{h+1}\NumEvalFunc + \NumFunc},\]

and define

<a id="equation-eq-iteration-independent-lyapunov-r"></a>

<a id="eq-iteration-independent-lyapunov-r"></a>



(5.26)\[\begin{split}\begin{aligned}
        \p{\forall k \in \naturals}
        \quad
            \mathcal{R}(W,w,k) {}={}&{} \quadform{W}{\p{\bx^{k},\bu^{k},\ldots,\bu^{k+h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}} \\
            &{} + w^{\top}\p{\bFcn^{k},\ldots,\bFcn^{k+h + \alpha + 1},\bFcn^{\star}},
    \end{aligned}\end{split}\]

for each \(\p{W,w}\in\set{\p{S,s},\p{T,t}}\), where


\[S,T\in\sym^{n+\p{h+\alpha+2}\NumEval+m}, \qquad
s, t \in\mathbb{R}^{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}.\]

We say that \(\p{Q,q,S,s}\) satisfies a
\(\p{P,p,T,t,\rho, h, \alpha}\)-*quadratic Lyapunov inequality*
for algorithm
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
over the problem class defined by
\((\mathcal{F}_i)_{i\in\IndexFunc}\) and
\((\mathcal{G}_i)_{i\in\IndexOp}\) if

<a id="eq-c1"></a>

<a id="eq-c2"></a>
<a id="eq-c3"></a>
<a id="eq-c4"></a>



\begin{align}
\p{\forall k \in \naturals} &\quad \mathcal{V}\p{Q,q,k+\alpha+1} \leq \rho \mathcal{V}\p{Q,q,k} - \mathcal{R}\p{S,s,k}, \tag{C1} \\
\p{\forall k \in \naturals} &\quad \mathcal{V}\p{Q,q,k} \geq \mathcal{V}\p{P,p,k} \geq 0, \tag{C2} \\
\p{\forall k \in \naturals} &\quad \mathcal{R}\p{S,s,k} \geq \mathcal{R}\p{T,t,k}\geq 0, \tag{C3} \\
\p{\forall k \in \naturals} &\quad \mathcal{R}\p{S,s,k+1} \leq \mathcal{R}\p{S,s,k}, \tag{C4}
\end{align}

hold for each


\[\bx_{0}, \qquad
\p{ ( \bx^{k}, \bu^{k}, \by^{k}, \bFcn^{k} ) }_{k\in\naturals}, \qquad
\p{y^{\star}, \hat{\bu}^{\star},\bFcn^{\star}},\]

and for each


\[\p{f_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc} \mathcal{F}_{i},
\qquad
\p{G_i}_{i\in\IndexOp} \in \prod_{i\in\IndexOp} \mathcal{G}_i,\]

where [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4) is an optional requirement that may be
removed.

Definition 5.2.1 is a trajectory-level condition:
[(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1)-[(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3) (and optionally
[(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4)) must hold for all iterates and all admissible
functions/operators in the class. This statement is not directly
computational. In practice, the user specifies
\(\p{P,p,T,t,\rho,h,\alpha}\) and searches for
\(\p{Q,q,S,s}\). The SDP sufficient condition that makes this search
computable is given in [Theorem 5.2.2 (Iteration-independent Lyapunov inequality via SDP)](https://autolyap.github.io/theory/iteration_independent_analyses/#thm-iteration-independent-lyapunov).
When such \(\p{Q,q,S,s}\) exists, the choice of
\(\p{P,p,T,t,\rho,h,\alpha}\) determines the convergence guarantee.
In particular, we obtain:

- Linear setting: if \(\rho \in [0,1[\), then


\[\begin{aligned}
        0 \leq \mathcal{V}\p{P,p,k} \leq \mathcal{V}\p{Q,q,k} \leq \rho^{\lfloor k/\p{\alpha+1}\rfloor }\max_{i\in\llbracket0,\alpha\rrbracket}\mathcal{V}\p{Q,q,i} \xrightarrow[k\rightarrow \infty]{} 0.
    \end{aligned}\]

  Thus, \((\mathcal{V}\p{P,p,k})_{k\in\naturals}\) converges to zero


\[\sqrt[\alpha + 1]{\rho}\textup{-linearly}.\]
- Sublinear setting: if \(\rho = 1\), then


\[\begin{aligned}
        \p{\forall k \in \naturals}\quad \sum_{i=0}^{k} \mathcal{R}\p{T,t,i} \leq \sum_{i=0}^{k} \mathcal{R}\p{S,s,i} \leq \sum_{j=0}^{\alpha} \mathcal{V}\p{Q,q,j},
    \end{aligned}\]

  by a telescoping summation argument. In particular,


\[(\mathcal{R}\p{T,t,k})_{k\in\naturals}\]

  is summable, converges to zero, and, e.g.,


\[\min_{i\in \llbracket 0,k \rrbracket } \mathcal{R}\p{T,t,i} \in \mathcal{O}\p{1/k} \quad \textup{ as } \quad k\to\infty.\]

  If the optional requirement [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4) holds, we obtain the
  stronger last-iterate convergence result


\[\mathcal{R}\p{T,t,k} \in o\p{1/k} \quad \textup{ as } \quad k\to\infty.\]

The user-chosen \(\p{P,p,T,t}\) fixes the particular Lyapunov and
residual expressions \(\mathcal{V}\p{P,p,\cdot}\) and
\(\mathcal{R}\p{T,t,\cdot}\) to be analyzed. For built-in choices:

<a id="theory-iter-independent-linear-builtins"></a>



- Linear convergence:
  [Linear-convergence helpers](https://autolyap.github.io/iteration_independent_analysis/#iter-independent-linear-helpers).

<a id="theory-iter-independent-sublinear-builtins"></a>



- Sublinear convergence:
  [Sublinear-convergence helpers](https://autolyap.github.io/iteration_independent_analysis/#iter-independent-sublinear-helpers).

For the corresponding API entry points, use
[`search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov") for fixed
\(\rho\), and
[`bisection_search_rho()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.bisection_search_rho "autolyap.IterationIndependent.LinearConvergence.bisection_search_rho")
to estimate the smallest feasible contraction factor in the
linear-convergence setting.

Role of \(h\) and \(\alpha\):

- \(h\) is a history parameter for \(\mathcal{V}\)
  (see [(5.25)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-v)).


\[\text{the } \mathcal{V}\text{-window length is } h+1.\]
- \(\alpha\) is an overlap parameter that induces the shift
  \(k \mapsto k+\alpha+1\) in [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1).
- \(h\) and \(\alpha\) determine the \(\mathcal{R}\) window
  (see [(5.26)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-r)).


\[\text{the } \mathcal{R}\text{-window length is } h+\alpha+2.\]

We now state the finite-dimensional SDP condition that certifies the
existence of such \(\p{Q,q,S,s}\).

<a id="thm-iteration-independent-lyapunov"></a>



**Theorem 5.2.2 (Iteration-independent Lyapunov inequality via SDP).**

Suppose that
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness), [Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation)
and [(5.24)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-constant-abcd) hold. Let


\[\rho \in [0,1], \qquad h\in\naturals, \qquad \alpha\in\naturals,\]

and let


\[P \in \sym^{n+\p{h+1}\NumEval+m}, \qquad
p\in\mathbb{R}^{\p{h+1}\NumEvalFunc + \NumFunc},\]

such that


\[\begin{aligned}
        \p{\forall k \in \naturals} \qquad \mathcal{V}\p{P,p,k} \geq 0,
    \end{aligned}\]

where \(\mathcal{V}\) is defined in
[(5.25)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-v),
and let


\[T\in\sym^{n+\p{h+\alpha+2}\NumEval+m}, \qquad
t\in\mathbb{R}^{\p{h+\alpha+2}\NumEvalFunc + \NumFunc},\]

such that


\[\begin{aligned}
        \p{\forall k \in \naturals} \qquad \mathcal{R}\p{T,t,k} \geq 0,
    \end{aligned}\]

where \(\mathcal{R}\) is defined in
[(5.26)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-r).
Then a sufficient condition for there to exist \(\p{Q,q,S,s}\)
that satisfies a \(\p{P,p,T,t,\rho, h, \alpha}\)-quadratic
Lyapunov inequality for algorithm
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
over the problem class defined by
\(\p{\mathcal{F}_i}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_i}_{i\in\IndexOp}\) (recall that
[(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4) is optional and may be omitted) is that the
following system of constraints, which reuses the lifted
matrices/vectors from [5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/), including the lifted state
matrices [(5.3)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-x-mats), and the interpolation terms
[(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op),
[(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq), and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq),

<a id="eq-iteration-independent-lyapunov-condition-s"></a>

<a id="eq-iteration-independent-lyapunov-condition"></a>
<a id="eq-iteration-independent-lyapunov-condition-func-ineq"></a>
<a id="eq-iteration-independent-lyapunov-condition-func-eq"></a>
<a id="eq-iteration-independent-lyapunov-condition-op"></a>
<a id="eq-iteration-independent-lyapunov-condition-psd"></a>
<a id="eq-iteration-independent-lyapunov-condition-zero"></a>
<a id="eq-iteration-independent-lyapunov-condition-q-mat"></a>
<a id="eq-iteration-independent-lyapunov-condition-q"></a>
<a id="eq-iteration-independent-lyapunov-condition-s-mat"></a>



\begin{align}
        & \textbf{for each}\ \textup{cond} \in\set{\href{#eq-c1}{\textup{C1}},\href{#eq-c2}{\textup{C2}},\href{#eq-c3}{\textup{C3}},\href{#eq-c4}{\textup{C4}}} \notag \\
        &\qquad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \qquad \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,\textup{cond}} \geq 0, \tag{5.28a}\\
        &\qquad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \qquad
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,\textup{cond}} \in \reals, \tag{5.28b}\\
        &\qquad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexOp \\
            \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \qquad
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,\textup{cond}} \geq 0, \tag{5.28c}\\
        &\qquad - W^{\textup{cond}}
        + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,\textup{cond}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-ineq}} \notag \\
        &\qquad \qquad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}} }}
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,\textup{cond}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-eq}} \notag \\
        &\qquad \qquad + \sum_{\substack{i\in\IndexOp \\ o \in \mathcal{O}^{\textup{op}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,\textup{cond}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{op}}
        \succeq 0, \tag{5.28d}\\
        &\qquad
        - w^{\textup{cond}}
        + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,\textup{cond}}
        F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-ineq}} \notag \\
        &\qquad \qquad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}} }}
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,\textup{cond}}
        F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-eq}}
        = 0, \tag{5.28e}\\
        & \mathbf{end} \notag \\
        & Q \in \sym^{n+\p{h+1}\NumEval+m}, \tag{5.28f}\\
        & q \in \mathbb{R}^{\p{h+1}\NumEvalFunc + \NumFunc}, \tag{5.28g}\\
        & S \in \sym^{n+\p{h+\alpha+2}\NumEval+m}, \tag{5.28h}\\
        & s \in \reals^{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}. \tag{5.28i}
\end{align}

is feasible for the scalars


\[\begin{split}\begin{aligned}
        &\lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,\textup{cond}}, \\
        &\nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,\textup{cond}}, \\
        &\lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,\textup{cond}},
    \end{aligned}\end{split}\]

the matrices \(Q\) and \(S\), and the vectors \(q\) and
\(s\), where

<a id="equation-eq-iteration-independent-lyapunov-w-c1-mat"></a>




(5.29)\[\begin{aligned}
        W^{\href{#eq-c1}{\textup{C1}}} &= \p{\Theta_{1}^{\href{#eq-c1}{\textup{C1}}}}^{\top}Q\Theta_{1}^{\href{#eq-c1}{\textup{C1}}} - \rho \p{\Theta_{0}^{\href{#eq-c1}{\textup{C1}}}}^{\top}Q\Theta_{0}^{\href{#eq-c1}{\textup{C1}}} + S,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c1"></a>




(5.30)\[\begin{aligned}
        w^{\href{#eq-c1}{\textup{C1}}} &= \p{\theta_{1}^{\href{#eq-c1}{\textup{C1}}} - \rho \theta_{0}^{\href{#eq-c1}{\textup{C1}}} }^{\top}q + s,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c2-mat"></a>




(5.31)\[\begin{aligned}
        W^{\href{#eq-c2}{\textup{C2}}} &= P-Q,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c2"></a>




(5.32)\[\begin{aligned}
        w^{\href{#eq-c2}{\textup{C2}}} &= p-q,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c3-mat"></a>




(5.33)\[\begin{aligned}
        W^{\href{#eq-c3}{\textup{C3}}} &= T - S,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c3"></a>




(5.34)\[\begin{aligned}
        w^{\href{#eq-c3}{\textup{C3}}} &= t - s,
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c4-mat"></a>




(5.35)\[\begin{aligned}
        W^{\href{#eq-c4}{\textup{C4}}} &= \p{\Theta_{1}^{\href{#eq-c4}{\textup{C4}}}}^{\top}S\Theta_{1}^{\href{#eq-c4}{\textup{C4}}} - \p{\Theta_{0}^{\href{#eq-c4}{\textup{C4}}}}^{\top}S\Theta_{0}^{\href{#eq-c4}{\textup{C4}}},
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-w-c4"></a>




(5.36)\[\begin{aligned}
        w^{\href{#eq-c4}{\textup{C4}}} &= \p{\theta_{1}^{\href{#eq-c4}{\textup{C4}}} - \theta_{0}^{\href{#eq-c4}{\textup{C4}}} }^{\top}s,
    \end{aligned}\]


\[\begin{split}\begin{aligned}
        \PEPMaxIter_{\textup{cond}} &=
        \begin{cases}
            h + \alpha + 1& \text{ if } \textup{cond} \in \set{\href{#eq-c1}{\textup{C1}},\href{#eq-c3}{\textup{C3}}},\\
            h & \text{ if } \textup{cond} \in \set{\href{#eq-c2}{\textup{C2}}}, \\
            h + \alpha + 2 & \text{ if } \textup{cond} \in \set{\href{#eq-c4}{\textup{C4}}},
        \end{cases} \nonumber
    \end{aligned}\end{split}\]

and

<a id="equation-eq-iteration-independent-lyapunov-theta0-c1-mat"></a>




(5.37)\[\begin{split}\begin{aligned}
        \Theta_{0}^{\href{#eq-c1}{\textup{C1}}} &=
        \underbracket{
        \begin{bmatrix}
            I_{n+\p{h+1}\NumEval } & 0_{\p{n+\p{h+1}\NumEval}\times\p{\alpha + 1}\NumEval } & 0_{\p{n+\p{h+1}\NumEval}\times m} \\
            0_{m \times \p{n+\p{h+1}\NumEval}} & 0_{m \times\p{\alpha + 1}\NumEval } & I_{m}
        \end{bmatrix}
        }_{
        \in \reals^{\p{n+\p{h+1}\NumEval+m}\times\p{n+\p{h + \alpha +2}\NumEval+m}}
        },
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta0-c1"></a>




(5.38)\[\begin{split}\begin{aligned}
        \theta_{0}^{\href{#eq-c1}{\textup{C1}}} &=
        \underbracket{
        \begin{bmatrix}
            I_{\p{h+1}\NumEvalFunc} & 0_{\p{h+1}\NumEvalFunc \times \p{\alpha + 1}\NumEvalFunc } & 0_{\p{h+1}\NumEvalFunc \times \NumFunc } \\
            0_{\NumFunc\times \p{h+1}\NumEvalFunc} &0_{\NumFunc\times \p{\alpha + 1}\NumEvalFunc} & I_{\NumFunc}
        \end{bmatrix}
        }_{
        \in \reals^{\p{\p{h+1}\NumEvalFunc + \NumFunc}\times\p{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}}
        },
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta1-c1-mat"></a>




(5.39)\[\begin{split}\begin{aligned}
        \Theta_{1}^{\href{#eq-c1}{\textup{C1}}} &=
        \underbracket{
        \begin{bmatrix}
          X_{\alpha + 1}^{0,h + \alpha + 1} \\
          0_{\p{\p{h + 1}\NumEval + m} \times \p{n + \p{\alpha + 1}\NumEval } } \quad I_{\p{h + 1}\NumEval + m}
        \end{bmatrix}
        }_{
        \in \reals^{\p{n+\p{h+1}\NumEval+m} \times \p{n+\p{h + \alpha +2}\NumEval+m}}
        }
        ,
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta1-c1"></a>




(5.40)\[\begin{aligned}
        \theta_{1}^{\href{#eq-c1}{\textup{C1}}} &=
        \underbracket{
        \begin{bmatrix}
            0_{\p{\p{h+1}\NumEvalFunc + \NumFunc} \times \p{\alpha+1}\NumEvalFunc } & I_{\p{h+1}\NumEvalFunc + \NumFunc }
        \end{bmatrix}
        }_{
        \in \reals^{\p{\p{h+1}\NumEvalFunc + \NumFunc}\times\p{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}}
        },
    \end{aligned}\]

<a id="equation-eq-iteration-independent-lyapunov-theta0-c4-mat"></a>




(5.41)\[\begin{split}\begin{aligned}
        \Theta_{0}^{\href{#eq-c4}{\textup{C4}}} &=
        \underbracket{
        \begin{bmatrix}
            I_{n+\p{h+\alpha+2}\NumEval } & 0_{\p{n+\p{h+\alpha+2}\NumEval}\times \NumEval } & 0_{\p{n+\p{h+\alpha+2}\NumEval}\times m} \\
            0_{m \times \p{n+\p{h+\alpha+2}\NumEval}} & 0_{m \times \NumEval } & I_{m}
        \end{bmatrix}
        }_{
        \in \reals^{\p{n+\p{h+\alpha+2}\NumEval+m}\times\p{n+\p{h + \alpha +3}\NumEval+m}}
        },
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta0-c4"></a>




(5.42)\[\begin{split}\begin{aligned}
        \theta_{0}^{\href{#eq-c4}{\textup{C4}}} &=
        \underbracket{
        \begin{bmatrix}
            I_{\p{h+\alpha+2}\NumEvalFunc} & 0_{\p{h+\alpha+2}\NumEvalFunc \times \NumEvalFunc } & 0_{\p{h+\alpha+2}\NumEvalFunc \times \NumFunc } \\
            0_{\NumFunc\times \p{h+\alpha+2}\NumEvalFunc} &0_{\NumFunc\times \NumEvalFunc} & I_{\NumFunc}
        \end{bmatrix}
        }_{
        \in \reals^{\p{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}\times\p{\p{h+\alpha+3}\NumEvalFunc + \NumFunc}}
        },
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta1-c4-mat"></a>




(5.43)\[\begin{split}\begin{aligned}
        \Theta_{1}^{\href{#eq-c4}{\textup{C4}}} &=
        \underbracket{
        \begin{bmatrix}
          X_{1}^{0,h + \alpha + 2} \\
          0_{\p{\p{h + \alpha + 2}\NumEval + m} \times \p{n + \NumEval } } \quad I_{\p{h + \alpha + 2}\NumEval + m}
        \end{bmatrix}
        }_{
        \in \reals^{\p{n+\p{h+\alpha+2}\NumEval+m} \times \p{n+\p{h + \alpha +3}\NumEval+m}}
        }
        ,
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-independent-lyapunov-theta1-c4"></a>




(5.44)\[\begin{aligned}
        \theta_{1}^{\href{#eq-c4}{\textup{C4}}} &=
        \underbracket{
        \begin{bmatrix}
            0_{\p{\p{h+\alpha+2}\NumEvalFunc + \NumFunc} \times \NumEvalFunc } & I_{\p{h+\alpha+2}\NumEvalFunc + \NumFunc }
        \end{bmatrix}
        }_{
        \in \reals^{\p{\p{h+\alpha+2}\NumEvalFunc + \NumFunc}\times\p{\p{h+\alpha+3}\NumEvalFunc + \NumFunc}}
        }.
    \end{aligned}\]

Furthermore, if the interpolation conditions for
\(\p{\mathcal{F}_{i}}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_{i}}_{i\in\IndexOp}\) are tight,


\[\begin{split}\begin{aligned}
        & \textbf{for each}\ \textup{cond} \in\set{\href{#eq-c1}{\textup{C1}},\href{#eq-c2}{\textup{C2}},\href{#eq-c3}{\textup{C3}},\href{#eq-c4}{\textup{C4}}}   \\
        & \quad \dim \calH \geq n + \p{\PEPMaxIter_{\textup{cond}}+1}\NumEval + m, \\
        & \mathbf{end}
    \end{aligned}\end{split}\]

and there exists


\[\begin{split}\begin{aligned}
        & \textbf{for each}\ \textup{cond} \in\set{\href{#eq-c1}{\textup{C1}},\href{#eq-c2}{\textup{C2}},\href{#eq-c3}{\textup{C3}},\href{#eq-c4}{\textup{C4}}}   \\
        & \quad G_{\textup{cond}}\in\sym_{++}^{n + \p{\PEPMaxIter_{\textup{cond}}+1}\NumEval + m}, \\
        & \quad \bchi_{\textup{cond}}\in\reals^{\p{\PEPMaxIter_{\textup{cond}}+1}\NumEvalFunc + \NumFunc}, \\
        & \mathbf{end}
    \end{aligned}\end{split}\]

such that


\[\begin{split}\begin{aligned}
        & \textbf{for each}\ \textup{cond} \in\set{\href{#eq-c1}{\textup{C1}},\href{#eq-c2}{\textup{C2}},\href{#eq-c3}{\textup{C3}},\href{#eq-c4}{\textup{C4}}} \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top}_{\textup{cond}} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-ineq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-ineq}}G_{\textup{cond}}} \leq 0,
        \end{aligned}
        \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top}_{\textup{cond}} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-eq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{func-eq}}G_{\textup{cond}}} = 0,
        \end{aligned}
        \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexOp \\
            \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{0,\PEPMaxIter_{\textup{cond}}}
        \end{array}
        \right) \quad
        \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{0,\,\PEPMaxIter_{\textup{cond}},\,\textup{op}}G_{\textup{cond}}} \leq 0,
        \\
        & \mathbf{end}
    \end{aligned}\end{split}\]

then [(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) is
also a necessary condition.

*Proof.* By induction, we only need to consider the case \(k=0\)
in [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1) to [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4). Additionally, note
that


\[\begin{split}\begin{aligned}
        \p{\bx^{0},\bu^{0},\ldots,\bu^{h},\hat{\bu}^{\star},y^{\star}} &= \p{\Theta_{0}^{\href{#eq-c1}{\textup{C1}}} \kron \Id }\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{0},\ldots,\bFcn^{h},\bFcn^{\star}} &= \theta_{0}^{\href{#eq-c1}{\textup{C1}}}\p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}}, \\
        \p{\bx^{\alpha + 1},\bu^{\alpha + 1},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}} &= \p{\Theta_{1}^{\href{#eq-c1}{\textup{C1}}} \kron \Id }\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{\alpha + 1},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} &= \theta_{1}^{\href{#eq-c1}{\textup{C1}}}\p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}}, \\
        \p{\bx^{0},\bu^{0},\ldots,\bu^{h+\alpha + 1},\hat{\bu}^{\star},y^{\star}} &= \p{\Theta_{0}^{\href{#eq-c4}{\textup{C4}}} \kron \Id }\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} &= \theta_{0}^{\href{#eq-c4}{\textup{C4}}} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}}, \\
        \p{\bx^{1},\bu^{1},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}} &= \p{\Theta_{1}^{\href{#eq-c4}{\textup{C4}}} \kron \Id }\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{1},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}} &= \theta_{1}^{\href{#eq-c4}{\textup{C4}}} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}},
    \end{aligned}\end{split}\]

where we have used
[(5.37)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta0-c1-mat),
[(5.38)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta0-c1),
[(5.39)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c1-mat),
[(5.40)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c1),
[(5.41)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta0-c4-mat),
[(5.42)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta0-c4),
[(5.43)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c4-mat), and
[(5.44)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c4), respectively.

First, suppose that the parameters \(\p{Q,q,S,s}\) are fixed.
Note that

<a id="equation-eq-iteration-independent-lyapunov-obj-c1"></a>

<a id="eq-iteration-independent-lyapunov-obj-c1"></a>



(5.45)\[\begin{split}\begin{aligned}
        &\mathcal{V}\p{Q,q,\alpha+1} - \rho \mathcal{V}\p{Q,q,0} + \mathcal{R}\p{S,s,0} \nonumber \\
        & = \quadform{Q}{\p{\bx^{\alpha + 1},\bu^{\alpha + 1},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}} + q^{\top}\p{\bFcn^{\alpha + 1},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \nonumber \\
        &\quad - \rho
        \quadform{Q}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h},\hat{\bu}^{\star},y^{\star}}} - \rho q^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h},\bFcn^{\star}}
         \nonumber \\
        &\quad + \quadform{S}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}} + s^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \nonumber \\
        & = \quadform{\p{\Theta_{1}^{\href{#eq-c1}{\textup{C1}}}}^{\top}Q\Theta_{1}^{\href{#eq-c1}{\textup{C1}}} - \rho \p{\Theta_{0}^{\href{#eq-c1}{\textup{C1}}}}^{\top}Q\Theta_{0}^{\href{#eq-c1}{\textup{C1}}} + S}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}} \nonumber \\
        &\quad + \p{q^{\top}\p{\theta_{1}^{\href{#eq-c1}{\textup{C1}}} - \rho \theta_{0}^{\href{#eq-c1}{\textup{C1}}} } + s^{\top}} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \nonumber \\
        &  = \quadform{W^{\href{#eq-c1}{\textup{C1}}}}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}}
        + \p{w^{\href{#eq-c1}{\textup{C1}}}}^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \nonumber
    \end{aligned}\end{split}\]

where [(5.29)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c1-mat)
and [(5.30)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c1)
are used in the last equality. Therefore, using
[(5.45)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-obj-c1)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition),
with \(\textup{cond} = \href{#eq-c1}{\textup{C1}}\), is a sufficient
condition for [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1). Note that

<a id="equation-eq-iteration-independent-lyapunov-obj-c2"></a>

<a id="eq-iteration-independent-lyapunov-obj-c2"></a>



(5.46)\[\begin{split}\begin{aligned}
        &\mathcal{V}\p{P,p,0} - \mathcal{V}\p{Q,q,0} \notag \\
        & = \quadform{P-Q}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h},\hat{\bu}^{\star},y^{\star}}}
        + \p{p-q}^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h},\bFcn^{\star}} \notag \\
        & = \quadform{W^{\href{#eq-c2}{\textup{C2}}}}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h},\hat{\bu}^{\star},y^{\star}}}
        + \p{w^{\href{#eq-c2}{\textup{C2}}}}^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h},\bFcn^{\star}}
    \end{aligned}\end{split}\]

where [(5.31)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c2-mat)
and [(5.32)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c2)
are used in the last equality. Therefore, using
[(5.46)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-obj-c2)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition),
with \(\textup{cond} = \href{#eq-c2}{\textup{C2}}\), is a sufficient
condition for [(C2)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c2). Note that

<a id="equation-eq-iteration-independent-lyapunov-obj-c3"></a>

<a id="eq-iteration-independent-lyapunov-obj-c3"></a>



(5.47)\[\begin{split}\begin{aligned}
        & \mathcal{R}\p{T,t,0} - \mathcal{R}\p{S,s,0} \notag \\
        & = \quadform{T-S}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}}
        + \p{t-s}^{\top}\p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \notag \\
        & = \quadform{W^{\href{#eq-c3}{\textup{C3}}}}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}}
        + \p{w^{\href{#eq-c3}{\textup{C3}}}}^{\top} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}}
    \end{aligned}\end{split}\]

where [(5.33)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c3-mat)
and [(5.34)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c3)
are used in the last equality. Therefore, using
[(5.47)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-obj-c3)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition),
with \(\textup{cond} = \href{#eq-c3}{\textup{C3}}\), is a sufficient
condition for [(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3). Note that

<a id="equation-eq-iteration-independent-lyapunov-obj-c4"></a>

<a id="eq-iteration-independent-lyapunov-obj-c4"></a>



(5.48)\[\begin{split}\begin{aligned}
        &\mathcal{R}\p{S,s,1} - \mathcal{R}\p{S,s,0} \notag \\
        &
        = \quadform{S}{\p{\bx^{1},\bu^{1},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}}} + s^{\top}\p{\bFcn^{1},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}} \notag \\
        & \quad -
        \quadform{S}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 1},\hat{\bu}^{\star},y^{\star}}} - s^{\top} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 1},\bFcn^{\star}} \notag \\
        &  = \quadform{\p{\Theta_{1}^{\href{#eq-c4}{\textup{C4}}}}^{\top}S\Theta_{1}^{\href{#eq-c4}{\textup{C4}}} - \p{\Theta_{0}^{\href{#eq-c4}{\textup{C4}}}}^{\top}S\Theta_{0}^{\href{#eq-c4}{\textup{C4}}}}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}}} \notag \\
        & \quad + \p{s^{\top}\p{\theta_{1}^{\href{#eq-c4}{\textup{C4}}} - \theta_{0}^{\href{#eq-c4}{\textup{C4}}} } } \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}} \nonumber \\
        &  = \quadform{W^{\href{#eq-c4}{\textup{C4}}}}{\p{\bx^{0},\bu^{0},\ldots,\bu^{h + \alpha + 2},\hat{\bu}^{\star},y^{\star}}} +  \p{w^{\href{#eq-c4}{\textup{C4}}}}^{\top} \p{\bFcn^{0},\ldots,\bFcn^{h + \alpha + 2},\bFcn^{\star}}
    \end{aligned}\end{split}\]

where [(5.35)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c4-mat)
and [(5.36)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-w-c4)
are used in the last equality. Therefore, using
[(5.48)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-obj-c4)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition),
with \(\textup{cond} = \href{#eq-c4}{\textup{C4}}\), is a sufficient
condition for [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4).

Second, note that the proof is complete if we let the parameters
\(\p{Q,q,S,s}\) free, as
in [(5.28f)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition-q-mat),
[(5.28g)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition-q),
[(5.28h)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition-s-mat), and
[(5.28i)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition-s). \(\square\)

TeX macro definitions used in this page:

```latex
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\PEPMaxIter}{\bar{k}}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bchi}{\boldsymbol{\chi}}
\newcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\kron}{\otimes}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\quadform}[2]{\mathcal{Q}\p{#1,#2}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
\newcommand{\sym}{\mathbb{S}}
\newcommand{\trace}{\mathrm{trace}}
\newcommand{\underbracket}[1]{\underbrace{#1}}
```
