# ESS Information Encounters and Immigration Attitudes

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23120714.svg)](https://doi.org/10.5281/zenodo.23120714)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Reproducible Python workflow for European Social Survey analysis of information encounters, human values, and immigration attitudes among European managers

This repository contains the reproducible Python workflow and supporting materials associated with the article:

**Ahmadinia, H.**  
*Shaping Immigration Attitudes: The Role of Human Values, Media Engagement and Sociopolitical Events Among European Managers.*  
**Nordic Journal of Migration Research**, 16(1), Article 6, 1–25.  
https://doi.org/10.33134/njmr.964

The archived version of this reproducibility workflow is available on Zenodo:

**Version 1.0.1**  
https://doi.org/10.5281/zenodo.23120714

---

## Overview

The study uses data from four rounds of the **European Social Survey (ESS)** to examine how human values and information encounters through general internet use and political/current-affairs news consumption are associated with attitudes towards immigration.

Particular attention is given to **European managers**, with other European workers included as a comparison group.

The workflow reproduces the data preparation, variable construction, descriptive analyses, multilevel models, main article tables and figures, supplementary analyses, and validation checks associated with the published study.

---

## Study scope

The analysis combines four ESS rounds:

| ESS round | Study year |
|---|---:|
| ESS 8 | 2016 |
| ESS 9 | 2018 |
| ESS 10 | 2020 |
| ESS 11 | 2023 |

The final analytical sample contains:

- **117,752 respondents**
- **9,263 European managers**
- **108,489 other European workers**
- **18 European countries**
- **4 ESS rounds**
- **72 country-year combinations**

The 18 countries included are:

- Belgium
- Switzerland
- Germany
- Spain
- Finland
- France
- United Kingdom
- Hungary
- Ireland
- Iceland
- Italy
- Lithuania
- Netherlands
- Norway
- Poland
- Portugal
- Sweden
- Slovenia

---

## Main variables

The workflow includes measures relating to:

- attitudes towards immigration;
- self-transcendence values;
- conservation values;
- general internet use;
- political and current-affairs news consumption;
- managerial status;
- age;
- gender;
- educational attainment;
- income adequacy;
- religiosity; and
- political orientation.

The analysis uses multilevel regression models to examine individual-, occupational-, country-, and time-related variation in attitudes towards immigration.

---

## Repository contents

The principal reproducibility notebook is:

```text
Reproducible_Python_Workflow_MultiRound_ESS_Analysis.ipynb
```

The repository contains:

```text
ess-information-encounters-immigration-attitudes/
│
├── README.md
├── LICENSE
├── .gitignore
├── .zenodo.json
├── CITATION.cff
├── DATA_README.md
├── requirements.txt
├── REPRODUCIBILITY_AUDIT.md
├── Reproducible_Python_Workflow_MultiRound_ESS_Analysis.ipynb
│
└── outputs/
    ├── figures/
    ├── models/
    ├── tables/
    └── validation/
```

The notebook is the primary computational research object.

---

## Workflow

The notebook performs the analytical workflow from the raw ESS input, including:

1. loading ESS rounds 8–11;
2. harmonising variables across rounds;
3. handling ESS missing-value codes;
4. preparing the analytical sample;
5. classifying European managers and other workers;
6. constructing human-value measures;
7. constructing the immigration-attitudes measure;
8. transforming internet-use and news-consumption variables;
9. preparing sociodemographic variables;
10. descriptive analyses;
11. outlier analysis;
12. multilevel model estimation;
13. reproduction of article tables and figures;
14. reproduction of Supplementary Appendices 1–9;
15. model and output diagnostics; and
16. automated comparison with the published article and supplementary material.

---

## Data availability

The respondent-level European Social Survey data are **not included in this GitHub repository or in the associated Zenodo archive**.

Researchers should obtain the appropriate ESS data directly from the official European Social Survey Data Portal:

https://ess.sikt.no/en/

The workflow expects the following input file:

```text
ESS8e02_3-ESS9e03_2-ESS10-ESS10SC-ESS11-subset.csv
```

Place this file in the repository root beside the Jupyter notebook.

The raw file is read without modification.

No pre-cleaned input dataset is required.

For additional information about the required data and data-access conditions, see:

```text
DATA_README.md
```

The ESS respondent-level data remain subject to the applicable European Social Survey terms and licence conditions.

The MIT licence covering the code in this repository does **not** apply to the ESS data.

---

## Running the workflow

### 1. Clone the repository

```bash
git clone https://github.com/Hamed-Ahmadinia/ess-information-encounters-immigration-attitudes.git
cd ess-information-encounters-immigration-attitudes
```

### 2. Obtain the required ESS data

Download or prepare the appropriate ESS input and place:

```text
ESS8e02_3-ESS9e03_2-ESS10-ESS10SC-ESS11-subset.csv
```

in the repository root.

### 3. Create a Python environment

For example:

```bash
python -m venv .venv
```

Activate the environment according to your operating system.

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

### 5. Run the notebook

Open:

```text
Reproducible_Python_Workflow_MultiRound_ESS_Analysis.ipynb
```

Restart the kernel and execute all cells in order.

The notebook creates and/or updates the analytical outputs under:

```text
outputs/
```

---

## Computational environment

The archived workflow was executed using **Python 3.11.9**.

Core package versions include:

| Package | Version |
|---|---:|
| pandas | 2.2.3 |
| NumPy | 1.26.4 |
| SciPy | 1.13.1 |
| statsmodels | 0.14.2 |
| scikit-learn | 1.4.2 |
| Matplotlib | 3.8.4 |
| seaborn | 0.13.2 |
| nbformat | 5.9.2 |
| nbclient | 0.8.0 |
| ipykernel | 6.28.0 |
| patsy | 0.5.6 |
| threadpoolctl | 2.2.0 |

The complete pinned environment is available in:

```text
requirements.txt
```

---

## Statistical models

The workflow reproduces four principal multilevel models.

### Model 1

Full analytical sample with individual-level predictors and country-year random intercepts.

- **N = 117,752**
- **72 country-year groups**

### Model 2

Full analytical sample with managerial status added to Model 1.

- **N = 117,752**
- **72 country-year groups**

### Model 3

European-manager subsample with country-level random effects and random slopes for internet use and political-news consumption.

- **N = 9,263**
- **18 countries**

### Model 4

European-manager subsample with country fixed effects and ESS-round/year random intercepts.

- **N = 9,263**
- **4 ESS rounds**

Detailed model outputs are available under:

```text
outputs/models/
```

and:

```text
outputs/tables/
```

---

## Reproduced main article outputs

The workflow reproduces the principal analytical outputs reported in the article.

### Figure 1

**Internet Use and News Consumption Across ESS Rounds 8–11**

### Figure 2

**Media Use & Immigration Attitudes by Country**

### Table 2

**Multilevel Model Regression Results**

### Figure 3

**Trends Analysis in Immigration Attitudes by Values and Media Use Among European Managers and Employees**

### Figure 4

**Interaction Effects of Internet and News Consumption on Immigration Attitudes by Managerial Status in Europe**

### Figure 5

**Country-Level Analysis of Immigration Attitudes by Values and Media Use Among European Managers and Workers**

Generated figures are available in:

```text
outputs/figures/
```

---

## Supplementary analyses

The workflow also reconstructs the published supplementary material:

1. **Appendix 1** — Country-wise Distribution of Managers and Other Workers Across ESS Rounds
2. **Appendix 2** — Sociodemographic Profile of European Managers and Employees in ESS Rounds 8–11
3. **Appendix 3** — Outlier Analysis Report
4. **Appendix 4** — Outlier Visualisation for Key Variables
5. **Appendix 5** — Mean (SD) Attitudes Towards Immigration by Country and Year
6. **Appendix 6** — Model 1 Summary
7. **Appendix 7** — Model 2 Summary
8. **Appendix 8** — Model 3 Summary
9. **Appendix 9** — Model 4 Summary

Machine-readable outputs are stored in:

```text
outputs/tables/
```

---

## Reproducibility validation

The notebook contains a dedicated validation framework comparing computed results with the final published article and supplementary appendices.

Published numerical values are used only as **validation references**.

They are not used to generate:

- fitted models;
- regression coefficients;
- descriptive statistics;
- figures;
- tables;
- sample counts; or
- model predictions.

The audit distinguishes among:

- direct agreement;
- rounding-only differences;
- label or reference-category differences;
- publication typographical discrepancies; and
- quantities that cannot be independently recovered from the published graphical material.

The workflow therefore does **not** artificially hard-code published values to force agreement.

Detailed validation information is available in:

```text
REPRODUCIBILITY_AUDIT.md
```

and:

```text
outputs/validation/
```

In particular:

```text
outputs/validation/publication_validation.csv
```

contains the detailed comparison between computed and published results.

---

## Reproducibility statement

This repository provides an executable reproduction of the original computational workflow.

It should not be interpreted as claiming that every historical printed number, label, significance marker, or graphical coordinate in the published article is necessarily reproduced without discrepancy.

Where publication-level inconsistencies, rounding differences, reference-category issues, or typographical differences were identified, they are retained and documented transparently rather than silently changed.

This distinction between **computed results** and **published presentation** is intentional.

---

## Raw-data integrity

The workflow calculates a SHA-256 checksum of the input data before analysis and verifies the input again at the end of execution.

The exact input CSV used for the archived execution had the SHA-256 checksum:

```text
3ffcd57ddc096167f608e5ef53beaa9812ae7f18105001f36050484261715a01
```

A separately obtained ESS extract with a different checksum should not automatically be assumed to be byte-for-byte identical to the archived analytical input.

See `DATA_README.md` for additional information.

---

## Archived release

The stable reproducibility release is archived permanently on Zenodo.

**Ahmadinia, H. (2026).**  
*Reproducible Python workflow for multi-round European Social Survey analysis of information encounters, human values, and immigration attitudes among European managers*  
Version 1.0.1. Zenodo.

**DOI:** https://doi.org/10.5281/zenodo.23120714

Zenodo record:

https://zenodo.org/records/23120714

---

## Associated publication

**Ahmadinia, H.**  
*Shaping Immigration Attitudes: The Role of Human Values, Media Engagement and Sociopolitical Events Among European Managers.*  
**Nordic Journal of Migration Research**, 16(1), Article 6, 1–25.

**Article DOI:**  
https://doi.org/10.33134/njmr.964

The journal article and this reproducibility archive are separate scholarly objects and therefore have separate DOIs.

---

## Citation

If you use this workflow, please cite the Zenodo software record:

> Ahmadinia, H. (2026). *Reproducible Python workflow for multi-round European Social Survey analysis of information encounters, human values, and immigration attitudes among European managers* (Version 1.0.1). Zenodo. https://doi.org/10.5281/zenodo.23120714

Please also cite the associated research article:

> Ahmadinia, H. *Shaping Immigration Attitudes: The Role of Human Values, Media Engagement and Sociopolitical Events Among European Managers.* Nordic Journal of Migration Research, 16(1), Article 6, 1–25. https://doi.org/10.33134/njmr.964

Machine-readable citation metadata are available in:

```text
CITATION.cff
```

---

## Licence

The source code and original computational workflow in this repository are released under the **MIT License**.

See:

```text
LICENSE
```

The MIT License applies to the original code and computational workflow in this repository.

It does **not** apply to:

- European Social Survey respondent-level data; or
- the associated published journal article.

Those materials remain governed by their respective licences and terms of use.

---

## Author

**Hamed Ahmadinia, PhD**  
Senior Researcher  
Economic Sociology  
Department of Social Research  
University of Turku  
Finland

**ORCID:**  
https://orcid.org/0000-0002-3505-8101

**Website:**  
https://www.ahmadinia.fi/

**GitHub:**  
https://github.com/Hamed-Ahmadinia

---

## Funding and acknowledgements

The associated study was conducted in connection with the **Mobile Futures** research project and was supported by the **Strategic Research Council established within the Research Council of Finland**.

For the complete funding statement, acknowledgements, methodological details, interpretation of results, and limitations, please refer to the published article.

---

## Version history

### v1.0.1

Current archived release.

- GitHub–Zenodo archival release
- corrected Zenodo metadata
- complete executable notebook
- reproducibility audit
- generated figures and tables
- model outputs
- publication-validation results
- data documentation

Zenodo DOI:

https://doi.org/10.5281/zenodo.23120714

### v1.0.0

Initial GitHub release prior to activation of the GitHub–Zenodo archival integration.

---

## Repository and archive

**GitHub repository:**  
https://github.com/Hamed-Ahmadinia/ess-information-encounters-immigration-attitudes

**Zenodo archive:**  
https://doi.org/10.5281/zenodo.23120714
