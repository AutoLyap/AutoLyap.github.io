Canonical HTML: https://autolyap.github.io/examples/douglas_rachford/cocoercive_plus_strongly_monotone/
Documentation version: 0.2.1

<a id="the-douglas-rachford-method-cocoercive-strongly-monotone"></a>



# The Douglas–Rachford method: Cocoercive + strongly monotone

<a id="problem-setup"></a>



## Problem setup

Consider the monotone inclusion


\[\text{find } x \in \calH \text{ such that } 0 \in G_1(x) + G_2(x),\]

where:

- \(G_1 : \calH \to \calH\) is \(\beta\)-cocoercive, with \(\beta>0\),
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

- \(G_1\) is modeled by
  [`Cocoercive`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.Cocoercive "autolyap.problemclass.Cocoercive").
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
    Cocoercive,
    InclusionProblem,
    StronglyMonotone,
)
from autolyap.iteration_independent import IterationIndependent

mu = 1.0
beta = 1.0
gamma = 1.0
lambda_value = 1.2

problem = InclusionProblem(
    [
        Cocoercive(beta=beta),  # G1
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

def rho_theoretical(mu: float, beta: float, lambda_value: float, gamma: float) -> float:
    scaled_mu = gamma * mu
    scaled_beta = beta / gamma

    if (
        (scaled_mu * scaled_beta - scaled_mu + scaled_beta < 0.0)
        and (
            lambda_value
            <= 2.0
            * ((scaled_beta + 1.0) * (scaled_mu - scaled_beta - scaled_mu * scaled_beta))
            / (
                scaled_mu
                + scaled_mu * scaled_beta
                - scaled_beta
                - scaled_beta ** 2
                - 2.0 * scaled_mu * scaled_beta ** 2
            )
        )
    ):
        return abs(1.0 - lambda_value * (scaled_beta / (scaled_beta + 1.0))) ** 2

    if (
        (scaled_mu * scaled_beta - scaled_mu - scaled_beta > 0.0)
        and (
            lambda_value
            <= 2.0
            * (
                scaled_mu ** 2
                + scaled_beta ** 2
                + scaled_mu * scaled_beta
                + scaled_mu
                + scaled_beta
                - scaled_mu ** 2 * scaled_beta ** 2
            )
            / (
                scaled_mu ** 2
                + scaled_beta ** 2
                + scaled_mu ** 2 * scaled_beta
                + scaled_mu * scaled_beta ** 2
                + scaled_mu
                + scaled_beta
                - 2.0 * scaled_mu ** 2 * scaled_beta ** 2
            )
        )
    ):
        return abs(
            1.0
            - lambda_value
            * (
                (1.0 + scaled_mu * scaled_beta)
                / ((scaled_mu + 1.0) * (scaled_beta + 1.0))
            )
        ) ** 2

    if lambda_value >= 2.0 * (scaled_mu * scaled_beta + scaled_mu + scaled_beta) / (
        2.0 * scaled_mu * scaled_beta + scaled_mu + scaled_beta
    ):
        return abs(1.0 - lambda_value) ** 2

    if (
        (scaled_mu * scaled_beta + scaled_mu - scaled_beta < 0.0)
        and (
            lambda_value
            <= 2.0
            * ((scaled_mu + 1.0) * (scaled_beta - scaled_mu - scaled_mu * scaled_beta))
            / (
                scaled_beta
                + scaled_mu * scaled_beta
                - scaled_mu
                - scaled_mu ** 2
                - 2.0 * scaled_mu ** 2 * scaled_beta
            )
        )
    ):
        return abs(1.0 - lambda_value * (scaled_mu / (scaled_mu + 1.0))) ** 2

    numerator = (
        (
            (2.0 - lambda_value) * scaled_mu * (scaled_beta + 1.0)
            + lambda_value * scaled_beta * (1.0 - scaled_mu)
        )
        * (
            (2.0 - lambda_value) * scaled_beta * (scaled_mu + 1.0)
            + lambda_value * scaled_mu * (1.0 - scaled_beta)
        )
    )
    denominator = scaled_mu * scaled_beta * (
        2.0 * scaled_mu * scaled_beta * (1.0 - lambda_value)
        + (2.0 - lambda_value) * (scaled_mu + scaled_beta + 1.0)
    )
    return ((np.sqrt(2.0 - lambda_value) / 2.0) * np.sqrt(numerator / denominator)) ** 2


rho_theory = rho_theoretical(mu, beta, lambda_value, gamma)

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
<a id="id1"></a>[[RTBG20](https://autolyap.github.io/examples/douglas_rachford/cocoercive_plus_strongly_monotone/#id176 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Theorem 4.1], i.e.,


\[\|v^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[\begin{split}\rho =
\begin{cases}
\left|1-\lambda\dfrac{\tilde{\beta}}{\tilde{\beta}+1}\right|^2,
& \text{if } \tilde{\mu}\tilde{\beta}-\tilde{\mu}+\tilde{\beta}<0
\text{ and } \lambda \le
2\dfrac{(\tilde{\beta}+1)(\tilde{\mu}-\tilde{\beta}-\tilde{\mu}\tilde{\beta})}
{\tilde{\mu}+\tilde{\mu}\tilde{\beta}-\tilde{\beta}-\tilde{\beta}^2-2\tilde{\mu}\tilde{\beta}^2},
\\
\left|1-\lambda\dfrac{1+\tilde{\mu}\tilde{\beta}}{(\tilde{\mu}+1)(\tilde{\beta}+1)}\right|^2,
& \text{if } \tilde{\mu}\tilde{\beta}-\tilde{\mu}-\tilde{\beta}>0
\text{ and } \lambda \le
2\dfrac{\tilde{\mu}^2+\tilde{\beta}^2+\tilde{\mu}\tilde{\beta}+\tilde{\mu}+\tilde{\beta}-\tilde{\mu}^2\tilde{\beta}^2}
{\tilde{\mu}^2+\tilde{\beta}^2+\tilde{\mu}^2\tilde{\beta}+\tilde{\mu}\tilde{\beta}^2+\tilde{\mu}+\tilde{\beta}-2\tilde{\mu}^2\tilde{\beta}^2},
\\
|1-\lambda|^2,
&
\text{if } \lambda \ge
2\dfrac{\tilde{\mu}\tilde{\beta}+\tilde{\mu}+\tilde{\beta}}
{2\tilde{\mu}\tilde{\beta}+\tilde{\mu}+\tilde{\beta}},
\\
\left|1-\lambda\dfrac{\tilde{\mu}}{\tilde{\mu}+1}\right|^2,
& \text{if } \tilde{\mu}\tilde{\beta}+\tilde{\mu}-\tilde{\beta}<0
\text{ and } \lambda \le
2\dfrac{(\tilde{\mu}+1)(\tilde{\beta}-\tilde{\mu}-\tilde{\mu}\tilde{\beta})}
{\tilde{\beta}+\tilde{\mu}\tilde{\beta}-\tilde{\mu}-\tilde{\mu}^2-2\tilde{\mu}^2\tilde{\beta}},
\\
\rho_{5},
& \text{otherwise},
\end{cases}\end{split}\]

with


\[\begin{split}\begin{aligned}
\rho_{5} &= \dfrac{2-\lambda}{4}
\dfrac{
\big((2-\lambda)\tilde{\mu}(\tilde{\beta}+1)+\lambda\tilde{\beta}(1-\tilde{\mu})\big)
\big((2-\lambda)\tilde{\beta}(\tilde{\mu}+1)+\lambda\tilde{\mu}(1-\tilde{\beta})\big)
}{
\tilde{\mu}\tilde{\beta}\big(2\tilde{\mu}\tilde{\beta}(1-\lambda)+(2-\lambda)(\tilde{\mu}+\tilde{\beta}+1)\big)
},\\
\tilde{\mu} &= \gamma\mu,\\
\tilde{\beta} &= \dfrac{\beta}{\gamma},
\end{aligned}\end{split}\]

and


\[x^\star \in \zer(G_1 + G_2).\]

Sweeping over 100 values of \(\lambda\) on \(0 < \lambda < 2\) (with
\(\mu=1\), \(\beta=1\), \(\gamma=1\)) gives the plot below, with the
theoretical rate from <a id="id2"></a>[[RTBG20](https://autolyap.github.io/examples/douglas_rachford/cocoercive_plus_strongly_monotone/#id176 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Theorem 4.1]
in black and AutoLyap certificates as blue dots.

[![Douglas-Rachford rho versus lambda for cocoercive-plus-strongly-monotone setting with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/douglas_rachford_cocoercive_plus_strongly_monotone_rho_vs_lambda.svg)](https://autolyap.github.io/_images/douglas_rachford_cocoercive_plus_strongly_monotone_rho_vs_lambda.svg)

<a id="references"></a>



## References

<a id="id3"></a>



<a id="id176"></a>



[RTBG20]
([1](https://autolyap.github.io/examples/douglas_rachford/cocoercive_plus_strongly_monotone/#id1),[2](https://autolyap.github.io/examples/douglas_rachford/cocoercive_plus_strongly_monotone/#id2))

Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. *SIAM Journal on Optimization*, 30(3):2251–2271, January 2020. [doi:10.1137/19m1304854](https://doi.org/10.1137/19m1304854).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\zer}{\operatorname*{zer}}
```
