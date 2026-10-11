# AMENDMENT 2026-10-11 — disclosure: official test split evaluated during baseline development

**Scope:** external-test discipline (highest-priority rule of the frozen plan).
**Locked:** 2026-10-11, before E5 (three-arm), E6 (conformal), or E11 (freeze) were executed.

## What happened

The evaluation harness of baseline 1 (the re-implemented official ViT, running across
five development seeds) computed test-split metrics at the end of every run, because the
script inherited the report section of the earlier gate validation, where a single test
evaluation was the sanctioned endpoint. As a result the official test split was evaluated
**five additional times** during development (seven passes in total, counting the two
sanctioned gate validations).

## Mitigating facts

- Every modeling choice (architecture, recipe, seeds) was fixed from validation data and
  the pre-registered plan **before** any of these test passes; no test number influenced
  any selection decision.
- All seven passes evaluated the same frozen configurations; no re-tuning against test
  occurred.

## What cannot be claimed

That seeing the per-seed test numbers had no influence on subsequent judgment. This
disclosure is made because that influence cannot be excluded retroactively.

## Disposition (ruled by the principal investigator)

1. An `EVAL_TEST` gate is now required for any test evaluation in the released code;
   the default is to never touch the test split.
2. The **final reported test numbers** for the main model and all baselines will come
   from a single clean frozen evaluation executed after all development is complete,
   under the pre-registered 5-seed protocol (per-seed forward passes of the frozen
   configuration; mean ± std, plus the 5-seed mean ensemble).
3. The per-seed development-time test numbers remain in the internal record and are
   superseded by the frozen evaluation.
