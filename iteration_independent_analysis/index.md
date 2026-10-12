Canonical HTML: https://autolyap.github.io/iteration_independent_analysis/
Documentation version: 0.2.1

<a id="iteration-independent-analysis"></a>



# Iteration-independent analysis

<a id="autolyap.IterationIndependent"></a>





```text
class autolyap.IterationIndependent
```



Bases: `object`

Iteration-independent Lyapunov analysis utilities.

For the mathematical formulation, notation, and convergence statements,
see [Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

This class provides the corresponding computational interface, with
[`search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov") as the main entry point.

<a id="autolyap.IterationIndependent.search_lyapunov"></a>





```text
static search_lyapunov( prob: InclusionProblem, algo: Algorithm, P: ndarray, T: ndarray, p: ndarray | None = None, t: ndarray | None = None, rho: float = 1.0, h: int = 0, alpha: int = 0, Q_equals_P: bool = False, S_equals_T: bool = False, q_equals_p: bool = False, s_equals_t: bool = False, remove_C2: bool = False, remove_C3: bool = False, remove_C4: bool = True, solver_options: SolverOptions | None = None, verbosity: int = 1, ) → Mapping[str, Any]
```



[InclusionProblem](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem), [Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any)

Search for an iteration-independent Lyapunov certificate via an SDP.

Given an inclusion problem, an algorithm, and user-specified targets
\((P,p,T,t,\rho,h,\alpha)\), this method formulates and solves a
semidefinite feasibility problem for certificate variables
\((Q,q,S,s)\).

**Connection to theory**

For the formal statement of the quadratic Lyapunov inequality, i.e., conditions
[(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1)-[(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4), and the role of
\((P,p,T,t,\rho,h,\alpha)\), see
[Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/).

**User-specified targets**

The tuple \((P,p,T,t)\) fixes the target lower bounds
\(\mathcal{V}(P,p,k)\) and \(\mathcal{R}(T,t,k)\).
The user is responsible for ensuring these target functions are
nonnegative over the relevant iterates and problem class, i.e.,


\[\begin{split}\begin{align}
    \p{\forall k \in \naturals}& \quad \mathcal{V}(P,p,k) \geq 0, \\
    \p{\forall k \in \naturals}& \quad \mathcal{R}(T,t,k) \geq 0.
\end{align}\end{split}\]

Built-in constructors for choosing \((P,p,T,t)\) are:

- Linear convergence:

  - [`get_parameters_distance_to_solution()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.get_parameters_distance_to_solution "autolyap.IterationIndependent.LinearConvergence.get_parameters_distance_to_solution")
  - [`get_parameters_function_value_suboptimality()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality "autolyap.IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality")
- Sublinear convergence:

  - [`get_parameters_fixed_point_residual()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual "autolyap.IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual")
  - [`get_parameters_duality_gap()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_duality_gap "autolyap.IterationIndependent.SublinearConvergence.get_parameters_duality_gap")
  - [`get_parameters_function_value_suboptimality()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality "autolyap.IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality")
  - [`get_parameters_optimality_measure()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.SublinearConvergence.get_parameters_optimality_measure "autolyap.IterationIndependent.SublinearConvergence.get_parameters_optimality_measure")

The SDP then searches for a feasible certificate
\((Q,q,S,s)\) consistent with those targets.

**Parameters**

- prob (`Type`[[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")]): An
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")
  instance containing interpolation conditions.
- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An
  [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm") instance providing
  dimensions and methods to compute matrices.
- P ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): Candidate symmetric matrix corresponding to
  \(P \in \sym^{n + (h+1)\NumEval + m}\).
- T ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): Candidate symmetric matrix corresponding to
  \(T \in \sym^{n + (h+\alpha+2)\NumEval + m}\).
- p ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): Candidate vector corresponding to
  \(p \in \mathbb{R}^{(h+1)\NumEvalFunc + \NumFunc}\) for functional components
  (required if \(\NumFunc > 0\)).
- t ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): Candidate vector corresponding to
  \(t \in \mathbb{R}^{(h+\alpha+2)\NumEvalFunc + \NumFunc}\) for functional components
  (required if \(\NumFunc > 0\)).
- rho (`float`): A scalar contraction parameter corresponding to \(\rho\) used in forming the Lyapunov inequality
  (typically \(\rho \in [0,1]\)).
- h ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative integer corresponding to \(h\) defining history.
- alpha ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative integer corresponding to \(\alpha\) defining overlap.
- Q\_equals\_P ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): If True, sets Q equal to P.
- S\_equals\_T ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): If True, sets S equal to T.
- q\_equals\_p ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): For functional components, if True, sets q equal to p.
- s\_equals\_t ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): For functional components, if True, sets s equal to t.
- remove\_C2 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C2)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c2).
- remove\_C3 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3).
- remove\_C4 ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Flag to remove [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4).
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
    solve status (None when unavailable).
  - rho (`float`): The input contraction factor.
  - certificate ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[`Mapping`[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")]]):
    None unless status == “feasible”.

  When status == “feasible”, certificate has:



```text
{
  "Q": np.ndarray,   # full symmetric matrix
  "S": np.ndarray,   # full symmetric matrix
  "q": np.ndarray | None,  # None when m_func == 0
  "s": np.ndarray | None,  # None when m_func == 0
  "multipliers": {
    "operator_lambda": List[record],
    "function_lambda": List[record],
    "function_nu": List[record]
  }
}
```



  Each multiplier list entry is a record with:



```text
record = {
  "condition": str,
  "component": int,
  "interpolation_index": int,
  "pairs": List[{"j": int | "star", "k": int | "star"}],
  "value": float
}
```



  Field meanings and ranges:

  - condition is in the active subset of
    [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1), [(C2)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c2),
    [(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3), and [(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4)
    (always including [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1)).
  - component corresponds to \(i\) and satisfies
    \(i \in \llbracket 1, m \rrbracket\).
  - interpolation\_index corresponds to \(o\), where
    \(o\) is the zero-based index from
    \(\text{enumerate}(\text{prob.get_component_data}(i))\), so
    \(o \in \llbracket 0, \text{len}(\text{prob.get_component_data}(i)) - 1 \rrbracket\).
  - pairs is the concrete interpolation-pair list used by that multiplier.
    Its length is determined by
    [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices"):
    1 for “r1” and 2 for “r1<r2”, “r1!=r2”, “r1!=star”.
    Typical examples are
    [{“j”: 2, “k”: 0}] and
    [{“j”: 1, “k”: 0}, {“j”: “star”, “k”: “star”}].

    Pair entries satisfy


\[\begin{split}\begin{aligned}
j &\in \llbracket 1, \bar m_i \rrbracket \cup \{\star\}, \\
k &\in \llbracket 0, k_{\textup{max}}(\text{condition}) \rrbracket \cup \{\star\},
\end{aligned}\end{split}\]

    with \(j=\star \Leftrightarrow k=\star\), where


\[\begin{split}\begin{aligned}
k_{\textup{max}}(\text{"C1"}) &= h+\alpha+1, \\
k_{\textup{max}}(\text{"C2"}) &= h, \\
k_{\textup{max}}(\text{"C3"}) &= h+\alpha+1, \\
k_{\textup{max}}(\text{"C4"}) &= h+\alpha+2.
\end{aligned}\end{split}\]

    where “C1”/”C2”/”C3”/”C4” correspond to
    [(C1)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c1)/[(C2)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c2)/[(C3)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c3)/[(C4)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-c4).
  - value is the scalar multiplier for that record.

**Raises**

- ValueError: If input dimensions or other conditions are violated.

<a id="linear-convergence-helpers"></a>



<a id="iter-independent-linear-helpers"></a>

## Linear-convergence helpers

<a id="autolyap.IterationIndependent.LinearConvergence.get_parameters_distance_to_solution"></a>





```text
static LinearConvergence.get_parameters_distance_to_solution( algo: Algorithm, h: int = 0, alpha: int = 0, i: int = 1, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.IterationIndependent.LinearConvergence.get_parameters_function_value_suboptimality"></a>





```text
static LinearConvergence.get_parameters_function_value_suboptimality( algo: Algorithm, h: int = 0, alpha: int = 0, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.IterationIndependent.LinearConvergence.bisection_search_rho"></a>





```text
static LinearConvergence.bisection_search_rho( prob: InclusionProblem, algo: Algorithm, P: ndarray, T: ndarray, p: ndarray | None = None, t: ndarray | None = None, h: int = 0, alpha: int = 0, Q_equals_P: bool = False, S_equals_T: bool = False, q_equals_p: bool = False, s_equals_t: bool = False, remove_C2: bool = False, remove_C3: bool = False, remove_C4: bool = True, lower_bound: float = 0.0, upper_bound: float = 1.0, tol: float = 1e-12, solver_options: SolverOptions | None = None, verbosity: int = 1, ) → Mapping[str, Any]
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

<a id="sublinear-convergence-helpers"></a>



<a id="iter-independent-sublinear-helpers"></a>

## Sublinear-convergence helpers

<a id="autolyap.IterationIndependent.SublinearConvergence.get_parameters_fixed_point_residual"></a>





```text
static SublinearConvergence.get_parameters_fixed_point_residual( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.IterationIndependent.SublinearConvergence.get_parameters_duality_gap"></a>





```text
static SublinearConvergence.get_parameters_duality_gap( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.IterationIndependent.SublinearConvergence.get_parameters_function_value_suboptimality"></a>





```text
static SublinearConvergence.get_parameters_function_value_suboptimality( algo: Algorithm, h: int = 0, alpha: int = 0, j: int = 1, tau: int = 0, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.IterationIndependent.SublinearConvergence.get_parameters_optimality_measure"></a>





```text
static SublinearConvergence.get_parameters_optimality_measure( algo: Algorithm, h: int = 0, alpha: int = 0, tau: int = 0, ) → Tuple[ndarray, ndarray] | Tuple[ndarray, ndarray, ndarray, ndarray]
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

TeX macro definitions used in this page:

```latex
\newcommand{\NumEval}{\bar{m}}
\newcommand{\NumEvalFunc}{\bar{m}_{\textup{func}}}
\newcommand{\NumFunc}{m_{\textup{func}}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
\newcommand{\sym}{\mathbb{S}}
```
