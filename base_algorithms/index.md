Canonical HTML: https://autolyap.github.io/base_algorithms/
Documentation version: 0.2.1

<a id="base-algorithm"></a>



# Base algorithm

<a id="autolyap.algorithms.Algorithm"></a>





```text
class autolyap.algorithms.Algorithm( n: int, m: int, m_bar_is: List[int], I_func: List[int], I_op: List[int], )
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

<a id="autolyap.algorithms.Algorithm.__init__"></a>





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

<a id="autolyap.algorithms.Algorithm.get_ABCD"></a>





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

TeX macro definitions used in this page:

```latex
\newcommand{\IndexFunc}{\mathcal{I}_{\textup{func}}}
\newcommand{\IndexOp}{\mathcal{I}_{\textup{op}}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
