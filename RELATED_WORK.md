# Method sources and contribution boundary — v1.1.0

The mean–CVaR risk representation and the MILP solver are established methods. This benchmark integrates them in a reproducible scheduling experiment and empirical comparison; it does not introduce a new risk measure or storage optimization method.

Rockafellar and Uryasev's CVaR representation supplies the auxiliary threshold/excess-loss objective. The exact empirical fixed-mass estimator is additionally verified against an independent LP in `tests/test_cvar.py`: [Optimization of Conditional Value-at-Risk (2000), author copy](https://sites.math.washington.edu/~rtr/papers/rtr179-CVaR1.pdf).

SciPy supplies the mixed-integer interface, including explicit integrality and bounds: [official `scipy.optimize.milp` documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.milp.html). The underlying solver is [HiGHS](https://highs.dev/). Local experiments used SciPy 1.17.0.

Common Gaussian factors, quantile interpolation and clipped interval-width scaling are transparent scenario heuristics. This benchmark does not prove calibrated joint paths or conformal guarantees for decisions. The oracle uses future prices. The timing record does not establish operational day-ahead forecast availability.

The report uses the directly relevant primary method sources above. No first-of-its-kind claim is made.
