Canonical HTML: https://autolyap.github.io/examples/heavy_ball/gradient_dominated_smooth/
Documentation version: 0.2.1

<a id="the-heavy-ball-method-gradient-dominated-and-smooth"></a>



# The heavy-ball method: Gradient-dominated and smooth

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x) \quad \iff \quad \text{find } x \in \calH \;\text{ such that }\; 0 \in \partial f(x) = \set{\nabla f(x)},\]

where \(f : \calH \to \reals\) is \(\mu_{\textup{gd}}\)-gradient
dominated and \(L\)-smooth with \(0<\mu_{\textup{gd}}\le L\).

For initial points \(x^{-1}, x^0 \in \calH\), momentum
\(\delta \in \reals\), and step size \(\gamma \in \reals_{++}\),
the heavy-ball update <a id="id1"></a>[[Pol64](https://autolyap.github.io/examples/heavy_ball/gradient_dominated_smooth/#id163 "B. T. Polyak. Some methods of speeding up the convergence of iteration methods. USSR Computational Mathematics and Mathematical Physics, 4(5):1–17, 1964. doi:10.1016/0041-5553(64)90137-5.")] is


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - \gamma \nabla f(x^k) + \delta(x^k - x^{k-1}).\]

In this example, we search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[f(x^k) - f(x^\star) \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where


\[x^\star \in \Argmin_{x \in \calH} f(x).\]

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(f\) is modeled as an intersection of
  [`GradientDominated`](https://autolyap.github.io/function_classes/#autolyap.problemclass.GradientDominated "autolyap.problemclass.GradientDominated") and
  [`Smooth`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Smooth "autolyap.problemclass.Smooth").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.HeavyBallMethod").
- Function-value parameters are obtained with
  [`IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality "autolyap.IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality").
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
from autolyap import SolverOptions
from autolyap.algorithms import HeavyBallMethod
from autolyap.problemclass import GradientDominated, InclusionProblem, Smooth
from autolyap.iteration_independent import IterationIndependent

mu_gd = 0.5
L = 1.0
gamma = 1.0
delta = 0.5

problem = InclusionProblem(
    [
        [GradientDominated(mu_gd=mu_gd), Smooth(L=L)],  # f in intersection
    ]
)
algorithm = HeavyBallMethod(gamma=gamma, delta=delta)
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

# Build V(P, p, k) and R(T, t, k) for function-value suboptimality analysis.
P, p, T, t = IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality(
    algorithm,
    tau=0,
)

result = IterationIndependent.LinearConvergence.bisection_search_rho(
    problem,
    algorithm,
    P,
    T,
    p=p,
    t=t,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate in the requested rho interval.")

print(f"rho (AutoLyap): {result['rho']:.8f}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: certified contraction factor when feasible.
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

Sweeping over 100 values of \(\gamma\) on \((0,2.30]\) and 100 values
of \(\delta\) on \([-1,1]\) with \(\mu_{\textup{gd}}=0.5\) and
\(L=1\) gives the plot below. Each dot is a feasible parameter pair, and
the color encodes the certified contraction factor \(\rho\).

[![Heavy-ball feasible (gamma, delta) pairs for gradient-dominated smooth objectives, with color showing rho.](https://autolyap.github.io/_images/heavy_ball_gradient_dominated_smooth.svg)](https://autolyap.github.io/_images/heavy_ball_gradient_dominated_smooth.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id163"></a>



[[Pol64](https://autolyap.github.io/examples/heavy_ball/gradient_dominated_smooth/#id1)]

B. T. Polyak. Some methods of speeding up the convergence of iteration methods. *USSR Computational Mathematics and Mathematical Physics*, 4(5):1–17, 1964. [doi:10.1016/0041-5553(64)90137-5](https://doi.org/10.1016/0041-5553(64)90137-5).

TeX macro definitions used in this page:

```latex
\newcommand{\Argmin}{\operatorname*{Argmin}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\set}[1]{\mathord{\left.\{ #1 \} \right. }}
```
