# AI, Economic Perceptions, and Consumer Sentiment Ahead of the 2026 Midterms

Aayush Shah (NetID: ajs10171) · NYU · FRE-GY 7871 A, NLP and the Investment Process (Topics in Financial Information Services and Technology) · final project

Question: do AI-measured economic narratives in news and social media help explain why University of Michigan consumer sentiment sits below an economics-only prediction? Analysis cutoff: 13 October 2026. Election: 3 November 2026.

**Main result.** Media tone does not add forecasting value once household and political controls are included. The raw tone-gap correlation (+0.49) is about zero before 2025 and about zero once the White House party is controlled for. See `report/`.

## What is in this repository
| Path | Contents |
|---|---|
| `final_project.ipynb` | The single working notebook. Parts 1 to 5 (baseline, gap model, corpus, labeling and validation, media tests). Run All rebuilds every table and figure from cached data |
| `colab_open_model_labeler_final.ipynb` | Colab notebook that labels the corpus with an open model (vLLM) |
| `LABELING_GUIDE.md` | Coding scheme, frozen before any AI labeling |
| `report/` | 10-page report (`FRE-GY-7871A_report.pdf`) |
| `AI_USE.md` | Disclosure of AI tools used |
| `dashboard/` | `dashboard_standalone.html`: the monitoring dashboard, open in any browser (works offline; data embedded) |
| `data/raw/` | Provided `Perceptual_Bias_Data.xlsx`, Michigan tables (`tbmics.csv`, `tbmiccice.csv`, `tbmpx1px5.csv`), FRED CSVs saved by hand |
| `data/cache/` | Downloads (about 800 MB). Only `manifest.json` (URL, UTC time, SHA-256 of every download) is in git |
| `data/processed/` | Tables, figures, label sheets and keys the notebook writes |
| `requirements.txt` | Python packages |

## Setup (Windows, VS Code)
```powershell
cd FRE-GY-7871A-Final-Project
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
code .
```
Open `final_project.ipynb`, choose the `.venv` kernel, then **Run All**.

## Caching rule
Every download goes through `fetch_cached()`. A file already in `data/cache/` is never downloaded again. To refresh on purpose, set `FORCE_REFRESH = True` in the config cell or delete the file.

## Re-creating the download cache
* **From scratch:** run the notebook with your own `GUARDIAN_API_KEY`. The first run takes over an hour because of the Reddit and GDELT rate limits. `manifest.json` lists what was downloaded.
* **Fast route:** if a `cache.zip` is shared with you, unzip it into `data/cache/`; the notebook then runs end to end without downloading.
* The Guardian key goes in a git-ignored `.env` file (`GUARDIAN_API_KEY=your-key`), or the environment variable of the same name, or alone on one line in git-ignored `guardian_key.txt`. Never commit a key.

## Parts of the notebook
* **Part 1 (Day 1):** economic and survey baseline, point-in-time panel (release-date lags, no look-ahead).
* **Part 2 (Day 2):** economics-only gap model (M1), plus price level (M2), plus expected inflation (M3), expanding-window forecasts from 2015, robustness checks.
* **Part 3 (Day 3):** media corpus. The protocol is written to `data/processed/day3_protocol.json` before any download. Sources: The Guardian and five subreddits. GDELT was collected but excluded from analysis (rate-limited, 135 items). Outputs: `day3_corpus.csv`, `day3_funnel.csv`, `day3_items_per_month.csv/.png`.
* **Part 4 (Day 4):** labeling and validation. 300 stratified items labeled by hand, 100 by a second rater. A dictionary baseline and name-swap tests are included. Label sheets and keys are frozen and never regenerated.
* **Part 5 (Day 5):** monthly media measures, association tests, rolling forecasts, lead-lag, sensitivity, power, salience, causal-design proposal.

### Labeling with an open model (no API credit needed)
`colab_open_model_labeler_final.ipynb` runs an open-weights model on a Colab GPU with the same prompt, fields and allowed values. The final labels used `Qwen/Qwen2.5-14B-Instruct-AWQ` (`QUANT="awq"`, L4 GPU), temperature 0, seed 7, JSON-schema output, about 40 minutes for the 20,214-item corpus.
1. Run notebook section 4.8 (writes the Colab input files).
2. Upload them to a Drive folder `fre7871_labels`, run the Colab notebook.
3. Copy the three `day4_ai_labels_OPEN_*.csv` files back into `data/processed/`.
4. Run sections 4.9 onward.

An OpenAI route also exists in section 4.4 (set `OPENAI_API_KEY` in `.env`; answers are cached and a spending cap applies). The 300-item validation sample was also labeled once by Claude through chat (`day4_ai_labels_EXTERNAL_sample.csv`); that file cannot be re-run by a grader, so the prompt and model are recorded in it.

### Pre-registered rule and recorded amendments
* Original rule: kappa of at least 0.60 on every field (0.70 for tone) and tone offset within ±0.30.
* **Amendment 1:** Reddit collected in weekly windows to avoid truncation.
* **Amendment 2:** format repair of open-model answers marked not-US-economy but with filled fields (28 of 300).
* **Amendment 3 (post hoc, disclosed):** frame (kappa 0.45) and blame (0.54) failed, so only `us_econ`, `topic` and `tone` from the open model are used.

### Statistics notes
* Main significance test is a phase-randomization (surrogate) test, because HAC p-values reject a true null too often for series this persistent (17% in the simulation on the real series, section 5.7). Holm adjustment across 24 specifications.
* Rolling forecasts compare: economics only; economics plus recalibration; plus media tone; plus controls without media (model 2b); plus media and controls. Media's contribution is model 3 against model 2b.
* Controls: Michigan expected inflation (same respondents as the sentiment index, so flagged as a perception control) and a White House party indicator.
* Section 5.3b checks sub-periods and partial associations.

## Day 5 outputs (`data/processed/`)
`day5_media_monthly.csv`, `day5_analysis_table.csv`, `day5_association_primary.csv`, `day5_forecast_comparison.csv`, `day5_leadlag.csv`, `day5_granger_style.csv`, `day5_sensitivity.csv`, `day5_power.csv`, `day5_salience.csv`, `day5_subperiods_partial.csv`, and figures.

## Day 6: dashboard, report, presentation
* `dashboard/dashboard_standalone.html` shows sentiment against the prediction, the gap, media tone, forecast comparison, source composition and missing months, label validity, events and release lags.
* To update after the election: rerun the notebook with refreshed Michigan files and new Guardian and Reddit items, then regenerate the dashboard from the new `data/processed/day5_*.csv` files (the tables are embedded in `dashboard_standalone.html`).
* Every number in the report comes from the notebook outputs in `data/processed/`.

## Prospective update plan
After the 3 November election: refresh Michigan files after the final October release, collect Guardian and Reddit items for October and November, rerun labeling and Part 5, and compare October and November sentiment with the predictions frozen at the cutoff before any refit.

## Limits
One UK outlet and five Reddit communities, no local news or TV, supply of coverage and not exposure, a second rater on 100 items only, factual calibration not measured, few independent months (minimum detectable correlation about 0.6).
