## Algorithmic & Data Pipeline Bias in U.S. Immigration Enforcement Data

A two-phase computational audit of U.S. immigration enforcement data.

**Phase 1 (Spatial Audit):** Tests whether public health infrastructure density predicts state-level ICE enforcement intensity. **Result: null.** The hypothesis is rejected.

**Phase 2 (Pipeline Bias Audit):** Audits 713,464 individual ICE arrest records for evidence that the administrative data pipeline through which ICE locates individuals constitutes a source of measurable bias. **Result: yes, substantially and robustly.** Community vs CAP odds ratios range 0.357–0.592 across 7 specifications. All p \< 1e-100.

## Data Sources

**Phase 1:** ICE Enforcement (DDP/FOIA) \+ HRSA FQHC Sites \+ ACS 2022

**Phase 2:** ICE Arrests individual-level, 713,464 records, Oct 2022–Mar 2026

## Setup & Run

```shell
pip install -r requirements.txt

# Phase 1
bash phase1_spatial/run_phase1.sh

# Phase 2 (requires arrests_core.csv from Colab notebook first)
bash phase2_pipeline/run_phase2.sh
```

## Key Results

### Phase 1 \- Null Result

- AAI → Arrests: β=−12.8, p=0.511 \- **null**  
- AAI → Detainers: β=−50.3, p\<0.001 \- **negative** (reversed)  
- 287(g) → Arrests: \+46.9 (p=0.020) \- **dominant predictor**

### Phase 2 \- Pipeline Bias (Community vs CAP Odds Ratios)

| Model | OR | p |
| :---- | :---- | :---- |
| B: Threat \+ Pipeline | 0.376 | \<2e-308 |
| D: \+ Year FE \+ Criminality | 0.474 | \<2e-308 |
| E: \+ AOR Fixed Effects | 0.448 | \<2e-308 |
| Sensitivity (resolved only) | 0.592 | \<1e-100 |
| Sensitivity (+ vol. departure) | 0.357 | \<2e-308 |

### Figure Index (Phase 2\)

| Fig | Content | Addresses |
| :---- | :---- | :---- |
| 1 | Core bias \+ ACTIVE upper bound | Right-censoring concern |
| 2 | Nationality → pipeline | Structural targeting |
| 3 | Within-AOR gap (8 field offices) | Geographic confound |
| 4 | Pipeline → enforcement track | Mechanism |
| 5 | Threat score contamination | Compounding |
| 6 | All 7 models OR forest plot | All debunks |
| 7 | Year × pipeline ratio | Policy responsiveness |
| 8 | Records integrity | Professor feedback |
| 9 | Extended outcome | Voluntary departure |

## Limitations

1. **No causal identification** \- pipeline selection is not random  
2. **Records falsification concern** \- DDP flags reliability issues; 98.3% label consistency is unusual  
3. **Policy confounders** \- 2022–2024 CAP expansion; year FEs partially control  
4. **Unobserved confounders** \- prior orders, legal representation, court backlog  
5. **Selection** \- only arrested individuals in dataset
