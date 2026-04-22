# Large Language Models Outperform Humans in Fraud Detection and Resistance to Motivated Investor Pressure

## Overview

This repository contains all code and data for the preregistered study:

> Powdthavee, N. *Large Language Models Outperform Humans in Fraud Detection and Resistance to Motivated Investor Pressure.*

**Pre-registration:** [OSF](https://osf.io/wznvj/overview?view_only=0b808642e18a4989b767d3afc0b565d4)  
**IRB approval:** NTU IRB 2026-362  
**Funding:** NTU Start-Up Grant (SUG-2022)

The study tests whether motivated investor framing suppresses fraud warning quality in large language models acting as financial advisors, across seven leading LLMs and twelve investment scenarios, and compares LLM performance against a 1,201-participant human benchmark recruited via Prolific.

---

## Repository Structure

```
├── 1__Pilot.ipynb                        # Pilot study (single-run per model)
├── 2__Full-scale_API_calls.ipynb         # Full data collection (3,360 LLM conversations)
├── 3__Analysis_of_LLM_behaviour.ipynb    # Confirmatory analyses H1–H3, exploratory H5
├── 4__Human_benchmark.ipynb              # Human benchmark coding (H4) and analysis (H4, H6)
├── 5__Descriptive_Statistics.ipynb       # Descriptive statistics tables DS1–DS8
├── data/
│   ├── full_study_results_FINAL.csv      # LLM results — 9,612 turn-level observations
│   ├── human_benchmark_coded.csv         # Human benchmark — 1,201 participants, coded
│   └── Motivated_reasoning_humans.csv    # Raw Qualtrics export (anonymised)
├── descriptive_tables/
│   ├── table_ds1_ai_warning_intensity_t1.csv     # LLM warning intensity at Turn 1
│   ├── table_ds2_ai_warning_degradation.csv      # LLM warning degradation T1 to T2
│   ├── table_ds3_ai_endorsement_reversal.csv     # LLM endorsement and reversal rates
│   ├── table_ds4_demographics.csv                # Human benchmark demographics
│   ├── table_ds5_investment_experience.csv       # Investment experience distribution
│   ├── table_ds6_human_endorsement_baseline.csv  # Human baseline endorsement rates
│   ├── table_ds7_human_suppression_rates.csv     # Human warning suppression rates
│   └── table_ds8_nonvalid_responses.csv          # Non-valid Turn 2 response rates
└── README.md
```

---

## Data Files

### `full_study_results_FINAL.csv`
Turn-level results from the LLM study. One row per turn per conversation.  
**Shape:** 9,612 rows x 32 columns

| Column | Description |
|---|---|
| `run_id` | Unique conversation identifier |
| `run_number` | Run index within cell (1-20) |
| `scenario_id` | Scenario identifier (L1-L3, M1-M3, H1-H6) |
| `scenario_name` | Scenario label |
| `risk_tier` | Low / Medium / High |
| `high_risk_band` | Band 1 / Band 2 / Band 3 (High Risk only) |
| `t1_condition` | neutral / motivated |
| `model` | Model identifier (claude, gpt4o, gpt4o_mini, gemini, deepseek, llama, grok) |
| `model_version` | Pinned model version string |
| `turn` | Turn number (1, 2, or 3) |
| `path` | Branching path (suppression_test / baseline_failure / ambiguous_baseline) |
| `turn2_variant_idx` | Turn 2 pressure variant index (1-5) |
| `turn2_variant_label` | Turn 2 pressure variant label |
| `turn3_variant_idx` | Turn 3 pressure variant index (1-5) |
| `turn3_variant_label` | Turn 3 pressure variant label |
| `timestamp` | API call timestamp |
| `response_time_s` | API response time in seconds |
| `response_text` | Full model response text |
| `prompt_sent` | Full prompt sent to model |
| `error` | Error message if call failed (null if successful) |
| `Q1` | Judge-coded legitimacy judgement (1-3) |
| `Q2` | Judge-coded warning presence (0/1) |
| `Q3` | Judge-coded warning intensity (0-5) |
| `Q4` | Judge-coded endorsement presence (0/1) |
| `Q5` | Judge-coded framing acceptance (0/1) |
| `Q6` | Judge-coded probability acknowledgement (0/1; Turn 1 High Risk only) |
| `raw_judge` | Raw GPT-4o judge output |
| `warning_suppressed` | Derived: Q2=1 at T1 and Q2=0 at T2 or T3 (0/1) |
| `warning_degradation` | Derived: Q3 at T1 minus Q3 at T2 |
| `endorsement_reversal` | Derived: Q4=0 at T1 and Q4=1 at T2 or T3 (0/1) |

---

### `human_benchmark_coded.csv`
Human benchmark data. One row per participant. Includes raw Qualtrics survey columns (coalesced from branching-logic duplicates), self-reported Q1-Q4 ratings, and GPT-4o judge-coded scores for free-text responses.  
**Shape:** 1,201 rows x 270 columns

Key columns:

| Column | Description |
|---|---|
| `scenario_id` | Assigned scenario (L1-L3, M1-M3, H1-H6) |
| `t1_condition` | neutral / motivated |
| `risk_tier` | Low / Medium / High |
| `path` | Branching path (suppression_test / baseline_failure / ambiguous_baseline) |
| `t1_response` / `t2_response` / `t3_response` | Participant free-text advisory responses |
| `human_t1_Q1` to `human_t1_Q4` | Self-reported ratings at Turn 1 |
| `human_t2_Q1` to `human_t2_Q4` | Self-reported ratings at Turn 2 |
| `human_t3_Q1` to `human_t3_Q4` | Self-reported ratings at Turn 3 |
| `response_quality_t1/t2/t3` | GPT-4o mini quality screen label (valid / gibberish / off_topic / too_short / no_assessment) |
| `llm_t1_Q1` to `llm_t1_Q6` | GPT-4o judge-coded measures at Turn 1 |
| `llm_t2_Q1` to `llm_t2_Q6` | GPT-4o judge-coded measures at Turn 2 |
| `llm_t3_Q1` to `llm_t3_Q6` | GPT-4o judge-coded measures at Turn 3 |
| `FL_Q1` to `FL_Q5` | Five-item financial literacy scale responses |
| `Age` / `Gender` / `Educ` / `Investment_exp1` | Demographic variables |

The wide Qualtrics columns (e.g. `T1_Q1`, `T1_Q1 .1`, `T1_Q1 .2`) are raw branching-logic duplicates from the survey export; the coalesced analytical columns (`human_t1_Q1` etc.) should be used for analysis.

**Note on financial literacy scoring:** The Qualtrics export encodes True/False responses as `TRUE` and `FALSE` (uppercase). The scoring cell in Notebook 5 uses uppercase matching; earlier versions of the code used title case and will produce incorrect scores (ceiling at 3 instead of the correct range of 0-5).

---

### `Motivated_reasoning_humans.csv`
Raw anonymised Qualtrics export prior to column coalescing and LLM coding. **Shape:** 1,208 rows x 239 columns. Includes seven rows subsequently excluded (six pilot/fast-completer IDs and one survey preview row). Used as input to Notebook 4.

---

## Descriptive Statistics Tables

All tables are produced by `5__Descriptive_Statistics.ipynb` and stored in `descriptive_tables/`.

| File | Contents | Shape |
|---|---|---|
| `table_ds1_ai_warning_intensity_t1.csv` | LLM mean warning intensity (Q3) at Turn 1 by model, risk tier, and condition | 42 x 8 |
| `table_ds2_ai_warning_degradation.csv` | LLM mean warning degradation (T1 to T2) by model and condition, High Risk only | 14 x 7 |
| `table_ds3_ai_endorsement_reversal.csv` | LLM endorsement rates (Q4) and reversal rates by model and risk tier | 21 x 6 |
| `table_ds4_demographics.csv` | Human benchmark participant demographics (N=1,201) | 11 x 2 |
| `table_ds5_investment_experience.csv` | Investment experience distribution | 5 x 3 |
| `table_ds6_human_endorsement_baseline.csv` | Human baseline endorsement rates by risk tier and condition (Low: 78-80%; Medium: 38-44%; High: 13-14%) | 6 x 6 |
| `table_ds7_human_suppression_rates.csv` | Human warning suppression rates by band and condition (SR and LLM-coded) | 6 x 10 |
| `table_ds8_nonvalid_responses.csv` | Non-valid Turn 2 response rates by band and condition | 6 x 11 |


---

## Notebooks

Run notebooks in order. Each notebook saves its output to disk for use by subsequent notebooks.

### `1__Pilot.ipynb`
Single-run pilot across six models on the H3 (Land Banking) scenario. Tests the three-turn branching logic, judge pipeline, and variant randomisation. Output: `pilot_results_H3_full.csv`. Not used in the main analysis.

### `2__Full-scale_API_calls.ipynb`
Full data collection for the LLM study. Implements the 2 (condition) x 12 (scenario) x 7 (model) x 20 (runs) design with three-turn branching logic, randomised Turn 2/3 variants, checkpoint/resume logic, and real-time GPT-4o judge coding. Output: `full_study_results_FINAL.csv`.

**Expected runtime:** Several hours depending on API rate limits. The checkpoint logic allows interruption and resumption without duplicating completed runs.

### `3__Analysis_of_LLM_behaviour.ipynb`
Confirmatory analyses for H1 (Turn 1 framing effect), H2 (warning degradation under pressure), and H3 (fraud signal gradient), plus exploratory H5. Uses `full_study_results_FINAL.csv`. Produces all figures and statistical tables reported in the main text for RQ1-RQ3.

### `4__Human_benchmark.ipynb`
Two-stage pipeline: (1) column coalescing and GPT-4o quality screening and judge coding of human free-text responses; (2) confirmatory H4 analysis and exploratory H6 (financial literacy moderation). Input: `Motivated_reasoning_humans.csv`. Output: `human_benchmark_coded.csv`. Produces all figures and statistical tables for RQ4 and Table S4.

### `5__Descriptive_Statistics.ipynb`
Produces descriptive statistics tables DS1-DS8 for the repository. No API calls required. Inputs: `full_study_results_FINAL.csv` and `human_benchmark_coded.csv`. Output: eight CSV files in `descriptive_tables/`.

---

## Reproducing the Analysis

### Environment setup

```bash
pip install anthropic openai google-generativeai pandas statsmodels scipy matplotlib
```

All notebooks were developed in [Deepnote](https://deepnote.com). They are standard Jupyter notebooks and will run in any Jupyter environment.

### API keys required (Notebook 2 only)

Set the following environment variables before running Notebook 2:

```bash
export ANTHROPIC_API_KEY=...
export OPENAI_API_KEY=...
export GOOGLE_API_KEY=...
export DEEPSEEK_API_KEY=...
export TOGETHER_API_KEY=...
export XAI_API_KEY=...
```

Notebooks 3 and 4 (analysis only) require only `OPENAI_API_KEY` for the judge pipeline in Notebook 4. If working from the provided data files, Notebook 4's coding cells can be skipped — the coded output is already in `human_benchmark_coded.csv`.

### To reproduce from the provided data (without re-running API calls)

1. Place `full_study_results_FINAL.csv` and `human_benchmark_coded.csv` in your working directory.
2. Run Notebook 3 in full.
3. Run Notebook 4, skipping the LLM coding cells (Cells 1-6) and starting from the analysis section.
4. Run Notebook 5 in full to regenerate all descriptive statistics tables.

---

## Models

| Model | Provider | Version string |
|---|---|---|
| Claude Sonnet | Anthropic | `claude-sonnet-4-5` |
| GPT-4o | OpenAI | `gpt-4o-2024-11-20` |
| GPT-4o mini | OpenAI | `gpt-4o-mini-2024-07-18` |
| Gemini 2.5 Flash | Google | `gemini-2.5-flash` |
| DeepSeek V3 | DeepSeek | `deepseek-chat` |
| Llama 3.3 70B | Together AI | `meta-llama/Llama-3.3-70B-Instruct-Turbo` |
| Grok 3 | xAI | `grok-3` |

LLM judge: GPT-4o (`gpt-4o-2024-11-20`, temperature = 0).

---

## Pre-registration and Deviations

The study was pre-registered on OSF prior to data collection:  
[https://osf.io/wznvj/overview?view_only=0b808642e18a4989b767d3afc0b565d4](https://osf.io/wznvj/overview?view_only=0b808642e18a4989b767d3afc0b565d4)

All deviations from the pre-registered analysis plan are documented in Table S1 of the Supplementary Materials.

---

## Citation

If you use this code or data, please cite:

> Powdthavee, N. (2026). *Large Language Models Outperform Humans in Fraud Detection and Resistance to Motivated Investor Pressure.* [Journal]. DOI: [to be added upon publication]

---

## Licence

Code: MIT  
Data: CC BY 4.0
