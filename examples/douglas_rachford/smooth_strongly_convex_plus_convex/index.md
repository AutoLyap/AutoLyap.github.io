Canonical HTML: https://autolyap.github.io/examples/douglas_rachford/smooth_strongly_convex_plus_convex/
Documentation version: 0.2.1

<a id="the-douglas-rachford-method-smooth-and-strongly-convex-convex"></a>



# The Douglas–Rachford method: Smooth and strongly convex + convex

<a id="problem-setup"></a>



## Problem setup

Consider the composite minimization problem


\[\minimize_{x \in \calH}\; f_1(x)+f_2(x),\]

where:

- \(f_1 : \calH \to \reals\) is \(\mu\)-strongly convex and
  \(L\)-smooth, with \(0 < \mu < L\),
- \(f_2 : \calH \to \reals \cup \set{\pm\infty}\) is proper, convex, and
  lower semicontinuous.

Equivalently, we solve


\[\text{find } x \in \calH \text{ such that } 0 \in \partial f_1(x) + \partial f_2(x).\]

For an initial point \(x^0 \in \calH\), step size
\(\gamma \in \reals_{++}\), and relaxation
\(\lambda \in \reals\), the Douglas–Rachford update is


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
v^k &= \prox_{\gamma f_1}(x^k), \\
w^k &= \prox_{\gamma f_2}(2v^k - x^k), \\
x^{k+1} &= x^k + \lambda (w^k - v^k).
\end{aligned}
\right.\end{split}\]

In this example, we fix \(\mu=1\), \(L=2\), \(\lambda=1\), sweep
\(\gamma \in (0,5]\), and search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[\|v^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[x^\star \in \zer\bigl(\partial f_1 + \partial f_2\bigr).\]

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(f_1\) is modeled by
  [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex").
- \(f_2\) is modeled by
  [`Convex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Convex "autolyap.problemclass.Convex").
- The full inclusion is built with
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.DouglasRachford") with
  `type="function"`.
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

from autolyap import IterationIndependent, SolverOptions
from autolyap.algorithms import DouglasRachford
from autolyap.problemclass import Convex, InclusionProblem, SmoothStronglyConvex

mu = 1.0
L = 2.0
gamma = 1.0
lambda_value = 1.0

problem = InclusionProblem(
    [
        SmoothStronglyConvex(mu=mu, L=L),  # f1
        Convex(),  # f2
    ]
)
algorithm = DouglasRachford(
    gamma=gamma,
    lambda_value=lambda_value,
    type="function",
)
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

P, p, T, t = IterationIndependent.LinearConvergence.get_parameters_distance_to_solution(
    algorithm
)

result = IterationIndependent.LinearConvergence.bisection_search_rho(
    problem,
    algorithm,
    P,
    T,
    p=p,
    t=t,
    S_equals_T=True,
    s_equals_t=True,
    remove_C3=True,
    tol=1e-3,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate in the requested rho interval.")

rho_autolyap = result["rho"]

alpha = lambda_value / 2.0
delta = max(
    (gamma * L - 1.0) / (gamma * L + 1.0),
    (1.0 - gamma * mu) / (1.0 + gamma * mu),
)
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
the rate expression in
<a id="id1"></a>[[GB17](https://autolyap.github.io/examples/douglas_rachford/smooth_strongly_convex_plus_convex/#id83 "Pontus Giselsson and Stephen Boyd. Linear convergence and metric selection for Douglas–Rachford splitting and admm. IEEE Transactions on Automatic Control, 62(2):532–544, February 2017. doi:10.1109/tac.2016.2564160."), Theorem 2] for this setting, i.e.,


\[\rho = \left(|1-\alpha| + \alpha \delta\right)^2, \quad
\alpha = \frac{\lambda}{2}, \quad
\delta = \max\left\{
\frac{\gamma L - 1}{\gamma L + 1},
\frac{1-\gamma\mu}{1+\gamma\mu}
\right\}.\]

Sweeping over 100 values of \(\gamma\) on \(0 < \gamma \le 5\) (with
\(\mu=1\), \(L=2\), \(\lambda=1\)) gives the plot below, with the
theoretical rate in black and AutoLyap certificates as blue dots.

[![Douglas-Rachford rho versus gamma for smooth strongly-convex plus convex splitting, with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/douglas_rachford_smooth_strongly_convex_plus_convex_rho_vs_gamma.svg)](https://autolyap.github.io/_images/douglas_rachford_smooth_strongly_convex_plus_convex_rho_vs_gamma.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id83"></a>



[[GB17](https://autolyap.github.io/examples/douglas_rachford/smooth_strongly_convex_plus_convex/#id1)]

Pontus Giselsson and Stephen Boyd. Linear convergence and metric selection for Douglas–Rachford splitting and admm. *IEEE Transactions on Automatic Control*, 62(2):532–544, February 2017. [doi:10.1109/tac.2016.2564160](https://doi.org/10.1109/tac.2016.2564160).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\prox}{{\rm{prox}}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
\newcommand{\zer}{\operatorname*{zer}}
```
