Canonical HTML: https://autolyap.github.io/
Documentation version: 0.2.1

<a id="autolyap"></a>



# AutoLyap

A Python package for automated Lyapunov-based convergence analyses of first-order optimization and inclusion methods.

[![PyPI version](https://img.shields.io/pypi/v/autolyap?style=flat&logo=pypi&logoColor=white&label=PyPI&labelColor=111111&color=2570ae)](https://pypi.org/project/autolyap/)
[![PyPI downloads](https://img.shields.io/pepy/dt/autolyap?style=flat&label=downloads&labelColor=111111&color=2570ae)](https://pepy.tech/projects/autolyap)
[![GitHub stars](https://img.shields.io/github/stars/AutoLyap/AutoLyap?style=flat&logo=github&logoColor=white&label=stars&labelColor=111111&color=2570ae)](https://github.com/AutoLyap/AutoLyap/stargazers)
[![Paper](https://img.shields.io/badge/paper-arXiv%3A2506.24076-2570ae?style=flat&logo=arxiv&logoColor=white&labelColor=111111)](https://arxiv.org/abs/2506.24076)

---

<a id="overview"></a>



## Overview

AutoLyap streamlines the process of constructing and verifying Lyapunov analyses by formulating them as semidefinite programs (SDPs). It supports a broad class of structured optimization and inclusion problems, providing computer-assisted proofs of linear or sublinear convergence rates for many well‑known algorithms.

A typical workflow:

1. Choose the class of optimization/inclusion problems.
2. Choose the first-order method to analyze.
3. Choose the type of Lyapunov analysis to search for or verify (which implies a convergence or performance conclusion).

AutoLyap builds the underlying SDP and solves it through configurable backend
solvers.

<a id="quick-start"></a>



## Quick start

For installation instructions and first end-to-end workflows, see
[Quick start](https://autolyap.github.io/quick_start/).

<a id="cite-this-project"></a>



## Cite this project

If AutoLyap contributes to your research or software, please cite
<a id="id1"></a>[[UDGT+26](https://autolyap.github.io/#id203 "Manu Upadhyaya, Shuvomoy Das Gupta, Adrien B. Taylor, Sebastian Banert, and Pontus Giselsson. The AutoLyap software suite for computer-assisted Lyapunov analyses of first-order methods. 2026. arXiv:2506.24076.")].



```bibtex
@misc{upadhyaya2026autolyap,
  author = {Upadhyaya, Manu and Das Gupta, Shuvomoy and Taylor, Adrien B. and Banert, Sebastian and Giselsson, Pontus},
  title = {The {AutoLyap} software suite for computer-assisted {L}yapunov analyses of first-order methods},
  year = {2026},
  archivePrefix = {arXiv},
  eprint = {2506.24076},
  primaryClass = {math.OC},
}
```



<a id="id2"></a>



<a id="id203"></a>



[[UDGT+26](https://autolyap.github.io/#id1)]

Manu Upadhyaya, Shuvomoy Das Gupta, Adrien B. Taylor, Sebastian Banert, and Pontus Giselsson. The AutoLyap software suite for computer-assisted Lyapunov analyses of first-order methods. 2026. [arXiv:2506.24076](https://arxiv.org/abs/2506.24076).

<a id="other-computer-assisted-methodologies"></a>



## Other computer-assisted methodologies

[PEPit](https://pepit.readthedocs.io) is a computer-assisted performance estimation framework that targets worst-case analyses of first-order methods through SDP formulations. AutoLyap is complementary: it focuses on Lyapunov analyses and automates the corresponding SDP formulations. In practice, PEPit is a strong choice for tight bounds, while AutoLyap is tailored to Lyapunov-based proofs and scalable analysis patterns.

<a id="contributors"></a>



## Contributors

- [Manu Upadhyaya](https://manuupadhyaya.github.io/): Creator and maintainer.
- [Shuvomoy Das Gupta](https://shuvomoy.github.io/): Creator; maintainer of [AutoLyap.jl](https://github.com/AutoLyap/AutoLyap.jl) (Julia version of AutoLyap).
- [Adrien B. Taylor](https://adrientaylor.github.io/): Creator.
- [Sebastian Banert](https://github.com/sbanert): Creator.
- [Pontus Giselsson](https://portal.research.lu.se/en/persons/pontus-giselsson/): Creator.
