Canonical HTML: https://autolyap.github.io/function_classes/
Documentation version: 0.2.1

<a id="function-classes"></a>



# Function classes

- [`Convex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Convex "autolyap.problemclass.Convex")
- [`StronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.StronglyConvex "autolyap.problemclass.StronglyConvex")
- [`WeaklyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.WeaklyConvex "autolyap.problemclass.WeaklyConvex")
- [`Smooth`](https://autolyap.github.io/function_classes/#autolyap.problemclass.Smooth "autolyap.problemclass.Smooth")
- [`SmoothConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothConvex "autolyap.problemclass.SmoothConvex")
- [`SmoothStronglyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothStronglyConvex "autolyap.problemclass.SmoothStronglyConvex")
- [`SmoothWeaklyConvex`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SmoothWeaklyConvex "autolyap.problemclass.SmoothWeaklyConvex")
- [`IndicatorFunctionOfClosedConvexSet`](https://autolyap.github.io/function_classes/#autolyap.problemclass.IndicatorFunctionOfClosedConvexSet "autolyap.problemclass.IndicatorFunctionOfClosedConvexSet")
- [`SupportFunctionOfClosedConvexSet`](https://autolyap.github.io/function_classes/#autolyap.problemclass.SupportFunctionOfClosedConvexSet "autolyap.problemclass.SupportFunctionOfClosedConvexSet")
- [`GradientDominated`](https://autolyap.github.io/function_classes/#autolyap.problemclass.GradientDominated "autolyap.problemclass.GradientDominated")

<a id="autolyap.problemclass.Convex"></a>





```text
class autolyap.problemclass.Convex
```



Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for proper, lower semicontinuous, and convex functions.

Let \(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be proper, lower semicontinuous, and convex.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} \in \partial f(x_{r_1})\), \(g_{r_2} \in \partial f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & 0 & 1 \\
    0 & 0 & 0 & -1 \\
    0 & 0 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

**References**

- <a id="id1"></a>[[THG16](https://autolyap.github.io/function_classes/#id188 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Smooth strongly convex interpolation and exact worst-case performance of first-order methods. Mathematical Programming, 161(1-2):307–345, May 2016. doi:10.1007/s10107-016-1009-3."), Theorem 4].

<a id="autolyap.problemclass.StronglyConvex"></a>





```text
class autolyap.problemclass.StronglyConvex( mu: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for proper, lower semicontinuous, and strongly convex functions.

Let \(\mu \in \mathbb{R}_{++}\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be proper, lower semicontinuous,
and \(\mu\)-strongly convex.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} \in \partial f(x_{r_1})\), \(g_{r_2} \in \partial f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle + \frac{\mu}{2}\|x_{r_1} - x_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2}
\begin{bmatrix}
    \mu & -\mu & 0 & 1 \\
    -\mu & \mu & 0 & -1 \\
    0 & 0 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Strong convexity parameter corresponding to
  \(\mu\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If mu is not valid.

**References**

- <a id="id1"></a>[[THG16](https://autolyap.github.io/function_classes/#id188 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Smooth strongly convex interpolation and exact worst-case performance of first-order methods. Mathematical Programming, 161(1-2):307–345, May 2016. doi:10.1007/s10107-016-1009-3."), Theorem 4].

<a id="autolyap.problemclass.WeaklyConvex"></a>





```text
class autolyap.problemclass.WeaklyConvex( mu_tilde: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for proper, lower semicontinuous, and weakly convex functions.

Let \(\tilde{\mu} \in \mathbb{R}_{++}\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be proper, lower semicontinuous,
and \(\tilde{\mu}\)-weakly convex.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} \in \partial f(x_{r_1})\), \(g_{r_2} \in \partial f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle - \frac{\tilde{\mu}}{2}\|x_{r_1} - x_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2}
\begin{bmatrix}
    -\tilde{\mu} & \tilde{\mu} & 0 & 1 \\
    \tilde{\mu} & -\tilde{\mu} & 0 & -1 \\
    0 & 0 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

**Parameters**

- mu\_tilde ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Weak convexity parameter corresponding to
  \(\tilde{\mu}\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If mu\_tilde is not valid.

**References**

- <a id="id1"></a>[[RGP22](https://autolyap.github.io/function_classes/#id171 "Teodor Rotaru, François Glineur, and Panagiotis Patrinos. Tight convergence rates of the gradient method on smooth hypoconvex functions. 2022. arXiv:2203.00775."), Theorem 3.1].

<a id="autolyap.problemclass.Smooth"></a>





```text
class autolyap.problemclass.Smooth( L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for smooth functions.

Let \(L \in \mathbb{R}_{++}\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be Fréchet differentiable
with \(L\)-Lipschitz gradient.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} = \nabla f(x_{r_1})\), \(g_{r_2} = \nabla f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle
+ \frac{1}{4L}\|g_{r_1} - g_{r_2} + L(x_{r_1} - x_{r_2})\|^2
- \frac{L}{2}\|x_{r_1} - x_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{4L}
\begin{bmatrix}
    -L^2 & L^2 & L & L \\
    L^2 & -L^2 & -L & -L \\
    L & -L & 1 & -1 \\
    L & -L & -1 & 1
\end{bmatrix}.\end{split}\]

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Smoothness parameter corresponding to
  \(L\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If L is not valid.

**References**

- <a id="id1"></a>[[THG17](https://autolyap.github.io/function_classes/#id189 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Exact worst-case performance of first-order methods for composite convex optimization. SIAM Journal on Optimization, 27(3):1283–1313, January 2017. doi:10.1137/16m108104x."), Theorem 3.10].

<a id="autolyap.problemclass.SmoothConvex"></a>





```text
class autolyap.problemclass.SmoothConvex( L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for smooth and convex functions.

Let \(L \in \mathbb{R}_{++}\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be convex, Fréchet differentiable,
with \(L\)-Lipschitz gradient.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} = \nabla f(x_{r_1})\), \(g_{r_2} = \nabla f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle
+ \frac{1}{2L}\|g_{r_1} - g_{r_2}\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2L}
\begin{bmatrix}
    0 & 0 & 0 & L \\
    0 & 0 & 0 & -L \\
    0 & 0 & 1 & -1 \\
    L & -L & -1 & 1
\end{bmatrix}.\end{split}\]

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Smoothness parameter corresponding to
  \(L\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If L is not valid.

**References**

- <a id="id1"></a>[[THG16](https://autolyap.github.io/function_classes/#id188 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Smooth strongly convex interpolation and exact worst-case performance of first-order methods. Mathematical Programming, 161(1-2):307–345, May 2016. doi:10.1007/s10107-016-1009-3."), Theorem 4].

<a id="autolyap.problemclass.SmoothStronglyConvex"></a>





```text
class autolyap.problemclass.SmoothStronglyConvex( mu: int | float, L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for smooth and strongly convex functions.

Let \(0 < \mu < L\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be \(\mu\)-strongly convex,
Fréchet differentiable, with \(L\)-Lipschitz gradient.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} = \nabla f(x_{r_1})\), \(g_{r_2} = \nabla f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle + \frac{\mu}{2}\|x_{r_1} - x_{r_2}\|^2
+ \frac{1}{2(L-\mu)}\|g_{r_1} - g_{r_2} - \mu(x_{r_1} - x_{r_2})\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2(L-\mu)}
\begin{bmatrix}
    L\mu & -L\mu & -\mu & L \\
    -L\mu & L\mu & \mu & -L \\
    -\mu & \mu & 1 & -1 \\
    L & -L & -1 & 1
\end{bmatrix}.\end{split}\]

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Strong convexity parameter corresponding to
  \(\mu\) (must be \(> 0\) and finite).
- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Smoothness parameter corresponding to \(L\)
  (must be \(> 0\) and finite) with \(\mu < L\).

**Raises**

- ValueError: If parameters are not valid.

**References**

- <a id="id1"></a>[[THG16](https://autolyap.github.io/function_classes/#id188 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Smooth strongly convex interpolation and exact worst-case performance of first-order methods. Mathematical Programming, 161(1-2):307–345, May 2016. doi:10.1007/s10107-016-1009-3."), Theorem 4].

<a id="autolyap.problemclass.SmoothWeaklyConvex"></a>





```text
class autolyap.problemclass.SmoothWeaklyConvex( mu_tilde: int | float, L: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_ParametrizedFunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition "autolyap.problemclass.functions._ParametrizedFunctionInterpolationCondition")

Function interpolation condition for smooth and weakly convex functions.

Let \(\tilde{\mu} \in \mathbb{R}_{++}\) and \(L \in \mathbb{R}_{++}\), with
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be \(\tilde{\mu}\)-weakly convex,
Fréchet differentiable, with \(L\)-Lipschitz gradient.

- Interpolation inequality:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} = \nabla f(x_{r_1})\), \(g_{r_2} = \nabla f(x_{r_2})\),
  and \(F_{r_1} = f(x_{r_1})\), \(F_{r_2} = f(x_{r_2})\),


\[F_{r_1} \ge F_{r_2} + \langle g_{r_2}, x_{r_1} - x_{r_2} \rangle - \frac{\tilde{\mu}}{2}\|x_{r_1} - x_{r_2}\|^2
+ \frac{1}{2(L+\tilde{\mu})}\|g_{r_1} - g_{r_2} + \tilde{\mu}(x_{r_1} - x_{r_2})\|^2.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\) and
  \(F = (F_{r_1}, F_{r_2})\), the same inequality is encoded as


\[a^\top F + \mathcal{Q}\p{M, z} \le 0,\]

  with


\[\begin{split}a = (-1, 1), \qquad
M = \frac{1}{2(L+\tilde{\mu})}
\begin{bmatrix}
    -L\tilde{\mu} & L\tilde{\mu} & \tilde{\mu} & L \\
    L\tilde{\mu} & -L\tilde{\mu} & -\tilde{\mu} & -L \\
    \tilde{\mu} & -\tilde{\mu} & 1 & -1 \\
    L & -L & -1 & 1
\end{bmatrix}.\end{split}\]

**Parameters**

- mu\_tilde ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Weak convexity parameter corresponding to
  \(\tilde{\mu}\) (must be \(> 0\) and finite).
- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): Smoothness parameter corresponding to
  \(L\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If parameters are not valid.

**References**

- <a id="id1"></a>[[RGP22](https://autolyap.github.io/function_classes/#id171 "Teodor Rotaru, François Glineur, and Panagiotis Patrinos. Tight convergence rates of the gradient method on smooth hypoconvex functions. 2022. arXiv:2203.00775."), Theorem 3.1].

<a id="autolyap.problemclass.IndicatorFunctionOfClosedConvexSet"></a>





```text
class autolyap.problemclass.IndicatorFunctionOfClosedConvexSet
```



Bases: [`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition")

Function interpolation condition for indicator functions of nonempty, closed, and convex sets.

Let \(C \subseteq \calH\) be nonempty, closed, and convex, and define
its indicator function \(\delta_C : \calH \to \reals \cup \{\pm\infty\}\)
by


\[\begin{split}\delta_C(x) =
\begin{cases}
    0, & \text{if } x \in C, \\
    +\infty, & \text{if } x \notin C.
\end{cases}\end{split}\]

Let \(N_C:\calH\rightrightarrows\calH\) denote the normal cone of \(C\), defined by


\[\begin{split}N_C(x) =
\begin{cases}
    \{g \in \calH : \langle g, z - x \rangle \le 0,\ \forall z \in C\},
    & \text{if } x \in C, \\
    \emptyset, & \text{if } x \notin C,
\end{cases}\end{split}\]

which coincides with the subdifferential of the indicator:


\[\partial \delta_C(x) = N_C(x), \qquad \forall x \in \calH.\]

- Interpolation inequalities used:

  For any \(x_{r_1}, x_{r_2} \in C\) with
  \(g_{r_1} \in N_C(x_{r_1})\), \(g_{r_2} \in N_C(x_{r_2})\),
  and \(F_{r_1} = \delta_C(x_{r_1})\), \(F_{r_2} = \delta_C(x_{r_2})\),


\[\langle g_{r_2}, x_{r_1} - x_{r_2} \rangle \le 0 \quad \text{for } r_1 \ne r_2,\]

  and


\[F_{r_1} = 0.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(F = (F_{r_1}, F_{r_2})\) and
  \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\), the inequality
  constraint has


\[\begin{split}a_1 = (0, 0), \qquad
M_1 = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & 0 & 1 \\
    0 & 0 & 0 & -1 \\
    0 & 0 & 0 & 0 \\
    1 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

  For the equality \(F_{r_1}=0\), with
  \(\hat{z}=(x_{r_1}, g_{r_1})\), the coefficients are


\[\begin{split}a_2 = (1), \qquad
M_2 =
\begin{bmatrix}
    0 & 0 \\
    0 & 0
\end{bmatrix}.\end{split}\]

This condition has no parameters.

**References**

- <a id="id1"></a>[[THG17](https://autolyap.github.io/function_classes/#id189 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Exact worst-case performance of first-order methods for composite convex optimization. SIAM Journal on Optimization, 27(3):1283–1313, January 2017. doi:10.1137/16m108104x."), Theorem 3.6].

<a id="autolyap.problemclass.SupportFunctionOfClosedConvexSet"></a>





```text
class autolyap.problemclass.SupportFunctionOfClosedConvexSet
```



Bases: [`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition")

Function interpolation condition for support functions of nonempty, closed, and convex sets.

Let \(C \subseteq \calH\) be nonempty, closed, and convex, and define
its support function \(\sigma_C : \calH \to \reals \cup \{\pm\infty\}\)
by


\[\sigma_C(x) = \sup_{c \in C} \langle x, c \rangle.\]

- Interpolation inequalities used:

  For any \(x_{r_1}, x_{r_2} \in \calH\) with
  \(g_{r_1} \in \partial \sigma_C(x_{r_1})\), \(g_{r_2} \in \partial \sigma_C(x_{r_2})\),
  and \(F_{r_1} = \sigma_C(x_{r_1})\), \(F_{r_2} = \sigma_C(x_{r_2})\),


\[F_{r_1} = \langle x_{r_1}, g_{r_1} \rangle,\]

  and


\[\langle x_{r_2}, g_{r_1} - g_{r_2} \rangle \le 0.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(F = (F_{r_1}, F_{r_2})\) and
  \(z = (x_{r_1}, x_{r_2}, g_{r_1}, g_{r_2})\), the inequality
  constraint has


\[\begin{split}a_1 = (0, 0), \qquad
M_1 = \frac{1}{2}
\begin{bmatrix}
    0 & 0 & 0 & 0 \\
    0 & 0 & 1 & -1 \\
    0 & 1 & 0 & 0 \\
    0 & -1 & 0 & 0
\end{bmatrix}.\end{split}\]

  For the equality \(F_{r_1}=\langle x_{r_1},g_{r_1}\rangle\),
  with \(\hat{z}=(x_{r_1}, g_{r_1})\), the coefficients are


\[\begin{split}a_2 = (-1), \qquad
M_2 = \frac{1}{2}
\begin{bmatrix}
    0 & 1 \\
    1 & 0
\end{bmatrix}.\end{split}\]

This condition has no parameters.

**References**

- <a id="id1"></a>[[THG17](https://autolyap.github.io/function_classes/#id189 "Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Exact worst-case performance of first-order methods for composite convex optimization. SIAM Journal on Optimization, 27(3):1283–1313, January 2017. doi:10.1137/16m108104x."), Corollary 3.7].

<a id="autolyap.problemclass.GradientDominated"></a>





```text
class autolyap.problemclass.GradientDominated( mu_gd: int | float, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`_FunctionInterpolationCondition`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._FunctionInterpolationCondition "autolyap.problemclass.base._FunctionInterpolationCondition")

Function interpolation condition for gradient-dominated functions.

Let \(\mu_{\textup{gd}} \in \mathbb{R}_{++}\) and
\(f: \calH \to \mathbb{R} \cup \{\pm\infty\}\) be Fréchet differentiable and
\(\mu_{\textup{gd}}\)-gradient dominated.

- Interpolation inequalities used:

  Let \(x_\star\) be a minimizer with \(F_\star = f(x_\star)\).
  For any \(x_{r_1} \in \calH\) with \(g_{r_1} = \nabla f(x_{r_1})\) and
  \(F_{r_1} = f(x_{r_1})\),


\[F_{r_1} - F_\star \le \frac{1}{2\mu_{\textup{gd}}}\|g_{r_1}\|^2
\quad \text{and} \quad
F_{r_1} \ge F_\star.\]
- Matrix/vector form used in [Interpolation conditions](https://autolyap.github.io/theory/interpolation_conditions/):

  With \(F=(F_{r_1},F_\star)\) and
  \(z=(x_{r_1},x_\star,g_{r_1},0)\), the two inequalities are
  encoded by


\[\begin{split}a_1 = (-1, 1), \qquad
M_1 =
\begin{bmatrix}
    0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0
\end{bmatrix},\end{split}\]

  and


\[\begin{split}a_2 = (1, -1), \qquad
M_2 =
\begin{bmatrix}
    0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 \\
    0 & 0 & -\frac{1}{2\mu_{\textup{gd}}} & 0 \\
    0 & 0 & 0 & 0
\end{bmatrix}.\end{split}\]

**Parameters**

- mu\_gd ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The gradient-dominated parameter corresponding
  to \(\mu_{\textup{gd}}\) (must be \(> 0\) and finite).

**Raises**

- ValueError: If mu\_gd is not a number, \(\le 0\), or infinite.

**Note**

- This condition is only sufficient for AutoLyap analyses; tightness of
  the resulting bound is not guaranteed.
- When used inside [`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"),
  the problem must have exactly one component. In the notation of
  [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/),
  \(m = 1\), \(m_{\textup{func}} = 1\), and
  \(m_{\textup{op}} = 0\). This single component may still
  contain a list of function conditions (an intersection).

<a id="references"></a>



## References

<a id="id1"></a>



<a id="id171"></a>



[RGP22]
([1](https://autolyap.github.io/function_classes/#id1),[2](https://autolyap.github.io/function_classes/#id1))

Teodor Rotaru, François Glineur, and Panagiotis Patrinos. Tight convergence rates of the gradient method on smooth hypoconvex functions. 2022. [arXiv:2203.00775](https://arxiv.org/abs/2203.00775).

<a id="id188"></a>



[THG16]
([1](https://autolyap.github.io/function_classes/#id1),[2](https://autolyap.github.io/function_classes/#id1),[3](https://autolyap.github.io/function_classes/#id1),[4](https://autolyap.github.io/function_classes/#id1))

Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Smooth strongly convex interpolation and exact worst-case performance of first-order methods. *Mathematical Programming*, 161(1-2):307–345, May 2016. [doi:10.1007/s10107-016-1009-3](https://doi.org/10.1007/s10107-016-1009-3).

<a id="id189"></a>



[THG17]
([1](https://autolyap.github.io/function_classes/#id1),[2](https://autolyap.github.io/function_classes/#id1),[3](https://autolyap.github.io/function_classes/#id1))

Adrien B. Taylor, Julien M. Hendrickx, and François Glineur. Exact worst-case performance of first-order methods for composite convex optimization. *SIAM Journal on Optimization*, 27(3):1283–1313, January 2017. [doi:10.1137/16m108104x](https://doi.org/10.1137/16m108104x).

TeX macro definitions used in this page:

```latex
\newcommand{\calH}{\mathcal{H}}
\newcommand{\p}[1]{\mathord{( #1 )}}
\newcommand{\reals}{\mathbb{R}}
```
