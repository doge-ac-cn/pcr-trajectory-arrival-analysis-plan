# Reproduction

The frozen specification maps to a private development repository that will be released
in full at manuscript submission. Mapping:

| specification element | implementation |
|---|---|
| §2 official split | `splits/SPLIT_VERIFICATION.md` (digests over the public metadata file) |
| §5 locked hyperparameters | training scripts for the main model (condition-injected ODE-RNN) and the re-implemented official ViT baseline |
| §6 conformal layer | `conformal/` (RCPS grid, three budget allocations, K = 4/2) |
| §8 experiment matrix E0–E11 | runner scripts with per-experiment logs |
| §9 interpretation grid | computed by the three-arm runner; adjudicated by the principal investigator |

Until release, this archive is self-contained: the specification, amendments, and split
verification can be checked without the private repository.

Attribution: see `README.md` and `LICENSE`.
