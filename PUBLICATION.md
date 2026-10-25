# Distribution contents

This repository contains the source, tests, configurations, simulated forecast inputs, main-reference results, supplementary scenario-count study and report needed to read and reproduce the current study. The main-reference baseline is under `corrected_v110/baseline/`.

## File inventory and experiment records

- `PUBLICATION_MANIFEST.json` inventories the distributed files with SHA-256 hashes and excludes itself from its own inventory.
- Run manifests under `corrected_v110/` describe each experiment's configuration, software, inputs and source snapshot at execution time. Their dates and source hashes describe those runs, not the entire current distribution.
- `corrected_v110/supplementary_20261021/study_manifest.json` records the supplementary study's execution status, sampling design and input checks.
- [VERIFICATION.md](VERIFICATION.md) gives current check commands and links to local check records. Repository CI results apply to the commit actually checked.

`<WORKSPACE>` is a redacted machine-specific path prefix, not an executable directory. New runs may record their own paths; the input hashes identify the data independently of these paths.

The baseline forecast file is byte-identical to the corresponding baseline export in the [P1 portfolio](https://github.com/KeithCai1995/electricity-price-forecasting-portfolio). See the [input contract](data/README.md) for its SHA-256 and information-timing limitation.
