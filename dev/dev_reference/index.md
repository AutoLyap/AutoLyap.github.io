Canonical HTML: https://autolyap.github.io/dev/dev_reference/
Documentation version: 0.2.1

<a id="developer-reference"></a>



# Developer reference

Use this page as a guide to AutoLyap internals.
It is intended for contributors implementing or refactoring core behavior.

Internal APIs can change without deprecation. For stable contracts, use the public docs first.

<a id="start-here"></a>



## Start here

1. Read [Internal problem-class modules](https://autolyap.github.io/dev/dev_internal_problemclass/) to understand interpolation-condition data contracts.
2. Read [Internal algorithm modules](https://autolyap.github.io/dev/dev_internal_algorithms/) to see how algorithm steps are lifted into matrix objects.
3. Read [Internal analysis and solver modules](https://autolyap.github.io/dev/dev_internal_core/) to follow SDP assembly and solver execution.
4. Use [Internal utility modules](https://autolyap.github.io/dev/dev_internal_utils/) as the shared helper reference across all layers.

<a id="documentation-map"></a>



## Documentation map

<a id="public-docs-stable-api"></a>



### Public docs (stable API)

| If you need to… | Go to… |
| --- | --- |
| Define interpolation-based inclusion problems | [Problem classes](https://autolyap.github.io/problem_class/) |
| Select and configure algorithms | [Algorithms](https://autolyap.github.io/algorithms/) |
| Run convergence analyses | [Lyapunov analyses](https://autolyap.github.io/lyapunov_analyses/) |
| Configure solver backends | [Solver backends](https://autolyap.github.io/solver_backends/) |

<a id="internal-docs-implementation-details"></a>



### Internal docs (implementation details)

| If you need to… | Go to… |
| --- | --- |
| Work on SDP assembly and solver option normalization | [Internal analysis and solver modules](https://autolyap.github.io/dev/dev_internal_core/) |
| Modify the internal `Algorithm` contract or matrix/projection accessors | [Internal algorithm modules](https://autolyap.github.io/dev/dev_internal_algorithms/) |
| Change interpolation-condition internals, indices, or inclusion-problem assembly | [Internal problem-class modules](https://autolyap.github.io/dev/dev_internal_problemclass/) |
| Reuse validators, matrix helpers, or backend typing protocols | [Internal utility modules](https://autolyap.github.io/dev/dev_internal_utils/) |

<a id="architecture-at-a-glance"></a>



## Architecture at a glance

| Layer | Core modules | What it owns |
| --- | --- | --- |
| Problem definitions | [`autolyap.problemclass`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass "autolyap.problemclass") | Interpolation conditions, index semantics, and component validation. |
| Algorithm lifting | `autolyap.algorithms` | Conversion of updates into lifted matrix objects (`X`, `Y`, `U`, `P`, `F`, `E`, `W`). |
| Analysis assembly | [`autolyap.iteration_independent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_independent "autolyap.iteration_independent"), [`autolyap.iteration_dependent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_dependent "autolyap.iteration_dependent") | Construction and solving of SDP certificates from problem and algorithm structure. |
| Backend normalization | [`autolyap.solver_options`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.solver_options "autolyap.solver_options") | One-pass normalization of solver options before backend-specific calls. |
| Shared utilities | [`autolyap.utils`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils "autolyap.utils") | Validation helpers, matrix helpers, and backend protocol types. |

<a id="common-extension-tasks"></a>



## Common extension tasks

| Task | Primary entry point | Keep in mind |
| --- | --- | --- |
| Add a new algorithm | [`autolyap.algorithms.algorithm.Algorithm`](https://autolyap.github.io/base_algorithms/#autolyap.algorithms.Algorithm "autolyap.algorithms.algorithm.Algorithm") | Implement `get_ABCD` consistently; the base class derives lifted matrix/projection accessors. |
| Add a new interpolation condition | [`autolyap.problemclass.base._InterpolationCondition.get_data()`](https://autolyap.github.io/dev/dev_internal_problemclass/#autolyap.problemclass.base._InterpolationCondition.get_data "autolyap.problemclass.base._InterpolationCondition.get_data") | Follow the existing [`autolyap.problemclass`](https://autolyap.github.io/dev/dev_internal_problemclass/#module-autolyap.problemclass "autolyap.problemclass") condition hierarchy. |
| Add or modify analysis workflows | [`autolyap.iteration_independent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_independent "autolyap.iteration_independent"), [`autolyap.iteration_dependent`](https://autolyap.github.io/dev/dev_internal_core/#module-autolyap.iteration_dependent "autolyap.iteration_dependent") | Preserve solver-backend parity across supported backends. |
| Add validation or backend typing logic | [`autolyap.utils.validation`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils.validation "autolyap.utils.validation"), [`autolyap.utils.backend_types`](https://autolyap.github.io/dev/dev_internal_utils/#module-autolyap.utils.backend_types "autolyap.utils.backend_types") | Keep shared checks centralized; avoid per-module duplication. |

<a id="invariants-and-common-pitfalls"></a>



## Invariants and common pitfalls

- Interpolation indices exposed to users are 1-based and strictly validated.
- Keep matrix dimensions aligned across `Y`, `P`, and interpolation blocks; shape drift usually appears as SDP assembly failures.
- Normalize solver options exactly once via [`autolyap.solver_options._normalize_solver_options()`](https://autolyap.github.io/dev/dev_internal_core/#autolyap.solver_options._normalize_solver_options "autolyap.solver_options._normalize_solver_options") before backend-specific solve calls.
- Keep numeric checks in shared validation helpers instead of duplicating them inside algorithm or condition modules.
