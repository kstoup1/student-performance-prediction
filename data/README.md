# Data files

This project used the publicly available UCI Student Performance **mathematics** dataset (395 Portuguese secondary-school student records).

The two cleaned datasets used in WEKA are:

- `Student_Without_Grades.csv`: 30 non-grade characteristics and PassFail (31 WEKA attributes). Excludes G1/G2/G3.
- `Student_With_G1.csv`: same 30 characteristics plus G1 and PassFail (32 WEKA attributes). Excludes G2/G3.

**These exact CSV files are not currently uploaded to this repository.** We did not recreate them without the original prepared files. To add them later, upload the originals directly to this folder after reviewing the dataset terms and any school-specific publication requirements.

Source: https://archive.ics.uci.edu/dataset/320/student+performance

PassFail definition: Pass for G3 ≥ 10; Fail otherwise. All 395 records and labels were checked for consistency during the project.
