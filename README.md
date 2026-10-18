# LandscapeSHAP

Code accompanying the paper **"LandscapeSHAP: Which Persistent Homology Class Gets the Credit?"** by Nikola Milićević ([arXiv:2609.31469](https://arxiv.org/abs/2609.31469) [math.AT]).

LandscapeSHAP allocates credit for a model's prediction to the individual points of a persistence diagram (i.e., to persistent homology classes) using Shapley values. Each coordinate of a persistence landscape is an order statistic, and the paper shows that it defines a cooperative game on the diagram points. This gives

- an exact closed-form Shapley value for every diagram point (no coalition sampling),
- the exact pullback of a linear model on landscapes to diagram points,
- a Monte Carlo permutation estimator, with a sample-complexity bound independent of the number of diagram points, for nonlinear models,
- stability of the credit allocation with respect to the bottleneck distance.

## Notebooks

| Notebook | Paper section | What it does |
|---|---|---|
| `landscapeshap-curvature-average.ipynb` | 6.1 | Curvature regression: linear SVR on average landscapes of point clouds sampled from discs of constant curvature; exact LandscapeSHAP values and representative cycles. |
| `linear-exact-dynamics-landscapeshap.ipynb` | 6.2.1 | Dynamical-system (linked twist map) classification with a linear SVM; exact Shapley values and representative cycles. |
| `linear-shapley-monte-carlo.ipynb` | 6.2.2 | Monte Carlo estimator (Algorithm 3) against the exact linear values; convergence in the number of permutations. |
| `nonlinear-svm-landscapeshap.ipynb` | 6.2.3 | RBF-kernel SVM on the same data; Shapley values estimated with Algorithm 3 (3000 permutations). |

The code for the closed form (Theorem 3.3, Algorithms 1 and 2) is inside the exact-value notebooks.

## Requirements

Python 3 with:

```
numpy
matplotlib
scikit-learn
gudhi
ripser
```

Install with `pip install numpy matplotlib scikit-learn gudhi ripser`.

The notebooks were run on Kaggle. Some of them are computationally heavy (the curvature experiment computes persistent homology for 101 curvature values x 100 repeats of 1000-point clouds, and the nonlinear notebook runs 3000 permutations per diagram), so running them on a hosted notebook service is convenient.

## Reproducibility

- All random number generators are seeded with `42`.
- Dynamical systems: five values of `r` in {2, 3.5, 4, 4.1, 4.3}, 100 point clouds of 1000 points each, alpha-complex persistent homology in dimension 1 (GUDHI), 15 landscape layers.
- Curvature: curvatures from -2 to 2 in steps of 0.04, 100 clouds of 1000 points each per curvature, Vietoris-Rips persistent homology in dimension 1 (`ripser`), 25 landscape layers, one averaged landscape per curvature.
- Landscapes are evaluated on a uniform grid of 100 points on `[0, G]`, where `G` is 1.1 times the 99th percentile of the observed death values. Diagram points with persistence below 2% of `G` are removed.
- Features are standardized with a scaler fit on the training split (75% / 25% train/test split). Weights of the linear models are mapped back to the original landscape coordinates by dividing by the scaler's standard deviations.
- Models (scikit-learn): `LinearSVR(C=1, epsilon=0, max_iter=20000)`, `LinearSVC(C=1, multi_class="ovr", max_iter=20000)`, and `SVC(kernel="rbf", C=10, gamma="scale")`. No hyperparameters were tuned.
- Convergence study: 8 independent runs at n = 10, 30, 100, 300, 1000, 3000 permutations.

See Section 6.3 (Hyperparameters) of the paper for the full description.

## Citation

```bibtex
@misc{milicevic2026landscapeshap,
  title         = {LandscapeSHAP: Which Persistent Homology Class Gets the Credit?},
  author        = {Nikola Milićević},
  year          = {2026},
  eprint        = {2609.31469},
  archivePrefix = {arXiv},
  primaryClass  = {math.AT}
}
```

## Acknowledgement

This material is based upon work supported by the National Science Foundation under Grant No. DMS-2424556 while the author was in residence at the Institute for Computational and Experimental Research in Mathematics in Providence, RI, during the Foundations of Computational Geometry and Topology workshop program. Claude (Anthropic) was used to assist in developing the code.

## License

Add a license of your choice (e.g., MIT) as `LICENSE`.
