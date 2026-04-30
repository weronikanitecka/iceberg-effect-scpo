# Data Sources

## Phase 2: Individual-level arrest records
File: arrests_core.csv (173MB — gitignored, not in repo due to size)

Source: Deportation Data Project (FOIA) — https://deportationdata.org/data.html

Download: arrests-latest.xlsx → run notebooks/00_preprocess_raw_ddp.py in Colab

Rows: 713,464 individual ICE arrest records, Oct 2022–Mar 2026

Columns used: apprehension_method, apprehension_criminality, case_threat_level,
              case_status, citizenship_country, gender, apprehension_aor,
              apprehension_date, case_criminality, birth_year


## Phase 1: State-aggregated enforcement
Files: ice_arrests_state.csv, ice_detainers_state.csv (included)

Source: Deportation Data Project (FOIA), aggregated from arrests-latest.xlsx
        and detainers-latest.xlsx via Colab notebook

Rows: ~50 states each | Columns: state, ice_arrests / ice_detainers

## Phase 1: HRSA health centers (full raw download)
File: hrsa_health_center_service_delivery.csv (included, 13MB)

Source: HRSA Data Warehouse — https://data.hrsa.gov/data/download

Search: Health Center Service Delivery and Look-Alike Sites → CSV

Rows: 18,884 individual FQHC sites with state, address, coordinates

Note: 02_clean_hrsa.py automatically detects the state column and aggregates

## ACS 2022 demographics

Source: U.S. Census Bureau ACS 5-year estimates

URL: https://api.census.gov

Note: Hardcoded in 01_get_acs.py as fallback; live Census API also supported
