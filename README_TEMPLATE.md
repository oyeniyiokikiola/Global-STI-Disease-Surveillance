# [Global STI Disease Surveillance: Identifying Where Intervention Coverage Fails to Match Disease Burden]
> *Analyzed 11 years of infectious disease surveillance data across 12 countries to identify which populations, regions, and diseases represent the highest-priority gaps between disease burden and treatment access*

---

## ⚙️ Project Type Flags

- [ ] Exploratory Data Analysis (EDA)
- [ ] Dashboard / Data Visualization
- [ ] Data Cleaning / Wrangling
- [ ] End-to-End


---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#8-analysis--metrics)
8. [Key Insights](#9-key-insights)
9. [Recommendations](#10-recommendations)
10. [Assumptions & Limitations](#11-assumptions--limitations)
11. [Future Enhancements](#12-future-enhancements)
12. [Deliverables](#13-deliverables)
13. [Author](#14-author)

---

## 1. Project Overview

**Context:** Health ministries and global health organizations track disease prevalence and treatment coverage as separate line items, two numbers sitting in two different reports, rarely looked at side by side. But put them next to each other for the same country, and a different picture shows up: some countries have high disease prevalence and high treatment coverage. Others have high prevalence and low coverage. The second group is where people are dying not because medicine doesn't exist, but because it isn't reaching them

**Problem Statement:** The specific question or challenge you were addressing.

**Approach:** This project takes 11 years of surveillance data (2015–2025) across 12 countries and four diseases — HIV/AIDS, HPV, Hepatitis B, and Syphilis — and asks a direct question: where is that gap between prevalence and treatment the widest, and for which diseases and populations?

**Outcome:** The analysis found that Africa as a region tests less than 60% of its at-risk population, meaning the prevalence numbers themselves are probably an undercount. It also found that vaccination programs for HPV and Hepatitis B only show a measurable effect on new infections once coverage crosses roughly 75% — a threshold most African countries in this dataset haven't reached.

---

## 2. Objectives

- **Primary Objective:** Identify which countries have the widest gap between disease prevalence and treatment coverage — the places where being sick and getting treated are most disconnected.
- **Secondary Objective 1:** Determine which disease causes the most deaths in absolute terms, not as an average.
- **Secondary Objective 2:** Test whether vaccination coverage for HPV and Hepatitis B actually correlates with fewer new infections.
- **Secondary Objective 3:** Track whether new cases for each disease have gone up, down, or stayed flat over the 2015-2025 period

---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|-----------|---------|
| **In Scope** | Surveillance data for HIV/AIDS, HPV, Hepatitis B, and Syphilis across 12 countries (Nigeria, USA, India, UK, Brazil, Kenya, China, Germany, Australia, Ghana, France, Canada, South Africa, Japan), 2015–2025. Analysis covers total cases, new cases, deaths, prevalence rate, incidence rate, tested population, vaccinated population, and treatment coverage, broken down by age group (15-24, 25-49, 50+) and gender |
| **Out of Scope** | Sub-annual (monthly/quarterly) trends were excluded — the dataset has annual granularity only, so seasonality cannot be assessed. |
| **Time Period** | 2015-2025 |
| **Granularity** | One row per Country–Year–Disease–Age Group–Gender combination (approximately 850 rows) |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Storage |  CSV files |
| Data Processing | Power Query |
| Analysis | DAX (Power BI measures) |
| Visualization | Power BI |
| Version Control | GitHub |
| Documentation | Markdown |


---

## 4. Repository Structure

```
[Global STI Surveillance Dashboard]/
│
├── data/
│   ├── raw/                  # Original Health_Dataset_csv- unmodified
│   ├── processed/            # Power Query cleaned export
│   └── external/             # image URLs table
│
├── visuals/                  # sti_surveillance.pbix
│
├── docs/                     # Problem_statement.md
└── README.md                 
```
---

## 5. Data Workflow

```
Health_Dataset CSV exports pulled from WHO website.
      ↓
Loaded into Power BI through Power Query
      ↓
Fixed formatting issues, trim/capitalize the country column, created custom column for Prevelance rate and treatment coverage in Power Query.
      ↓
DAX: KPI measures, YoY measures, dynamic insights
      ↓
Three-page Power BI dashboard

```

1. **Source:** Single CSV file pulled from WHO website containing global infectious disease surveillance data — approximately 850 rows, 15 columns, covering 12 countries and 4 diseases from 2015–2025.
2. **Ingestion:** Loaded directly into Power BI Desktop via Get Data → CSV
3. **Cleaning:** The dataset contained mixed-format percentage fields — Prevalence_Rate and Treatment_Coverage were stored inconsistently across rows, with some values as text strings like "4.00%" and others as plain numbers like 4 or 0.04. A custom Power Query column standardized all values to a single decimal-based format using conditional logic. The Country column had inconsistent spacing and capitalization for three countries (China, France, Nigeria), which caused them to be excluded from slicers and visuals — resolved using Trim and Capitalize Each Word transformations. The New_Cases and Year columns were verified and converted to whole number types to ensure correct chronological sorting on trend charts.
4. **Transformation:** Created DAX measures for Avg Prevalence Rate %, Avg Treatment Coverage %, Vaccination Rate % (filtered to HPV and Hepatitis B only, since HIV/AIDS and Syphilis have no approved vaccine and report zero vaccinated population), Year-over-Year (YoY) % change for new cases, and a Treatment Gap From WHO Target measure. Three dynamic insight measures were created using SWITCH on the Disease slicer to generate page-specific takeaways.
5. **Analysis:** Descriptive aggregation (totals, averages) by disease, region, country, age group, and gender; year-over-year trend analysis for new cases and treatment coverage; correlation analysis between vaccination rate and new case counts for vaccine-eligible diseases; gap analysis comparing country-level prevalence against treatment coverage to identify intervention priorities.
6. **Output:** A three-page interactive Power BI dashboard (The Burden, The Response, The Gap

---

## 6. Data Model & Schema

### Dataset / Table: `[Health_Dataset]`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `[Year]` | int | Surveillance Year | 2020 |
| `[Country]` | string | Country where data was recorded | Nigeria |
| `[Region]` | string | Continental Region | Africa |
| `[Disease]` | string | One of the sexually transmitted diseases | Hepatitis B |
| `[Age_Group]` | string | Age cohort | 25-29 |
| `[Gender]` | string | Female or Male | Male |
| `[Population]` | int | At-risk population for this country-disease-age-gender combination] | 18000000 |
| `[Total_Cases]` | int | Cumulative total cases recorded | 720000 |
| `[New_Cases]` | int | New cases recorded in that year] | 86400 |
| `[Deaths]` | int | Total deaths recorded] | 36000 |
| `[Prevelance_Rate]` | decimal | Total cases as a proportion of population] | 0.04 |
| `[Incidence_Rate]` | int | New cases per population scalar | 480 |
| `[Tested_Population]` | int | Number of people from the at-risk population who were screened] | 9000000 |
| `[Vaccinated_Population]` | int | Number of people who received the vaccine for Hepatiti B and HPV only | 623000 |
| `[Treatment_Coverage]` | decimal | Proportion of diagnosed population receiving treatment | 0.05 |

> **Row count (approx.):** 850 rows
> **Date range:** 2015 – 2025


---


## 7. Analysis & Metrics

### Analytical Approach

This started as exploratory analysis, but it became hypothesis-driven. The assumption going in was that high disease prevalence and low treatment coverage don't always move together, and the mismatch is where the real story sits. Everything that follows was built to test that.

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|--------|--------------------------|----------------|
| `Avg Prevalence Rate %` | The average proportion of the at-risk population currently living with a given disease, across the selected filter context. | Identifies which countries and diseases carry the highest disease concentration — the starting point for prioritization |
| `Avg Treatment Rate %` | The average proportion of diagnosed individuals who are receiving treatment. | Directly measures whether people who are sick are being helped — the core "is the system working" metric. |
| `Vaccination Rate %` | Vaccinated population as a percentage of tested population, calculated only for HPV and Hepatitis B (diseases with an approved vaccine). | Tests whether vaccination programs are reaching enough people to produce a measurable effect on new infections |
| `Testing Rate %` | Tested population as a percentage of total at-risk population, by region. | Reveals the size of the "hidden" disease burden — people who are sick but never counted because they were never tested. |
| `YoY New Cases % Change` | The percentage change in new cases compared to the same disease in the prior year. | Answers whether incidence is rising or falling — the central question for evaluating whether current interventions are working. |
| `Treatment Gap from WHO Target` | The number of percentage points a region's average treatment coverage falls below the WHO 90% target. | Converts an abstract coverage percentage into a concrete, comparable gap size — directly usable for prioritizing where funding should go.|

### Methods Used

- Descriptive statistics - totals, averages, and distribution across diseases, region, country, age group, and gender
- Trend analysis across 2015-2025
- Segmentation / group comparison by age group, gender
- Correlation analysis between vaccination rate and new case counts
- Gap analysis between prevelance rate and treatment coverage rate
- Custom aggregation or transformation logic in DAX

---

## 8. Key Insights

**Insight 1: Africa's testing gap means the recorded case count understates the real burden**
Africa tests only 55-58% of its at-risk population — the widest gap of any region, compared to 65-67% in Europe and Oceania. This means a significant share of Africa's actual disease burden is never recorded in surveillance data. Any regional comparison based on recorded case counts alone understates Africa's true burden relative to other regions, and any intervention planning based on these numbers should account for this hidden gap.

**Insight 2: HIV/AIDS carries the highest total death burden across the dataset**
HIV/AIDS accounts for the largest share of recorded deaths across all 12 countries and 11 years, with Hepatitis B second. This places HIV/AIDS as the top priority disease for mortality-focused intervention, particularly given that effective treatment exists and is not a question of medical availability but of access.

**Insight 3: The 25-49 age group carries the dominant disease burden across all four diseases**
Across nearly every disease and country, the 25-49 age group accounts for the largest share of total cases, with the female cohort in this group consistently representing the highest-risk population. This concentrates the intervention target population — working-age adults, disproportionately female — and suggests that programs targeting this specific demographic would address the largest share of overall burden.

**Insight 4 : Vaccination above approximately 75% correlates with measurably fewer new infections**
For HPV and Hepatitis B — the only two diseases in this dataset with an approved vaccine — countries achieving vaccination coverage above approximately 75% show lower new case counts than countries below that threshold. Africa's average vaccination coverage sits below 65% for both diseases, placing it below the threshold where the correlation becomes protective. This suggests scaling vaccination programs in Africa toward the 75% mark is one of the highest-return interventions available in this dataset.

**Insight 5 : Treatment coverage has not closed the gap to the WHO 90% target in any region, and Africa remains furthest behind**
Across all five regions, average treatment coverage falls short of the WHO 90% target. Africa's gap is the largest at approximately 20 percentage points below target, compared to roughly 1 point for Oceania. Over the 11-year period, this gap has not meaningfully narrowed — Africa's treatment coverage moved only marginally while other regions moved further toward the target, meaning the relative gap has not closed and may be widening.

**Insight 6 : Nigeria, South Africa, Ghana, and Kenya represent the clearest intervention-priority cluster**
The prevalence-vs-treatment-coverage analysis places these four countries consistently in the "high prevalence, low treatment coverage" quadrant across diseases. Since treatment for all four diseases in this dataset is medically available and effective, the persistence of this cluster points to an access and delivery problem, making it the most actionable finding in the dataset for resource allocation decisions.


---

## 9. Recommendations

| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | Direct expanded testing programs toward Africa specifically, targeting the 42-45% of the at-risk population currently unscreened, to establish a more accurate baseline of true disease burden before further intervention planning. | Insight 1— Africa's testing gap | Regional Health ministries/WHO Africa regional office |
| High | Prioritize treatment access programs (not new treatment development) in Nigeria, South Africa, Ghana, and Kenya, focusing on delivery infrastructure and access barriers rather than medical availability. | Insight 6— Intervention priority | Nationl Health Ministries |
| Medium | Scale HPV and Hepatitis B vaccination programs in Africa toward the 75% coverage threshold, using the correlation with reduced new cases as the evidence basis for funding requests. | Insight 4— Vaccination threshold correlation | Vaccination program coordinators |
| Low | Conduct a follow-up analysis isolating Africa's treatment coverage trend against the global average over time, to quantify whether the gap is widening, narrowing, or static | Insight 5— Treatment coverage gap | Future project itireation |

---

## 10. Assumptions & Limitations

### Assumptions
- Where age group data was absent for a specific Country–Disease–Year combination (e.g., no 15-24 Hepatitis B records before 2022), this was treated as a genuine data gap reflecting surveillance scope at that time, not a data error — confirmed during the data quality audit.
- The Population field for each row was treated as the relevant at-risk cohort denominator (Country–Disease–Age Group–Gender specific), not a national population figure — prevalence and testing rates were calculated against this row-level population, not external census data.
- The dataset's annual granularity was accepted as given; no assumption was made about within-year (monthly/quarterly) patterns.

### Limitations
- Male HPV records are only present in the dataset from 2021 onward. Any pre-2021 HPV analysis is effectively female-only, and gender comparisons for HPV are only valid for the 2021-2025 window.
- The 50+ age group first appears in the dataset from 2018 onward and becomes widespread by 2022. This means post-2018 total case counts reflect both a possible real increase and an expansion in surveillance scope — the two effects cannot be fully separated in this dataset.

---

## 11. Future Enhancements

- [ ] Enhancement 1 - Add an Africa-vs-global-average line to the Treatment Coverage Trend chart on "The Gap" page, to directly visualize whether the regional gap identified in Insight 5 is widening or narrowing over time.
- [ ] Enhancement 1 - Extend the dashboard with a country-level drill-through page, allowing a user to select one country (e.g., Nigeria) and see all four diseases' burden, vaccination, and treatment metrics on a single focused view.


---

## 12. Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| Health_Dataset_csv | Raw surveillance data — 12 countries, 4 diseases, 2015–2025, ~850 rows | `/path/to/file` |
| sti_surveillance.pbix | Three-page interactive Power BI dashboard (The Burden, The Response, The Gap) | `/path/to/file` |
| README.md | This documentation | `/README.md` |

---

## 13. Author

**Okikiola Oyeniyi**
Health Data Analyst

- 🔗 https://www.linkedin.com/in/okikiola-oyeniyi-
- 💼 https://oyeniyiokikiola.github.io/
- 📧 oyeniyiokikiola@gmail.com

---

*Last updated: June 2026*

