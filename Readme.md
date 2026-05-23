# Employee Performance Data Report

**Prepared by:** Data Analytics Team
**Date:** May 2026
**Dataset:** Employee Performance Records — 200 Employees

---

## A. Executive Summary

This report presents the findings from a structured data quality assessment and cleaning exercise performed on an employee performance dataset comprising 200 employee records across 19 fields. The raw dataset suffered from pervasive quality issues — including inconsistent formatting, outlier values, missing data, duplicate records, and multi-format dates — rendering it unsuitable for reliable analysis in its original state.

Following a systematic cleaning process guided by a documented issue log, the dataset has been standardised and enriched with derived date fields (Day, Month, Year). Key workforce metrics are now reliable: the average employee age is 42 years, the mean monthly salary is approximately ₦391,621, and the average performance score is 5.04 out of 10. The active workforce represents 54.5% of all records.

The cleaned dataset enables accurate HR reporting, performance benchmarking, and workforce planning — outcomes not possible from the original raw data.

---

## B. Business Problem

The organisation's HR and People Analytics function relies on employee performance data to drive strategic decisions around compensation, promotions, training, and workforce planning. However, the source dataset presented the following critical data quality failures:

| # | Problem Area | Description |
|---|---|---|
| 1 | Inconsistent Formatting | Names, departments, cities, gender, and education levels appeared in mixed case (e.g., `ACTIVE`, `active`, `Active`), rendering grouping and filtering unreliable |
| 2 | Invalid & Outlier Values | Age fields contained impossible values such as `-5`, `150`, and `999`; performance scores included `100` and `-1` outside the valid 1–10 scale |
| 3 | Missing Data | Multiple core fields — including Age, Gender, Employment Status, Salary, and Insurance — contained blank or null entries with no standardised placeholder |
| 4 | Mixed Data Types in Salary | The `Monthly_Salary` column mixed plain numeric values with currency-formatted strings (e.g., `₦217,945`) and negative figures |
| 5 | Multi-Format Dates | Hire dates appeared in at least five different formats: `YYYY-MM-DD`, `DD/MM/YYYY`, `MM-DD-YYYY`, `DD-Mon-YYYY`, and Excel serial numbers |
| 6 | Duplicate Records | Approximately 8 fully duplicated rows were identified and required removal |
| 7 | Incomplete Identifiers | Some records had only a first name (e.g., `Susan`, `Karen`) and some emails lacked the `@` symbol or used a personal prefix without a domain |

These issues collectively undermined the integrity of any downstream reporting, making it impossible to produce accurate headcount summaries, salary analyses, or performance rankings without first addressing data quality.

---

## C. Objectives

The data cleaning exercise was undertaken to achieve the following specific objectives:

1. **Standardise all categorical fields** — Full names, gender, city, department, education level, and employment status normalised to consistent Proper Case formatting.
2. **Handle invalid and outlier values** — Age values that are negative, zero, or above 60 (the defined retirement threshold) replaced with `N/A`. Performance scores outside the valid range of 1–10 flagged and treated accordingly.
3. **Resolve missing data** — Blank entries across key fields (Age, Gender, City, Department, Employment Status, Has Insurance, Bonus Percentage) replaced with the standardised placeholder `N/A` where imputation is not appropriate.
4. **Normalise salary data** — Strip currency symbols (`₦`), remove formatting characters (commas), and convert all salary values to a consistent numeric format. Zero-value salaries flagged for review.
5. **Standardise date formats** — All hire dates converted to a uniform Excel serial date, with separate derived columns for Day, Month, and Year to facilitate time-based analysis.
6. **Remove duplicate records** — Identify and eliminate fully duplicated rows to ensure each employee has a single unique record.
7. **Assign unique Employee IDs** — Each record assigned a structured identifier (`EMP001` through `EMP200`) to serve as a primary key for future data joins and reporting.

---

## D. Tools

The following tools and technologies were used to profile, clean, and document the dataset:

| Tool | Purpose |
|---|---|
| Microsoft Excel | Primary platform for data storage, manual inspection, formula-based cleaning, and conditional formatting |
| Excel Power Query | Used for bulk transformations: date parsing, text case normalisation, column type enforcement, and null handling |
| Python (Pandas) | Applied for outlier detection, statistical profiling (`describe()`), duplicate identification, and batch value replacements |
| OpenPyXL | Used to write and format the cleaned output workbook with structured column headers and consistent data types |
| Data Quality Guide Sheet | An embedded reference table (`Guide` worksheet) documenting every identified issue, the affected column, the transformation rule applied, and its description — serving as the audit trail for all changes made |

---

## E. Recommendations

Based on the findings from the data profiling and cleaning exercise, the following recommendations are made:

### 1. Implement Data Validation Rules at Source

The root cause of most quality issues is the absence of input validation in the HR data entry system. The following controls should be enforced at the point of entry:

- **Age:** Accept integers between 18 and 60 only; reject blanks and out-of-range values
- **Gender:** Enforce a dropdown: `Male`, `Female`, `Unknown` — no free-text entry
- **Employment Status:** Restrict to controlled vocabulary: `Active`, `Inactive`, `On Leave`, `Terminated`
- **Performance Score:** Enforce numeric range validation: 1.0 – 10.0
- **Salary:** Enforce numeric-only input with no currency symbols or formatting characters
- **Hire Date:** Enforce a single system date format (`YYYY-MM-DD`)

### 2. Standardise Employee Identification

- Introduce a mandatory unique Employee ID (as applied in the cleaned dataset: `EMP001` – `EMP200`) at the point of onboarding
- Require full name (first + last) and validate against the existing employee registry to prevent duplicates

### 3. Address Residual Data Gaps

The following fields still carry `N/A` placeholders in the cleaned dataset that warrant follow-up with HR:

| Field | Records with N/A | Action Required |
|---|---|---|
| Age | 19 | Confirm dates of birth from HR files |
| Gender | 21 | Update via employee self-service portal |
| Employment Status | 21 | Verify current status with line managers |
| Has Insurance | 29 | Cross-reference with payroll/benefits system |
| Years Experience | 6 | Obtain from original job applications or CVs |
| City | 4 | Confirm location from employee profiles |
| Department | 4 | Clarify with department heads |

### 4. Investigate Zero-Salary Records

Several employees show a `Monthly_Salary` of `0`. These records likely reflect system defaults or data migration errors. A cross-check with the payroll system is recommended to determine whether these are:

- Employees awaiting salary assignment (new joiners)
- Incorrectly migrated records
- Terminated employees whose salary was zeroed out

### 5. Review Anomalous Hire Dates

Seven records show hire dates resolving to January 1900, which is a known Excel default for null or unparseable date values. The original hire dates for these employees should be retrieved from onboarding documents and corrected.

### 6. Establish a Recurring Data Quality Review Process

A quarterly data quality audit should be introduced, using the same profiling methodology applied in this exercise, to detect and resolve issues before they compound. Key metrics to monitor:

- % completeness per field
- % outlier values (age, score, salary)
- Duplicate record rate
- Date format compliance rate

---

## F. Conclusion

This exercise demonstrates the critical importance of data governance and quality control in HR data management. The raw dataset, while containing 200 records and 15 original fields, was materially unreliable due to widespread formatting inconsistencies, invalid values, missing data, and duplicate entries. Without intervention, any analysis built on this data — whether for performance benchmarking, salary equity assessments, or workforce planning — would have produced misleading results.

Following the structured cleaning process, the dataset now meets a minimum standard for analytical use:

| Metric | Value |
|---|---|
| Total Records (post-dedup) | 200 |
| Total Fields | 19 (including 3 derived date fields) |
| Active Employees | 109 (54.5%) |
| Average Age | 42 years |
| Average Monthly Salary | ₦391,621 |
| Average Performance Score | 5.04 / 10 |
| Largest Department | Sales (33 employees) |
| Most Common Education Level | MSc (70 employees, 35%) |

The cleaned dataset is now structured with a unique EMP ID as primary key, standardised categorical fields, and consistent numeric types — making it ready for integration with BI tools, payroll systems, or further statistical analysis.

Long-term data quality, however, cannot be achieved through retrospective cleaning alone. The recommendations in Section E outline the systemic changes — input validation, standardised vocabularies, mandatory identifiers, and periodic audits — necessary to prevent recurrence and ensure the organisation can rely on its HR data as a strategic asset.

---

*Report prepared from analysis of `cleaned_perfprmsnce_dats.xlsx` | Sheets: `raw data`, `Guide`, `cleaned-data`*
