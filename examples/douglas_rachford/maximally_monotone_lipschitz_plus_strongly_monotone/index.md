Canonical HTML: https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_lipschitz_plus_strongly_monotone/
Documentation version: 0.2.1

<a id="the-douglas-rachford-method-maximally-monotone-lipschitz-strongly-monotone"></a>



# The Douglas–Rachford method: Maximally monotone/Lipschitz + strongly monotone

<a id="problem-setup"></a>



## Problem setup

Consider the monotone inclusion


\[\text{find } x \in \calH \text{ such that } 0 \in G_1(x) + G_2(x),\]

where:

- \(G_1 : \calH \to \calH\) is monotone and
  \(L\)-Lipschitz continuous, with \(L>0\),
- \(G_2 : \calH \rightrightarrows \calH\) is \(\mu\)-strongly monotone
  and maximally monotone, with \(\mu>0\).

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

- \(G_1\) is modeled as an intersection of
  [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone") and
  [`LipschitzOperator`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.LipschitzOperator "autolyap.problemclass.LipschitzOperator").
- \(G_2\) is modeled by
  [`StronglyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.StronglyMonotone "autolyap.problemclass.StronglyMonotone").
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
L = 1.0
gamma = 1.0
lambda_value = 1.2

problem = InclusionProblem(
    [
        [MaximallyMonotone(), LipschitzOperator(L=L)],  # G1
        StronglyMonotone(mu=mu),  # G2
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

def rho_theoretical(mu: float, L: float, lambda_value: float, gamma: float) -> float:
    scaled_mu = gamma * mu
    scaled_L = gamma * L

    term1 = 2.0 * (lambda_value - 1.0) * scaled_mu + lambda_value - 2.0
    term2 = scaled_L ** 2 * (lambda_value - 2.0 * (scaled_mu + 1.0))
    denominator = np.sqrt(term1 ** 2 + scaled_L ** 2 * (lambda_value - 2.0 * (scaled_mu + 1.0)) ** 2)
    condition_a_lhs = scaled_mu * (-term1 + term2) / denominator

    if condition_a_lhs <= np.sqrt(scaled_L ** 2 + 1.0):
        numerator_a = lambda_value + np.sqrt(
            (term1 ** 2 + scaled_L ** 2 * (lambda_value - 2.0 * (scaled_mu + 1.0)) ** 2)
            / (scaled_L ** 2 + 1.0)
        )
        return (numerator_a / (2.0 * (scaled_mu + 1.0))) ** 2

    threshold_b = (
        2.0 * (scaled_mu + 1.0) * (scaled_L + 1.0)
        * (scaled_mu + scaled_mu * scaled_L ** 2 - scaled_L ** 2 - 2.0 * scaled_mu * scaled_L - 1.0)
    ) / (
        2.0 * scaled_mu ** 2 - scaled_mu + scaled_mu * scaled_L ** 3 - scaled_L ** 3
        - 3.0 * scaled_mu * scaled_L ** 2 - scaled_L ** 2
        - 2.0 * scaled_mu ** 2 * scaled_L - scaled_mu * scaled_L - scaled_L - 1.0
    )
    if (
        scaled_L < 1.0
        and scaled_mu > (scaled_L ** 2 + 1.0) / ((scaled_L - 1.0) ** 2)
        and lambda_value <= threshold_b
    ):
        return abs(1.0 - lambda_value * (scaled_L + scaled_mu) / ((scaled_mu + 1.0) * (scaled_L + 1.0))) ** 2

    term3 = lambda_value * (scaled_L ** 2 + 1.0) - 2.0 * scaled_mu * (lambda_value + scaled_L ** 2 - 1.0)
    term4 = lambda_value * (1.0 + 2.0 * scaled_mu + scaled_L ** 2) - 2.0 * (scaled_mu + 1.0) * (scaled_L ** 2 + 1.0)
    numerator_c = (2.0 - lambda_value) * term3 * term4
    denominator_c = 4.0 * scaled_mu * (scaled_L ** 2 + 1.0) * (
        2.0 * scaled_mu * (lambda_value + scaled_L ** 2 - 1.0)
        - (2.0 - lambda_value) * (1.0 - scaled_L ** 2)
    )
    return numerator_c / denominator_c

rho_theory = rho_theoretical(mu, L, lambda_value, gamma)

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
<a id="id1"></a>[[RTBG20](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_lipschitz_plus_strongly_monotone/#id176 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Theorem 4.3], i.e.,


\[\|v^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[\begin{split}\rho =
\begin{cases}
\left(\dfrac{\lambda +
\sqrt{\dfrac{\left(2(\lambda-1)\tilde{\mu}+\lambda-2\right)^2
+\tilde{L}^2\left(\lambda-2(\tilde{\mu}+1)\right)^2}{\tilde{L}^2+1}}}
{2(\tilde{\mu}+1)}\right)^2,
& \text{if } (a),
\\
\left|1-\lambda\dfrac{\tilde{L}+\tilde{\mu}}{(\tilde{\mu}+1)(\tilde{L}+1)}\right|^2,
& \text{if } (b),
\\
\rho_{3},
& \text{otherwise},
\end{cases}\end{split}\]

with


\[\begin{split}\begin{aligned}
(a)\quad &
\dfrac{\tilde{\mu}\left(-2(\lambda-1)\tilde{\mu}-\lambda+2
+\tilde{L}^2\left(\lambda-2(\tilde{\mu}+1)\right)\right)}
{\sqrt{\left(2(\lambda-1)\tilde{\mu}+\lambda-2\right)^2
+\tilde{L}^2\left(\lambda-2(\tilde{\mu}+1)\right)^2}}
\le \sqrt{\tilde{L}^2+1},
\\
(b)\quad &
\tilde{L}<1
\text{ and } \tilde{\mu}>\dfrac{\tilde{L}^2+1}{(\tilde{L}-1)^2}
\text{ and } \lambda \le
\dfrac{2(\tilde{\mu}+1)(\tilde{L}+1)
\left(\tilde{\mu}+\tilde{\mu}\tilde{L}^2-\tilde{L}^2-2\tilde{\mu}\tilde{L}-1\right)}
{2\tilde{\mu}^2-\tilde{\mu}+\tilde{\mu}\tilde{L}^3-\tilde{L}^3-3\tilde{\mu}\tilde{L}^2-\tilde{L}^2-2\tilde{\mu}^2\tilde{L}-\tilde{\mu}\tilde{L}-\tilde{L}-1},
\end{aligned}\end{split}\]

and


\[\begin{split}\begin{aligned}
\rho_{3} &= \dfrac{(2-\lambda)
\left(\lambda(\tilde{L}^2+1)-2\tilde{\mu}(\lambda+\tilde{L}^2-1)\right)
\left(\lambda(1+2\tilde{\mu}+\tilde{L}^2)-2(\tilde{\mu}+1)(\tilde{L}^2+1)\right)}
{4\tilde{\mu}(\tilde{L}^2+1)
\left(2\tilde{\mu}(\lambda+\tilde{L}^2-1)-(2-\lambda)(1-\tilde{L}^2)\right)},\\
\tilde{\mu} &= \gamma\mu,\\
\tilde{L} &= \gamma L,
\end{aligned}\end{split}\]

and


\[x^\star \in \zer(G_1 + G_2).\]

Sweeping over 100 values of \(\lambda\) on \(0 < \lambda < 2\) (with
\(\mu=1\), \(L=1\), \(\gamma=1\)) gives the plot below, with the
theoretical rate from <a id="id2"></a>[[RTBG20](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_lipschitz_plus_strongly_monotone/#id176 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Theorem 4.3]
in black and AutoLyap certificates as blue dots.

[![Douglas-Rachford rho versus lambda for maximally-monotone-Lipschitz plus strongly-monotone setting with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/douglas_rachford_maximally_monotone_lipschitz_plus_strongly_monotone_rho_vs_lambda.svg)](https://autolyap.github.io/_images/douglas_rachford_maximally_monotone_lipschitz_plus_strongly_monotone_rho_vs_lambda.svg)

<a id="references"></a>



## References

<a id="id3"></a>



<a id="id176"></a>



[RTBG20]
([1](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_lipschitz_plus_strongly_monotone/#id1),[2](https://autolyap.github.io/examples/douglas_rachford/maximally_monotone_lipschitz_plus_strongly_monotone/#id2))

Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. *SIAM Journal on Optimization*, 30(3):2251–2271, January 2020. [doi:10.1137/19m1304854](https://doi.org/10.1137/19m1304854).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\zer}{\operatorname*{zer}}
```
