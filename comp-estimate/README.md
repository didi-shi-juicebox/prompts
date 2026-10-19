# Comp estimate: per-bucket prompts

One prompt per coverage bucket, so each edge case can be tuned and evaluated on its own.
Buckets are defined in [coverage-buckets.md](coverage-buckets.md).

The pipeline lives in `juicebox-data/databricks/experiments/comp_estimate_poc` (prompts in
`src/comp_poc/prompts.py`). These files are the working drafts; port them into `prompts.py` to run them.

## Layout

```
baseline/        the POC prompts as of juicebox-ai#5018@40817f38+comp-components-v3, verbatim; do not edit
  choose_titles_and_levels.md, choose_job_levels.md   selection prompts, unchanged and shared by all buckets
  compensation_estimate.md                            the single estimate prompt being split up
shared/
  estimate_core.md   task, pay components, confidence rubric, output format, general rules; slots for the bucket
  comp_priors.md     pay structure by industry, unchanged from baseline
buckets/             one file per bucket (see below)
```

## How an estimate prompt is assembled

`shared/estimate_core.md` is a Mustache template with two partials:

- `{{> bucket_guidance}}` → the bucket's file in `buckets/`
- `{{> comp_priors}}` → `shared/comp_priors.md`

`{{profile_data}}` and `{{compensation_data}}` are filled per profile as today. Everything fixed comes before
them, so the prefix up to `<profile>` stays cacheable per bucket. The output format matches baseline, so
`port/llm_processor.py` and the judge work unchanged.

What moved where, vs baseline: the extrapolation definition and the "reading the rows" rules that apply to every
bucket went into `<rules>` in the core. The rules that were really about one case (sparse data, wrong job family,
other country, vague/non-English titles, intern/contract roles) moved into that bucket's file.

## Bucket → prompt

| Bucket | File | Kind | Notes |
| --- | --- | --- | --- |
| 1 Ready (levels.fyi) | `1_ready_levelsfyi.md` | estimate module | closest to baseline |
| 1 Ready (Minerva) | `1_ready_minerva.md` | estimate module | Minerva rows: no levels, Min/Median/Max, mixed Additional Pay |
| 2A Thin: one group | `2a_thin_one_levelsfyi_group.md` | estimate module | |
| 2B Thin: Minerva, other titles | `2b_thin_minerva_other_titles.md` | estimate module | |
| 2C Thin: title doesn't map | `2c_thin_title_no_family_match.md` | estimate module | always "Extrapolated:" |
| 3 No data in country | `3_no_data_in_country.md` | estimate module | always "Extrapolated:" |
| 4 Fixable profile | `4_profile_cleanup.md` | **standalone, runs first** | normalizes title/seniority/experience, then the profile is re-bucketed; no pay estimate |
| 5 Hard role | `5_hard_role_classify.md` | **standalone, runs first** | picks a category; `regular` re-buckets normally, `student`/`retired` → no estimate |
| 5 → intern, part-time, contract | `5_hard_role_annualize.md` | estimate module | needs `{{hard_role_category}}`, `{{hours_assumption}}` from the classifier |
| 5 → founder, owner, self-employed | `5_hard_role_founder_owner.md` | estimate module | needs `{{hard_role_category}}`, `{{company_size}}` |
| 5 → executive | `5_hard_role_executive.md` | estimate module | needs `{{company_size}}` |
| 6A Minerva global average only | `6a_minerva_global_average_only.md` | estimate module | **new pipeline path**: today these profiles are ineligible |
| 6B In neither source | `6b_no_source_data.md` | estimate module | **new pipeline path**, empty `compensation_data` |
| 7 No current role | – | – | no estimate |

Each module states an expected confidence range; the core tells the model to stay inside it.

## Versioning

Runs record `SOURCE` from `prompts.py`. Give each bucket its own version suffix (e.g. `bucket-2b-v1`) and bump
only the bucket you changed, so runs on other buckets stay comparable.

## Status

All bucket files are first drafts (2026-10-19), not yet run. Next steps:
1. Compute the bucket per profile next to source routing in `port/eligibility.py`, and record it on each run.
2. Build a per-bucket eval set: ~30-50 real profiles per bucket from the 1% sample.
3. Run baseline vs the bucket prompt on each set; judge scores per bucket.
