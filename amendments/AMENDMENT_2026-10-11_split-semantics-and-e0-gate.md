# AMENDMENT 2026-10-11 — split-column semantics pinned; E0 gate outcome; pipeline-validity gate redefined

**Scope:** §2 (Data) and §3 (primary endpoints) of the frozen plan.
**Locked:** 2026-10-11, before E5 (three-arm), E6 (conformal), or E11 (freeze) were executed.

## A. Official split-column semantics (pinned)

The semantics of the metadata column `test` are pinned to: **0 = train (1,099),
1 = test (176), 2 = validation (177)**, on the 1,452 labeled rows.

Evidence: the official results notebook `plot_pCR_article_results.ipynb` (cell 5)
selects the article's test set with `df[df['test']==1.0]`, and the resulting sizes
(1,099 / 177 / 176) match the published train/validation/test split. An earlier
internal working note had the 1/2 mapping reversed; the corrected mapping is
authoritative and verification digests are in `splits/SPLIT_VERIFICATION.md`.

## B. E0 gate outcome (recorded, with attribution)

The pre-registered pipeline gate — reproduce the official ViT result (overall test
AUC ≈ 0.72 ± 0.02) from the released weights and released min-crop volumes — **failed**
(overall 0.526; aggregation variants 0.517–0.533). A line-by-line audit reproduced the
official inference notebook's own demo outputs to four decimals and identified that the
released checkpoint's classification head is at random initialization; additional
release defects (z-axis/CSV coordinate mismatch; a missing mask folder for one cohort)
are documented in a public issue on the dataset authors' repository.

**Attribution:** the failure is attributable to the release, not to the local pipeline.

## C. Pipeline-validity gate redefined (Option A)

The pre-registered gate presumed the 0.72 anchor was reproducible. The principal
investigator ruled (Option A): the pipeline-validity gate is redefined as follows —

1. Baseline 1 (official ViT, imaging-only) is a **re-implementation** trained by us
   under the released data and a documented protocol; the paper-reported numbers
   (0.72 / 0.85 / 0.94-as-referenced) are cited as *reported* values with a footnote
   describing this audit.
2. Gate criterion: the re-implementation trains to convergence and reaches
   **val AUC ≥ 0.68**. Outcome of the first re-implementation round: 0.5709, computed
   on the official test split under the incident-tainted protocol (see the test-eval
   incident amendment) — recorded as insufficient; a regularized round and a
   seed-variance check are queued before the gate verdict is final.
3. The benchmark-hygiene audit (release defects above) is itself recorded as a
   citable finding, with a public issue on the authors' repository.

Nothing in this amendment changes any other locked element (endpoints, splits,
hyperparameters, conformal specification, interpretation grid).
