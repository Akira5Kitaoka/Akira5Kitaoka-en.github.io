<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

Last updated: October 17, 2026

# Counterfactual explanation based on paths via curve energy minimization

**Keywords**: Counterfactual explanation, XAI (explainable AI), machine learning, numerical analysis, differential geometry

**Background**: Path-based counterfactual explanation is an explanation method that proposes a path from the input features in order to obtain a desired prediction from a trained classifier. Demand for this kind of explanation is growing in domains such as loan screening and medical checkups.

## Counterfactual explanation based on paths via curve energy minimization [[P11](#K11)] (with Yuzuru Okajima, Yoichi Sasaki, Rin Takano)

**Previous issues**: In path-based counterfactual explanation, the proposed paths tend to zigzag and are hard to interpret, and it is difficult to add sparsity.

**Proposed method**: Following [[P10](#K10)], we define a path as a solution of the energy minimization problem of curves, approximate this optimization problem by the finite difference method, and reduce it to a nonlinear programming problem.

**Impact**: In experiments, we confirm that smooth paths can be obtained and that sparsity is achieved; specifically, the number of required features is reduced to 73% of that of the existing method.

<!-- 
<details><summary>Details</summary>

Under construction
</details>
-->

## References

[<a id="K11">P11</a>] Akira Kitaoka, Yuzuru Okajima, Yoichi Sasaki, Rin Takano, Counterfactual Explanation Based on Paths via Curve Energy Minimization (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/1Win4-97/advanced), 1Win4-97, May 27, 2025. \[[Paper](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_1Win497/_article/-char/ja/)\] \[[Errata & Comment](https://akira5kitaoka.github.io/papers/JSAI2025_energy_minimization_CE/JSAI_energy_minimization_CE_errata.pdf)\]

[<a id="K10">P10</a>] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
