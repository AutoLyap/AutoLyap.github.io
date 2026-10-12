Canonical HTML: https://autolyap.github.io/operator_classes/
Documentation version: 0.2.1

<a id="operator-classes"></a>



# Operator classes

- [`MaximallyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.MaximallyMonotone "autolyap.problemclass.MaximallyMonotone")
- [`StronglyMonotone`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.StronglyMonotone "autolyap.problemclass.StronglyMonotone")
- [`LipschitzOperator`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.LipschitzOperator "autolyap.problemclass.LipschitzOperator")
- [`Cocoercive`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.Cocoercive "autolyap.problemclass.Cocoercive")
- [`WeakMintyVariationalInequality`](https://autolyap.github.io/operator_classes/#autolyap.problemclass.WeakMintyVariationalInequality "autolyap.problemclass.WeakMintyVariationalInequality")

<a id="autolyap.problemclass.MaximallyMonotone"></a>





```text
class autolyap.problemclass.MaximallyMonotone
```



Bases: [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition")

Operator interpolation condition for maximally monotone operators.

Let \(G: \calH \rightrightarrows \calH\) be maximally monotone.

- Interpolation inequality:

  For any \((x_{r_1},u_{r_1}),(x_{r_2},u_{r_2}) \in \operatorname{gra} G\),


\[\langle u_{r_1} - u_{r_2}, x_{r_1} - x_{r_2} \rangle \ge 0.\]

- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With an interpolation vector \(z\) built from the stacked
  variables in the inequality above, the same condition is encoded as


\[\mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}M = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & -1 & 1 \\
    0 & 0 & 1 & -1 \\
    -1 & 1 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

This condition has no parameters.

**References**

- <a id="id1"></a>[[BC17](https://autolyap.github.io/operator_classes/#id21 "Heinz H. Bauschke and Patrick L. Combettes. Convex Analysis and Monotone Operator Theory in Hilbert Spaces. Springer International Publishing, 2017. ISBN 9783319483115. doi:10.1007/978-3-319-48311-5."), Theorem 20.21].

<a id="autolyap.problemclass.StronglyMonotone"></a>





```text
class autolyap.problemclass.StronglyMonotone( mu: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition")

Operator interpolation condition for strongly and maximally monotone operators.

Let \(\mu \in \mathbb{R}_{++}\) and
\(G: \calH \rightrightarrows \calH\) be \(\mu\)-strongly monotone.

- Interpolation inequality:

  For any \((x_{r_1},u_{r_1}),(x_{r_2},u_{r_2}) \in \operatorname{gra} G\),


\[\langle u_{r_1} - u_{r_2}, x_{r_1} - x_{r_2} \rangle \ge \mu \|x_{r_1} - x_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With an interpolation vector \(z\) built from the stacked
  variables in the inequality above, the same condition is encoded as


\[\mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}M = \frac{1}{2}
\begin{bmatrix}
    2\mu & -2\mu & -1 & 1 \\
    -2\mu & 2\mu & 1 & -1 \\
    -1 & 1 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The strong monotonicity parameter corresponding
  to \(\mu\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If mu is not a number, \(\le 0\), or infinite.

**References**

- <a id="id1"></a>[[RTBG20](https://autolyap.github.io/operator_classes/#id174 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Proposition 1].

<a id="autolyap.problemclass.LipschitzOperator"></a>





```text
class autolyap.problemclass.LipschitzOperator( L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition")

Operator interpolation condition for Lipschitz operators.

Let \(L \in \mathbb{R}_{++}\) and
\(G: \calH \to \calH\) be \(L\)-Lipschitz continuous.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(u_{r_1} = G(x_{r_1})\) and \(u_{r_2} = G(x_{r_2})\),


\[\|u_{r_1} - u_{r_2}\|^2 \le L^2 \|x_{r_1} - x_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With an interpolation vector \(z\) built from the stacked
  variables in the inequality above, the same condition is encoded as


\[\mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}M =
\begin{bmatrix}
    -L^2 & L^2 & 0 & 0 \\
    L^2 & -L^2 & 0 & 0 \\
    0 & 0 & 1 & -1 \\
    0 & 0 & -1 & 1
\end{bmatrix}.\end{split}\]

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The Lipschitz parameter corresponding to
  \(L\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If L is not a number, \(\le 0\), or infinite.

**References**

- Kirszbraun–Valentine theorem:
  <a id="id1"></a>[[Kir34](https://autolyap.github.io/operator_classes/#id110 "M. Kirszbraun. Über die zusammenziehende und Lipschitzsche transformationen. Fundamenta Mathematicae, 22(1):77–108, 1934.")],
  <a id="id2"></a>[[Val43](https://autolyap.github.io/operator_classes/#id205 "F. A. Valentine. On the extension of a vector function so as to preserve a Lipschitz condition. Bulletin of the American Mathematical Society, 49(2):100–108, 1943.")],
  <a id="id3"></a>[[Val45](https://autolyap.github.io/operator_classes/#id206 "F. A. Valentine. A Lipschitz condition preserving extension for a vector function. American Journal of Mathematics, 67(1):83–93, January 1945. doi:10.2307/2371917.")].

<a id="autolyap.problemclass.Cocoercive"></a>





```text
class autolyap.problemclass.Cocoercive( beta: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition")

Operator interpolation condition for cocoercive operators.

Let \(\beta \in \mathbb{R}_{++}\) and
\(G: \calH \to \calH\) be \(\beta\)-cocoercive.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(u_{r_1} = G(x_{r_1})\) and \(u_{r_2} = G(x_{r_2})\),


\[\langle u_{r_1} - u_{r_2}, x_{r_1} - x_{r_2} \rangle \ge \beta \|u_{r_1} - u_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With an interpolation vector \(z\) built from the stacked
  variables in the inequality above, the same condition is encoded as


\[\mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}M = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & -1 & 1 \\
    0 & 0 & 1 & -1 \\
    -1 & 1 & 2\beta & -2\beta \\
    1 & -1 & -2\beta & 2\beta
\end{bmatrix}.\end{split}\]

**Parameters**

- beta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The cocoercivity parameter corresponding
  to \(\beta\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If beta is not a number, \(\le 0\), or infinite.

**References**

- <a id="id1"></a>[[RTBG20](https://autolyap.github.io/operator_classes/#id174 "Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. SIAM Journal on Optimization, 30(3):2251–2271, January 2020. doi:10.1137/19m1304854."), Proposition 2].

<a id="autolyap.problemclass.WeakMintyVariationalInequality"></a>





```text
class autolyap.problemclass.WeakMintyVariationalInequality( rho_minty: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_OperatorInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._OperatorInterpolationCondition "autolyap.problemclass.base._OperatorInterpolationCondition")

Operator interpolation condition for operators that fulfill the weak Minty variational inequality.

Let \(\rho_{\textup{minty}} \in \mathbb{R}\) and
\(G: \calH \rightrightarrows \calH\) satisfy a weak Minty variational inequality.

- Interpolation inequality:

  There exists \(x_\star \in \calH\) with \(0 \in G(x_\star)\) such that for any
  \((x_{r_1},u_{r_1}) \in \operatorname{gra} G\),


\[\langle u_{r_1}, x_{r_1} - x_\star \rangle \ge \rho_{\textup{minty}} \|u_{r_1}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With an interpolation vector \(z\) built from the stacked
  variables in the inequality above, the same condition is encoded as


\[\mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}M = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & -1 & 0 \\
    0 & 0 & 1 & 0 \\
    -1 & 1 & 2\rho_{\textup{minty}} & 0 \\
    0 & 0 & 0 & 0
\end{bmatrix}.\end{split}\]

**Parameters**

- rho\_minty ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The weak MVI parameter corresponding to
  \(\rho_{\textup{minty}}\) (must be finite).

**Raises**

- ValueError: If rho\_minty is not a number or not finite.

**Note**

- When used inside [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"),
  the problem must have exactly one component. In the notation of
  [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/),
  \(m = 1\), \(m_{\textup{op}} = 1\), and
  \(m_{\textup{func}} = 0\). This single component may still
  contain a list of operator conditions (an intersection).

<a id="references"></a>



## References

<a id="id1"></a>



<a id="id21"></a>



[[BC17](https://autolyap.github.io/operator_classes/#id1)]

Heinz H. Bauschke and Patrick L. Combettes. *Convex Analysis and Monotone Operator Theory in Hilbert Spaces*. Springer International Publishing, 2017. ISBN 9783319483115. [doi:10.1007/978-3-319-48311-5](https://doi.org/10.1007/978-3-319-48311-5).

<a id="id110"></a>



[[Kir34](https://autolyap.github.io/operator_classes/#id1)]

M. Kirszbraun. Über die zusammenziehende und Lipschitzsche transformationen. *Fundamenta Mathematicae*, 22(1):77–108, 1934.

<a id="id174"></a>



[RTBG20]
([1](https://autolyap.github.io/operator_classes/#id1),[2](https://autolyap.github.io/operator_classes/#id1))

Ernest K. Ryu, Adrien B. Taylor, Carolina Bergeling, and Pontus Giselsson. Operator splitting performance estimation: Tight contraction factors and optimal parameter selection. *SIAM Journal on Optimization*, 30(3):2251–2271, January 2020. [doi:10.1137/19m1304854](https://doi.org/10.1137/19m1304854).

<a id="id205"></a>



[[Val43](https://autolyap.github.io/operator_classes/#id2)]

F. A. Valentine. On the extension of a vector function so as to preserve a Lipschitz condition. *Bulletin of the American Mathematical Society*, 49(2):100–108, 1943.

<a id="id206"></a>



[[Val45](https://autolyap.github.io/operator_classes/#id3)]

F. A. Valentine. A Lipschitz condition preserving extension for a vector function. *American Journal of Mathematics*, 67(1):83–93, January 1945. [doi:10.2307/2371917](https://doi.org/10.2307/2371917).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\p}[1]{\mathord{( #1 )}}
```
