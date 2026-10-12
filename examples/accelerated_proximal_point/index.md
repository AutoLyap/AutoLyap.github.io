Canonical HTML: https://autolyap.github.io/examples/accelerated_proximal_point/
Documentation version: 0.2.1

<a id="the-accelerated-proximal-point-method"></a>



# The accelerated proximal point method

<a id="problem-setup"></a>



## Problem setup

Consider the monotone inclusion


\[\text{find } x \in \calH \text{ such that } 0 \in G(x),\]

where \(G:\calH\rightrightarrows\calH\) is maximally monotone.

For initial points \(x^0,y^0,y^{-1}\in\calH\), step size
\(\gamma\in\reals_{++}\), and
\(\lambda_k = k/(k+2)\), the accelerated proximal point method
<a id="id1"></a>[[Kim21](https://autolyap.github.io/examples/accelerated_proximal_point/#id111 "Donghwan Kim. Accelerated proximal point method for maximally monotone operators. Mathematical Programming, 190(1-2):57–87, March 2021. doi:10.1007/s10107-021-01643-0.")] is


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
x^{k+1} &= J_{\gamma G}(y^k), \\
y^{k+1} &= x^{k+1} + \lambda_k(x^{k+1} - x^k) - \lambda_k(x^k - y^{k-1}).
\end{aligned}
\right.\end{split}\]

In this example, we use AutoLyap to compute the smallest certificate constant
\(c_K\) such that


\[\|x^{K+1} - y^K\|^2 \le c_K \|y^0 - y^\star\|^2,\]

where \(K \in \mathbb{N}\) is the iteration budget (horizon), and
\(y^\star \in \zer G\).

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-c-k"></a>



## Model the problem in AutoLyap and search for the smallest c\_K

- \(G\) is modeled by
  [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone").
- The monotone inclusion is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`AcceleratedProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.AcceleratedProximalPoint "autolyap.algorithms.AcceleratedProximalPoint").
- Initial-horizon parameters
  \(\|y^0-y^\star\|^2\) are obtained with
  [`IterationDependent.get_parameters_state_component_distance_to_solution`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_distance_to_solution "autolyap.IterationDependent.get_parameters_state_component_distance_to_solution").
- Final-horizon parameters
  \(\|x^{K+1}-y^K\|^2\) are obtained with
  [`IterationDependent.get_parameters_state_component_cross_iteration_difference`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference "autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference").
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
from autolyap.algorithms import AcceleratedProximalPoint
from autolyap.problemclass import InclusionProblem, MaximallyMonotone

gamma = 1.0
K = 10

problem = InclusionProblem([MaximallyMonotone()])
algorithm = AcceleratedProximalPoint(gamma=gamma, type="operator")
solver_options = SolverOptions(backend="mosek_fusion")

# V(0) = ||y^0 - y^*||^2. In APP state x^k = (x^k, y^k, y^{k-1}),
# y^k is state component ell=2 (1-indexed).
Q_0 = IterationDependent.get_parameters_state_component_distance_to_solution(
    algorithm,
    k=0,
    ell=2,
)

# V(K) = ||x^{K+1} - y^K||^2 = ||x_1^{K+1} - x_2^K||^2
# with 1-indexed state components: ell=1 and ell_prime=2.
Q_K = IterationDependent.get_parameters_state_component_cross_iteration_difference(
    algorithm,
    k=K,
    ell=1,
    ell_prime=2,
)

result = IterationDependent.search_lyapunov(
    problem,
    algorithm,
    K,
    Q_0,
    Q_K,
    solver_options=solver_options,
)
if result["status"] != "feasible":
    raise RuntimeError("No feasible chained Lyapunov certificate for APP.")

c_K_autolyap = result["c_K"]
c_K_kim = 1.0 / (K + 1.0) ** 2

print(f"APP c_K (AutoLyap): {c_K_autolyap:.6e}")
print(f"APP c_K (Kim):      {c_K_kim:.6e}")
```



The computed value `APP c_K (AutoLyap)` matches (up to solver numerical
tolerances) the theoretical horizon-`K` expression from
<a id="id2"></a>[[Kim21](https://autolyap.github.io/examples/accelerated_proximal_point/#id111 "Donghwan Kim. Accelerated proximal point method for maximally monotone operators. Mathematical Programming, 190(1-2):57–87, March 2021. doi:10.1007/s10107-021-01643-0."), Theorem 4.1], i.e.,


\[\|x^{K+1} - y^K\|^2 \le \frac{1}{(K+1)^2}\|y^0 - y^\star\|^2.\]

Sweeping over \(K \in \llbracket 1, 100\rrbracket\) gives the plot
below, with Kim’s theoretical constant from
<a id="id3"></a>[[Kim21](https://autolyap.github.io/examples/accelerated_proximal_point/#id111 "Donghwan Kim. Accelerated proximal point method for maximally monotone operators. Mathematical Programming, 190(1-2):57–87, March 2021. doi:10.1007/s10107-021-01643-0."), Theorem 4.1]
\(c_K^{\mathrm{Kim}} = \frac{1}{(K+1)^2}\) as a line and AutoLyap
certificates as blue dots.

[![Accelerated-proximal-point c_K versus K in log-log scale with Kim's bound and AutoLyap points.](https://autolyap.github.io/_images/accelerated_proximal_point_method_c_vs_K_loglog.svg)](https://autolyap.github.io/_images/accelerated_proximal_point_method_c_vs_K_loglog.svg)

<a id="references"></a>



## References

<a id="id4"></a>



<a id="id111"></a>



[Kim21]
([1](https://autolyap.github.io/examples/accelerated_proximal_point/#id1),[2](https://autolyap.github.io/examples/accelerated_proximal_point/#id2),[3](https://autolyap.github.io/examples/accelerated_proximal_point/#id3))

Donghwan Kim. Accelerated proximal point method for maximally monotone operators. *Mathematical Programming*, 190(1-2):57–87, March 2021. [doi:10.1007/s10107-021-01643-0](https://doi.org/10.1007/s10107-021-01643-0).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\zer}{\operatorname*{zer}}
```
