<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

# Invariants of parabolic geometry with BGG complexes
**Keywords**:  Rumin complex, analytic torsion, contact geometry, parabolic geometry, geometric analysis

**Motivation in brief**: The aim is to define invariants and notions that are defined via the de Rham complex in the same way for the Bernstein-Gelfand-Gelfand complex (BGG complex), which appears in contact geometry and parabolic geometry, and to find geometric constraints by clarifying the relationship between the two.

**Terminology**
BGG complex: A complex constructed for parabolic geometries and filtered manifolds, whose cohomology coincides with the de Rham cohomology.
Rumin complex: The Bernstein-Gelfand-Gelfand complex (BGG complex) for contact manifolds (cf. [[P7](#K7)]). It also appears naturally when one takes the sub-Riemannian limit [[Rum00](#Rum00),[AQ22](#AQ22)].



## Eigenvalue decomposition of the Rumin Laplacian on spheres [[P1](#K1)]

**Motivation**: By computing the eigenvalues of the Rumin Laplacian, a partial differential operator determined by the Rumin complex, we aim to compute on spheres the quantities conjectured to be geometric invariants. Geometric invariants can be defined using a partial differential operator defined via the de Rham complex (the Hodge-de Rham Laplacian). Examples include the dimension of cohomology, the Euler number, the analytic torsion (the Euler number can be written as an identity via the heat kernel, and the analytic torsion is obtained by taking a variation of this formula), and the $\eta$-invariant. All of these can be defined using the eigenvalues of the Hodge-de Rham Laplacian. Therefore, when building the theory for BGG complexes corresponding to the theory of the de Rham complex, we expect that knowing the eigenvalues of the Rumin Laplacian allows us to compute what appear to be the corresponding invariants.

**About the main theorem**: The eigenvalues of the Hodge-de Rham Laplacian on symmetric spaces can be written in terms of Casimir elements [[IT78](#IT78)]. Whether the Rumin Laplacian can also be expressed by something like Casimir elements is an interesting question.

## Coincidence of the torsion determined by the Rumin complex (contact torsion) and the analytic torsion on lens spaces [[P3](#K3)]

**Motivation**: Rumin and Seshadri showed that, on $3$-dimensional Sasakian manifolds with an $S^1$-action, the analytic torsion of the Rumin-Seshadri Laplacian coincides with the analytic torsion (of the Hodge-de Rham Laplacian) [[RS12](#RS12)]. A natural question is whether this also holds on higher-dimensional Sasakian manifolds.



## Coincidence of the $0$-eigenspace of the Rumin Laplacian and the $0$-eigenspace of the Hodge-de Rham Laplacian [[P4](#K4)]

**Motivation**: It is known that the cohomology of the Rumin complex is isomorphic to the de Rham cohomology [[Rum94](#Rum94)]. On the other hand, a natural question arises as to how much the $0$-eigenspaces of the Laplacians differ.

**About the main theorem**: On Sasakian manifolds, the $0$-eigenspace of the Rumin Laplacian coincides with the $0$-eigenspace of the Hodge-de Rham Laplacian [[P4](#K4)]. On Sasakian manifolds, it was known that the $0$-eigenvectors of the Hodge-de Rham Laplacian are primitive [[Tac65](#Tac65), Theorems 7.1, 8.1] [[Fuj66](#Fuj66), Corollary 4.1]. Our result means that these statements are rewritten in the language of the Rumin complex.

## References

[<a id="K1">P1</a>] Akira Kitaoka, Analytic torsions associated with the Rumin complex on contact spheres, International Journal of Mathematics 31 (2020), no. 13, 2050112.
[[article link](https://www.worldscientific.com/doi/10.1142/S0129167X20501128)]
[[arXiv:1911.03092](https://arxiv.org/abs/1911.03092)]

<details><summary>Behind the scenes (how the paper [P1] came about)</summary>

This research started when I was a master's student. Besides the Rumin Laplacian, there is another partial differential operator defined using the Rumin complex: the Rumin-Seshadri Laplacian. Since Rumin and Seshadri had shown that the analytic torsion of the Rumin-Seshadri Laplacian coincides with the analytic torsion (of the Hodge-de Rham Laplacian) on $3$-dimensional Sasakian manifolds with an $S^1$-action, I started this research thinking that I could prove the same on higher-dimensional Sasakian manifolds. In August of my second year of the master's program, I was able to compute the eigenvalues of the Rumin-Seshadri Laplacian on spheres. However, when I computed the quantity corresponding to the analytic torsion, it did not coincide with the analytic torsion, and I could not understand why they differed. When I consulted my advisor, Professor Hirachi, his advice **"Go back to the definitions"** left a strong impression on me. After revisiting the definitions, in August of my first year of the doctoral program, I noticed that the eigenvalues of the Rumin Laplacian on spheres can be written by a simple formula, and I was able to compute the difference between the analytic torsion of the Rumin Laplacian and the analytic torsion (of the Hodge-de Rham Laplacian).
</details>

[<a id="K3">P3</a>] Akira Kitaoka, Ray-Singer Torsion and the Rumin Laplacian on lens spaces, 	
Symmetry, Integrability and Geometry: Methods and Applications (SIGMA) 18 (2022), 091, 16 pages.
[[article link](https://doi.org/10.3842/SIGMA.2022.091)][[arXiv:2009.03276](https://arxiv.org/abs/2009.03276)]

<details><summary>Behind the scenes (how the paper [P3] came about)</summary>

The key point was to use the Littlewood-Richardson rule to reduce the eigenvalues of the Rumin Laplacian on lens spaces to computations of characters in representation theory. This became my doctoral thesis.
</details>

[<a id="K4">P4</a>] Akira Kitaoka, Harmonic forms and the Rumin complex on Sasakian manifolds, Osaka Journal of Mathematics, to appear.
[[arXiv:2204.03446](https://arxiv.org/abs/2204.03446)]

<details><summary>Behind the scenes (how the paper [P4] came about)</summary>

The result was already obtained during my doctoral program. I wrote it up because it seemed better to turn it into a paper for the time being.
</details>

[<a id="K7">P7</a>] Akira Kitaoka, Ray-Singer torsion and the Rumin Laplacian on lens spaces, [Geometric Structures and Differential Equations - Symmetry, Singularity, and Quantization -](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/2268.html), RIMS Kôkyûroku, Kyoto University, November 2023. [Manuscript](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/pdf/2268-10.pdf)

<details><summary>If you want an RTA (real-time attack) to defining BGG complexes, this might be the fastest</summary>

or so I think.
</details>

[<a id="AQ22">AQ22</a>] P. Albin, H. Quan, Sub-Riemannian limit of the differential form heat kernels of contact manifolds, Int. Math. Res. Not. 2022 (2022), 5818–5881.

[<a id="Fuj66">Fuj66</a>] T. Fujitani, Complex-valued differential forms on normal contact Riemannian manifolds, Tohoku Math. J. 81 (1966), 349–361.

[<a id="IT78">IT78</a>] A. Ikeda and Y. Taniguchi, Spectra and eigenforms of the Laplacian on $S^n$ and $P^n(C)$, Osaka J. Math. 15 (1978), no. 3, 515–546.

[<a id="Rum94">Rum94</a>] M. Rumin, Formes différentielles sur les variétés de contact (French), J. Differential Geom. 39 (1994), no. 2, 281–330.

[<a id="Rum00">Rum00</a>] M. Rumin, Sub-Riemannian limit of the differential form spectrum of contact manifolds, Geom. Funct. Anal.
10 (2000), 407–452.

[<a id="RS12">RS12</a>] M. Rumin and N. Seshadri, Analytic torsions on contact manifolds, Ann. Inst. Fourier
(Grenoble) 62 (2012), 727–782.

[<a id="Tac65">Tac65</a>] S. Tachibana, On harmonic tensors in compact Sasakian spaces, Tohoku Math. J. (2) 17 (1965), 271–284.

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
