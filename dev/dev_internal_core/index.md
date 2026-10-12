Canonical HTML: https://autolyap.github.io/dev/dev_internal_core/
Documentation version: 0.2.1

<a id="internal-analysis-and-solver-modules"></a>



# Internal analysis and solver modules

This page focuses on internals that support analysis assembly and solver execution.

Public API docs for these topics are in [Lyapunov analyses](https://autolyap.github.io/lyapunov_analyses/) and [Solver backends](https://autolyap.github.io/solver_backends/).

<a id="module-map"></a>



## Module map

| Module | Responsibility |
| --- | --- |
| [`autolyap.iteration_independent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_independent "autolyap.iteration_independent") | Iteration-independent SDP assembly and feasibility checks. |
| [`autolyap.iteration_dependent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_dependent "autolyap.iteration_dependent") | Finite-horizon SDP assembly and chained-certificate checks. |
| [`autolyap.solver_options`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.solver_options "autolyap.solver_options") | Backend normalization and CVXPY solver-argument preparation. |

<a id="module-autolyap.iteration_independent"></a>



<a id="module-docstrings"></a>

## Module docstrings

<a id="iteration-independent-helper-namespaces"></a>



<a id="module-autolyap.solver_options"></a><a id="module-autolyap.iteration_dependent"></a>

## Iteration-independent helper namespaces

<a id="autolyap.iteration_independent._LinearConvergence"></a>





```text
class autolyap.iteration_independent._LinearConvergence
```



Bases: `object`

Internal namespace for linear-convergence helpers.

This class groups static utilities exposed through
[`IterationIndependent`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent "autolyap.IterationIndependent").

**Notes**

- This class is internal and not part of the public API.

<a id="autolyap.iteration_independent._LinearConvergence.bisection_search_rho"></a>





```text
static bisection_search_rho( prob: InclusionProblem, algo: Algorithm, P: ndarray, T: ndarray, p: ndarray | None = None, t: ndarray | None = None, h: int = 0, alpha: int = 0, Q_equals_P: bool = False, S_equals_T: bool = False, q_equals_p: bool = False, s_equals_t: bool = False, remove_C2: bool = False, remove_C3: bool = False, remove_C4: bool = True, lower_bound: float = 0.0, upper_bound: float = 1.0, tol: float = 1e-12, solver_options: SolverOptions | None = None, verbosity: int = 1, ) → Mapping[str, Any]
```



[InclusionProblem](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem), [Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any)

Perform a bisection search to find the minimum contraction parameter \(\rho\).

This method performs a bisection search over \(\rho\) in the interval
\([{\text{lower_bound}}, {\text{upper_bound}}]\) to find the minimal value for which the
iteration-independent Lyapunov inequality holds. At each step it re-solves the same
model with an updated \(\rho\) until the interval size is below \({\text{tol}}\).

Each feasibility check is performed by
[`search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov");
see its documentation for the enforced SDP feasibility checks.

**Parameters**

- prob (`Type`[[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")]): An [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")
  instance containing interpolation conditions.
- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm") instance providing
  dimensions and methods.
- P ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A symmetric matrix corresponding to \(P \in \sym^{n + (h+1)\NumEval + m}\).
- T ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A symmetric matrix corresponding to \(T \in \sym^{n + (h+\alpha+2)\NumEval + m}\).
- p ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A vector corresponding to \(p \in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc}\) for functional components (if applicable).
- t ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A vector corresponding to \(t \in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}\) for functional components (if applicable).
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative integer corresponding to \(h\) defining the history for the matrices.
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative integer corresponding to \(\alpha\) for extending the horizon.
- Q\_equals\_P ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): If True, set Q equal to P.
- S\_equals\_T ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): If True, set S equal to T.
- q\_equals\_p ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): For functional components, if True, set q equal to p.
- s\_equals\_t ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): For functional components, if True, set s equal to t.
- remove\_C2 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C2)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c2).
- remove\_C3 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3).
- remove\_C4 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4).
- lower\_bound (`float`): Lower bound for \(\rho\).
- upper\_bound (`float`): Upper bound for \(\rho\).
- tol (`float`): Tolerance for the bisection search stopping criterion.
- solver\_options ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`SolverOptions`](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions "autolyap.solver_options.SolverOptions")]):
  Optional backend and parameter settings. Defaults to
  SolverOptions(backend=”mosek\_fusion”).
- verbosity ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative output level. Defaults to 1.
  Set 0 to disable user-facing diagnostics, 1 for concise summaries,
  and 2 for per-constraint detail.

**Returns**

- (`Mapping`[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")]): Result mapping
  with keys status, solve\_status, rho, and certificate.

  - status ([`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")): One of “feasible”, “infeasible”, or
    “not\_solved”.
  - solve\_status ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]): Raw backend
    solve status for the terminal check (None when unavailable).
  - rho ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[`float`]): Best feasible
    bisection value when status == “feasible”; otherwise None.
  - certificate ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[`Mapping`[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")]]):
    Feasibility certificate when status == “feasible”; otherwise None.

  The certificate schema is exactly the same as in
  [`search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov").

**Raises**

- ValueError: If any input is out of range or the bounds are inconsistent.
- mosek.fusion.OptimizeError: If the MOSEK backend is selected and raises
  a license-related error during optimization.

<a id="autolyap.iteration_independent._LinearConvergence.get_parameters_distance_to_solution"></a>





```text
static get_parameters_distance_to_solution( algo: Algorithm, h: int = 0, alpha: int = 0, i: int = 1, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices for the distance-to-solution metric.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

With this choice of \((P,p,T,t)\),


\[\begin{split}\begin{aligned}
\mathcal{V}(P,p,k) &= \|y_{i,j}^{k+\tau} - y^\star\|^2,\\
\mathcal{R}(T,t,k) &= 0.
\end{aligned}\end{split}\]

**Matrix construction**

The matrix \(P\) is constructed as


\[P = \left( P_{(i,j)}\, Y_\tau^{0,h} - P_{(i,\star)}\, Y_\star^{0,h} \right)^{\top}
    \left( P_{(i,j)}\, Y_\tau^{0,h} - P_{(i,\star)}\, Y_\star^{0,h} \right),\]

where:

- \(Y_\tau^{0,h}\) is the \(Y\) matrix at iteration \(\tau\) over \(\llbracket 0, h\rrbracket\),
  retrieved via [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = 0 and k\_max = h.
- \(Y_\star^{0,h}\) is the “star” \(Y\) matrix over \(\llbracket 0, h\rrbracket\),
  retrieved via [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = 0 and k\_max = h.
- \(P_{(i,j)}\) and \(P_{(i,\star)}\) are the projection matrices for component \(i\),
  retrieved via [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").

The remaining matrices and vectors are set to zero:

- \(T = 0\).
- If \(\NumFunc > 0\), then \(p = 0\) and \(t = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm"). It must
  provide algo.m, algo.m\_bar\_is, [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), and
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the time horizon
  \(\llbracket 0, h\rrbracket\)
  for \(Y\) matrices.
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) (and \(t\)).
- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Component index (1-indexed) corresponding to \(i\). Default is 1; must satisfy
  \(i \in \llbracket 1, m\rrbracket\), where m = algo.m.
- j ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Evaluation index for component i corresponding to \(j\). Default is 1; must satisfy
  \(j \in \llbracket 1, \NumEval_i\rrbracket\), where \(\NumEval_i\) is given by
  algo.m\_bar\_is[i-1].
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\). Default is 0; must satisfy
  \(\tau \in \llbracket 0, h\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns (P, T) with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m}.
\end{aligned}\end{split}\]

  Otherwise, returns (P, p, T, t), where \(P\) is computed as above and
  \(T\), \(p\), and \(t\) are zero arrays with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If any input is out of its valid range or if required matrices are missing.

<a id="autolyap.iteration_independent._LinearConvergence.get_parameters_function_value_suboptimality"></a>





```text
static get_parameters_function_value_suboptimality( algo: Algorithm, h: int = 0, alpha: int = 0, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices and vectors for function-value suboptimality.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

With this choice of \((P,p,T,t)\),


\[\begin{split}\begin{aligned}
\mathcal{V}(P,p,k) &= f_1(y_{1,j}^{k+\tau}) - f_1(y^\star),\\
\mathcal{R}(T,t,k) &= 0.
\end{aligned}\end{split}\]

**Matrix construction**

This method applies only when \(m = \NumFunc = 1\).

The vector \(p\) is constructed as


\[p = \left( F_{(1,j,\tau)}^{0,h} - F_{(1,\star,\star)}^{0,h} \right)^{\top},\]

where:

- \(F_{(1,j,\tau)}^{0,h}\) is retrieved via [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"),
  using k\_min = 0 and k\_max = h.
- \(F_{(1,\star,\star)}^{0,h}\) is retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = 0 and k\_max = h.

The remaining matrices and vectors are set to zero:

- \(P = 0\).
- \(T = 0\).
- \(t = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm"). It must
  satisfy algo.m == 1, algo.m\_func == 1, and provide
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs").
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the horizon
  \(\llbracket 0, h\rrbracket\) for
  \(F\) matrices.
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) and \(t\).
- j ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Evaluation index for component 1 corresponding to \(j\). Default is 1; must satisfy
  \(j \in \llbracket 1, \NumEval_1\rrbracket\), where \(\NumEval_1\) is given by
  algo.m\_bar\_is[0].
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\). Default is 0; must satisfy
  \(\tau \in \llbracket 0, h\rrbracket\).

**Returns**

- ([`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A tuple \((P, p, T, t)\),
  where \(p\) is computed as above (a one-dimensional NumPy array), and \(P\), \(T\), and
  \(t\) are zero arrays, with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If algo.m != 1 or algo.m\_func != 1, if any input parameter is out of range,
  or if the required \(F\) matrices are not found.

<a id="autolyap.iteration_independent._SublinearConvergence"></a>





```text
class autolyap.iteration_independent._SublinearConvergence
```



Bases: `object`

Internal namespace for sublinear-convergence helpers.

This class groups static utilities exposed through
[`IterationIndependent`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent "autolyap.IterationIndependent").

**Notes**

- This class is internal and not part of the public API.

<a id="autolyap.iteration_independent._SublinearConvergence.get_parameters_duality_gap"></a>





```text
static get_parameters_duality_gap( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices for the duality gap.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

For iteration index \(\tau\) (with \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\)),

Warning

TODO.

**Matrix construction**

The matrix \(T\) and vector \(t\) are constructed as


\[\begin{split}T = -\frac{1}{2} \sum_{i=1}^{m} \begin{bmatrix}
P_{(i,\star)}\, U_\star^{0,h+\alpha+1} \\
P_{(i,1)}\, Y_\tau^{0,h+\alpha+1}
\end{bmatrix}^{\top}
\begin{bmatrix}
0 & 1 \\
1 & 0
\end{bmatrix}
\begin{bmatrix}
P_{(i,\star)}\, U_\star^{0,h+\alpha+1} \\
P_{(i,1)}\, Y_\tau^{0,h+\alpha+1}
\end{bmatrix},\end{split}\]

and


\[t = \sum_{i=1}^{m} \left( F_{(i,1,\tau)}^{0,h+\alpha+1} - F_{(i,\star,\star)}^{0,h+\alpha+1} \right)^{\top}.\]

where:

- \(U_\star^{0,h+\alpha+1}\) is retrieved via
  [`_get_Us()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Us "autolyap.algorithms.algorithm.Algorithm._get_Us"), using k\_min = 0 and k\_max = h+alpha+1.
- \(Y_\tau^{0,h+\alpha+1}\) is retrieved via
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = 0 and k\_max = h+alpha+1.
- \(F_{(i,1,\tau)}^{0,h+\alpha+1}\) and \(F_{(i,\star,\star)}^{0,h+\alpha+1}\) are retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = 0 and k\_max = h+alpha+1.
- \(P_{(i,\star)}\) and \(P_{(i,1)}\) are retrieved via
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").

The remaining matrices and vectors are set to zero:

- \(P = 0\).
- \(p = 0\).

**Requirements**

Requires \(m = \NumFunc\) (i.e., all components are functional).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm") (with
  algo.m == algo.m\_func).
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the time horizon
  \(\llbracket 0, h\rrbracket\) for
  \(P\).
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) and \(t\).
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\) for computing the duality gap. Must satisfy
  \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\).

**Returns**

- ([`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A tuple \((P, p, T, t)\), where
  \(t\) is a one-dimensional NumPy array, with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If any input parameter is out of its valid range, if required matrices
  are missing, or if \(m \ne \NumFunc\).

<a id="autolyap.iteration_independent._SublinearConvergence.get_parameters_fixed_point_residual"></a>





```text
static get_parameters_fixed_point_residual( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices for the fixed-point residual.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

For iteration index \(\tau\) (with \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\)),
this choice of \((P,p,T,t)\) gives:


\[\begin{split}\begin{aligned}
\mathcal{V}(P,p,k) &= 0,\\
\mathcal{R}(T,t,k) &= \|\bx^{k+\tau+1} - \bx^{k+\tau}\|^2.
\end{aligned}\end{split}\]

**Matrix construction**

The matrix \(T\) is constructed as


\[T = \left( X_{\tau+1}^{0, h+\alpha+1} - X_{\tau}^{0, h+\alpha+1} \right)^{\top}
    \left( X_{\tau+1}^{0, h+\alpha+1} - X_{\tau}^{0, h+\alpha+1} \right),\]

where:

- \(X_{\tau}^{0,h+\alpha+1}\) is retrieved via [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"),
  using k\_min = 0 and k\_max = h+alpha+1.
- \(X_{\tau+1}^{0,h+\alpha+1}\) is retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = 0 and k\_max = h+alpha+1.

The remaining matrices and vectors are set to zero:

- \(P = 0\).
- If \(\NumFunc > 0\), then \(p = 0\) and \(t = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm").
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the time horizon
  \(\llbracket 0, h\rrbracket\) for
  \(P\).
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) (and \(t\)).
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\) for computing the fixed-point residual.
  Must satisfy
  \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns (P, T) with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m}.
\end{aligned}\end{split}\]

  Otherwise, returns (P, p, T, t) with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If any input parameter is out of its valid range or if the required \(X\) matrices are missing.

<a id="autolyap.iteration_independent._SublinearConvergence.get_parameters_function_value_suboptimality"></a>





```text
static get_parameters_function_value_suboptimality( algo: Algorithm, h: int = 0, alpha: int = 0, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices and vectors for function-value suboptimality.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

With this choice of \((P,p,T,t)\),


\[\begin{split}\begin{aligned}
\mathcal{V}(P,p,k) &= 0,\\
\mathcal{R}(T,t,k) &= f_1(y_{1,j}^{k+\tau}) - f_1(y^\star).
\end{aligned}\end{split}\]

**Matrix construction**

This method applies only when \(m = \NumFunc = 1\).

The vector \(t\) is constructed as


\[t = \left( F_{(1,j,\tau)}^{0,h+\alpha+1} - F_{(1,\star,\star)}^{0,h+\alpha+1} \right)^{\top},\]

where:

- \(F_{(1,j,\tau)}^{0,h+\alpha+1}\) is retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = 0 and k\_max = h+alpha+1.
- \(F_{(1,\star,\star)}^{0,h+\alpha+1}\) is retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = 0 and k\_max = h+alpha+1.

The remaining matrices and vectors are set to zero:

- \(P = 0\).
- \(T = 0\).
- \(p = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm"). It must
  satisfy algo.m == 1, algo.m\_func == 1, and provide
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs").
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the horizon
  \(\llbracket 0, h + \alpha + 1\rrbracket\) for \(F\) matrices.
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) and \(t\).
- j ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Evaluation index for component 1 corresponding to \(j\). Default is 1; must satisfy
  \(j \in \llbracket 1, \NumEval_1\rrbracket\), where \(\NumEval_1\) is given by
  algo.m\_bar\_is[0].
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\). Default is 0; must satisfy
  \(\tau \in \llbracket 0, h + \alpha + 1\rrbracket\).

**Returns**

- ([`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A tuple \((P, p, T, t)\), where
  \(t\) is computed as above (a one-dimensional NumPy array), and \(P\), \(T\), and \(p\)
  are zero arrays, with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If algo.m != 1 or algo.m\_func != 1, if any input parameter is out of range,
  or if the required \(F\) matrices are not found.

<a id="autolyap.iteration_independent._SublinearConvergence.get_parameters_optimality_measure"></a>





```text
static get_parameters_optimality_measure( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute matrices for the optimality measure.

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((P,p,T,t)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**Resulting lower bounds**

For iteration index \(\tau\) (with \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\)),
this choice of \((P,p,T,t)\) gives:


\[\mathcal{V}(P,p,k) = 0,\]

and


\[\begin{split}\mathcal{R}(T,t,k)
=
\begin{cases}
    \|u_{1,1}^{k+\tau}\|^2 & \text{if } m = 1, \\
    \left\|\sum_{i=1}^{m} u_{i,1}^{k+\tau}\right\|^2
    + \sum_{i=2}^{m} \|y_{1,1}^{k+\tau} - y_{i,1}^{k+\tau}\|^2
    & \text{if } m > 1.
\end{cases}\end{split}\]

**Matrix construction**

The matrix \(T\) is constructed as


\[\begin{split}T =
\begin{cases}
  \left( P_{(1,1)}\, U_\tau^{0,h+\alpha+1} \right)^{\top} \left( P_{(1,1)}\, U_\tau^{0,h+\alpha+1} \right)
  & \text{if } m = 1, \\[1em]
  \left( \left( \sum_{i=1}^{m} P_{(i,1)}\, U_\tau^{0,h+\alpha+1} \right)^{\top} \left( \sum_{i=1}^{m} P_{(i,1)}\, U_\tau^{0,h+\alpha+1} \right)
  + \sum_{i=2}^{m} \left( \left( P_{(1,1)} - P_{(i,1)} \right) Y_\tau^{0,h+\alpha+1} \right)^{\top} \left( \left( P_{(1,1)} - P_{(i,1)} \right) Y_\tau^{0,h+\alpha+1} \right) \right)
  & \text{if } m > 1,
\end{cases}\end{split}\]

where:

- \(U_\tau^{0,h+\alpha+1}\) is retrieved via [`_get_Us()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Us "autolyap.algorithms.algorithm.Algorithm._get_Us"),
  using k\_min = 0 and k\_max = h+alpha+1.
- \(Y_\tau^{0,h+\alpha+1}\) is retrieved via [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"),
  using k\_min = 0 and k\_max = h+alpha+1.
- \(P_{(i,1)}\) are projection matrices retrieved via
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").

The remaining matrices and vectors are set to zero:

- \(P = 0\).
- If \(\NumFunc > 0\), then \(p = 0\) and \(t = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An instance of [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm").
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(h\) defining the time horizon
  \(\llbracket 0, h\rrbracket\) for
  \(P\).
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A nonnegative integer corresponding to \(\alpha\) for extending the horizon
  for \(T\) (and \(t\)).
- tau ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Iteration index corresponding to \(\tau\) for computing the optimality measure. Must satisfy
  \(\tau \in \llbracket 0, h+\alpha+1\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns (P, T) with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m}.
\end{aligned}\end{split}\]

  Otherwise, returns (P, p, T, t) with


\[\begin{split}\begin{aligned}
P &\in \sym^{n + (h+1)\NumEval + m},\\
T &\in \sym^{n + (h+\alpha+2)\NumEval + m},\\
p &\in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc},\\
t &\in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If any input parameter is out of its valid range or if required matrices are missing.

<a id="solver-option-helper-functions"></a>



## Solver option helper functions

<a id="autolyap.solver_options._normalize_solver_options"></a>





```text
autolyap.solver_options._normalize_solver_options( options: SolverOptions | None, ) → SolverOptions
```



[SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions)

Validate and normalize user-provided solver options.

If options is None, returns the default `SolverOptions()` profile.
Otherwise, this routine checks backend names, normalizes mapping fields,
and returns a sanitized immutable SolverOptions instance.

<a id="autolyap.solver_options._get_cvxpy_solve_kwargs"></a>





```text
autolyap.solver_options._get_cvxpy_solve_kwargs( options: SolverOptions, ) → Dict[str, Any]
```



[SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any)

Build keyword arguments for `cvxpy.Problem.solve(...)`.

Merges:
1. Selected solver name (solver).
2. Built-in defaults for known solvers.
3. User overrides from `cvxpy_solver_params`.

Always defaults `warm_start=True` unless explicitly overridden.

<a id="autolyap.solver_options._get_cvxpy_accepted_statuses"></a>





```text
autolyap.solver_options._get_cvxpy_accepted_statuses( cp: CvxpyStatusModuleProtocol, options: SolverOptions, ) → set[str]
```



[CvxpyStatusModuleProtocol](https://autolyap.github.io/dev/dev_internal_utils/#autolyap.utils.backend_types.CvxpyStatusModuleProtocol), [SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Return CVXPY statuses treated as successful solves.

Always includes `OPTIMAL`; optionally includes
`OPTIMAL_INACCURATE` when enabled by solver options.

<a id="autolyap.solver_options._normalize_mapping"></a>





```text
autolyap.solver_options._normalize_mapping( mapping: Mapping[str, Any] | None, name: str, ) → Dict[str, Any] | None
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict)

Validate an optional mapping and return it as a plain dict copy.

<a id="autolyap.solver_options._normalize_named_mapping"></a>





```text
autolyap.solver_options._normalize_named_mapping( mapping: Mapping[str, Any] | None, name: str, ) → Dict[str, Any] | None
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict)

Validate an optional mapping with string keys and return a dict copy.

Keys must be non-empty strings after stripping whitespace.

TeX macro definitions used in this page:

```latex
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\sym}{\mathbb{S}}
```
