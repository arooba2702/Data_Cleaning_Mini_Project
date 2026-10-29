# Data Cleaning with Python (pandas): Messy Sales Dataset

A beginner-friendly data cleaning project that takes a deliberately messy
500-row sales dataset and turns it into a clean, analysis-ready table.
Every cleaning step is applied in code and documented in a log.

**Author:** Arooba Waseem
**Tools:** Python, pandas, NumPy, Google Colab, Excel
**Repository:** https://github.com/arooba2702/Data_Cleaning_Mini_Project

---

## Project objective

Real-world data is rarely clean. This project demonstrates a complete
cleaning workflow: inspect the data, find quality problems, fix them step
by step, validate the result, and document what was done and why — without
inventing values that cannot be justified.

## Results at a glance (verified)

| Metric | Raw data | Cleaned data |
|---|---|---|
| Rows | 500 | 466 |
| Missing cells | 308 | 25 (order dates only) |
| Repeated Order IDs | 30 | 0 |

The 25 remaining blanks are in `Order Date`. They are left empty on purpose,
because a date cannot be reliably invented.

Verified checks on the cleaned data:

- No duplicate Order IDs
- Age range 18–65, quantity range 1–5
- Phones in `0XXX-XXXXXXX` format, or `Not provided` where invalid
- No invalid emails (invalid ones became `Not provided`, never guessed)
- Amount agrees with quantity × unit price (no difference over 1.00)
- Total cleaned sales: **316,910.42**
- Largest product by sales: Laptop (**144,470.78**), followed by Phone (74,203.84)

## Dataset

`Raw_Sales_Data.xlsx` (sheet `Sales_Data`) and the same data as
`Raw_Sales_Data.csv`: 500 rows × 13 columns of synthetic sales orders.
The data is synthetic and was generated for practice. It contains no real
personal information.

| Column | Description |
|---|---|
| Order ID | Unique order number |
| Customer Name, Email, Phone, Age | Customer details |
| City | Customer city |
| Product, Quantity, Unit Price, Amount | Order details |
| Order Date | Date of order |
| Payment Method, Status | Payment and order status |

## Data quality problems found

- **Duplicates:** exact copies and near-copies (differing only in case or spacing)
- **Missing values:** gaps in every column, plus rows with only an Order ID
- **Text formatting:** extra spaces, mixed upper/lower case
- **Inconsistent categories:** `LHR`, `Lahor`, `lahore` for Lahore; `CC` vs `Credit Card`; `Canceled` vs `Cancelled`
- **Invalid emails and phones:** missing `@`, missing domain, too-short numbers, many formats
- **Dates:** five different formats and impossible dates (`31/02/2026`)
- **Numbers stored as text:** `$1,200.00`, `Rs. 1,200`, `1200 USD`, `two`
- **Impossible values:** ages such as 250 and -5, quantities such as 0 and 100, unit prices 100× too high

## Cleaning steps

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

Why this order: standardize first, remove duplicates second, handle outliers
third, fill missing values last. Otherwise near-duplicates would be missed.

### Missing value strategy

1. Calculate from related columns (`amount = quantity × unit_price`)
2. Use the median (numbers) or the most common value (quantity), by product where possible
3. Use labels `Unknown` / `Not provided` for text fields
4. Leave `Order Date` blank

## Assumptions

- Dates in `dd/mm/yyyy` format are read day-first.
- Ages outside 16-100 and quantities outside 1-20 are invalid.
- A unit price more than 3× its product median is treated as an outlier
  (in this dataset, 7 such prices; they were set to missing and filled later).
- Invalid emails and phone numbers are set to missing, not guessed.
- `Rs.` amounts are treated as plain numbers (no currency conversion).

## Repository contents (actual filenames)

```
├── README.md
├── Data_Cleaning_Project.ipynb    # Colab notebook with all steps and explanations
├── Raw_Sales_Data.xlsx            # Raw (messy) dataset, sheet Sales_Data
├── Raw_Sales_Data.csv             # Same data as CSV
├── Cleaned_Dataset.xlsx           # Sheets: Cleaned_Data, Raw_Data, Cleaning_Log
└── Cleaned_Data.csv               # Cleaned output
```

## How to run

**On Google Colab (recommended)**

1. Open `Data_Cleaning_Project.ipynb` in Google Colab.
2. Run all cells (**Runtime → Run all**).
3. The notebook loads the CSV directly from the GitHub raw link, so no
   file upload is needed. At the end, use the Colab download step to save
   `Cleaned_Dataset.xlsx` and `Cleaned_Data.csv`.

**Locally**

```bash
pip install -r requirements.txt
jupyter notebook Data_Cleaning_Project.ipynb
```

Note: two lines in the notebook are Colab-only (`google.colab` data-table
formatting and file download) and the `display()` calls need Jupyter/IPython.
When run as a plain Python script, replace `display(...)` with `print(...)`
and skip the Colab-only lines. The cleaning logic itself is plain pandas.

## Skills demonstrated

Data inspection · missing value handling · duplicate detection · text
standardization · regex · date parsing · outlier handling · data validation
· documentation

## Possible next steps

- Add summary visualizations (sales by city, product, month)
- Build a SQL version of the same cleaning pipeline
- Create a dashboard from the cleaned data

## Limitations

- Order dates that were missing or impossible (25 rows) were not invented.
- Names, emails and phones that were missing or invalid were labelled
  `Unknown` / `Not provided`, never guessed.
- Filled ages, quantities and prices use medians/modes from the data itself,
  so they are reasonable estimates for analysis, not original records.
- Results are for this synthetic 500-row dataset only.

