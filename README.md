# HDS-F26-Group6-malaga-cpet

# Project title:
malaga-cpet

## Link to dataset from Physionet:
https://physionet.org/content/treadmill-exercise-cardioresp/1.0.1/

## Group members
- Andreas Sundstrøm Pedersen
- Jonas Visby Jensen
- Jonas Corluy

## Project topic
Malaga treadmill maximal exercise tests (2021)

## Dataset
PhysioNet — *Treadmill Maximal Exercise Tests from the Exercise Physiology
and Human Performance Lab of the University of Malaga*, v1.0.1 (2021).
DOI: 10.13026/7ezk-j442

992 maximal cardiopulmonary exercise tests (CPET) from 857 participants,
collected 2008–2018. Ages 10–63, 15% female.

| File | Rows | Contents |
|---|---|---|
| `subject-info.csv` | 992 | ID, ID_test, age, sex, weight, height, lab temperature, humidity |
| `test_measure.csv` | 575,087 | Breath-by-breath: time, Speed (km/h), HR (bpm), VO2 (mL/min), VCO2 (mL/min), RR (breaths/min), VE (L/min) |

~22 MB uncompressed. Access requires a PhysioNet account and acceptance of
the Contributor Review Health Data Use Agreement. Data files are **not**
committed to this repo.

Notes: VO2/VCO2/VE missing for 30 tests. `RR` = respiration rate, not R-R
interval. Recovery phase is included but unlabelled — inferred from Speed
returning to 5 km/h.

## Project status
Repository created in Lecture 1.
