# The ICEberg Effect
## Arrested by the Algorithm: Pipeline Bias in U.S. Immigration Enforcement Data

**Sciences Po · Decoding Bias in AI · April 2026**
Apostolos Kritikos · Weronika Nitecka · Paul · Rahma

---

## Overview

A two-phase computational audit of U.S. immigration enforcement data.

**Phase 1 (Spatial Audit):** Tests whether public health infrastructure density predicts state-level ICE enforcement intensity. **Result: null.** The hypothesis is rejected.

**Phase 2 (Pipeline Bias Audit):** Audits 713,464 individual ICE arrest records for evidence that the administrative data pipeline through which ICE locates individuals constitutes a source of measurable bias. **Result: yes, substantially and robustly.** Community vs CAP odds ratios range 0.357–0.592 across 7 specifications. All p < 1e-100.

---

## Repository Structure

```
iceberg-effect/
├── shared/config.py                        # Paths, constants, palette
├── phase1_spatial/
│   ├── run_phase1.sh
│   └── src/
│       ├── 01_get_acs.py                   # ACS state controls
│       ├── 02_clean_hrsa.py                # HRSA FQHC density
│       ├── 03_clean_ice_state.py           # ICE → state level
│       ├── 04_build_aai.py                 # Administrative Accessibility Index
│       ├── 05_model_enforcement.py         # OLS + permutation (999 iter)
│       └── 06_make_figures_phase1.py       # 6 figures
├── phase2_pipeline/
│   ├── run_phase2.sh
│   └── src/
│       ├── 01_classify_pipelines.py        # Pipeline taxonomy
│       ├── 02_core_bias_analysis.py        # Cross-tab: Claims 1-3
│       ├── 02b_regression_chain.py         # Models A-D (professor feedback)
│       ├── 03_threat_score_and_trend.py    # Threat contamination + trend
│       ├── 04b_make_figures_regression.py  # OR plots
│       ├── 05_robustness_suite.py          # 7 models + 5 robustness tests
│       └── 06_make_all_figures_final.py    # All 9 final figures
├── notebooks/00_preprocess_raw_ddp.py      # Colab: xlsx → csv
├── data/{raw,interim,processed}/
├── results/{phase1,phase2}/
├── figures/{phase1,phase2}/
└── docs/                                   # GitHub Pages site
```

---

## Data Sources

**Phase 1:** ICE Enforcement (DDP/FOIA) + HRSA FQHC Sites + ACS 2022

**Phase 2:** ICE Arrests individual-level, 713,464 records, Oct 2022–Mar 2026

Cite as: *"Government data provided by ICE in response to a FOIA request, processed by the Deportation Data Project."*

---

## Setup & Run

```bash
pip install -r requirements.txt

# Phase 1
bash phase1_spatial/run_phase1.sh

# Phase 2 (requires arrests_core.csv from Colab notebook first)
bash phase2_pipeline/run_phase2.sh
```

---

## Key Results

### Phase 1 — Null Result
- AAI → Arrests: β=−12.8, p=0.511 — **null**
- AAI → Detainers: β=−50.3, p<0.001 — **negative** (reversed)
- 287(g) → Arrests: +46.9 (p=0.020) — **dominant predictor**

### Phase 2 — Pipeline Bias (Community vs CAP Odds Ratios)

| Model | OR | p |
|-------|----|---|
| B: Threat + Pipeline | 0.376 | <2e-308 |
| D: + Year FE + Criminality | 0.474 | <2e-308 |
| E: + AOR Fixed Effects | 0.448 | <2e-308 |
| Sensitivity (resolved only) | 0.592 | <1e-100 |
| Sensitivity (+ vol. departure) | 0.357 | <2e-308 |

### Figure Index (Phase 2)

| Fig | Content | Addresses |
|-----|---------|-----------|
| 1 | Core bias + ACTIVE upper bound | Right-censoring concern |
| 2 | Nationality → pipeline | Structural targeting |
| 3 | Within-AOR gap (8 field offices) | Geographic confound |
| 4 | Pipeline → enforcement track | Mechanism |
| 5 | Threat score contamination | Compounding |
| 6 | All 7 models OR forest plot | All debunks |
| 7 | Year × pipeline ratio | Policy responsiveness |
| 8 | Records integrity | Professor feedback |
| 9 | Extended outcome | Voluntary departure |

---

## Critical Limitations

1. **No causal identification** — pipeline selection is not random
2. **Records falsification concern** — DDP flags reliability issues; 98.3% label consistency is unusual
3. **Policy confounders** — 2022–2024 CAP expansion; year FEs partially control
4. **Unobserved confounders** — prior orders, legal representation, court backlog
5. **Selection** — only arrested individuals in dataset

---

## References

Gardner & Kohli (2009). *The C.A.P. Effect.* Warren Institute UC Berkeley.
Golash-Boza (2015). *Deported.* NYU Press.
Eubanks (2018). *Automating Inequality.* St. Martin's Press.
Crawford (2021). *Atlas of AI.* Yale UP.
Obermeyer et al. (2019). Racial bias in algorithm. *Science* 366(6464).
Friedler et al. (2019). Fairness-enhancing interventions. *FAccT*.
Scott (1998). *Seeing Like a State.* Yale UP.

---

## Citation

```
Kritikos, A., Nitecka, W., et al. (2026). The ICEberg Effect: Arrested by the Algorithm.
Sciences Po · Decoding Bias in AI. Data: Deportation Data Project (FOIA).
```

Code: MIT · Data: DDP terms of use
