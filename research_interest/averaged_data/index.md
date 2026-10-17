<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>

# Learning from averaged data

**Background**: Data aggregation techniques, including data averaging, are useful for data summarization, compression, reducing annotation costs, improving data quality, and protecting privacy [[Zhang+ 2020](#Zhang2020)]. In particular, averaging over multiple samples is effective for privacy protection because it makes it difficult to fully recover the original data before averaging.

**Issue**: Ecological Regression is known as a machine learning method for inputs and outputs averaged over multiple samples [[Robinson 50](#Robinson1950), [Gelman+ 01](#Gelman2001)]. Ecological Regression trains a linear regression model using the prediction error on the averaged data [[Bhowmik+ 16](#Bhomik2016)]. However, this training method is not suitable for nonlinear regression models. To the best of our knowledge, no method was known for learning general nonlinear models from input-output data averaged over multiple samples.

**Method**: [[P19](#K19)] formulates the likelihood of a prediction model for averaged data under the assumptions of additive homoscedastic Gaussian noise and normally distributed inputs, and trains the prediction model by maximizing an approximate likelihood derived via Taylor approximation. Furthermore, [[P19](#K19)] derives an efficient numerical method for computing this approximate likelihood based on theoretical analysis.

**Impact**: [[P19](#K19)] is the first method for learning nonlinear models from averaged data, specifically targeting two-sample averaged data.

**Remark** [[P12](#K12)] is a Japanese article explaining [[P19](#K19)].



## References

[<a id="K12">P12</a>] Ryuta Matsuno, Akira Kitaoka, Keita Sakuma, Masakazu Hirokawa, Learning Nonlinear Regression Model from Two-Sample Averaged Data (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/2L4-GS-1-05/advanced), 2L4-GS-1-05, May 28, 2025.  \[[Paper](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_2L4GS105/_article/-char/ja/)\]

[<a id="K19">P19</a>] Ryuta Matsuno, Akira Kitaoka, Learning from Two-Sample Averaged Data, [ICDM 2025](https://www3.cs.stonybrook.edu/~icdm2025/index.html), November 12, 2025. [[Paper](https://ieeexplore.ieee.org/document/11391798)] [[Video presentation](https://video.computer.org/ICDM25/10ME7GxDtJrOktMO8JeVyl-DM531-Matsuno.mp4.html)]

[<a id="Bhomik2016">Bhowmik+ 16</a>] Bhowmik, A., Ghosh, J., and Koyejo, O.: Sparse Parameter Recovery from Aggregated Data, in International Conference on Machine Learning (2016)

[<a id="Gelman2001">Gelman+ 01</a>] Gelman, A., Park, D., Ansolabehere, S., Price, P., and Minnite, L.: Models, assumptions and model checking in ecological regressions, Journal of the Royal Statistical Society. Series A: Statistics in Society, Vol. 164, No. 1 (2001)

[<a id="Robinson1950">Robinson 50</a>] Robinson, W. S.: Ecological Correlations and the Behavior of Individuals, American Sociological Review, Vol. 15, No. 3 (1950)

[<a id="Zhang2020">Zhang+ 20</a>] Zhang, Y., Charoenphakdee, N., Wu, Z., and Sugiyama, M.: Learning from Aggregate Observations, in Advances in Neural Information Processing Systems, Vol. 33 (2020)

<a href="{{ '/research_interest' | relative_url }}">Back to Research Interest>></a>
