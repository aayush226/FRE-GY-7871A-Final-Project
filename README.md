# FRE-GY-7871A Final Project: AI, Economic Perceptions, and Consumer Sentiment (2026 Midterms)

One working notebook: `final_project.ipynb`. Each project day adds a numbered part.

## Setup (Windows, VS Code)
```powershell
cd final_project
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
code .
```
Open `final_project.ipynb`, choose the `.venv` kernel, then **Run All**.

## Folder layout
| Path | Contents | In git? |
|---|---|---|
| `data/raw/` | Provided `Perceptual_Bias_Data.xlsx`, Michigan tables (`tbmics.csv`, `tbmiccice.csv`, `tbmpx1px5.csv`), and any FRED CSVs you save by hand | yes |
| `data/cache/` | Downloaded files + `manifest.json` (URL, UTC time, SHA-256) | yes (keeps results reproducible) |
| `data/processed/` | Clean tables and figures the notebook writes | yes |

## Caching rule
Every download goes through `fetch_cached()`. A file already in `data/cache/` is **never** downloaded again. To refresh on purpose, set `FORCE_REFRESH = True` in the config cell (or delete the file).

## Status
- Part 1 (Day 1): economic and survey baseline, point-in-time panel. See "Open items" at the end of the notebook.

## Day 3: media corpus
* Guardian API key: put `GUARDIAN_API_KEY=your-key` in a `.env` file in the project folder (git-ignored), or set the environment variable `GUARDIAN_API_KEY` (Windows: `setx GUARDIAN_API_KEY "your-key"`, then restart VS Code) **or** put the key alone on one line in `guardian_key.txt` (git-ignored). Never commit a key.
* All media downloads are cached in `data/cache/` (one file per month/window). Re-running resumes where it stopped.
* Protocol is written to `data/processed/day3_protocol.json` before any download.
* Outputs: `day3_corpus.csv`, `day3_funnel.csv`, `day3_items_per_month.csv/.png`.

## Re-creating the download cache
`data/cache/` is not in git (about 800 MB). `data/cache/manifest.json` lists every download (URL, UTC time, SHA-256).
* **Fast route:** unzip `cache.zip` (shared separately, link below) into `data/cache/`. The notebook then runs end to end without downloading.
* **From scratch:** run the notebook with your own `GUARDIAN_API_KEY`. The first run takes over an hour because of the Reddit and GDELT rate limits.
* Cache zip link: _(add here after uploading)_
