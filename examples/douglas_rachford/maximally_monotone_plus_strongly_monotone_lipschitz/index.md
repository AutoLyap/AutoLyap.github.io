Canonical HTML: https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_plus_strongly_monotone_lipschitz/
Documentation version: 0.2.1

<a id="the-douglas-rachford-method-maximally-monotone-strongly-monotone-lipschitz"></a>



# The Douglas–Rachford method: Maximally monotone + strongly monotone/Lipschitz

<a id="problem-setup"></a>



## Problem setup

Consider the monotone inclusion


\[\text{find } x \in \calH \text{ such that } 0 \in G_1(x) + G_2(x),\]

where:

- \(G_1 : \calH \rightrightarrows \calH\) is maximally monotone,
- \(G_2 : \calH \to \calH\) is both \(\mu\)-strongly monotone and
  \(L\)-Lipschitz, with \(0 < \mu \le L\).

For an initial point \(x^0 \in \calH\), step size
\(\gamma \in \reals_{++}\), and relaxation
\(\lambda \in \reals\), the Douglas–Rachford update is


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
v^k &= J_{\gamma G_1}(x^k), \\
w^k &= J_{\gamma G_2}(2v^k - x^k), \\
x^{k+1} &= x^k + \lambda (w^k - v^k).
\end{aligned}
\right.\end{split}\]

In this example, we search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[\|v^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[x^\star \in \zer(G_1 + G_2).\]

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(G_1\) is modeled by
  [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone").
- \(G_2\) is modeled as an intersection of
  [`StronglyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.StronglyMonotone "autolyap.problemclass.StronglyMonotone") and
  [`LipschitzOperator`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.LipschitzOperator "autolyap.problemclass.LipschitzOperator").
- The full inclusion is built with
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.DouglasRachford").
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
import numpy as np

from autolyap import SolverOptions
from autolyap.algorithms import DouglasRachford
from autolyap.problemclass import (
    InclusionProblem,
    LipschitzOperator,
    MaximallyMonotone,
    StronglyMonotone,
)
from autolyap.iteration_independent import IterationIndependent

mu = 1.0
L = 2.0
gamma = 1.0
lambda_value = 2.0

problem = InclusionProblem(
    [
        MaximallyMonotone(),  # G1
        [StronglyMonotone(mu=mu), LipschitzOperator(L=L)],  # G2 in intersection
    ]
)
algorithm = DouglasRachford(gamma=gamma, lambda_value=lambda_value, type="operator")
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

# Build V(P, p, k) and R(T, t, k) for distance-to-solution analysis.
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

alpha = lambda_value / 2.0
delta = np.sqrt(1.0 - (4.0 * gamma * mu) / (1.0 + 2.0 * gamma * mu + (gamma * L) ** 2))
rho_theory = (abs(1.0 - alpha) + alpha * delta) ** 2

print(f"rho (AutoLyap): {rho_autolyap:.8f}")
print(f"rho (theory):   {rho_theory:.8f}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: certified contraction factor when feasible.
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

The computed value `rho (AutoLyap)` matches (up to solver numerical tolerances)
the theoretical rate expression in
<a id="id1"></a>[[Gis17](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_plus_strongly_monotone_lipschitz/#id84 "Pontus Giselsson. Tight global linear convergence rate bounds for Douglas–Rachford splitting. Journal of Fixed Point Theory and Applications, 19(4):2241–2270, 2017. doi:10.1007/s11784-017-0417-1."), Theorem 6.5], i.e.,


\[\|v^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[\rho = \left(|1-\alpha| + \alpha \delta\right)^2, \quad
\alpha = \frac{\lambda}{2}, \quad
\delta = \sqrt{1 - \frac{4\gamma\mu}{1 + 2\gamma\mu + (\gamma L)^2}},\]

and


\[x^\star \in \zer(G_1 + G_2).\]

Sweeping over 100 values of \(\gamma\) on \(0 < \gamma \le 5\) (with
\(\mu=1\), \(L=2\), \(\lambda=2\)) gives the plot below, with the
theoretical rate in black and AutoLyap certificates as blue dots.

[![Douglas-Rachford rho versus gamma with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/douglas_rachford_maximally_monotone_plus_strongly_monotone_lipschitz_rho_vs_gamma.svg)](https://autolyap.github.io/_images/douglas_rachford_maximally_monotone_plus_strongly_monotone_lipschitz_rho_vs_gamma.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id84"></a>



[[Gis17](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_plus_strongly_monotone_lipschitz/#id1)]

Pontus Giselsson. Tight global linear convergence rate bounds for Douglas–Rachford splitting. *Journal of Fixed Point Theory and Applications*, 19(4):2241–2270, 2017. [doi:10.1007/s11784-017-0417-1](https://doi.org/10.1007/s11784-017-0417-1).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\zer}{\operatorname*{zer}}
```
