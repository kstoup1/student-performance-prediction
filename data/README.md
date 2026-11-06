# Data

This project used the publicly available **UCI Student Performance** dataset, mathematics subset: 395 student records from two Portuguese secondary schools.

- **Source:** https://archive.ics.uci.edu/dataset/320/student+performance
- **Original file:** `student-mat.csv` (semicolon-separated, 33 attributes: 30 non-grade attributes plus grades G1, G2, G3)
- **Citation:** P. Cortez and A. Silva, *Using Data Mining to Predict Secondary School Student Performance*, FUBUTEC 2008, pp. 5–12.

Please refer to the UCI page for the dataset's license and terms of use.

## Prepared files used in WEKA

| File | Contents | WEKA attributes |
|---|---|---|
| `Student_Without_Grades.csv` | 30 non-grade attributes + `PassFail` (G1, G2, G3 removed) | 31 |
| `Student_With_G1.csv` | 30 non-grade attributes + G1 + `PassFail` (G2, G3 removed) | 32 |

**Label:** `PassFail` = `Pass` if G3 ≥ 10, otherwise `Fail` (265 Pass, 130 Fail). G3 was removed after the label was created. All 395 labels were checked for consistency during the project.

> **Note:** The team's prepared CSV files will be added to this folder from the original project files. They have not been recreated or approximated. Until then, they can be rebuilt from the UCI source using the steps in the main [README](../README.md#how-to-reproduce).
