Canonical HTML: https://autolyap.github.io/theory/performance_estimation_via_sdps/
Documentation version: 0.2.1

<a id="performance-estimation-via-sdps"></a>



# 5.1. Performance estimation via SDPs

This page introduces a technical SDP primitive associated with the
algorithm representation in [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity)
and the inclusion problem in [(2.1)](https://autolyap.github.io/theory/problem_class/#equation-eq-the-problem-inclusion).
This primitive is used in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/)
to formulate the search for a Lyapunov analysis as an SDP.
This page can be skipped on a first reading.

For readers interested in the technical details, we first introduce the
matrices needed for this SDP primitive. If \(m\geq 2\), let

<a id="equation-eq-sumtozeromat"></a>

<a id="eq-sumtozeromat"></a>



(5.2)\[\begin{split}\begin{aligned}
    \SumToZeroMat =
    \begin{bmatrix}
        I_{m-1}\\
        -\mathbf{1}_{m-1}^{\top}
    \end{bmatrix}\in\reals^{m\times (m-1)}.\end{aligned}\end{split}\]

For each


\[\PEPMinIter,\PEPMaxIter\in\llbracket0,K\rrbracket, \qquad
\PEPMinIter\leq\PEPMaxIter,\]

we define the matrices


\[X_{k}^{\PEPMinIter, \PEPMaxIter}\in \reals^{n\times\p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}\]

as

<a id="equation-eq-x-mats"></a>

<a id="eq-x-mats"></a>



(5.3)\[\begin{split}\begin{aligned}
    X_{k}^{\PEPMinIter, \PEPMaxIter} =
    \begin{cases}
        \begin{bmatrix}
            I_{n} & 0_{n\times\p{\p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}
        \end{bmatrix} & \text{if } k=\PEPMinIter, \\[0.5em]
        \begin{bmatrix}
            A_{\PEPMinIter} & B_{\PEPMinIter} & 0_{n\times\p{\p{\PEPMaxIter-\PEPMinIter}\NumEval + m}}
        \end{bmatrix} & \text{if } k=\PEPMinIter + 1, \\[0.5em]
        \begin{bmatrix}
            \p{A_{k-1}\cdots A_{\PEPMinIter}}^{\top} \\
            \p{A_{k-1}\cdots A_{\PEPMinIter+1}B_{\PEPMinIter}}^{\top} \\
            \p{A_{k-1}\cdots A_{\PEPMinIter+2}B_{\PEPMinIter+1}}^{\top} \\
            \vdots \\
            \p{A_{k-1}A_{k-2}B_{k-3}}^{\top} \\
            \p{A_{k-1}B_{k-2}}^{\top} \\
            B_{k-1}^{\top} \\
            0_{n \times \p{\p{\PEPMaxIter+1-k}\NumEval + m} }^{\top}
        \end{bmatrix}^{\top}
        &
        \begin{aligned}
            &\text{if } k \in \llbracket\PEPMinIter+2,\PEPMaxIter+1\rrbracket \\
            &\text{and } \PEPMinIter + 1 \leq \PEPMaxIter,
        \end{aligned}
    \end{cases}\end{aligned}\end{split}\]

the matrices


\[Y_{k}^{\PEPMinIter, \PEPMaxIter} \in \reals^{\NumEval\times\p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}\]

as

<a id="equation-eq-y-mats"></a>

<a id="eq-y-mats"></a>



(5.4)\[\begin{split}\begin{aligned}
    Y_{k}^{\PEPMinIter, \PEPMaxIter} =
    \begin{cases}
        \begin{bmatrix}
            C_{\PEPMinIter} & D_{\PEPMinIter} & 0_{\NumEval\times\p{\p{\PEPMaxIter-\PEPMinIter}\NumEval + m}}
        \end{bmatrix} & \text{if } k=\PEPMinIter, \\[0.5em]
        \begin{bmatrix}
            \p{C_{\PEPMinIter+1}A_{\PEPMinIter}}^{\top} \\
            \p{C_{\PEPMinIter+1}B_{\PEPMinIter}}^{\top} \\
            D_{\PEPMinIter+1}^{\top} \\
            0_{\NumEval\times \p{\p{\PEPMaxIter-\PEPMinIter-1}\NumEval + m} }^{\top}
        \end{bmatrix}^{\top}
        &
        \begin{aligned}
            &\text{if } k=\PEPMinIter + 1 \\
            &\text{and } \PEPMinIter + 1 \leq \PEPMaxIter,
        \end{aligned}
         \\[0.5em]
        \begin{bmatrix}
            \p{C_{k}A_{k-1}\cdots A_{\PEPMinIter}}^{\top} \\
            \p{C_{k}A_{k-1}\cdots A_{\PEPMinIter+1}B_{\PEPMinIter}}^{\top} \\
            \p{C_{k}A_{k-1}\cdots A_{\PEPMinIter+2}B_{\PEPMinIter+1}}^{\top} \\
            \vdots \\
            \p{C_{k}A_{k-1}B_{k-2}}^{\top} \\
            \p{C_{k}B_{k-1}}^{\top} \\
            D_{k}^{\top} \\
            0_{\NumEval\times \p{\p{\PEPMaxIter-k}\NumEval + m} }^{\top}
        \end{bmatrix}^{\top}
        &
        \begin{aligned}
            & \text{if } k \in \llbracket\PEPMinIter+2,\PEPMaxIter\rrbracket \\
            &\text{and } \PEPMinIter + 2 \leq \PEPMaxIter,
        \end{aligned}
    \end{cases}\end{aligned}\end{split}\]

the matrix

<a id="equation-eq-y-star"></a>

<a id="eq-y-star"></a>



(5.5)\[\begin{aligned}
    Y_{\star}^{\PEPMinIter, \PEPMaxIter} & =
    \underbracket{
    \begin{bmatrix}
        0_{m\times\p{n+ \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m - 1 } } & \mathbf{1}_{m}
    \end{bmatrix}
    }_{
    \in \reals^{m\times\p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}
    }, \end{aligned}\]

the matrices


\[U_{k}^{\PEPMinIter, \PEPMaxIter}\in\reals^{\NumEval\times\p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}\]

as

<a id="equation-eq-u-mats"></a>

<a id="eq-u-mats"></a>



(5.6)\[\begin{aligned}
    \p{\forall k \in \llbracket\PEPMinIter,\PEPMaxIter\rrbracket}\quad
    U_{k}^{\PEPMinIter, \PEPMaxIter} =
    \begin{bmatrix}
        0_{\NumEval\times\p{n+\p{k-\PEPMinIter}\NumEval} } & I_{\NumEval} & 0_{\NumEval\times\p{ \p{\PEPMaxIter-k}\NumEval + m } }
    \end{bmatrix},\end{aligned}\]

the matrix

<a id="equation-eq-u-star"></a>

<a id="eq-u-star"></a>



(5.7)\[\begin{aligned}
    U_{\star}^{\PEPMinIter, \PEPMaxIter} & =
    \underbracket{
    \begin{bmatrix}
        0_{m\times\p{n+ \p{\PEPMaxIter-\PEPMinIter+1}\NumEval } } & \SumToZeroMat & 0_{m\times 1 }
    \end{bmatrix}
    }_{
    \in \reals^{m\times\p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}}
    }\end{aligned}\]

with the interpretation that the block column containing
\(\SumToZeroMat\) is removed from
\(U_{\star}^{\PEPMinIter, \PEPMaxIter}\) when \(m=1\), the
matrices

<a id="equation-eq-p-mats"></a>

<a id="eq-p-mats"></a>



(5.8)\[\begin{split}\begin{split}
    \Bigp{
        \begin{array}{@{}c@{}}
            \forall i \in\llbracket1,m\rrbracket\\
            \forall j \in\llbracket1,\NumEval_{i}\rrbracket
        \end{array}
        }
    \quad
    P_{\p{i,j}} &=
    \underbracket{
    \begin{bmatrix}
        0_{1 \times \sum_{r=1}^{i-1}\NumEval_{r}} & \p{e_{j}^{\NumEval_{i}}}^{\top} & 0_{1 \times \sum_{r=i+1}^{m}\NumEval_{r}}
    \end{bmatrix}
    }_{
    \in \reals^{1 \times \NumEval}
    },
    \\
    \p{\forall i \in\llbracket1,m\rrbracket}
    \quad
    P_{\p{i,\star}} &= \p{e_{i}^{m}}^{\top} \in \reals^{1 \times m },
\end{split}\end{split}\]

and the matrices


\[F_{\p{i,j,k}}^{\PEPMinIter, \PEPMaxIter},F_{\p{i,\star,\star}}^{\PEPMinIter, \PEPMaxIter}\in \reals^{1\times\p{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}}\]

as

<a id="equation-eq-f-mats"></a>

<a id="eq-f-mats"></a>



(5.9)\[\begin{split}\begin{split}
    \Bigp{
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc\\
            \forall j \in\llbracket1,\NumEval_{i}\rrbracket\\
            \forall k \in \llbracket\PEPMinIter,\PEPMaxIter\rrbracket
        \end{array}
        }
    \quad
    F_{\p{i,j,k}}^{\PEPMinIter, \PEPMaxIter} & =
    \begin{bmatrix}
        0_{1\times\p{\p{k-\PEPMinIter}\NumEvalFunc + \sum_{r=1}^{\kappa\p{i}-1}\NumEval_{\kappa^{-1}\p{r}} }}^{\top} \\[1.5em]
        e_{j}^{\NumEval_{i} } \\
        0_{1 \times \p{ \p{\p{\PEPMaxIter-k} }\NumEvalFunc +  \NumFunc + \sum_{r=\kappa\p{i}+1}^{\NumFunc}\NumEval_{\kappa^{-1}\p{r}} }}^{\top}
    \end{bmatrix}^{\top},
    \\
    \p{\forall i \in\IndexFunc}
    \quad
    F_{\p{i,\star,\star}}^{\PEPMinIter, \PEPMaxIter} & =
    \begin{bmatrix}
        0_{1\times\p{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc}} & \p{e_{\kappa\p{i} }^{\NumFunc}}^{\top}
    \end{bmatrix},
\end{split}\end{split}\]

where \(\kappa: \IndexFunc \to \llbracket1,\NumFunc\rrbracket\) is a
bijective and increasing function (and therefore uniquely specified).

Next, we present the main object of interest. Let


\[\PEPMinIter,\PEPMaxIter\in\llbracket0,K\rrbracket, \qquad
\PEPMinIter\leq\PEPMaxIter,\]


\[\PEPObjMat \in \sym^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}, \qquad
\PEPObjVec \in \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc},\]

and consider the optimization problem

<a id="equation-eq-pep"></a>

<a id="eq-pep"></a>



(5.10)\[\begin{split}\begin{aligned}
 \nonumber
        & \underset{}{\text{maximize}} & &  \quadform{\PEPObjMat}{\p{\bx^{\PEPMinIter},\bu^{\PEPMinIter},\ldots,\bu^{\PEPMaxIter},\hat{\bu}^{\star},y^{\star}}} + \PEPObjVec^{\top}\p{\bFcn^{\PEPMinIter},\ldots,\bFcn^{\PEPMaxIter},\bFcn^{\star}} \\ \nonumber
        & \text{subject to} & & \textbf{for each}\  k \in\llbracket\PEPMinIter,\PEPMaxIter\rrbracket  \\ \nonumber
        & & & \quad \bx^{k+1} = \p{A_{k} \kron \Id} \bx^{k} + \p{B_{k} \kron \Id} \bu^{k}, \\ \nonumber
        & & & \quad \by^{k} =  \p{C_{k} \kron \Id} \bx^{k} + \p{D_{k} \kron \Id} \bu^{k}, \\ \nonumber
        & & & \quad \p{\bu^{k}_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\bm{\partial}\bfcn_{i}\p{\by_{i}^{k}}, \\ \nonumber
        & & & \quad \p{\bu^{k}_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}\bm{G}_{i}\p{\by_{i}^{k}}, \\ \nonumber
        & & & \quad \bFcn^{k} =\p{\bfcn_{i}\p{\by_{i}^{k}} }_{i\in\IndexFunc} \in\reals^{\NumEvalFunc },\\ \nonumber
        & & & \quad \bu^{k}=\p{\bu^{k}_{1},\ldots,\bu^{k}_{m}}\in\prod_{i=1}^{m}\calH^{\NumEval_i}, \\  \nonumber
        & & & \quad \by^{k}=\p{\by^{k}_{1},\ldots,\by^{k}_{m}}\in\prod_{i=1}^{m}\calH^{\NumEval_i}, \\ \nonumber
        & & & \mathbf{end} \\ \nonumber
        & & & \bu^{\star} = \p{u^{\star}_{1},\ldots,u^{\star}_{m}} \in \calH^{m},\\ \nonumber
        & & & y^{\star} \in \calH,\\ \nonumber
        & & & \p{u^{\star}_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\partial f_{i}\p{y^{\star}}, \\ \nonumber
        & & & \p{u^{\star}_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}G_{i}\p{y^{\star}}, \\ \nonumber
        & & & \sum_{i=1}^{m} u^{\star}_{i} = 0,\\ \nonumber
        & & & \hat{\bu}^{\star}=\p{u^{\star}_{1},\ldots,u^{\star}_{m-1}}, \\\nonumber
        & & & \bFcn^{\star} =\p{f_{i}\p{y^{\star}}}_{i\in\IndexFunc}\in\reals^{\NumFunc }, \\ \nonumber
        & & & \p{f_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc} \mathcal{F}_{i}, \\ \nonumber
        & & & \p{G_i}_{i\in\IndexOp} \in \prod_{i\in\IndexOp} \mathcal{G}_i,
    \end{aligned}\end{split}\]

where all quantities except


\[\PEPObjMat, \quad \PEPObjVec, \quad \PEPMinIter, \quad \PEPMaxIter,\]


\[\p{\p{A_{k},B_{k},C_{k},D_{k}}}_{k=\PEPMinIter}^{\PEPMaxIter}, \quad
\calH, \quad
\p{\mathcal{F}_{i}}_{i\in\IndexFunc}, \quad
\p{\mathcal{G}_{i}}_{i\in\IndexOp}\]

are optimization variables.
I.e., [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep) asks for the worst-case value of the
objective or performance measure


\[\quadform{\PEPObjMat}{\p{\bx^{\PEPMinIter},\bu^{\PEPMinIter},\ldots,\bu^{\PEPMaxIter},\hat{\bu}^{\star},y^{\star}}}
+ \PEPObjVec^{\top}\p{\bFcn^{\PEPMinIter},\ldots,\bFcn^{\PEPMaxIter},\bFcn^{\star}}\]

over all trajectories and problem instances that satisfy the algorithm
dynamics and the imposed function/operator class assumptions, respectively.

Note that the last two constraints in [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep) are
infinite-dimensional. In the theorem below, we use
[Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation) to
reduce these
infinite-dimensional constraints to a finite set of quadratic
constraints.
In particular, [(D-PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-dpep) in [Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero) is the SDP primitive discussed above.

<a id="thm-pep-leq-zero"></a>



**Theorem 5.1.1 (Performance estimation via SDP).**

Suppose that
[Assumption 3.1 (Well-posedness)](https://autolyap.github.io/theory/algorithm_representation/#ass-well-posedness) and
[Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation)
hold, let
[(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep)\(^\star\) be the optimal value
of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep), and consider the matrices defined in
[(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats) to [(5.9)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-mats). A
sufficient condition for [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep)\(^\star \leq 0\) is that
the following system

<a id="equation-eq-dpep"></a>

<a id="eq-dpep"></a>



(5.11)\[\begin{split}\begin{aligned}
 \nonumber
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}} \geq 0,
        \\ \nonumber
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}} \in \reals,
        \\ \nonumber
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexOp \\
            \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}} \geq 0,
        \\  \nonumber
        & - W
        + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
        \\ \nonumber
        & \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} \\  \nonumber
        & \quad + \sum_{\substack{i\in\IndexOp \\ o \in \mathcal{O}^{\textup{op}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}}
        W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}
        \succeq 0, \\  \nonumber
        &
        - w
        + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
        \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
        F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}  \\  \nonumber
        & \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
        \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
        F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
        = 0,
    \end{aligned}\end{split}\]

is feasible for the scalars


\[\begin{split}\begin{aligned}
            &\lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}, \\
            &\nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}, \\
            &\lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}},
        \end{aligned}\end{split}\]

where

<a id="equation-eq-w-func-ineq"></a>




(5.12)\[\begin{aligned}
   W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
   &=
   \underbracket{
   \p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
   M_{\p{i,o}}^{\textup{func-ineq}}
   E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}
   }_{
   \in \sym^{ n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m }
   }
   ,
\end{aligned}\]

<a id="equation-eq-w-func-eq"></a>




(5.13)\[\begin{aligned}
   W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
   &=
   \underbracket{
   \p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
   M_{\p{i,o}}^{\textup{func-eq}}
   E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}
   }_{
   \in \sym^{ n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m }
   }
   ,
\end{aligned}\]

<a id="equation-eq-w-op"></a>




(5.14)\[\begin{aligned}
   W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}
   &=
   \underbracket{
   \p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
   M_{\p{i,o}}^{\textup{op}}
   E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}
   }_{
   \in \sym^{ n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m }
   }
   ,
\end{aligned}\]

<a id="equation-eq-f-func-ineq"></a>




(5.15)\[\begin{aligned}
   F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
   &=
   \underbracket{
   \begin{bmatrix}
      \p{F_{\p{i,j_{1},k_{1}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
      & \cdots &
      \p{F_{\p{i,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
   \end{bmatrix}
   a_{\p{i,o}}^{\textup{func-ineq}}
   }_{
   \in \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}
   }
   ,
\end{aligned}\]

<a id="equation-eq-f-func-eq"></a>




(5.16)\[\begin{aligned}
   F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
   &=
   \underbracket{
   \begin{bmatrix}
      \p{F_{\p{i,j_{1},k_{1}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
      & \cdots &
      \p{F_{\p{i,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}}^{\top}
   \end{bmatrix}
   a_{\p{i,o}}^{\textup{func-eq}}
   }_{
   \in \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}
   }
   ,
\end{aligned}\]

<a id="equation-eq-e"></a>




(5.17)\[\begin{split}\begin{aligned}
   E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}
   &=
   \underbracket{
   \begin{bmatrix}
      P_{\p{i,j_{1}}}Y_{k_{1}}^{\PEPMinIter, \PEPMaxIter} \\
      \vdots \\
      P_{\p{i,j_{n_{i,o}}}}Y_{k_{n_{i,o}}}^{\PEPMinIter, \PEPMaxIter} \\
      P_{\p{i,j_{1}}}U_{k_{1}}^{\PEPMinIter, \PEPMaxIter} \\
      \vdots \\
      P_{\p{i,j_{n_{i,o}}}}U_{k_{n_{i,o}}}^{\PEPMinIter, \PEPMaxIter}
   \end{bmatrix}
   }_{
   \in \reals^{2 n_{i,o} \times \p{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m} }
   }
\end{aligned}\end{split}\]

Furthermore, if the interpolation conditions for
\(\p{\mathcal{F}_{i}}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_{i}}_{i\in\IndexOp}\) are tight,
\(\dim \calH \geq n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m\),
and there exists
\(G\in\sym_{++}^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}\)
and
\(\bchi\in\reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}\)
such that


\[\begin{split}\begin{aligned}
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}G} \leq 0,
        \end{aligned}
        \\
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexFunc \\
            \forall o \in \mathcal{O}^{\textup{func-eq}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad
        \begin{aligned}
            & \bm{\chi}^{\top} F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} \\
            & + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}G} = 0,
        \end{aligned}
        \\
        &
        \left(
        \begin{array}{@{}c@{}}
            \forall i \in\IndexOp \\
            \forall o \in \mathcal{O}^{\textup{op}}_{i} \\
            \forall \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}
        \end{array}
        \right) \quad
        \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}G} \leq 0,
    \end{aligned}\end{split}\]

then the feasibility of [(D-PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-dpep) is a necessary
condition for [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep)\(^\star\leq 0\).

*Proof.* We prove [Theorem 5.1.1 (Performance estimation via SDP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#thm-pep-leq-zero) in a
sequence of steps:

<a id="formulating-a-primal-semidefinite-program"></a>



Formulating a primal semidefinite program.

Note that


\[\begin{split}\begin{aligned}
    \bu^{\star} =
    \begin{cases}
        0 & \text{if }m=1, \\
        \p{\SumToZeroMat\kron\Id}\hat{\bu}^{\star} & \text{if }m\geq2,
    \end{cases}\end{aligned}\end{split}\]

for \(\hat{\bu}^{\star}\) as defined in
[(5.1)](https://autolyap.github.io/theory/lyapunov_analyses/#equation-eq-solution) and \(\SumToZeroMat\) given in
[(5.2)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-sumtozeromat).
Moreover, back substitution gives


\[\begin{split}\begin{aligned}
    \bx^{\PEPMinIter+1} {}={}& \p{A_{\PEPMinIter} \kron \Id} \bx^{\PEPMinIter} + \p{B_{\PEPMinIter} \kron \Id} \bu^{\PEPMinIter}, \\
    %\bx^{\PEPMinIter+2} &= \p{A_{\PEPMinIter+1} \kron \Id} \bx^{\PEPMinIter+1} + \p{B_{\PEPMinIter+1} \kron \Id} \bu^{\PEPMinIter+1} \\
    %            &= \p{A_{\PEPMinIter+1}A_{\PEPMinIter} \kron \Id} \bx^{\PEPMinIter} + \p{A_{\PEPMinIter+1}B_{\PEPMinIter} \kron \Id} \bu^{\PEPMinIter} + \p{B_{\PEPMinIter+1} \kron \Id} \bu^{\PEPMinIter+1} \\
    %\bx^{\PEPMinIter+3} &= \p{A_{\PEPMinIter+2} \kron \Id} \bx^{\PEPMinIter+2} + \p{B_{\PEPMinIter+2} \kron \Id} \bu^{\PEPMinIter+2} \\
    %            &= \p{A_{\PEPMinIter+2}A_{\PEPMinIter+1}A_{\PEPMinIter} \kron \Id} \bx^{\PEPMinIter} + \p{A_{\PEPMinIter+2}A_{\PEPMinIter+1}B_{\PEPMinIter} \kron \Id} \bu^{\PEPMinIter} + \p{A_{\PEPMinIter+2}B_{\PEPMinIter+1} \kron \Id} \bu^{\PEPMinIter+1} + \p{B_{\PEPMinIter+2} \kron \Id} \bu^{\PEPMinIter+2} \\
    \p{\forall k \in \llbracket\PEPMinIter+2,\PEPMaxIter+1\rrbracket} \quad \bx^{k} {}={}& \p{A_{k-1}\cdots A_{\PEPMinIter}\kron \Id} \bx^{\PEPMinIter}  \\
    &{} + \sum_{i=\PEPMinIter}^{k-2}\p{A_{k-1}\cdots A_{i+1}B_{i} \kron \Id} \bu^{i} \\
    &{} + \p{B_{k-1} \kron \Id} \bu^{k-1}.\end{aligned}\end{split}\]

Thus, the constraints of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep) can equivalently be
written as


\[\begin{split}\begin{aligned}
    & \bx^{\PEPMinIter} \in \calH^{n}, \\
    & \by^{\PEPMinIter} = \p{C_{\PEPMinIter} \kron \Id} \bx^{\PEPMinIter} + \p{D_{\PEPMinIter} \kron \Id} \bu^{\PEPMinIter}, \\
    & \by^{\PEPMinIter+1} = \p{C_{\PEPMinIter+1}A_{\PEPMinIter} \kron \Id} \bx^{\PEPMinIter} + \p{C_{\PEPMinIter+1}B_{\PEPMinIter} \kron \Id} \bu^{\PEPMinIter} + \p{D_{\PEPMinIter+1} \kron \Id} \bu^{\PEPMinIter+1}, \\
    & \textbf{for each}\  k \in\llbracket\PEPMinIter+2,\PEPMaxIter\rrbracket \\
    & \quad \by^{k} = \p{C_{k}A_{k-1}\cdots A_{\PEPMinIter}\kron \Id} \bx^{\PEPMinIter} + \sum_{i=\PEPMinIter}^{k-2}\p{C_{k}A_{k-1}\cdots A_{i+1}B_{i} \kron \Id} \bu^{i}  \\
    & \quad\quad\quad +\p{C_{k}B_{k-1} \kron \Id} \bu^{k-1} + \p{D_{k} \kron \Id} \bu^{k}, \\
    & \mathbf{end} \\
    & \textbf{for each}\  k \in\llbracket\PEPMinIter,\PEPMaxIter\rrbracket \\
    & \quad \bu^{k}=\p{\bu^{k}_{1},\ldots,\bu^{k}_{m}}\in\prod_{i=1}^{m}\calH^{\NumEval_i},\\
    & \quad \by^{k}=\p{\by^{k}_{1},\ldots,\by^{k}_{m}}\in\prod_{i=1}^{m}\calH^{\NumEval_i},\\
    & \quad \p{\bu^{k}_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\bm{\partial}\bfcn_{i}\p{\by_{i}^{k}},\\
    & \quad \p{\bu^{k}_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}\bm{G}_{i}\p{\by_{i}^{k}},\\
    & \quad \bFcn^{k} =\p{\bfcn_{i}\p{\by_{i}^{k}} }_{i\in\IndexFunc}\in\reals^{\NumEvalFunc },\\
    & \mathbf{end} \\
    & \bu^{\star} = \p{u^{\star}_{1},\ldots,u^{\star}_{m}} =
    \begin{cases}
        0 & \text{if }m=1, \\
        \p{\SumToZeroMat\kron\Id}\hat{\bu}^{\star} & \text{if }m\geq2, \text{ where }\hat{\bu}^{\star} \in \calH^{m-1},
    \end{cases} \\
    & y^{\star} \in \calH,\\
    & \p{u^{\star}_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc}\partial f_{i}\p{y^{\star}}, \\
    & \p{u^{\star}_{i}}_{i\in\IndexOp} \in \prod_{i\in\IndexOp}G_{i}\p{y^{\star}}, \\
    & \bFcn^{\star} =\p{f_{i}\p{y^{\star}}}_{i\in\IndexFunc}\in\reals^{\NumFunc },\\
    & \p{f_{i}}_{i\in\IndexFunc} \in \prod_{i\in\IndexFunc} \mathcal{F}_{i}, \\
    & \p{G_i}_{i\in\IndexOp} \in \prod_{i\in\IndexOp} \mathcal{G}_i, \end{aligned}\end{split}\]

or equivalently

<a id="equation-eq-pep-constraints-1"></a>

<a id="eq-pep-constraints-1"></a>



(5.18)\[\begin{split}\begin{aligned}
 \nonumber
        & \textbf{for each}\  i \in\IndexFunc \\ \nonumber
        & \quad \textbf{for each}\  j \in\llbracket1,\NumEval_{i}\rrbracket \\ \nonumber
        & \quad \quad \textbf{for each}\  k \in\llbracket\PEPMinIter,\PEPMaxIter\rrbracket \\ \nonumber
        & \quad \quad \quad \p{P_{\p{i,j}}U_{k}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta \in \partial f_{i}\p{\p{P_{\p{i,j}}Y_{k}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta},\\ \nonumber
        & \quad \quad \quad F_{\p{i,j,k}}^{\PEPMinIter, \PEPMaxIter} \bm{\chi} = f_{i}\p{\p{P_{\p{i,j}}Y_{k}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta}, \\ \nonumber
        & \quad \quad \mathbf{end} \\ \nonumber
        & \quad \mathbf{end} \\ \nonumber
        & \quad \p{P_{\p{i,\star}}U_{\star}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta \in \partial f_{i}\p{\p{P_{\p{i,\star}}Y_{\star}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta}, \\ \nonumber
        & \quad F_{\p{i,\star,\star}}^{\PEPMinIter, \PEPMaxIter} \bm{\chi} = f_{i}\p{\p{P_{\p{i,\star}}Y_{\star}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta}, \\ \nonumber
        & \quad f_{i} \in \mathcal{F}_{i}, \\ \nonumber
        & \mathbf{end} \\
        & \textbf{for each}\  i \in\IndexOp \\ \nonumber
        & \quad  \textbf{for each}\  j \in\llbracket1,\NumEval_{i}\rrbracket \\ \nonumber
        & \quad \quad \textbf{for each}\  k \in\llbracket\PEPMinIter,\PEPMaxIter\rrbracket \\\nonumber
        & \quad \quad \quad \p{P_{\p{i,j}}U_{k}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta \in G_{i}\p{\p{P_{\p{i,j}}Y_{k}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta},\\ \nonumber
        & \quad \quad \mathbf{end} \\ \nonumber
        & \quad \mathbf{end} \\ \nonumber
        & \quad \p{P_{\p{i,\star}}U_{\star}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta \in G_{i}\p{\p{P_{\p{i,\star}}Y_{\star}^{\PEPMinIter, \PEPMaxIter} \kron \Id}\bzeta}, \\ \nonumber
        & \quad G_i \in \mathcal{G}_i, \\ \nonumber
        & \mathbf{end} \\ \nonumber
        & \bzeta=\p{\bx^{\PEPMinIter},\bu^{\PEPMinIter},\ldots,\bu^{\PEPMaxIter},\hat{\bu}^{\star},y^{\star}}\in\calH^{n}\times\p{\prod_{i=\PEPMinIter}^{\PEPMaxIter}\calH^{\NumEval}}\times\calH^{m-1}\times\calH, \\ \nonumber
        & \bm{\chi} = \p{\bFcn^{\PEPMinIter},\ldots,\bFcn^{\PEPMaxIter},\bFcn^{\star}} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc},\end{aligned}\end{split}\]

where we have used [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats),
[(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star), [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats),
[(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star), [(5.8)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-p-mats), and
[(5.9)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-mats). Using
[Assumption 4.1 (Interpolation conditions)](https://autolyap.github.io/theory/interpolation_conditions/#ass-interpolation),
we get the following relaxation
(an equivalent representation if the interpolation conditions for
\(\p{\mathcal{F}_{i}}_{i\in\IndexFunc}\) and
\(\p{\mathcal{G}_{i}}_{i\in\IndexOp}\) are tight) of
[(5.18)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-constraints-1):

<a id="equation-eq-relaxed-pep-constraints-1"></a>

<a id="eq-relaxed-pep-constraints-1"></a>



(5.19)\[\begin{split}\begin{aligned}
 \nonumber
    & \textbf{for each}\  i \in\IndexFunc \\ \nonumber
    & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad
    \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}} }^{\top} \bm{\chi}
    + \quadform{M_{\p{i,o}}^{\textup{func-ineq}}}{\p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}\kron\Id}\bzeta} \leq 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \quad \textbf{for each}\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad
    \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} }^{\top} \bm{\chi}
    + \quadform{M_{\p{i,o}}^{\textup{func-eq}}}{\p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}\kron\Id}\bzeta} = 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \mathbf{end} \\ \nonumber
    & \textbf{for each}\  i \in\IndexOp \\ \nonumber
    & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{op}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad \quadform{M_{\p{i,o}}^{\textup{op}}}{\p{E_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}}}}^{\PEPMinIter, \PEPMaxIter}\kron\Id}\bzeta} \leq 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \mathbf{end} \\ \nonumber
    & \bzeta\in\calH^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}, \\ \nonumber
    & \bm{\chi} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc},\end{aligned}\end{split}\]

where we have used [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq), [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq), and
[(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e). If we use [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and
[(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op), the constraints in [(5.19)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-relaxed-pep-constraints-1)
can equivalently be written as

<a id="equation-eq-relaxed-pep-constraints-2"></a>

<a id="eq-relaxed-pep-constraints-2"></a>



(5.20)\[\begin{split}\begin{aligned}
 \nonumber
    & \textbf{for each}\  i \in\IndexFunc \\ \nonumber
    & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}\gramFunc\p{{\bzeta}}} \leq 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \quad \textbf{for each}\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}\gramFunc\p{{\bzeta}}} = 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \mathbf{end} \\ \nonumber
    & \textbf{for each}\  i \in\IndexOp \\ \nonumber
    & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{op}}_{i} \\ \nonumber
    & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & \quad \quad \quad \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}\gramFunc\p{{\bzeta}}} \leq 0,\\ \nonumber
    & \quad \quad \mathbf{end} \\ \nonumber
    & \quad \mathbf{end} \\ \nonumber
    & \mathbf{end} \\ \nonumber
    & \bzeta\in\calH^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}, \\ \nonumber
    & \bm{\chi} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}.\end{aligned}\end{split}\]

Next, we consider the objective function of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep).
It can be written as


\[\begin{aligned}
    \quadform{\PEPObjMat}{\p{\bx^{\PEPMinIter},\bu^{\PEPMinIter},\ldots,\bu^{\PEPMaxIter},\hat{\bu}^{\star},y^{\star}}} + \PEPObjVec^{\top}\p{\bFcn^{\PEPMinIter},\ldots,\bFcn^{\PEPMaxIter},\bFcn^{\star}} = \trace\p{\PEPObjMat\gramFunc\p{{\bzeta}} } + \PEPObjVec^{\top}\bm{\chi}.
\end{aligned}\]

If we combine this observation about the objective function
of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep) with the (possibly relaxed) constraints
in [(5.20)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-relaxed-pep-constraints-2),
we conclude that a (possibly relaxed) version
of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep) can be written as

<a id="equation-eq-pep-rewritten"></a>

<a id="eq-pep-rewritten"></a>



(5.21)\[\begin{split}\begin{aligned}
 \nonumber
    & \underset{}{\text{maximize}} & & \trace\p{\PEPObjMat \gramFunc\p{\bzeta}} + \PEPObjVec^{\top}\bm{\chi} \\ \nonumber
    &\text{subject to}&& \textbf{for each}\  i \in\IndexFunc \\ \nonumber
    &&& \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \nonumber
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    &&& \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}\gramFunc\p{{\bzeta}}} \leq 0,\\ \nonumber
    &&& \quad \quad \mathbf{end} \\ \nonumber
    &&& \quad \mathbf{end} \\ \nonumber
    &&& \quad \textbf{for each}\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \nonumber
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    &&& \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}\gramFunc\p{{\bzeta}}} = 0,\\ \nonumber
    &&& \quad \quad \mathbf{end} \\ \nonumber
    &&& \quad \mathbf{end} \\ \nonumber
    &&& \mathbf{end} \\ \nonumber
    &&& \textbf{for each}\  i \in\IndexOp \\ \nonumber
    &&& \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{op}}_{i} \\ \nonumber
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    &&& \quad \quad \quad \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}\gramFunc\p{{\bzeta}}} \leq 0,\\ \nonumber
    &&& \quad \quad \mathbf{end} \\ \nonumber
    &&& \quad \mathbf{end} \\ \nonumber
    &&& \mathbf{end} \\ \nonumber
    &&& \bzeta\in\calH^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}, \\ \nonumber
    &&& \bm{\chi} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}.\end{aligned}\end{split}\]

The problem

<a id="eq-pep-relaxed-interpolation-func-ineq"></a>

<a id="eq-pep-relaxed"></a>
<a id="eq-pep-relaxed-interpolation-func-eq"></a>
<a id="eq-pep-relaxed-interpolation-op"></a>



\begin{align}
    & \underset{}{\text{maximize}} & & \trace\p{\PEPObjMat G} + \PEPObjVec^{\top}\bm{\chi} \notag \\
    &\text{subject to}&& \textbf{for each}\  i \in\IndexFunc \notag \\
    &&& \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \notag \\
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \notag \\
    &&& \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}G} \leq 0,\tag{5.21a}\\
    &&& \quad \quad \mathbf{end} \notag \\
    &&& \quad \mathbf{end} \notag \\
    &&& \quad \textbf{for each}\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \notag \\
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \notag \\
    &&& \quad \quad \quad \p{F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} }^{\top} \bm{\chi}
    + \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}G} = 0,\tag{5.21b}\\
    &&& \quad \quad \mathbf{end} \notag \\
    &&& \quad \mathbf{end} \notag \\
    &&& \mathbf{end} \notag \\
    &&& \textbf{for each}\  i \in\IndexOp \notag \\
    &&& \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{op}}_{i} \notag \\
    &&& \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \notag \\
    &&& \quad \quad \quad \trace\p{W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}G} \leq 0,\tag{5.21c}\\
    &&& \quad \quad \mathbf{end} \notag \\
    &&& \quad \mathbf{end} \notag \\
    &&& \mathbf{end} \notag \\
    &&& G\in\sym^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}_{+}, \notag \\
    &&& \bm{\chi} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}. \notag
\end{align}

is a relaxation of [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-rewritten), and
therefore has optimal value greater than or equal
to the optimal value of [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep).

We will make use of the following fact: If \(\dim\calH\geq k\),
then \(G\in\sym_+^k\) if and only if there exists
\(\bz\in\calH^k\) such that \(G=\gramFunc\p{\bz}\).
As shown in <a id="id1"></a>[[RTBG20](https://autolyap.github.io/theory/performance_estimation_via_sdps/#id175 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Lemma 3.1],
the result for the case \(k=4\) is based on the
Cholesky decomposition of positive semidefinite matrices. The general
case is a straightforward extension. This fact implies that if
\(\dim \calH \geq n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m\),
then [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed) has optimal value equal to that of
[(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-rewritten). Note
that [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed) is a convex semidefinite
program.

<a id="dual-problem"></a>



Dual problem.

For
[(5.21a)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed-interpolation-func-ineq),
[(5.21b)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed-interpolation-func-eq), and
[(5.21c)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed-interpolation-op),
we introduce corresponding dual variables


\[\begin{split}\begin{aligned}
    &\lambda^{\textup{func-ineq}}_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}} \geq 0, \\
    &\nu^{\textup{func-eq}}_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}} \in \reals, \\
    & \lambda^{\textup{op}}_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}} \geq 0,\end{aligned}\end{split}\]

respectively. With this, the objective function of the Lagrange dual
problem of [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed) becomes


\[\begin{split}\begin{aligned}
    & \sup_{ G\in\sym^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}_{+}  }
    \trace
    \left(\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right.
    \left(\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right.
    W
    \\ %%%%%
    & \quad\quad\quad - \sum_{\substack{i\in\IndexFunc \\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} } }
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
    \\ %%%%%
    & \quad\quad\quad - \sum_{\substack{i\in\IndexFunc \\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}  }}
    \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
    \\ %%%%%
    & \quad\quad\quad- \sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}
    \left.\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right)
    G
    \left.\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right)
    \\ %%%%%
    & + \sup_{ \bm{\chi} \in  \reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc} }
    \left(\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right.
    w
    \\ %%%%%
    & \quad\quad\quad - \sum_{\substack{i\in\IndexFunc \\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
    F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
    \\ %%%%%
    & \quad\quad\quad - \sum_{\substack{i\in\IndexFunc \\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter}  }}
    \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
    F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
    \left.\vphantom{\sum_{\substack{i\in\IndexOp \\  o \in \mathcal{O}^{\textup{op}}_{i}}}}\right)^{\top}\bm{\chi}.\end{aligned}\end{split}\]

Since the dual problem is a minimization problem over the dual
variables, we conclude that it can be written as

<a id="equation-eq-pep-relaxed-dual"></a>

<a id="eq-pep-relaxed-dual"></a>



(5.23)\[\begin{split}\begin{aligned}
 \nonumber
    & \underset{}{\text{minimize}} & & 0 \\ \nonumber
    & \text{subject to} & & \textbf{for each}\  i \in\IndexFunc \\ \nonumber
    & & & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \nonumber
    & & & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & & & \quad \quad \quad  \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}} \geq 0, \\ \nonumber
    & & & \quad \quad \mathbf{end} \\ \nonumber
    & & & \quad \mathbf{end} \\ \nonumber
    & & & \quad \textbf{for each}\  o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \nonumber
    & & & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & & & \quad \quad \quad \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}} \in \reals, \\ \nonumber
    & & & \quad \quad \mathbf{end} \\ \nonumber
    & & & \quad \mathbf{end} \\ \nonumber
    & & & \mathbf{end} \\ \nonumber
    & & & \textbf{for each}\  i \in\IndexOp  \\ \nonumber
    & & & \quad  \textbf{for each}\  o \in \mathcal{O}^{\textup{op}}_{i} \\ \nonumber
    & & & \quad \quad \textbf{for each}\  \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} \\ \nonumber
    & & & \quad \quad \quad \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}} \geq 0,\\ \nonumber
    & & & \quad \quad \mathbf{end} \\ \nonumber
    & & & \quad \mathbf{end} \\
    & & & \mathbf{end} \\ \nonumber
    & & & - W
    + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}
    \\ \nonumber
    & & & \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}} \\ \nonumber
    & & & \quad + \sum_{\substack{i\in\IndexOp \\ o \in \mathcal{O}^{\textup{op}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{op}}
    W_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{op}}
    \succeq 0, \\ \nonumber
    & & &
    - w
    + \sum_{\substack{i\in\IndexFunc \\ o \in \mathcal{O}^{\textup{func-ineq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \lambda_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-ineq}}
    F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-ineq}}  \\ \nonumber
    & & & \quad + \sum_{\substack{i\in\IndexFunc  \\ o \in \mathcal{O}^{\textup{func-eq}}_{i} \\ \p{ \p{j_{1},k_{1}}, \ldots, \p{j_{n_{i,o}},k_{n_{i,o}} }}\in\mathcal{J}_{i,o}^{\PEPMinIter,\PEPMaxIter} }}
    \nu_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\textup{func-eq}}
    F_{\p{i,j_{1},k_{1},\ldots,j_{n_{i,o}},k_{n_{i,o}},o}}^{\PEPMinIter,\,\PEPMaxIter,\,\textup{func-eq}}
    = 0.\end{aligned}\end{split}\]

which is a feasibility problem.
Since [(5.23)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-relaxed-dual) is the dual
of [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed), we conclude that if
[(5.23)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-relaxed-dual) is feasible,
then [(PEP)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep)\(^\star\leq 0\).

Next, suppose that the primal
problem [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed) has a Slater point,
i.e., there exists
\(G\in\sym_{++}^{n + \p{\PEPMaxIter-\PEPMinIter+1}\NumEval + m}\)
and
\(\bchi\in\reals^{\p{\PEPMaxIter-\PEPMinIter+1}\NumEvalFunc + \NumFunc}\)
such that
[(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed)
holds. Then there is no duality gap, i.e., strong duality holds
between the primal problem [(5.21)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#eq-pep-relaxed) and
the dual problem [(5.23)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-pep-relaxed-dual).

This concludes the proof. \(\square\)

References

<a id="id2"></a>



<a id="id175"></a>



[[RTBG20](https://autolyap.github.io/theory/performance_estimation_via_sdps/#id1)]

Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. *SIAM Journal on Optimization*, 30(3):2251–2271, January 2020. [doi:10.1137/19m1304854](https://doi.org/10.1137/19m1304854).

TeX macro definitions used in this page:

```latex
\newcommand{\Bigp}[1]{\mathord{\left( #1 \right)}}
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\PEPMaxIter}{\bar{k}}
\newcommand{\PEPMinIter}{\underline{k}}
\newcommand{\PEPObjMat}{W}
\newcommand{\PEPObjVec}{w}
\newcommand{\SumToZeroMat}{N}
\newcommand{\bFcn}{\mathbf{F}}
\newcommand{\bchi}{\boldsymbol{\chi}}
\newcommand{\bfcn}{\mathbf{f}}
\newcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\bz}{\mathbf{z}}
\newcommand{\bzeta}{\boldsymbol{\zeta}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\gramFunc}{\mathtt{G}}
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
