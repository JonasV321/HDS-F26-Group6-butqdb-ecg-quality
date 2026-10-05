# butqdb-ecg-quality

HDS-F26 Group 6 — analysis of ECG signal quality in long-term wearable recordings from the Brno University of Technology ECG Quality Database (BUT QDB).

## Group members

- Andreas Sundstrøm Pedersen
- Jonas Visby Jensen
- Jonas Corluy

## Project status

Repository created in Lecture 1. Dataset changed from the Málaga treadmill CPET dataset to BUT QDB. Analysis scope not yet decided.

## Dataset

**Title:** Brno University of Technology ECG Quality Database (BUT QDB)

**PhysioNet URL:** https://physionet.org/content/butqdb/1.0.0/

**Database slug:** `butqdb`

**Version:** 1.0.0 (published 22 July 2020)

**Access type:** Open Access. Anyone can access the files, provided they conform to the license. License: Creative Commons Attribution 4.0 International (CC BY 4.0). No PhysioNet account or Data Use Agreement is required.

**DOI:** https://doi.org/10.13026/kah4-0w24

## Citation

Nemcova, A., Smisek, R., Opravilová, K., Vitek, M., Smital, L., & Maršánová, L. (2020). Brno University of Technology ECG Quality Database (BUT QDB) (version 1.0.0). PhysioNet. RRID:SCR_007345. https://doi.org/10.13026/kah4-0w24

**Related publication:** Smital, L., Felton, C. L., Gilbert, B., Holmes, D., Haider, C. R., Vitek, M., … Provaznik, I. (2020). Real-time quality assessment of long-term ECG signals recorded by wearables in free-living conditions. IEEE Transactions on Biomedical Engineering. https://doi.org/10.1109/tbme.2020.2969719

**PhysioNet platform citation:** Pollard, T., Moody, B. E., Lehman, L., Gow, B., Fernandes, C., Xie, C., Johnson, A., Mark, R. G., & Heldt, T. (2026). PhysioNet as a global platform for biomedical research. Nature Health. https://doi.org/10.1038/s44360-026-00096-z

## Contents

18 long-term (≥ 24 h) recordings from 15 subjects (9 female, 6 male), aged 21–83, collected August 2018 – October 2019 under free-living conditions with a Bittium Faros 180. 13 subjects were recorded once, one twice, one three times. ~4.2 GB uncompressed (2.2 GB ZIP).

| File | Contents |
|---|---|
| `<record>/<record>_ECG.dat` / `.hea` | Single-lead ECG, 1000 Hz, WFDB format |
| `<record>/<record>_ACC.dat` / `.hea` | 3-axis accelerometer, 100 Hz, WFDB format |
| `<record>/<record>_ANN.csv` | Signal-quality annotations, 12 columns (see below) |
| `subject-info.csv` | Demographics: sex, age, height, weight, smoking status |
| `RECORDS`, `ANNOTATORS`, `ann_reader.m` | Record list, annotator list, MATLAB annotation reader |

Record names are six digits: first three = subject ID, last three = measurement number (e.g. `103002` = subject 103, second recording).

**Annotation format:** 3 columns per annotator × 3 annotators + 3 consensus columns. Per group: start sample, end sample, quality class.

| Class | Meaning |
|---|---|
| 1 | P, QRS and T clearly visible; onsets/offsets reliably detectable |
| 2 | Increased noise; fine waveform points unreliable, but QRS reliably detectable |
| 3 | QRS not reliably detectable; unsuitable for analysis |
| 0 | Not annotated |

## Records used

All 18 records are in scope at present; no subset has been selected yet. Any exclusion applied later will be recorded here, with its rationale, before it is implemented in code.

Known data issues affecting selection:

- Only 3 records are fully annotated. The other 15 have two annotated 20-minute segments each; five extra segments (four of 20 min, one of 2 min) were added to increase the share of noisy data. Everything else is class 0.
- Annotation sample indices refer to the 1000 Hz ECG. Divide by 10 to align with the 100 Hz accelerometer.
- Water activities (showering, swimming) were avoided, so those conditions are not represented.
- Small sample: 15 subjects, with repeated recordings from two of them. Splits must be done by subject, not by record, to avoid leakage.

## Downloading the data

Data files are not committed to this repository. They are downloaded into a local `data/` directory, which is git-ignored. Full download is ~4.2 GB.

Install the client:

```
pip install wfdb pandas
```

Download one record (example):

```python
import wfdb

rec = "100001"
wfdb.dl_files(
    db="butqdb/1.0.0",
    dl_dir="data",
    files=[
        "subject-info.csv",
        f"{rec}/{rec}_ECG.hea", f"{rec}/{rec}_ECG.dat",
        f"{rec}/{rec}_ACC.hea", f"{rec}/{rec}_ACC.dat",
        f"{rec}/{rec}_ANN.csv",
    ],
)
```

Full database:

```
wget -r -N -c -np https://physionet.org/files/butqdb/1.0.0/

aws s3 sync --no-sign-request s3://physionet-open/butqdb/1.0.0/ ./data/
```

Or the 2.2 GB ZIP archive from the project page.

## Loading

```python
import pandas as pd
import wfdb

rec = "100001"
ecg = wfdb.rdrecord(f"data/{rec}/{rec}_ECG")   # ecg.p_signal, ecg.fs == 1000
acc = wfdb.rdrecord(f"data/{rec}/{rec}_ACC")   # acc.p_signal (3 axes), acc.fs == 100
ann = pd.read_csv(f"data/{rec}/{rec}_ANN.csv", header=None)
subjects = pd.read_csv("data/subject-info.csv")
```

Consensus labels are in the last three annotation columns (`ann.iloc[:, 9:12]`).

## Expected layout

```
.
├── README.md
├── .gitignore        # contains data/
└── data/
    ├── subject-info.csv
    ├── RECORDS
    ├── ANNOTATORS
    ├── 100001/
    │   ├── 100001_ECG.dat
    │   ├── 100001_ECG.hea
    │   ├── 100001_ACC.dat
    │   ├── 100001_ACC.hea
    │   └── 100001_ANN.csv
    └── ...
```
