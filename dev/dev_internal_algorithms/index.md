Canonical HTML: https://autolyap.github.io/dev/dev_internal_algorithms/
Documentation version: 0.2.1

<a id="internal-algorithm-modules"></a>



# Internal algorithm modules

This page is for contributors implementing or modifying algorithm internals.

Public class-level API docs are in [Algorithms](https://autolyap.github.io/algorithms/).

<a id="base-contract"></a>



## Base contract

Use the base class contract when introducing a new algorithm module.



```text
class autolyap.algorithms.algorithm.Algorithm( n: int, m: int, m_bar_is: List[int], I_func: List[int], I_op: List[int], )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List)

Bases: [`ABC`](https://autolyap.github.io/dev/dev_external_reference_targets/#abc.ABC "abc.ABC")

Abstract base class (ABC) for algorithms expressed in the state-space
representation used by AutoLyap.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation and representation conventions.

**Note**

- For a given method, the state-space representation is not unique.
- Concrete subclasses must implement [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD"); all other methods are implemented by this base class.



```text
__init__( n: int, m: int, m_bar_is: List[int], I_func: List[int], I_op: List[int], ) → None
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List)

Initialize an [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.Algorithm") with the
structural parameters of the representation.

**Parameters**

- n ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): the state dimension, i.e., \(\bx^{k} \in \calH^{n}\).
- m ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): the number of components \(m\) in the inclusion problem.
- m\_bar\_is ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")]): the list \((\bar{m}_i)_{i=1}^{m}\) of evaluation counts per component,
  with \(\bar{m}_i \in \mathbb{N}\) for each \(i\).
- I\_func ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")]): the index set \(\IndexFunc\) for functional components.
- I\_op ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")]): the index set \(\IndexOp\) for operator components.

**Raises**

- ValueError: if \(n < 1\), \(m < 1\), \(m \neq \text{len}(m\_bar\_is)\),
  if any \(\bar{m}_i \le 0\),
  \(\IndexFunc\) and \(\IndexOp\) are not disjoint, do not cover
  \(\llbracket 1, m\rrbracket\), or are not strictly increasing sequences.



```text
_cache_get( cache: OrderedDict[Tuple[int, int], CacheValueT], key: Tuple[int, int], ) → CacheValueT | None
```



[OrderedDict](https://autolyap.github.io/dev/dev_external_reference_targets/#collections.OrderedDict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [CacheValueT](https://autolyap.github.io/dev/dev_external_reference_targets/#autolyap.algorithms.algorithm.CacheValueT)

Return one cached entry and refresh its LRU recency on hit.

`key` is a horizon key `(k_min, k_max)`. On a cache hit, the entry
is moved to the end of the [`collections.OrderedDict`](https://autolyap.github.io/dev/dev_external_reference_targets/#collections.OrderedDict "collections.OrderedDict") so it is
treated as most recently used. On miss, return `None`.



```text
_cache_set( cache: OrderedDict[Tuple[int, int], CacheValueT], key: Tuple[int, int], value: CacheValueT, ) → CacheValueT
```



[OrderedDict](https://autolyap.github.io/dev/dev_external_reference_targets/#collections.OrderedDict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [CacheValueT](https://autolyap.github.io/dev/dev_external_reference_targets/#autolyap.algorithms.algorithm.CacheValueT)

Store one horizon-keyed cache entry with bounded LRU eviction.

The inserted/updated entry becomes most recently used. If the cache
exceeds `_cache_maxsize`, the least recently used entry is evicted.
Returns `value` so call sites can assign and return in one step.



```text
_clear_all_caches() → None
```



Clear every cached artifact after structural updates.

Structural fields (for example `n`, `m`, and index partitions) may
change matrix dimensions and projection maps. This clears dynamic
horizon caches, static horizon caches (`_cache_Us`, `_cache_Fs`),
and the projection cache `_Ps_cache`.



```text
_clear_dynamic_caches() → None
```



Clear caches tied to dynamic (non-structural) algorithm parameters.

These horizon-indexed caches are rebuilt when attributes such as step
sizes change: `_cache_AsBsCsDs`, `_cache_Ys`, and `_cache_Xs`.



```text
_compute_E( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute the lifted matrix \(E\) for one component and a list of pairs.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e) from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/),
using [(5.8)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-p-mats) together with [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats)/ [(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star)
and [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats)/ [(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star) for non-star/star pairs,
respectively.
The resulting \(E\) is used in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq),
[(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op) for the func-ineq, func-eq,
and op cases, respectively, and therefore in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

The pair ordering in `pairs` is preserved in the row stacking.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The computed E matrix.

**Raises**

- ValueError: if the pairs are malformed or the iteration bounds are invalid.



```text
_compute_F_aggregated( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, a: ndarray, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Compute one aggregated lifted vector \(F\).

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq) and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq)
for the func-ineq and func-eq cases, respectively,
from [5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
by stacking selected rows from [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs")
and weighting them with `a`.

The resulting vectors are the linear terms used in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\) (must satisfy \(i \in \IndexFunc\)).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- a ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): weight vector \(a \in \mathbb{R}^{\text{number of pairs}}\).
  This comes from the functional interpolation data
  (e.g., \(a_{(i,o)}^{\textup{func-ineq}}\) or \(a_{(i,o)}^{\textup{func-eq}}\))
  for `func-ineq` and `func-eq` constraints, respectively. See the
  functional interpolation-condition
  [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition.get_data "autolyap.problemclass.base._FunctionInterpolationCondition.get_data")
  contract or
  [`_get_component_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data "autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data").
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The aggregated F vector as a column vector.

**Raises**

- ValueError: if any input conditions are not met.



```text
_compute_W( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, M: ndarray, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Compute one lifted quadratic constraint matrix \(W\).

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements the matrix definitions
[(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op) for the
func-ineq, func-eq, and op cases, respectively, from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
using \(W = E^\top M E\).

The resulting matrices are the building blocks in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- M ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): symmetric matrix \(M\) with shape
  \([2 \cdot (\text{number of pairs}) \times 2 \cdot (\text{number of pairs})]\).
  This comes from the interpolation data of the function/operator class
  (e.g., \(M_{(i,o)}^{\textup{func-ineq}}\), \(M_{(i,o)}^{\textup{func-eq}}\), or
  \(M_{(i,o)}^{\textup{op}}\)) for `func-ineq`, `func-eq`, and
  `op` constraints, respectively. See the
  interpolation-condition
  [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition.get_data "autolyap.problemclass.base._InterpolationCondition.get_data")
  contract or
  [`_get_component_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data "autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data").
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The computed W matrix.

**Raises**

- ValueError: if any input conditions are not met.



```text
_generate_F( i: int, j: int | None = None, k: int | None = None, star: bool = False, k_min: int = 0, k_max: int = 0, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Generate one lifted function-value selector row.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements [(5.9)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-mats) from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
Its outputs are combined into [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq) and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq).
These appear in both
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): functional component index \(i\) (must satisfy \(i \in \IndexFunc\)).
- j ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")]): evaluation index \(j\) for the non-star case.
- k ([`Optional`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Optional "typing.Optional")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")]): iteration index \(k\) for the non-star case (must satisfy
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\)).
- star ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): if True, generate the star row.
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A \(1 \times \text{total_dim}\) numpy array representing the generated F row.

**Raises**

- ValueError: if the indices are invalid or inconsistent with the star/non-star case.



```text
_generate_U( k_min: int, k_max: int, k: int | None = None, star: bool = False, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Generate one lifted input matrix.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for the state-space model [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity).

This method implements [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats) and [(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star) for the
non-star and star cases, respectively, from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
These matrices are then used in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/)
respectively, through [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E").

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): current iteration index \(k\) (required when star is False).
- star ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): if True, generate \(U_{\star}\).

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The generated matrix
  \(U_k^{k_{\textup{min}},k_{\textup{max}}}\) or
  \(U_{\star}^{k_{\textup{min}},k_{\textup{max}}}\) for
  `star=False` and `star=True`, respectively, each with


\[n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\]

  columns.

**Raises**

- ValueError: if k is missing when star is False or if
  \(k \notin \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).



```text
_generate_X_k( sys_mats: Dict[int, Tuple[ndarray, ndarray, ndarray, ndarray]], k: int, k_min: int, k_max: int, ) → ndarray
```



[Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Generate one lifted state matrix.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity) and [(3.2)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-abcd).

This method implements [(5.3)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-x-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
for


\[k \in \llbracket k_{\textup{min}}, k_{\textup{max}} + 1\rrbracket.\]

The resulting matrices are used in
[(5.39)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c1-mat),
[(5.43)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c4-mat), and
[(5.58)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta1-mat), i.e., in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- sys\_mats ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]): dictionary
  mapping each iteration index \(k\) to \((A_k, B_k, C_k, D_k)\).
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): current iteration index \(k\).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The generated matrix
  \(X_k^{k_{\textup{min}},k_{\textup{max}}}\).

**Raises**

- ValueError: if \(k \notin \llbracket k_{\textup{min}}, k_{\textup{max}}+1\rrbracket\).



```text
_generate_Y( sys_mats: Dict[int, Tuple[ndarray, ndarray, ndarray, ndarray]], k_min: int, k_max: int, k: int | None = None, star: bool = False, ) → ndarray
```



[Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Generate one lifted output matrix.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for [(3.1)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-linear-system-with-nonlinearity) and [(3.2)](https://autolyap.github.io/theory/algorithm_representation/#equation-eq-abcd).

This method implements [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats) and [(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star) for the
non-star and star cases, respectively, from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
These matrices are then used in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/)
respectively, through [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E").

**Parameters**

- sys\_mats ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]): dictionary
  mapping each iteration index \(k\) to \((A_k, B_k, C_k, D_k)\).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): current iteration index \(k\) (required when star is False).
- star ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): if True, generate \(Y_{\star}\).

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The generated matrix
  \(Y_k^{k_{\textup{min}},k_{\textup{max}}}\) or
  \(Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}\) for
  `star=False` and `star=True`, respectively, each with


\[n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\]

  columns.

**Raises**

- ValueError: if k is missing when star is False.



```text
_get_AsBsCsDs( k_min: int, k_max: int, ) → Dict[int, Tuple[ndarray, ndarray, ndarray, ndarray]]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary mapping each iteration index \(k\) to
\((A_k, B_k, C_k, D_k)\) for \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
In compact form:


\[\{(A_k, B_k, C_k, D_k)\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}.\]

Each entry is the tuple returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

These system matrices are used to build output and state matrices via
[`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys") and [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs").

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]): A dictionary whose keys
  are iteration indices and values are tuples \((A_k, B_k, C_k, D_k)\), where


\[A_k \in \mathbb{R}^{n \times n}, \quad
B_k \in \mathbb{R}^{n \times \bar{m}}, \quad
C_k \in \mathbb{R}^{\bar{m} \times n}, \quad
D_k \in \mathbb{R}^{\bar{m} \times \bar{m}}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).



```text
_get_Fs( k_min: int, k_max: int, ) → Dict[Tuple[int, int | str, int | str], ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of F matrices for all functional components.
In compact form,


\[\{F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}}\}_{i\in\IndexFunc,\, j\in\llbracket 1,\bar{m}_i\rrbracket,\, k\in\llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket}
\cup \{F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}\}_{i\in\IndexFunc}.\]

The dictionary keys are defined as follows. For non-star F matrices, keys are of the form
\((i, j, k)\) with \(i \in \IndexFunc\), \(j \in \llbracket 1,\bar{m}_i\rrbracket\), and
\(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\). For star F matrices, keys are of the form
\((i,\star,\star)\) for each \(i \in \IndexFunc\).

The matrix definition is [(5.9)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
These F matrices are used by [`_compute_F_aggregated()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_F_aggregated "autolyap.algorithms.algorithm.Algorithm._compute_F_aggregated") and
enter the linear terms [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq) and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq) that appear in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")], [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary mapping keys to the corresponding
  F row matrices \(F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}\), where


\[F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}},
F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{1 \times d}, \quad
d = (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m}_{\text{func}} + m_{\text{func}}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).



```text
_get_Ps() → Dict[Tuple[int, int | str], ndarray]
```



[Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of projection matrices \(P\).
In compact form,


\[\{P_{(i,j)}\}_{j \in \llbracket 1,\NumEval_i\rrbracket,i \in \llbracket 1,m\rrbracket}
\cup \{P_{(i,\star)}\}_{i \in \llbracket 1,m\rrbracket}.\]

The matrix definitions are given by [(5.8)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-p-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).

These projection matrices are used by [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E")
through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e), and therefore in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq),
[(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op), which feed
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")], [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are tuples
  \((i, j)\) with \(i \in \llbracket 1,m\rrbracket\),
  \(j \in \llbracket 1,\NumEval_i\rrbracket\), and \((i,\star)\), with values
  \(P_{(i,j)}\) and \(P_{(i,\star)}\), where


\[P_{(i,j)} \in \mathbb{R}^{1 \times \NumEval}, \quad
P_{(i,\star)} \in \mathbb{R}^{1 \times m}.\]



```text
_get_Us( k_min: int, k_max: int, ) → Dict[int | str, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of U matrices for iterations \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\),
including the star matrix.
In compact form,


\[\{U_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}
\cup \{U_{\star}^{k_{\textup{min}},k_{\textup{max}}}\}.\]

The matrix definitions are [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats) and [(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
They are used in [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E") (and therefore
[`_compute_W()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_W "autolyap.algorithms.algorithm.Algorithm._compute_W")) through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e),
and then in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op),
i.e., in lifted interpolation constraints that enter
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are iteration indices
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\) and ‘star’, with
  values \(U_k^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(U_{\star}^{k_{\textup{min}},k_{\textup{max}}}\), respectively, where


\[U_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{\bar{m} \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}, \quad
U_{\star}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{m \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).



```text
_get_Xs( k_min: int, k_max: int, ) → Dict[int, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary mapping each iteration index \(k\)
(for \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}} + 1\rrbracket\)) to the corresponding
\(X_k\) matrix.
In compact form,


\[\{X_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}+1}.\]

The matrix definition is [(5.3)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-x-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).

The \(X_k\) matrices are constructed from system matrices returned by
[`_get_AsBsCsDs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs "autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs") (and therefore [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD")).

In the convergence analyses, these matrices enter
[(5.39)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c1-mat),
[(5.43)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c4-mat), and
[(5.58)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta1-mat) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary mapping
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}+1\rrbracket\) to
  \(X_k^{k_{\textup{min}},k_{\textup{max}}}\), where


\[X_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{n \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).



```text
_get_Ys( k_min: int, k_max: int, ) → Dict[int | str, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of Y matrices for iterations \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\),
including \(Y_{\star}\).
In compact form,


\[\{Y_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}
\cup \{Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}\}.\]

The matrix definitions are [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats) and [(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
The \(Y\) matrices are constructed from system matrices returned by
[`_get_AsBsCsDs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs "autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs") (which calls
[`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD")) and are used by
[`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E") through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e) for
lifted interpolation constraints in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are iteration indices
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\) and ‘star’, with
  values \(Y_k^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}\), respectively, where


\[Y_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{\bar{m} \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}, \quad
Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{m \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).



```text
static _is_star_pair( j: object, k: object, ) → bool
```



[bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Return True when a pair corresponds to the star entry.



```text
_readonly_tuple( mats: Tuple[ndarray, ndarray, ndarray, ndarray], ) → Tuple[ndarray, ndarray, ndarray, ndarray]
```



[Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a tuple of read-only views for (A, B, C, D) matrices.



```text
static _readonly_view( array: ndarray, ) → ndarray
```



[ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a read-only view to discourage accidental mutation of cached matrices.

**Parameters**

- array ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): Input array to view.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): A read-only view of the input array.



```text
_set_dynamic_parameter( name: str, value: object, ) → None
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Update a dynamic scalar parameter only when its value changes.

This avoids unnecessary cache invalidation for repeated assignments
of the same parameter value.



```text
static _validate_finite_real( value: Any, name: str, ) → float
```



[Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Validate a finite real scalar parameter.



```text
static _validate_k_bounds( k_min: int, k_max: int, ) → Tuple[int, int]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Validate and normalize iteration bounds.



```text
static _validate_nonnegative_integral( value: Any, name: str, ) → int
```



[Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Validate a nonnegative integer parameter.



```text
static _validate_nonstar_pair( j: object, k: object, m_bar_i: int, k_min: int, k_max: int, i: int, ) → Tuple[int, int]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple)

Validate and normalize a non-star pair (j, k).



```text
static _validate_pairs_container( pairs: List[Tuple[int, int] | Tuple[str, str]], ) → None
```



[List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Validate that pairs is a non-empty list/tuple of 2-tuples.



```text
classmethod _validate_positive_finite_real( value: object, name: str, ) → float
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Validate a strictly positive finite real scalar parameter.



```text
abstractmethod get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/) for notation and representation
conventions.

Return the system matrices \((A_k, B_k, C_k, D_k)\) at iteration \(k\).

**Parameters**

- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): iteration index \(k\).

**Returns**

- ([`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A tuple
  \((A_k, B_k, C_k, D_k)\) of numpy arrays with


\[A_k \in \mathbb{R}^{n \times n}, \quad
B_k \in \mathbb{R}^{n \times \bar{m}}, \quad
C_k \in \mathbb{R}^{\bar{m} \times n}, \quad
D_k \in \mathbb{R}^{\bar{m} \times \bar{m}}.\]

<a id="private-matrix-projection-accessors"></a>



## Private matrix/projection accessors

<a id="autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs"></a>





```text
Algorithm._get_AsBsCsDs( k_min: int, k_max: int, ) → Dict[int, Tuple[ndarray, ndarray, ndarray, ndarray]]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary mapping each iteration index \(k\) to
\((A_k, B_k, C_k, D_k)\) for \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
In compact form:


\[\{(A_k, B_k, C_k, D_k)\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}.\]

Each entry is the tuple returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

These system matrices are used to build output and state matrices via
[`_get_Ys()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Ys "autolyap.algorithms.algorithm.Algorithm._get_Ys") and [`_get_Xs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Xs "autolyap.algorithms.algorithm.Algorithm._get_Xs").

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]]): A dictionary whose keys
  are iteration indices and values are tuples \((A_k, B_k, C_k, D_k)\), where


\[A_k \in \mathbb{R}^{n \times n}, \quad
B_k \in \mathbb{R}^{n \times \bar{m}}, \quad
C_k \in \mathbb{R}^{\bar{m} \times n}, \quad
D_k \in \mathbb{R}^{\bar{m} \times \bar{m}}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).

<a id="autolyap.algorithms.algorithm.Algorithm._get_Us"></a>





```text
Algorithm._get_Us( k_min: int, k_max: int, ) → Dict[int | str, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of U matrices for iterations \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\),
including the star matrix.
In compact form,


\[\{U_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}
\cup \{U_{\star}^{k_{\textup{min}},k_{\textup{max}}}\}.\]

The matrix definitions are [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats) and [(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
They are used in [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E") (and therefore
[`_compute_W()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_W "autolyap.algorithms.algorithm.Algorithm._compute_W")) through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e),
and then in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op),
i.e., in lifted interpolation constraints that enter
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are iteration indices
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\) and ‘star’, with
  values \(U_k^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(U_{\star}^{k_{\textup{min}},k_{\textup{max}}}\), respectively, where


\[U_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{\bar{m} \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}, \quad
U_{\star}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{m \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).

<a id="autolyap.algorithms.algorithm.Algorithm._get_Ys"></a>





```text
Algorithm._get_Ys( k_min: int, k_max: int, ) → Dict[int | str, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of Y matrices for iterations \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\),
including \(Y_{\star}\).
In compact form,


\[\{Y_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}}
\cup \{Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}\}.\]

The matrix definitions are [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats) and [(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
The \(Y\) matrices are constructed from system matrices returned by
[`_get_AsBsCsDs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs "autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs") (which calls
[`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD")) and are used by
[`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E") through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e) for
lifted interpolation constraints in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are iteration indices
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\) and ‘star’, with
  values \(Y_k^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}\), respectively, where


\[Y_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{\bar{m} \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}, \quad
Y_{\star}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{m \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).

<a id="autolyap.algorithms.algorithm.Algorithm._get_Xs"></a>





```text
Algorithm._get_Xs( k_min: int, k_max: int, ) → Dict[int, ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary mapping each iteration index \(k\)
(for \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}} + 1\rrbracket\)) to the corresponding
\(X_k\) matrix.
In compact form,


\[\{X_k^{k_{\textup{min}},k_{\textup{max}}}\}_{k=k_{\textup{min}}}^{k_{\textup{max}}+1}.\]

The matrix definition is [(5.3)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-x-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).

The \(X_k\) matrices are constructed from system matrices returned by
[`_get_AsBsCsDs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs "autolyap.algorithms.algorithm.Algorithm._get_AsBsCsDs") (and therefore [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD")).

In the convergence analyses, these matrices enter
[(5.39)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c1-mat),
[(5.43)](https://autolyap.github.io/theory/iteration_independent_analyses/#equation-eq-iteration-independent-lyapunov-theta1-c4-mat), and
[(5.58)](https://autolyap.github.io/theory/iteration_dependent_analyses/#equation-eq-iteration-dependent-lyapunov-theta1-mat) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary mapping
  \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}+1\rrbracket\) to
  \(X_k^{k_{\textup{min}},k_{\textup{max}}}\), where


\[X_k^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{n \times \left(n + (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m} + m\right)}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).

<a id="autolyap.algorithms.algorithm.Algorithm._get_Ps"></a>





```text
Algorithm._get_Ps() → Dict[Tuple[int, int | str], ndarray]
```



[Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of projection matrices \(P\).
In compact form,


\[\{P_{(i,j)}\}_{j \in \llbracket 1,\NumEval_i\rrbracket,i \in \llbracket 1,m\rrbracket}
\cup \{P_{(i,\star)}\}_{i \in \llbracket 1,m\rrbracket}.\]

The matrix definitions are given by [(5.8)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-p-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).

These projection matrices are used by [`_compute_E()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_E "autolyap.algorithms.algorithm.Algorithm._compute_E")
through [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e), and therefore in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq),
[(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op), which feed
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) in
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")], [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary whose keys are tuples
  \((i, j)\) with \(i \in \llbracket 1,m\rrbracket\),
  \(j \in \llbracket 1,\NumEval_i\rrbracket\), and \((i,\star)\), with values
  \(P_{(i,j)}\) and \(P_{(i,\star)}\), where


\[P_{(i,j)} \in \mathbb{R}^{1 \times \NumEval}, \quad
P_{(i,\star)} \in \mathbb{R}^{1 \times m}.\]

<a id="autolyap.algorithms.algorithm.Algorithm._get_Fs"></a>





```text
Algorithm._get_Fs( k_min: int, k_max: int, ) → Dict[Tuple[int, int | str, int | str], ndarray]
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [Dict](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Return a dictionary of F matrices for all functional components.
In compact form,


\[\{F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}}\}_{i\in\IndexFunc,\, j\in\llbracket 1,\bar{m}_i\rrbracket,\, k\in\llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket}
\cup \{F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}\}_{i\in\IndexFunc}.\]

The dictionary keys are defined as follows. For non-star F matrices, keys are of the form
\((i, j, k)\) with \(i \in \IndexFunc\), \(j \in \llbracket 1,\bar{m}_i\rrbracket\), and
\(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\). For star F matrices, keys are of the form
\((i,\star,\star)\) for each \(i \in \IndexFunc\).

The matrix definition is [(5.9)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-mats) in
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/).
These F matrices are used by [`_compute_F_aggregated()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._compute_F_aggregated "autolyap.algorithms.algorithm.Algorithm._compute_F_aggregated") and
enter the linear terms [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq) and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq) that appear in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).

**Returns**

- ([`Dict`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Dict "typing.Dict")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any"), [`Any`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any "typing.Any")], [`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")]): A dictionary mapping keys to the corresponding
  F row matrices \(F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}}\) and
  \(F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}\), where


\[F_{(i,j,k)}^{k_{\textup{min}},k_{\textup{max}}},
F_{(i,\star,\star)}^{k_{\textup{min}},k_{\textup{max}}}
\in \mathbb{R}^{1 \times d}, \quad
d = (k_{\textup{max}} - k_{\textup{min}} + 1)\bar{m}_{\text{func}} + m_{\text{func}}.\]

**Raises**

- ValueError: if \(k_{\textup{min}} < 0\) or \(k_{\textup{max}} < k_{\textup{min}}\).

<a id="autolyap.algorithms.algorithm.Algorithm._compute_E"></a>





```text
Algorithm._compute_E( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray)

Compute the lifted matrix \(E\) for one component and a list of pairs.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements [(5.17)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-e) from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/),
using [(5.8)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-p-mats) together with [(5.4)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-mats)/ [(5.5)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-y-star)
and [(5.6)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-mats)/ [(5.7)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-u-star) for non-star/star pairs,
respectively.
The resulting \(E\) is used in [(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq),
[(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op) for the func-ineq, func-eq,
and op cases, respectively, and therefore in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

The pair ordering in `pairs` is preserved in the row stacking.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The computed E matrix.

**Raises**

- ValueError: if the pairs are malformed or the iteration bounds are invalid.

<a id="autolyap.algorithms.algorithm.Algorithm._compute_W"></a>





```text
Algorithm._compute_W( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, M: ndarray, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Compute one lifted quadratic constraint matrix \(W\).

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements the matrix definitions
[(5.12)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-ineq), [(5.13)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-func-eq), and [(5.14)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-w-op) for the
func-ineq, func-eq, and op cases, respectively, from
[5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
using \(W = E^\top M E\).

The resulting matrices are the building blocks in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- M ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): symmetric matrix \(M\) with shape
  \([2 \cdot (\text{number of pairs}) \times 2 \cdot (\text{number of pairs})]\).
  This comes from the interpolation data of the function/operator class
  (e.g., \(M_{(i,o)}^{\textup{func-ineq}}\), \(M_{(i,o)}^{\textup{func-eq}}\), or
  \(M_{(i,o)}^{\textup{op}}\)) for `func-ineq`, `func-eq`, and
  `op` constraints, respectively. See the
  interpolation-condition
  [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition.get_data "autolyap.problemclass.base._InterpolationCondition.get_data")
  contract or
  [`_get_component_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data "autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data").
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The computed W matrix.

**Raises**

- ValueError: if any input conditions are not met.

<a id="autolyap.algorithms.algorithm.Algorithm._compute_F_aggregated"></a>





```text
Algorithm._compute_F_aggregated( i: int, pairs: List[Tuple[int, int] | Tuple[str, str]], k_min: int, k_max: int, a: ndarray, validate: bool = True, ) → ndarray
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int), [List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [Tuple](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple), [str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [ndarray](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Compute one aggregated lifted vector \(F\).

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for notation.

This method implements [(5.15)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-ineq) and [(5.16)](https://autolyap.github.io/theory/performance_estimation_via_sdps/#equation-eq-f-func-eq)
for the func-ineq and func-eq cases, respectively,
from [5.1. Performance estimation via SDPs](https://autolyap.github.io/theory/performance_estimation_via_sdps/)
by stacking selected rows from [`_get_Fs()`](https://autolyap.github.io/dev/dev_internal_algorithms/#autolyap.algorithms.algorithm.Algorithm._get_Fs "autolyap.algorithms.algorithm.Algorithm._get_Fs")
and weighting them with `a`.

The resulting vectors are the linear terms used in
[(5.28)](https://autolyap.github.io/theory/iteration_independent_analyses/#eq-iteration-independent-lyapunov-condition) and
[(5.51)](https://autolyap.github.io/theory/iteration_dependent_analyses/#eq-iteration-dependent-lyapunov-condition) from
[5.2. Iteration-independent analyses](https://autolyap.github.io/theory/iteration_independent_analyses/)
and [5.3. Iteration-dependent analyses](https://autolyap.github.io/theory/iteration_dependent_analyses/),
respectively.

**Parameters**

- i ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): component index \(i\) (must satisfy \(i \in \IndexFunc\)).
- pairs ([`List`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List "typing.List")[[`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), [`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")], [`Tuple`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Tuple "typing.Tuple")[[`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str"), [`str`](https://autolyap.github.io/dev/dev_external_reference_targets/#str "str")]]]): list of \((j, k)\) pairs. For non-star pairs,
  \(j \in \llbracket 1,\bar{m}_i\rrbracket\) and \(k \in \llbracket k_{\textup{min}}, k_{\textup{max}}\rrbracket\).
  For the star case, use (‘star’, ‘star’).
- k\_min ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): minimum iteration index \(k_{\textup{min}}\).
- k\_max ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): maximum iteration index \(k_{\textup{max}}\).
- a ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): weight vector \(a \in \mathbb{R}^{\text{number of pairs}}\).
  This comes from the functional interpolation data
  (e.g., \(a_{(i,o)}^{\textup{func-ineq}}\) or \(a_{(i,o)}^{\textup{func-eq}}\))
  for `func-ineq` and `func-eq` constraints, respectively. See the
  functional interpolation-condition
  [`get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition.get_data "autolyap.problemclass.base._FunctionInterpolationCondition.get_data")
  contract or
  [`_get_component_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data "autolyap.problemclass.inclusion_problem.InclusionProblem._get_component_data").
- validate ([`bool`](https://autolyap.github.io/dev/dev_external_reference_targets/#bool "bool")): Whether to validate inputs.

**Returns**

- ([`numpy.ndarray`](https://autolyap.github.io/dev/dev_external_reference_targets/#numpy.ndarray "numpy.ndarray")): The aggregated F vector as a column vector.

**Raises**

- ValueError: if any input conditions are not met.

<a id="module-map"></a>



## Module map

| Class | Implementation module |
| --- | --- |
| [`AcceleratedProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.AcceleratedProximalPoint "autolyap.algorithms.accelerated_proximal_point.AcceleratedProximalPoint") | `autolyap.algorithms.accelerated_proximal_point` |
| [`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.chambolle_pock.ChambollePock") | `autolyap.algorithms.chambolle_pock` |
| [`DavisYin`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DavisYin "autolyap.algorithms.davis_yin.DavisYin") | `autolyap.algorithms.davis_yin` |
| [`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.douglas_rachford.DouglasRachford") | `autolyap.algorithms.douglas_rachford` |
| [`Extragradient`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.Extragradient "autolyap.algorithms.extragradient.Extragradient") | `autolyap.algorithms.extragradient` |
| [`ForwardMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ForwardMethod "autolyap.algorithms.forward.ForwardMethod") | `autolyap.algorithms.forward` |
| [`GradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientMethod "autolyap.algorithms.gradient.GradientMethod") | `autolyap.algorithms.gradient` |
| [`GradientNesterovMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientNesterovMomentum "autolyap.algorithms.gradient_with_Nesterov_like_momentum.GradientNesterovMomentum") | `autolyap.algorithms.gradient_with_Nesterov_like_momentum` |
| [`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.heavy_ball.HeavyBallMethod") | `autolyap.algorithms.heavy_ball` |
| [`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.information_theoretic_exact_method.ITEM") | `autolyap.algorithms.information_theoretic_exact_method` |
| [`MalitskyTamFRB`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.MalitskyTamFRB "autolyap.algorithms.malitsky_tam_frb.MalitskyTamFRB") | `autolyap.algorithms.malitsky_tam_frb` |
| [`NesterovConstant`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovConstant "autolyap.algorithms.nesterov_constant.NesterovConstant") | `autolyap.algorithms.nesterov_constant` |
| [`NesterovFastGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovFastGradientMethod "autolyap.algorithms.nesterov_fast_gradient_method.NesterovFastGradientMethod") | `autolyap.algorithms.nesterov_fast_gradient_method` |
| [`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.optimized_gradient_method.OptimizedGradientMethod") | `autolyap.algorithms.optimized_gradient_method` |
| [`ProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ProximalPoint "autolyap.algorithms.proximal_point.ProximalPoint") | `autolyap.algorithms.proximal_point` |
| [`TripleMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TripleMomentum "autolyap.algorithms.triple_momentum.TripleMomentum") | `autolyap.algorithms.triple_momentum` |
| [`TsengFBF`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TsengFBF "autolyap.algorithms.tseng_fbf.TsengFBF") | `autolyap.algorithms.tseng_fbf` |

TeX macro definitions used in this page:

```latex
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\NumEval}{\bar{m}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
