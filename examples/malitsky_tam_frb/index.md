Canonical HTML: https://autolyap.github.io/examples/malitsky_tam_frb/
Documentation version: 0.2.1

<a id="the-malitsky-tam-forward-reflected-backward-method"></a>



# The Malitsky–Tam forward-reflected-backward method

<a id="problem-setup"></a>



## Problem setup

Consider the monotone inclusion


\[\text{find } x \in \calH \text{ such that } 0 \in G_1(x) + G_2(x),\]

where:

- \(G_1 : \calH \to \calH\) is monotone and
  \(L\)-Lipschitz continuous, with \(L>0\),
- \(G_2 : \calH \rightrightarrows \calH\) is \(\mu\)-strongly monotone
  and maximally monotone, with \(\mu>0\).

For an initial pair \((x^{-1},x^0)\in\calH^2\) and step size
\(\gamma \in \reals_{++}\), the Malitsky–Tam forward-reflected-backward method
<a id="id1"></a>[[MT20](https://autolyap.github.io/examples/malitsky_tam_frb/#id125 "Yura Malitsky and Matthew K. Tam. A forward-backward splitting method for monotone inclusions without cocoercivity. SIAM Journal on Optimization, 30(2):1451–1472, January 2020. doi:10.1137/18m1207260.")] is given by


\[(\forall k \in \naturals)\quad
x^{k+1} = J_{\gamma G_2}\!\left(x^k - 2\gamma G_1(x^k) + \gamma G_1(x^{k-1})\right).\]

In this example, we fix \(\mu=1\), \(L=1\), sweep
\(\gamma \in (0,1]\), and search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[\|x^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[x^\star \in \zer(G_1 + G_2).\]

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(G_1\) is modeled as an intersection of
  [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone") and
  [`LipschitzOperator`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.LipschitzOperator "autolyap.problemclass.LipschitzOperator").
- \(G_2\) is modeled by
  [`StronglyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.StronglyMonotone "autolyap.problemclass.StronglyMonotone").
- The full inclusion is built with
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`MalitskyTamFRB`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.MalitskyTamFRB "autolyap.algorithms.MalitskyTamFRB").
- Distance-to-solution parameters are obtained with
  [`IterationIndependent.LinearConvergence.get_parameters_distance_to_solution`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.get_parameters_distance_to_solution "autolyap.IterationIndependent.LinearConvergence.get_parameters_distance_to_solution").
- The contraction factor is searched with
  [`IterationIndependent.LinearConvergence.bisection_search_rho`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.bisection_search_rho "autolyap.IterationIndependent.LinearConvergence.bisection_search_rho").

<a id="run-the-iteration-independent-analysis"></a>



## Run the iteration-independent analysis

This example uses the MOSEK Fusion backend (`backend="mosek_fusion"`).
Install the optional MOSEK dependency first:



```bash
pip install "autolyap[mosek]"
```





```python
from autolyap import IterationIndependent, SolverOptions
from autolyap.algorithms import MalitskyTamFRB
from autolyap.problemclass import (
    InclusionProblem,
    LipschitzOperator,
    MaximallyMonotone,
    StronglyMonotone,
)

mu = 1.0
L = 1.0
gamma = 0.2

problem = InclusionProblem(
    [
        [MaximallyMonotone(), LipschitzOperator(L=L)],  # G1
        StronglyMonotone(mu=mu),  # G2
    ]
)
algorithm = MalitskyTamFRB(gamma=gamma)
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

P, T = IterationIndependent.LinearConvergence.get_parameters_distance_to_solution(
    algorithm
)

result = IterationIndependent.LinearConvergence.bisection_search_rho(
    problem,
    algorithm,
    P,
    T,
    S_equals_T=True,
    s_equals_t=True,
    remove_C3=True,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate in the requested rho interval.")

rho_autolyap = result["rho"]

if not (0.0 < gamma < 1.0 / (2.0 * L)):
    raise ValueError("Theorem 2.9 rate expression requires 0 < gamma < 1/(2L).")

epsilon = min(0.5 - gamma * L, 5.0 * mu * gamma)
alpha = min(1.0 + 4.0 * mu * gamma - 0.75 * epsilon, 1.0 + 0.5 * epsilon)
rho_theory = 1.0 / alpha

print(f"rho (AutoLyap): {rho_autolyap:.8f}")
print(f"rho (theory):   {rho_theory:.8f}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: certified contraction factor when feasible.
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

The computed value `rho (AutoLyap)` is compared against the rate
expression from
<a id="id2"></a>[[MT20](https://autolyap.github.io/examples/malitsky_tam_frb/#id125 "Yura Malitsky and Matthew K. Tam. A forward-backward splitting method for monotone inclusions without cocoercivity. SIAM Journal on Optimization, 30(2):1451–1472, January 2020. doi:10.1137/18m1207260."), Theorem 2.9].
Under the step-size condition \(0<\gamma<1/(2L)\), define


\[\varepsilon = \min\!\left\{\frac{1}{2}-\gamma L,\;5\mu\gamma\right\},
\qquad
\alpha = \min\!\left\{1+4\mu\gamma-\frac{3}{4}\varepsilon,\;1+\frac{1}{2}\varepsilon\right\},\]

and set


\[\rho_{\mathrm{MT}} = \frac{1}{\alpha}.\]

Then the theoretical bound is


\[\|x^k - x^\star\|^2 \in \mathcal{O}(\rho_{\mathrm{MT}}^k) \quad \textup{ as } \quad k\to\infty,\]

where \(x^\star \in \zer(G_1 + G_2)\).

Sweeping over 100 values of \(\gamma\) on \((0,1]\) gives the plot
below, with the Theorem 2.9-derived expression (on
\(0<\gamma<1/(2L)\)) in black and AutoLyap certificates as blue dots.

[![Malitsky-Tam FRB rho versus gamma with theorem-derived line and AutoLyap points.](https://autolyap.github.io/_images/malitsky_tam_frb_rho_vs_gamma.svg)](https://autolyap.github.io/_images/malitsky_tam_frb_rho_vs_gamma.svg)

<a id="references"></a>



## References

<a id="id3"></a>



<a id="id125"></a>



[MT20]
([1](https://autolyap.github.io/examples/malitsky_tam_frb/#id1),[2](https://autolyap.github.io/examples/malitsky_tam_frb/#id2))

Yura Malitsky and Matthew K. Tam. A forward-backward splitting method for monotone inclusions without cocoercivity. *SIAM Journal on Optimization*, 30(2):1451–1472, January 2020. [doi:10.1137/18m1207260](https://doi.org/10.1137/18m1207260).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\zer}{\operatorname*{zer}}
```
