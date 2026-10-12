Canonical HTML: https://autolyap.github.io/contributing/development_workflow/
Documentation version: 0.2.1

<a id="development-workflow"></a>



# Development workflow

<a id="keep-your-branch-current"></a>



## Keep your branch current

Rebase your branch onto `upstream/main` before opening or updating a PR:



```bash
git fetch upstream
git rebase upstream/main
```



<a id="run-local-checks-before-opening-a-pr"></a>



## Run local checks before opening a PR

Run the one-command local CI helper:



```bash
make check
```



If `make check` reports missing lint/typecheck tools, install or refresh test extras:



```bash
python -m pip install -e '.[test]'
```



The CI test matrix runs on Python `3.9`, `3.10`, `3.11`, `3.12`, and `3.13`.
At minimum, run the checks above locally on one supported Python version.

If your change may impact MOSEK-backed functionality, run:



```bash
make check-mosek
```



<a id="documentation-changes"></a>



## Documentation changes

- Keep docstrings, docs, and mathematical notation consistent with existing structure and style.
- To build the documentation locally:



```bash
make -C docs deps
make -C docs dirhtml
```



The generated site is written to `docs/build/dirhtml/` (open `docs/build/dirhtml/index.html`).
