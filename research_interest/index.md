# Research Interest

I explain what Kitaoka has researched so far.
(as of October 25, 2026)

## Main research themes so far

### Inverse optimization for mixed-integer programming
- We are developing fast algorithms that learn the weights of the objective function and the constraints of (mixed-integer) linear programs such that the given data are optimal solutions.
- <a href="{{ '/research_interest/inverse_optimization' | relative_url }}">Details here</a>
- References (expository)
    - [P20] Akira Kitaoka, Inverse Mixed-Integer Programming: Learning of Constraints then Objective Functions (Japanese), [JSAI2026](https://pub.confit.atlas.jp/ja/event/jsai2026/presentation/5H2-OS-18a-02), \[5H2-OS-18a\], June 12, 2026. \[[Paper](https://www.jstage.jst.go.jp/article/pjsai/JSAI2026/0/JSAI2026_5H2OS18a02/_article/-char/ja/)\]
    - [P9] Akira Kitaoka, Riki Eto, Equivalence of Suboptimality Loss and Prediction Loss in Inverse Problems of Mixed Integer Linear Programming (Japanese), [JSAI2024](https://confit.atlas.jp/guide/event/jsai2024/subject/1F3-GS-1-02/date?cryptoId=), Hamamatsu, May 28, 2024.
- References (papers)
    - [P21] Akira Kitaoka, Explicit Iteration Complexity of Exact Data-Driven Inverse Optimization for Integer Linear Programs, preprint.
[[arXiv:2607.22263](https://arxiv.org/abs/2607.22263)]
    - [P13] Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]
    - [P8] Akira Kitaoka, Exact Solution to Data-Driven Inverse Optimization of MILPs in Finite Time via Gradient-Based Methods, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]
    - [P6] Akira Kitaoka, Riki Eto, A proof of imitation of Wasserstein inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.10089](https://arxiv.org/abs/2305.10089)]
    - [P5] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

### Outperformance inverse optimization
- I am researching methods that learn the weights of an objective function whose optimal solutions outperform the observed actions of an agent in every component, rather than reproducing those actions.
- I designed a loss function that can be computed using only solvers of the forward problem, and proposed a gradient-based method and a DC optimization method applicable to mixed-integer programs. I also showed that the generalization error decreases in inverse proportion to the number of observations.
- References (papers)
    - [P23] Akira Kitaoka, Outperformance Inverse Optimization: Learning Objective Functions that Outperform Agent Decisions, preprint.
[[arXiv:2610.09890](https://arxiv.org/abs/2610.09890)]

### Online inverse optimization
- We research methods that learn the weights of a linear objective function from the optimal actions of an agent observed sequentially.
- We proposed methods whose number of mistakes and regret do not depend on the total number of rounds when the forward problem is an integer linear program (ILP).
- <a href="{{ '/research_interest/online_inverse_optimization' | relative_url }}">Details here</a>
- References (papers)
    - [P22] Akira Kitaoka, Online Inverse Integer Linear Optimization via Small-Gradient Skipping: Constant Regret and Finite Mistakes, preprint.
[[arXiv:2609.09809](https://arxiv.org/abs/2609.09809)]

### Counterfactual explanation, Algorithmic recourse
- We research counterfactual explanations, i.e., AIs that explain “how to modify the current input to obtain the desired output”.
- Among counterfactual explanations, we research methods that present actions as paths.
- <a href="{{ '/research_interest/counterfactual_explanation' | relative_url }}">Details here</a>
- References (expository)
    - [P11] Akira Kitaoka, Yuzuru Okajima, Yoichi Sasaki, Rin Takano, Counterfactual Explanation Based on Paths via Curve Energy Minimization (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/1Win4-97/advanced), \[1Win4-97\], May 27, 2025. \[[Paper](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_1Win497/_article/-char/ja/)\] \[[Errata & Comment](https://akira5kitaoka.github.io/papers/JSAI2025_energy_minimization_CE/JSAI_energy_minimization_CE_errata.pdf)\]

### Learning from averaged data
- Data aggregation techniques, including data averaging, are useful for data summarization, compression, reducing annotation costs, improving data quality, and protecting privacy.
- We were researching methods for machine learning from averaged data.
- <a href="{{ '/research_interest/averaged_data' | relative_url }}">Details here</a>
- References (expository)
    - [P12] Ryuta Matsuno, Akira Kitaoka, Keita Sakuma, Masakazu Hirokawa, Learning Nonlinear Regression Model from Two-Sample Averaged Data (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/2L4-GS-1-05/advanced), \[2L4-GS-1-05\], May 28, 2025. \[[Paper](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_2L4GS105/_article/-char/ja/)\]
- References (papers)
    - [P19] Ryuta Matsuno, Akira Kitaoka, Learning from Two-Sample Averaged Data, [ICDM 2025](https://www3.cs.stonybrook.edu/~icdm2025/index.html), November 12, 2025. [[Paper](https://ieeexplore.ieee.org/document/11391798)] [[Video presentation](https://video.computer.org/ICDM25/10ME7GxDtJrOktMO8JeVyl-DM531-Matsuno.mp4.html)]

### Algorithm of approximating minimal geodesics
- I was conducting research on error estimates for length minimization through energy minimization of curves in real coordinate space using finite difference methods and numerical integration.
- <a href="{{ '/research_interest/geodesic' | relative_url }}">Details here</a>
- References (papers)
    - [P10] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

### Invariants of parabolic geometry with BGG complexes
- I was conducting research on finding geometric constraints by defining invariants and notions defined via the de Rham complex in the same way for the Bernstein-Gelfand-Gelfand complex (BGG complex), which appears in contact geometry and parabolic geometry, and clarifying the relationship between the two.
- <a href="{{ '/research_interest/bgg_complex' | relative_url }}">Details here</a>
- References (expository)
    - [P7] Akira Kitaoka, Ray-Singer torsion and the Rumin Laplacian on lens spaces, [Geometric Structures and Differential Equations - Symmetry, Singularity, and Quantization - ](http://www.math.ritsumei.ac.jp/~dtarama/GSDE2022/index.html), online, Nov 2023. [[Proceedings](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/pdf/2268-10.pdf)]
- References (papers)
    - [P4] Akira Kitaoka, Harmonic forms and the Rumin complex on Sasakian manifolds, [Osaka Journal of Mathematics](http://www.math.sci.osaka-u.ac.jp/ojm/), to appear.
[[arXiv:2204.03446](https://arxiv.org/abs/2204.03446)]
    - [P3] Akira Kitaoka, Ray-Singer Torsion and the Rumin Laplacian on lens spaces, Symmetry, Integrability and Geometry: Methods and Applications (SIGMA) 18 (2022), 091, 16 pages.
[[article link](https://doi.org/10.3842/SIGMA.2022.091)][[arXiv:2009.03276](https://arxiv.org/abs/2009.03276)]
    - [P1] Akira Kitaoka, Analytic torsions associated with the Rumin complex on contact spheres, International Journal of Mathematics 31 (2020), no. 13, 2050112.
[[article link](https://www.worldscientific.com/doi/10.1142/S0129167X20501128)][[arXiv:1911.03092](https://arxiv.org/abs/1911.03092)]


## Research tidbits
Small topics from my own research.

### Characterization of piecewise linear maps as compositions of min, max, and affine maps
- Piecewise linear maps (PL maps) are basic functions that appear in optimization problems, machine learning, statistics, control theory, topology, and so on. Here, as a characterization of piecewise linear maps, I show that they are given by compositions of min, max, and affine maps.
- [Link (Japanese)](https://zenn.dev/akira5kitaoka/articles/216c252af23dce)

### Dimension of the solution space of linear ordinary differential equations with analytic coefficients
- It is known that when the leading coefficient of a linear ordinary differential equation does not vanish at any point, the dimension of the solution space equals the order of the equation. What happens to the dimension when the leading coefficient has zeros? Here, I show that when the coefficients are real analytic, the difference between the dimension of the solution space of a linear ordinary differential equation and its order can be estimated by the orders of the zeros of the leading coefficient.
- [Link (Japanese)](https://zenn.dev/akira5kitaoka/articles/8f2a74a55047b7)
