Canonical HTML: https://autolyap.github.io/examples/information_theoretic_exact_method/
Documentation version: 0.2.1

<a id="the-information-theoretic-exact-method"></a>



# The information-theoretic exact method

<a id="problem-setup"></a>



## Problem setup

Consider the unconstrained minimization problem


\[\minimize_{x \in \calH} f(x),\]

where \(f : \calH \to \reals\) is \(\mu\)-strongly convex and
\(L\)-smooth with \(0 < \mu < L\).

For initial points \(x^0, z^0 \in \calH\), set
\(q=\mu/L\), \(y^{-1}=x^0=z^0\), and \(\tilde{A}_0=0\).
For each \(k \in \naturals\), define


\[\begin{split}\begin{aligned}
\tilde{A}_{k+1}
&=
\frac{(1+q)\tilde{A}_k + 2\left(1 + \sqrt{(1+\tilde{A}_k)(1+q\tilde{A}_k)}\right)}
{(1-q)^2},
\\
\beta_k
&=
\frac{\tilde{A}_k}{(1-q)\tilde{A}_{k+1}},
\\
\delta_k
&=
\frac{(1-q)^2\tilde{A}_{k+1}-(1+q)\tilde{A}_k}{2(1+q+q\tilde{A}_k)}.
\end{aligned}\end{split}\]

The information-theoretic exact method (ITEM)
<a id="id1"></a>[[TD23](https://autolyap.github.io/examples/information_theoretic_exact_method/#id196 "Adrien Taylor and Yoel Drori. An optimal gradient method for smooth strongly convex minimization. Mathematical Programming, 199(1-2):557–594, 2023. doi:10.1007/s10107-022-01839-y.")] is


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
y^k &= (1-\beta_k)z^k + \beta_k x^k, \\
x^{k+1} &= y^k - \frac{1}{L}\nabla f(y^k), \\
z^{k+1} &= (1-q\delta_k)z^k + q\delta_k y^k - \frac{\delta_k}{L}\nabla f(y^k).
\end{aligned}
\right.\end{split}\]

In this example, we search for the smallest AutoLyap certificate constant
\(c_K\) such that


\[\|z^K-x^\star\|^2 \le c_K\|z^0-x^\star\|^2,\]

where \(x^\star \in \Argmin_{x \in \calH} f(x)\).

<a id="model-the-problem-in-autolyap-and-search-for-the-smallest-c-k"></a>



## Model the problem in AutoLyap and search for the smallest c\_K

- \(f\) is modeled by
  [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex").
- The optimization problem is represented by
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").
- The update rule is represented by
  [`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.ITEM").
- Initial and final distance-to-solution parameters
  \(\|z^k-x^\star\|^2\) are obtained with
  [`IterationDependent.get_parameters_state_component_distance_to_solution`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_distance_to_solution "autolyap.IterationDependent.get_parameters_state_component_distance_to_solution")
  using `ell=2` (ITEM state is \((x^k,z^k)\)).
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
from autolyap.algorithms import ITEM
from autolyap.problemclass import InclusionProblem, SmoothStronglyConvex

mu = 1.0
L = 200.0
K = 10
q = mu / L

problem = InclusionProblem([SmoothStronglyConvex(mu=mu, L=L)])
algorithm = ITEM(mu=mu, L=L)
solver_options = SolverOptions(backend="mosek_fusion")
# License-free option:
# solver_options = SolverOptions(backend="cvxpy", cvxpy_solver="CLARABEL")

Q_0, q_0 = IterationDependent.get_parameters_state_component_distance_to_solution(
    algorithm,
    k=0,
    ell=2,
)
Q_K, q_K = IterationDependent.get_parameters_state_component_distance_to_solution(
    algorithm,
    k=K,
    ell=2,
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
    raise RuntimeError("No feasible chained Lyapunov certificate for ITEM.")

c_K_autolyap = result["c_K"]
A_tilde_K = algorithm.get_A(K)
c_K_theory = 1.0 / (1.0 + q * A_tilde_K)

print(f"ITEM c_K (AutoLyap): {c_K_autolyap:.6e}")
print(f"ITEM c_K (theory):   {c_K_theory:.6e}")
```



The computed value `ITEM c_K (AutoLyap)` matches (up to solver numerical
tolerances) the theoretical horizon-`K` expression from
<a id="id2"></a>[[TD23](https://autolyap.github.io/examples/information_theoretic_exact_method/#id196 "Adrien Taylor and Yoel Drori. An optimal gradient method for smooth strongly convex minimization. Mathematical Programming, 199(1-2):557–594, 2023. doi:10.1007/s10107-022-01839-y."), Theorem 3]:


\[\|z^K-x^\star\|^2 \le c_K\|z^0-x^\star\|^2,
\qquad
c_K = \frac{1}{1+q\tilde{A}_K}.\]

Sweeping over \(K \in \llbracket 1, 100\rrbracket\) gives the log-log plot
below, with the theoretical bound in black and AutoLyap certificates as blue
dots.

[![ITEM c_K versus K in log-log scale with theoretical line and AutoLyap points.](https://autolyap.github.io/_images/information_theoretic_exact_method_c_vs_K_loglog.svg)](https://autolyap.github.io/_images/information_theoretic_exact_method_c_vs_K_loglog.svg)

<a id="references"></a>



## References

<a id="id3"></a>



<a id="id196"></a>



[TD23]
([1](https://autolyap.github.io/examples/information_theoretic_exact_method/#id1),[2](https://autolyap.github.io/examples/information_theoretic_exact_method/#id2))

Adrien Taylor and Yoel Drori. An optimal gradient method for smooth strongly convex minimization. *Mathematical Programming*, 199(1-2):557–594, 2023. [doi:10.1007/s10107-022-01839-y](https://doi.org/10.1007/s10107-022-01839-y).

TeX macro definitions used in this page:

```latex
\newcommand{\Argmin}{\operatorname*{Argmin}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\minimize}{\operatorname*{minimize}}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
