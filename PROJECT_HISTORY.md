# Project background

This repository publishes the current research benchmark as a fresh snapshot. Its initial commit is a publication date, not the start of the project. The [development repository](https://github.com/KeithCai1995/p2-storage-decision-under-forecast-uncertainty) retains the earlier Git history and versions separately.

## Development milestones

- **18 October 2026 — corrected v1.1.0:** exclusive battery operation, corrected P1 forecast inputs, paired scenarios and expanded verification.
- **19 October — personal rerun:** 20 tests, the default baseline and the three-weight release comparisons were run locally. This was not personal execution of the full sensitivity grid.
- **20 October — alpha-reporting fix:** adaptive risk-neutral scheduling now reports in-sample CVaR at the configured `risk.cvar_alpha`, without adding a risk penalty to its optimization. Regression tests cover alpha .80, .90, .95 and .99.
- **21 October — technical update:** safer output paths, explicit fresh-versus-reference study modes, and a paired N=60/200 supplementary study with an idle baseline. The original seven-strategy reference results were retained. Battery sensitivity is labelled as median risk-neutral scheduling over the first 28 days.
- **21–22 October — presentation and readability:** machine-specific path prefixes were redacted as `<WORKSPACE>`, and documentation and comments were simplified. Experiment dates and numerical results were retained.
- **24 October — publication snapshot:** active code, tests, simulated inputs, corrected results and the report were copied into this repository. Earlier Git commits, superseded material and development logs are not included; the original repository was not deleted or rewritten.

The snapshot is based on source revision `1bf98ea20ee6b1fa1eba4ab8e8548894ca059d02`, merged into the development repository as `7dee77bafc0550b2fb597c2b560b1d0bef681a31`. Publication edits update navigation and report context, not the scheduling calculations or numerical reference results.

## Sources

Initial code and reference analyses were supplied. The project integrates established forecasting and optimization methods; [RELATED_WORK.md](RELATED_WORK.md) credits the relevant method sources. The [P1 portfolio](https://github.com/KeithCai1995/electricity-price-forecasting-portfolio) supplies the simulated probabilistic forecasts used here. Dated run manifests describe their original execution snapshots; [PUBLICATION.md](PUBLICATION.md) describes this distribution.
