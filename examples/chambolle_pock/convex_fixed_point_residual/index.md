Canonical HTML: https://autolyap.github.io/examples/chambolle_pock/convex_fixed_point_residual/
Documentation version: 0.2.1

<a id="the-chambolle-pock-method-convex"></a>



# The Chambolle–Pock method: Convex

<a id="problem-setup"></a>



## Problem setup

Suppose \(f_1,f_2:\calH \to \reals\cup\set{\pm\infty}\) are proper, convex, and lower semicontinuous.
In the identity-operator case, the Chambolle–Pock method
<a id="id1"></a>[[CP11](https://autolyap.github.io/examples/chambolle_pock/convex_fixed_point_residual/#id44 "Antonin Chambolle and Thomas Pock. A first-order primal-dual algorithm for convex problems with applications to imaging. Journal of Mathematical Imaging and Vision, 40(1):120–145, 2011. doi:10.1007/s10851-010-0251-1.")] solves


\[\minimize_{x\in\calH}\; f_1(x)+f_2(x)\]

by solving the inclusion


\[\text{find } x\in\calH\;\text{such that}\; 0\in\partial f_1(x)+\partial f_2(x),\]

with iterations


\[\begin{split}(\forall k\in\naturals)\quad
\left[
\begin{aligned}
x^{k+1} &= \prox_{\tau f_1}(x^k-\tau y^k), \\
y^{k+1} &= \prox_{\sigma f_2^*}\!\left(y^k + \sigma\left(x^{k+1}+\theta(x^{k+1}-x^k)\right)\right),
\end{aligned}
\right.\end{split}\]

where \(\tau,\sigma\in\reals_{++}\) are primal and dual step sizes, respectively, and
\(\theta\in\reals\) is a relaxation parameter.

For this example, the performance measure is the squared fixed-point residual
\(\|\bx^{k+1}-\bx^k\|^2\) with \(\bx^k=(x^k,y^k)\).
If it is zero, then \(x^k\) solves the inclusion above.

<a id="model-the-problem-in-autolyap-and-certify-summability"></a>



## Model the problem in AutoLyap and certify summability

- \(f_1,f_2\) are modeled by
  [`Convex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Convex "autolyap.problemclass.Convex").
- The problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.ChambollePock").
- Fixed-point-residual parameters are obtained with
  [`IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual "autolyap.IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual").
- Feasibility is checked with
  [`IterationIndependent.search_lyapunov`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov")
  at \(\rho=1\), using the history parameter \(h\) and overlap
  parameter \(\alpha\).

<a id="run-the-iteration-independent-analysis"></a>



## Run the iteration-independent analysis

This example uses the MOSEK Fusion backend (`backend="mosek_fusion"`).
Install the optional MOSEK dependency first:



```bash
pip install "autolyap[mosek]"
```





```python
from autolyap import IterationIndependent, SolverOptions
from autolyap.algorithms import ChambollePock
from autolyap.problemclass import Convex, InclusionProblem

tau = 1.0
sigma = 1.0
theta = 1.0
h = 1
alpha = 1

problem = InclusionProblem([
    Convex(),  # f_1
    Convex(),  # f_2
])
algorithm = ChambollePock(tau=tau, sigma=sigma, theta=theta)

MOSEK_PARAMS = {
    "intpntCoTolPfeas": 1e-6,
    "intpntCoTolDfeas": 1e-6,
    "intpntCoTolRelGap": 1e-6,
    "intpntMaxIterations": 10000,
}
solver_options = SolverOptions(
    backend="mosek_fusion",
    mosek_params=MOSEK_PARAMS,
)
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

P, p, T, t = IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual(
    algorithm,
    h=h,
    alpha=alpha,
)

result = IterationIndependent.search_lyapunov(
    problem,
    algorithm,
    P,
    T,
    p=p,
    t=t,
    rho=1.0,
    h=h,
    alpha=alpha,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate for the selected parameters.")

print("Feasible certificate found.")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: fixed contraction parameter (`1.0` in this setup).
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

In this setup, feasibility certifies that


\[\bigl(\|\bx^{k+1}-\bx^k\|^2\bigr)_{k\in\naturals}\]

is summable, and therefore,


\[\|\bx^{k+1}-\bx^k\|^2 \to 0 \quad \textup{ as } \quad k\to\infty.\]

Sweeping over multiple values of \(\tau=\sigma\in(1,2)\) and
\(\theta\in(0,3/2)\) for several \((h,\alpha)\) settings gives the
layered region plot below.

[![Feasible regions for Chambolle--Pock fixed-point residual summability in the (tau=sigma, theta) plane for several (h, alpha) settings.](https://autolyap.github.io/_images/chambolle_pock_fixed_point_residual_layers.svg)](https://autolyap.github.io/_images/chambolle_pock_fixed_point_residual_layers.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id44"></a>



[[CP11](https://autolyap.github.io/examples/chambolle_pock/convex_fixed_point_residual/#id1)]

Antonin Chambolle and Thomas Pock. A first-order primal-dual algorithm for convex problems with applications to imaging. *Journal of Mathematical Imaging and Vision*, 40(1):120–145, 2011. [doi:10.1007/s10851-010-0251-1](https://doi.org/10.1007/s10851-010-0251-1).

TeX macro definitions used in this page:

```latex
\newcommand{\bx}{\mathbf{x}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\prox}{{\rm{prox}}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
```
