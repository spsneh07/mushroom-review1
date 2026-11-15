# 00. Project Overview — Technical Audit

**Repository**: `https://github.com/spsneh07/mushroom-review1`  
**Audit Date**: November 15, 2026  
**Auditor Roles**: Machine Learning Researcher, Probability & Statistics Specialist, Academic Documentation Auditor  
**Target Submission Context**: SRM Institute of Science and Technology — Machine Learning Project Report (Review 1)  

---

## 1. Executive Summary & Purpose

This document provides the high-level technical audit and verification of the project repository **"mushroom-review1"**, focused on **Probability-Based Independence Analysis Using the UCI Mushroom Dataset**.

The primary objective of the audited implementation is to determine:
1. Whether two morphological and sensory attributes of mushrooms—**Foul Odor (Event A)** and **Brown Cap Color (Event B)**—occur independently across the general population of 8,124 mushrooms.
2. Whether these two attributes exhibit **conditional independence** when restricted to the subset of **Poisonous Mushrooms (Event C)**.

The audit team has inspected every file in the repository, analyzed the Jupyter Notebook (`mushrooms.ipynb`) cell by cell, independently recalculated all probabilistic metrics directly from the raw dataset (`agaricus-lepiota.data`), and verified all visualizations and textual claims.

---

## 2. Core Audit Findings & Verification Status

| Metric / Claim | Audited / Notebook Value | Raw Data Independent Verification | Discrepancy / Audit Note | Status |
|---|---|---|---|---|
| **Total Dataset Records** | 8,124 | 8,124 rows | Exact match | **VERIFIED** |
| **Total Columns** | 23 (1 class + 22 features) | 23 columns (1 target + 22 predictors) | Exact match | **VERIFIED** |
| **Edible Class Count ($e$)** | 4,208 (51.80%) | 4,208 instances (51.7971%) | Exact match | **VERIFIED** |
| **Poisonous Class Count ($p$)** | 3,916 (48.20%) | 3,916 instances (48.2029%) | Exact match | **VERIFIED** |
| **Missing Attribute Values** | `stalk-root` has 2,480 `'?'` | 2,480 `'?'` in column 11 (`stalk-root`) | Not dropped or imputed; not used in probability analysis | **VERIFIED** |
| **Event A: Foul Odor ($A$)** | 2,160 instances | 2,160 instances (`odor == 'f'`) | All 2,160 are poisonous ($C$) | **VERIFIED** |
| **Event B: Brown Cap ($B$)** | 2,284 instances | 2,284 instances (`cap-color == 'n'`) | 1,020 poisonous + 1,264 edible | **VERIFIED** |
| **Joint Event ($A \cap B$)** | 288 instances | 288 instances (`odor == 'f'` & `cap-color == 'n'`) | All 288 are poisonous ($C$) | **VERIFIED** |
| **$P(A)$** | `0.2658788774002954` | $2160 / 8124 \approx 0.2658788774002954$ | README rounds to `0.2659` | **VERIFIED** |
| **$P(B)$** | `0.2811422944362383` | $2284 / 8124 \approx 0.2811422944362383$ | README rounds to `0.2811` | **VERIFIED** |
| **$P(A \cap B)$** | `0.03545051698670606` | $288 / 8124 \approx 0.03545051698670606$ | README rounds to `0.0355` | **VERIFIED** |
| **$P(A) \times P(B)$** | `0.07474979763445036` | $(2160/8124) \times (2284/8124) \approx 0.07474979763445036$ | README rounds to `0.0747` | **VERIFIED** |
| **Phase 1 Decision** | $A$ and $B$ are **DEPENDENT** | $|P(A \cap B) - P(A)P(B)| \approx 0.0393 \neq 0$ | Clear mathematical dependency | **VERIFIED** |
| **$P(A \mid C)$** | `0.5515832482124617` | $2160 / 3916 \approx 0.5515832482124617$ | README rounds to `0.5516` | **VERIFIED** |
| **$P(B \mid C)$** | `0.2604698672114402` | $1020 / 3916 \approx 0.2604698672114402$ | README rounds to `0.2605` | **VERIFIED** |
| **$P(A \cap B \mid C)$** | `0.0735444330949949` | $288 / 3916 \approx 0.07354443309499489$ | README rounds to `0.0735` | **VERIFIED** |
| **$P(A \mid C) \times P(B \mid C)$** | `0.1436708154179548` | $(2160/3916) \times (1020/3916) \approx 0.14367081541795479$ | README rounds to `0.1437` | **VERIFIED** |
| **Phase 2 Decision** | **NOT CONDITIONALLY INDEPENDENT** | $|P(A \cap B \mid C) - P(A \mid C)P(B \mid C)| \approx 0.0701 \neq 0$ | Clear non-conditional independence | **VERIFIED** |

---

## 3. Scope of Implementation vs. Missing Components

### What IS Implemented:
1. **Raw Data Ingestion**: Pandas reading `agaricus-lepiota.data` without header row, manually supplying standard 23 column names.
2. **Boolean Event Indexing**: Boolean Series creation for events $A$, $B$, and condition $C$.
3. **Exact Frequency Counting**: Summation of boolean vectors for marginal and joint event counts.
4. **Marginal & Joint Probability Calculations**: Direct quotient calculation against total size $N = 8,124$.
5. **Conditional Probability Calculations**: Mean calculation over boolean masks filtered to $df[\text{"class"}] == \text{"p"}$.
6. **Tolerance-based Independence Checks**: Evaluation using threshold `abs(diff) < 1e-10`.
7. **Exploratory Visualizations**: 5 distinct Seaborn / Matplotlib plots (class count, odor count, odor vs. class, cap-color vs. class, probability event comparison barplot).

### What IS NOT Implemented (Auditor Observations):
1. **No Formal Hypothesis Tests**: No Chi-Square test of independence ($\chi^2$), Fisher's exact test, or p-values are computed.
2. **No Machine Learning Models**: No classification algorithms (e.g. Logistic Regression, Naive Bayes, Decision Trees) are trained.
3. **No Missing Data Imputation**: The 2,480 missing values in `stalk-root` (`?`) are neither imputed nor encoded, because `stalk-root` is omitted from downstream logic.
4. **No Feature Engineering**: All features remain in raw categorical string form.

---

## 4. Structure of Audit Evidence Dossier

The technical audit is organized into the following specialized evidence documents:

- **`00_PROJECT_OVERVIEW.md`**: Executive summary, core findings, verification matrix, and scope.
- **`01_REPOSITORY_STRUCTURE.md`**: Complete inventory of repository files, file sizes, git tracking, commit history, and integrity.
- **`02_DATASET_DESCRIPTION_AND_VERIFICATION.md`**: Formal specification of the UCI Agaricus-Lepiota dataset, attribute mapping, cardinality, and ground truth validation.
- **`03_PREPROCESSING_AND_DATA_QUALITY.md`**: Data loading mechanics, missing value analysis for `stalk-root`, data types, and filtering logic.
- **`04_PROBABILITY_MATHEMATICAL_FORMULATION.md`**: Formal academic probability definitions (sample space, marginals, joint, conditional, independence axioms).
- **`05_INDEPENDENCE_CALCULATIONS.md`**: Comprehensive verification of Phase 1 calculations, fractions, decimals, and threshold decisions.
- **`06_CONDITIONAL_INDEPENDENCE_CALCULATIONS.md`**: Comprehensive verification of Phase 2 calculations, conditioning on poisonous class $C$.
- **`07_EDA_AND_VISUALIZATION_AUDIT.md`**: Cell-by-cell visual audit of all 5 notebook plots, academic captions, extracted figures, and statistical interpretation.
- **`08_NOTEBOOK_AND_IMPLEMENTATION_AUDIT.md`**: Code execution audit, kernel specs, execution order analysis, cell output logs, and code quality review.
- **`09_REFERENCES.md`**: Verified academic and archival citations (Audubon guide, Schlimmer 1987, UCI ML repo, Python libraries).
- **`10_LIMITATIONS_AND_MISSING_INFORMATION.md`**: Explicit academic limitations, unperformed tests, rounding discrepancies, and recommended future work for Review 2 / Final Report.
- **`figures/`**: Directory containing all 5 extracted PNG visualization artifacts directly recovered from notebook execution outputs.
