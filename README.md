# State/UT-wise Cybercrime in India Evidence Base

This repository contains the project's evidence base for official state/UT-wise cybercrime data in India. The data spans a 10-year period (2014 to 2023) across 36 States and Union Territories (UTs). 

The primary metric measured is **cyber crime cases registered by police (FIRs)** according to the National Crime Records Bureau (NCRB) definitions.

## 🗂️ Package Structure

The project is deliberately organized to preserve the original values and state names exactly as they were printed in the source documents.

* **`data/raw/`**: Contains 11 per-table CSV files and one combined CSV (`cybercrime_raw_all_sources.csv`) which stacks all 1,794 rows of data. The primary 10-year series is located in `S01_LS_USQ438_2025-12-02_AnnexII_2014-2023.csv`.
* **`docs/`**: Includes the source inventory (`source_inventory.csv`), extraction log, download register, and a state/UT naming reference (`state_ut_naming_reference.csv`).
* **`scripts/`**: Includes `build_raw.py`, a script that parses extraction transcripts and rebuilds every raw CSV file without altering the source values.

## 📊 Data Sources 

The main figures are reproduced from **NCRB Crime in India** data via Lok Sabha Unstarred Question 438, answered on December 2, 2025. 

A total of 18 official Government of India sources—specifically Ministry of Home Affairs parliamentary replies and PIB documents—were recorded in the inventory to compile and verify this dataset.

## ⚠️ Data Comparability and Validation

Automated checks were performed on the extracted data. **Note:** The figures have not yet been manually compared by eye with the printed PDF pages. 
* **Validation Check**: 42 out of 46 table-years successfully reconcile with the printed totals. In a cross-source check, 318 State/UT-year values appeared in two or more documents with zero disagreements. 
* **Comparable Years (2018-2023)**: This is the recommended core window for main analysis, state comparisons, and panel models, as every value is confirmed by at least two documents.
* **Partially Comparable (2014-2016)**: The NCRB collected fewer crime heads before the 2017 edition. This data should only be used for long-run descriptive analysis and must be clearly labeled. 
* **Use with Caution (2017)**: Only one source provides state values for 2017, and it has not yet been verified against the official Crime in India 2019 report. 

## 🛠️ Known Issues / TODOs

- [ ] **Missing Source Files**: The original PDFs were blocked from automated access and need to be downloaded manually from the register so the team can verify transcripts against the printed tables.
- [ ] **Ladakh Data**: Data for Ladakh is completely blank in the `S01` source for 2014-2023. This needs to be addressed and filled in a cleaned dataset copy (do not edit the raw files).
- [ ] **Population Data**: Population data must be collected manually from NCRB's Crime in India population columns to accurately compute crime rates.

## 🚀 Getting Started

To load the primary series and start exploring the data in Python:

```python
import pandas as pd

# Load the core 10-year dataset
df = pd.read_csv("data/raw/S01_LS_USQ438_2025-12-02_AnnexII_2014-2023.csv")

# Filter for the recommended analysis window (2018-2023) and isolate state rows
panel = df[(df.row_type == "state_ut") & (df.year.between(2018, 2023))]

# Pivot to view trends by state over time
wide = panel.pivot(index="state_ut_original", columns="year", values="reported_cases_raw")

print(wide.head())
