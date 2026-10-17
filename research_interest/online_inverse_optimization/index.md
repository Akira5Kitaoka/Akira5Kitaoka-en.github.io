<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

Last updated: October 17, 2026

# Online inverse optimization

**Keywords**: Online inverse linear optimization, inverse optimization, online learning, regret analysis, integer linear programming

**Research summary**: We studied methods that learn the weights of a linear objective function in an online setting where states arrive sequentially, using only the optimal actions of an agent as clues. In particular, we proposed methods whose number of mistakes and regret do not depend on the total number of rounds $T$ when the forward problem is an integer linear program (ILP) [[P22](#K22)].

**Background**: The problem of estimating, from observed decisions, the criterion (objective function) by which a decision-maker chooses actions has been studied as inverse optimization and inverse reinforcement learning. The estimated weights can be interpreted as an objective function representing the reasons behind decisions, with applications such as estimating customer preferences and estimating undisclosed objective functions in electricity markets and healthcare. In many real situations, decision data are not obtained all at once but observed sequentially, like responses to recommendations or daily plans. In such cases, we need to keep making good proposals while updating the estimate at every observation, without waiting for the data to be collected. Online inverse optimization (also called contextual recommendation) formalizes this, and the question is how small the cumulative loss incurred during learning (the regret) can be kept. (cf. [[P22](#K22), [Oki and Sakaue 2026](#oki2026finite)])

## What is online inverse optimization?

Inverse optimization is the problem of estimating an objective function from observed optimal solutions (cf. [Inverse optimization for mixed-integer programming]({{ '/research_interest/inverse_optimization' | relative_url }})). In online inverse optimization, data are not given all at once but arrive one by one.

Consider two players, a learner and an agent. The agent has unknown weights $\theta^* \in \mathbb{R}^d$ and, given a state $s$, chooses as its action an optimal solution with $\theta = \theta^*$ of the forward problem

$$
x^*(\theta, s) \in \mathrm{argmax}_{x \in X(s)} \langle \theta, x \rangle .
$$

Here $X(s) \subset \mathbb{R}^d$ is the set of feasible actions in state $s$, which need not be convex (e.g., the feasible set of an integer linear program). In each round $t = 1, 2, \ldots, T$, the following is repeated.

1. A state $s^t$ (action set $X(s^t)$) is presented.
2. The learner predicts weights $\hat{\theta}^t$ from past observations and proposes the action $\hat{x}^t \in \mathrm{argmax}_{x \in X(s^t)} \langle \hat{\theta}^t, x \rangle$ based on them.
3. The agent's optimal action $x^t = x^*(\theta^*, s^t)$ is observed. The value of the objective function itself is not observed.

For example, in recommendation (contextual recommendation), the learner makes a recommendation from each set of candidates, observes only what the user actually chose (the user's optimal choice), and learns the weights of the user's preferences. In scheduling, the learner observes the duty roster $x^t$ made by an expert for each day's situation $s^t$ and learns the weights of the metrics the expert values.

The performance of the learner is mainly measured by the following quantities.

- **Cumulative decision regret** (often simply called the regret): the cumulative shortfall of the objective value of the proposals, measured with the true weights,
$$
R^{\mathrm{est}}_T := \sum_{t=1}^T \langle \theta^*, x^t - \hat{x}^t \rangle .
$$
- **Cumulative suboptimality regret**: the cumulative amount by which the agent's actions are not explained from the viewpoint of the predicted weights $\hat{\theta}^t$,
$$
R^{\mathrm{sub}}_T := \sum_{t=1}^T \langle \hat{\theta}^t, \hat{x}^t - x^t \rangle .
$$
- **Number of mistakes** $K$: the number of rounds in which the proposal failed to explain the agent's action (the suboptimality loss was positive).

The central questions in this field are how the regret grows with $T$, and, if there is an upper bound independent of $T$, how it depends on the dimension $d$.

## <a id="S1">1</a>. $T$-independent regret and finitely many mistakes via small-gradient skipping [[P22](#K22)]

### Overview of [[P22](#K22)]

||Details|
|---|---|
|Background|There is a framework that applies online learning methods (OGD, ONS, MetaGrad) to the suboptimality loss, with regret $O(\sqrt{T})$ for OGD and $O(d \log T)$ for second-order methods.|
|Issue|Existing $T$-independent upper bounds had limitations: exponential in the dimension ($\exp(O(d\log d))$), proportional to the inverse square of a gap, or requiring the computation of a centroid in every round. Whether guarantees that are independent of $T$ and polynomial in the dimension can be obtained with light computation for general forward problems, including integer programming, was open.|
|Assumption|**Uniform margin**: under the true weights, the difference in objective value between the correct action and any other candidate action is at least some $\gamma > 0$, uniformly over all states. If the forward problem is an integer linear program, the margin is automatically positive.|
|Proposed method|We incorporate **small-gradient skipping** (SGS), which updates neither the iterate nor the internal state in rounds without mistakes (rounds where $0$ can be chosen as a subgradient), into OGD, ONS, and MetaGrad.|
|Theoretical result 1|For all three methods, the number of mistakes $K$, $R^{\mathrm{sub}}_T$, and $R^{\mathrm{est}}_T$ are bounded by quantities independent of $T$ (deterministic guarantees).|
|Theoretical result 2|We explicitly bound the margin $\gamma$ from below using the combinatorial structure of the feasible set (general ILPs, linear inequalities via Graver bases, total unimodularity, M-convexity, M$^\natural$-convexity).|
|Theoretical result 3|Substituting these, we obtain explicit upper bounds for each problem class. In particular, for ILPs with the probability simplex as the weight space, ONS achieves $R^{\mathrm{est}}_T = O(d^2 \lVert M\rVert_2 \log(2\lVert M\rVert_2))$ with light computation: $O(d^2)$ and one projection per mistake round.|

Here $M_i := \sup_{s} \left( \max_{x \in X(s)} x_i - \min_{x \in X(s)} x_i \right)$ is the coordinate-wise range of the actions, and $M = (M_1, \ldots, M_d)$.

### How to read the comparison

The tables below are arranged chronologically from top to bottom (by first appearance on arXiv). Bold entries are our work [[P22](#K22)]. "Finite mistakes" means that the number of mistakes $K$ (the number of rounds in which the proposal failed to explain the agent's action) is bounded by a quantity independent of $T$. "—" means that this article has not checked the claim of the corresponding paper.

Since the regret values depend on the normalization, we compare them separately in two settings: (A) the normalization in which the Euclidean norms of the utility vector and the actions are at most $1$, and (B) the case where the weight space is the probability simplex $\Theta = \Delta^{d-1}$. Furthermore, in (C) we compare results under structures of the feasible set such as integer programming.

### Table 1 (A): Comparison of regret under the normalization with Euclidean norms at most $1$

The case where both the utility vector $\theta^*$ and the actions $x$ lie in the $d$-dimensional unit ball (the same normalization as Table 1 of [[Sakaue 2026b](#sakaue2026tight)]).

|Method|$R^{\mathrm{est}}_T$|Additional assumption|Proper|Finite mistakes|Computation per round|
|---|---|---|---|---|---|
|OGD [[Bärmann et al. 2018](#barmann2018online)]|$O(\sqrt{T})$|None|Yes|—|One projection|
|ProjectedCones [[Besbes et al. 2021, 2025](#besbes2025contextual)]|$O(d^4 \log T)$|None|Yes|—|—|
|Cutting-plane method [[Gollapudi et al. 2021](#gollapudi2021contextual)]|$O(d \log T)$|None|Yes|—|Computing a centroid|
|Cutting-plane method [[Gollapudi et al. 2021](#gollapudi2021contextual)]|$\exp(O(d \log d))$|None|Yes|—|Computing a John ellipsoid|
|ONS [[Sakaue et al. 2025b](#sakaue2025online)]|$O(d \log T)$|None|Yes|—|$O(d^2)$ + one projection|
|Second-order perceptron (CoRectron) [[Sakaue 2026a](#sakaue2026simple)]|$O(d \log T)$|None|Yes|—|One linear optimization + $O(d^2)$ (no projection)|
|Multiscale reward hedging [[Dewasurendra 2026](#dewasurendra2026)]|$O(d)$|None|No|—|In general, $T^{\Theta(d)}$ proxies (maximizing a nonconvex vote)|
|**SGS-ONS, SGS-MetaGrad [[P22](#K22)]**|$O\left(d \log \left(2 + \frac{1}{\gamma}\right)\right)$ *1|Uniform margin $\gamma>0$|Yes|**Yes**|$O(d^2)$ + one projection, only in mistake rounds|
|Variable-metric algorithm [[Cai et al. 2026](#cai2026efficient)]|$O(d)$|None|Yes|—|One linear optimization + $O(d^2)$|
|Multiscale matrix weights [[Sakaue 2026b](#sakaue2026tight)]|$O(\sqrt{d})$ (in expectation)|None|No|—|$(dT)^{O(d)}$ linear optimizations|
|[[Gupta et al. 2026](#gupta2026optimal)]|$O(\sqrt{d})$|None|Yes|—|One linear optimization + $O(td^2+t^2d)$ arithmetic operations (round $t$, deterministic)|
|Lower bound [[Sakaue et al. 2025b](#sakaue2025online), [Sakaue 2026b](#sakaue2026tight)]|$\Omega(\sqrt{d})$ ($T \geq d$)|—|—|—|—|

Note: A proper learner fixes a nonzero estimate of the utility before observing the current action set and recommends a maximizer of that estimate (the usage in [[Sakaue 2026b](#sakaue2026tight)]). [[Sakaue 2026a](#sakaue2026simple)] calls its method "improper" in the sense that the scale of the estimate is not restricted, but it is a proper learner under this definition.

*1 Obtained by substituting $L, D \leq 2$ under this normalization into the ONS bound of [[P22](#K22)], $R^{\mathrm{est}}_T \le LD\left(1 + 2d \log\left(2 + \frac{2LD}{\gamma}\right)\right)$, where $D$ is the diameter of the weight space and $L := \sup_{s} \sup_{x \in X(s)} \lVert x - x^*(\theta^*, s) \rVert$ is the spread of the actions.

### Table 1 (B): Comparison of regret when the weight space is the probability simplex $\Theta = \Delta^{d-1}$

We compare studies that allow general bounded closed convex sets, including the probability simplex, as the weight space $\Theta$. $L$ and $D$ are the same constants as in *1.

|Method|$R^{\mathrm{sub}}_T$|$R^{\mathrm{est}}_T$|Additional assumption|Finite mistakes|
|---|---|---|---|---|
|OGD [[Bärmann et al. 2018](#barmann2018online)]|$O(LD\sqrt{T})$|$O(LD\sqrt{T})$|None|—|
|FTRL [[Sakaue et al. 2025a](#sakaue2025aistats)]|$O(1/\Delta^2)$|$O(1/\Delta^2)$|Gap condition $\Delta > 0$|—|
|ONS, MetaGrad [[Sakaue et al. 2025b](#sakaue2025online)]|$O\left(LD\, d \log \frac{T}{d}\right)$|$O\left(LD\, d \log \frac{T}{d}\right)$|None|—|
|**SGS-OGD [[P22](#K22)]**|$\dfrac{L^2D^2}{2\gamma}$|$\dfrac{2L^2D^2}{\gamma}$|Uniform margin $\gamma>0$|**Yes**|
|**SGS-ONS [[P22](#K22)]**|$LD\left(1 + d \log \max\left(\frac{LD}{\gamma}, 1\right)\right)$|$LD\left(1 + 2d \log\left(2 + \frac{2LD}{\gamma}\right)\right)$|Uniform margin $\gamma>0$|**Yes**|

The constants of [[Sakaue et al. 2025a](#sakaue2025aistats)] depend on the regularizer and the sizes of $\Theta$ and $X(s)$.

### Table 1 (C): Regret and number of mistakes under structures such as integer programming

We compare results when the forward problem is an integer linear program (ILP) or the action set is M-convex. The rows of [[P22](#K22)] take $\Theta = \Delta^{d-1}$ and show the bounds of ONS (growing-grid SGS-MetaGrad has the same order). Here $M_i := \sup_{s} \left( \max_{x \in X(s)} x_i - \min_{x \in X(s)} x_i \right)$ is the coordinate-wise range of the actions, $M = (M_1, \ldots, M_d)$, and $C_g$ is the $\ell_\infty$-norm of the Graver basis of the slack-augmented matrix ($C_g = 1$ if totally unimodular).

|Method|Feasible set|Lower bound on the margin $\gamma$|$R^{\mathrm{est}}_T$|Number of mistakes $K$|Computation per round|
|---|---|---|---|---|---|
|Centroid method [[Oki and Sakaue 2026](#oki2026finite)]|M-convex|(margin not used)|$O(d \log d)$ *2|Not claimed|Computing a centroid (\#P-hard to compute exactly)|
|**[[P22](#K22)]**|General ILP|$\dfrac{1}{2^{d-1}\max(d-1,\sqrt{2})\,\lVert M\rVert_2^{d-1}}$|$O\left(d^2 \lVert M\rVert_2 \log(2\lVert M\rVert_2)\right)$|$O\left(d^{3} 2^{d} \lVert M\rVert_2^{d} \log (2\lVert M\rVert_2)\right)$|$O(d^2)$ + one projection, only in mistake rounds|
|**[[P22](#K22)]**|Linear inequalities ($C_g$)|$\dfrac{1}{\max(d-1,\sqrt{2})\,(2C_g\sqrt{d})^{d-1}}$|$O\left(L d^2 \log(2C_g dL)\right)$|$O\left(L d^{3} (2C_g\sqrt{d})^{d} \log (2C_g dL)\right)$|Same as above|
|**[[P22](#K22)]**|M-convex|$\dfrac{2}{d(d-1)}$|$O\left(L d \log(2dL)\right)$|$O\left(L d^{3} \log (2dL)\right)$|Same as above|
|**[[P22](#K22)]**|M$^\natural$-convex|$\dfrac{2}{d(d+1)}$|$O\left(L d \log(2dL)\right)$|$O\left(L d^{3} \log (2dL)\right)$|Same as above|
|Reference: $T$-independent methods in Table 1 (A)|Any compact set (including ILPs)|(margin not used)|$O(d)$, $O(\sqrt{d})$, etc. (normalization of Table 1 (A))|—|See Table 1 (A)|

*2 The bound of [[Oki and Sakaue 2026](#oki2026finite)] is under the normalization in which the regret per round is $O(1)$. Removing the normalization, it becomes $O(\lVert \theta^* \rVert L\, d \log d)$ (this conversion is due to [[P22](#K22)]), which differs from the bound $O(Ld\log(2dL))$ of [[P22](#K22)] in the M-convex case only inside the logarithm. [[Oki and Sakaue 2026](#oki2026finite)] also give a polynomial-time randomized implementation with approximate centroids, but then the bound holds in expectation and the computation per round is about $O(d^6 \log d \log T)$.


As Table 1 (C) shows, **for general ILPs, [[P22](#K22)] is the only work that bounds the number of mistakes $K$ by a quantity independent of $T$**. In [[Oki and Sakaue 2026](#oki2026finite)], an estimate of the number of mistakes appears only within the proof of Theorem 4.2, and it moreover requires M-convexity of the action set.

### Subsequent improvements

After [[P22](#K22)], the $T$-independent $R^{\mathrm{est}}_T$ was improved to $O(d)$ in the setting without a margin assumption [[Cai et al. 2026](#cai2026efficient)], and further to $O(\sqrt{d})$, which matches the lower bound up to a constant factor [[Sakaue 2026b](#sakaue2026tight)]; $O(\sqrt{d})$ was also achieved by a deterministic polynomial-time method [[Gupta et al. 2026](#gupta2026optimal)]. Hence, regarding the dimension dependence of $R^{\mathrm{est}}_T$ under the normalization of Table 1 (A), the bound of [[P22](#K22)] has been improved by subsequent work.

On the other hand, in addition to $R^{\mathrm{est}}_T$, [[P22](#K22)] bounds **the number of mistakes $K$** and **the cumulative suboptimality regret $R^{\mathrm{sub}}_T$** by quantities independent of $T$, and gives explicit lower bounds on the margin for general ILPs (Table 1 (C)).

### Table 2: Main results of [[P22](#K22)]

Under a uniform margin $\gamma$, the following hold for any (possibly adaptive) sequence of states. All right-hand sides are independent of $T$.

||SGS-OGD|ONS|Growing-grid SGS-MetaGrad|
|---|---|---|---|
|$K$|$\dfrac{2L^2D^2}{\gamma^2}$|$d + \dfrac{2LD}{\gamma}\left(1 + d \log \max\left(\dfrac{2LD}{\gamma}, 1\right)\right)$|$O\left(\dfrac{dLD}{\gamma} \log \max\left(\dfrac{2LD}{\gamma}, 2\right)\right)$|
|$R^{\mathrm{sub}}_T$|$\dfrac{L^2D^2}{2\gamma}$|$LD\left(1 + d \log \max\left(\dfrac{LD}{\gamma}, 1\right)\right)$|$O\left(LD\, d \log \max\left(\dfrac{LD}{\gamma}, 1\right)\right)$|
|$R^{\mathrm{sub}}_T + R^{\mathrm{est}}_T$|$\dfrac{2L^2D^2}{\gamma}$|$LD\left(1 + 2d \log\left(2 + \dfrac{2LD}{\gamma}\right)\right)$|$O\left(LD\, d \log \max\left(\dfrac{LD}{\gamma}, 2\right)\right)$|

Since $R^{\mathrm{sub}}_T, R^{\mathrm{est}}_T \geq 0$, $R^{\mathrm{est}}_T$ also has the bound in the last row. ONS and MetaGrad depend on the margin only logarithmically.

## References

[<a id="K22">P22</a>] Akira Kitaoka, Online Inverse Integer Linear Optimization via Small-Gradient Skipping: Constant Regret and Finite Mistakes, preprint.
[[arXiv:2609.09809](https://arxiv.org/abs/2609.09809)]

[<a id="barmann2018online">Bärmann et al. 2018</a>] A. Bärmann, A. Martin, S. Pokutta, O. Schneider, An online-learning approach to inverse optimization, 2018. [[arXiv:1810.12997](https://arxiv.org/abs/1810.12997)]

[<a id="besbes2025contextual">Besbes et al. 2025</a>] O. Besbes, Y. Fonseca, I. Lobel, Contextual inverse optimization: Offline and online learning, Operations Research 73(1) (2025), 424–443.

[<a id="cai2026efficient">Cai et al. 2026</a>] Y. Cai, A. Gupta, V. Gupta, G. Guruganesh, Y. Jiang, C. Liaw, A. Mehta, R. Paes Leme, G. Velegkas, D. Wang, Efficient Online Inverse Optimization with $O(d)$ Regret, preprint, 2026. [[arXiv:2609.13440](https://arxiv.org/abs/2609.13440)]

[<a id="dewasurendra2026">Dewasurendra 2026</a>] P. Dewasurendra, Multiscale Reward Hedging from Correct Demonstrations, preprint, 2026. [[arXiv:2608.06825v1](https://arxiv.org/abs/2608.06825v1)] (Withdrawn by arXiv on September 24, 2026 (v2); this article is based on v1.)

[<a id="gollapudi2021contextual">Gollapudi et al. 2021</a>] S. Gollapudi, G. Guruganesh, K. Kollias, P. Manurangsi, R. Paes Leme, J. Schneider, Contextual recommendations and low-regret cutting-plane algorithms, Advances in Neural Information Processing Systems 34 (2021), 22498–22508.

[<a id="gupta2026optimal">Gupta et al. 2026</a>] A. Gupta, G. Guruganesh, H. Lin, V. Mirrokni, R. Paes Leme, D. P. Woodruff, Optimal and Efficient Online Inverse Optimization, preprint, 2026. [[arXiv:2610.08735](https://arxiv.org/abs/2610.08735)]

[<a id="oki2026finite">Oki and Sakaue 2026</a>] T. Oki, S. Sakaue, Finite and Corruption-Robust Regret Bounds in Online Inverse Linear Optimization under M-Convex Action Sets, preprint, 2026. [[arXiv:2602.01682](https://arxiv.org/abs/2602.01682)]

[<a id="sakaue2025aistats">Sakaue et al. 2025a</a>] S. Sakaue, H. Bao, T. Tsuchiya, Revisiting Online Learning Approach to Inverse Linear Optimization: A Fenchel–Young Loss Perspective and Gap-Dependent Regret Analysis, Proceedings of the 28th International Conference on Artificial Intelligence and Statistics (AISTATS 2025), 46–54. [[arXiv:2501.13648](https://arxiv.org/abs/2501.13648)]

[<a id="sakaue2025online">Sakaue et al. 2025b</a>] S. Sakaue, T. Tsuchiya, H. Bao, T. Oki, Online Inverse Linear Optimization: Efficient Logarithmic-Regret Algorithm, Robustness to Suboptimality, and Lower Bound, Advances in Neural Information Processing Systems 38 (2025). [[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]

[<a id="sakaue2026simple">Sakaue 2026a</a>] S. Sakaue, Simple Projection-Free Algorithm for Contextual Recommendation with Logarithmic Regret and Robustness, preprint, 2026. [[arXiv:2603.20826](https://arxiv.org/abs/2603.20826)]

[<a id="sakaue2026tight">Sakaue 2026b</a>] S. Sakaue, Tight Regret Bound for Online Inverse Linear Optimization via Multiscale Matrix Weights, preprint, 2026. [[arXiv:2609.26978](https://arxiv.org/abs/2609.26978)]

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
