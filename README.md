# HDS-F26-Group6-malaga-cpet

HDS-F26 Group 6 — analysis of maximal cardiopulmonary exercise tests (CPET)
from the University of Málaga.

## Group members

- Andreas Sundstrøm Pedersen
- Jonas Visby Jensen
- Jonas Corluy

## Project status

Repository created in Lecture 1. Analysis scope not yet decided.

## Dataset

**Title:** Treadmill Maximal Exercise Tests from the Exercise Physiology and
Human Performance Lab of the University of Malaga

**PhysioNet URL:** https://physionet.org/content/treadmill-exercise-cardioresp/1.0.1/

**Database slug:** `treadmill-exercise-cardioresp`

**Version:** 1.0.1 (published 10 December 2021)

**Access type:** Open Access. Files are listed as accessible to anyone who
conforms to the license, but the project carries the *PhysioNet Contributor
Review Health Data License 1.5.0* and an accompanying Data Use Agreement,
which must be accepted from the project page before downloading.

**DOI:** https://doi.org/10.13026/7ezk-j442

### Citation

Mongin, D., García Romero, J., & Alvero Cruz, J. R. (2021). Treadmill Maximal
Exercise Tests from the Exercise Physiology and Human Performance Lab of the
University of Malaga (version 1.0.1). PhysioNet. RRID:SCR_007345.
https://doi.org/10.13026/7ezk-j442

Original publication:
Mongin, D., Chabert, C., Courvoisier, D. S., García-Romero, J., &
Alvero-Cruz, J. R. (2021). Heart rate recovery to assess fitness: Comparison
of different calculation methods in a large cross-sectional study.
*Research in Sports Medicine*. https://doi.org/10.1080/15438627.2021.1954513

PhysioNet platform citation:
Pollard, T., Moody, B. E., Lehman, L., Gow, B., Fernandes, C., Xie, C.,
Johnson, A., Mark, R. G., & Heldt, T. (2026). PhysioNet as a global platform
for biomedical research. *Nature Health*.
https://doi.org/10.1038/s44360-026-00096-z

### Contents

| File | Rows | Contents |
|---|---|---|
| `subject-info.csv` | 992 | ID, ID_test, age, sex, weight, height, lab temperature, humidity |
| `test_measure.csv` | 575,087 | Breath-by-breath: time (s), Speed (km/h), HR (bpm), VO2 (mL/min), VCO2 (mL/min), RR (breaths/min), VE (L/min), ID, ID_test |

992 maximal cardiopulmonary exercise tests from 857 participants, collected
2008–2018. Ages 10–63, 15% female. ~22 MB uncompressed.

### Records used

All 992 tests are in scope at present; no subset has been selected yet.
Any exclusion applied later will be recorded here, with its rationale, before
it is implemented in code.

Known data issues affecting selection:
- VO2, VCO2 and VE are missing for 30 tests.
- `RR` is respiration rate, not R-R interval.
- The recovery phase is present but unlabelled; it is inferred from `Speed`
  returning to 5 km/h.

### Downloading the data

Data files are **not** committed to this repository. Download them into a
local `data/` directory, which is git-ignored.

1. Create a PhysioNet account and accept the Data Use Agreement on the
   project page linked above.
2. Download by one of:

```bash
# Option A — whole project via wget
wget -r -N -c -np https://physionet.org/files/treadmill-exercise-cardioresp/1.0.1/

# Option B — AWS CLI
aws s3 sync --no-sign-request \
  s3://physionet-open/treadmill-exercise-cardioresp/1.0.1/ ./data/

# Option C — ZIP archive (6.3 MB) from the project page
```

3. Place `subject-info.csv` and `test_measure.csv` in `data/` so paths resolve
   as `data/subject-info.csv` and `data/test_measure.csv`.

Expected layout:
Repository created in Lecture 1.
