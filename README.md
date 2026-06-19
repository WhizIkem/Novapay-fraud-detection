# Novapay-fraud-detection

## Applied data cleaning rules

This repository uses `03_cleaning.ipynb` to clean and normalize the raw transaction dataset while preserving the original source file.

### Raw data handling

- Keep `nova_pay_combined.csv` as the immutable raw source.
- Work from a copy for cleaning and processing to avoid overwriting raw data.
- Identify all missing values and wrong data types

### Data cleaning

- convert timestamp to datetime, coercing invalid values to NaT (Not a Time).
- Convert amouunt_src to numeric float, setting non-numeric values to NaN.
- Develop a conversion exchange rate dict, to fill in the missing values in amount_usd.
- Fill in missing values for fees, ip_country, kyc_tier, device_trust_score,  
- 
### Category normalization

- count negative values in key numeric columns, and keep only values greater than or equal to zero for the key numeric columns
- check for future timestamps in the dataset
- check for location mismatch
- check for unique value counts in categorical columns
- resolve the spacing, spelling, and unknown data errors
- drop na



