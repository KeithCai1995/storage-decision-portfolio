# Publication snapshot — 24 October 2026

This distribution retains the active source, tests, configurations, simulated forecast inputs, corrected v1.1.0 results, supplementary study and report. The source revision is `1bf98ea20ee6b1fa1eba4ab8e8548894ca059d02`, merged into the development repository as `7dee77bafc0550b2fb597c2b560b1d0bef681a31`. Publication edits update navigation and report context; calculation code and numerical reference results are retained.

The new repository starts with one snapshot commit. It does not include earlier Git commits, the superseded `historical_v102/` archive, the development `evidence/` directory or the duplicate root `outputs/` baseline. The baseline is distributed once under `corrected_v110/baseline/`. The earlier dated-audit script is also omitted because it writes into the development evidence directory. Current verification uses `scripts/verify_release.py`.

The [development repository](https://github.com/KeithCai1995/p2-storage-decision-under-forecast-uncertainty) and its history remain separate. References to earlier results in the report describe earlier experiments, not additional files distributed here.

## Manifests and dated records

- `PUBLICATION_MANIFEST.json` inventories this publication's files with SHA-256 hashes. It excludes itself from its own inventory.
- Run manifests under `corrected_v110/` describe the relevant experiment's configuration, software, inputs and source snapshot. Their dates and source hashes were not changed to make them describe the entire current repository.
- `corrected_v110/supplementary_20261021/study_manifest.json` records that study's execution status, sampling design and input checks.

`<WORKSPACE>` is a redacted machine-specific path prefix, not an executable directory. It does not change input hashes or experiment results. New local runs may record their own paths.

The baseline forecast file is byte-identical to the corresponding corrected baseline export in the [published P1 portfolio](https://github.com/KeithCai1995/electricity-price-forecasting-portfolio). See the [input contract](data/README.md) for its SHA-256 and information-timing limitation.

Current verification commands are in [VERIFICATION.md](VERIFICATION.md). GitHub Actions results belong to the commit checked in this repository; checks on the development repository do not replace them.
