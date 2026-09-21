# Task 1: Data Cleaning & Preparation

Part of the **SWYNEX Technologies Data Analytics Internship** program.

---

## 📌 Project Overview
Real-world operational data is rarely structured or clean. This project implements an automated, deterministic cleaning pipeline using Python and Pandas on an uncurated point-of-sale dataset (`dirty_cafe_sales.csv`) containing 10,000 transaction records.

The pipeline addresses common data corruption challenges: sentinel placeholder values, mixed data types, calculated value discrepancies, and high-sparsity categorical fields.

---

## 🔍 Dataset Diagnosis & Flaws Identified

| Issue Category | Description in Raw Dataset | Impact |
| :--- | :--- | :--- |
| **Sentinel Values** | Arbitrary string placeholders (`"ERROR"`, `"UNKNOWN"`) populated across text, numeric, and date fields | Forced numeric and datetime columns into generic Python `object` types |
| **Type Inconsistencies** | Quantities, prices, totals, and timestamps stored as mixed-type strings | Prevented mathematical aggregations and time-series operations |
| **High Categorical Sparsity** | Over 31% missing in `Payment Method` and over 39% missing in `Location` | Incomplete operational reporting |
| **Mathematical Derivation Errors** | Rows with missing `Total Spent` or `Quantity` despite adjacent price metrics | Internal accounting mismatch |

---

## 🛠️ Cleaning Pipeline & Methodology

1. **Sentinel Normalization:** Converted string artifacts (`"ERROR"`, `"UNKNOWN"`) to standard IEEE `NaN` representations.
2. **Deterministic Price Mapping:** Extracted static item prices from clean records to build an item-price lookup table (`Coffee` = $2.0, `Salad` = $5.0, `Tea` = $1.5, etc.) and imputed missing `Price Per Unit` based on the item purchased.
3. **Arithmetic Reconstruction:** 
   - Reconstructed missing `Quantity` values using $\frac{\text{Total Spent}}{\text{Price Per Unit}}$.
   - Calculated missing `Total Spent` figures using $\text{Quantity} \times \text{Price Per Unit}$.
4. **Data Type Standardization:**
   - Cast `Quantity` to discrete integers (`int64`).
   - Cast financial values (`Price Per Unit`, `Total Spent`) to floating points (`float64`).
   - Formatted string timestamps into standardized `datetime64[ns]` objects.
5. **Categorical Imputation:** Labeled missing transactions in `Item`, `Payment Method`, and `Location` as `"Unknown"`.

---

## 📊 Before vs. After Summary

| Column Name | Raw Data Type | Raw Missing / Corrupted | Cleaned Data Type | Cleaned Nulls |
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

## 📂 Repository Structure

```text
SWYNEX-Data-Analysis/
├── README.md
└── Task-1-Data-Cleaning-Preparation/
    ├── README.md
    ├── clean_cafe_sales.py
    ├── dirty_cafe_sales.csv
    └── cleaned_cafe_sales.csv