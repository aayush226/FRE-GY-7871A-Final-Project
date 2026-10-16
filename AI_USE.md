# AI Use Disclosure

**Author:** Aayush Shah (NetID: ajs10171) · FRE-GY 7871 A, NLP and the Investment Process

## Tools
- **Claude (Anthropic)**, used as a coding and research assistant in an interactive chat and agent session on claude.ai and Claude Code, across several sessions from 13 to 16 October 2026. Configured as Sonnet 5.5; the model serving a given turn may have differed from the configured one.
- **Qwen2.5-14B-Instruct (AWQ 4-bit, `Qwen/Qwen2.5-14B-Instruct-AWQ`)**, an open-weights model run through vLLM on a Google Colab GPU, used as a measurement tool (not as an assistant). It labeled all 20,214 corpus items for `us_econ`, `topic` and `tone`. Temperature 0, seed 7, prompt hash `7bd722c0`; prompt and settings are in `data/processed/day4_ai_prompt.txt` and `colab_open_model_labeler_final.ipynb`.

## What AI helped with
- Designing the project plan from the brief: the point-in-time economic panel, the gap model, the corpus protocol, the coding scheme, the validation rules and the statistical design.
- Writing and revising code for the notebook and the Colab labeler: cached downloads, the expanding-window models, the label-agreement statistics, the swap test, the persistence-aware (phase-randomization) test, block bootstraps, rolling forecasts, and the power simulation.
- Debugging problems found during runs: slow or rate-limited downloads, the frozen label-sheet mismatch, invalid model answers, and Colab server issues.
- Labeling the 300-item validation sample and the 100-item swap sheet once through a Claude chat session (`day4_ai_labels_EXTERNAL_sample.csv`, `day4_swap_labels_v2_EXTERNAL.csv`). These files cannot be re-run by a grader; the model and prompt are recorded in them.
- Producing the report figures and text, the dashboard, the presentation, and the README from the notebook outputs.
- Helping interpret results, including a methods change after the data showed that the usual HAC test rejected a true null too often (replaced by a surrogate test as the main test).

## What the author did
- Wrote the human labels for the 300-item stratified sample, following `LABELING_GUIDE.md`, which was frozen before any AI labeling. A classmate independently labeled a 100-item subset. The AI had no part in these human labels.
- Obtained the Guardian API key, kept it out of the repository, and ran the notebook and the Colab labeling on his own machine and account.
- Decided to drop GDELT from the analysis, and approved the amendments recorded in the README (including Amendment 3, which was made after seeing the validation result).
- Reviewed the outputs, the report, the dashboard and the slides, and made the final decisions on what to submit.

## Limits of AI use that affect the results
- The Claude labeler and the author's labels were made within the same project, and the open model was validated against the same human sample. The sample was not used to tune anything afterwards.
- Frame and blame labels from the open model failed validation and are not used in any result.
- The report presents a null finding (no detectable media effect beyond controls); the AI was not asked to find a positive result.
