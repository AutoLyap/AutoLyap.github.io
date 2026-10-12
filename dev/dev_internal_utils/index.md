Canonical HTML: https://autolyap.github.io/dev/dev_internal_utils/
Documentation version: 0.2.1

<a id="internal-utility-modules"></a>



# Internal utility modules

These helpers are shared across algorithms, problem classes, and analysis/solver modules.

Public API docs using these helpers are surfaced through [Solver backends](https://autolyap.github.io/solver_backends/) and [API reference](https://autolyap.github.io/api_reference/).

<a id="module-map"></a>



## Module map

| Module | Responsibility |
| --- | --- |
| [`autolyap.utils.validation`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils.validation "autolyap.utils.validation") | Scalar, index, and finite-array validation helpers. |
| [`autolyap.utils.helper_functions`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils.helper_functions "autolyap.utils.helper_functions") | Symmetric-matrix construction helpers. |
| [`autolyap.utils.backend_types`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils.backend_types "autolyap.utils.backend_types") | Protocol-based structural typing aliases for optional solver backends. |

<a id="module-autolyap.utils"></a>



<a id="package-namespace"></a>

## Package namespace

<a id="module-autolyap.utils.validation"></a>



<a id="validation-helpers"></a>

## Validation helpers

Validation helpers shared across algorithms and problem classes.

<a id="autolyap.utils.validation.ensure_finite_array"></a>





```text
autolyap.utils.validation.ensure_finite_array( array: ndarray, name: str, ) → None
```



[ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Raise if array contains non-finite entries (NaN or inf).

<a id="autolyap.utils.validation.ensure_index_list"></a>





```text
autolyap.utils.validation.ensure_index_list( values: Iterable[int | float | integer | floating], name: str, m: int, ) → List[int]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [integer](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.integer), [floating](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.floating), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List)

Validate a sorted, duplicate-free component-index list.

The returned list is guaranteed to contain integers in `[1, m]` in
strictly increasing order.

<a id="autolyap.utils.validation.ensure_integral"></a>





```text
autolyap.utils.validation.ensure_integral( value: int | float | integer | floating, name: str, minimum: int | None = None, maximum: int | None = None, ) → int
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [integer](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.integer), [floating](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.floating), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Validate and normalize an integer-valued scalar.

Rejects booleans explicitly and optionally enforces lower/upper bounds.

<a id="autolyap.utils.validation.ensure_m_bar_list"></a>





```text
autolyap.utils.validation.ensure_m_bar_list( values: Iterable[int | float | integer | floating], m: int, ) → List[int]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [integer](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.integer), [floating](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.floating), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List)

Validate per-component evaluation counts.

Ensures the sequence length equals `m` and every entry is an integer
strictly greater than zero.

<a id="autolyap.utils.validation.ensure_real_number"></a>





```text
autolyap.utils.validation.ensure_real_number( value: int | float | integer | floating, name: str, finite: bool = False, minimum: float | None = None, maximum: float | None = None, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [integer](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.integer), [floating](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.floating), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Validate and normalize a real-valued scalar.

Rejects booleans and NaNs explicitly. Optional flags enforce finiteness
and lower/upper bounds.

<a id="module-autolyap.utils.helper_functions"></a>



<a id="symmetric-matrix-helpers"></a>

## Symmetric-matrix helpers

<a id="autolyap.utils.helper_functions.create_symmetric_matrix"></a>





```text
autolyap.utils.helper_functions.create_symmetric_matrix( upper_triangle_values: UpperTriangleValuesProtocol, n: int, ) → ndarray
```



[UpperTriangleValuesProtocol](https://autolyap.github.io/dev/dev_internal_utils/#autolyap.utils.backend_types.UpperTriangleValuesProtocol), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Convert a list of upper triangle values to a symmetric matrix.

**Parameters**

- upper\_triangle\_values: List of length \(n(n+1)/2\) containing the upper triangle
  and diagonal values.
- n: Size of the symmetric matrix.

**Returns**

- Symmetric matrix of size \(n \times n\).

**Raises**

- ValueError: If the length of upper\_triangle\_values is not \(n(n+1)/2\).

<a id="autolyap.utils.helper_functions.create_symmetric_matrix_expression"></a>





```text
autolyap.utils.helper_functions.create_symmetric_matrix_expression( Xij: MosekUpperTriangleVectorProtocol, n: int, ) → Any
```



[MosekUpperTriangleVectorProtocol](https://autolyap.github.io/dev/dev_internal_utils/#autolyap.utils.backend_types.MosekUpperTriangleVectorProtocol), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any)

Convert a list of upper triangle variables to a symmetric matrix expression.

**Parameters**

- Xij: MOSEK variable containing the upper triangle and diagonal values.
- n: Size of the symmetric matrix.

**Returns**

- Symmetric matrix expression of size \(n \times n\).

<a id="module-autolyap.utils.backend_types"></a>



<a id="backend-typing-protocols"></a>

## Backend typing protocols

Shared structural typing protocols for optional solver backends.

<a id="autolyap.utils.backend_types.CvxpyModuleProtocol"></a>





```text
class autolyap.utils.backend_types.CvxpyModuleProtocol( *args, **kwargs, )
```



Subset of the CVXPY module API used by AutoLyap.

<a id="autolyap.utils.backend_types.CvxpyStatusModuleProtocol"></a>





```text
class autolyap.utils.backend_types.CvxpyStatusModuleProtocol( *args, **kwargs, )
```



Subset of CVXPY status constants used by AutoLyap.

<a id="autolyap.utils.backend_types.CvxpyValueHandleProtocol"></a>





```text
class autolyap.utils.backend_types.CvxpyValueHandleProtocol( *args, **kwargs, )
```



Protocol for solved CVXPY handles exposing `value`.

<a id="autolyap.utils.backend_types.MosekExprProtocol"></a>





```text
class autolyap.utils.backend_types.MosekExprProtocol( *args, **kwargs, )
```



Subset of `mosek.fusion.Expr` used in helper builders.

<a id="autolyap.utils.backend_types.MosekFusionModuleProtocol"></a>





```text
class autolyap.utils.backend_types.MosekFusionModuleProtocol( *args, **kwargs, )
```



Subset of MOSEK Fusion module attributes used by AutoLyap.

<a id="autolyap.utils.backend_types.MosekLevelHandleProtocol"></a>





```text
class autolyap.utils.backend_types.MosekLevelHandleProtocol( *args, **kwargs, )
```



Protocol for solved MOSEK handles exposing `level()`.

<a id="autolyap.utils.backend_types.MosekModelProtocol"></a>





```text
class autolyap.utils.backend_types.MosekModelProtocol( *args, **kwargs, )
```



Protocol for MOSEK Fusion model parameter forwarding.

<a id="autolyap.utils.backend_types.MosekUpperTriangleSolutionHandleProtocol"></a>





```text
class autolyap.utils.backend_types.MosekUpperTriangleSolutionHandleProtocol( *args, **kwargs, )
```



Protocol for MOSEK upper-triangle handles reused after solve.

<a id="autolyap.utils.backend_types.MosekUpperTriangleVectorProtocol"></a>





```text
class autolyap.utils.backend_types.MosekUpperTriangleVectorProtocol( *args, **kwargs, )
```



Protocol for MOSEK upper-triangle vector handles.

<a id="autolyap.utils.backend_types.SupportsScalarProduct"></a>





```text
class autolyap.utils.backend_types.SupportsScalarProduct( *args, **kwargs, )
```



Protocol for scalar-like symbols participating in backend products.

<a id="autolyap.utils.backend_types.SupportsStringConversion"></a>





```text
class autolyap.utils.backend_types.SupportsStringConversion( *args, **kwargs, )
```



Protocol for values normalized through `str(value)`.

<a id="autolyap.utils.backend_types.UpperTriangleValuesProtocol"></a>





```text
class autolyap.utils.backend_types.UpperTriangleValuesProtocol( *args, **kwargs, )
```



Finite indexable container of numeric upper-triangle entries.
