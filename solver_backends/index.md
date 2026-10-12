Canonical HTML: https://autolyap.github.io/solver_backends/
Documentation version: 0.2.1

<a id="solver-backends"></a>



# Solver backends

<a id="autolyap.SolverOptions"></a>





```text
class autolyap.SolverOptions( backend: str = 'mosek_fusion', mosek_params: Mapping[str, Any] | None = None, cvxpy_solver: str | None = None, cvxpy_solver_params: Mapping[str, Any] | None = None, cvxpy_accept_inaccurate: bool = True, )
```



[str](https://autolyap.github.io/dev/dev_external_reference_targets/#str), [Any](https://autolyap.github.io/dev/dev_external_reference_targets/#typing.Any), [bool](https://autolyap.github.io/dev/dev_external_reference_targets/#bool)

Bases: `object`

Configure solver backend selection and backend-specific options.

**Used in**

- `solver_options` argument of
  [`search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov").
- `solver_options` argument of
  [`bisection_search_rho()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.bisection_search_rho "autolyap.IterationIndependent.LinearConvergence.bisection_search_rho").
- `solver_options` argument of
  [`search_lyapunov()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.search_lyapunov "autolyap.IterationDependent.search_lyapunov").

**Parameters**

`backend` (`str`, default `"mosek_fusion"`)

Selects the modeling/solver backend.
`"mosek_fusion"` builds and solves directly with MOSEK Fusion.
`"cvxpy"` builds the problem in CVXPY and solves it with
`cvxpy_solver`.

`mosek_params` (`Mapping[str, Any] | None`, default `None`)

Extra MOSEK Fusion solver options as key-value pairs.
Used only when `backend="mosek_fusion"`.
If `None`, AutoLyap applies the explicit default MOSEK profile:
`intpntCoTolPfeas=1e-8`, `intpntCoTolDfeas=1e-8`,
`intpntCoTolRelGap=1e-8`, and `intpntMaxIterations=1000`.

`cvxpy_solver` (`str | None`, default `None`)

Name of the CVXPY solver, for example `"CLARABEL"`, `"SCS"`,
`"MOSEK"`, `"SDPA"`, or `"COPT"`.
If `None`, CVXPY chooses the solver.
Used only when `backend="cvxpy"`.

`cvxpy_solver_params` (`Mapping[str, Any] | None`, default `None`)

Extra solver options for the selected CVXPY solver.
See the examples below for recommended profiles and common options.
AutoLyap applies `warm_start=True` by default.
Used only when `backend="cvxpy"`.

`cvxpy_accept_inaccurate` (`bool`, default `True`)

Controls which CVXPY statuses are accepted as successful solves.
If `True`, accept both `OPTIMAL` and `OPTIMAL_INACCURATE`.
If `False`, require `OPTIMAL`.
Used only when `backend="cvxpy"`.

**Examples**



```python
# MOSEK Fusion (explicit default profile; requires `pip install mosek`)
SolverOptions(
    backend="mosek_fusion",
    mosek_params={
        "intpntCoTolPfeas": 1e-8,  # default
        "intpntCoTolDfeas": 1e-8,  # default
        "intpntCoTolRelGap": 1e-8,  # default
        "intpntMaxIterations": 1000,  # default
    },
)

# CVXPY + CLARABEL (explicit default profile)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="CLARABEL",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "max_iter": 2000,  # default
        "tol_feas": 1e-8,  # default
        "tol_gap_abs": 1e-8,  # default
        "tol_gap_rel": 1e-8,  # default
        "warm_start": True,  # default
    },
)

# CVXPY + MOSEK (explicit default profile; requires `pip install mosek`)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="MOSEK",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "MSK_DPAR_INTPNT_CO_TOL_PFEAS": 1e-8,  # default
        "MSK_DPAR_INTPNT_CO_TOL_DFEAS": 1e-8,  # default
        "MSK_DPAR_INTPNT_CO_TOL_REL_GAP": 1e-8,  # default
        "warm_start": True,  # default
    },
)

# CVXPY + SDPA (explicit default profile; requires `pip install sdpa-python`)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="SDPA",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "maxIteration": 100,  # default
        "epsilonStar": 1e-7,  # default
        "epsilonDash": 1e-7,  # default
        "warm_start": True,  # default
    },
)

# CVXPY + SDPA multiprecision (high-precision profile; requires `pip install sdpa-multiprecision`)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="SDPA",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "maxIteration": 500,
        "epsilonStar": 1e-30,
        "epsilonDash": 1e-30,
        "mpfPrecision": 512,
        "warm_start": True,  # default
    },
)

# CVXPY + SCS (explicit default profile)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="SCS",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "eps": 1e-6,  # default
        "max_iters": 200000,  # default
        "acceleration_lookback": 0,  # default
        "warm_start": True,  # default
    },
)

# CVXPY + COPT (explicit default profile; requires `pip install coptpy`)
SolverOptions(
    backend="cvxpy",
    cvxpy_solver="COPT",
    cvxpy_accept_inaccurate=True,  # default
    cvxpy_solver_params={
        "SDPMethod": 0,  # default
        "BarIterLimit": 500,  # default
        "FeasTol": 1e-7,  # default
        "DualTol": 1e-7,  # default
        "RelGap": 1e-8,  # default
        "AbsGap": 1e-8,  # default
        "Presolve": -1,  # default
        "Scaling": -1,  # default
        "Dualize": -1,  # default
        "warm_start": True,  # default
    },
)
```
