# GLP-1RA Cardiovascular Risk Reduction Heterogeneity in Type 2 Diabetes

Code repository for the study:  
**"Consistency of Cardiovascular Risk Reduction Associated with Glucagon-Like Peptide-1 Receptor Agonists Among Patients with Type 2 Diabetes"**

---

## Repository Structure

```
├── 01_initial_cohort_generation.ipynb     # Study cohort construction and eligibility criteria
├── 02_outcome_identification.ipynb        # Outcome definitions and endpoint identification
├── 03_mice_imputation.ipynb               # Multiple imputation for baseline covariates
├── 04_analysis_pipeline.ipynb             # Main analysis pipeline including OW-pLASSO 
└── README.md
```

---

## Study Overview

This study evaluates whether cardiovascular risk reduction associated with glucagon-like peptide-1 receptor agonists (GLP-1RA) is consistent across clinically relevant patient subgroups among adults with type 2 diabetes in routine clinical practice in the United States.

The study was conducted using a target trial emulation framework with a new-user active-comparator design using electronic health record (EHR) data from the Truveta platform.

Patients were grouped according to initial treatment exposure::

- **GLP-1 receptor agonists (GLP-1RA)**
- **Comparator therapies**: SGLT2 inhibitors, DPP-4 inhibitors

Patients were further stratified into:

- **Primary prevention cohort**: no prior cardiovascular disease at baseline
- **Secondary prevention cohort**: prior cardiovascular disease at baseline

---

## Methods

| Component | Detail |
|---|---|
| Design | Target trial emulation, active comparator new-user cohort study |
| Data Source | Truveta nationwide EHR platform |
| Missing data | Multiple imputation by chained equations (MICE; m=5) |
| Confounder adjustment | Propensity score overlap weighting |
| Heterogeneous treatment effect estimation | Overlap-weighted penalized LASSO (OW-pLASSO) |
| Primary estimand | Average Treatment Effect in the Overlap population (ATO) |
| Time-to-event analysis | Weighted Cox proportional hazards models |
| Variance | Robust sandwich SE; pooled via Rubin's rules |
| Software | R 4.4.1 |

---

## Outcomes

All outcomes were assessed over a fixed 3-year follow-up period beginning at the index date.

**Primary** 
- The primary endpoint was time to first major adverse cardiovascular event (MACE), defined as the composite of myocardial infarction (MI) or stroke. 

**Secondary** 
- All-cause mortality
- Expanded MACE, defined as a composite of myocardial infarction, stroke, or heart failure hospitalization 
- Chronic kidney disease (CKD stage 3-5)
- Metabolic dysfunction-associated steatotic liver disease (MASLD)
- Change in systolic blood pressure at 6 months
- Change in hemoglobin A1c (HbA1c) at 6 months

---

## Reproducibility

All analyses are reproducible conditional on access to the study data.

---

## Key Packages

| Package | Version | Purpose |
|---|---|---|
| `glmnet` | 4.1.10 | LASSO and penalized logistic regression for variable selection and propensity score modeling |
| `mice` | 3.18.0 | Multiple imputation |
| `survival` | 3.8.3 | Cox proportional hazards models and Kaplan-Meier estimation |
| `data.table` | 1.17.0 | Efficient data manipulation and large-scale tabular processsing |
| `Matrix` | 1.7.3 | Sparse matrix operations for high-dimensional modeling workflows |

Additional visualization and reporting packages (e.g., ggplot2) were used as needed for figure generation.

---

## Data Availability

The data used in this study are from the Truveta platform. Truveta is a health data 
company that aggregates longitudinal EHR data from a network of US health systems, 
encompassing clinical notes, diagnoses, medications, laboratory results, and procedures. 
Data are not publicly available due to data sharing agreements. Qualified researchers 
may apply for access at https://www.truveta.com.


## Notes

- Data Privacy & Environment: Raw patient-level data are not included in this repository in accordance with data sharing agreements. Access to the study data requires an authorized Truveta research environment.
- Execution order: Notebooks are intended to be run sequentially: `01_initial_study_cohort_generation.ipynb` → `02_outcome_identification.ipynb` → `03_mice_imputation.ipynb` → `04_analysis_pipeline.ipynb`.
- Core Analytical Workflow (`04_analysis_pipeline.ipynb`): This notebook implements the main causal inference framework optimized for Causal Subgroup Analysis (SGA). To balance the bias-variance tradeoff inherent in high-dimensional observational data, we implemented the Overlap-Weighted Penalized LASSO (OW-pLASSO) algorithm (Yang et al., 2021). This notebook specifically contains the analytical workflow for the Type 2 Diabetes Secondary Prevention Cohort as the representative baseline. All other primary and secondary outcome analyses follow this identical framework with cohort- and outcome-specific modifications.
