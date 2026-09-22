# Task 1: Data Cleaning & Preparation

Part of the **SWYNEX Technologies Data Analytics Internship** program[cite: 1].

---

## 📌 Project Overview
Real-world operational datasets frequently suffer from corrupted values, inconsistent data types, and incomplete fields[cite: 1]. This project delivers an automated, reproducible data cleaning and preparation pipeline using **Python** and **Pandas** on an uncurated point-of-sale dataset (`dirty_cafe_sales.csv`) containing 10,000 transaction records[cite: 1, 2].

The pipeline resolves structural and numeric bugs, standardizes categorical fields, enforces schema types, and outputs a ready-to-analyze dataset (`cleaned_cafe_sales.csv`)[cite: 1, 2].

---

## 🔍 Dataset Diagnosis & Flaws Identified

| Issue Category | Description in Raw Dataset | Impact |
| :--- | :--- | :--- |
| **Sentinel Strings** | Injected placeholder strings (`"ERROR"`, `"UNKNOWN"`) present across text, numeric, and date fields[cite: 1, 2] | Forced entire numeric and temporal columns to load as generic Python `object` types |
| **Type Inconsistencies** | Quantities, unit prices, totals, and timestamps stored as text/object representations[cite: 1, 2] | Prevented numeric operations, financial aggregations, and time-series analysis[cite: 1] |
| **Sparsity & Missing Values** | High volume of nulls: ~31% in `Payment Method` (3,178 records) and ~39% in `Location` (3,961 records)[cite: 1, 2] | Missing operational dimensions required for customer cohort segmentation[cite: 1] |
| **Arithmetic Mismatches** | Missing `Total Spent` or `Quantity` fields despite existing prices[cite: 1, 2] | Internal revenue accounting discrepancies[cite: 1] |

---

## 🛠️ Cleaning Methodology & Steps

1. **Deduplication:** Scanned for and dropped identical duplicate records[cite: 1, 2].
2. **Sentinel Replacement:** Mapped all occurrences of `"ERROR"` and `"UNKNOWN"` to standard IEEE `NaN` representations[cite: 2].
3. **Deterministic Domain Imputation:** Extracted menu pricing from valid records (`Coffee` = $2.0, `Cake` = $3.0, `Salad` = $5.0, `Tea` = $1.5, etc.) and imputed missing `Price Per Unit` based on the item ordered[cite: 2].
4. **Mathematical Derivation:**
   - Imputed missing `Quantity` via $\frac{\text{Total Spent}}{\text{Price Per Unit}}$[cite: 2].
   - Calculated missing `Total Spent` via $\text{Quantity} \times \text{Price Per Unit}$[cite: 2].
5. **Data Type Casting:** Cast `Quantity` to `int64`, prices/spending to `float64`, and transaction dates to `datetime64[ns]`[cite: 1, 2].
6. **Categorical Standardisation:** Filled remaining unidentifiable entries in `Item`, `Payment Method`, and `Location` with `"Unknown"`[cite: 2].

---

## 📊 Before vs. After Cleaning Metrics

| Column Name | Raw Data Type | Raw Missing / Corrupted Count | Cleaned Data Type | Cleaned Nulls |
| :--- | :--- | :--- | :--- | :--- |
| **Transaction ID** | `object` | 0 | `object` | 0 |
| **Item** | `object` | 969 (333 null + 636 flags) | `object` | 0 |
| **Quantity** | `object` | 479 (138 null + 341 flags) | `int64` | 0 |
| **Price Per Unit** | `object` | 533 (179 null + 354 flags) | `float64` | 0 |
| **Total Spent** | `object` | 502 (173 null + 329 flags) | `float64` | 0 |
| **Payment Method** | `object` | 3,178 (2,579 null + 599 flags) | `object` | 0 |
| **Location** | `object` | 3,961 (3,265 null + 696 flags) | `object` | 0 |
| **Transaction Date** | `object` | 460 (159 null + 301 flags) | `datetime64[ns]` | 0 |

---

## 📂 Folder Contents

```text
Task-1-Data-Cleaning-Preparation/
├── README.md                 # Documentation of Task 1 cleaning procedures
├── Clean_Cafe_Sales.ipynb    # Google Colab / Jupyter Notebook walkthrough
├── clean_cafe_sales.py       # Standalone automated Python cleaning pipeline
├── dirty_cafe_sales.csv      # Original raw dataset with errors
└── cleaned_cafe_sales.csv    # Final sanitized dataset ready for analysis