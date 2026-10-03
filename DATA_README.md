# Data README

## Data source

This repository contains the computational workflow associated with:

**Ahmadinia, H.**  
*Shaping Immigration Attitudes: The Role of Human Values, Media Engagement and Sociopolitical Events Among European Managers.*  
**Nordic Journal of Migration Research**, 16(1), Article 6, 1–25.  
DOI: https://doi.org/10.33134/njmr.964

The analysis uses data from the **European Social Survey (ESS), rounds 8–11**, covering the study years:

| ESS round | Study year |
|---|---:|
| ESS 8 | 2016 |
| ESS 9 | 2018 |
| ESS 10 | 2020 |
| ESS 11 | 2023 |

The analysis covers 18 European countries:

Belgium, Switzerland, Germany, Spain, Finland, France, the United Kingdom,
Hungary, Ireland, Iceland, Italy, Lithuania, the Netherlands, Norway, Poland,
Portugal, Sweden, and Slovenia.

## Why the raw data are not included

The respondent-level ESS data are **not redistributed in this GitHub repository or in the associated Zenodo software archive**.

Researchers should obtain the appropriate data directly from the official European Social Survey data portal:

https://ess.sikt.no/en/

The ESS data remain subject to the applicable ESS terms of use and data licence.
The MIT licence used for the code in this repository does **not** apply to the ESS respondent-level data.

## Required input file

The reproducibility notebook expects the following file in the repository root, beside the notebook:

```text
ESS8e02_3-ESS9e03_2-ESS10-ESS10SC-ESS11-subset.csv
```

The principal notebook is:

```text
Reproducible_Python_Workflow_MultiRound_ESS_Analysis.ipynb
```

The notebook reads the raw CSV without modifying it and performs the cleaning,
harmonisation, variable construction, statistical modelling, figure generation,
table generation, and publication-validation checks programmatically.

## Analytical sample produced by the workflow

After applying the study's processing and inclusion rules, the workflow produces:

- **117,752 respondents**
- **9,263 European managers**
- **108,489 other European workers**
- **18 countries**
- **4 ESS rounds**
- **72 country-year combinations**

These values refer to the final analytical sample produced by the workflow, not
necessarily to the dimensions of a newly downloaded raw ESS file.

## Integrity check for the archived run

The exact input CSV used for the archived reproducibility run had the following SHA-256 checksum:

```text
3ffcd57ddc096167f608e5ef53beaa9812ae7f18105001f36050484261715a01
```

The notebook calculates the checksum before analysis and verifies it again at the
end of execution.

If your independently obtained or reconstructed input has a different checksum,
the notebook may still run, but it should not be assumed to be byte-for-byte
identical to the input used for the archived run.

## Running the workflow

1. Obtain the required ESS data from the official ESS portal.
2. Prepare the input file expected by the workflow and name it exactly:

   `ESS8e02_3-ESS9e03_2-ESS10-ESS10SC-ESS11-subset.csv`

3. Place the CSV in the repository root.
4. Install the pinned Python dependencies:

   ```bash
   pip install -r requirements.txt
   ```

5. Open:

   `Reproducible_Python_Workflow_MultiRound_ESS_Analysis.ipynb`

6. Restart the kernel and run all cells in order.

The notebook generates aggregate outputs under `outputs/`, including figures,
tables, model diagnostics, and publication-validation files.

## Reproducibility notes

The workflow distinguishes between:

- computed results;
- values printed in the published article and appendices;
- rounding-only differences;
- label/reference-category differences;
- publication typographical discrepancies; and
- values that cannot be independently recovered from the published graphical material.

See:

- `REPRODUCIBILITY_AUDIT.md`
- `outputs/validation/publication_validation.csv`

for the detailed audit.

## Code licence

The source code and original computational workflow in this repository are
released under the **MIT License**. See `LICENSE`.

The MIT License does not apply to the ESS respondent-level data or to the
published journal article.
