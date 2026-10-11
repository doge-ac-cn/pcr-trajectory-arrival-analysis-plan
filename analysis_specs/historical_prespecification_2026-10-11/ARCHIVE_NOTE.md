# Historical pre-specification, as archived 2026-10-11

`ANALYSIS_PLAN_v1.0_ZH_as_archived.md` is the v1.0 analysis plan as first archived on
2026-10-11 (single root commit `a88020c` of the predecessor repository
`pcr-trajectory-arrival-analysis-plan`, pushed to GitHub before any outcome-armed
experiment ran). Working language at freeze time was Chinese; the authoritative English
text is `analysis_specs/ANALYSIS_PLAN.md`.

## Redaction and parity notes

- The archived Chinese file has been **redacted for public release**: internal working
  references (document numbers and internal gate labels) are replaced by
  `[internal reference removed]`. The unredacted original is retained in the internal
  record; its sha256 is `c89f28ea6d7b92ed0ecef21407d8162064d4454f622b2f0f45a0035e1a3192ee`
  (recorded here for provenance; the redaction itself is covered by the amendment log).
- One stale header statement in the Chinese original ("test column == 2, n = 176") predates
  the split-semantics correction; the authoritative mapping (0 = train, 1 = test, 2 =
  validation) is stated in §2 of the English plan and in the first amendment.
- The English text contains three documented specification additions absent from the
  Chinese original: the nominal-observation-time disclosure (§4), the public-amendment
  governance rule for the European external cohort (§2), and the consolidated §15 pointer
  to `amendments/`. These are translation-time clarifications, listed here for parity.
- The internal development-phase errata table (nine rows) is mirrored in English as
  `amendments/AMENDMENT_2026-10-11_development-record.md`.
