# Protocol amendments

These documents amend the frozen analysis plan (`analysis_specs/ANALYSIS_PLAN.md`).
Each amendment was **locked before the outcome-armed experiment it governs was
executed** and is reproduced here byte-for-byte; its sha256 is recorded in
`checksums/SHA256SUMS.txt`.

| file | scope | locked |
|---|---|---|
| `AMENDMENT_2026-10-11_split-semantics-and-e0-gate.md` | official split column semantics pinned (test == 1); E0 gate outcome recorded (the released weights fail the 0.72 anchor); the pipeline-validity gate redefined as a re-implementation under the principal investigator's Option A | locked 2026-10-11 |
| `AMENDMENT_2026-10-11_test-eval-incident.md` | disclosure: the E0b script evaluated the official test split during baseline-1 development (5 additional passes); an `EVAL_TEST` gate now blocks test evaluation outside the frozen protocol | locked 2026-10-11 |

Reading notes:

- **The amendments were not prospectively pre-registered.** They were locked after the
  E0-gate events (a failed reproduction attempt and its dual audit) and **before**
  outcome-armed execution (E5 three-arm, E6 conformal, E11 freeze). The frozen plan
  (§15) anticipated amendment-driven governance from the start.
- All development results referenced in the amendments (frozen embeddings, the five-seed
  main-model run, the baseline panel) predate the amendments and use train/validation data
  only. The official test split was evaluated twice before the incident (pipeline
  reproduction; gate validation) and five times during it; the single frozen evaluation
  supersedes all of these.
- References to internal working documents have been removed from the public text; the
  dated adjudication point (2026-11-15) is the only remaining internal-protocol term.
- Typographic correction on record: `splits/SPLIT_VERIFICATION.md` cites its input-
  provenance note as a `.md` file; the released file is `checksums/INPUT_PROVENANCE.txt`.
  The citation is left as printed because the file is OpenTimestamps-stamped.
