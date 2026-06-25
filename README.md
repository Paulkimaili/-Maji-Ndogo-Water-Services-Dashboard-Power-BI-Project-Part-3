


# 💧 Maji Ndogo Water Services Dashboard — Power BI Project (Part 3: Communicating)

An end-to-end Power BI project that turns the Maji Ndogo water survey data into a multi-page, stakeholder-ready report — complete with national and provincial drill-through pages, DAX-driven budget calculations, and interactive bookmark navigation — designed to help decision-makers plan and fund water access upgrades across the region.

---

## 📋 Overview

The **MD Water Services** dataset contains operational and field survey data collected to assess water access, quality, infrastructure, and service delivery across different locations in Maji Ndogo. It combines information on water sources, water quality, customer visits, queue times, pollution levels, infrastructure costs, project progress, and water-related incidents.

The dataset is organized into several related tables:

| Table | Description |
|---|---|
| `visits` | Records of field visits and service observations |
| `location` | Geographic information for surveyed areas |
| `water_source` | Details of available water sources |
| `well_pollution` | Water quality and contamination records |
| `queue_composition` | Information on waiting times and user queues |
| `project_progress` | Status of water improvement projects |
| `infrastructure_cost` | Costs associated with water infrastructure |
| `water_source_related_crime` | Crime incidents linked to water access points |

This phase of the project (**Part 3**) focuses on **report design and stakeholder communication**: building a national overview page, five provincial drill-through pages, and an interactive improvements/budget view — all powered by DAX measures and calculated columns layered on top of the cleaned, modeled data from Part 2.

### 👥 Designed Around Real User Stories

The report layout was driven by the needs of leaders of two levels:

**National Level**
1. Wants the key points of the survey results to understand the overall status of water access in Maji Ndogo, split by rural vs. urban populations.
2. Wants to know how many people are affected by water access challenges and what those challenges are — including the distribution and ranking of source types.
3. Wants to know how much money is needed to complete the upgrades and where that money should be spent — nationally, provincially, by rural/urban split, and by improvement type.
4. Wants to view this data both nationally and provincially, with the ability to drill down into each category.

**Provincial Leaders (Provincial Level)**
1. Need to see total people served for each water source type in their own province.
2. Need the number of water sources by type and rural/urban classification.
3. Need town-level stats relevant to their province.
4. Need relevant local data — queues, gender composition, crime, broken taps by town — where applicable.
5. Need a summary of improvements and their associated costs.

These user stories directly shaped the **National Overview page** and the **five Provincial drill-through pages** (Amanzi, Akatsi, Hawassa, Kilimani, Sokoto) described below.

---

## 📊 Dashboard Analytics

The report is built as a **National page** with **drill-through to five Provincial pages**, plus a toggleable **Province / Improvements** view on each provincial page.

### 🇰🇪 National Page
- **Total budget:** $147M across all of Maji Ndogo.
- **34% of the population has basic water access**, while **66% remains below basic access** — the central problem statement of the report.
- Rural vs. urban access varies sharply by province: **Akatsi and Kilimani** have the largest rural populations served, while **Hawassa** has a smaller, more balanced rural/urban split.
- **Shared taps and wells** serve the largest populations among rural communities, while **tap_in_home** sources dominate urban access — though many of these are intentionally de-emphasized in the visuals since they are not part of the upgrade scope.
- **Kilimani** has the highest number of planned improvements (~6,700), followed by Sokoto, Akatsi, Hawassa, and Amanzi.
- The most common improvement types across Maji Ndogo are, in order: **Install RO filter, Repair infrastructure, Install UV and RO filter, Install public tap(s), and Drill well**.
- Provincial budget breakdown: **Amanzi $13.43M, Hawassa $22.55M, Akatsi $31.36M, Kilimani $39.25M, Sokoto $40.15M** — totaling **$146.7M** in budgeted improvements against **27,628,140** people and **25,398** aggregated improvements.
- **Sokoto and Kilimani combined account for more than half of the total national budget** ($79,400,700 combined — i.e., Sokoto's $40,151,475 + Kilimani's $39,249,225).
- **Amanzi has the lowest cost per citizen to improve**, at roughly **$2.47/citizen**, compared to Akatsi ($5.23), Hawassa ($5.87), and Sokoto ($6.95) — making it the most cost-efficient province to upgrade.
- The improvement category with the **fewest individual records often costs the most overall** — for example, **drilling wells**, despite lower unit counts than tap installations, consumes a disproportionately large share of several provincial budgets (notably Sokoto, where it accounts for nearly half the province's total spend).
- **Aggregated "Install tap(s)" figures represent shared-tap sites**, not individual taps — each site may require between 1 and 8 taps, so a count like "3,696 Install tap(s)" refers to 3,696 shared-tap *sites* requiring new taps, not 3,696 individual taps or homes.

### 🏞️ Provincial Pages (Sokoto, Amanzi, Hawassa, Akatsi, Kilimani)
Each province has its own page with a `town_name` slicer, population, access %, budget allocation donut, rural/urban spending pie, improvement bar chart, and a town-level budget/improvement table.

- **Sokoto** — Population 5,774,434, **31% basic access**, total budget **$40.15M**. Drilling wells is the single largest cost driver in Sokoto, consuming nearly half of the province's budget. The **Rural** segment alone accounts for $32.08M (79.8%) of spend.
- **Amanzi** — Population 5,431,826, **41% basic access** (the highest of all provinces), total budget **$13.43M** — the smallest provincial budget and the lowest cost-per-citizen in Maji Ndogo. *Repair infrastructure* is the leading improvement type here.
- **Hawassa** — Population 3,843,810, **30% basic access**, total budget **$22.55M**, with rural spending dominating at $17.91M (79.4%). *Install UV and RO filter* and *Install RO filter* are the top improvement types.
- **Akatsi** — Population 5,993,306, **37% basic access**, total budget **$31.36M**, with $27.5M (87.72%) directed to rural areas. *Install RO filter* leads the improvement list, well ahead of repairs and tap installations.
- **Kilimani** — Population 6,584,764, **28% basic access** (the lowest of all provinces), total budget **$39.25M**. *Install RO filter* and *Install UV and RO filter* together make up the bulk of spend.
- Towns with the **highest budgets in their own province** are: **Ilanga (Sokoto), Zuri (Kilimani), Amina (Hawassa), Asmara (Akatsi), and Lusaka (Akatsi)** — note these are the top town *per province*, not the top towns overall across Maji Ndogo.
- A drill-through lookup example: the town with the budget breakdown — Install RO filter (87 units), Install UV and RO filter (104), Drill well (7), Install 8/7/6/5 taps nearby, and Diagnose local infrastructure, totaling roughly $858,500 — corresponds to **Djenne** (Hawassa province).

### 💧 Basic Water Access — Classification Logic
A water source is only classified as providing **basic access** if it meets all of the following criteria:
- **Rivers** are excluded — they are not an improved source.
- **Wells** count as basic only if they are **clean** (i.e., not chemically or biologically contaminated).
- **Public/shared taps** count as basic only if the **queue time is under 30 minutes**.
- **Broken infrastructure** (e.g., `tap_in_home_broken`) does not count as basic, since it is non-functional.
- **All taps installed in homes** (`tap_in_home`) are considered at least basic, and in fact qualify as **safely managed** — the highest tier of access.

This logic underpins the **"Pct basic access"** and **"Below basic access"** KPIs shown on the National and Provincial pages.

---

## 🧹 Data Cleaning

Building on the cleaning performed in Part 2, the following steps ensured the data was analysis-ready for reporting:

- **De-duplicated visits:** The `visits` table was filtered to handle instances where a single water source had been visited more than once, preventing inflated counts in downstream measures.
- **Removed NaNs in infrastructure data:** Null/blank entries were removed from the `infrastructure_cost` table so DAX calculations on cost wouldn't be skewed or error out.
- Standardized data types across all eight tables (dates, numeric fields, categorical text) to support accurate aggregation and relationship behavior.
- Corrected inconsistent casing/whitespace in categorical fields such as `province_name`, `town_name`, `improvement`, and `victim_gender`.
- Verified that every `source_id`, `location_id`, and `record_id` used in relationships resolved correctly across tables (no orphaned keys).

---

## 🔧 Data Transformation (Extracted/New Columns & DAX)

This phase relied heavily on **DAX measures and calculated columns** to convert raw operational data into the budget, access, and classification metrics used throughout the report:

- **`Basic_water_access` / `Basic_water_access_level`** — calculated columns in `water_source` that classify each source as basic or sub-basic, applying the river/well-pollution/queue-time/broken-infrastructure rules described above.
- **`Average_queue_time`** — calculated per source from `visits` and `queue_composition` data to support the 30-minute basic-access threshold for public taps.
- **`Rural_adjusted_cost`** — a calculated column in `infrastructure_cost` that applies an initial 50% cost increase to rural improvements (reflecting the higher cost of rural works), forming the base for any further province-specific adjustments.
- **`Sokoto_budget` (example adjustment measure)** — demonstrates applying an additional 30% increase on top of the rural adjustment:
  ```dax
  Sokoto_budget = infrastructure_cost[Rural_adjusted_cost] * 1.3
  ```
  This correctly compounds onto the existing rural adjustment rather than recalculating from the raw `unit_cost_USD`.
- **`cost_per_citizen`** — a measure calculated as:
  ```dax
  cost_per_citizen = SUM(Budgeted_improvement_cost) / SUM(number_of_people_served)
  ```
  used to compare improvement cost-efficiency across provinces (e.g., Amanzi ≈ $2.47, Akatsi ≈ $5.23, Hawassa ≈ $5.87, Sokoto ≈ $6.95).
- **`Aggregated_improvements`** — a grouping column/measure that consolidates granular improvement types (e.g., "Install 5 taps nearby", "Install 6 taps nearby" … "Install 8 taps nearby") into a single **"Install tap(s)"** category, so the number of *sites* needing taps is represented correctly instead of being split across near-duplicate categories.
- **`Pct_basic_access` / `Below_basic_access`** — national and provincial KPI measures expressing the share of the population with/without basic access.
- **`Total_budget` / `Budgeted_improvement_cost`** — measures rolling up `infrastructure_cost` line items into province-level and national-level totals shown in card visuals and donut charts.

---

## 🔗 Model View

The relational model (see `model_view.png`) connects all eight tables through a consistent set of keys, enabling cross-filtering across every page and visual:

- `location` (1) → (∞) `visits` — each location can have multiple recorded visits.
- `location` (1) → (∞) `water_source_related_crime` — crime incidents linked via `loc_id`.
- `visits` (1) → (∞) `queue_composition` — each visit links to its queue composition record via `record_id`.
- `visits` (1) → (∞) `water_source` and `well_pollution` — both linked to the underlying `source_id`.
- `water_source` (1) → (∞) `project_progress` — improvement projects tracked per source.
- `project_progress` (1) → (∞) `infrastructure_cost` — cost line items tied to each improvement.

Relationships were rectified to resolve ambiguous or duplicated join paths between `visits`, `water_source`, and `well_pollution`, ensuring slicers (province, town, improvement type) filter consistently across every visual on every page rather than producing conflicting results.

📷 *See `model_view.png` in the resources for the full relationship diagram.*

---

## 📈 Key Visuals

| Visual Type | Example Use in This Report |
|---|---|
| **Pie / Donut Chart** | Budgeted improvement cost by province (national); Budget allocation by improvement type (provincial); Rural/urban spending split |
| **Map (Shape Map)** | Province name selector/highlighter on National and Provincial pages |
| **Simple Bar/Column Chart** | Number of improvements by province; Aggregated improvements by type; Improvement bar charts on each provincial page |
| **Comparative/Stacked Bar Chart** | Rural vs. urban water access by province; Rural vs. urban water access by source type |
| **Card / KPI Visual** | Total budget, Pct basic access, Below basic access, Total population, Cost per citizen |
| **Table/Matrix** | Province-level budget & population table (national); Town-level population/budget/cost-per-citizen table (provincial) |
| **Line Chart** | Used in supporting queue-time and trend analysis carried over from Part 2 (average queue time by hour/day) |
| **Scatter Plot** | Cost-efficiency analysis exploring the relationship between budget, population served, and improvement quantity |
| **Toggle Buttons / Bookmarks** | Province ↔ Improvements view toggle on provincial pages |

---

## 🛠️ Tools & Technologies

- **Power BI Desktop** — report design, page layout, and visualization
- **Power Query (M language)** — data cleaning and transformation
- **DAX** — calculated columns and measures (basic access classification, rural-adjusted costs, cost per citizen, aggregated improvements)
- **Power BI Bookmarks & Selection Pane** — interactive Province/Improvements toggle
- **Drill-through Pages** — province-level navigation from the National page
- **Excel (.xlsx)** — original source data format (`Md_water_services.xlsx`)
- **GitHub** — version control and project documentation

---

## ▶️ How to Use

1. **Clone or download** this repository.
   ```bash
   git clone https://github.com/<your-username>/maji-ndogo-water-services-report.git
   ```
2. Open **`Md_water_services.xlsx`** to review the raw source tables (optional).
3. Open the **`.pbix`** file in **Power BI Desktop**.
4. Review the **Power Query Editor** steps and the **DAX measures/calculated columns** in the Fields pane to understand the transformation logic.
5. Go to **Model View** to inspect the relationships between tables.
6. On the **National page**, right-click any province in the map or table and select **Drill through → [Province name]** to open its dedicated provincial page.
7. On each provincial page, use the **Province / Improvements toggle buttons** to switch between the town-level summary table and the improvements/budget breakdown.
8. Use the **`town_name` slicer** on each provincial page to filter all visuals down to a single town.

---

## 🌳 Project Structure

```
maji-ndogo-water-services-report/
│
├── data/
│   └── Md_water_services.xlsx          # Raw source dataset (all 8 tables)
│
├── images/
│   ├── water_sources.png               # National/provincial water source visuals
│   ├── queue_information.png           # Queue analysis visuals
│   ├── pollution_status.png            # Well pollution status visuals
│   ├── water_source_related_crimes.png # Crime analysis visuals
│   └── model_view.png                  # Power BI data model / relationships
│
├── Maji_Ndogo_Water_Services_Report.pbix   # Main Power BI report file
│
└── README.md                           # Project documentation (this file)
```

---

## ✅ Project Challenge — Sample Analysis Findings

This project was validated against a 10-question report-design and analysis challenge (scored on accuracy and design reasoning), confirming the report correctly supports findings such as:

- A clear, logical layout that guides the viewer's eye is a key consideration for effective visual communication on a Power BI report page.
- Drill-through functionality is the correct mechanism for letting a user select a province on the main page and view its details on a separate page.
- The correctly adjusted Sokoto upgrade cost formula multiplies the already rural-adjusted cost by 1.3, rather than reapplying the increase to the raw unit cost.
- A figure such as "3,696 Install tap(s)" represents the number of shared-tap **sites** requiring new taps, not the number of individual taps or homes.
- Hawassa is **not** the only province where rural homes with taps outnumber urban homes with taps — Akatsi shares this pattern, and Sokoto shows a similar pattern for broken taps.
- The combined budget of Kilimani and Sokoto is **$79,400,700**.
- **Drilling wells** accounts for the highest cost of any improvement type in Sokoto.
- The towns with the highest budget within their own province are **Ilanga (Sokoto), Zuri (Kilimani), Amina (Hawassa), Asmara (Akatsi), and Lusaka (Akatsi)**.
- The town matching a specific improvement-cost breakdown (RO filters, UV/RO filters, well drilling, and various tap installations) is **Djenne**.
- **Amanzi** has the lowest cost per citizen to improve, at approximately **$2.47/citizen**.

These results demonstrate that the report's structure, DAX logic, and drill-through/bookmark navigation correctly support detailed financial and access-related analysis at both the national and provincial level.

---

## 👤 Author / Developer

**[Your Name]**
Data Analyst | Power BI Developer

- GitHub: [your-github-profile](https://github.com/your-username)
- LinkedIn: [your-linkedin-profile](https://linkedin.com/in/your-username)

If you found this project useful or interesting, please consider **starring ⭐ the repo** — it helps others discover it and supports more projects like this one!

---

## 📚 List of Resources

| Resource | Description |
|---|---|
| `Md_water_services.xlsx` | Original raw dataset containing all 8 source tables |
| `water_sources.png` | Dashboard view of water source distribution across towns/provinces |
| `queue_information.png` | Dashboard view of queue time and composition analysis |
| `pollution_status.png` | Dashboard view of well pollution status by province |
| `water_source_related_crimes.png` | Dashboard view of crime statistics linked to water access |
| `model_view.png` | Power BI data model showing table relationships |

---

⭐ *If this project helped you understand Power BI report design, DAX, and stakeholder-driven dashboard storytelling, give it a star!*
