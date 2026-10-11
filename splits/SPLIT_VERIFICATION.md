# Split verification

The study uses the official patient-level split distributed with BreastDCEDL v3
(`BreastDCEDL_metadata_min_crop.csv`), **without re-splitting**:

| split | rows (`test` column) | n | pCR+ |
|---|---|---|---|
| train | 0 | 1,099 | 322 |
| test | 1 | 176 | 53 |
| validation | 2 | 177 | 53 |
| total labeled | — | 1,452 | 428 |

Column semantics (0 = train, 1 = test, 2 = validation, on the labeled subset) are pinned
by `amendments/AMENDMENT_2026-10-11_split-semantics-and-e0-gate.md`.

## Verification digests

The split is defined by the public metadata file; no identifier tables are distributed
here. Two digests over a canonical serialization of the split columns are provided.
Anyone can recompute them from the public source file.

| digest | definition | sha256 |
|---|---|---|
| all-split | all 2,070 rows, lines `pid,test`, sorted by pid, `\n`-joined, trailing newline, UTF-8 | `46b7228ffdb5b4d402ac5f819baefc5cdf4bcbf9d6b1ed35900fb629bcc050b2` |
| labeled-split | the 1,452 labeled rows, lines `pid,test,pCR`, same rules | `9d1b8e39dd7c27d6086d4a865a3aa11d07dfbd176c87a7409cc7fd1ca248279c` |

Source-file digest (full CSV, pandas 2.x environment):
`see checksums/INPUT_PROVENANCE.md`.

## Recomputation

```python
import hashlib, pandas as pd
df = pd.read_csv("BreastDCEDL_metadata_min_crop.csv")   # from Zenodo 10.5281/zenodo.18114231
la = sorted(f"{p},{int(t)}" for p, t in zip(df.pid, df.test))
print(hashlib.sha256(("\n".join(la) + "\n").encode()).hexdigest())
lab = df[df.pCR.notna()]
ll = sorted(f"{p},{int(t)},{int(c)}" for p, t, c in zip(lab.pid, lab.test, lab.pCR))
print(hashlib.sha256(("\n".join(ll) + "\n").encode()).hexdigest())
```

If your digests differ, stop: your split is not the official one, and no result in this
project is comparable.
