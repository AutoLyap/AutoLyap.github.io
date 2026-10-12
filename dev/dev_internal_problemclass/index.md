Canonical HTML: https://autolyap.github.io/dev/dev_internal_problemclass/
Documentation version: 0.2.1

<a id="internal-problem-class-modules"></a>



# Internal problem-class modules

This page documents problem-class implementation helpers used by contributors.

Public condition class docs are in [Problem classes](https://autolyap.github.io/problem_class/).

<a id="module-map"></a>



## Module map

| Module | Responsibility |
| --- | --- |
| [`autolyap.problemclass`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass "autolyap.problemclass") | Package-level exports for the problem-class subsystem. |
| [`autolyap.problemclass.indices`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass.indices "autolyap.problemclass.indices") | Parses and stores interpolation index constraints. |
| [`autolyap.problemclass.base`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass.base "autolyap.problemclass.base") | Abstract interfaces for operator and function interpolation conditions. |
| [`autolyap.problemclass.functions`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass.functions "autolyap.problemclass.functions") | Concrete function interpolation conditions and parameter checks. |
| [`autolyap.problemclass.operators`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass.operators "autolyap.problemclass.operators") | Concrete operator interpolation conditions and parameter checks. |
| [`autolyap.problemclass.inclusion_problem`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass.inclusion_problem "autolyap.problemclass.inclusion_problem") | Container that aggregates interpolation conditions into one problem definition. |

<a id="module-autolyap.problemclass"></a>



<a id="module-docstrings"></a>

## Module docstrings

<a id="module-autolyap.problemclass.base"></a>



Abstract interpolation-condition interfaces.

<a id="module-autolyap.problemclass.functions"></a>



Concrete function interpolation-condition classes.

<a id="module-autolyap.problemclass.operators"></a>



Concrete operator interpolation-condition classes.

<a id="module-autolyap.problemclass.inclusion_problem"></a>



Inclusion-problem container and validation logic.

<a id="key-internal-inclusion-problem-method"></a>



## Key internal inclusion-problem method

<a id="autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data"></a>





```text
InclusionProblem._get_component_data( index: int, ) → List[Tuple[ndarray, _InterpolationIndices] | Tuple[ndarray, ndarray, bool, _InterpolationIndices]]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [_InterpolationIndices](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Return the raw interpolation data for a component.

Tuple conventions follow the class-level reference in
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem").

**Parameters**

- index ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): The component index (1-indexed).

**Returns**

- ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")]]]):
  A list of tuples containing the interpolation data. For operator conditions, each tuple is
  (matrix, interpolation\_indices); for function conditions, each tuple is
  (matrix, vector, eq, interpolation\_indices).

**Raises**

- ValueError: If the index is not defined.

<a id="module-autolyap.problemclass.indices"></a>



<a id="internal-condition-and-index-types"></a>

## Internal condition and index types

Interpolation index helpers for problem classes.

<a id="autolyap.problemclass.indices._InterpolationIndices"></a>





```text
class autolyap.problemclass.indices._InterpolationIndices( value: str, )
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Bases: `object`

Wrapper for interpolation indices that enforces allowed values.

<a id="class-level-reference"></a>



### Class-level reference

This class-level docstring centralizes the allowed symbolic labels used to
index interpolation pairs in problem classes.

**Allowed values**

- `r1<r2`: unordered pairs of distinct interpolation entries \(r_1\) and \(r_2\).
- `r1!=r2`: ordered pairs of distinct interpolation entries \(r_1\) and \(r_2\).
- `r1`: single-entry interpolation constraints (index \(r_1\)).
- `r1!=star`: pairs between any interpolation entry \(r_1\) and the distinguished star entry.

**Parameters**

- value ([`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")): The interpolation index string.

**Raises**

- ValueError: If value is not in the allowed set.

<a id="autolyap.problemclass.base._InterpolationCondition"></a>





```text
class autolyap.problemclass.base._InterpolationCondition
```



Bases: [`ABC`](https://autolyap.github.io/dev/dev_external_reference_targets/#abc.ABC "abc.ABC")

Abstract base class for an interpolation condition.

<a id="class-level-reference"></a>



### Class-level reference

This class-level docstring centralizes the data contract implemented by
interpolation-condition subclasses.

Derived classes must implement [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition.get_data "autolyap.problemclass.base._InterpolationCondition.get_data"), which returns interpolation data as a list
of tuples encoding the relevant vectors, matrices, and interpolation indices.
See [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition") and
[`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition") for
the detailed notation and the corresponding interpolation constraints.

<a id="autolyap.problemclass.base._InterpolationCondition.get_data"></a>





```text
abstractmethod get_data() → Sequence[Tuple[ndarray, _InterpolationIndices] | Tuple[ndarray, ndarray, bool, _InterpolationIndices]]
```



[Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [_InterpolationIndices](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Return interpolation data.

Shared tuple conventions follow the class-level reference in
[`_InterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition "autolyap.problemclass.base._InterpolationCondition").

**Returns**

- ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")]]]):
  A list of tuples representing the interpolation data.

<a id="autolyap.problemclass.base._OperatorInterpolationCondition"></a>





```text
class autolyap.problemclass.base._OperatorInterpolationCondition
```



Bases: [`_InterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition "autolyap.problemclass.base._InterpolationCondition")

Base class for operator interpolation conditions.

<a id="class-level-reference"></a>



### Class-level reference

This class-level docstring centralizes notation and tuple conventions for
operator interpolation constraints.

Return a list of tuples of the form:

(matrix, interpolation\_indices)

where:

- **matrix** is a square, symmetric 2D numpy array.
- **interpolation\_indices** is an instance of [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices").

The returned **matrix** corresponds to the quadratic term
\(M_{(i,o)}^{\textup{op}}\), where \(i \in \IndexOp\) indexes the operator component
and \(o\) selects one of its interpolation constraints. These enter inequalities of the form


\[\langle z, (M_{(i,o)}^{\textup{op}} \kron \Id) z \rangle \le 0,\]

where \(z\) stacks the relevant \(y_{i,j}^{k}\) and \(u_{i,j}^{k}\) entries selected
by the interpolation indices. Concretely, if the indices select pairs
\((j_1,k_1),\ldots,(j_b,k_b)\), then


\[z = (y_{i,j_1}^{k_1},\ldots,y_{i,j_b}^{k_b},u_{i,j_1}^{k_1},\ldots,u_{i,j_b}^{k_b}).\]

For operator components, \(u_{i,j}^{k} \in G_i(y_{i,j}^{k})\).
No linear term appears for operator conditions.

For any matrix \(M \in \mathbb{R}^{d \times b}\), the tensor product
\(M \kron \Id\) denotes the linear map \(\calH^{b} \to \calH^{d}\) defined by


\[(M \kron \Id)z = \Big(\sum_{\ell=1}^{b}[M]_{1,\ell} z_\ell,\ldots,\sum_{\ell=1}^{b}[M]_{d,\ell} z_\ell\Big),\]

for \(z = (z_1,\ldots,z_b) \in \calH^{b}\).

<a id="autolyap.problemclass.base._OperatorInterpolationCondition.get_data"></a>





```text
abstractmethod get_data() → Sequence[Tuple[ndarray, _InterpolationIndices]]
```



[Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [_InterpolationIndices](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices)

Return operator interpolation data.

The tuple format and notation follow the class-level reference in
[`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition").

**Returns**

- ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")]]): A list of tuples, each containing a square
  symmetric matrix and an instance of [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices").

<a id="autolyap.problemclass.base._FunctionInterpolationCondition"></a>





```text
class autolyap.problemclass.base._FunctionInterpolationCondition
```



Bases: [`_InterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition "autolyap.problemclass.base._InterpolationCondition")

Base class for function interpolation conditions.

<a id="class-level-reference"></a>



### Class-level reference

This class-level docstring centralizes notation and tuple conventions for
function interpolation constraints.

Return a list of tuples of the form:

(matrix, vector, eq, interpolation\_indices)

where:

- **matrix** is a square, symmetric 2D numpy array.
- **vector** is a 1D numpy array.
- **eq** is a boolean flag (True for equality, False for inequality).
- **interpolation\_indices** is an instance of [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices").
- **matrix** has shape \((2b, 2b)\) where \(b\) is the length of **vector**.

The returned **vector** corresponds to the linear term
\(a_{(i,o)}^{\textup{func-ineq}}\) or \(a_{(i,o)}^{\textup{func-eq}}\), where
\(i \in \IndexFunc\) indexes the functional component and \(o\) selects one of its
interpolation constraints, in function values,
and the **matrix** corresponds to the quadratic term
\(M_{(i,o)}^{\textup{func-ineq}}\) or \(M_{(i,o)}^{\textup{func-eq}}\) in constraints of the form


\[\begin{split}\big(a_{(i,o)}^{\textup{func-ineq}}\big)^{\top}\begin{bmatrix}
    F_{i,j_1}^{k_1} \\
    \vdots \\
    F_{i,j_b}^{k_b}
\end{bmatrix}
+ \langle z, (M_{(i,o)}^{\textup{func-ineq}} \kron \Id) z \rangle \le 0,\end{split}\]

and


\[\begin{split}\big(a_{(i,o)}^{\textup{func-eq}}\big)^{\top}\begin{bmatrix}
    F_{i,j_1}^{k_1} \\
    \vdots \\
    F_{i,j_b}^{k_b}
\end{bmatrix}
+ \langle z, (M_{(i,o)}^{\textup{func-eq}} \kron \Id) z \rangle = 0.\end{split}\]

Here \(z\) stacks the corresponding \(y_{i,j_\ell}^{k_\ell}\) and \(u_{i,j_\ell}^{k_\ell}\)
terms. Concretely, if the indices select pairs \((j_1,k_1),\ldots,(j_b,k_b)\), then


\[z = (y_{i,j_1}^{k_1},\ldots,y_{i,j_b}^{k_b},u_{i,j_1}^{k_1},\ldots,u_{i,j_b}^{k_b}).\]

For functional components, \(u_{i,j}^{k} \in \partial f_i(y_{i,j}^{k})\), and
\(F_{i,j}^{k} = f_i(y_{i,j}^{k})\). The flag **eq** selects equality vs. inequality.

For any matrix \(M \in \mathbb{R}^{d \times b}\), the tensor product
\(M \kron \Id\) denotes the linear map \(\calH^{b} \to \calH^{d}\) defined by


\[(M \kron \Id)z = \Big(\sum_{\ell=1}^{b}[M]_{1,\ell} z_\ell,\ldots,\sum_{\ell=1}^{b}[M]_{d,\ell} z_\ell\Big),\]

for \(z = (z_1,\ldots,z_b) \in \calH^{b}\).

<a id="autolyap.problemclass.base._FunctionInterpolationCondition.get_data"></a>





```text
abstractmethod get_data() → Sequence[Tuple[ndarray, ndarray, bool, _InterpolationIndices]]
```



[Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [_InterpolationIndices](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices)

Return function interpolation data.

The tuple format and notation follow the class-level reference in
[`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition").

**Returns**

- ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")]]): A list of tuples, each
  containing a square symmetric matrix, a 1D vector, a boolean flag, and an instance of
  [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices").

<a id="autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition"></a>





```text
class autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition( mu: int | float, L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition")

Base class for function interpolation conditions parameterized by \(\mu\) and \(L\).

<a id="class-level-reference"></a>



### Class-level reference

This class-level docstring centralizes notation for the shared
\((\mu, L)\) interpolation family used by its concrete subclasses.

Provides a helper to compute interpolation data from \(\mu\) and \(L\).
This base class supports both smooth and nonsmooth conditions by setting \(L\) appropriately.

This template applies to \(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) with
parameters satisfying \(-\infty < \mu < L \le +\infty\) and \(L > 0\)
(nonsmooth cases are encoded by \(L = +\infty\)).

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} \in \partial f(x_{r_1})\), \(g_{r_2} \in \partial f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\), the condition enforced is


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle + \frac{\mu}{2}\|x_{r_1} - x_{r_2}\|^2
+ \frac{1}{2(L-\mu)}\|g_{r_1} - g_{r_2} - \mu(x_{r_1} - x_{r_2})\|^2,\]

  where \(\frac{1}{2(L-\mu)}\) is interpreted as \(0\) when \(L = +\infty\).
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(F = (F_{r_1}, F_{r_2})\) and
  \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\), the interpolation inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0, \qquad a = (-1, 1),\]

  with


\[\begin{split}M =
\begin{cases}
  \frac{1}{2(L-\mu)}
  \begin{bmatrix}
    L\mu & -L\mu & -\mu & L \\
    -L\mu & L\mu & \mu & -L \\
    -\mu & \mu & 1 & -1 \\
    L & -L & -1 & 1
  \end{bmatrix}, & \text{if } L < +\infty, \\
  \frac{1}{2}
  \begin{bmatrix}
    \mu & -\mu & 0 & 1 \\
    -\mu & \mu & 0 & -1 \\
    0 & 0 & 0 & 0 \\
    1 & -1 & 0 & 0
  \end{bmatrix}, & \text{if } L = +\infty.
\end{cases}\end{split}\]

  The returned interpolation indices are `r1!=r2`.
  The eq flag returned by [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition.get_data "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition.get_data") is `False`.

Note: Many classes below are specializations obtained by choosing \(\mu\) and \(L\)
(e.g., weakly convex uses \(\mu = -\tilde{\mu}\) with \(L=+\infty\); smooth uses
\(\mu = -L\)).

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The convexity parameter corresponding to \(\mu\).
  For strongly convex functions, \(\mu > 0\); for convex functions, \(\mu = 0\); for weakly convex
  functions, \(\mu < 0\).
- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The smoothness parameter corresponding to \(L\).
  For nonsmooth functions, \(L\) is set to infinity.

**Raises**

- ValueError: If mu is not a number, if \(L \le 0\) with \(L < +\infty\),
  or if \(\mu \geq L\).

<a id="autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition.get_data"></a>





```text
get_data() → List[Tuple[ndarray, ndarray, bool, _InterpolationIndices]]
```



[List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [_InterpolationIndices](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices)

Return interpolation data for the function condition.

The tuple format and notation follow the class-level reference in
[`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition").

**Returns**

- ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool"), [`_InterpolationIndices`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.indices._InterpolationIndices "autolyap.problemclass.indices._InterpolationIndices")]]): A list containing one
  tuple with the matrix, vector, eq flag, and interpolation indices.

<a id="internal-parameter-validation-helpers"></a>



## Internal parameter-validation helpers

<a id="autolyap.problemclass.functions._ensure_positive_finite"></a>





```text
autolyap.problemclass.functions._ensure_positive_finite( value: int | float, parameter_name: str, error_message: str, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Return value as float after enforcing strict positivity and finiteness.

<a id="autolyap.problemclass.functions._ensure_positive_mu_tilde"></a>





```text
autolyap.problemclass.functions._ensure_positive_mu_tilde( mu_tilde: int | float, context_name: str, ) → Tuple[float, float]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Validate \(\tilde{\mu} > 0\) and return both \((\tilde{\mu}, -\tilde{\mu})\).

Many weakly-convex templates are written with \(\mu=-\tilde{\mu}\).
This helper keeps that conversion explicit and centralized.

<a id="autolyap.problemclass.operators._ensure_positive_finite"></a>





```text
autolyap.problemclass.operators._ensure_positive_finite( value: int | float, parameter_name: str, error_message: str, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Return value as float after enforcing strict positivity and finiteness.

<a id="autolyap.problemclass.operators._ensure_finite"></a>





```text
autolyap.problemclass.operators._ensure_finite( value: int | float, parameter_name: str, error_message: str, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Return value as float after enforcing finiteness.

TeX macro definitions used in this page:

```latex
\newcommand{\Id}{\operatorname*{Id}}
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\kron}{\otimes}
\newcommand{\p}[1]{\mathord{( #1 )}}
```
