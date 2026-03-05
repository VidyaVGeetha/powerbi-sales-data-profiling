# AdventureWorks Sales Data Profiling using Power BI

## Project Overview

This project demonstrates **data cleaning, transformation, and profiling using Microsoft Power Query in Power BI**.

The dataset comes from the **AdventureWorks sales system**, containing transactional sales records, product information, and reseller data.

The objective of this project is to perform **data validation and profiling before analysis**, which is a critical step in real-world data analytics workflows.

---

# Business Problem

The management team wants to analyze sales performance across different regions and products.

Before performing analysis, the raw dataset must be validated to ensure:

• Data types are correct
• Currency values are converted properly
• No missing or erroneous values exist
• Data distribution and quality are verified

This project focuses on **preparing high-quality analytical data using Power Query best practices**.

---

# Dataset

The dataset contains three tables:

| Table    | Description                     |
| -------- | ------------------------------- |
| Sales    | Transaction-level sales records |
| Product  | Product details                 |
| Reseller | Reseller information            |

---

# Data Preparation Steps

## 1 Data Import

Imported CSV datasets using **Power BI Power Query**.

```
Get Data → Text/CSV
```

Files imported:

* Sales.csv
* Product.csv
* Reseller.csv

---

# 2 Data Type Validation

Validated column data types:

| Column            | Data Type      |
| ----------------- | -------------- |
| SalesOrderNumber  | Text           |
| OrderDate         | Date           |
| ProductKey        | Whole Number   |
| ResellerKey       | Whole Number   |
| EmployeeKey       | Whole Number   |
| SalesTerritoryKey | Whole Number   |
| Quantity          | Whole Number   |
| Unit Price        | Decimal Number |
| Sales             | Decimal Number |
| Cost              | Decimal Number |

---

# 3 Filtering Relevant Business Period

Filtered the dataset to include **sales records after January 1, 2017**.

```
OrderDate > 01/01/2017
```

This ensures the analysis focuses on **recent business performance**.

---

# 4 Currency Data Cleaning

The columns **Unit Price** and **Sales** were initially stored as text because they contained:

* Currency symbol `$`
* Thousands separators `,`

These were converted to **Decimal Numbers** using locale transformation.

```
Change Type → Using Locale
Locale: English (United States)
```

---

# 5 Data Quality Validation

Power Query Data Profiling tools were used to validate the dataset.

Enabled:

```
View → Column Quality
View → Column Distribution
View → Column Profile
```

Checks performed:

• Null values
• Error values
• Distinct and unique values
• Value distribution
• Minimum / Maximum values

---

# Key Findings

## Sales Table

| Metric                | Value  |
| --------------------- | ------ |
| Total Records         | 57,851 |
| Errors                | 0      |
| Empty Values          | 0      |
| Distinct Sales Orders | 3,616  |

---

## Product Table

| Metric                 | Value |
| ---------------------- | ----- |
| Total Products         | 397   |
| Distinct Product Names | 295   |
| Unique Products        | 218   |

---

## Unit Price Statistics

| Metric  | Value   |
| ------- | ------- |
| Minimum | 1.33    |
| Maximum | 2146.96 |
| Average | 446.38  |

---

# Power Query (M Code)

```
let
Source = Csv.Document(File.Contents("Sales.csv"),
[Delimiter="	", Columns=10, Encoding=1252]),

PromotedHeaders = Table.PromoteHeaders(Source),

ChangedTypes =
Table.TransformColumnTypes(
PromotedHeaders,
{
{"SalesOrderNumber", type text},
{"OrderDate", type date},
{"ProductKey", Int64.Type},
{"ResellerKey", Int64.Type},
{"EmployeeKey", Int64.Type},
{"SalesTerritoryKey", Int64.Type},
{"Quantity", Int64.Type},
{"Unit Price", type text},
{"Sales", type text},
{"Cost", type text}
}
),

FilteredSales =
Table.SelectRows(
ChangedTypes,
each [OrderDate] > #date(2017,1,1)
),

UnitPriceConverted =
Table.TransformColumnTypes(
FilteredSales,
{{"Unit Price", type number}},
"en-US"
),

SalesConverted =
Table.TransformColumnTypes(
UnitPriceConverted,
{{"Sales", type number}},
"en-US"
)

in
SalesConverted
```

---

# Tools Used

• Microsoft Power BI
• Power Query
• Data Profiling Tools

---

# Key Skills Demonstrated

• Data Cleaning
• Data Type Validation
• Currency Conversion
• Data Profiling
• Data Quality Checks
• Power Query M Language

---

# Author

**Vidya Vishnuvihar Geetha**
