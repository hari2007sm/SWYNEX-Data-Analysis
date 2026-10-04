# Task 4: Final Data Analytics Capstone Report

Part of the **SWYNEX Technologies Data Analytics Internship** program.

---

## 📌 Project Overview
This repository subfolder contains the finalized end-to-end case study for the retail cafe sales analytics program. It consolidates the problem statement, data engineering pipeline, exploratory statistical modeling, interactive dashboard design, and strategic business interventions into an executive-facing analytics report.

---

## 🏗️ Analytics Lifecycle & Methodology
┌─────────────────────────────────┐
│ 1. Data Ingestion & Sanitation  │ ──► Reconstructed missing values & removed corrupt sentinels
└─────────────────────────────────┘
│
▼
┌─────────────────────────────────┐
│ 2. Exploratory Data Analysis    │ ──► Computed distributions, item margins & seasonal trends
└─────────────────────────────────┘
│
▼
┌─────────────────────────────────┐
│ 3. Power BI Dashboard Design    │ ──► Deployed dynamic cross-filtering & KPI scorecards
└─────────────────────────────────┘
│
▼
┌─────────────────────────────────┐
│ 4. Strategic Business Roadmap   │ ──► Formulated food bundling & margin maximization plans
└─────────────────────────────────┘


---

## 🔍 Dataset Diagnostic & Pipeline Remediation

The uncurated transaction log (`dirty_cafe_sales.csv`) contained 10,000 raw sales entries with systematic structural flaws:

* **Corrupted Sentinel Tokens:** Fields were populated with string placeholders (`"ERROR"`, `"UNKNOWN"`), coercing numeric fields into generic object datatypes.
* **Accounting Inconsistencies:** Missing entries in `Total Spent` and `Quantity` despite valid price references.
* **Categorical Sparsity:** Missing values across operational dimensions (`Payment Method` at ~31.8% and `Location` at ~39.6%).

### Engineering Solutions:
1. **Sentinel Mapping:** Converted all placeholder tokens into standard IEEE `NaN` floating-point representations.
2. **Deterministic Domain Lookup:** Mapped menu item standard prices (`Salad`: $5.0, `Sandwich`: $4.0, `Smoothie`: $4.0, `Cake`: $3.0, `Juice`: $3.0, `Coffee`: $2.0, `Tea`: $1.5, `Cookie`: $1.0) to impute missing unit costs deterministically without distorting variance.
3. **Mathematical Identity Reconstruction:** Recovered missing amounts via:
   $$\text{Total Spent} = \text{Quantity} \times \text{Price Per Unit}$$
   $$\text{Quantity} = \frac{\text{Total Spent}}{\text{Price Per Unit}}$$
4. **Schema Enforcement:** Standardized quantities to integer types, currencies to floating-point numbers, and timestamps to ISO `datetime64[ns]`.

---

## 📊 Core Empirical Findings & KPI Benchmarks

Across 10,000 sanitized cafe transactions spanning 2023:

* **Total Revenue:** $89,304.50
* **Total Transactions:** 10,000 orders
* **Gross Unit Volume Sold:** 30,249 items
* **Average Order Value (AOV):** $8.93 (Standard Deviation: $6.00)
* **Median Order Spend:** $8.00

### Top 5 Strategic Discoveries:
1. **Salad Drives Maximum Top-Line Value:** Generated **$17,375.00** across 3,475 units sold, commanding the highest average ticket size (**$15.14**) due to its $5.00 price point.
2. **Coffee is a High-Frequency Loss/Lead Driver:** Led the menu in overall unit volume (**3,551 units** across 1,165 tickets) but yielded only **$7,102.00** in total sales due to its low unit price ($2.00).
3. **Mid-Year Spending Seasonality:** Sales remained stable around a $7,442.00/month baseline, reaching an annual maximum in **June ($7,799.50)** and a localized trough in **February ($7,032.00)**.
4. **Uniform Basket Sizing:** Order quantities between 1 and 5 units exhibited a near-perfect 20% distribution each, demonstrating equal operational demand for solo snacks and multi-person group orders.
5. **Channel Parity Across Payment Modes:** Known transactions were split evenly across **Digital Wallets (33.6%)**, **Credit Cards (33.3%)**, and **Cash (33.1%)**.

---

## 💻 Business Intelligence Solution (Power BI)

An executive dashboard was designed in **Microsoft Power BI Desktop** styled with a cafe-inspired espresso-and-cream aesthetic:

* **Executive Cards:** Real-time visibility into Total Revenue ($89.30K), Total Orders (10K), Units Sold (30.25K), and AOV ($8.93).
* **Multi-Attribute Slicers:** Responsive dropdown filtering across Store Locations, Menu Items, Payment Methods, and a continuous 365-day Date timeline slider.
* **Visual Components:**
  * Horizontal bar chart ranking menu item revenue contribution.
  * Shaded area trend chart tracking monthly seasonality cycles.
  * Donut chart mapping payment channel adoption shares.
  * Column chart breaking down order quantity frequencies.

---

## 💡 Actionable Business Recommendations

| Focus Area | Identified Bottleneck | Proposed Intervention | Projected Outcome |
| :--- | :--- | :--- | :--- |
| **Morning Ticket Sizes** | High coffee traffic (3,551 units) with low spend ($6.10 avg ticket). | Introduce paired breakfast bundles (e.g., Coffee + Sandwich for $5.50). | Lifts beverage-only tickets toward the $8.93 storewide average. |
| **High-Margin Upselling** | Salad yields the highest ticket size ($15.14) and top revenue ($17.4K). | Position salads as primary combo upgrades on digital order boards. | Expands lunch revenue contribution. |
| **Seasonality Smoothing** | Revenue dips to annual lows in February ($7,032.00). | Launch limited-time warm beverage promotions and loyalty point boosters in Q1. | Dampens post-holiday revenue volatility. |
| **POS Infrastructure** | Payment modes are split equally between digital wallets, cards, and cash. | Maintain dual hardware redundancy across contactless terminals and cash tills. | Eliminates checkout delays and reduces lost transactions. |

---

## 📁 Folder Contents

```text
Task-4-Final-Analytics-Report/
└── README.md              # Comprehensive final case study and analytics documentation