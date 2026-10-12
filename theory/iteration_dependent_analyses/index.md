Canonical HTML: https://autolyap.github.io/theory/iteration_dependent_analyses/
Documentation version: 0.2.1

<a id="iteration-dependent-analyses"></a>



# 5.3. Iteration-dependent analyses

Here, we focus on finite-horizon analyses, which apply to
algorithms that run indefinitely, as well as algorithms with finite
iteration budgets matching or exceeding the horizon. For notational
simplicity, we assume that


\[K\in\mathbb{N}\]

in
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity),
representing the horizon.

Following <a id="id1"></a>[[TB19](https://autolyap.github.io/theory/iteration_dependent_analyses/#id195 "Adrien Taylor and Francis Bach. Stochastic first-order methods: Non-asymptotic and computer-aided analyses via potential functions. In Conference on Learning Theory (COLT). 2019.")], we adopt an ansatz
consisting of a sequence of iteration-dependent and quadratic Lyapunov
functions, also often referred to as *potential functions*
<a id="id2"></a>[[BG19](https://autolyap.github.io/theory/iteration_dependent_analyses/#id19 "Nikhil Bansal and Anupam Gupta. Potential-function proofs for gradient methods. Theory of Computing, 15(1):1–32, 2019. doi:10.4086/toc.2019.v015a004.")].

<a id="def-iteration-dependent-lyapunov-chain"></a>



**Definition 5.3.1 (Chained Lyapunov inequalities).**

Suppose that
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness) holds. Let


\[\bx_{0}\in\calH^{n}, \qquad
\p{ ( \bx^{k}, \bu^{k}, \by^{k}, \bFcn^{k} ) }_{k=0}^{K},\]

be an initial point and a sequence of iterates satisfying
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity), and let


\[\p{y^{\star}, \hat{\bu}^{\star}, \bFcn^{\star}}\]

be a point satisfying [(5.1)](https://autolyap.github.io/theory/lyapunov_analyses/#equation-eq-solution). Also let


\[c_K\in\reals_{+}.\]

Define

<a id="equation-eq-iteration-dependent-lyapunov-chain-v"></a>

<a id="eq-iteration-dependent-lyapunov-chain-v"></a>



(5.49)\[\begin{split}\begin{aligned}
        \p{\forall k \in \llbracket0,K\rrbracket} \quad
        \left[
        \begin{aligned}
            &\mathcal{V}(Q_k,q_k,k) {}={} \quadform{Q_{k}}{\p{\bx^{k},\bu^{k},\hat{\bu}^{\star},y^{\star}}} + q_{k}^{\top}\p{\bFcn^{k},\bFcn^{\star}}, \\
            &Q_{k} \in \sym^{n+\NumEval+m}, \\
            &q_{k} \in \reals^{\NumEvalFunc + \NumFunc}.
        \end{aligned}
        \right.
    \end{aligned}\end{split}\]

We say that \((\p{Q_{k},q_{k}})_{k=0}^{K}\) and \(c_K\) satisfy
a *length* \(K\) *sequence of chained Lyapunov inequalities for
algorithm*
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
*over the problem class defined by*
\((\mathcal{F}_i)_{i\in\IndexFunc}\) *and*
\((\mathcal{G}_i)_{i\in\IndexOp}\) if

<a id="equation-eq-iteration-dependent-lyapunov-chain"></a>

<a id="eq-iteration-dependent-lyapunov-chain"></a>



(5.50)\[\begin{split}\begin{aligned}
        \mathcal{V}\p{Q_K,q_K,K} &\leq \mathcal{V}\p{Q_{K-1},q_{K-1},K-1} \\
        &\leq \ldots \\
        &\leq \mathcal{V}\p{Q_1,q_1,1} \\
        &\leq c_K \mathcal{V}\p{Q_0,q_0,0}
    \end{aligned}\end{split}\]

holds for each


\[\bx_{0}, \qquad
\p{ ( \bx^{k}, \bu^{k}, \by^{k}, \bFcn^{k} ) }_{k=0}^{K}, \qquad
\p{y^{\star}, \hat{\bu}^{\star},\bFcn^{\star}},\]

and for each


\[\p{f_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc} \mathcal{F}_{i},
\qquad
\p{G_i}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}\mathcal{G}_i.\]

In the proposed methodology, the user specifies the boundary parameters
\(\p{Q_{0},q_{0},Q_{K},q_{K}}\), which define the initial and final
Lyapunov expressions \(\mathcal{V}\p{Q_0,q_0,0}\) and
\(\mathcal{V}\p{Q_K,q_K,K}\). The SDP then searches for
\((\p{Q_{k},q_{k}})_{k=1}^{K-1}\) and the smallest \(c_K\) such that
[(5.50)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-chain) holds. When feasible, this yields


\[\begin{aligned}
    \mathcal{V}\p{Q_K,q_K,K} \leq c_K \mathcal{V}\p{Q_0,q_0,0}.\end{aligned}\]

For built-in choices of these boundary terms:

- Distance to solution:
  [`Distance-to-solution helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_distance_to_solution "autolyap.IterationDependent.get_parameters_distance_to_solution").
- State-component distance to solution:
  [`State-component-distance helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_distance_to_solution "autolyap.IterationDependent.get_parameters_state_component_distance_to_solution").
- State-component same-iteration difference:
  [`State-component-difference helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_difference "autolyap.IterationDependent.get_parameters_state_component_difference").
- State-component cross-iteration difference:
  [`State-component-cross-iteration-difference helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference "autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference").
- Function-value suboptimality:
  [`Function-value-suboptimality helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_function_value_suboptimality "autolyap.IterationDependent.get_parameters_function_value_suboptimality").
- Fixed-point residual:
  [`Fixed-point-residual helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_fixed_point_residual "autolyap.IterationDependent.get_parameters_fixed_point_residual").
- Optimality measure:
  [`Optimality-measure helper`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_optimality_measure "autolyap.IterationDependent.get_parameters_optimality_measure").

The corresponding API entry point is
[`search_lyapunov()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.search_lyapunov "autolyap.IterationDependent.search_lyapunov").

The SDP characterization is given in
[Theorem 5.3.2 (Chained Lyapunov inequalities via SDP)](https://autolyap.github.io/theory/iteration_dependent_analyses/#thm-iteration-dependent-lyapunov). Computationally,
each inequality in [(5.50)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-chain) is enforced by
a one-step analysis with positive semidefinite constraints of constant
dimension. As a result, the number of variables and constraints grows
linearly with \(K\).

<a id="thm-iteration-dependent-lyapunov"></a>



**Theorem 5.3.2 (Chained Lyapunov inequalities via SDP).**

Suppose that
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness) and
[Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation)
hold, and


\[K\in\mathbb{N}, \qquad
Q_{0},Q_{K} \in \sym^{n+\NumEval+m}, \qquad
q_{0},q_{K} \in \reals^{\NumEvalFunc + \NumFunc}.\]

Then a sufficient condition for there to exist


\[\p{ (Q_{k},q_{k}) }_{k=1}^{K-1}, \qquad c_K,\]

such that \(\p{ (Q_{k},q_{k}) }_{k=0}^{K}\) and \(c_K\)
satisfy a length \(K\) sequence of chained Lyapunov
inequalities for algorithm
[(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
over the problem class defined by
\(\p{\mathcal{F}_i}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_i}_{i\in\IndexOp}\) is that the following
system of constraints, which reuses the lifted
matrices/vectors from [5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/), including the lifted state
matrices [(5.3)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-x-mats), and the interpolation terms
[(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op),
[(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq), and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq),

<a id="eq-iteration-dependent-lyapunov-condition-func-ineq"></a>

<a id="eq-iteration-dependent-lyapunov-condition"></a>
<a id="eq-iteration-dependent-lyapunov-condition-func-eq"></a>
<a id="eq-iteration-dependent-lyapunov-condition-op"></a>
<a id="eq-iteration-dependent-lyapunov-condition-psd"></a>
<a id="eq-iteration-dependent-lyapunov-condition-zero"></a>
<a id="eq-iteration-dependent-lyapunov-condition-q-mat"></a>
<a id="eq-iteration-dependent-lyapunov-condition-q"></a>
<a id="eq-iteration-dependent-lyapunov-condition-c-k"></a>



\begin{align}
            &\textbf{for each}\ k \in \llbracket 0, K-1 \rrbracket \notag \\
            &\quad
            \left(
            \begin{array}{@{}c@{}}
                \forall i \in\IndexFunc \\
                \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
                \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
            \end{array}
            \right) \quad \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,k} \geq 0, \tag{5.51a}\\
            &\quad
            \left(
            \begin{array}{@{}c@{}}
                \forall i \in\IndexFunc \\
                \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
                \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
            \end{array}
            \right) \quad
            \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,k} \in \reals, \tag{5.51b}\\
            &\quad
            \left(
            \begin{array}{@{}c@{}}
                \forall i \in\IndexOp \\
                \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
                \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
            \end{array}
            \right) \quad
            \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,k} \geq 0, \tag{5.51c}\\
            &\quad - W_{k}
            + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1} }}
            \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,k}
            W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-ineq}} \notag \\
            &\quad \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1} }}
            \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,k}
            W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-eq}} \notag \\
            &\quad \quad + \sum_{\substack{i\in\IndexOp \\ o \in \mathcal{O}^{\textup{op}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1} }}
            \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,k}
            W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{op}}
            \succeq 0, \tag{5.51d}\\
            &\quad
            - w_{k}
            + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1} }}
            \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,k}
            F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-ineq}} \notag \\
            &\quad \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1} }}
            \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,k}
            F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-eq}}
            = 0, \tag{5.51e}\\
            &\mathbf{end} \notag \\
            & \p{\forall k\in \llbracket1,K-1\rrbracket} \quad Q_{k} \in \sym^{n+\NumEval+m}, \tag{5.51f}\\
            & \p{\forall k\in \llbracket1,K-1\rrbracket} \quad q_{k} \in \mathbb{R}^{\NumEvalFunc + \NumFunc}, \tag{5.51g}\\
            & c_K \in \reals_{+}. \tag{5.51h}
        \end{align}

is feasible for the scalars


\[\begin{split}\begin{aligned}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq},\,k}, \\
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq},\,k}, \\
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op},\,k},
    \end{aligned}\end{split}\]

the matrices and vectors \(\p{\p{Q_{k},q_{k}}}_{k=1}^{K-1}\), and
the scalar \(c_K\), where we have introduced

<a id="equation-eq-iteration-dependent-lyapunov-w-0-mat"></a>




(5.52)\[\begin{aligned}
        W_{0} &= \p{\Theta_{1}^{\p{0}}}^{\top}Q_{1}\Theta_{1}^{\p{0}}
        - c_K \Theta_{0}^{\top}Q_{0}\Theta_{0}.
    \end{aligned}\]

<a id="equation-eq-iteration-dependent-lyapunov-w-0-vec"></a>




(5.53)\[\begin{aligned}
        w_{0} &= \theta_{1}^{\top}q_{1} - c_K\theta_{0}^{\top}q_{0}.
    \end{aligned}\]

<a id="equation-eq-iteration-dependent-lyapunov-w-k-mat"></a>




(5.54)\[\begin{aligned}
        \p{\forall k\in\llbracket1,K-1\rrbracket}\quad
        W_{k} &= \p{\Theta_{1}^{\p{k}}}^{\top}Q_{k+1}\Theta_{1}^{\p{k}}
        - \Theta_{0}^{\top}Q_{k}\Theta_{0}.
    \end{aligned}\]

<a id="equation-eq-iteration-dependent-lyapunov-w-k-vec"></a>




(5.55)\[\begin{aligned}
        \p{\forall k\in\llbracket1,K-1\rrbracket}\quad
        w_{k} &= \theta_{1}^{\top}q_{k+1} - \theta_{0}^{\top}q_{k}.
    \end{aligned}\]

and

<a id="equation-eq-iteration-dependent-lyapunov-theta0-mat"></a>




(5.56)\[\begin{split}\begin{aligned}
        \Theta_{0} &=
        \underbracket{
        \begin{bmatrix}
            I_{n+\NumEval } & 0_{\p{n+\NumEval}\times\NumEval } & 0_{\p{n+\NumEval}\times m} \\
            0_{m \times \p{n+\NumEval}} & 0_{m \times\NumEval } & I_{m}
        \end{bmatrix}
        }_{
        \in \reals^{ \p{n + \NumEval + m} \times \p{n + 2\NumEval + m} }
        }.
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-dependent-lyapunov-theta0-vec"></a>




(5.57)\[\begin{split}\begin{aligned}
        \theta_{0} &=
        \underbracket{
        \begin{bmatrix}
            I_{\NumEvalFunc} & 0_{\NumEvalFunc \times \NumEvalFunc } & 0_{\NumEvalFunc \times \NumFunc } \\
            0_{\NumFunc\times \NumEvalFunc} &0_{\NumFunc\times \NumEvalFunc} & I_{\NumFunc}
        \end{bmatrix}
        }_{
        \in \reals^{\p{\NumEvalFunc + \NumFunc}\times\p{2\NumEvalFunc + \NumFunc}}
        }.
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-dependent-lyapunov-theta1-mat"></a>




(5.58)\[\begin{split}\begin{aligned}
        \p{\forall k\in\llbracket0,K-1\rrbracket}\quad
        \Theta_{1}^{\p{k}} &=
        \underbracket{
        \begin{bmatrix}
            X_{k+1}^{k,k+1} \\
            0_{\p{\NumEval + m} \times \p{n + \NumEval } } \quad I_{\NumEval + m}
        \end{bmatrix}
        }_{
        \in \reals^{ \p{n + \NumEval + m} \times \p{n + 2\NumEval + m} }
        }.
    \end{aligned}\end{split}\]

<a id="equation-eq-iteration-dependent-lyapunov-theta1-vec"></a>




(5.59)\[\begin{aligned}
        \theta_{1} &=
        \underbracket{
        \begin{bmatrix}
            0_{\p{\NumEvalFunc + \NumFunc}\times\NumEvalFunc} & I_{\NumEvalFunc + \NumFunc}
        \end{bmatrix}
        }_{
        \in \reals^{\p{\NumEvalFunc + \NumFunc}\times\p{2\NumEvalFunc + \NumFunc}}
        }.
    \end{aligned}\]

Furthermore, if the interpolation conditions for
\(\p{\mathcal{F}_{i}}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_{i}}_{i\in\IndexOp}\) are tight,
\(\dim \calH \geq n + 2\NumEval + m\), and there exists


\[\begin{split}\begin{aligned}
        \p{\forall k\in \llbracket0,K-1\rrbracket}
        \quad
        \left[
        \begin{aligned}
            &G_{k}\in\sym_{++}^{n + 2\NumEval + m}, \\
            &\bchi_{k}\in\reals^{2\NumEvalFunc + \NumFunc},
        \end{aligned}
        \right.
    \end{aligned}\end{split}\]

such that


\[\begin{split}\begin{aligned}
        & \textbf{for each}\ k\in \llbracket0,K-1\rrbracket \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top}_{k} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-ineq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-ineq}}G_{k}} \leq 0,
        \end{aligned}
        \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top}_{k} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-eq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{func-eq}}G_{k}} = 0,
        \end{aligned}
        \\
        & \quad
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexOp \\
            \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{k,k+1}
        \end{array}
        \right) \quad
        \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{k,\,k+1,\,\textup{op}}G_{k}} \leq 0,
        \\
        & \mathbf{end}
    \end{aligned}\end{split}\]

then [(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) is
also a necessary condition.

*Proof.* Note that


\[\begin{split}\begin{aligned}
        \p{\bx^{k},\bu^{k},\hat{\bu}^{\star},y^{\star}} &= \p{\Theta_{0} \kron \Id }\p{\bx^{k},\bu^{k},\bu^{k+1},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{k},\bFcn^{\star}} &= \theta_{0}\p{\bFcn^{k},\bFcn^{k+1},\bFcn^{\star}}, \\
        \p{\bx^{k+1},\bu^{k+1},\hat{\bu}^{\star},y^{\star}} &= \p{ \Theta_{1}^{\p{k}} \kron \Id }\p{\bx^{k},\bu^{k},\bu^{k+1},\hat{\bu}^{\star},y^{\star}}, \\
        \p{\bFcn^{k+1},\bFcn^{\star}} &= \theta_{1}\p{\bFcn^{k},\bFcn^{k+1},\bFcn^{\star}},
    \end{aligned}\end{split}\]

where we have used [(5.56)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta0-mat),
[(5.57)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta0-vec),
[(5.58)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta1-mat), and
[(5.59)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta1-vec), respectively.

First, suppose that the parameters
\(\p{\p{Q_{k},q_{k}}}_{k=1}^{K-1}\) and \(c_K\) are fixed and
consider \(\p{\mathcal{V}\p{Q_k,q_k,k}}_{k=0}^{K}\) in
[(5.49)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-chain-v).
Note that

<a id="equation-eq-iteration-dependent-lyapunov-obj-0"></a>

<a id="eq-iteration-dependent-lyapunov-obj-0"></a>



(5.60)\[\begin{split}\begin{aligned}
        &\mathcal{V}\p{Q_1,q_1,1} - c_K\mathcal{V}\p{Q_0,q_0,0} \notag\\
        & = \quadform{\p{\Theta_{1}^{\p{0}}}^{\top}Q_{1}\Theta_{1}^{\p{0}}  - c_K\Theta_{0}^{\top}Q_{0}\Theta_{0}}{\p{\bx^{0},\bu^{0},\bu^{1},\hat{\bu}^{\star},y^{\star}}}  \notag \\
        & \quad + \p{q_{1}^{\top}\theta_{1} - c_K q_{0}^{\top}\theta_{0} } \p{\bFcn^{0},\bFcn^{1},\bFcn^{\star}} \notag \\
        &= \quadform{W_{0}}{\p{\bx^{0},\bu^{0},\bu^{1},\hat{\bu}^{\star},y^{\star}}} + w_{0}^{\top} \p{\bFcn^{0},\bFcn^{1},\bFcn^{\star}}
    \end{aligned}\end{split}\]

where [(5.52)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-w-0-mat)
and [(5.53)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-w-0-vec)
are used in the last equality. Therefore, using
[(5.60)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-obj-0)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition),
with \(k=0\), is a sufficient condition for the inequality
\(\mathcal{V}\p{Q_1,q_1,1} \leq c_K\mathcal{V}\p{Q_0,q_0,0}\).

For \(k\in \llbracket1,K-1\rrbracket\), note that

<a id="equation-eq-iteration-dependent-lyapunov-obj-k"></a>

<a id="eq-iteration-dependent-lyapunov-obj-k"></a>



(5.61)\[\begin{split}\begin{aligned}
        &\mathcal{V}\p{Q_{k+1},q_{k+1},k+1} - \mathcal{V}\p{Q_k,q_k,k} \notag \\
        & =\quadform{Q_{k+1}}{\p{\bx^{k+1},\bu^{k+1},\hat{\bu}^{\star},y^{\star}}}  + q_{k+1}^{\top}\p{\bFcn^{k+1},\bFcn^{\star}} \notag\\
        &\quad -\quadform{Q_{k}}{\p{\bx^{k},\bu^{k},\hat{\bu}^{\star},y^{\star}}}  - q_{k}^{\top} \p{\bFcn^{k},\bFcn^{\star}} \notag \\
        & = \quadform{\p{\Theta_{1}^{\p{k}}}^{\top}Q_{k+1}\Theta_{1}^{\p{k}} - \Theta_{0}^{\top}Q_{k}\Theta_{0}}{\p{\bx^{k},\bu^{k},\bu^{k+1},\hat{\bu}^{\star},y^{\star}}} \notag\\
        &\quad + \p{q_{k+1}^{\top}\theta_{1} - q_{k}^{\top}\theta_{0} } \p{\bFcn^{k},\bFcn^{k+1},\bFcn^{\star}} \notag \\
        & = \quadform{W_k}{\p{\bx^{k},\bu^{k},\bu^{k+1},\hat{\bu}^{\star},y^{\star}}} + w_k^{\top}\p{\bFcn^{k},\bFcn^{k+1},\bFcn^{\star}}
    \end{aligned}\end{split}\]

where [(5.54)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-w-k-mat)
and [(5.55)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-w-k-vec)
are used in the last equality. Therefore, using
[(5.61)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-obj-k)
as the objective function in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep),
[Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero)
gives that [(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition),
constrained to this particular \(k\), is a sufficient condition
for the inequality \(\mathcal{V}\p{Q_{k+1},q_{k+1},k+1} \leq \mathcal{V}\p{Q_k,q_k,k}\).

Second, note that the proof is complete if we let the parameters
\(\p{\p{Q_{k},q_{k}}}_{k=1}^{K-1}\) and \(c_K\) free, as
in [(5.51f)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition-q-mat),
[(5.51g)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition-q), and
[(5.51h)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition-c-k). \(\square\)

References

<a id="id3"></a>



<a id="id19"></a>



[[BG19](https://autolyap.github.io/theory/iteration_dependent_analyses/#id2)]

Nikhil Bansal and Anupam Gupta. Potential-function proofs for gradient methods. *Theory of Computing*, 15(1):1–32, 2019. [doi:10.4086/toc.2019.v015a004](https://doi.org/10.4086/toc.2019.v015a004).

<a id="id195"></a>



[[TB19](https://autolyap.github.io/theory/iteration_dependent_analyses/#id1)]

Adrien Taylor and Francis Bach. Stochastic first-order methods: Non-asymptotic and computer-aided analyses via potential functions. In *Conference on Learning Theory (COLT)*. 2019.

TeX macro definitions used in this page:

```latex
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bchi}{\boldsymbol{\chi}}
\newcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\kron}{\otimes}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\quadform}[2]{\mathcal{Q}\p{#1,#2}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\sym}{\mathbb{S}}
\newcommand{\trace}{\mathrm{trace}}
\newcommand{\underbracket}[1]{\underbrace{#1}}
```
