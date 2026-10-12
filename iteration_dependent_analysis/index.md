Canonical HTML: https://autolyap.github.io/iteration_dependent_analysis/
Documentation version: 0.2.1

<a id="iteration-dependent-analysis"></a>



# Iteration-dependent analysis

<a id="autolyap.IterationDependent"></a>





```text
class autolyap.IterationDependent
```



Bases: `object`

Iteration-dependent Lyapunov analysis utilities.

For the mathematical formulation, notation, and convergence statements,
see [Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

This class provides the corresponding computational interface, with
[`search_lyapunov()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.search_lyapunov "autolyap.IterationDependent.search_lyapunov") as the main entry point.

<a id="autolyap.IterationDependent.get_parameters_distance_to_solution"></a>





```text
static get_parameters_distance_to_solution( algo: Algorithm, k: int, i: int = 1, j: int = 1, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for the distance-to-solution metric at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = \|y_{i,j}^{k} - y^{\star}\|^{2}.\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[Q_k
=
\left(P_{(i,j)}Y_k^{k,k} - P_{(i,\star)}Y_\star^{k,k}\right)^\top
\left(P_{(i,j)}Y_k^{k,k} - P_{(i,\star)}Y_\star^{k,k}\right),\]

where:

- \(Y_k^{k,k}\) is retrieved via
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = k and k\_max = k.
- \(Y_\star^{k,k}\) is retrieved via
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = k and k\_max = k.
- \(P_{(i,j)}\) and \(P_{(i,\star)}\) are projection matrices retrieved via
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing m, m\_bar\_is,
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), and
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).
- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Component index, with
  \(i \in \llbracket 1, m\rrbracket\).
- j ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Evaluation index for component i, with
  \(j \in \llbracket 1, \NumEval_i\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or a required matrix is missing.

<a id="autolyap.IterationDependent.get_parameters_fixed_point_residual"></a>





```text
static get_parameters_fixed_point_residual( algo: Algorithm, k: int, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for the fixed-point residual at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = \|\bx^{k+1} - \bx^{k}\|^{2}.\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[Q_k
=
\left(X_{k+1}^{k,k} - X_k^{k,k}\right)^\top
\left(X_{k+1}^{k,k} - X_k^{k,k}\right),\]

where:

- \(X_k^{k,k}\) is retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = k and k\_max = k.
- \(X_{k+1}^{k,k}\) is retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = k and k\_max = k.

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing dimensions and
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or required \(X\) matrices are missing.

<a id="autolyap.IterationDependent.get_parameters_function_value_suboptimality"></a>





```text
static get_parameters_function_value_suboptimality( algo: Algorithm, k: int, j: int = 1, ) → Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute Lyapunov parameters for function-value suboptimality at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = f_{1}(y_{1,j}^{k}) - f_{1}(y^{\star}).\]

**Matrix construction**

This method applies only when \(m = \NumFunc = 1\).

The vector \(q_k\) is constructed as


\[q_k = \left(F_{(1,j,k)}^{k,k} - F_{(1,\star,\star)}^{k,k}\right)^\top,\]

where:

- \(F_{(1,j,k)}^{k,k}\) is retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = k and k\_max = k.
- \(F_{(1,\star,\star)}^{k,k}\) is retrieved via
  [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs"), using k\_min = k and k\_max = k.

The remaining matrix is set to zero:

- \(Q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance with m = m\_func = 1,
  m\_bar\_is, and [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).
- j ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Evaluation index for component 1, with
  \(j \in \llbracket 1, \NumEval_1\rrbracket\).

**Returns**

- ([`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]):
  A tuple \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid, required \(F\) matrices are missing,
  or \(m \ne 1\) / \(\NumFunc \ne 1\).

<a id="autolyap.IterationDependent.get_parameters_optimality_measure"></a>





```text
static get_parameters_optimality_measure( algo: Algorithm, k: int, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for the optimality measure at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\begin{split}\mathcal{V}(Q_k,q_k,k)
=
\begin{cases}
    \|u_{1,1}^{k}\|^{2}, & \text{if } m = 1, \\[0.5em]
    \left\|\sum_{i=1}^{m} u_{i,1}^{k}\right\|^{2}
    + \sum_{i=2}^{m} \|y_{1,1}^{k} - y_{i,1}^{k}\|^{2},
    & \text{if } m > 1.
\end{cases}\end{split}\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[\begin{split}Q_k
=
\begin{cases}
    \left(P_{(1,1)}U_k^{k,k}\right)^\top \left(P_{(1,1)}U_k^{k,k}\right), & \text{if } m = 1, \\[1em]
    \left(\sum_{i=1}^{m} P_{(i,1)}U_k^{k,k}\right)^\top
    \left(\sum_{i=1}^{m} P_{(i,1)}U_k^{k,k}\right)
    + \sum_{i=2}^{m}
    \left(\left(P_{(1,1)} - P_{(i,1)}\right)Y_k^{k,k}\right)^\top
    \left(\left(P_{(1,1)} - P_{(i,1)}\right)Y_k^{k,k}\right),
    & \text{if } m > 1,
\end{cases}\end{split}\]

where:

- \(U_k^{k,k}\) is retrieved via
  [`_get_Us()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Us "autolyap.algorithms.algorithm.Algorithm._get_Us"), using k\_min = k and k\_max = k.
- \(Y_k^{k,k}\) is retrieved via
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = k and k\_max = k.
- \(P_{(i,1)}\) are projection matrices retrieved via
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing m, m\_bar, m\_bar\_is,
  [`_get_Us()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Us "autolyap.algorithms.algorithm.Algorithm._get_Us"),
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), and
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or required matrices are missing.

<a id="autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference"></a>





```text
static get_parameters_state_component_cross_iteration_difference( algo: Algorithm, k: int, ell: int = 1, ell_prime: int = 1, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for a cross-iteration state-component difference metric at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = \|x_{\ell}^{k+1} - x_{\ell^{\prime}}^{k}\|^{2}.\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[Q_k
=
\left(e_\ell^\top X_{k+1}^{k,k} - e_{\ell^{\prime}}^\top X_k^{k,k}\right)^\top
\left(e_\ell^\top X_{k+1}^{k,k} - e_{\ell^{\prime}}^\top X_k^{k,k}\right),\]

where:

- \(X_k^{k,k}\) and \(X_{k+1}^{k,k}\) are retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = k and k\_max = k.
- \(e_\ell, e_{\ell^\prime} \in \mathbb{R}^{n}\) are standard basis vectors.

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing dimensions and
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).
- ell ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): State coordinate index at iteration \(k+1\), with
  \(\ell \in \llbracket 1, n\rrbracket\).
- ell\_prime ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): State coordinate index at iteration \(k\), with
  \(\ell^\prime \in \llbracket 1, n\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or a required matrix is missing.

<a id="autolyap.IterationDependent.get_parameters_state_component_difference"></a>





```text
static get_parameters_state_component_difference( algo: Algorithm, k: int, ell: int = 1, ell_prime: int = 1, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for a same-iteration state-component difference metric at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = \|x_{\ell}^{k} - x_{\ell^{\prime}}^{k}\|^{2}.\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[Q_k
=
\left(e_\ell^\top X_k^{k,k} - e_{\ell^{\prime}}^\top X_k^{k,k}\right)^\top
\left(e_\ell^\top X_k^{k,k} - e_{\ell^{\prime}}^\top X_k^{k,k}\right),\]

where:

- \(X_k^{k,k}\) is retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = k and k\_max = k.
- \(e_\ell, e_{\ell^\prime} \in \mathbb{R}^{n}\) are standard basis vectors.

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing dimensions and
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).
- ell ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): State coordinate index at iteration \(k\), with
  \(\ell \in \llbracket 1, n\rrbracket\).
- ell\_prime ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): State coordinate index at iteration \(k\), with
  \(\ell^\prime \in \llbracket 1, n\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or a required matrix is missing.

<a id="autolyap.IterationDependent.get_parameters_state_component_distance_to_solution"></a>





```text
static get_parameters_state_component_distance_to_solution( algo: Algorithm, k: int, ell: int = 1, ) → ndarray | Tuple[ndarray, ndarray]
```



[Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Compute Lyapunov parameters for a state-component distance-to-solution metric at iteration \(k\).

For the matrix constructions used in this method, see
[Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
For the role of \((Q_k,q_k)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**Resulting lower bounds**

With this choice of \((Q_k,q_k)\),


\[\mathcal{V}(Q_k,q_k,k) = \|x_{\ell}^{k} - y^{\star}\|^{2}.\]

**Matrix construction**

The matrix \(Q_k\) is constructed as


\[Q_k
=
\left(e_\ell^\top X_k^{k,k} - P_{(1,\star)}Y_\star^{k,k}\right)^\top
\left(e_\ell^\top X_k^{k,k} - P_{(1,\star)}Y_\star^{k,k}\right),\]

where:

- \(X_k^{k,k}\) is retrieved via
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"), using k\_min = k and k\_max = k.
- \(Y_\star^{k,k}\) is retrieved via
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), using k\_min = k and k\_max = k.
- \(P_{(1,\star)}\) is retrieved via
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").
- \(e_\ell \in \mathbb{R}^{n}\) is the ell-th standard basis vector.

The remaining vector is set to zero:

- If \(\NumFunc > 0\), then \(q_k = 0\).

**Parameters**

- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): Algorithm instance providing dimensions and
  [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs"),
  [`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys"), and
  [`_get_Ps()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ps "autolyap.algorithms.algorithm.Algorithm._get_Ps").
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative iteration index \(k\).
- ell ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): State coordinate index, with
  \(\ell \in \llbracket 1, n\rrbracket\).

**Returns**

- ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]):
  If algo.m\_func == 0, returns \(Q_k\) with


\[Q_k \in \sym^{n + \NumEval + m}.\]

  Otherwise, returns \((Q_k, q_k)\) with


\[\begin{split}\begin{aligned}
Q_k &\in \sym^{n + \NumEval + m},\\
q_k &\in \mathbb{R}^{\NumEvalFunc + \NumFunc}.
\end{aligned}\end{split}\]

**Raises**

- ValueError: If an input is invalid or a required matrix is missing.

<a id="autolyap.IterationDependent.search_lyapunov"></a>





```text
static search_lyapunov( prob: InclusionProblem, algo: Algorithm, K: int, Q_0: ndarray, Q_K: ndarray, q_0: ndarray | None = None, q_K: ndarray | None = None, solver_options: SolverOptions | None = None, verbosity: int = 1, ) → Mapping[str, Any]
```



[InclusionProblem](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem), [Algorithm](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [SolverOptions](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any)

Search for an iteration-dependent Lyapunov certificate via an SDP.

Given an inclusion problem, an algorithm, and user-specified targets
\((Q_0,q_0,Q_K,q_K,K)\), this method formulates and solves a
semidefinite feasibility problem for certificate variables
\((\{Q_k,q_k\}_{k=1}^{K-1}, c_K)\).

**Connection to theory**

For the formal statement of the chained quadratic Lyapunov inequality
and the role of \((Q_0,q_0,Q_K,q_K,K)\), see
[Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/).

**User-specified targets**

The tuple \((Q_0,q_0,Q_K,q_K)\) fixes the endpoint Lyapunov values
\(\mathcal{V}(Q_0,q_0,0)\) and \(\mathcal{V}(Q_K,q_K,K)\).
These endpoint parameters are fixed inputs to the SDP
(not optimization variables).

Built-in constructors for choosing endpoint parameters are:

- [`get_parameters_distance_to_solution()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_distance_to_solution "autolyap.IterationDependent.get_parameters_distance_to_solution")
- [`get_parameters_state_component_distance_to_solution()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_distance_to_solution "autolyap.IterationDependent.get_parameters_state_component_distance_to_solution")
- [`get_parameters_state_component_difference()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_difference "autolyap.IterationDependent.get_parameters_state_component_difference")
- [`get_parameters_state_component_cross_iteration_difference()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference "autolyap.IterationDependent.get_parameters_state_component_cross_iteration_difference")
- [`get_parameters_function_value_suboptimality()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_function_value_suboptimality "autolyap.IterationDependent.get_parameters_function_value_suboptimality")
- [`get_parameters_fixed_point_residual()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_fixed_point_residual "autolyap.IterationDependent.get_parameters_fixed_point_residual")
- [`get_parameters_optimality_measure()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.get_parameters_optimality_measure "autolyap.IterationDependent.get_parameters_optimality_measure")

The SDP then searches for intermediate \((Q_k,q_k)\) for
\(k \in \llbracket 1, K-1\rrbracket\) and the minimum feasible
\(c_K \ge 0\) so that the chained inequalities hold.
When \(\NumFunc = 0\), the vectors \(q_k\)
(and inputs q\_0, q\_K) are omitted.

**Parameters**

- prob (`Type`[[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")]): An
  [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem")
  instance containing interpolation conditions.
- algo (`Type`[[`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm")]): An
  [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm") instance providing
  dimensions and matrix helpers.
- K ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): A positive integer corresponding to \(K\) defining the iteration budget.
- Q\_0 ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A symmetric matrix corresponding to
  \(Q_0 \in \sym^{n + \NumEval + m}\).
- Q\_K ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A symmetric matrix corresponding to
  \(Q_K \in \sym^{n + \NumEval + m}\).
- q\_0 ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A vector corresponding to
  \(q_0 \in \mathbb{R}^{\NumEvalFunc + \NumFunc}\) for functional components
  (required if \(\NumFunc > 0\); otherwise None).
- q\_K ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A vector corresponding to
  \(q_K \in \mathbb{R}^{\NumEvalFunc + \NumFunc}\) for functional components
  (required if \(\NumFunc > 0\); otherwise None).
- solver\_options ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`SolverOptions`](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions "autolyap.solver_options.SolverOptions")]):
  Optional backend and parameter settings. Defaults to
  SolverOptions(backend=”mosek\_fusion”).
- verbosity ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Nonnegative output level. Defaults to 1.
  Set 0 to disable user-facing diagnostics, 1 for concise summaries,
  and 2 for per-iteration detail.

**Returns**

- (`Mapping`[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")]): Result mapping
  with keys status, solve\_status, c\_K, and certificate.

  - status ([`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")): One of “feasible”, “infeasible”, or
    “not\_solved”.
  - solve\_status ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]): Raw backend
    solve status (None when unavailable).
  - c\_K ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[`float`]): Optimal objective value when
    status == “feasible”; otherwise None. This is the horizon-dependent scalar.
  - certificate ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[`Mapping`[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")]]):
    None unless status == “feasible”.

  When status == “feasible”, certificate has:



```text
{
  "Q_sequence": List[np.ndarray],   # length K+1, Q_sequence[k] = Q_k
  "q_sequence": List[np.ndarray] | None,  # None when m_func == 0
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
  "iteration": int,
  "component": int,
  "interpolation_index": int,
  "pairs": List[{"j": int | "star", "k": int | "star"}],
  "value": float
}
```



  Field meanings and ranges:

  - iteration corresponds to \(k\) and satisfies
    \(k \in \llbracket 0, K-1 \rrbracket\).
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
    [{“j”: 2, “k”: 5}] and
    [{“j”: 1, “k”: 5}, {“j”: “star”, “k”: “star”}].

    Pair entries satisfy


\[\begin{split}\begin{aligned}
j &\in \llbracket 1, \bar m_i \rrbracket \cup \{\star\}, \\
k_{\text{pair}} &\in \{k, k+1\} \cup \{\star\},
\end{aligned}\end{split}\]

    with \(j=\star \Leftrightarrow k_{\text{pair}}=\star\).
  - value is the scalar multiplier for that record.

**Raises**

- ValueError: If input dimensions or other conditions are violated.

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
