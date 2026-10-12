Canonical HTML: https://autolyap.github.io/problem_class/
Documentation version: 0.2.1

<a id="problem-classes"></a>



# Problem classes

<a id="autolyap.problemclass.InclusionProblem"></a>





```text
class autolyap.problemclass.InclusionProblem( components: List[_InterpolationCondition | List[_InterpolationCondition]], )
```



[List](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.List), [_InterpolationCondition](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition)

Bases: `object`

Representation of an inclusion problem defined by interpolation conditions.

See [2. Problem classes](https://autolyap.github.io/theory/problem_class/) for notation and
component conventions.

**Parameters**

- components (`list`): A list of components indexed by
  \(\llbracket 1, m\rrbracket\), where \(m = \text{len(components)}\).
  Entry components[i-1] defines component \(i\). Each entry is either
  one interpolation-condition instance or a list of such instances.
  All conditions for a given component must be of the same type (all
  operator conditions or all function conditions). Condition classes are
  documented in [Function classes](https://autolyap.github.io/function_classes/) and [Operator classes](https://autolyap.github.io/operator_classes/).

**Raises**

- ValueError: If components is empty or if any entry contains invalid conditions.

**Note**

- For [`GradientDominated`](https://autolyap.github.io/function_classes/#autolyap.problemclass.GradientDominated "autolyap.problemclass.GradientDominated"), the problem must have exactly one
  component, i.e., \(m = 1\); in the notation of
  [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/),
  \(m_{\textup{func}} = 1\) and \(m_{\textup{op}} = 0\).
  This single component may still contain a list of function
  conditions (an intersection).
- For [`WeakMintyVariationalInequality`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.WeakMintyVariationalInequality "autolyap.problemclass.WeakMintyVariationalInequality"), the problem must have
  exactly one component, i.e., \(m = 1\); in the same notation,
  \(m_{\textup{op}} = 1\) and \(m_{\textup{func}} = 0\).
  This single component may still contain a list of operator
  conditions (an intersection).

- [Function classes](https://autolyap.github.io/function_classes/)
- [Operator classes](https://autolyap.github.io/operator_classes/)

TeX macro definitions used in this page:

```latex
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
