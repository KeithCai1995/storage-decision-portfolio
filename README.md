# Forecast uncertainty and battery scheduling

A simulated research benchmark connecting probabilistic electricity-price forecasts to 24-hour battery schedules. SciPy/HiGHS mixed-integer programming enforces exclusive charging and discharging. The experiments compare risk-neutral and CVaR policies, using shared scenario draws to examine how forecast uncertainty changes decisions.

This is the 24 October 2026 publication snapshot of corrected v1.1.0. It uses forecasts from [P1: electricity-price forecasting](https://github.com/KeithCai1995/electricity-price-forecasting-portfolio). All prices and profits are simulated; no employer or trading data are used. [Project history](PROJECT_HISTORY.md) describes the development background and sources.

## Start here

- [Technical report](report/report_v110.md) and [PDF](report/Haorui_Cai_Storage_Report_v110.pdf).
- `corrected_v110/baseline/`, `cvar_weight_070/` and `cvar_weight_085/`: the seven-strategy reference results at three risk weights.
- `corrected_v110/multiseed/`: scenario-draw seeds on the same simulated market and forecasts.
- `corrected_v110/market_seeds/`: three simulated P1 markets at weight .55, with a fixed scenario seed.
- [Supplementary study](corrected_v110/supplementary_20261021/README.md): an idle baseline and paired N=60/200 static/adaptive CVaR runs, with three seeds and all 83 days.
- [Verification](VERIFICATION.md), [input contract](data/README.md) and [publication scope](PUBLICATION.md).

## Main findings

Risk adjustment changes both profit and downside risk; there is no consistently best policy. In the supplementary scenario-count study, N=200 improves mean profit and empirical CVaR loss in all six paired comparisons. That does not mean every risk metric improves: adaptive seed 20260817's fifth-percentile daily profit falls from -18.345239 to -20.323186 EUR. Two scenario counts and three seeds do not establish convergence.

The no-transaction alternative has zero profit and zero loss. It helps distinguish risk reduction from a claim that every trading strategy beats doing nothing on every day.

## Model and sampling

Each period has a binary mode: `charge <= power * mode` and `discharge <= power * (1 - mode)`. SOC transitions, capacity and initial/terminal SOC are enforced in the optimizer. Schedules are not clipped or repaired afterwards. The default formulation is `"milp"`; `optimise_schedule(..., formulation="lp_relaxation")` runs the continuous comparison model.

The negative-price regression shows why exclusivity matters. At -100 EUR/MWh, efficiencies .94, initial/terminal SOC 1 MWh and throughput cost 2 EUR/MWh, the LP charges 1 MW and discharges .8836 MW simultaneously, earning 7.8728 EUR. The MILP stays idle and earns zero.

Empirical CVaR averages exactly the worst `1-alpha` probability mass, including a fractional boundary observation. At alpha .90, the tail contains six observations for 60 scenarios and 8.3 observations for 83 evaluation days. Tests compare the estimator with an independent Rockafellar–Uryasev LP, including tied losses.

Raw, static and adaptive modes share latent uniforms within each day, seed and scenario count; frontier weights reuse the same draws. The Gaussian common-factor loading .65 gives exchangeable latent cross-hour correlation .4225. Seven quantiles are interpolated and rescaled about the median using `clip(width / raw_width, .35, 4)`. Manifests record capped widths and infinite source intervals.

## Reproduce

Use Python >=3.11 and run commands from this repository's root:

On Windows, use a short checkout path: release verification appends temporary
subdirectories, which can exceed the path limit inside deeply nested folders.

```text
python -m pip install -r requirements-lock.txt
python -m unittest discover -s tests -v
python scripts/verify_release.py
```

The release checker reruns three risk weights, compares six tables per run with the supplied references at absolute tolerance 1e-8, and recalculates full schedule diagnostics. Fresh experiments use separate output directories:

```text
python scripts/run_experiment.py --config configs/base.yaml --output-root local_runs/baseline
python scripts/run_experiment.py --config configs/cvar_weight_070.yaml --output-root local_runs/cvar_weight_070
python scripts/run_experiment.py --config configs/high_risk_aversion.yaml --output-root local_runs/cvar_weight_085
python scripts/run_experiment.py --config configs/base.yaml --seed 20260818 --output-root local_runs/scenario_seed_20260818
python scripts/run_experiment.py --config configs/base.yaml --seed 20260817 --source-forecasts data/market_seed_20260818_p1_forecasts.csv --output-root local_runs/market_seed_20260818
python scripts/run_multiseed.py --output-root local_runs/scenario_seed_study
python scripts/run_market_seed_study.py --output-root local_runs/market_seed_study
python scripts/run_supplementary.py --output-root local_runs/scenario_count_study
```

Use a new `local_runs/` destination for each fresh run. Non-empty directories are rejected unless `--overwrite` is given for a recognized output directory of the same experiment type. It replaces known artifacts without deleting unrelated files; source and supplied reference paths stay protected.

For either seed-study script, `--reuse-reference` checks configuration and input hashes, aggregates the supplied runs, and records `reused_supplied_reference` in `study_manifest.json`. Without that flag, it runs a fresh grid. Supplied summaries are not overwritten.

The supplementary study draws 200 uniforms per day/seed and uses the first 60 for N=60, with shared ranks across both modes. This prefix differs from the earlier standalone N=60 draw. A two-day timing pilot is available with `--pilot-days 2`; it is marked incomplete.

To rebuild the PDF, install `requirements-report.txt`, then run `python scripts/build_report.py --output report/Haorui_Cai_Storage_Report_v110.pdf`. The source is `report/report_v110.md`; visually review the rebuilt PDF.

## Scope

Horizon 1 has `issue_time == target_time` on all 83 days. There is no separate prior-day publication or gate-closure timestamp, so this experiment does not establish operational day-ahead availability. The battery is price-taking, with perfect hourly execution and linear throughput cost. Network, reserve, bidding, imbalance and detailed degradation effects are omitted. The oracle uses future prices and is only an upper-bound diagnostic.

Quantile interpolation and clipped width rescaling are scenario heuristics, not calibrated marginal or joint distributions. They need not preserve interval endpoints or conformal coverage, including when an infinite interval is capped. Three scenario seeds, three simulated markets and two scenario counts do not establish real-market robustness or future risk reduction.

Battery capacity/efficiency sensitivity uses median risk-neutral scheduling on the first 28 days, not adaptive CVaR over all 83. The baseline input contains 1,992 corrected P1 forecasts; its SHA-256 and timing details are in the [input contract](data/README.md).
