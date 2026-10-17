<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

Last updated: December 26, 2025

# Inverse optimization for mixed-integer programming

**Keywords**: Inverse optimization, mathematical optimization, combinatorial optimization

**Research summary**: We developed fast algorithms that estimate the weights of the objective function and the thresholds of the constraints of (mixed-integer) linear programs such that the given data are optimal solutions.

**Background**: Optimization problems are often defined as forward models of various processes and systems, ranging from human decision-making to natural phenomena. In practice, however, the true objective functions and constraints of such models are rarely known in advance. Therefore, the problem of estimating objective functions and constraints from observed optimal solutions, that is, **inverse optimization**, is of great practical importance. Inverse optimization has applications in geophysics, transportation, power systems, scheduling, and healthcare, and has also developed as a foundation for various machine learning methods, including inverse reinforcement learning and contrastive learning. (cf. [[Sakaue et al. 2025](#sakaue2025online), [P13](#K13)])
<!-- 
Early research in this field emerged from geophysics, aiming to estimate subsurface structures from seismic wave data \citep{Tarantola1988-tq,Burton1992-dc}.  
Since then, inverse optimization has been widely studied \citep{Ahuja2001-cv,Heuberger2004-zv,Chan2019-zg,Chan2023-qk} and applied to various fields such as transportation \citep{Bertsimas2015-kw}, power systems \citep{Birge2017-il}, and healthcare \citep{Chan2022-uq}.  
Furthermore, it has developed as a foundation for various machine learning methods, including inverse reinforcement learning \citep{Ng2000-sf} and contrastive learning \citep{Shi2023-nd}.
-->

The research Kitaoka has been involved in is as follows. (Each part can be read independently.)
- [1](#S1). Proposing fast algorithms for the weights of the objective function in mixed-integer linear programming [[P5](#K5), [P8](#K8)]

- [2](#S2). Proposing fast algorithms for the weights of the objective function and the thresholds of the constraints in mixed-integer linear programming [[P13](#K13)]

## <a id="S1">1</a>. Proposing fast algorithms for the weights of the objective function in mixed-integer linear programming [[P5](#K5), [P8](#K8)]

### Overview of [[P8](#K8)]

||Details|
|---|---|
|Background|In mixed-integer linear programming, data-driven inverse optimization, which learns objective functions from observed data, plays an important role in building appropriate mathematical models in various fields such as power systems and scheduling.|
|Issue|Existing methods are computationally inefficient. In particular, the computational efficiency deteriorates significantly as the dimension grows. With $k$ the number of learning iterations, even an existing method regarded as efficient has an error of $O(k^{-1/(d-1)})$ in the weights.|
|Proposed method|We propose the projected subgradient method with step size $k^{-1/2}$ for the suboptimality loss.|
|Theoretical result 1|Learning is efficient. In particular, we show that there exists $\gamma >0$ such that the error from the true weights is bounded by $O\left(k^{1/(\gamma+1)} \exp\left(-\frac{\gamma}{\gamma+2}k^{1/2}\right)\right)$.|
|Theoretical result 2|We show that the observed data can be recovered in a finite number of iterations.|
|Experimental results|Experimentally, we demonstrate that the proposed method solves inverse optimization problems of MILPs with fewer than $1/7$ of the MILP calls of known methods, and converges in a finite number of iterations.|

### (Preliminaries) Problem setting
We consider a basic and important class of problems, including mixed-integer linear programs, in which the objective function of the forward problem is written as a linear combination of piecewise linear functions.
For $i = 1 , \ldots ,d $,
let $h_i \colon \mathbb{R}^{\bullet} \to \mathbb{R}$ be piecewise linear.
Let the set $\mathcal{S}$ be

$$
    \mathcal{S} = \{ s = (A,b) \in  \mathbb{R}^{\bullet \times \bullet} \times \mathbb{R}^{\bullet} \}
$$

and for $s \in \mathcal{S}$, let

$$
    X(s) := \{ x \in \mathbb{R}^{\bullet} \times \mathbb{Z}^{\bullet} | Ax \leq b\} 
$$

be a set.
Let the probability simplex be $$\Delta^{d-1} := \{ \phi \in \mathbb{R}_{\geq 0 }^d | \sum_i \phi_i =1 \} $$.
We define the forward problem as follows.

$$
\begin{equation*}
    \begin{split}
        x^* (\phi , s ) & \in \mathrm{argmax}_{x \in X (s)} \phi^{\intercal} h ( x ) \left( 
        =
        \sum_{i=1}^d \phi_i h_i (x)
        \right)
        , 
        \\
        \quad a(\phi , s ) & := h (x^*(\phi , s ))
        .
    \end{split}
    %\label{eq:forward_optimization_problem}
\end{equation*}
$$

We define the inverse optimization problem of the forward problem.
Assume that for the data $\mathcal{D} = \\{ (s^{(n)} , a^{(n)} ) \\}_{n=1}^N$ there exist unknown true weights $\phi^* $ satisfying

$$
   s^{(n)} \in \mathcal{S} , 
   \quad a^{(n)} = a ( \phi^* , s^{(n)} ).
   %\label{assu:data-follows-solver}  
$$

We consider the problem of estimating these unknown weights $\phi^* $ from the data $\mathcal{D}$, that is, the problem of minimizing the prediction loss of the solution

$$
\begin{equation*}%\label{eq:def_minimization_PLS} 
    \ell_{\mathrm{pres}} (\phi) := \frac{1}{N}\sum_{n=1}^N \| a(\phi , s^{(n)} ) - a^{(n)} \|_2^2.
\end{equation*}
$$

### (Preliminaries) Forward and inverse problems seen through scheduling

Taking scheduling as an example, we explain the challenges in formulating the forward problem and what inverse optimization solves as a way to address these challenges.

Formulating scheduling as mathematical programming means modeling the objective function and the constraints. However, the question arises of how to design the objective function.

(1) When deciding the objective function, it is rare to have only one candidate; in practice there are several. For example, candidates for the objective function in scheduling include
- the degree to which requested days off are reflected,
- the degree of workload leveling,
- labor costs,

and so on [[P5](#K5)]. Thus, one way to decide the objective function is to add up multiple candidate objective functions with ratios $\theta \in \Theta$.

Now, through scheduling, we explain how inverse optimization helps design the objective function. Solving the inverse optimization problem in scheduling using scheduling data $\{ (s^{(n)}, a^{(n)}) \}$ means, given a mathematical model $\mathrm{FOP}$, determining the ratios $\theta \in \Theta$ of the objective function that reproduce the data $\{ (s^{(n)}, a^{(n)}) \}$. Concretely, inverse optimization determines the ratios $\theta \in \Theta$ for the candidate objective functions listed in (1) such that the data $\{ (s^{(n)}, a^{(n)}) \}$ are reproduced.

### Issue
Existing methods are inefficient at minimizing the prediction loss of the solution and, especially in high dimensions, suffer from the effect of dimension. Naive methods include taking points uniformly from
the probability simplex $\Delta^{d-1}$
(UPA) and taking points randomly (RPA). UPA and RPA are strongly affected by the dimension $d$.


| Method | $\left\|\left\| \phi_k - \phi^* \right\|\right\| $ *when the prediction loss of the solution is not $0$ |
|------|-------------------------|
| UPA | $O(k^{-1/(d-1)})$ |
| RPA | $O_{\mathbb{P}}\left(\left( \frac{\log k}{k} \right)^{1/(d-1)}\right)$ |
| PSGD2 (proposed) [[P8](#K8)] | $O\left(k^{1/(\gamma+1)} \exp\left(-\frac{\gamma}{\gamma+2}k^{1/2}\right)\right)$ |

### Impact  

The proposed method PSGD2 [[P5](#K5), [P8](#K8)] attains the minimum of the prediction loss of the solution faster than the existing methods RPA and UPA, although it involves a positive constant $\gamma$ that is hard to estimate.

Moreover, for almost every true weight $\phi^*$, PSGD2 can make the prediction loss of the solution $0$ in a finite number of iterations.

### Proposed method  

We explain that, to imitate the given solutions,
it suffices to minimize the suboptimality loss

$$
\begin{equation*}
    \ell_{\mathrm{sub}} (\phi) := \frac{1}{N} \sum_{n=1}^N \left( \phi^{ \intercal} a(\phi , s^{(n)} ) - \phi^{ \intercal} a^{(n)} \right).
\end{equation*}
$$

The suboptimality loss is piecewise linear (Lipschitz) and convex, and
has the subgradient

$$
\begin{equation*}
    g (\phi) = \frac{1}{N}
    \sum_{n=1}^N 
    \left(
        a ( \phi , s^{(n)} )
        -
        a^{(n)}
    \right).
\end{equation*} 
$$

The subgradient $g$ is the average of the terms inside the norm appearing in the prediction loss of the solution. The prediction loss of the solution attains its minimum $0$ if and only if the subgradient $g$ is $0$ [Lemma 5.11, [P8](#K8)].
Assume that the subgradient $g$ attains $0$ if and only if the suboptimality loss attains its minimum.
Then we may regard the prediction loss of the solution attaining its minimum $0$ as equivalent to the suboptimality loss attaining its minimum.

The suboptimality loss is convex and Lipschitz, and its subgradient is given by $g$. Hence, to minimize the suboptimality loss over $\Delta^{d-1}$, it suffices to apply the projected subgradient method on $\Delta^{d-1}$ to the suboptimality loss. [Algorithm 1](#alg:1) gives the projected subgradient method applied to the suboptimality loss with learning rates $$\left\{ \alpha_k \right\}_{k} \subset \mathbb{R}_ {>0}$$.

In [Algorithm 1](#alg:1),
we call the method with learning rate $
   \alpha_k =
    \ell_{\mathrm{sub}} (\phi_k ) / \|\| g (\phi_k ) \|\|^2  
$
PSGDP,
and the method with learning rate
$
    \alpha_k = k^{-1/2}
    /
        \left\|\left\|
         g (\phi_k)
        \right\|\right\|
$
PSGD2.


<div class="algorithm-box" markdown="0">
<div class="algorithm-caption"><b><a id="alg:1">Algorithm 1</a></b> Suboptimality loss minimization [Algorithm 1, <a href="#K5">P5</a>]</div>
<ol class="algorithmic">
<li>Initialize $\phi_1 = \left(\frac{1}{d}, \ldots, \frac{1}{d}\right) \in \Delta^{d-1}$</li>
<li><b>for</b> $k = 1, \ldots, K-1$ <b>do</b></li>
<li class="indent-1">Compute $\phi_{k+1} \leftarrow \phi_k - \alpha_k g(\phi_k)$</li>
<li class="indent-1">Project $\phi_{k+1}$ onto $\Delta^{d-1}$</li>
<li><b>end for</b></li>
<li><b>return</b> $\phi^{\mathrm{best}}_K \in \mathrm{argmin}_{\phi \in \left\{\phi_k \right\}^K_{k=1}} \ell_{\mathrm{sub}}(\phi)$</li>
</ol>
</div>

## <a id="S2">2</a>. Proposing fast algorithms for the weights of the objective function and the thresholds of the constraints in mixed-integer linear programming [[P13](#K13)]

### Overview of [[P13](#K13)]

||Details|
|---|---|
|Background|In mixed-integer linear programming, data-driven inverse optimization, which learns objective functions and constraints from observed data, plays an important role in building appropriate mathematical models in various fields such as power systems and scheduling.|
|Issue|No method for learning both has been reported in previous studies.|
|Proposed method|For a class of problems in which the objective function is a linear combination of functions and the constraints are expressed by functions and thresholds, we propose a two-stage approach that first learns the constraints and then learns the objective function.|
|Theoretical result 1|Theoretically, a guarantee that the proposed method solves the inverse optimization problem with finite data|
|Theoretical result 2|Development of statistical learning theory on pseudometric spaces and with sub-Gaussianity|
|Theoretical result 3|We build a statistical learning theory for inverse optimization.| 
|Experimental results|Experimentally, we demonstrate that learning is possible even for scheduling formulated as an integer linear program with $100$ decision variables.|

Among these, we explain the formulation of the inverse optimization problem, the proposed method, and Theoretical result 1.

### (Preliminaries) Inverse optimization
We consider a basic and important class of problems, including mixed-integer linear programs, in which the objective function of the forward problem is written as a linear combination of piecewise linear functions.

- **State space** $\mathcal{S}$: a nonempty set

- **Set of decision variables** $\mathcal{X}$: a nonempty subset of $\mathbb{R}^{k}$

- **Parameter set of the objective function** $\Theta$: a nonempty subset of $\mathbb{R}^{d}$

- **Probability simplex**  
  $$\Delta^{d-1} := \left\{ \theta = (\theta_i)_i \in \mathbb{R}_{\geq 0}^d \,\middle|\, \sum_i \theta_i = 1 \right\}$$

- **(Feature) functions** $f_i \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($i = 1, \ldots, d$): piecewise linear functions

- **Number of constraint labels** $J$: an integer

- **Set of constraint thresholds** $\Phi$: a nonempty subset of $\mathbb{R}^J$

- **Constraint functions** $h_j \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($j = 1, \ldots, J$)

- **Constraint map**  
  $$h := (h_1, \ldots, h_J)$$

- **Feasible set** $\mathcal{X}(\phi, s)$ (for constraint thresholds $\phi \in \Phi$ and state $s \in \mathcal{S}$): 
  $$\mathcal{X}(\phi, s) := \left\{ x \in \mathcal{X} \,\middle|\, h(x, s) \leq \phi \right\}$$

For $\theta \in \Theta $, $\phi \in \Phi$, $s \in \mathcal{S}$,
the forward problem is to find $x^*$ such that

$$
\begin{equation}
    x^* (\theta, \phi, s)
    \in
    \mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s).
    \tag{2.1}
%    \label{eq:FOP_linear}
\end{equation}
$$

Given data of optimal solutions $\hat{x } \colon \mathcal{S} \to \mathcal{X}$, the data-driven inverse optimization problem (DDIOP) is to find $\theta \in \Theta$ and $\phi \in \Phi$ satisfying the following:
for any state $s \in \mathcal{S} $,

$$
\begin{equation}
    \hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)
    .
    \tag{2.2}
%    \label{eq:IOP_linear}
\end{equation}
$$

Regarding the difference between the forward problem and the inverse optimization problem,
- $$\mathcal{X}(\phi, s) := \left\{ x \in \mathcal{X} \,\middle|\, h(x, s) \leq \phi \right\}$$
- $\mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s)$
- $$\hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)$$

we obtain the following table.

|Forward problem (optimization)||Inverse problem (inverse optimization)|
|---|---|---|
|$\theta, \phi,s,f,h$|Known|$\hat{x} (s),s,f,h$|
|Optimal solution $\hat{x} (s)$|Unknown|Ratios of the objective function $\theta$, thresholds of the constraints $\phi$|


### (Preliminaries) Forward and inverse problems seen through scheduling

Taking scheduling as an example, we explain the challenges in formulating the forward problem and what inverse optimization solves as a way to address these challenges.

Formulating scheduling as mathematical programming means modeling the objective function and the constraints. However, the questions arise of (1) how to design the objective function and (2) how to design the constraints.

(1) When deciding the objective function, it is rare to have only one candidate; in practice there are several. For example, candidates for the objective function in scheduling include
- the degree to which requested days off are reflected,
- the degree of workload leveling,
- labor costs,

and so on [[P5](#K5)]. Thus, one way to decide the objective function is to add up multiple candidate objective functions with ratios $\theta \in \Theta$.


(2) When deciding the constraints, we need to consider what the appropriate thresholds of the constraints are, or which constraints are truly necessary. One way to address these issues is to use constraint templates [[Suenaga et al. 2024](#suenaga2024kaigoshi)]. Examples of constraint templates include
- the number of consecutive working days, with ratio $\phi_1$,
- assigning the start and end of night shifts consecutively within at most $\phi_2$ days per week,
- whether a specific work pattern (night shift -> day off -> night shift) is prohibited: $\phi_3 = 0$ if it is prohibited, $\phi_3 = 1$ if not,
- for each shift $\tau$, the minimum required number of staff $\phi_{4,\tau}$ and the maximum number of staff $\phi_{5,\tau}$,

and so on. By determining the constraint thresholds $\phi \in \Phi$, we can determine the constraints.

Incidentally, some constraints do not require thresholds to be determined because of laws, physical constraints, and so on. For example,
- the Labor Standards Act,
- 24 hours in a day,
- no multitasking,

and so on.

Now, through scheduling, we explain how inverse optimization helps design the objective function and the constraints. Solving the inverse optimization problem in scheduling using scheduling data $\hat{x} \colon \mathcal{S} \to \mathcal{X}$ means, given a mathematical model $\mathrm{FOP}$, determining the ratios $\theta \in \Theta$ of the objective function and the thresholds $\phi \in \Phi$ of the constraints that reproduce the data $\hat{x}$. Concretely, inverse optimization determines the ratios $\theta \in \Theta$ for the candidate objective functions listed in (1) and the thresholds $\phi \in \Phi$ of the constraint templates listed in (2) such that the data $\hat{x}$ are reproduced.



### (Preliminaries) A metric for inverse optimization

We introduce the suboptimality loss function, a metric for judging whether the inverse optimization problem (Eq. (2.2)) has been solved. For $u \in \mathbb{R}$, let the ReLU function be $$\mathrm{ReLU} (u) := \max (u , 0)$$.
Let $\lambda \in \mathbb{R}_{\geq 0 }$ be a constant.
We define the suboptimality loss $$\ell^{\mathrm{sub}, \lambda} \colon \mathcal{X} \times \Theta \times \Phi \times \mathcal{S} \to \mathbb{R}_{\geq 0 } $$ ([[Ren et al. 2025](#ren2025inverse), [P13](#K13)]) by

$$
\begin{equation*}
    \begin{split}
        \ell^{\mathrm{sub}, \lambda} \left( x, \theta ,\phi ,s \right) 
        & := 
        \mathrm{ReLU} \left(
                \max_{x^{\star} \in \mathcal{X} ( \phi , s )}
                \theta^{\top} f (x^{\star},s) 
            - \theta^{\top} f (x,s)
        \right)
        \\
        & \quad \quad + \lambda \sum_{j=1}^J
            \mathrm{ReLU} \left( 
                h_j ( x , s) - \phi_j
            \right).
    \end{split}
\end{equation*}
$$

The suboptimality loss has the following property:

<div class="theorem-box" markdown="1">
**Proposition 2.1**
Let $\lambda > 0 $ be a constant and let $x \in \mathcal{X}$. Then $x \in \mathrm{FOP}( \theta , \phi , s)$ if and only if $\ell^{\mathrm{sub}, \lambda} ( x, \theta , \phi ,s ) = 0$.
</div>

### Proposed method

Let $\Theta = \Delta^{d-1}$.
We propose the following algorithm for solving the inverse optimization problem.

<div class="algorithm-box" markdown="0">
<div class="algorithm-caption"><b><a id="alg:2.1">Algorithm 2.1</a></b> Maximizing feasible set then minimizing suboptimality loss [Algorithm 2, <a href="#K13">P13</a>]</div>
<ol class="algorithmic">
<li>Take $\varepsilon \geq 0$.</li>
<li>$\phi^{\sup} := \min_{\phi \in \Phi} \{ \phi | h ( x^{*} (s) , s) \leq \phi \text{ for } s \in \mathcal{S}^\prime \}$</li>
<li>Compute $\theta^{\sup} \in \Delta^{d-1}$ satisfying $\mathbb{E}_{S} \ell^{\mathrm{sub}, 0} ( \hat{x}^{*} (S), \theta^{\sup} , \phi^{\sup} ,S ) \leq \varepsilon$</li>
<li><b>return</b> $\theta^{\sup} \in \Delta^{d-1}, \phi^{\sup} \in \Phi$</li>
</ol>
</div>

**Remark 2.2**: [[P8](#K8), Algorithm 1] is one way to implement line 3 of [Algorithm 2.1](#alg:2.1).

### (Theoretical result 1) Theory of imitation

[Algorithm 2.1](#alg:2.1) solves the inverse optimization problem (Eq. (2.2)); that is, the following theorem holds.

<div class="theorem-box" markdown="1">
**Theorem 2.3**
We say that a map $\hat{x} \colon \mathcal{S} \to \mathcal{X}$ is an optimal solution map if there exist $\theta^{\mathrm{true}} \in \Theta = \Delta^{d-1}$ and $\phi^{\mathrm{true}} \in \Phi $ such that $\hat{x} (s) = x^* (\theta^{\mathrm{true}} , \phi^{\mathrm{true}} , s)$ for any $s \in \mathcal{S}$. Let $\varepsilon = 0 $.
Then, for almost every $\theta^{\mathrm{true}} \in \Delta^{d-1}$, the output $\theta^{\sup}, \phi^{\sup}$ of [Algorithm 2.1](#alg:2.1) with [[P8](#K8), Algorithm 1] incorporated into line 3 of [Algorithm 2.1](#alg:2.1) satisfies $\hat{x}^{*} (s ) \in \mathbf{FOP} (\theta^{\sup}, \phi^{\sup}, s)$, that is, Eq. (2.2).
</div>

# References

[<a id="K5">P5</a>] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

[<a id="K8">P8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]


[<a id="K13">P13</a>]  Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]

[<a id="ren2025inverse">Ren et al. 2025</a>] Ren, K., Esfahani, P. M., and Georghiou, A. (2025). Inverse optimization via learning feasible regions. In The 42nd International Conference on Machine Learning, to appear. [[arXiv:2505.15025](https://arxiv.org/abs/2505.15025)]

[<a id="sakaue2025online">Sakaue et al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, NeurIPS 2025, to appear.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]

[<a id="suenaga2024kaigoshi">Suenaga et al. 2024</a>] Koki Suenaga, Yuya Nagai, Kazuto Kashiwagi, Satoshi Ono, An Attempt to Extract Constraints in Care Worker Scheduling Problems (Japanese), Proceedings of the Annual Conference of JSAI, JSAI2024, the 38th (2024), Session ID 2M1-OS-11a-04, p. 2M1OS11a04.

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
