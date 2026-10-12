Canonical HTML: https://autolyap.github.io/examples/optimized_gradient/
Documentation version: 0.2.1

<a id="the-optimized-gradient-method"></a>



# The optimized gradient method

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x),\]

where \(f : \calH \to \reals\) is convex and \(L\)-smooth with
\(L>0\).

For initial points \(x^0, y^0 \in \calH\) and iteration budget
\(K \in \mathbb{N}\), the optimized gradient method <a id="id1"></a>[[KF15](https://autolyap.github.io/examples/optimized_gradient/#id108 "Donghwan Kim and Jeffrey A. Fessler. Optimized first-order methods for smooth convex minimization. Mathematical Programming, 159(1-2):81–107, October 2015. doi:10.1007/s10107-015-0949-3.")] is given by


\[\begin{split}(\forall k \in \llbracket 0, K-1 \rrbracket)\quad
\left[
\begin{aligned}
    y^{k+1} &= x^k - \frac{1}{L}\nabla f(x^k), \\
    x^{k+1} &= y^{k+1}
    + \frac{\theta_k - 1}{\theta_{k+1}}(y^{k+1} - y^k)
    + \frac{\theta_k}{\theta_{k+1}}(y^{k+1} - x^k).
\end{aligned}
\right.\end{split}\]


\[\begin{split}\theta_k =
\begin{cases}
    1, & \text{if } k = 0, \\
    \dfrac{1 + \sqrt{1 + 4\theta_{k-1}^2}}{2},
    & \text{if } k \in \llbracket 1, K-1 \rrbracket, \\
    \dfrac{1 + \sqrt{1 + 8\theta_{k-1}^2}}{2},
    & \text{if } k = K.
\end{cases}\end{split}\]

In this example, we search for the smallest AutoLyap certificate constant
\(c_K\) such that


\[f(x^K)-f(x^\star)\le c_K\|x^0-x^\star\|^2,\]

where \(x^\star \in \Argmin_{x \in \calH} f(x)\).

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-c-k"></a>



## Model the problem in AutoLyap and search for the smallest c\_K

- \(f\) is modeled by
  [`SmoothConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothConvex "autolyap.problemclass.SmoothConvex").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.OptimizedGradientMethod").
- Initial-horizon parameters are obtained with
  [`IterationDependent.get_parameters_distance_to_solution`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_distance_to_solution "autolyap.IterationDependent.get_parameters_distance_to_solution").
- Final-horizon function-value parameters are obtained with
  [`IterationDependent.get_parameters_function_value_suboptimality`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_function_value_suboptimality "autolyap.IterationDependent.get_parameters_function_value_suboptimality").
- The certificate constant is computed with
  [`IterationDependent.search_lyapunov`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.search_lyapunov "autolyap.IterationDependent.search_lyapunov").

<a id="run-the-iteration-dependent-analysis"></a>



## Run the iteration-dependent analysis

This example uses the MOSEK Fusion backend (`backend="mosek_fusion"`).
Install the optional MOSEK dependency first:



```bash
pip install "autolyap[mosek]"
```





```python
from autolyap import IterationDependent, SolverOptions
from autolyap.algorithms import OptimizedGradientMethod
from autolyap.problemclass import InclusionProblem, SmoothConvex

L = 1.0
K = 5

problem = InclusionProblem([SmoothConvex(L)])
algorithm = OptimizedGradientMethod(L=L, K=K)
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

Q_0, q_0 = IterationDependent.get_parameters_distance_to_solution(
    algorithm,
    0,
    i=1,
    j=1,
)
Q_K, q_K = IterationDependent.get_parameters_function_value_suboptimality(
    algorithm,
    K,
)

result = IterationDependent.search_lyapunov(
    problem,
    algorithm,
    K,
    Q_0,
    Q_K,
    q_0=q_0,
    q_K=q_K,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible chained Lyapunov certificate for this setup.")

c_K = result["c_K"]
theta_K = algorithm.compute_theta(K, K)
c_K_theory = L / (2.0 * theta_K ** 2)

print(f"c_K (AutoLyap): {c_K:.6e}")
print(f"c_K (theory):   {c_K_theory:.6e}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["c_K"]`: certified finite-horizon constant when feasible.
- `result["certificate"]`: chained Lyapunov certificate matrices/vectors.

The computed value `c_K (AutoLyap)` matches (up to solver numerical
tolerances) the theoretical horizon-`K` expression:


\[f(x^K) - f(x^\star) \le c_K\,\|x^0 - x^\star\|^2, \qquad
c_K = \frac{L}{2\theta_K^2}.\]

In particular,


\[f(x^K) - f(x^\star) \in \mathcal{O}\!\left(\frac{1}{\theta_K^2}\right)
\quad \textup{ as } \quad K\to\infty,
\qquad
f(x^K) - f(x^\star) \in \mathcal{O}\!\left(\frac{1}{K^2}\right)
\quad \textup{ as } \quad K\to\infty.\]

Sweeping over \(K \in \llbracket 1, 100\rrbracket\) gives the log-log plot
below, with the theoretical bound in black and AutoLyap certificates as blue
dots.

[![Optimized-gradient c_K versus K in log-log scale with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/optimized_gradient_method_c_vs_K_loglog.svg)](https://autolyap.github.io/_images/optimized_gradient_method_c_vs_K_loglog.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id108"></a>



[[KF15](https://autolyap.github.io/examples/optimized_gradient/#id1)]

Donghwan Kim and Jeffrey A. Fessler. Optimized first-order methods for smooth convex minimization. *Mathematical Programming*, 159(1-2):81–107, October 2015. [doi:10.1007/s10107-015-0949-3](https://doi.org/10.1007/s10107-015-0949-3).

TeX macro definitions used in this page:

```latex
\newcommand{\Argmin}{\operatorname*{Argmin}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
