# Data Cleaning Project: Cosmetic Sales Data

## Overview

This project involved cleaning a 265-row raw sales dataset for a fictional aesthetic
clinic chain operating six outlets across Singapore. The raw data contained a range of
realistic data-quality issues: inconsistent text casing, whitespace errors, category
name typos, mixed date formats, inconsistent phone number formats, missing values,
currency text embedded in a numeric column, invalid rating values, malformed emails,
duplicate rows, and blank rows. The goal was to produce a clean, consistent,
analysis-ready dataset while preserving data integrity — never inventing values that
weren't verifiable.

**Tools used:** Google Sheets (formulas: TRIM, XLOOKUP, VLOOKUP, SUBSTITUTE, VALUE, ABS,
Pivot Tables, Named Ranges)

**Result:** 250 clean rows (from 265 raw rows — 12 duplicate rows and 3 blank rows removed)

---

## Step 1: Profiled the Raw Data

Before making any changes, I used **Pivot Tables** to list every unique value in the
Product, Outlet, and Payment Method columns. This gave me a full picture of exactly
which typos, casing inconsistencies, and spelling variants existed in the data before
deciding how to standardise them, rather than guessing.

## Step 2: Removed Duplicate and Blank Rows

Using Google Sheets' built-in duplicate removal (Data > Data cleanup > Remove
duplicates), I identified and removed 12 exact duplicate rows. I also identified and
removed 3 fully blank rows using a helper column with `COUNTA()` to flag empty rows.

**Result:** 265 raw rows → 250 clean rows.

## Step 3: Standardised Category Columns (Product, Outlet, Payment Method)

For each of these three columns, I built a separate **mapping table** pairing every
raw/messy value discovered in Step 1 with its correct standard value (e.g. "Hydrafacial",
"hydra facial", and "Hydra Facial" all mapped to "HydraFacial").

I used a nested lookup formula to pull the raw value from the source data, trim any
whitespace, and then match it against the mapping table:

```
=XLOOKUP(TRIM(XLOOKUP(A2,Raw_data!A:A,Raw_data!E:E)),ProductMap,ProductMap2,"Not Found")
```

**QA catch:** On first pass, the Outlet column still contained trailing and double
spaces even after applying TRIM, and the Product column contained a typo I had
introduced myself in my own mapping table ("Skin Consulatation" instead of
"Skin Consultation"). I caught both issues on review by re-checking the unique value
list after cleaning, corrected the mapping table entry, and re-applied TRIM more
carefully to the Outlet column. This step mattered: it's not enough to clean once and
assume it worked — checking your own output against the same profiling method you used
on the raw data is what catches these kinds of residual errors.

## Step 4: Standardised Phone Numbers

Raw phone numbers appeared in multiple formats (`+65 9953 7568`, `6589259254`,
`9702-3273`, `88245844`). I standardised every number to a single consistent format:
`+65XXXXXXXX`, stripping spaces and dashes and adding the country code where missing.

## Step 5: Cleaned the Amount Column

The raw Amount column mixed plain numbers with currency symbols and text
(`$300`, `SGD 158`, `-108`). I used `SUBSTITUTE()` to strip `$` and `SGD` text,
`VALUE()` to convert the result into a true number, and `ABS()` to correct negative
values, which I treated as data-entry errors rather than genuine refunds — an
assumption I'm noting explicitly here rather than applying silently.

## Step 6: Validated Staff Rating

The valid range for Staff Rating is 1–5. Any value outside this range (a planted "6" in
the raw data) was relabelled "Not in Range" rather than being silently deleted or
guessed at, so the data quality issue remains visible rather than hidden.

## Step 7: Handled Missing Values

Rather than inventing values, I left missing Email, Phone, Order Date, Amount, and
Staff Rating entries blank. Filling these in with guessed values would have
misrepresented the dataset — a blank is honest; a fabricated value is not.

| Column | Missing values (out of 250) |
|---|---|
| Email | 20 |
| Phone | 17 |
| Order Date | 9 |
| Amount | 10 |
| Staff Rating | 12 |

## Result

- **250 clean rows**, down from 265 raw rows (12 duplicates + 3 blank rows removed)
- **6 standardised outlet names**, 0 whitespace issues remaining
- **10 standardised product categories**, 0 typos remaining
- **5 standardised payment methods**
- **0 negative amounts, 0 currency symbols/text** remaining in Amount
- **0 out-of-range Staff Rating values** left unflagged
- Genuine missing values preserved as blank and documented, not fabricated

## What I'd highlight in an interview

The most important part of this project wasn't the formulas — it was the discipline of
re-checking my own cleaned output against the same profiling method I used on the raw
data, which caught a typo I had introduced in my own mapping table and an incomplete
whitespace fix. Real data cleaning isn't a single pass; it's clean, verify, and correct.
