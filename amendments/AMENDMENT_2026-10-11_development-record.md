# AMENDMENT 2026-10-11 — development-phase record (internal errata, mirrored)

**Scope:** documentation of development-phase decisions and findings produced between
the freeze (§15 baseline) and the outcome-armed experiments. None of these entries
changes a locked element of the specification.

| id | entry | disposition |
|---|---|---|
| a | MAMA-MIA cohort composition: the upstream protocol's appendix figures (ISPY2 960 / NACT 84) are a transcription error; the published Table 1 figures (ISPY2 980 / NACT 64) are authoritative, cross-checked against the Synapse archive | manuscript and statements use the published figures |
| b | split-column semantics reversed in an internal note (1/2 swapped); pinned by evidence to 0 = train (1,099), 1 = test (176), 2 = validation (177) | corrected in the plan text; verification digests updated |
| c | E0 gate outcome: the released weights fail the 0.72 anchor (0.526 overall); dual audit attributes the failure to the release (untrained classifier head; coordinate mismatch) | pipeline halt executed; gate redefined by ruling (Option A, below); public issue filed on the dataset authors' repository |
| d | release defects recorded: z-axis/CSV coordinate mismatch (ISPY2 959/982 follow mask-span+4); one cohort's min-crop ships no masks (bbox pseudo-mask used for 915 cases, flagged in a `mask_source` column); demo notebook operates on noise-image semantics | documented as citable findings |
| e | pipeline-validity gate (Option A): re-implementation round 1 computed 0.5709 on the official test split under the incident-tainted protocol (see the test-eval-incident amendment); recorded as insufficient; a regularized round and seed-variance check queued before the gate verdict is final | gate verdict pending |
| f | frozen embeddings extracted for three encoders × 2,070 patients; linear probe selects the retrained encoder (val 0.540); imaging-only linear signal ~0.5–0.6, below reported values — makes the three-arm comparison (E5) decisive and the risk-4 contingency active | recorded |
| g | five-seed main-model run: val 0.6204 ± 0.0074; baseline panel: clinical-only LR 0.6853 leads all imaging models (0.51–0.63) on validation | recorded; motivates the three-arm design |
| h | one case (public dataset ID `Breast_MRI_577`) excluded: bounding-box fields absent in the source metadata; ROI features cover 2,069 × 41 columns; one cohort's min-crop release ships no masks (bbox pseudo-mask used for 915 cases, flagged in a `mask_source` column) | documented |
| i | European-cohort working default set to "unavailable" (see ANALYSIS_PLAN §2); the organizer inquiry remains an option, not an assumption | recorded |
