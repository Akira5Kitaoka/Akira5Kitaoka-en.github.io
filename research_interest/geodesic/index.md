<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

# Algorithm of approximating minimal geodesics

**Keywords**: Numerical analysis, geodesics, mathematical optimization, optimal control

**Background**: The problem of determining minimal geodesics, which realize the shortest length, has attracted attention in various fields such as computer vision, robotics, and machine learning. (cf. [[P10](#K10)])


## An example where the solution of length minimization approximated by finite differences and numerical integration does not converge to the true length minimization [[P10](#K10), $\S 5$]

**Impact**: This means that even if we implement functional minimization by approximating it with finite differences and numerical integration, we cannot achieve the true functional minimization.

##  Error estimates for length minimization through energy minimization of curves in real coordinate space using finite differences and numerical integration [[P10](#K10)]

**Background**: One way to implement minimal geodesics is to linearly interpolate the solution of a nonlinear optimization problem obtained by approximating the energy minimization problem of curves with finite differences and numerical integration. This method tends to be easier to implement than other methods.


**Previous issue**: For Riemannian metrics on real coordinate space, it was not known whether the method of approximately solving energy minimization with finite differences and numerical integration actually minimizes the length of curves.

**Impact**: We proved that the length of the curve obtained by linearly interpolating the solution of the optimization problem, in which the energy of curves is approximated by finite differences and the trapezoidal rule, converges to the minimal length of curves with order $1/2$ in the number of points used in the numerical integration. We also proved that, when the left-point rule is used instead, the length of the approximate curve likewise converges to the minimal length of curves with order $1/2$ in the number of points used in the numerical integration.

**Key ideas**: A priori estimates, variational principles, theory of Sobolev spaces

## References

[<a id="K10">P10</a>] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

<!--
<details><summary>Behind the scenes (how the paper [P10] came about)</summary>

Secret
</details>
<br>
-->

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
