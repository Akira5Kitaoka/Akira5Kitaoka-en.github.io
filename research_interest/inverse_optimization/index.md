<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

Last updated: October 17, 2026

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
- [1](#S1). Proposing fast algorithms for the weights of the objective function in mixed-integer linear programming, and explicitly evaluating their number of iterations for integer linear programming [[P5](#K5), [P8](#K8), [P21](#K21)]

- [2](#S2). Proposing fast algorithms for the weights of the objective function and the thresholds of the constraints in mixed-integer linear programming [[P13](#K13)]

## <a id="S1">1</a>. Proposing fast algorithms for the weights of the objective function in mixed-integer linear programming, and explicitly evaluating their number of iterations for integer linear programming [[P5](#K5), [P8](#K8), [P21](#K21)]

### Overview of [[P8](#K8)]

||Details|
|---|---|
|Background|In mixed-integer linear programming (MILP), the data-driven inverse optimization problem (DDIOP), which estimates objective functions (weights) consistent with observed optimal-solution data, plays an important role in building appropriate mathematical models in various fields such as power systems and scheduling.|
|Issue|The prediction loss of features (PLF), the evaluation metric, is discontinuous in the weights, so gradient-based optimization cannot be applied. The suboptimality loss, an alternative metric, is Lipschitz continuous and convex, but most existing studies only give asymptotic convergence guarantees as the number of iterations $T\to\infty$, and whether the problem can be solved exactly ($\ell_{\mathrm{sub}}=0$) in a finite number of iterations had not been clarified. Naive point-set search methods (UPA/RPA) are strongly affected by the dimension $d$.|
|Proposed method|We apply a broad class of gradient-based optimization methods, including the projected subgradient method (PSGD), to the suboptimality loss.|
|Theoretical result 1 (exact solvability in finitely many iterations)|We show that gradient-based optimization methods that (A) update using only past iterates and subgradients and (B) have standard convergence guarantees for Lipschitz convex functions reach $\ell_{\mathrm{sub}}=0$ in a finite number of iterations and solve the DDIOP for MILPs exactly.|
|Theoretical result 2 (explicit number of iterations)|For representative step-size rules of PSGD, we give upper bounds on the number of iterations needed to reach the solution in finitely many iterations, via a constant $\gamma(\ell_{\mathrm{sub}})$.|
|Theoretical result 3 (derived result)|We show that PSGD reaches the minimum value $0$ of the PLF in a finite number of iterations.|
|Experimental results|On synthetic data (LP) and scheduling problems, we demonstrate that the method minimizes the PLF with fewer than $1/7$ (LP) and $1/10$ (scheduling) of the iterations of existing methods, and achieves PLF$=0$ with probability $100\%$.|

### (Preliminaries) Problem setting
We consider a basic and important class of problems, including mixed-integer linear programs (MILPs), in which the objective function of the forward problem is written as a linear combination of piecewise linear functions.
Let $\mathcal{X} \subset \mathbb{R}^{d_{\mathcal{X}}}$ be the set of decision variables and $\mathcal{S}$ the set of states. For each state $s \in \mathcal{S}$, let the feasible region $X(s) \subset \mathcal{X}$ be a finite union of bounded closed convex polyhedra.
For each $i = 1 , \ldots ,d $, let $f_i \colon \mathcal{X} \to \mathbb{R}$ be piecewise linear, and let $f = (f_1 , \ldots , f_d )$.
Let the weight space $\Theta \subset \mathbb{R}^d$ be a bounded closed convex set; in particular, we consider the probability simplex
$$\Delta^{d-1} := \{ \theta \in \mathbb{R}_{\geq 0 }^d \mid \sum_i \theta_i =1 \}. $$
We define the forward optimization problem (FOP) and the features of its optimal solution as follows.

$$
    x^* (\theta , s ) \in \mathrm{argmax}_{x \in X (s)} \theta^{\top} f ( x ) \left( 
        = \sum_{i=1}^d \theta_i f_i (x) \right)
        , 
    \quad a^* (\theta , s ) := f (x^*(\theta , s ))
        .
$$

We define the inverse optimization problem of the forward problem.
Assume that for the data $\mathcal{D} = \\{ (s^{(n)} , x^{(n)} ) \\}_{n=1}^N$ there exist unknown true weights $\theta^* \in \Theta$ satisfying

$$
   s^{(n)} \in \mathcal{S} , 
   \quad x^{(n)} = x^* ( \theta^* , s^{(n)} ).
   %\label{assu:data-follows-solver}  
$$

Let $a^{(n)} := f (x^{(n)})$.
This data-driven inverse optimization problem (DDIOP) is the problem of finding weights $\theta \in \Theta$ consistent with the observed optimal solutions, that is, $\theta$ satisfying

$$
    x^{(n)} \in \mathrm{argmax}_{x \in X (s^{(n)})} \theta^{\top} f ( x )
$$

for any $n = 1 , \ldots , N$.
As a metric of whether the DDIOP has been solved, there is the prediction loss of features (PLF)

$$
    \ell_{\mathrm{plf}} (\theta) := \frac{1}{N}\sum_{n=1}^N \| a^* (\theta , s^{(n)} ) - a^{(n)} \|_2^2 .
$$

If $\ell_{\mathrm{plf}} (\theta) = 0$, the DDIOP has been solved.

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
Since the PLF is discontinuous in the weights $\theta$, gradient-based optimization methods cannot be applied directly. Naive methods for solving the DDIOP include taking a point set uniformly from the probability simplex $\Delta^{d-1}$ (UPA) and taking a point set randomly (RPA). However, with $T$ the number of points, the distance between the true weights $\theta^*$ and the point set is bounded by $O(T^{-1/(d-1)})$ and $O\left(\left( \frac{\log T}{T} \right)^{1/(d-1)}\right)$, respectively, which deteriorates rapidly as the dimension $d$ grows.

On the other hand, the suboptimality loss $\ell_{\mathrm{sub}}$ described below is a Lipschitz continuous and convex loss that can determine whether the DDIOP has been solved, and first-order methods such as online optimization can be applied to it. However, most existing studies only give asymptotic convergence guarantees that "$\ell_{\mathrm{sub}}$ approaches $0$ as $T\to\infty$", and **whether $\ell_{\mathrm{sub}}=0$ can be achieved exactly in a finite number of iterations** had not been clarified.

| Method | Typical guarantee | $\ell_{\mathrm{sub}}=0$ in finitely many iterations |
|------|------|------|
| Point-set search UPA/RPA | Approximation based on covering error (strong dependence on dimension) | No |
| Online optimization (MWU/ONS/MetaGrad, etc.) | Asymptotic decrease of $\ell_{\mathrm{sub}}$ (e.g., $O(1/\sqrt{T})$, $O(\log T/T)$) | No |
| This work (gradient-based) [[P8](#K8)] | Convexity, piecewise linearity, and interior points guarantee $0$ in finitely many iterations | **Yes** |

### Proposed method  

While the PLF is discontinuous, the suboptimality loss, an alternative metric for imitating the given solutions,

$$
    \ell_{\mathrm{sub}} (\theta) := \frac{1}{N} \sum_{n=1}^N \left( \theta^{\top} a^* (\theta , s^{(n)} ) - \theta^{\top} a^{(n)} \right) 
$$

is Lipschitz continuous and convex, and its subgradient is given by

$$
    g (\theta) = \frac{1}{N}
    \sum_{n=1}^N 
    \left(
        a^* ( \theta , s^{(n)} )
        -
        a^{(n)}
    \right)    
$$

[Proposition 3.1, [P8](#K8)]. The DDIOP is solved if and only if $\ell_{\mathrm{sub}}(\theta) = 0$. Therefore, it suffices to minimize the suboptimality loss over $\Delta^{d-1}$.

The proposed method applies to the suboptimality loss a gradient-based optimization method that iteratively updates $\theta$ using the subgradient information $g$ ([Algorithm 1](#alg:1)). As the update rule $\mathrm{update}_t$, we use, for example, the projected subgradient descent (PSGD)
$$\mathrm{update}_t = \mathrm{Proj}_{\Theta} \left( \theta^t - \alpha_t g(\theta^t) \right).$$
Learning rates $\alpha_t$ include nonsummable step sizes (NSS) and step lengths (NSL), in particular SRSS/SRSL of the form $\beta t^{-1/2}$, and Polyak's step size when the minimum value is known.


<div class="algorithm-box" markdown="0">
<div class="algorithm-caption"><b><a id="alg:1">Algorithm 1</a></b> Suboptimality loss minimization [Algorithm 1, <a href="#K8">P8</a>]</div>
<ol class="algorithmic">
<li>Initialize $\theta^1 \in \Theta$</li>
<li><b>for</b> $t = 1, \ldots, T-1$ <b>do</b></li>
<li class="indent-1">Solve $x^*(\theta^t, s^{(n)})$ for each $n$</li>
<li class="indent-1">$\theta^{t+1} \leftarrow \mathrm{update}_t \left( \{ \theta^{t^\prime} \}_{t^\prime=1}^t \mid \ell_{\mathrm{sub}}, g \right)$</li>
<li><b>end for</b></li>
<li><b>return</b> $\theta^{\mathrm{best}}_T \in \mathrm{argmin}_{\theta \in \left\{\theta^t \right\}^T_{t=1}} \ell_{\mathrm{sub}}(\theta)$</li>
</ol>
</div>

### Main result (exact solvability in finitely many iterations)

Suppose that a gradient-based optimization method (A) updates using only past iterates and subgradients and (B) has a standard convergence guarantee $Q$ for the best iterate on any Lipschitz convex function. Then there exists a positive constant $\gamma(\ell_{\mathrm{sub}})$ such that, if $T \geq Q(\gamma(\ell_{\mathrm{sub}}))$, then $\min_{t = 1, \ldots, T}\ell_{\mathrm{sub}}(\theta^t)=0$; that is, the DDIOP for MILPs is solved exactly [Main theorem, [P8](#K8)].

In particular, for PSGD (SRSS/SRSL), $Q$ can be constructed explicitly, giving an upper bound (roughly of order $O(\gamma(\ell_{\mathrm{sub}})^{-2})$) on the number of iterations needed to reach the solution in finitely many iterations. As a derived result, PSGD (SRSS/SRSL) also reaches the minimum value $0$ of the PLF in a finite number of iterations.

### Idea of the proof (convexity + piecewise linearity + interior points)

The suboptimality loss (i) is convex and piecewise linear, (ii) has minimum value $0$, and (iii) (for almost every true weight $\theta^*$) has a set of minimizers with nonempty interior. When a piecewise linear convex function has a set of minimizers with nonempty interior, we can construct a convex piecewise linear function $\widetilde{\ell}$ whose values are pushed down only in the interior of the set of minimizers, so that $\ell_{\mathrm{sub}}=\widetilde{\ell}$ outside the set of minimizers. Hence the iterates of a first-order method applied to the two functions coincide until they enter the set of minimizers. Setting $\gamma:=\min \ell_{\mathrm{sub}} - \min\widetilde{\ell}>0$, the standard convergence guarantee for $\widetilde{\ell}$ yields $\widetilde{\ell}(\theta^T)\le \min\widetilde{\ell}+\gamma=\min\ell_{\mathrm{sub}}$ in finitely many iterations, and since the iterates coincide, $\ell_{\mathrm{sub}}(\theta^T)=0$ is reached at the same $T$.

### Impact  

Although the number of iterations of the proposed method [[P5](#K5), [P8](#K8)] depends on a positive constant $\gamma(\ell_{\mathrm{sub}})$ that is hard to estimate, what is new is that, unlike UPA, RPA, and online optimization, **it solves the DDIOP exactly in a finite number of iterations**. In numerical experiments, we confirmed that PSGD (SRSL) minimizes the PLF with fewer than $1/7$ (LP) and $1/10$ (scheduling) of the iterations of known methods (UPA/RPA/CHAN), and achieves PLF$=0$ with probability $100\%$.
The explicit evaluation of the constant $\gamma(\ell_{\mathrm{sub}})$ in terms of the problem size was carried out in [[P21](#K21)] for the case where the forward problem is an integer linear program ([next subsection](#S1-P21)).

### <a id="S1-P21"></a>[[P21](#K21)]: Explicit evaluation of the number of iterations for integer linear programming

||Details|
|---|---|
|Background|By [[P8](#K8)], gradient-based optimization methods reach $\ell_{\mathrm{sub}}=0$ in a finite number of iterations, and the number of iterations is bounded by $O(\gamma(\ell_{\mathrm{sub}})^{-2})$ using a geometric constant $\gamma(\ell_{\mathrm{sub}})>0$.|
|Issue|$\gamma(\ell_{\mathrm{sub}})$ is an abstract constant determined by the geometry of the problem, and apart from its positivity, no way was known to bound it from below as a function of the problem size. Hence the number of iterations sufficient to solve the problem exactly could not be given as an explicit function of the number of samples $N$ or the feature dimension $d$.|
|Proposed method|When the forward problem is an integer linear program (ILP), we derive explicit lower bounds on $\gamma(\ell_{\mathrm{sub}})$ from the structure of integer programming (total unimodularity, Graver bases, M-convexity/M$^\natural$-convexity), and substitute them into the iteration upper bounds of [[P8](#K8)].|
|Theoretical result 1 (lower bounds on $\gamma(\ell_{\mathrm{sub}})$)|We give explicit lower bounds on $\gamma(\ell_{\mathrm{sub}})$ for general ILPs, linear inequality constraints (Graver bases), totally unimodular constraints, and M-convex/M$^\natural$-convex sets.|
|Theoretical result 2 (explicit number of iterations)|We give the number of iterations sufficient to solve the DDIOP exactly (and to reach the minimum value $0$ of the PLF) as an explicit function of $N$, $d$, the ranges of the features, and the structure of the constraint matrix.|
|Theoretical result 3 (tightness)|We construct ILP instances for which $\gamma(\ell_{\mathrm{sub}})$ is exponentially small in the dimension $d$, showing that the exponential dependence on $d$ of the lower bound for general ILPs is essential.|

In what follows, let $\Theta = \Delta^{d-1}$, and let the ranges of the features be $m_i := \sup_{s \in \mathcal{S}} \left( \max_{x \in X(s)} f_i(x) - \min_{x \in X(s)} f_i(x) \right)$ and $m = (m_i)_i$. When the feasible region is given by a system of linear inequalities $X(s)=\\{x\in\mathbb{Z}^d \mid A x\le b(s)\\}$, we use the $\ell_\infty$-norm $C:=g_\infty(\widetilde{A}) \in \mathbb{Z}_{\geq 1}$ of the Graver basis of the slack-augmented matrix $\widetilde{A}=[A\mid I]$.
Since the iteration upper bounds of PSGD (SRSS/SRSL) are of the form $O(\gamma(\ell_{\mathrm{sub}})^{-2}) \times \mathrm{poly}$ (where $\mathrm{poly} := \mathrm{poly}(\mathrm{diam}(\Theta),\beta,L(\ell_{\mathrm{sub}}))$), substituting the lower bounds on $\gamma(\ell_{\mathrm{sub}})$ yields the following table [[P21](#K21)].

|Setting|Lower bound on $\gamma(\ell_{\mathrm{sub}})$|Iteration upper bound (SRSS, SRSL)|
|---|---|---|
|General ILP|$\dfrac{1}{N^{d}\max(d-1,\sqrt2)\,\lVert m\rVert_2^{d-1}}$|$O\left(N^{2d}\,d^{2}\,\lVert m\rVert_2^{2(d-1)}\,\mathrm{poly}\right)$|
|M-convex, M$^\natural$-convex|$\Omega\left(\dfrac{1}{N d^{2}}\right)$|$O\left(N^{2}d^{4}\,\mathrm{poly}\right)$|
|Linear inequalities (constant $C$)|$\Omega\left(\dfrac{1}{N\,d^{(d+1)/2}(2C)^{d-1}}\right)$|$O\left(N^{2}\,d^{d+1}(2C)^{2(d-1)}\,\mathrm{poly}\right)$|
|Totally unimodular ($C=1$)|$\Omega\left(\dfrac{1}{N\,d^{(d+1)/2}2^{d-1}}\right)$|$O\left(N^{2}\,d^{d+1}4^{d-1}\,\mathrm{poly}\right)$|

The number of iterations needed to reach the minimum value $0$ of the PLF can also be evaluated with the same order.

**Impact**: This shows that (i) a budget for the number of iterations and a stopping criterion can be fixed before running the algorithm, (ii) guarantees of finite-step attainment and regret-type asymptotic guarantees can be compared on the same scale of the problem size, and (iii) even for the same feasible region, the estimate of the number of iterations can be polynomial or exponential in the dimension depending on which structure is used to evaluate $\gamma(\ell_{\mathrm{sub}})$ ($\Omega(1/(Nd^2))$ when M$^\natural$-convexity is used directly, and $\Omega(1/(N d^{(d+1)/2}(2C)^{d-1}))$ when the set is treated as general linear inequalities).


## <a id="S2">2</a>. Proposing fast algorithms for the weights of the objective function and the thresholds of the constraints in mixed-integer linear programming [[P13](#K13)]

### Overview of [[P13](#K13)]

||Details|
|---|---|
|Background|In mixed-integer linear programming (MILP), the data-driven inverse optimization problem (DDIOP), which learns objective functions and constraints from observed data, plays an important role in building appropriate mathematical models in various fields such as power systems and scheduling.|
|Issue|There are studies that learn both for linear programming, but no method for learning both the objective function and the constraints in the DDIOP for MILPs has been reported in previous studies.|
|Proposed method|For a class of problems in which the objective function is a linear combination of features and the constraints are expressed by functions and thresholds, we propose a two-stage approach that first learns the constraints and then learns the objective function.|
|Theoretical result 1 (imitation)|Under finite distributions, we guarantee that the proposed method exactly reproduces the observed optimal solutions (imitation).|
|Theoretical result 2 (extension to pseudometrics)|Noting that the distance between parameters that naturally appears in inverse optimization is a pseudometric, we extend statistical learning theory under sub-Gaussian assumptions (covering number analysis) from metric spaces to pseudometric spaces.|
|Theoretical result 3 (generalization error)|Using the above, we derive a generalization error bound for inverse optimization (suboptimality loss minimization).|
|Experimental results|We demonstrate that, even for scheduling formulated as an integer linear program with $100$ decision variables ($D=10$), the observed solutions can be reproduced as optimal solutions after learning (the DDIOP can be solved).|

Among these, we explain the formulation of the inverse optimization problem, the proposed method, and Theoretical result 1.

### (Preliminaries) Inverse optimization
We consider a basic and important class of problems, including mixed-integer linear programs, in which the objective function of the forward problem is written as a linear combination of piecewise linear functions.

- **State space** $\mathcal{S}$: a nonempty set

- **Set of decision variables** $\mathcal{X}$: a nonempty subset of $\mathbb{R}^{k}$

- **Parameter set of the objective function** $\Theta$: a nonempty bounded closed convex subset of $\mathbb{R}^{D}$

- **Probability simplex**  
  $$\Delta^{D-1} := \left\{ \theta = (\theta_i)_i \in \mathbb{R}_{\geq 0}^D \,\middle|\, \sum_i \theta_i = 1 \right\}$$

- **(Feature) functions** $f_i \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($i = 1, \ldots, D$): piecewise linear functions

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
    x^* (\theta, \phi, s)
    \in
    \mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s)
    \tag{2.1}
$$

Given data of optimal solutions $\hat{x } \colon \mathcal{S} \to \mathcal{X}$, the data-driven inverse optimization problem (DDIOP) is to find $\theta \in \Theta$ and $\phi \in \Phi$ satisfying the following:
for any state $s \in \mathcal{S} $,

$$
    \hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)
    .
    \tag{2.2}
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
\begin{aligned}
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
\end{aligned}
$$

The suboptimality loss has the following property:

<div class="theorem-box" markdown="1">
**Proposition 2.1**
Let $\lambda > 0 $ be a constant and let $x \in \mathcal{X}$. Then $x \in \mathrm{FOP}( \theta , \phi , s)$ if and only if $\ell^{\mathrm{sub}, \lambda} ( x, \theta , \phi ,s ) = 0$.
</div>

### Proposed method

Let $\Theta = \Delta^{D-1}$.
We propose the following algorithm for solving the inverse optimization problem.

<div class="algorithm-box" markdown="0">
<div class="algorithm-caption"><b><a id="alg:2.1">Algorithm 2.1</a></b> Maximizing feasible set then minimizing suboptimality loss [Algorithm 2, <a href="#K13">P13</a>]</div>
<ol class="algorithmic">
<li>Take $\varepsilon \geq 0$.</li>
<li>$\phi^{\sup} := \min_{\phi \in \Phi} \{ \phi \mid h ( \hat{x} (s) , s) \leq \phi \text{ for } s \in \mathcal{S} \}$</li>
<li>Compute $\theta^{\sup} \in \Delta^{D-1}$ satisfying $\mathbb{E}_{S} \ell^{\mathrm{sub}, 0} ( \hat{x} (S), \theta^{\sup} , \phi^{\sup} ,S ) \leq \varepsilon$</li>
<li><b>return</b> $\theta^{\sup} \in \Delta^{D-1}, \phi^{\sup} \in \Phi$</li>
</ol>
</div>

**Remark 2.2**: [[P8](#K8), Algorithm 1] is one way to implement line 3 of [Algorithm 2.1](#alg:2.1).

### (Theoretical result 1) Theory of imitation

[Algorithm 2.1](#alg:2.1) solves the inverse optimization problem (Eq. (2.2)); that is, the following theorem holds.

<div class="theorem-box" markdown="1">
**Theorem 2.3**
We say that a map $\hat{x} \colon \mathcal{S} \to \mathcal{X}$ is an optimal solution map if there exist $\theta^{\mathrm{true}} \in \Theta = \Delta^{D-1}$ and $\phi^{\mathrm{true}} \in \Phi $ such that $\hat{x} (s) = x^* (\theta^{\mathrm{true}} , \phi^{\mathrm{true}} , s)$ for any $s \in \mathcal{S}$. Let $\varepsilon = 0 $.
Then, for almost every $\theta^{\mathrm{true}} \in \Delta^{D-1}$, the output $\theta^{\sup}, \phi^{\sup}$ of [Algorithm 2.1](#alg:2.1) with [[P8](#K8), Algorithm 1] incorporated into line 3 of [Algorithm 2.1](#alg:2.1) satisfies $\hat{x} (s) \in \mathbf{FOP} (\theta^{\sup}, \phi^{\sup}, s)$, that is, Eq. (2.2).
</div>

# References

[<a id="K5">P5</a>] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

[<a id="K8">P8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]


[<a id="K13">P13</a>]  Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]

[<a id="K21">P21</a>] Akira Kitaoka, Explicit Iteration Complexity of Exact Data-Driven Inverse Optimization for Integer Linear Programs, preprint.
[[arXiv:2607.22263](https://arxiv.org/abs/2607.22263)]

[<a id="ren2025inverse">Ren et al. 2025</a>] Ren, K., Esfahani, P. M., and Georghiou, A. (2025). Inverse optimization via learning feasible regions. In The 42nd International Conference on Machine Learning. to appear. [[arXiv:2505.15025](https://arxiv.org/abs/2505.15025)]

[<a id="sakaue2025online">Sakaue et al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, NeurIPS 2025, to appear.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]

[<a id="suenaga2024kaigoshi">Suenaga et al. 2024</a>] Koki Suenaga, Yuya Nagai, Kazuto Kashiwagi, Satoshi Ono, An Attempt to Extract Constraints in Care Worker Scheduling Problems (Japanese), Proceedings of the Annual Conference of JSAI, JSAI2024, the 38th (2024), Session ID 2M1-OS-11a-04, p. 2M1OS11a04.

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
