# 🧹 E-Commerce Data Quality Improvement using Excel & Power Query

![Excel](https://img.shields.io/badge/Tool-Microsoft%20Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Power Query](https://img.shields.io/badge/Tool-Power%20Query-FFCA28?style=for-the-badge&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Level](https://img.shields.io/badge/Project%20Level-Beginner%20to%20Intermediate-blue?style=for-the-badge)

---

## 📌 Project Overview

This project focuses on cleaning and standardizing a raw **e-commerce order dataset** using **Microsoft Excel** and **Power Query**, turning it into an analysis-ready dataset suitable for **SQL queries, Power BI dashboards, and reporting**.

Raw business data is rarely analysis-ready. It often contains inconsistent text casing, missing values, and unverified formats — all of which can lead to **incorrect KPIs, broken joins, and misleading dashboards**. This project demonstrates how those issues are identified, validated, and resolved using a structured Power Query workflow.

**Business problem solved:** Before this dataset can power any sales dashboard or be queried in SQL, every field has to be trustworthy — correct data types, no blank coupon values, no duplicate or invalid orders, and consistent text formatting across categorical fields. This project delivers that trustworthy, validated dataset.

---

## 🎯 Objectives

- Audit every column in the raw dataset for data quality issues
- Identify and remove invalid/duplicate order records
- Standardize categorical text fields (e.g., Coupon Codes) into consistent Proper Case formatting
- Handle missing values in `CouponCode` using business-appropriate replacement logic
- Validate data types for identifiers, numeric fields, and dates
- Verify numeric integrity (Quantity, UnitPrice, ItemsInCart, TotalPrice)
- Produce a clean, analysis-ready dataset for SQL and Power BI use

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Dataset Information](#-dataset-information)
- [Project Workflow](#-project-workflow)
- [Complete Column Reference](#-complete-column-reference)
- [Phase 1 — Understanding the Dataset](#-phase-1--understanding-the-dataset)
- [Phase 2 — Data Cleaning](#-phase-2--data-cleaning)
- [Phase 3 — Data Quality Validation](#-phase-3--data-quality-validation)
- [Data Quality Summary](#-data-quality-summary)
- [Before vs After Cleaning Summary](#-before-vs-after-cleaning-summary)
- [Business Value](#-business-value)
- [Project Structure](#-project-structure)
- [How to Use This Project](#-how-to-use-this-project)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 📂 Dataset Information

| Detail | Original Dataset | Cleaned Dataset |
|---|---|---|
| File name | `Dataset_for_Data_Analytics.xlsx` | `cleaned_sales_data.xlsx` |
| Number of records | 1,200 | 1,195 |
| Number of columns | 14 | 14 |
| File format | Excel (.xlsx) | Excel (.xlsx) |
| Sheet name | Sheet1 | cleaned_sales_data |
| Date range | Jan 2023 – Jun 2025 | Jan 2023 – Jun 2025 |

---

## 🔄 Project Workflow

```
Raw Dataset
   ↓
Excel
   ↓
Power Query
   ↓
Data Cleaning
   ↓
Validation
   ↓
Clean Dataset
   ↓
Analysis Ready Dataset
```

---

## 📖 Complete Column Reference

| Column | Data Type | Business Meaning |
|---|---|---|
| `OrderID` | Text | Unique identifier for each order (e.g., ORD200000). Primary key for joins in SQL/Power BI. |
| `Date` | Date | Date the order was placed. Used for trend analysis and time-based reporting. |
| `CustomerID` | Text | Unique identifier for the customer who placed the order (e.g., C72649). |
| `Product` | Text | Product purchased (Monitor, Phone, Tablet, Chair, Printer, Laptop, Desk). |
| `Quantity` | Whole Number | Number of units of the product purchased in the order. |
| `UnitPrice` | Decimal | Price per unit of the product, in currency units. |
| `ShippingAddress` | Text | Delivery address associated with the order. |
| `PaymentMethod` | Text | Mode of payment used (Debit Card, Credit Card, Online, Cash, Gift Card). |
| `OrderStatus` | Text | Current status of the order (Shipped, Delivered, Returned, Cancelled, Pending). |
| `TrackingNumber` | Text | Unique shipment tracking code (e.g., TRK37947903). Kept as text to preserve format. |
| `ItemsInCart` | Whole Number | Total number of items in the customer's cart at checkout. |
| `CouponCode` | Text | Discount/coupon applied to the order (Save10, Freeship, Winter15, or No Coupon). |
| `ReferralSource` | Text | Channel through which the customer discovered the store (Instagram, Google, Email, Facebook, Referral). |
| `TotalPrice` | Decimal | Final order value, equal to Quantity × UnitPrice. |

---

## 🔍 Phase 1 — Understanding the Dataset

Before any transformation, every column was reviewed individually to understand its role in the dataset:

**Identifiers**
- `OrderID` — unique per order, used to detect duplicate transactions
- `CustomerID` — links multiple orders to the same customer
- `TrackingNumber` — unique shipment reference per order

**Categorical Fields**
- `Product`, `PaymentMethod`, `OrderStatus`, `ReferralSource`, `CouponCode` — fixed sets of values used for segmentation and filtering in dashboards

**Numerical Fields**
- `Quantity`, `UnitPrice`, `ItemsInCart`, `TotalPrice` — drive revenue calculations and cart-behavior metrics

**Date Fields**
- `Date` — order placement date, used for monthly/quarterly trend reporting

This review confirmed the dataset structure was already largely consistent, with the data quality issues concentrated in the `CouponCode` field and a small number of order records.

---

## 🛠️ Phase 2 — Data Cleaning

### Step 1: Column-by-Column Review

**Business Context:** Before applying any transformation, each of the 14 columns was inspected for data type correctness, blank values, inconsistent casing, and outliers.

**Transformation Applied:** Manual and Power Query-assisted review of all columns using column profiling (Column Distribution / Column Quality view).

**Reason:** Identifies exactly where cleaning effort is needed instead of applying blanket transformations that could damage already-correct data.

**Expected Outcome:** A targeted cleaning plan focused on the columns that actually required correction — primarily `CouponCode` and overall record-level validation.

---

### Step 2: Removing Invalid Order Records

**Business Context:** A small number of order records did not meet data integrity standards for inclusion in the analysis-ready dataset.

**Transformation Applied:** 5 order records were removed during the cleaning process, reducing the dataset from 1,200 rows to 1,195 rows.

**Reason:** Records that fail data integrity checks can distort revenue totals, order counts, and customer-level metrics if left in the dataset.

**Expected Outcome:** A dataset where every remaining row represents a valid, analysis-ready order record.

---

### Step 3: Checking Duplicate Order IDs

**Business Context:** `OrderID` is the primary key for this dataset — duplicates would mean the same order is counted more than once in revenue and order-count metrics.

**Transformation Applied:** Verified `OrderID` for duplicate values across all 1,195 records using Power Query's duplicate detection.

**Reason:** Duplicate primary keys break SQL joins and inflate KPIs such as total revenue and total orders.

**Expected Outcome:** Confirmed — zero duplicate `OrderID` values in the cleaned dataset.

---

### Step 4: Applying Trim to Text Columns

**Business Context:** Raw data exports frequently contain hidden leading/trailing spaces that look identical to clean values but break exact-match filters, joins, and groupings.

**Transformation Applied:** `Text.Trim()` applied across all text-based columns (`OrderID`, `CustomerID`, `Product`, `ShippingAddress`, `PaymentMethod`, `OrderStatus`, `TrackingNumber`, `CouponCode`, `ReferralSource`).

**Reason:** Even invisible whitespace can cause `"Email"` and `"Email "` to be treated as two different categories in a Pivot Table or SQL `GROUP BY`.

**Expected Outcome:** All text fields contain clean values with no leading or trailing whitespace, ensuring accurate grouping and filtering.

---

### Step 5: Applying Proper Case Formatting — CouponCode

**Business Context:** The raw `CouponCode` column used inconsistent, all-uppercase formatting (`SAVE10`, `FREESHIP`, `WINTER15`), which is not ideal for reporting labels or dashboard legends.

**Transformation Applied:** `Text.Proper()` applied to the `CouponCode` column, converting:
- `SAVE10` → `Save10`
- `FREESHIP` → `Freeship`
- `WINTER15` → `Winter15`

**Reason:** Proper Case formatting presents better in Power BI visuals and reports, and ensures the coupon naming convention is consistent across the dataset.

**Expected Outcome:** All `CouponCode` values follow a consistent Proper Case format.

---

### Step 6: Handling Null Values in CouponCode

**Business Context:** 309 out of 1,200 records had a blank `CouponCode`, representing orders where no coupon was applied. A blank value is ambiguous — it could mean "no coupon used" or "data not captured."

**Transformation Applied:** All null/blank `CouponCode` values were replaced with `"No Coupon"` using Power Query's **Replace Values** function.

**Reason:** Blank cells are excluded or misread in Pivot Tables, COUNTIFS, and SQL aggregations. Explicitly labeling these orders as `"No Coupon"` makes coupon-usage analysis (e.g., "% of orders with no coupon") accurate and complete.

**Expected Outcome:** Zero null values in `CouponCode`. The cleaned dataset contains 308 records labeled `"No Coupon"`, alongside `Save10` (283), `Freeship` (312), and `Winter15` (292).

---

### Step 7: Assigning Correct Data Types

**Business Context:** Power Query and Excel sometimes auto-detect incorrect data types on import (e.g., treating identifiers as numbers), which causes formatting and calculation errors downstream.

**Transformation Applied:** Explicit data type assignment for every column:
- `OrderID`, `CustomerID`, `TrackingNumber`, `Product`, `ShippingAddress`, `PaymentMethod`, `OrderStatus`, `CouponCode`, `ReferralSource` → **Text**
- `Date` → **Date**
- `Quantity`, `ItemsInCart` → **Whole Number**
- `UnitPrice`, `TotalPrice` → **Decimal Number**

**Reason:** Correct data types are critical for accurate SQL imports, Power BI relationships, and avoiding silent calculation errors (e.g., an `OrderID` like `200045` being treated as a number and losing its `ORD` prefix or leading zeros).

**Expected Outcome:** Every column carries the correct, consistent data type throughout the dataset.

---

### Step 8: Validating TrackingNumber as Text

**Business Context:** `TrackingNumber` values follow the format `TRK` + 8 digits (e.g., `TRK37947903`). If misclassified as a number, the `TRK` prefix would be lost or the field would throw conversion errors.

**Transformation Applied:** Confirmed and locked `TrackingNumber` as a **Text** data type; verified all 1,195 values follow the consistent 11-character `TRK` + digits format.

**Reason:** Tracking numbers are identifiers, not numeric quantities — they should never be summed, averaged, or have their formatting altered.

**Expected Outcome:** All `TrackingNumber` values retained their original format with no data loss.

---

### Step 9: Validating ItemsInCart Values

**Business Context:** `ItemsInCart` represents the number of items a customer had in their cart at checkout. Logically, this value should always be a positive whole number and should be consistent with `Quantity`.

**Transformation Applied:** Verified `ItemsInCart` contains only positive whole numbers (range: 1–10), with no zero, negative, or non-numeric values.

**Reason:** Invalid cart values would distort cart-size analysis and any "average items per cart" metric used in customer behavior reporting.

**Expected Outcome:** All `ItemsInCart` values confirmed valid and within a logical range (1–10).

---

### Step 10: Checking TotalPrice for Nulls, Errors, and Negative Values

**Business Context:** `TotalPrice` is the core revenue field for this dataset. Any null, error, or negative value here would directly distort revenue reporting.

**Transformation Applied:** Verified `TotalPrice`:
- Contains no null values
- Contains no error values
- Contains no negative or zero values (range: 11.39 – 3,456.40)
- Is consistent with `Quantity × UnitPrice` for every record

**Reason:** Revenue figures are the most business-critical metric in an e-commerce dataset — even a few corrupted values can significantly skew total revenue and average order value KPIs.

**Expected Outcome:** `TotalPrice` confirmed accurate and consistent with `Quantity × UnitPrice` across all 1,195 records.

---

### Step 11: Standardizing Text Formatting Across Categorical Fields

**Business Context:** Consistent text formatting across categorical columns ensures clean grouping in Pivot Tables, Power BI visuals, and SQL `GROUP BY` queries.

**Transformation Applied:** Verified consistent capitalization across `Product`, `PaymentMethod`, `OrderStatus`, and `ReferralSource` (e.g., `Monitor`, `Debit Card`, `Shipped`, `Instagram`), in addition to the Proper Case standardization applied to `CouponCode`.

**Reason:** Inconsistent casing (e.g., `shipped` vs `Shipped`) would cause the same category to appear as multiple separate groups in any aggregation.

**Expected Outcome:** All categorical fields use consistent, standardized text casing throughout the dataset.

---

## ✅ Phase 3 — Data Quality Validation

| Validation Area | Check Performed | Result |
|---|---|---|
| Duplicate checking | `OrderID` checked for duplicates across all records | No duplicates found |
| Null checking | All 14 columns checked for blank/null values | `CouponCode` nulls resolved (309 → 0) |
| Data type validation | Every column assigned and verified for correct type | All types confirmed correct |
| ID validation | `OrderID`, `CustomerID`, `TrackingNumber` format checked | All identifiers valid and correctly formatted |
| Numeric validation | `Quantity`, `UnitPrice`, `ItemsInCart`, `TotalPrice` checked for nulls, errors, negatives | All numeric fields valid |
| Text standardization | Trim + Proper Case applied where required | All text fields standardized |
| Final validation | Full dataset reviewed before Close & Load | Dataset confirmed analysis-ready |

---

## 📊 Data Quality Summary

| Issue | Action Taken | Result |
|---|---|---|
| Invalid order records present | Removed 5 records that failed data integrity checks | 1,200 → 1,195 valid records |
| `CouponCode` had 309 blank values | Replaced nulls with `"No Coupon"` | 0 null values remaining |
| `CouponCode` used inconsistent uppercase formatting | Applied Proper Case (`Text.Proper`) | Consistent `Save10`, `Freeship`, `Winter15`, `No Coupon` |
| Possible hidden whitespace in text fields | Applied `Text.Trim` to all text columns | No leading/trailing spaces remain |
| Unverified data types on import | Explicitly assigned correct types to all 14 columns | All columns have correct, consistent types |
| `TrackingNumber` risk of numeric misclassification | Validated and locked as Text | Format preserved (`TRK` + 8 digits) |
| Revenue field (`TotalPrice`) integrity unverified | Checked for nulls, errors, negatives, and formula consistency | Confirmed accurate, matches Quantity × UnitPrice |

---

## 🔁 Before vs After Cleaning Summary

| Metric | Before Cleaning | After Cleaning |
|---|---|---|
| Total records | 1,200 | 1,195 |
| Total columns | 14 | 14 |
| Null values in `CouponCode` | 309 | 0 |
| `CouponCode` formatting | Inconsistent (UPPERCASE) | Standardized (Proper Case) |
| Duplicate `OrderID` values | 0 | 0 |
| Data type accuracy | Unverified | Fully validated |
| Analysis readiness | Not ready | Ready for SQL / Power BI |

---

## 💼 Business Value

A cleaned dataset directly improves:

- **Accuracy** — revenue and order metrics reflect only valid, verified records
- **Consistency** — standardized coupon and category formatting prevents duplicate groupings in reports
- **Reliability** — explicit data types and zero nulls remove a major source of broken formulas and failed imports
- **Dashboard creation** — Power BI visuals can group and filter `CouponCode`, `OrderStatus`, and `Product` without inconsistency errors
- **SQL analysis** — clean identifiers and validated numeric fields allow direct import without pre-processing
- **Decision making** — stakeholders can trust KPIs like total revenue, average order value, and coupon usage rate without manual re-checking

---

## 📁 Project Structure

```
ecommerce-data-cleaning-project/
│
├── data/
│   ├── raw/
│   │   └── Dataset_for_Data_Analytics.xlsx
│   └── cleaned/
│       └── cleaned_sales_data.xlsx
│
├── README.md
└── docs/
    └── data_dictionary.md
```

---

## 🚀 How to Use This Project

1. Download `cleaned_sales_data.xlsx` from the `data/cleaned/` folder
2. Use it directly as a source for Power BI dashboards or SQL database imports
3. Reference the [Complete Column Reference](#-complete-column-reference) section to understand each field before building visuals or queries
4. The raw dataset is included for transparency — compare it against the cleaned version to see the exact transformations applied

---

## 🔮 Future Improvements

- Automate this cleaning workflow with Power Query parameters for recurring data refreshes
- Build a Power BI dashboard on top of the cleaned dataset (revenue trends, coupon usage, referral source performance)
- Extend analysis with SQL queries for customer segmentation and repeat-purchase behavior
- Add data validation rules at the source to reduce future cleaning effort

---

## 👩‍💻 Author

**Nancy Khasa**
Aspiring Data Analyst
Excel | Power Query | SQL | Python | Power BI
