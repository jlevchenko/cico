# cico

**Check-In/Check-Out (CICO) Data Analysis -- Multilevel Models of Direct Observation and BASC-3 Outcomes**

This repository contains the R notebook, rendered report, and summary slide
deck for the Check-In/Check-Out (CICO) outcome analysis. CICO is a Tier 2
school-based behavioral intervention; this analysis estimates its effect on
student engagement and behavior using systematic direct observation (SDO)
and teacher BASC-3 ratings, while accounting for clustering by classroom and
school.

---

## Research Questions

1. **RQ1:** Do CICO students show higher on-task behavior and lower disruptive behavior over time compared to business-as-usual (BAU)?
2. **RQ2:** Do CICO students have better BASC-3 outcomes at posttest after adjusting for baseline?

---

## Data Sources

| Source | Description |
|--------|-------------|
| CICO direct observations (SDO) | Repeated classroom observations of target students, 2021-22 through 2024-25 |
| BASC-3 teacher ratings | Pre- and post-intervention ratings on Externalizing, Internalizing, School Problems, and Adaptive Skills |
| Linkage IDs | Student, teacher, school, and site identifiers used to link SDO and BASC-3 records and to define clustering structure |

**Sample:** 1,153 phase-level SDO observations across 326 unique students in
24 schools; 249 students across 149 teachers with BASC-3 data.

---

## Design and Analytic Approach

- **Design:** Students (Level 1) nested within teachers/classrooms (Level 2) and schools (Level 3); treatment is CICO vs. BAU.
- **Proximal outcomes (RQ1):** Percent on-task and percent disruptive behavior across four phases (Baseline, Mid-Intervention, Post-Intervention, Maintenance), modeled with three-level growth models (time centered at week 12, the end of intervention), fixed effects for schools, and random intercepts/slopes for students.
- **Distal outcomes (RQ2):** BASC-3 Externalizing, Internalizing, School Problems, and Adaptive Skills, modeled with two-level ANCOVA (baseline covariate + classroom random intercept). Primary contrast is CICO minus BAU with pooled baseline SD for Cohen's d.
- **Missing data:** Handled via maximum likelihood (`na.exclude`), with baseline equivalence and differential attrition checked for both outcome families before modeling.
- **Reproducibility:** Random seed set to 2025; package versions and session info printed in the notebook; file paths defined at the top for easy re-run.

---

## Repository Structure

```
cico/
├── CICO_Data_Analysis.Rmd            # Full analysis notebook (source)
├── CICO_Data_Analysis_nb.html        # Rendered HTML notebook (knit output)
├── CICO_Data_Analysis.pptx           # Summary slide deck for team/stakeholder review
└── data/
    ├── CICO-2021-22_DirectObs_students.sav
    ├── CICO-2022-23_DirectObs_students.sav
    ├── CICO-2023-24_DirectObs_students.sav
    ├── CICO-2024-25_DirectObs_students.sav
    ├── BASC pre post only multivariate.sav
    └── BASC TRS combined across cohorts.sav
```

> **Note on source data:** The SPSS (`.sav`) source files are not included in
> this repository due to data use restrictions. Update `data_dir` in the
> Import Data chunk of `CICO_Data_Analysis.Rmd` to point to your local copy.

---

## Notebook: `CICO_Data_Analysis.Rmd`

Knits to HTML (with Word and PDF also configured). Sections follow the
analysis outline set out at the top of the notebook:

- **Setup** -- package loading, global options, seed
- **Import Data** -- reads and combines the four years of SDO `.sav` files and the two BASC-3 files
- **Variable Setup and Clustering** -- ID standardization/cleaning and diagnostics across student, teacher, and school identifiers
- **Reliability Filtering** -- drops reliability-observer rows, keeping primary observer data
- **Data Reshaping: Wide / Long** -- builds phase-level wide and long SDO datasets
- **Cluster Size Summaries** / **Missing Data Handling**
- **RQ1 (SDO):**
  - Baseline Equivalence and Attrition
  - Unconditional Models and ICCs
  - Model Output (three-level growth models)
  - Standardized Effects by Phase
  - Figure: model-based SDO estimates across phases by group
- **RQ2 (BASC-3):**
  - Pre-model checklist (baseline equivalence, attrition, ICCs) looped across all four BASC-3 scales
  - Model Output (two-level ANCOVA)

Each modeling section is followed by an `### Interpretation` subsection with
plain-language summaries of the results, written for sharing with the
project team.

---

## Key Findings

**RQ1 -- Direct Observation (SDO)**
- Groups were equivalent at baseline (on-task g = 0.12; disruptive g = 0.11).
- ICCs were high (on-task = .49; disruptive = .60), supporting the multilevel approach.
- Differential attrition was significant (CICO 6.8% vs. BAU 0.7%, p = .017) and was included as a covariate.
- At posttest (week 12), CICO students were more on-task (b = 0.13, p = .001) and less disruptive (b = -0.16, p < .001), with on-task gains growing faster over time for CICO (b = 0.005, p = .001). Effects persisted or strengthened through the maintenance phase.

**RQ2 -- BASC-3 Teacher Ratings**
- Baseline differences were small (g = -0.02 to -0.16); attrition was low (~6.6%) and balanced between groups (p = .94).
- ICCs (.23-.47 depending on scale; ~0 for School Problems) supported a two-level classroom model.
- CICO students showed lower (better) Externalizing, Internalizing, and School Problems scores at posttest. Adaptive Skills scores were also lower, which needs contextual interpretation since lower is not necessarily better on that scale.

See `CICO_Data_Analysis.pptx` for the stakeholder-facing summary of these
results, and `CICO_Data_Analysis_nb.html` for the full notebook with all
tables, diagnostics, and interpretation text.

---

## Usage

1. Clone the repository
2. Update `data_dir` in the Import Data chunk of `CICO_Data_Analysis.Rmd` to point to your local copy of the SDO and BASC-3 `.sav` files
3. Knit the Rmd to HTML (or open as a notebook in RStudio and run all chunks)

**R packages required:**
```r
install.packages(c("dplyr", "tibble", "tidyr", "purrr", "stringr",
                   "ggplot2", "readr", "knitr", "haven",
                   "lme4", "lmerTest", "performance", "emmeans",
                   "effectsize", "broom.mixed"))
```

---

## Related Repositories

- [`rep`](https://github.com/jlevchenko) -- Resilience Education Program (REP) 3.1/4.1 outcome analyses and power analysis
- [`metrics`](https://github.com/jlevchenko) -- METRICS national TA center data pipelines (APR/IPR reporting, tier assignment, ROI analysis)
