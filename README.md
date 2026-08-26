# Cosmetic Sales Data Cleaning

A data cleaning project on a simulated 265-row sales dataset for a fictional aesthetic
clinic chain with six outlets across Singapore. The raw data was intentionally messy —
duplicate rows, inconsistent formatting, typos, and missing values — to mirror the kind
of real-world data quality issues analysts deal with day to day.

## Objective

Turn an unreliable, inconsistent spreadsheet into a clean, consistent, analysis-ready
dataset — without ever inventing data that couldn't be verified.

## Tools

Google Sheets — Pivot Tables, XLOOKUP, VLOOKUP, TRIM, SUBSTITUTE, VALUE, ABS, Named Ranges

## Files URL and Description

- File Path -> https://drive.google.com/drive/folders/1SSHNM-GTEl88qnUpadcQobJkKX2n0X7e?usp=sharing
- messy_cosmetic_sales_data -> Original raw dataset (before cleaning)
- cleaned_cosmetic_sales_data (cleaned data sheet) -> Final cleaned dataset (after cleaning) 

## Issues Found in the Raw Data

- Inconsistent text casing across Customer Name, Product, Outlet, and Payment Method
- Leading/trailing/extra whitespace in text fields
- Typos and spelling variants in category names (e.g. "Hydrafacial" vs "HydraFacial")
- Five different date formats mixed in a single column
- Four different phone number formats mixed in a single column
- Missing values in Email, Phone, Order Date, Amount, and Staff Rating
- Currency symbols and text mixed into a numeric Amount column (`$150`, `SGD 150`)
- Invalid values (a rating outside the valid 1–5 range)
- Malformed emails
- 12 exact duplicate rows and 3 fully blank rows

## What I Did

1. **Profiled the data first** using Pivot Tables to see every unique value before
   deciding how to clean anything.
2. **Removed duplicates and blank rows** — 265 raw rows → 250 clean rows.
3. **Standardised categories** (Product, Outlet, Payment Method) using mapping tables
   and nested `XLOOKUP`/`TRIM` formulas.
4. **Standardised phone numbers** into a single consistent format.
5. **Cleaned the Amount column** — stripped currency symbols/text, converted to numeric,
   corrected negative values (documented as an explicit assumption, not a silent fix).
6. **Flagged invalid ratings** ("Not in Range") instead of deleting or guessing.
7. **Left genuinely missing values blank** rather than fabricating data — a blank is
   honest, a guessed value is not.

## 🔎 A Real QA Catch (and why it matters)

On my first cleaning pass, two issues slipped through: incomplete whitespace trimming
in the Outlet column. I caught by re-running the
same Pivot Table profiling process on my *cleaned* output and comparing it against what
I expected to see.

**Takeaway:** cleaning data isn't a single pass — verifying your own output the same way
you profiled the raw data is what catches the errors you introduce yourself.

## Results

| Metric | Before | After |
|---|---|---|
| Total rows | 265 | 250 |
| Duplicate rows | 12 | 0 |
| Blank rows | 3 | 0 |
| Outlet name variants | 18 | 6 |
| Product name variants | ~35 | 10 |
| Payment method variants | ~17 | 5 |
| Negative/invalid amounts | present | 0 |
| Out-of-range ratings | unflagged | flagged |

## Download

https://drive.google.com/drive/folders/1SSHNM-GTEl88qnUpadcQobJkKX2n0X7e?usp=sharing to explore interactively.
