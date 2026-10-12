Canonical HTML: https://autolyap.github.io/examples/triple_momentum/
Documentation version: 0.2.1

<a id="the-triple-momentum-method"></a>



# The triple-momentum method

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x),\]

where \(f : \calH \to \reals\) is \(\mu\)-strongly convex and
\(L\)-smooth, with \(0 < \mu < L\).

Let \(q=\mu/L\) and define


\[\alpha_{\mathrm{tm}} = \frac{2-\sqrt{q}}{L}, \qquad
\beta_{\mathrm{tm}} = \frac{(1-\sqrt{q})^2}{1+\sqrt{q}}, \qquad
\gamma_{\mathrm{tm}} =
\frac{(1-\sqrt{q})^2}{(2-\sqrt{q})(1+\sqrt{q})}.\]

For initial points \(x^{-1},x^0\in\calH\), the triple-momentum method
<a id="id1"></a>[[VSFL18](https://autolyap.github.io/examples/triple_momentum/#id209 "Bryan Van Scoy, Randy A. Freeman, and Kevin M. Lynch. The fastest known globally convergent first-order method for minimizing strongly convex functions. IEEE Control Systems Letters, 2(1):49–54, January 2018. doi:10.1109/lcsys.2017.2722406.")] is given by


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
y^k &= x^k + \gamma_{\mathrm{tm}}(x^k-x^{k-1}), \\
x^{k+1} &= x^k + \beta_{\mathrm{tm}}(x^k-x^{k-1})
             - \alpha_{\mathrm{tm}}\nabla f(y^k).
\end{aligned}
\right.\end{split}\]

In this
example, we search for the smallest contraction factor
\(\rho\in[0,1)\) provable using AutoLyap such that


\[\|y^k-x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,\]

where \(x^\star\in\Argmin_{x\in\calH} f(x)\).

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-rho"></a>



## Model the problem in AutoLyap and search for the smallest rho

- \(f\) is modeled by
  [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`TripleMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TripleMomentum "autolyap.algorithms.TripleMomentum").
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
from autolyap.algorithms import TripleMomentum
from autolyap.problemclass import InclusionProblem, SmoothStronglyConvex

mu = 1.0
L = 4.0

problem = InclusionProblem([SmoothStronglyConvex(mu=mu, L=L)])
algorithm = TripleMomentum(mu=mu, L=L)
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
rho_theory = (1.0 - (mu / L) ** 0.5) ** 2

print(f"rho (AutoLyap): {rho_autolyap:.8f}")
print(f"rho (theory):   {rho_theory:.8f}")
```



What to inspect in `result`:

- `result["status"]`: `feasible`, `infeasible`, or `not_solved`.
- `result["solve_status"]`: raw backend status.
- `result["rho"]`: certified contraction factor when feasible.
- `result["certificate"]`: Lyapunov certificate matrices/scalars.

The computed value `rho (AutoLyap)` matches, up to solver numerical tolerances,
the theoretical rate expression for the triple-momentum method
<a id="id2"></a>[[VSFL18](https://autolyap.github.io/examples/triple_momentum/#id209 "Bryan Van Scoy, Randy A. Freeman, and Kevin M. Lynch. The fastest known globally convergent first-order method for minimizing strongly convex functions. IEEE Control Systems Letters, 2(1):49–54, January 2018. doi:10.1109/lcsys.2017.2722406."), Theorem 1 and Corollary 1]:


\[\|y^k-x^\star\|^2 \in \mathcal{O}(\rho^k) \quad \textup{ as } \quad k\to\infty,
\qquad
\rho = \left(1-\sqrt{\frac{\mu}{L}}\right)^2.\]

Equivalently,


\[\|y^k-x^\star\|
\in \mathcal{O}\!\left(\left(1-\sqrt{\frac{\mu}{L}}\right)^k\right)
\quad \textup{ as } \quad k\to\infty.\]

Sweeping over 100 values of \(L\) on \((1,100]\), with \(\mu=1\)
fixed, gives the plot below. The theoretical rate is shown in black and the
MOSEK-backed AutoLyap certificates as blue dots.

[![Triple-momentum rho versus L with the theoretical line and AutoLyap points.](https://autolyap.github.io/_images/triple_momentum_rho_vs_smoothness.svg)](https://autolyap.github.io/_images/triple_momentum_rho_vs_smoothness.svg)

<a id="references"></a>



## References

<a id="id3"></a>



<a id="id209"></a>



[VSFL18]
([1](https://autolyap.github.io/examples/triple_momentum/#id1),[2](https://autolyap.github.io/examples/triple_momentum/#id2))

Bryan Van Scoy, Randy A. Freeman, and Kevin M. Lynch. The fastest known globally convergent first-order method for minimizing strongly convex functions. *IEEE Control Systems Letters*, 2(1):49–54, January 2018. [doi:10.1109/lcsys.2017.2722406](https://doi.org/10.1109/lcsys.2017.2722406).

TeX macro definitions used in this page:

```latex
\newcommand{\Argmin}{\operatorname*{Argmin}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
```
