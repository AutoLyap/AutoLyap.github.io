Canonical HTML: https://autolyap.github.io/examples/gradient_method/smooth_strongly_convex/
Documentation version: 0.2.1

<a id="the-gradient-method-smooth-and-strongly-convex"></a>



# The gradient method: Smooth and strongly convex

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x),\]

where \(f : \calH \to \reals\) is \(\mu\)-strongly convex and
\(L\)-smooth, with \(0 < \mu < L\).

For an initial point \(x^0 \in \calH\) and step size
\(0 < \gamma < 2/L\), the gradient update is


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - \gamma \nabla f(x^k).\]

In this example, we search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[\|x^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[x^\star \in \Argmin_{x \in \calH} f(x).\]

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(f\) is modeled by
  [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`GradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientMethod "autolyap.algorithms.GradientMethod").
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
from autolyap.algorithms import GradientMethod
from autolyap.problemclass import InclusionProblem, SmoothStronglyConvex

mu = 1.0
L = 4.0
gamma = 0.2

problem = InclusionProblem([SmoothStronglyConvex(mu, L)])
algorithm = GradientMethod(gamma=gamma)
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
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate in the requested rho interval.")

rho_autolyap = result["rho"]
rho_theory = max(gamma * L - 1.0, 1.0 - gamma * mu) ** 2

print(f"rho (AutoLyap): {rho_autolyap:.8f}")
print(f"rho (theory):   {rho_theory:.8f}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: certified contraction factor when feasible.
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

The computed value `rho (AutoLyap)` matches (up to solver numerical
tolerances) the theoretical rate expression for gradient methods
<a id="id1"></a>[[Pol63](https://autolyap.github.io/examples/gradient_method/smooth_strongly_convex/#id162 "B. T. Polyak. Gradient methods for the minimisation of functionals. USSR Computational Mathematics and Mathematical Physics, 3(4):864–878, 1963. doi:10.1016/0041-5553(63)90382-3.")]:


\[\|x^k - x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty, \qquad
\rho = \max\{|1-\gamma L|,\;|1-\gamma\mu|\}^2.\]

Equivalently,


\[\|x^k - x^\star\| \in \mathcal{O}\!\left(\max\{|1-\gamma L|,\;|1-\gamma\mu|\}^k\right)
\quad \textup{ as } \quad k\to\infty.\]

Sweeping over 100 values of \(\gamma\) up to \(2/L\) gives the plot
below, with the endpoint included as the limiting \(\rho=1\) reference,
the theoretical rate in black, and AutoLyap certificates as blue dots.

[![Gradient-method rho versus gamma with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/gradient_method_rho_vs_gamma.svg)](https://autolyap.github.io/_images/gradient_method_rho_vs_gamma.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id162"></a>



[[Pol63](https://autolyap.github.io/examples/gradient_method/smooth_strongly_convex/#id1)]

B. T. Polyak. Gradient methods for the minimisation of functionals. *USSR Computational Mathematics and Mathematical Physics*, 3(4):864–878, 1963. [doi:10.1016/0041-5553(63)90382-3](https://doi.org/10.1016/0041-5553(63)90382-3).

TeX macro definitions used in this page:

```latex
\newcommand{\Argmin}{\operatorname*{Argmin}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
```
