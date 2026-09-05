# Business Intelligence & Data Warehousing Portfolio

Two hands-on projects covering the full analytics chain: **source data → ETL → dimensional model → data warehouse → Power BI reporting**.

| | Project | Context | Core stack |
|---|---|---|---|
| 1 | [AI & Gaming EDIH – KPI analysis](#1-ai--gaming-edih--kpi-analysis) | Professional work, University of Zagreb, Faculty of Organization and Informatics (FOI) | Power BI, star schema, Azure Maps |
| 2 | [New York State public-sector salaries – end-to-end data warehouse](#2-new-york-state-public-sector-salaries--end-to-end-data-warehouse) | MSc coursework, *Data Warehousing and Business Intelligence* | MySQL, SQL, ETL, Power BI |

---

## 1. AI & Gaming EDIH – KPI analysis

**`/Analiza KPI za EDIH projekt`**

A Power BI reporting solution built for the first official reporting period of the **AI & Gaming EDIH** project (European Digital Innovation Hub supporting SMEs across Central Croatia and the Northern Adriatic).

### Data model

A star schema built on the project's operational reporting data:

- **Fact table** — one row per service cycle delivered to a beneficiary company, with the measures *service value* and *state-aid amount*, plus cycle start and end dates split into day / month / year attributes.
- **Dimensions (8)** — company, company type, legal-form category, service, delivery cycle, status, city, and NUTS2 region.

Splitting a wide operational export into conformed dimensions is what makes the report sliceable: every visual below is driven by the same model rather than by one-off queries.

### Reports

**Page 1 — Overview**

- Status distribution of company applications for state aid
- Beneficiary companies by NUTS2 region
- Legal-form breakdown of beneficiary companies
- Geographic coverage of EDIH services (Azure Maps)
- Cycle duration table
- Aid amounts, service values and number of delivered services per cycle
- Companies that participated in training

**Page 2 — Value of state aid**

- Aid amount by legal form of company
- Aid amount by service
- Aid amount by NUTS2 region
- Aid utilisation over the reporting period (trend)

### Why it matters for a Data & Analytics Engineer role

The report is the reporting layer of a funded programme: the figures feed official reporting obligations, so the model had to be correct, reproducible and understandable to non-technical project staff. Requirements came from project stakeholders, not from a specification — translating "we need to show what we delivered and to whom" into a dimensional model and a two-page dashboard was the actual work.

**Files:** `EDIH_prvoIzvjestajnoRazdoblje.pbix` (report + model), `dwhProjekt_*.xlsx` (fact and dimension tables).

---

## 2. New York State public-sector salaries – end-to-end data warehouse

**`/Analiza_podataka_o_plaćama_u_javnim_službama_države_New_York`**

MSc project for the course *Data Warehousing and Business Intelligence* (Databases and Knowledge Bases programme, FOI; mentor: prof. dr. sc. Kornelije Rabuzin, May 2021). Built from scratch: a public raw dataset in, a working data warehouse and a Power BI report set out.

### ETL

Source: publicly published salary information for New York State public authorities — raw CSV, 20 columns.

**Extract / profile**
- Assessed completeness per attribute; dropped `Has Employees`, `Middle Initial`, `Department` and `Paid by State or Local Government` for missing or unknown values
- Removed authorities with no recorded employees
- Dropped undocumented or analytically irrelevant attributes (`Exempt Indicator`, `Paid By Another Entity`)

**Transform**
- `Pay Type` decoded from `PT` / `FT` to `Part time` / `Full time`
- `Fiscal Year End Date` reformatted from `mm/dd/yyyy` to `yyyy-mm-dd` for MySQL compatibility
- Added an explicit currency attribute so monetary measures carry their unit

Result: a clean 15-attribute dataset ready for loading.

**Load**
- Staged into a `dataset` table in MySQL (MySQL Workbench, local XAMPP server) via `LOAD DATA INFILE`

### Dimensional model

Database `analizaPlacaSPPI`, star schema:

- **Fact table** `placa_zaposlenika` — measures: base annualised salary, actual salary paid, overtime paid, performance bonus, extra pay, other compensation, total compensation
- **Dimensions** — employee, authority (company), date, job title / role, job group, employment type, currency

The date dimension is generated in SQL with a date-spine query rather than typed by hand, and each dimension is populated with `INSERT … SELECT DISTINCT` from the staged table; foreign keys on the fact table are then resolved through helper tables. All modelling is done in SQL — the repository's PDF contains every statement used.

### Power BI reports

Power BI connected directly to the MySQL warehouse. Five reports:

1. Average base annual salary of employees in New York State public authorities
2. Average base annual salary over time
3. Average total annual compensation by job group
4. Average total annual compensation by role
5. Paid overtime by authority

**Files:** `izvjestaji.pbix` (Power BI report), `Antolos_Jerry_John_…pdf` (full written report — data selection, ETL, SQL, model, reports).

> **Note:** the written report is in Croatian. The SQL, the schema, the screenshots and the Power BI file are language-independent; this README summarises the content in English.

---

## Skills demonstrated

| Area | Evidence |
|---|---|
| SQL and data querying | Full warehouse built in SQL — DDL, `INSERT … SELECT DISTINCT` dimension loads, date-spine generation, fact-table key resolution |
| Data warehousing and data modeling | Two independent star schemas: employee-salary facts (7 dimensions) and EDIH service-cycle facts (8 dimensions) |
| ETL / ELT and data integration | Profiling, cleansing, decoding, date and format standardisation, staged load into MySQL; Excel-sourced dimensional load for the EDIH model |
| Business Intelligence and reporting | Two Power BI report sets — 5 salary reports and a 2-page EDIH KPI dashboard with maps, trends and breakdowns |
| Power BI | Semantic model design, relationships, report pages, Azure Maps, custom visuals |
| Data quality | Attribute-level completeness assessment and documented exclusion decisions before modelling |
| Working with stakeholders | EDIH reporting built to serve project reporting obligations and non-technical project staff |

## Stack

`MySQL` · `SQL` · `Power BI` · `Power Query` · `Excel` · `MySQL Workbench` · `XAMPP` · `Azure Maps`

---

**Jerry J. Antolos** — MSc in Informatics (Databases and Knowledge Bases), University of Zagreb
[LinkedIn](https://www.linkedin.com/in/jerryjantolos/) · [Google Scholar](https://scholar.google.com/citations?hl=en&user=G69tcycAAAAJ)
