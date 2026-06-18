# Data Assessment
# With the Help of Ai
## Dataset scope

- Row count: `11,400`
- Column count: `26`
- Data type: transaction-level remittance / fraud detection dataset
- Key fields:
  - Identifiers: `transaction_id`, `customer_id`, `device_id`
  - Transaction context: `timestamp`, `home_country`, `source_currency`, `dest_currency`, `channel`, `amount_src`, `amount_usd`, `fee`
  - Risk / behavior features: `ip_country`, `location_mismatch`, `ip_risk_score`, `kyc_tier`, `device_trust_score`, `chargeback_history_count`, `risk_score_internal`, `txn_velocity_1h`, `txn_velocity_24h`, `corridor_risk`
  - Target: `is_fraud`

## Imbalance

- `is_fraud` distribution:
  - `0`: `10,403` records
  - `1`: `997` records
- Fraud rate: `8.7%`
- Class ratio: roughly `10:1` non-fraud to fraud
- Risk: moderate class imbalance means accuracy is not sufficient. Use precision/recall, AUC, stratified splits, or class weighting.

## Data quality and bias risks

### Missingness

- Missing values are concentrated in:
  - `amount_usd`: `2.7%`
  - `ip_address`: `2.7%`
  - `ip_country`: `2.6%`
  - `kyc_tier`: `2.6%`
  - `device_trust_score`: `2.6%`
  - `fee`: `2.6%`
  - `timestamp`: `0.25%`
- Risk: non-random missingness may correlate with fraud or geography and bias models if imputed improperly.

### Categorical noise and labeling issues

- `home_country`, `ip_country`, `channel`, and `kyc_tier` contain inconsistent values and misspellings:
  - `US`, ` US`, `CA`, ` CA`, `UK`, ` UK`
  - `mobile`, ` mobile`, `MOBILE`, `mobille`
  - `standard`, `enhanced`, `low`, `standrd`, `enhancd`
- Risk: inconsistent categories will split identical groups and distort model learning.

### Duplicate and reuse concerns

- `transaction_id` duplicates: `200`
- `customer_id` unique ratio: `~11.5%`
- Risk: duplicate transaction IDs may indicate bad data or repeated transactions, while repeat customers can create leakage if not validated carefully.

### Geographic and cohort bias

- Home country concentration:
  - `US`: dominant
  - `UK`: second largest
  - `CA`: third largest
- Smaller groups and `unknown` values are underrepresented.
- Risk: model may generalize poorly outside the main corridors and may underperform for low-frequency geographies.

### Feature source bias

- Risk features such as `risk_score_internal`, `ip_risk_score`, `corridor_risk`, and `kyc_tier` may reflect prior screening policies.
- Risk: using these fields directly can amplify existing operational bias instead of learning independent fraud signals.

## Recommendations

- Normalize and clean categorical variables before modeling.
- Review and resolve duplicate `transaction_id` entries.
- Handle missing values carefully, especially for `ip_country`, `kyc_tier`, and `device_trust_score`.
- Use imbalanced classification techniques and metrics.
- Validate model behavior across top geographies and underrepresented groups.


# By exploring data
## Data Scope
- Row count = "11400"
- Column count = "26"
- Headers = "transaction_id", "customer_id", "timestamp",	"home_country",	"source_currency",	"dest_currency",	"channel",	"amount_src",	"amount_usd",	"fee"	...	"ip_risk_score",	"kyc_tier", "account_age_days",	"device_trust_score",	"chargeback_history_count",	"risk_score_internal",	"txn_velocity_1h",	"txn_velocity_24h",	"corridor_risk",	"is_fraud".

## Imbalance
- Target Header = is_fraud distribution shows 
- 0    0.912544 not fraud
- 1    0.087456 is fraud. Highly imbalanced.

## Bias risks
- timestamp                     29
- amount_usd                   305
- fee                          295
- ip_address                   305
- ip_country                   301
- kyc_tier                     300
- device_trust_score           295

## Quality risks
- timestamp                     object  "should be datetime"
- amount_src                    object  "should be float"
