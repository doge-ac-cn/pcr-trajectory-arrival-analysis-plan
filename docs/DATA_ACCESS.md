# Data access

| source | role | access | license |
|---|---|---|---|
| BreastDCEDL v3 (`min_crop`) | development cohort, official split | Zenodo 10.5281/zenodo.18114231 | CC BY-NC 4.0 |
| NACT-PILOT (TCIA `BREAST-MRI-NACT-Pilot`) | external test 1 (pending patient-ID cross-check) | TCIA, DOI 10.7937/K9/TCIA.2016.QHsyhJKy | CC BY 3.0 |
| MAMA-MIA | not used in the frozen specification (see ANALYSIS_PLAN §2) | Synapse syn60868042 | CC BY-NC 4.0 |

The official test split of BreastDCEDL is evaluated once, after the final model and
conformal layer are frozen. External cohorts are evaluated once, on the frozen model,
with confidence-first reporting. NACT-PILOT additionally requires a documented
patient-ID cross-check against I-SPY1/2 before any use.

No patient-level data are redistributed by this project. The public archive contains
only aggregate statistics, verification digests, and code.
