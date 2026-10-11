# ANALYSIS PLAN v1.0 (FROZEN)

> **Status:** v1.0 frozen. Integrates the upstream protocol appendix (pre-registration
> points), method formalization, hyperparameter table, metrics, statistical procedures,
> registered tables/figures, and all pre-registered decision entries. Experiment IDs E0–E11.
> **The narrative-selection adjudication point (2026-11-15) freezes this plan together with
> the title/narrative choice**; any deviation afterwards is recorded in §15 (Amendments are
> mirrored in `amendments/`).
>
> **External-test discipline (highest priority):** the official test split (CSV `test`
> column == 1; n = 176) is run exactly once, after the final model + conformal layer are
> frozen. NACT-PILOT and the MAMA-MIA European cohort follow the same rule. All development
> decisions use train (1,099) / validation (177) only.

## 1. Adjudication rules and dual-track titles

- If the corresponding trial arm-data license (Sage Bionetworks / Project Data Sphere) is
  granted before the adjudication point (2026-11-15) → **Tier-2 main narrative** (title
  switches to the pre-registered upgraded version; arm embedding u(t) enters the vector
  field). Otherwise → **Tier-1 finalization**. Mid-review arrival → revision-stage addendum
  only.
- Tier-1 title: *Trajectory Arrival, Not Static Classification: Biomarker- and
  Time-Conditioned Neural ODEs for Early Prediction of Pathologic Complete Response in
  Breast Cancer DCE-MRI, with Group-Conditional Conformal Risk Control.* Tier-2
  pre-registered title kept in the internal record.
- Adjudication is executed by the principal investigator; the analysis support role only
  prepares materials.

## 2. Data (verified 2026-10-11)

- **BreastDCEDL v3** (Zenodo 10.5281/zenodo.18114231; 26.91 GB / 10 files, all MD5-verified):
  2,070 cases (spy2 = 982, duke = 916, spy1 = 172); 1,452 with pCR labels. Official split
  (metadata CSV `test` column, labeled subset): **0 = train 1,099, 1 = test 176, 2 = val
  177** (evidence: official results notebook `plot_pCR_article_results.ipynb` cell 5 uses
  `df[df['test']==1.0]`; sizes match the paper's train/val/test). **Used as-is, no
  re-splitting** (split files released with code). n_i ≥ 3 holds for all (duke/spy1 min 3,
  spy2 min 4); HR missing 2, HER2 missing 5 (missing masks enter c_i).
- **External test 1 — NACT-PILOT** (TCIA DOI 10.7937/K9/TCIA.2016.QHsyhJKy, CC BY 3.0):
  **n = 64 patients, locked from the descriptor text** (68 recruited, 4 excluded; 189
  studies; 2,791 series incl. SEG objects). UCSF pilot NACT study (1995–2002; 4×AC ±
  taxane in a 17-patient subgroup); timepoints MRI1/2/3 for all + MRI4 subgroup. Patient-ID
  cross-check against I-SPY1/2 is mandatory (first pass: zero overlap; final version from
  DICOM headers + protocol-lineage argument).
- **External test 2**: not part of this frozen specification. A European held-out cohort
  (MAMA-MIA) could be added only by a public amendment, and only if response-label access
  is actually granted; its absence is not backfilled by any substitute.
- **Forbidden**: MAMA-MIA non-I-SPY2 subsets (same-source leakage); the 56 GB
  full-resolution I-SPY2 subset (Zenodo 17578255); any reflux of the official test rows
  into training.

## 3. Primary and secondary endpoints

- **Primary**: overall AUROC and AUPRC on the official test set (Tier-1 model vs the
  official ViT + clinical 0.85 baseline; DeLong).
- **Key secondary**: imaging-dynamics ΔAUC over clinical features (three-arm ablation) and
  group-conditional conformal coverage attainment.
- Hard anchors: official ViT 0.72 / +clinical 0.85 (bold rows, Table 2). E0 gate =
  reproduce 0.72 ± 0.02; failure halts all modeling (deviation recorded — see amendments).

## 4. Model and loss

- Notation: patient i has observation times T_i = {t_i1 < … < t_i,n_i}, n_i ≥ 3;
  observation x_ij = frozen ViT embedding (768 → projection 128); y ∈ {0,1}
  (ypT0/is ypN0, per the BreastDCEDL data dictionary); conditioning vector
  b_i = (HR, HER2, race, menopause, age, baseline tumor volume [, MammaPrint with missing
  mask]); c_i = ψ_θ(b_i). T_i^end = max T_i (surgery-proximal proxy; sensitivity reported).
  Observation times are **nominal protocol times (weeks 0 / 3 / 12)**, per the AC schedule
  of the source trials; real acquisition dates are not carried in the min-crop release —
  this deviation is recorded in §15 and a timing-sensitivity analysis is deferred to the
  ablation program.
- Condition-injected ODE-RNN: z_i(0) = μ_θ(x_i1, b_i); between observations z evolves by
  f_θ(z, c_i, t) (2–3-layer tanh MLP, dopri5), with GRU evidence updates at observations.
  Tier-2 (pre-registered): f_θ(z, c_i, u_i(t), t).
- Trajectory arrival: response coordinate r_i(t) = h_φ(z_i(t)) with learnable threshold
  τ_φ; â = 1[r(T_end) ≥ τ]; P̂ = σ((r(T_end) − τ)/s), s learnable. Early readouts filter
  up to t_k.
- Loss: weighted BCE (pos_weight = n_neg/n_pos) + λ_mono monotone hinge on r +
  optional pairwise ranking.

## 5. Locked hyperparameters

| item | setting |
|---|---|
| features | A = frozen official ViT (patch-token mean, 768→128); B = engineered ROI features (volume/SER stats/GLCM 8–12/morphology, from official masks) |
| latent | z ∈ R⁶⁴; GRU hidden 128; readout 128→64→1 |
| ODE | dopri5, rtol 1e-3 / atol 1e-4, t in weeks |
| training | AdamW 1e-3, wd 1e-4, cosine, ≤100 epochs, val-AUROC early stop (patience 15), batch 64 patients, dropout 0.2, grad clip 1.0, weighted BCE, 5 seeds (0–4) |
| λ_mono | {0, 0.1, 0.5, 1}, tuned on validation folds, ablated |
| conformal | Λ = validation-quantile grid (99 points); α ∈ {0.05, 0.10}; δ = 0.05; δ_g = δ/K |

## 6. Conformal specification

- Groups G = {HR+/HER2−, HR+/HER2+, HR−/HER2− (TN), HR−/HER2+} (K = 4; pre-registered
  fallback K = 2 = HR+/HER2− vs rest, risk 5). De-escalation rule s ≥ λ;
  R_g(λ) = P(y = 0 | g, s ≥ λ).
- RCPS upper bound (Hoeffding; Bentkus reported as comparison):
  R̂⁺_g(λ) = R̂_g(λ) + sqrt(ln(1/δ_g)/(2 n_g(λ))); λ̂_g = inf{λ ∈ Λ : R̂⁺_g(λ) ≤ α_g}.
- **Three error-budget allocations (all ablated)**: equity (α_g = α); information-shrunk
  (α_g = α·n_g/(n_g + n₀), n₀ = median group size); utility-LP (argmax Σ E_g(α_g) s.t.
  Σ p_g α_g ≤ ᾱ, grid linear program). δ split across K groups by Bonferroni (joint FWER ≤ δ).
- LTT: fixed-sequence across (model m, λ); timepoint dimension δ/(K·T_max). Deliverable =
  the list of (g, k, m) combinations usable under guarantee.
- Primary figure: per-group empirical non-response rate vs nominal α (coverage);
  transportability of guarantees on external data reported as such (weighted conformal as
  sensitivity, not a primary claim).
- Dual (upgrade) rule s ≤ λ′: FNR-type bound, appendix.

## 7. Metrics and statistical procedures

- Discrimination: AUROC, **AUPRC (primary under imbalance)**, sens@95%spec; strata =
  overall × four receptor groups × cohort.
- Decision lead: per-timepoint AUC; **earliest reliable decision timepoint** = smallest k
  with AUROC(t_k) ≥ AUROC(T_end) − 0.02 (bootstrap CI excluding >0.02 drop); lead time =
  surgery week (≈26–34) − t_k (median, IQR).
- Calibration/utility: Brier, ECE (15 bins), calibration slope; DCA (de-escalation
  strategy vs treat-all vs clinical-only strategy).
- Inference: 5 seeds mean±std; patient-level bootstrap (1,000) 95% CI; paired AUC DeLong;
  AUPRC deltas via bootstrap; coverage via Clopper–Pearson; external sets Holm-corrected;
  ablations paired Wilcoxon (same seed/fold).
- External power (pre-registered): NACT-PILOT n = 64 (~16–19 pCR+) → CI half-width
  ±0.10–0.13; **CI-first reporting** (effect sizes + exact CIs, no significance claims).

## 8. Experiment matrix E0–E11

E0 pipeline gate (0.72 ± 0.02; outcome recorded — see amendments) → E1 frozen features →
E2 Tier-2 adjudication (administrative) → E3 main model v1 + leave-one-cohort-out
(>0.15 drop in any held-out cohort → domain-shift promoted to main text) → E4 ten
baselines → E5 three-arm (grid in §9) → E6 conformal (group with <5% de-escalable →
fall back to K = 2) → E7 seven ablations → E8 external validation (CI-first, Holm) →
E9 identifiability diagnostics → E10 ComBat sensitivity (only if E3 triggers) → E11
freeze + reproduction package (seed×3 variance < ±0.5 pp on development data).
Baseline list = the ten of the upstream protocol §6.2 (including clinical-only
LR/XGBoost), same features, same splits, 5 seeds.

## 9. Interpretation grid (pre-registered; adjudicated by the principal investigator)

Δ = AUC(both − clinical-only), with DCA net benefit:
- **Δ ≥ 0.03 and DCA positive** → main narrative holds;
- **0.02 ≤ Δ < 0.03** → stratified refinement (n_i > 3 vs = 3);
- **Δ < 0.02** → risk-4 narrative downgrade triggered (emphasis moves to conditioned
  trajectory heterogeneity + conformal layer + protocol contribution).

## 10. Multiplicity control

External sets: Holm. Conformal grid (g, k, m): Bonferroni/fixed-sequence (LTT). No
uncorrected cell-wise significance claims in the main table. Every "first/increment"
claim is bound to a specific test.

## 11. Theory boundary

Proposition 1 (conditional-effect identifiability) concerns **population-level
c-contrast effects** under A1–A3, not full vector-field recovery — wording aligned with
the honest boundary. Proposition 2 (Tier 2) is restricted to "arm-conditioned trajectory
description" (conditional association; no causal claims). Empirical diagnostic =
condition-contrast excitation index E (permutation test), reported per group (E9).

## 12. Registered tables and figures

Table 1 cohort roles + no-overlap statement; **Table 2 main table** (10 baselines + ours ×
AUROC/AUPRC/sens@95spec/NPV/Brier; overall + four groups; bold 0.72/0.85 anchors);
Table 3 ablations; Table 4 external (CI-first); Fig 1 architecture; **Fig 2 mechanism**
(per-group r_i(t) + threshold + arrival annotation); **Fig 3 conformal** (coverage +
λ̂_g/de-escalable fractions); Fig 4 per-timepoint AUC + lead weeks (RECIST reference);
Fig 5 DCA + calibration.

## 13. Writing red lines (checked per manuscript)

The manuscript does not cite references that could not be verified (internal check
ruling). "First" claims are restricted to three: first group-conditional conformal risk
control on breast neoadjuvant treatment decisions; formalization of conditioned latent
dynamics for longitudinal imaging pCR; first dynamics-modeling evaluation on the official
leakage-controlled benchmark. No "first longitudinal pCR prediction" (Comes/Yan) and no
"first medical CRC / longitudinal-imaging CRC" (Si 2026) claims. Tier-2 statements remain
"pre-registered upgrade path" until adjudicated.

## 14. Scooping surveillance (quarterly)

Europe PMC queries: `neural ODE treatment response imaging` | `conformal risk control
pCR` | `conformal neoadjuvant breast` | `BreastDCEDL` | `MAMA-MIA longitudinal`.
Preprint posted the week of submission, with the venue noted.

## 15. Amendments and deviations

Frozen 2026-10-11. All post-freeze deviations are recorded in `amendments/` (log +
individual amendment files) and mirrored in the internal working copy. None at freeze
time.
