# Data Cleaning with Python (pandas): Messy Sales Dataset

A beginner-friendly data cleaning project that takes a deliberately messy 500-row sales dataset and turns it into a clean, analysis-ready table. Every cleaning step is applied in code and documented in a log.

**Author:** Arooba Waseem
**Tools:** Python, pandas, NumPy, Google Colab, Excel

---

## Project Overview

Real-world data is rarely clean. This project demonstrates a complete cleaning workflow: inspecting the data, finding quality problems, fixing them step by step, validating the result, and documenting what was done and why.

## Results at a Glance

| Metric | Raw data | Cleaned data |
|---|---|---|
| Rows | 500 | 466 |
| Missing cells | 308 | 25 (order dates only) |
| Repeated Order IDs | 30 | 0 |

The 25 remaining blanks are in `Order Date`. They are left empty on purpose, because a date cannot be reliably invented.

## Dataset

`messy_sales_data.xlsx` (sheet `Sales_Data`): 500 rows × 13 columns of synthetic sales orders.

| Column | Description |
|---|---|
| Order ID | Unique order number |
| Customer Name, Email, Phone, Age | Customer details |
| City | Customer city |
| Product, Quantity, Unit Price, Amount | Order details |
| Order Date | Date of order |
| Payment Method, Status | Payment and order status |

The data is synthetic and was generated for practice. It contains no real personal information.

## Data Quality Problems Found

- **Duplicates:** exact copies and near-copies (differing only in case or spacing)
- **Missing values:** gaps in every column, plus rows with only an Order ID
- **Text formatting:** extra spaces, mixed upper/lower case
- **Inconsistent categories:** `LHR`, `Lahor`, `lahore` for Lahore; `CC` vs `Credit Card`; `Canceled` vs `Cancelled`
- **Invalid emails and phones:** missing `@`, missing domain, too-short numbers, many formats
- **Dates:** five different formats and impossible dates (`31/02/2026`)
- **Numbers stored as text:** `$1,200.00`, `Rs. 1,200`, `1200 USD`, `two`
- **Impossible values:** ages such as 250 and -5, quantities such as 0 and 100, unit prices 100× too high

## Cleaning Steps

| # | Step | Method |
|---|---|---|
| 1 | Standardize column names | snake_case |
| 2 | Remove empty rows | `dropna` on all columns except the ID |
| 3 | Clean names | trim, collapse spaces, Title Case |
| 4 | Validate emails | lowercase, regex check, invalid set to missing |
| 5 | Standardize phones | digits only, `+92` converted, format `0XXX-XXXXXXX` |
| 6 | Fix age | convert to numeric, valid range 16-100 |
| 7 | Standardize categories | mapping dictionaries for city, product, payment, status |
| 8 | Fix dates | parse multiple formats (day-first), invalid set to missing |
| 9 | Fix quantity, unit price, amount | words to numbers, strip currency symbols, outlier check |
| 10 | Remove duplicates | by Order ID, after standardizing |
| 11 | Handle missing values | derive from related columns, then median/mode, then labels |
| 12 | Validate and export | assertions, sorted output, Excel and CSV |

Why this order: standardize first, remove duplicates second, handle outliers third, fill missing values last. Otherwise near-duplicates would be missed.

### Missing value strategy
1. Calculate from related columns (`amount = quantity × unit_price`)
2. Use the median (numbers) or the most common value (quantity), by product where possible
3. Use labels `Unknown` / `Not provided` for text fields
4. Leave `Order Date` blank

## Assumptions

- Dates in `dd/mm/yyyy` format are read day-first.
- Ages outside 16-100 and quantities outside 1-20 are invalid.
- A unit price more than 3× its product median is treated as an outlier.
- Invalid emails and phone numbers are set to missing, not guessed.
- `Rs.` amounts are treated as plain numbers (no currency conversion).

## Repository Contents

```
├── README.md
├── PROJECT_DESCRIPTION.md
├── Data_Cleaning_Project.ipynb    # Colab notebook with all steps and explanations
├── messy_sales_data.xlsx          # Raw (messy) dataset
├── messy_sales_data.csv           # Same data as CSV
├── cleaned_dataset.xlsx           # Sheets: Cleaned_Data, Raw_Data, Cleaning_Log
└── cleaned_data.csv               # Cleaned output
```

## How to Run

**On Google Colab**
1. Open `Data_Cleaning_Project.ipynb` in Google Colab.
2. Run all cells (**Runtime → Run all**).
3. When asked, upload `messy_sales_data.xlsx`. (Or change Step 2 to read the CSV from the GitHub raw link.)

**Locally**
```bash
pip install pandas numpy openpyxl
jupyter notebook Data_Cleaning_Project.ipynb
```

## Skills Demonstrated

Data inspection · missing value handling · duplicate detection · text standardization · regex · date parsing · outlier handling · data validation · documentation

## Possible Next Steps

- Add summary visualizations (sales by city, product, month)
- Build a SQL version of the same cleaning pipeline
- Create a dashboard from the cleaned data
