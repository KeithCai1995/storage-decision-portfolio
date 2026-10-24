# Checks and reproducibility

The suite has 35 tests covering battery constraints, empirical CVaR, configured risk levels, shared scenario ranks, input validation, output paths and fresh versus reused study results.

On 24 October 2026, the publication copy passed all 35 tests. The release verifier
passed all 18 table comparisons with zero numerical difference and checked 3,486
saved/rerun schedules. The complete verifier ran from a byte-identical short-path
Windows copy; the deeply nested preparation folder exceeded the Windows path
limit when temporary figure names were appended.

Independent saved-result checks also verified 996 supplementary strategy schedules,
83 idle schedules, 12 metric groups and six paired comparisons. All 249 nested
uniform draws were regenerated; their prefix equality and recorded hashes matched.
These checks did not rerun the full N=60/200 optimization study or replace its
dated timings. Core code and numerical reference files were byte-checked against
the source revision. These local results are separate from this repository's CI.

From the repository root:

```text
python -m pip install -r requirements-lock.txt
python -m unittest discover -s tests -v
python scripts/verify_release.py
```

The release verifier reruns three reference risk weights, compares six tables per run at absolute tolerance 1e-8, and checks all saved and rerun schedules for the full battery constraints. It uses temporary output, leaving supplied reference results unchanged.

## Supplementary results

`corrected_v110/supplementary_20261021/` contains paired N=60/200 results and the idle baseline. Its dated study covers 996 strategy-day schedules and 83 idle schedules: 25,896 periods with zero full-constraint violations. These are the saved study's checks, not an assertion that the full supplementary experiment has just been rerun. For fresh execution, use the [README commands](README.md#reproduce).

## This repository's checks

The [GitHub Actions page](https://github.com/KeithCai1995/storage-decision-portfolio/actions) reports checks for this repository's commits. The workflow runs on Ubuntu and Windows with Python 3.11, 3.12 and 3.13; every job runs the tests and release verifier. Development-repository runs are not substitutes for a successful run on the published commit.

Earlier execution records retain their experiment dates and source-revision hashes. They are not inventories of the current distribution. [PUBLICATION.md](PUBLICATION.md) explains the snapshot manifest and the omitted development archives.
