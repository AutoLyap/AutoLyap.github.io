Canonical HTML: https://autolyap.github.io/release_notes/v0_2_0/
Documentation version: 0.2.1

<a id="v0-2-0"></a>



# v0.2.0

AutoLyap 0.2.0 focuses on clearer Lyapunov APIs, broader solver choices, and
much stronger user documentation.
Status: Published on 2026-03-02.

<a id="highlights"></a>



## Highlights

- Added top-level [`SolverOptions`](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions "autolyap.SolverOptions") for
  explicit backend configuration in analysis calls.
- Added first-class CVXPY backend support and solver profiles
  (`CLARABEL`, `SCS`, `MOSEK`, `SDPA`, `SDPA` multiprecision, `COPT`) alongside
  MOSEK Fusion.
- Updated Lyapunov APIs to return structured result dictionaries with
  certificates (`status`, `solve_status`, `rho`/`c_K`, `certificate`).
- Added `verbosity` controls (default `verbosity=1`) and concise runtime
  diagnostics for certificate checks.
- Expanded docs with a full [Quick start](https://autolyap.github.io/quick_start/), a dedicated
  [Theory](https://autolyap.github.io/theory/) section, and many new worked examples (proximal/gradient,
  heavy-ball/Nesterov, Douglas-Rachford, Chambolle-Pock, Davis-Yin, Malitsky-Tam
  FRB, APP, and ITEM).
- Added project citation metadata (`CITATION.cff`) and richer reference wiring.

<a id="breaking-changes"></a>



## Breaking changes

- Interpolation index tokens were renamed:

  - `i<j` -> `r1<r2`
  - `i!=j` -> `r1!=r2`
  - `i` -> `r1`
  - `i!=star` -> `r1!=star`
- Lyapunov entry points were renamed:

  - `IterationIndependent.verify_iteration_independent_Lyapunov` ->
    `IterationIndependent.search_lyapunov`
  - `IterationDependent.verify_iteration_dependent_Lyapunov` ->
    `IterationDependent.search_lyapunov`
- Deprecated Lyapunov alias methods were removed:

  - `IterationIndependent.verify_iteration_independent_Lyapunov`
  - `IterationDependent.verify_iteration_dependent_Lyapunov`
- Lyapunov API return types changed from scalars/booleans to result
  dictionaries:

  - [`autolyap.IterationIndependent.search_lyapunov()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.search_lyapunov "autolyap.IterationIndependent.search_lyapunov")
    now returns `{"status", "solve_status", "rho", "certificate"}`.
  - [`autolyap.IterationIndependent.LinearConvergence.bisection_search_rho()`](https://autolyap.github.io/iteration_independent_analysis/#autolyap.IterationIndependent.LinearConvergence.bisection_search_rho "autolyap.IterationIndependent.LinearConvergence.bisection_search_rho")
    now returns `{"status", "solve_status", "rho", "certificate"}`.
  - [`autolyap.IterationDependent.search_lyapunov()`](https://autolyap.github.io/iteration_dependent_analysis/#autolyap.IterationDependent.search_lyapunov "autolyap.IterationDependent.search_lyapunov")
    now returns `{"status", "solve_status", "c_K", "certificate"}`.
- Legacy `success` booleans were removed. Use `status == "feasible"` to
  detect certified success.
- Result-status semantics are now explicit:

  - `status="feasible"` when a certificate is returned.
  - `status="infeasible"` for genuine infeasibility.
  - `status="not_solved"` for solver/interface errors or indeterminate
    backend statuses (with details in `solve_status`).
- Removed `autolyap.algorithms.deterministic_proxskip` (`ProxSkip`).

<a id="migration-from-0-1-0"></a>



## Migration from 0.1.0

1. Replace old interpolation keys with the new `r1*` forms.
2. Rename deprecated Lyapunov entry points to `search_lyapunov`.
3. Update result handling to dictionary outputs; treat only
   `status == "feasible"` as certified success.
4. Configure solver backend/solver explicitly with
   [`SolverOptions`](https://autolyap.github.io/solver_backends/#autolyap.SolverOptions "autolyap.SolverOptions") when needed.
5. If you imported `ProxSkip`, remove or replace that dependency.

<a id="see-also"></a>



## See also

- [Quick start](https://autolyap.github.io/quick_start/)
- [Examples](https://autolyap.github.io/examples/)
- [API reference](https://autolyap.github.io/api_reference/)
- [Solver backends](https://autolyap.github.io/solver_backends/)
- [Full changelog](https://github.com/AutoLyap/AutoLyap/blob/main/CHANGELOG.md)
