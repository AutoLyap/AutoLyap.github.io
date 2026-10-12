Canonical HTML: https://autolyap.github.io/examples/heavy_ball/smooth_convex/
Documentation version: 0.2.1

<a id="the-heavy-ball-method-smooth-and-convex"></a>



# The heavy-ball method: Smooth and convex

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x) \quad \iff \quad \text{find } x \in \calH \;\text{ such that }\; 0 \in \partial f(x) = \set{\nabla f(x)},\]

where \(f : \calH \to \reals\) is convex and \(L\)-smooth with
\(L>0\).

For initial points \(x^{-1}, x^0 \in \calH\), momentum
\(\delta \in \reals\), and step size \(\gamma \in \reals_{++}\),
the heavy-ball update <a id="id1"></a>[[Pol64](https://autolyap.github.io/examples/heavy_ball/smooth_convex/#id163 "B. T. Polyak. Some methods of speeding up the convergence of iteration methods. USSR Computational Mathematics and Mathematical Physics, 4(5):1–17, 1964. doi:10.1016/0041-5553(64)90137-5.")] is


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - \gamma \nabla f(x^k) + \delta(x^k - x^{k-1}).\]

In this example, we search for a feasible Lyapunov certificate with
\(\rho=1\) and condition (C4) enabled such that


\[f(x^k) - f(x^\star) \in o\!\left(\frac{1}{k}\right) \quad \textup{ as } \quad k\to\infty,\]

where \(x^\star \in \Argmin_{x \in \calH} f(x)\).

<a id="model-the-problem-in-autolyap-and-certify-sublinear-convergence"></a>



## Model the problem in AutoLyap and certify sublinear convergence

- \(f\) is modeled by
  [`SmoothConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothConvex "autolyap.problemclass.SmoothConvex").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.HeavyBallMethod").
- Sublinear function-value parameters are obtained with
  [`IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality "autolyap.IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality").
- Feasibility at fixed \(\rho=1\) is checked with
  [`IterationIndependent.search_lyapunov`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov") and `remove_C4=False`.

<a id="run-the-iteration-independent-analysis"></a>



## Run the iteration-independent analysis

This example uses the MOSEK Fusion backend (`backend="mosek_fusion"`).
Install the optional MOSEK dependency first:



```bash
pip install "autolyap[mosek]"
```





```python
from autolyap import IterationIndependent, SolverOptions
from autolyap.algorithms import HeavyBallMethod
from autolyap.problemclass import InclusionProblem, SmoothConvex

L = 1.0
gamma = 1.0
delta = 0.5

problem = InclusionProblem([SmoothConvex(L)])
algorithm = HeavyBallMethod(gamma=gamma, delta=delta)

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

# Build V(P, p, k) and R(T, t, k) for function-value suboptimality.
P, p, T, t = IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality(
    algorithm,
    tau=0,
)

result = IterationIndependent.search_lyapunov(
    problem,
    algorithm,
    P,
    T,
    p=p,
    t=t,
    rho=1.0,
    remove_C4=False,
    solver_options=solver_options,
)

if result["status"] != "feasible":
    raise RuntimeError("No feasible Lyapunov certificate for this (gamma, delta) pair.")

print("Feasible certificate found with C4 enabled.")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: the fixed contraction parameter (`1.0` in this sublinear setup).
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

When the certificate is feasible, the certified function-value convergence is


\[f(x^k) - f(x^\star) \in o\!\left(\frac{1}{k}\right)
\quad \textup{ as } \quad k\to\infty,\]

where \(x^\star \in \Argmin_{x \in \calH} f(x)\).

Equivalently,


\[\lim_{k \to \infty} k\bigl(f(x^k)-f(x^\star)\bigr)=0.\]

Sweeping over 100 values of \(\gamma\) on \((0,2.61]\) and 100
values of \(\delta\) on \([-1,1]\) with \(L=1\) gives the plot
below, where each blue dot denotes a parameter pair with a feasible certificate.

[![Certified heavy-ball smooth-convex region in the (gamma, delta) plane.](https://autolyap.github.io/_images/heavy_ball_smooth_convex.svg)](https://autolyap.github.io/_images/heavy_ball_smooth_convex.svg)

<a id="references"></a>



## References

<a id="id2"></a>



<a id="id163"></a>



[[Pol64](https://autolyap.github.io/examples/heavy_ball/smooth_convex/#id1)]

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
