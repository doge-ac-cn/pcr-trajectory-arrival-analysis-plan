# Pre-registered analysis plan (frozen) — trajectory-arrival pCR prediction with group-conditional conformal risk control

**Manuscript:** in preparation (target: methods-oriented journal; venue per pre-registered decision grid)
**Status:** ✅ **COMMITTED AND PUBLICLY ARCHIVED — this is the pre-specification of record.**
**Version:** v1.0 (frozen), archived 2026-10-11.

```
GIT COMMIT (this repository):        see `git log --format=%H` — single root commit
ARCHIVED (UTC):                      2026-10-11T13:00Z (repository push)
PLAN sha256:                         see checksums/SHA256SUMS.txt (ANALYSIS_PLAN.md row)
SPLIT VERIFICATION:                  splits/SPLIT_VERIFICATION.md (sha256 digests + recomputation)
THIRD-PARTY TIMESTAMP:               OpenTimestamps, Bitcoin-anchored (pending):
                                     analysis_specs/ANALYSIS_PLAN.md.ots,
                                     checksums/SHA256SUMS.txt.ots,
                                     splits/SPLIT_VERIFICATION.md.ots
```

**No outcome-armed experiment had been run at this commit.** The three-arm clinical-increment
comparison (E5, including the `both` arm), the conformal layer (E6), and the frozen test-set
evaluation (E11) had **not** been executed when this specification was archived. Their
interpretation grid was fixed here, in §9, before their results existed.

## Associated work

This archive documents the analysis specification for a study on early prediction of
pathologic complete response (pCR) during neoadjuvant therapy in breast cancer, framed as a
**trajectory-arrival** problem: a condition-injected latent neural ODE propagates irregularly
sampled DCE-MRI timepoints, and a group-conditional conformal risk-control layer (RCPS/LTT)
attaches finite-sample, subgroup-wise upper bounds on the non-response rate to
de-escalation recommendations.

It is informed by, and explicitly delineated from, the concurrent re-analysis of Nakano &
Shiinoki (2026), *J Imaging Inform Med*, DOI 10.1007/s10278-026-02344-w, which asked a
different question (treatment-effect *prioritization* via RATE/AUTOC) on overlapping data
(I-SPY2 via the MAMA-MIA curation). The mechanism of this archive — frozen analysis
specification, amendments, split-verification digests — follows the practice of that group's
public analysis archive.

## What is included

| folder / file | content |
|---|---|
| `analysis_specs/ANALYSIS_PLAN.md` | the frozen analysis plan v1.0 (English): endpoints, model, locked hyperparameters, conformal specification, metrics, statistical procedures, E0–E11 experiment verdicts, pre-registered interpretation grid |
| `analysis_specs/historical_prespecification_2026-10-11/` | the v1.0 plan as first archived (working language: Chinese), byte-preserved as a historical record |
| `amendments/` | protocol amendments locked after the E0 gate events and before outcome-armed execution, each with scope and hash; plus the amendment log |
| `checksums/` | sha256 manifests of this release and of the frozen inputs the analysis consumes |
| `splits/SPLIT_VERIFICATION.md` | the official split (train 1,099 / test 176 / val 177, used as-is without re-splitting) as verification digests with recomputation commands — the split is defined by the public metadata file, not distributed as identifier tables |
| `docs/DATA_ACCESS.md` | data sources, licenses, access conditions |
| `docs/REPRODUCTION.md` | how the specification maps to the released code and what remains internal until manuscript submission |

Patient-level data are **not** redistributed and remain subject to the access conditions of
the original repositories. One public dataset identifier (`Breast_MRI_577`) is named in the
amendment log solely as a documented exclusion (missing bounding-box fields in the source
metadata); no other case-level identifier appears in this archive.

## License

[CC BY 4.0](LICENSE) — Honglin Zhu (ORCID 0009-0003-6314-4714), University of Shanghai for
Science and Technology, School of Health Science and Engineering.
