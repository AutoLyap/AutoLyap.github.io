Canonical HTML: https://autolyap.github.io/contributing/pull_request_process/
Documentation version: 0.2.1

<a id="pull-request-process"></a>



# Pull request process

<a id="open-a-pull-request"></a>



## Open a pull request

1. Push your branch to your fork:



```bash
git push -u origin <feature-branch>
```



2. Open a PR from your fork branch to `AutoLyap/AutoLyap:main`.

<a id="pull-request-checklist"></a>



## Pull request checklist

- [ ] Code changes are scoped and readable.
- [ ] CI-equivalent local checks pass.
- [ ] New behavior is covered by tests when applicable.
- [ ] Docs are updated when API or behavior changed.
- [ ] Docs build locally (`make -C docs dirhtml`) if docs were changed.
- [ ] No generated files from `docs/build/` are committed.
- [ ] PR description states what changed and why.
