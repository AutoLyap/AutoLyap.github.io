Canonical HTML: https://autolyap.github.io/concrete_algorithms/
Documentation version: 0.2.1

<a id="concrete-algorithms"></a>



# Concrete algorithms

- [`AcceleratedProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.AcceleratedProximalPoint "autolyap.algorithms.AcceleratedProximalPoint")
- [`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.ChambollePock")
- [`DavisYin`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DavisYin "autolyap.algorithms.DavisYin")
- [`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.DouglasRachford")
- [`Extragradient`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.Extragradient "autolyap.algorithms.Extragradient")
- [`ForwardMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ForwardMethod "autolyap.algorithms.ForwardMethod")
- [`GradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientMethod "autolyap.algorithms.GradientMethod")
- [`GradientNesterovMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientNesterovMomentum "autolyap.algorithms.GradientNesterovMomentum")
- [`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.HeavyBallMethod")
- [`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.ITEM")
- [`MalitskyTamFRB`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.MalitskyTamFRB "autolyap.algorithms.MalitskyTamFRB")
- [`NesterovConstant`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovConstant "autolyap.algorithms.NesterovConstant")
- [`NesterovFastGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovFastGradientMethod "autolyap.algorithms.NesterovFastGradientMethod")
- [`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.OptimizedGradientMethod")
- [`ProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ProximalPoint "autolyap.algorithms.ProximalPoint")
- [`TripleMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TripleMomentum "autolyap.algorithms.TripleMomentum")
- [`TsengFBF`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TsengFBF "autolyap.algorithms.TsengFBF")

<a id="autolyap.algorithms.AcceleratedProximalPoint"></a>





```text
class autolyap.algorithms.AcceleratedProximalPoint( gamma: float, type: str = 'operator', )
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Accelerated proximal point method <a id="id1"></a>[[Kim21](https://autolyap.github.io/concrete_algorithms/#id108 "Donghwan Kim. Accelerated proximal point method for maximally monotone operators. Mathematical Programming, 190(1-2):57–87, March 2021. doi:10.1007/s10107-021-01643-0.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

Let \(\lambda_k = k/(k+2)\). For initial points
\(x^0,y^0,y^{-1} \in \calH\) and \(\gamma \in \reals_{++}\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    x^{k+1} &=
    \begin{cases}
        J_{\gamma G}(y^k), & \text{if type = operator}, \\
        \prox_{\gamma f}(y^k), & \text{if type = function},
    \end{cases} \\
    y^{k+1} &= x^{k+1} + \lambda_k(x^{k+1} - x^k) - \lambda_k(x^k - y^{k-1}).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, y^k, y^{k-1}), \qquad
\bu^k = \frac{y^k - x^{k+1}}{\gamma}, \qquad
\by^k = y^k.\]

With \(\lambda_k = k/(k+2)\), the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    0 & 1 & 0 \\
    -2\lambda_k & 1+\lambda_k & \lambda_k \\
    0 & 1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\gamma \\
    -\gamma(1+\lambda_k) \\
    0
    \end{bmatrix}, \\
    C_k &= \begin{bmatrix} 0 & 1 & 0 \end{bmatrix}, &
    D_k &= \begin{bmatrix} -\gamma \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[\text{type}=\text{"operator"}:\quad
n = 3,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1,\quad
I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1\}.\]


\[\text{type}=\text{"function"}:\quad
n = 3,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1,\quad
I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.AcceleratedProximalPoint.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.AcceleratedProximalPoint.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`AcceleratedProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.AcceleratedProximalPoint "autolyap.algorithms.AcceleratedProximalPoint").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.ChambollePock"></a>





```text
class autolyap.algorithms.ChambollePock( tau: float, sigma: float, theta: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Chambolle–Pock primal-dual method <a id="id1"></a>[[CP11](https://autolyap.github.io/concrete_algorithms/#id43 "Antonin Chambolle and Thomas Pock. A first-order primal-dual algorithm for convex problems with applications to imaging. Journal of Mathematical Imaging and Vision, 40(1):120–145, 2011. doi:10.1007/s10851-010-0251-1.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

In the identity-operator case, for initial points \(x^0,y^0 \in \calH\),
primal and dual step sizes \(\tau,\sigma \in \reals_{++}\), and
relaxation parameter \(\theta \in \reals\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    x^{k+1} &= \prox_{\tau f_1}(x^k - \tau y^k), \\
    y^{k+1} &= \prox_{\sigma f_2^*}\!\left(y^k + \sigma \left(x^{k+1} + \theta (x^{k+1} - x^k)\right)\right).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, y^k), \qquad
\bu^k = \left(\frac{x^k-\tau y^k-x^{k+1}}{\tau},\; y^{k+1}\right),\]


\[\by^k = \left(
x^{k+1},\;
\frac{y^k + \sigma\left(x^{k+1} + \theta(x^{k+1}-x^k)\right)-y^{k+1}}{\sigma}
\right).\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1 & -\tau \\
    0 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\tau & 0 \\
    0 & 1
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 & -\tau \\
    1 & \frac{1}{\sigma} - \tau(1+\theta)
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    -\tau & 0 \\
    -\tau(1+\theta) & -\frac{1}{\sigma}
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (1,1),\quad \bar{m} = 2.\]


\[I_{\text{func}} = \{1,2\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.ChambollePock.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.ChambollePock.set_sigma"></a>





```text
set_sigma( sigma: float, ) → None
```



Set the dual step size \(\sigma\).

Shared notation follows the class-level reference in
[`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.ChambollePock").

**Parameters**

- sigma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\sigma\).

**Raises**

- ValueError: If sigma is not a finite real number or if \(\sigma \le 0\).

<a id="autolyap.algorithms.ChambollePock.set_tau"></a>





```text
set_tau( tau: float, ) → None
```



Set the primal step size \(\tau\).

Shared notation follows the class-level reference in
[`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.ChambollePock").

**Parameters**

- tau ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\tau\).

**Raises**

- ValueError: If tau is not a finite real number or if \(\tau \le 0\).

<a id="autolyap.algorithms.ChambollePock.set_theta"></a>





```text
set_theta( theta: float, ) → None
```



Set the relaxation parameter \(\theta\).

Shared notation follows the class-level reference in
[`ChambollePock`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ChambollePock "autolyap.algorithms.ChambollePock").

**Parameters**

- theta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\theta\).

**Raises**

- ValueError: If theta is not a finite real number.

<a id="autolyap.algorithms.DavisYin"></a>





```text
class autolyap.algorithms.DavisYin( gamma: float, lambda_value: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Davis–Yin three-operator splitting <a id="id1"></a>[[DY17](https://autolyap.github.io/concrete_algorithms/#id61 "Damek Davis and Wotao Yin. A three-operator splitting scheme and its optimization applications. Set-Valued and Variational Analysis, 25(4):829–858, June 2017. doi:10.1007/s11228-017-0421-z.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\), step size
\(\gamma \in \reals_{++}\), and relaxation parameter
\(\lambda \in \reals\), define,
for all \(k \in \naturals\),


\[\begin{split}\left[
\begin{aligned}
    v^k &= \prox_{\gamma f_1}(x^k), \\
    w^k &= \prox_{\gamma f_3}\!\left(2v^k - x^k - \gamma \nabla f_2(v^k)\right), \\
    x^{k+1} &= x^k + \lambda (w^k - v^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\begin{split}\begin{aligned}
    \bx^k &= x^k, \\
    \bu^k &= \left(
    \frac{x^k-v^k}{\gamma},\;
    \nabla f_2(v^k),\;
    \frac{2v^k-x^k-\gamma\nabla f_2(v^k)-w^k}{\gamma}
    \right), \\
    \by^k &= (v^k, v^k, w^k).
\end{aligned}\end{split}\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, &
    B_k &= \begin{bmatrix} -\gamma\lambda & -\gamma\lambda & -\gamma\lambda \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 \\
    1 \\
    1
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    -\gamma & 0 & 0 \\
    -\gamma & 0 & 0 \\
    -2\gamma & -\gamma & -\gamma
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 1,\quad m = 3,\quad (\bar{m}_i)_{i=1}^{m} = (1,1,1),\quad \bar{m} = 3.\]


\[I_{\text{func}} = \{1,2,3\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.DavisYin.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.DavisYin.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`DavisYin`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DavisYin "autolyap.algorithms.DavisYin").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.DavisYin.set_lambda"></a>





```text
set_lambda( lambda_value: float, ) → None
```



Set the relaxation parameter \(\lambda\).

Shared notation follows the class-level reference in
[`DavisYin`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DavisYin "autolyap.algorithms.DavisYin").

**Parameters**

- lambda\_value ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\lambda\).

**Raises**

- ValueError: If lambda\_value is not a finite real number.

<a id="autolyap.algorithms.DouglasRachford"></a>





```text
class autolyap.algorithms.DouglasRachford( gamma: float, lambda_value: float, type: str = 'operator', )
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Douglas–Rachford splitting <a id="id1"></a>[[DR56](https://autolyap.github.io/concrete_algorithms/#id65 "Jim Douglas and H. H. Rachford. On the numerical solution of heat conduction problems in two and three space variables. Transactions of the American Mathematical Society, 82(2):421–439, 1956. doi:10.1090/s0002-9947-1956-0084194-4.")], <a id="id2"></a>[[EB92](https://autolyap.github.io/concrete_algorithms/#id71 "Jonathan Eckstein and Dimitri P. Bertsekas. On the Douglas–Rachford splitting method and the proximal point algorithm for maximal monotone operators. Mathematical Programming, 55(1-3):293–318, April 1992. doi:10.1007/bf01581204.")], <a id="id3"></a>[[LM79](https://autolyap.github.io/concrete_algorithms/#id117 "P. L. Lions and B. Mercier. Splitting algorithms for the sum of two nonlinear operators. SIAM Journal on Numerical Analysis, 16(6):964–979, December 1979. doi:10.1137/0716071.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\), step size \(\gamma \in \reals_{++}\),
and relaxation parameter \(\lambda \in \reals\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    v^k &=
    \begin{cases}
        J_{\gamma G_1}(x^k), & \text{if type = operator}, \\
        \prox_{\gamma f_1}(x^k), & \text{if type = function},
    \end{cases} \\
    w^k &=
    \begin{cases}
        J_{\gamma G_2}(2v^k - x^k), & \text{if type = operator}, \\
        \prox_{\gamma f_2}(2v^k - x^k), & \text{if type = function},
    \end{cases} \\
    x^{k+1} &= x^k + \lambda (w^k - v^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\begin{split}\begin{aligned}
    \bx^k &= x^k, \\
    \bu^k &= \left(
    \frac{x^k-v^k}{\gamma},\;
    \frac{2v^k-x^k-w^k}{\gamma}
    \right), \\
    \by^k &= \left(v^k,\; w^k\right).
\end{aligned}\end{split}\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, &
    B_k &= \begin{bmatrix} -\gamma\lambda & -\gamma\lambda \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 \\
    1
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    -\gamma & 0 \\
    -2\gamma & -\gamma
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[\text{type}=\text{"operator"}:\quad
n = 1,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (1,1),\quad \bar{m} = 2,\quad
I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1,2\}.\]


\[\text{type}=\text{"function"}:\quad
n = 1,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (1,1),\quad \bar{m} = 2,\quad
I_{\text{func}} = \{1,2\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.DouglasRachford.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.DouglasRachford.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.DouglasRachford").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.DouglasRachford.set_lambda"></a>





```text
set_lambda( lambda_value: float, ) → None
```



Set the relaxation parameter \(\lambda\).

Shared notation follows the class-level reference in
[`DouglasRachford`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.DouglasRachford "autolyap.algorithms.DouglasRachford").

**Parameters**

- lambda\_value ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\lambda\).

**Raises**

- ValueError: If lambda\_value is not a finite real number.

<a id="autolyap.algorithms.Extragradient"></a>





```text
class autolyap.algorithms.Extragradient( gamma: float, delta: float, type: str = 'unconstrained', )
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str)

Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Extragradient method <a id="id1"></a>[[Kor76](https://autolyap.github.io/concrete_algorithms/#id112 "Galina M. Korpelevich. The extragradient method for finding saddle points and other problems. Matecon, 12:747–756, 1976.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\), step sizes
\(\gamma,\delta \in \reals_{++}\), and \(k \in \naturals\),


\[\begin{split}\left[
\begin{aligned}
    &\bar{x}^k = x^k - \gamma G_1(x^k), \\
    &x^{k+1} = x^k - \delta G_1(\bar{x}^k),
\end{aligned}
\right.
\qquad \text{if type = unconstrained},\end{split}\]

and


\[\begin{split}\left[
\begin{aligned}
    &\bar{x}^k = \prox_{\gamma f}(x^k - \gamma G_1(x^k)), \\
    &x^{k+1} = \prox_{\delta f}(x^k - \delta G_1(\bar{x}^k)),
\end{aligned}
\right.
\qquad \text{if type = constrained}.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = x^k.\]

If type = unconstrained, use


\[\bu^k = (G_1(x^k), G_1(\bar{x}^k)), \qquad
\by^k = (x^k, \bar{x}^k),\]

with the system matrices


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, &
    B_k &= \begin{bmatrix} 0 & -\delta \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 \\
    1
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    0 & 0 \\
    -\gamma & 0
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD") when
type = unconstrained.

If type = constrained, use


\[\bu^k = \left(
G_1(x^k),\;
G_1(\bar{x}^k),\;
\frac{x^k - \gamma G_1(x^k) - \bar{x}^k}{\gamma},\;
\frac{x^k - \delta G_1(\bar{x}^k) - x^{k+1}}{\delta}
\right),\]


\[\by^k = (x^k, \bar{x}^k, \bar{x}^k, x^{k+1}),\]

with the system matrices


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, &
    B_k &= \begin{bmatrix} 0 & -\delta & 0 & -\delta \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 \\
    1 \\
    1 \\
    1
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    0 & 0 & 0 & 0 \\
    -\gamma & 0 & -\gamma & 0 \\
    -\gamma & 0 & -\gamma & 0 \\
    0 & -\delta & 0 & -\delta
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD") when
type = constrained.

<a id="structural-parameters"></a>



## Structural parameters


\[\text{type}=\text{"unconstrained"}:\quad
n = 1,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (2),\quad \bar{m} = 2,\quad
I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1\}.\]


\[\text{type}=\text{"constrained"}:\quad
n = 1,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (2,2),\quad \bar{m} = 4,\quad
I_{\text{func}} = \{2\},\quad I_{\text{op}} = \{1\}.\]

<a id="autolyap.algorithms.Extragradient.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.Extragradient.set_delta"></a>





```text
set_delta( delta: float, ) → None
```



Set the second step-size parameter \(\delta\).

Shared notation follows the class-level reference in
[`Extragradient`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.Extragradient "autolyap.algorithms.Extragradient").

**Parameters**

- delta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\delta\).

**Raises**

- ValueError: If delta is not a finite real number or if \(\delta \le 0\).

<a id="autolyap.algorithms.Extragradient.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the first step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`Extragradient`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.Extragradient "autolyap.algorithms.Extragradient").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.ForwardMethod"></a>





```text
class autolyap.algorithms.ForwardMethod( gamma: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Forward method.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\) and step size \(\gamma \in \reals_{++}\),


\[(\forall k \in \naturals)\quad x^{k+1} = x^k - \gamma G_1(x^k).\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = x^k, \qquad
\bu^k = G_1(x^k), \qquad
\by^k = x^k.\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, & B_k &= \begin{bmatrix} -\gamma \end{bmatrix}, \\
    C_k &= \begin{bmatrix} 1 \end{bmatrix}, & D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 1,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1\}.\]

<a id="autolyap.algorithms.ForwardMethod.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.ForwardMethod.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`ForwardMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ForwardMethod "autolyap.algorithms.ForwardMethod").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.GradientMethod"></a>





```text
class autolyap.algorithms.GradientMethod( gamma: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Gradient method.

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\) and step size \(\gamma \in \reals_{++}\),


\[(\forall k \in \naturals)\quad x^{k+1} = x^k - \gamma \nabla f(x^k).\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = x^k, \qquad
\bu^k = \nabla f(x^k), \qquad
\by^k = x^k.\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, & B_k &= \begin{bmatrix} -\gamma \end{bmatrix}, \\
    C_k &= \begin{bmatrix} 1 \end{bmatrix}, & D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 1,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.GradientMethod.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.GradientMethod.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`GradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientMethod "autolyap.algorithms.GradientMethod").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.GradientNesterovMomentum"></a>





```text
class autolyap.algorithms.GradientNesterovMomentum( gamma: float, delta: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Gradient method with Nesterov-like momentum <a id="id1"></a>[[Nes18](https://autolyap.github.io/concrete_algorithms/#id146 "Yurii Nesterov. Lectures on Convex Optimization. Springer International Publishing, 2018. ISBN 9783319915784. doi:10.1007/978-3-319-91578-4.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For initial points \(x^{-1},x^0 \in \calH\), step size \(\gamma \in \reals_{++}\),
and momentum parameter \(\delta \in \reals\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    y^k &= x^k + \delta (x^k - x^{k-1}), \\
    x^{k+1} &= y^k - \gamma \nabla f(y^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, x^{k-1}), \qquad
\bu^k = \nabla f(y^k), \qquad
\by^k = y^k.\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1+\delta & -\delta \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\gamma \\
    0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1+\delta & -\delta
    \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.GradientNesterovMomentum.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.GradientNesterovMomentum.set_delta"></a>





```text
set_delta( delta: float, ) → None
```



Set the momentum parameter \(\delta\).

Shared notation follows the class-level reference in
[`GradientNesterovMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientNesterovMomentum "autolyap.algorithms.GradientNesterovMomentum").

**Parameters**

- delta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\delta\).

**Raises**

- ValueError: If delta is not a finite real number.

<a id="autolyap.algorithms.GradientNesterovMomentum.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`GradientNesterovMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.GradientNesterovMomentum "autolyap.algorithms.GradientNesterovMomentum").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.HeavyBallMethod"></a>





```text
class autolyap.algorithms.HeavyBallMethod( gamma: float, delta: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Heavy-ball method <a id="id1"></a>[[Pol64](https://autolyap.github.io/concrete_algorithms/#id162 "B. T. Polyak. Some methods of speeding up the convergence of iteration methods. USSR Computational Mathematics and Mathematical Physics, 4(5):1–17, 1964. doi:10.1016/0041-5553(64)90137-5.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For initial points \(x^{-1},x^0 \in \calH\), step size \(\gamma \in \reals_{++}\),
and momentum parameter \(\delta \in \reals\),


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - \gamma \nabla f(x^k) + \delta (x^k - x^{k-1}).\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, x^{k-1}), \qquad
\bu^k = \nabla f(x^k), \qquad
\by^k = x^k.\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1+\delta & -\delta \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\gamma \\
    0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 & 0
    \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.HeavyBallMethod.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.HeavyBallMethod.set_delta"></a>





```text
set_delta( delta: float, ) → None
```



Set the momentum parameter \(\delta\).

Shared notation follows the class-level reference in
[`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.HeavyBallMethod").

**Parameters**

- delta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\delta\).

**Raises**

- ValueError: If delta is not a finite real number.

<a id="autolyap.algorithms.HeavyBallMethod.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`HeavyBallMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.HeavyBallMethod "autolyap.algorithms.HeavyBallMethod").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.ITEM"></a>





```text
class autolyap.algorithms.ITEM( mu: float, L: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Information-theoretic exact method (ITEM) <a id="id1"></a>[[TD23](https://autolyap.github.io/concrete_algorithms/#id194 "Adrien Taylor and Yoel Drori. An optimal gradient method for smooth strongly convex minimization. Mathematical Programming, 199(1-2):557–594, 2023. doi:10.1007/s10107-022-01839-y.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

Let \(q = \mu/L\), \(\tilde{A}_0 = 0\), and for all \(k \in \naturals\),


\[\begin{split}\begin{aligned}
    \tilde{A}_{k+1}
    &= \frac{(1+q)\tilde{A}_k + 2\left(1 + \sqrt{(1+\tilde{A}_k)(1+q\tilde{A}_k)}\right)}{(1-q)^2}, \\
    \beta_k
    &= \frac{\tilde{A}_k}{(1-q)\tilde{A}_{k+1}}, \\
    \delta_k
    &= \frac{(1-q)^2\tilde{A}_{k+1} - (1+q)\tilde{A}_k}{2(1+q+q\tilde{A}_k)}.
\end{aligned}\end{split}\]

For initial points \(x^0,z^0 \in \calH\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    y^k &= (1-\beta_k)z^k + \beta_k x^k, \\
    x^{k+1} &= y^k - \frac{1}{L}\nabla f(y^k), \\
    z^{k+1} &= (1-q\delta_k)z^k + q\delta_k y^k - \frac{\delta_k}{L}\nabla f(y^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, z^k), \qquad
\bu^k = \nabla f(y^k), \qquad
\by^k = y^k.\]

Let \(q = \mu/L\), with \(\beta_k\) and \(\delta_k\) as in the
standard form above. The system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    \beta_k & 1-\beta_k \\
    q\beta_k\delta_k & 1-q\beta_k\delta_k
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\frac{1}{L} \\
    -\frac{\delta_k}{L}
    \end{bmatrix}, \\
    C_k &= \begin{bmatrix} \beta_k & 1-\beta_k \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.ITEM.get_A"></a>





```text
get_A( k: int, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Return the scalar sequence value \(\tilde{A}_k\).

This scalar follows the recurrence stated in the class-level reference of
[`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.ITEM").
The system matrix \(A_k\) in [`get_ABCD()`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM.get_ABCD "autolyap.algorithms.ITEM.get_ABCD") uses the same letter by
convention; this accessor returns only the scalar sequence
\(\tilde{A}_k\).

<a id="autolyap.algorithms.ITEM.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.ITEM.set_L"></a>





```text
set_L( L: float, ) → None
```



Set the smoothness parameter \(L\).

Shared notation follows the class-level reference in
[`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.ITEM").

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(L\).

**Raises**

- ValueError: If L is not a finite real number, if \(L \le 0\),
  or if \(\mu \ge L\).

<a id="autolyap.algorithms.ITEM.set_mu"></a>





```text
set_mu( mu: float, ) → None
```



Set the strong-convexity parameter \(\mu\).

Shared notation follows the class-level reference in
[`ITEM`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ITEM "autolyap.algorithms.ITEM").

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\mu\).

**Raises**

- ValueError: If mu is not a finite real number, if \(\mu \le 0\),
  or if \(\mu \ge L\).

<a id="autolyap.algorithms.MalitskyTamFRB"></a>





```text
class autolyap.algorithms.MalitskyTamFRB( gamma: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Malitsky–Tam forward-reflected-backward method <a id="id1"></a>[[MT20](https://autolyap.github.io/concrete_algorithms/#id123 "Yura Malitsky and Matthew K. Tam. A forward-backward splitting method for monotone inclusions without cocoercivity. SIAM Journal on Optimization, 30(2):1451–1472, January 2020. doi:10.1137/18m1207260.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial pair \((x^{-1}, x^0) \in \calH^2\) and step size
\(\gamma \in \reals_{++}\),


\[(\forall k \in \naturals)\quad
x^{k+1} = J_{\gamma G_2}\left(x^k - 2\gamma G_1(x^k) + \gamma G_1(x^{k-1})\right).\]

Equivalently,


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - 2\gamma G_1(x^k) + \gamma G_1(x^{k-1}) - \gamma G_2(x^{k+1}).\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k,\; x^{k-1}), \qquad
\bu^k = \left(G_1(x^k),\; G_1(x^{k-1}),\; G_2(x^{k+1})\right), \qquad
\by^k = \left(x^k,\; x^{k-1},\; x^{k+1}\right).\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1 & 0 \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -2\gamma & \gamma & -\gamma \\
    0 & 0 & 0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 & 0 \\
    0 & 1 \\
    1 & 0
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    0 & 0 & 0 \\
    0 & 0 & 0 \\
    -2\gamma & \gamma & -\gamma
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (2,1),\quad \bar{m} = 3.\]


\[I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1,2\}.\]

<a id="autolyap.algorithms.MalitskyTamFRB.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.MalitskyTamFRB.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`MalitskyTamFRB`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.MalitskyTamFRB "autolyap.algorithms.MalitskyTamFRB").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.NesterovConstant"></a>





```text
class autolyap.algorithms.NesterovConstant( mu: float, L: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Nesterov’s constant-step scheme <a id="id1"></a>[[Nes18](https://autolyap.github.io/concrete_algorithms/#id146 "Yurii Nesterov. Lectures on Convex Optimization. Springer International Publishing, 2018. ISBN 9783319915784. doi:10.1007/978-3-319-91578-4."), Constant step scheme III].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

Let \(q = \mu/L\) and
\(\eta = (1-\sqrt{q})/(1+\sqrt{q})\). For initial points
\(x^{-1},x^0 \in \calH\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    y^k &= x^k + \eta (x^k - x^{k-1}), \\
    x^{k+1} &= y^k - \frac{1}{L}\nabla f(y^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, x^{k-1}), \qquad
\bu^k = \nabla f(y^k), \qquad
\by^k = y^k.\]

Let \(q = \mu/L\). With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    \frac{2}{1+\sqrt{q}} & -\frac{1-\sqrt{q}}{1+\sqrt{q}} \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\frac{1}{L} \\
    0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    \frac{2}{1+\sqrt{q}} & -\frac{1-\sqrt{q}}{1+\sqrt{q}}
    \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.NesterovConstant.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.NesterovConstant.set_L"></a>





```text
set_L( L: float, ) → None
```



Set the smoothness parameter \(L\).

Shared notation follows the class-level reference in
[`NesterovConstant`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovConstant "autolyap.algorithms.NesterovConstant").

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(L\).

**Raises**

- ValueError: If L is not a finite real number or if \(L \le 0\).

<a id="autolyap.algorithms.NesterovConstant.set_mu"></a>





```text
set_mu( mu: float, ) → None
```



Set the strong-convexity parameter \(\mu\).

Shared notation follows the class-level reference in
[`NesterovConstant`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovConstant "autolyap.algorithms.NesterovConstant").

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\mu\).

**Raises**

- ValueError: If mu is not a finite real number or if \(\mu \le 0\).

<a id="autolyap.algorithms.NesterovFastGradientMethod"></a>





```text
class autolyap.algorithms.NesterovFastGradientMethod( gamma: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Nesterov’s fast gradient method <a id="id1"></a>[[Nes83](https://autolyap.github.io/concrete_algorithms/#id144 "Yurii Nesterov. A method for solving the convex programming problem with convergence rate o(1/k^2). Doklady Akademii Nauk SSSR, 269(3):543–547, 1983.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For initial points \(x^{-1},x^0 \in \calH\), step size \(\gamma \in \reals_{++}\),
and \(\lambda_0 = 1\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    y^k &= x^k + \delta_k (x^k - x^{k-1}), \\
    x^{k+1} &= y^k - \gamma \nabla f(y^k), \\
    \lambda_{k+1} &= \frac{1 + \sqrt{1 + 4\lambda_k^2}}{2}, \\
    \delta_k &= \frac{\lambda_k - 1}{\lambda_{k+1}}.
\end{aligned}
\right.\end{split}\]

This implementation also keeps an additional evaluation of \(\nabla f(x^k)\)
for analysis templates that require it.

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, x^{k-1}), \qquad
\bu^k = (\nabla f(y^k), \nabla f(x^k)), \qquad
\by^k = (y^k, x^k).\]

With \(\lambda_0 = 1\), \(\lambda_{k+1} = \frac{1+\sqrt{1+4\lambda_k^2}}{2}\),
and \(\alpha_k = \frac{\lambda_k - 1}{\lambda_{k+1}}\), the system
matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1+\alpha_k & -\alpha_k \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\gamma & 0 \\
    0 & 0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1+\alpha_k & -\alpha_k \\
    1 & 0
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    0 & 0 \\
    0 & 0
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (2),\quad \bar{m} = 2.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.NesterovFastGradientMethod.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.NesterovFastGradientMethod.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`NesterovFastGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.NesterovFastGradientMethod "autolyap.algorithms.NesterovFastGradientMethod").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.OptimizedGradientMethod"></a>





```text
class autolyap.algorithms.OptimizedGradientMethod( L: float, K: int, )
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Optimized gradient method <a id="id1"></a>[[KF15](https://autolyap.github.io/concrete_algorithms/#id107 "Donghwan Kim and Jeffrey A. Fessler. Optimized first-order methods for smooth convex minimization. Mathematical Programming, 159(1-2):81–107, October 2015. doi:10.1007/s10107-015-0949-3.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For initial points \(x^0,y^0 \in \calH\), smoothness constant
\(L \in \reals_{++}\), and iteration budget \(K \in \naturals\),


\[\begin{split}(\forall k \in \llbracket 0, K-1 \rrbracket)\quad
\left[
\begin{aligned}
    y^{k+1} &= x^k - \frac{1}{L}\nabla f(x^k), \\
    x^{k+1} &= y^{k+1} + \frac{\theta_k - 1}{\theta_{k+1}}(y^{k+1} - y^k) + \frac{\theta_k}{\theta_{k+1}}(y^{k+1} - x^k).
\end{aligned}
\right.\end{split}\]


\[\begin{split}\theta_k =
\begin{cases}
    1, & \text{if } k = 0, \\
    \dfrac{1 + \sqrt{1 + 4\theta_{k-1}^2}}{2},
    & \text{if } k \in \llbracket 1, K-1 \rrbracket, \\
    \dfrac{1 + \sqrt{1 + 8\theta_{k-1}^2}}{2},
    & \text{if } k = K.
\end{cases}\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, y^k), \qquad
\bu^k = \nabla f(x^k), \qquad
\by^k = x^k.\]

For \(k \in \llbracket 0, K-1 \rrbracket\), with \(\theta_k\) as in
the standard form, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1+\frac{\theta_k-1}{\theta_{k+1}} & \frac{1-\theta_k}{\theta_{k+1}} \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\frac{1+(2\theta_k-1)/\theta_{k+1}}{L} \\
    -\frac{1}{L}
    \end{bmatrix}, \\
    C_k &= \begin{bmatrix} 1 & 0 \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD") for
\(k \in \llbracket 0, K-1 \rrbracket\).

For \(k = K\), the system matrices are chosen for convenience as


\[\begin{split}\begin{aligned}
    A_K &=
    \begin{bmatrix}
    0 & 0 \\
    0 & 0
    \end{bmatrix}, &
    B_K &=
    \begin{bmatrix}
    0 \\
    0
    \end{bmatrix}, \\
    C_K &= \begin{bmatrix} 1 & 0 \end{bmatrix}, &
    D_K &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the terminal system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD") for
\(k = K\).

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.OptimizedGradientMethod.compute_theta"></a>





```text
compute_theta( k: int, K: int, ) → float
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Compute \(\theta_k\) for horizon \(K\).

Shared notation follows the class-level reference in
[`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.OptimizedGradientMethod").

**Parameters**

- k ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Target index \(k\), with \(0 \le k \le K\).
- K ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): Horizon index \(K\).

**Returns**

- (`float`): The scalar \(\theta_k\) defined by the OGM recurrence.

**Raises**

- ValueError: If k or K are invalid, or if k > K.

<a id="autolyap.algorithms.OptimizedGradientMethod.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.OptimizedGradientMethod.set_K"></a>





```text
set_K( K: int, ) → None
```



[int](https://autolyap.github.io/dev/dev_external_reference_targets/#int)

Set the iteration budget \(K\).

Shared notation follows the class-level reference in
[`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.OptimizedGradientMethod").

**Parameters**

- K ([`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int")): The value corresponding to \(K\).

**Raises**

- ValueError: If K is not an integer or if \(K < 0\).

<a id="autolyap.algorithms.OptimizedGradientMethod.set_L"></a>





```text
set_L( L: float, ) → None
```



Set the smoothness parameter \(L\).

Shared notation follows the class-level reference in
[`OptimizedGradientMethod`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.OptimizedGradientMethod "autolyap.algorithms.OptimizedGradientMethod").

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(L\).

**Raises**

- ValueError: If L is not a finite real number or if \(L \le 0\).

<a id="autolyap.algorithms.ProximalPoint"></a>





```text
class autolyap.algorithms.ProximalPoint( gamma: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Proximal point method <a id="id1"></a>[[Mor65](https://autolyap.github.io/concrete_algorithms/#id169 "Jean-Jacques Moreau. Proximité et dualité dans un espace hilbertien. Bulletin de la Société Mathématique de France, 93:273–299, 1965. doi:10.24033/bsmf.1625.")], <a id="id2"></a>[[Mar70](https://autolyap.github.io/concrete_algorithms/#id168 "B. Martinet. Brève communication. Régularisation d'inéquations variationnelles par approximations successives. Revue française d'informatique et de recherche opérationnelle. Série rouge, 4(R3):154–158, 1970. doi:10.1051/m2an/197004R301541.")], <a id="id3"></a>[[Roc76](https://autolyap.github.io/concrete_algorithms/#id167 "R. Tyrrell Rockafellar. Monotone operators and the proximal point algorithm. SIAM Journal on Control and Optimization, 14(5):877–898, 1976. doi:10.1137/0314056.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\) and step size
\(\gamma \in \reals_{++}\),


\[(\forall k \in \naturals)\quad
x^{k+1} = \prox_{\gamma f}(x^k).\]

Equivalently,


\[(\forall k \in \naturals)\quad
x^{k+1} = x^k - \gamma u^k, \qquad u^k \in \partial f(x^{k+1}).\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = x^k, \qquad
\bu^k = u^k, \qquad
\by^k = x^{k+1}.\]

Using the equivalent standard form,


\[u^k \in \partial f(x^{k+1}).\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, & B_k &= \begin{bmatrix} -\gamma \end{bmatrix}, \\
    C_k &= \begin{bmatrix} 1 \end{bmatrix}, & D_k &= \begin{bmatrix} -\gamma \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 1,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.ProximalPoint.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.ProximalPoint.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`ProximalPoint`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.ProximalPoint "autolyap.algorithms.ProximalPoint").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.TripleMomentum"></a>





```text
class autolyap.algorithms.TripleMomentum( mu: float, L: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Triple-momentum method <a id="id1"></a>[[VSFL18](https://autolyap.github.io/concrete_algorithms/#id207 "Bryan Van Scoy, Randy A. Freeman, and Kevin M. Lynch. The fastest known globally convergent first-order method for minimizing strongly convex functions. IEEE Control Systems Letters, 2(1):49–54, January 2018. doi:10.1109/lcsys.2017.2722406.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

Let \(q = \mu/L\),


\[\alpha_{\mathrm{tm}} = \frac{2-\sqrt{q}}{L}, \qquad
\beta_{\mathrm{tm}} = \frac{(1-\sqrt{q})^2}{1+\sqrt{q}}, \qquad
\gamma_{\mathrm{tm}} = \frac{(1-\sqrt{q})^2}{(2-\sqrt{q})(1+\sqrt{q})}.\]

For initial points \(x^{-1},x^0 \in \calH\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    y^k &= x^k + \gamma_{\mathrm{tm}}(x^k - x^{k-1}), \\
    x^{k+1} &= x^k + \beta_{\mathrm{tm}}(x^k - x^{k-1}) - \alpha_{\mathrm{tm}} \nabla f(y^k).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\bx^k = (x^k, x^{k-1}), \qquad
\bu^k = \nabla f(y^k), \qquad
\by^k = y^k.\]

Let \(q = \mu/L\), and define


\[\alpha_{\mathrm{tm}} = \frac{2-\sqrt{q}}{L}, \qquad
\beta_{\mathrm{tm}} = \frac{(1-\sqrt{q})^2}{1+\sqrt{q}}, \qquad
\gamma_{\mathrm{tm}} = \frac{(1-\sqrt{q})^2}{(2-\sqrt{q})(1+\sqrt{q})}.\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &=
    \begin{bmatrix}
    1+\beta_{\mathrm{tm}} & -\beta_{\mathrm{tm}} \\
    1 & 0
    \end{bmatrix}, &
    B_k &=
    \begin{bmatrix}
    -\alpha_{\mathrm{tm}} \\
    0
    \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1+\gamma_{\mathrm{tm}} & -\gamma_{\mathrm{tm}}
    \end{bmatrix}, &
    D_k &= \begin{bmatrix} 0 \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 2,\quad m = 1,\quad (\bar{m}_i)_{i=1}^{m} = (1),\quad \bar{m} = 1.\]


\[I_{\text{func}} = \{1\},\quad I_{\text{op}} = \varnothing.\]

<a id="autolyap.algorithms.TripleMomentum.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.TripleMomentum.set_L"></a>





```text
set_L( L: float, ) → None
```



Set the smoothness parameter \(L\).

Shared notation follows the class-level reference in
[`TripleMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TripleMomentum "autolyap.algorithms.TripleMomentum").

**Parameters**

- L ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(L\).

**Raises**

- ValueError: If L is not a finite real number or if \(L \le 0\).

<a id="autolyap.algorithms.TripleMomentum.set_mu"></a>





```text
set_mu( mu: float, ) → None
```



Set the strong-convexity parameter \(\mu\).

Shared notation follows the class-level reference in
[`TripleMomentum`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TripleMomentum "autolyap.algorithms.TripleMomentum").

**Parameters**

- mu ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\mu\).

**Raises**

- ValueError: If mu is not a finite real number or if \(\mu \le 0\).

<a id="autolyap.algorithms.TsengFBF"></a>





```text
class autolyap.algorithms.TsengFBF( gamma: float, theta: float, )
```



Bases: [`Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm")

Tseng’s forward-backward-forward method <a id="id1"></a>[[Tse00](https://autolyap.github.io/concrete_algorithms/#id201 "Paul Tseng. A modified forward-backward splitting method for maximal monotone mappings. SIAM Journal on Control and Optimization, 38(2):431–446, January 2000. doi:10.1137/s0363012998338806.")].

See [3. Algorithm representation](https://autolyap.github.io/theory/algorithm_representation/)
for mathematical notation and definitions.

Notation-driven assumptions are declared by the user via
[`InclusionProblem`](https://autolyap.github.io/problem_class/#autolyap.problemclass.InclusionProblem "autolyap.problemclass.InclusionProblem"): when present, terms written with
\(\nabla\) use differentiable functions, terms written with
\(\prox_{\gamma f}\) use proper, lower semicontinuous, convex functions,
and terms written with \(J_{\gamma G}\) use maximally monotone operators.

<a id="standard-form"></a>



## Standard form

For an initial point \(x^0 \in \calH\), step size
\(\gamma \in \reals_{++}\), and relaxation parameter
\(\theta \in \reals\),


\[\begin{split}(\forall k \in \naturals)\quad
\left[
\begin{aligned}
    \bar{x}^k &= J_{\gamma G_2}(x^k - \gamma G_1(x^k)), \\
    x^{k+1} &= x^k + \theta\Big(\bar{x}^k - \gamma G_1(\bar{x}^k) - (x^k - \gamma G_1(x^k))\Big).
\end{aligned}
\right.\end{split}\]

<a id="state-space-representation"></a>



## State-space representation

The update can be written in the algorithm representation with


\[\begin{split}\begin{aligned}
    \bx^k &= x^k, \\
    \bu^k &= \left(
    G_1(x^k),\;
    G_1(\bar{x}^k),\;
    \frac{x^k-\gamma G_1(x^k)-\bar{x}^k}{\gamma}
    \right), \\
    \by^k &= (x^k, \bar{x}^k, \bar{x}^k).
\end{aligned}\end{split}\]

With this representation, the system matrices are


\[\begin{split}\begin{aligned}
    A_k &= \begin{bmatrix} 1 \end{bmatrix}, &
    B_k &= \begin{bmatrix} 0 & -\gamma\theta & -\gamma\theta \end{bmatrix}, \\
    C_k &=
    \begin{bmatrix}
    1 \\
    1 \\
    1
    \end{bmatrix}, &
    D_k &=
    \begin{bmatrix}
    0 & 0 & 0 \\
    -\gamma & 0 & -\gamma \\
    -\gamma & 0 & -\gamma
    \end{bmatrix}.
\end{aligned}\end{split}\]

These are the system matrices returned by [`get_ABCD()`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm.get_ABCD "autolyap.algorithms.Algorithm.get_ABCD").

<a id="structural-parameters"></a>



## Structural parameters


\[n = 1,\quad m = 2,\quad (\bar{m}_i)_{i=1}^{m} = (2,1),\quad \bar{m} = 3.\]


\[I_{\text{func}} = \varnothing,\quad I_{\text{op}} = \{1,2\}.\]

<a id="autolyap.algorithms.TsengFBF.get_ABCD"></a>





```text
get_ABCD( k: int, ) → Tuple[ndarray, ndarray, ndarray, ndarray]
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

<a id="autolyap.algorithms.TsengFBF.set_gamma"></a>





```text
set_gamma( gamma: float, ) → None
```



Set the step-size parameter \(\gamma\).

Shared notation follows the class-level reference in
[`TsengFBF`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TsengFBF "autolyap.algorithms.TsengFBF").

**Parameters**

- gamma ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\gamma\).

**Raises**

- ValueError: If gamma is not a finite real number or if \(\gamma \le 0\).

<a id="autolyap.algorithms.TsengFBF.set_theta"></a>





```text
set_theta( theta: float, ) → None
```



Set the relaxation parameter \(\theta\).

Shared notation follows the class-level reference in
[`TsengFBF`](https://autolyap.github.io/concrete_algorithms/#autolyap.algorithms.TsengFBF "autolyap.algorithms.TsengFBF").

**Parameters**

- theta ([`Union`](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Union "typing.Union")[[`int`](https://autolyap.github.io/dev/dev_external_reference_targets/#int "int"), `float`]): The value corresponding to \(\theta\).

**Raises**

- ValueError: If theta is not a finite real number.

<a id="references"></a>



## References

<a id="id1"></a>



<a id="id43"></a>



[[CP11](https://autolyap.github.io/concrete_algorithms/#id1)]

Antonin Chambolle and Thomas Pock. A first-order primal-dual algorithm for convex problems with applications to imaging. *Journal of Mathematical Imaging and Vision*, 40(1):120–145, 2011. [doi:10.1007/s10851-010-0251-1](https://doi.org/10.1007/s10851-010-0251-1).

<a id="id61"></a>



[[DY17](https://autolyap.github.io/concrete_algorithms/#id1)]

Damek Davis and Wotao Yin. A three-operator splitting scheme and its optimization applications. *Set-Valued and Variational Analysis*, 25(4):829–858, June 2017. [doi:10.1007/s11228-017-0421-z](https://doi.org/10.1007/s11228-017-0421-z).

<a id="id65"></a>



[[DR56](https://autolyap.github.io/concrete_algorithms/#id1)]

Jim Douglas and H. H. Rachford. On the numerical solution of heat conduction problems in two and three space variables. *Transactions of the American Mathematical Society*, 82(2):421–439, 1956. [doi:10.1090/s0002-9947-1956-0084194-4](https://doi.org/10.1090/s0002-9947-1956-0084194-4).

<a id="id71"></a>



[[EB92](https://autolyap.github.io/concrete_algorithms/#id2)]

Jonathan Eckstein and Dimitri P. Bertsekas. On the Douglas–Rachford splitting method and the proximal point algorithm for maximal monotone operators. *Mathematical Programming*, 55(1-3):293–318, April 1992. [doi:10.1007/bf01581204](https://doi.org/10.1007/bf01581204).

<a id="id108"></a>



[[Kim21](https://autolyap.github.io/concrete_algorithms/#id1)]

Donghwan Kim. Accelerated proximal point method for maximally monotone operators. *Mathematical Programming*, 190(1-2):57–87, March 2021. [doi:10.1007/s10107-021-01643-0](https://doi.org/10.1007/s10107-021-01643-0).

<a id="id107"></a>



[[KF15](https://autolyap.github.io/concrete_algorithms/#id1)]

Donghwan Kim and Jeffrey A. Fessler. Optimized first-order methods for smooth convex minimization. *Mathematical Programming*, 159(1-2):81–107, October 2015. [doi:10.1007/s10107-015-0949-3](https://doi.org/10.1007/s10107-015-0949-3).

<a id="id112"></a>



[[Kor76](https://autolyap.github.io/concrete_algorithms/#id1)]

Galina M. Korpelevich. The extragradient method for finding saddle points and other problems. *Matecon*, 12:747–756, 1976.

<a id="id117"></a>



[[LM79](https://autolyap.github.io/concrete_algorithms/#id3)]

P. L. Lions and B. Mercier. Splitting algorithms for the sum of two nonlinear operators. *SIAM Journal on Numerical Analysis*, 16(6):964–979, December 1979. [doi:10.1137/0716071](https://doi.org/10.1137/0716071).

<a id="id123"></a>



[[MT20](https://autolyap.github.io/concrete_algorithms/#id1)]

Yura Malitsky and Matthew K. Tam. A forward-backward splitting method for monotone inclusions without cocoercivity. *SIAM Journal on Optimization*, 30(2):1451–1472, January 2020. [doi:10.1137/18m1207260](https://doi.org/10.1137/18m1207260).

<a id="id168"></a>



[[Mar70](https://autolyap.github.io/concrete_algorithms/#id2)]

B. Martinet. Brève communication. Régularisation d'inéquations variationnelles par approximations successives. *Revue française d'informatique et de recherche opérationnelle. Série rouge*, 4(R3):154–158, 1970. [doi:10.1051/m2an/197004R301541](https://doi.org/10.1051/m2an/197004R301541).

<a id="id169"></a>



[[Mor65](https://autolyap.github.io/concrete_algorithms/#id1)]

Jean-Jacques Moreau. Proximité et dualité dans un espace hilbertien. *Bulletin de la Société Mathématique de France*, 93:273–299, 1965. [doi:10.24033/bsmf.1625](https://doi.org/10.24033/bsmf.1625).

<a id="id144"></a>



[[Nes83](https://autolyap.github.io/concrete_algorithms/#id1)]

Yurii Nesterov. A method for solving the convex programming problem with convergence rate o(1/k^2). *Doklady Akademii Nauk SSSR*, 269(3):543–547, 1983.

<a id="id146"></a>



[Nes18]
([1](https://autolyap.github.io/concrete_algorithms/#id1),[2](https://autolyap.github.io/concrete_algorithms/#id1))

Yurii Nesterov. *Lectures on Convex Optimization*. Springer International Publishing, 2018. ISBN 9783319915784. [doi:10.1007/978-3-319-91578-4](https://doi.org/10.1007/978-3-319-91578-4).

<a id="id162"></a>



[[Pol64](https://autolyap.github.io/concrete_algorithms/#id1)]

B. T. Polyak. Some methods of speeding up the convergence of iteration methods. *USSR Computational Mathematics and Mathematical Physics*, 4(5):1–17, 1964. [doi:10.1016/0041-5553(64)90137-5](https://doi.org/10.1016/0041-5553(64)90137-5).

<a id="id167"></a>



[[Roc76](https://autolyap.github.io/concrete_algorithms/#id3)]

R. Tyrrell Rockafellar. Monotone operators and the proximal point algorithm. *SIAM Journal on Control and Optimization*, 14(5):877–898, 1976. [doi:10.1137/0314056](https://doi.org/10.1137/0314056).

<a id="id194"></a>



[[TD23](https://autolyap.github.io/concrete_algorithms/#id1)]

Adrien Taylor and Yoel Drori. An optimal gradient method for smooth strongly convex minimization. *Mathematical Programming*, 199(1-2):557–594, 2023. [doi:10.1007/s10107-022-01839-y](https://doi.org/10.1007/s10107-022-01839-y).

<a id="id201"></a>



[[Tse00](https://autolyap.github.io/concrete_algorithms/#id1)]

Paul Tseng. A modified forward-backward splitting method for maximal monotone mappings. *SIAM Journal on Control and Optimization*, 38(2):431–446, January 2000. [doi:10.1137/s0363012998338806](https://doi.org/10.1137/s0363012998338806).

<a id="id207"></a>



[[VSFL18](https://autolyap.github.io/concrete_algorithms/#id1)]

Bryan Van Scoy, Randy A. Freeman, and Kevin M. Lynch. The fastest known globally convergent first-order method for minimizing strongly convex functions. *IEEE Control Systems Letters*, 2(1):49–54, January 2018. [doi:10.1109/lcsys.2017.2722406](https://doi.org/10.1109/lcsys.2017.2722406).

TeX macro definitions used in this page:

```latex
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\calH}{\mathcal{H}}
\newcommand{\llbracket}{\lbrack\!\lbrack}
\newcommand{\naturals}{\mathbb{N}_{0}}
\newcommand{\prox}{{\rm{prox}}}
\newcommand{\reals}{\mathbb{R}}
\newcommand{\rrbracket}{\rbrack\!\rbrack}
```
