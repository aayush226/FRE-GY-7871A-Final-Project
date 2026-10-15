# Labeling guide (version 1, fixed before any labeling or any AI/dictionary coding)

You label each item using ONLY the headline, standfirst and text excerpt in the sheet. Do not click links, do not look up the source, and ignore who wrote it (the sheet hides outlet and date on purpose). Label what the text *says*, not whether you agree with it.

## Fields (one row per item)

| Column | Allowed values | Rule |
|---|---|---|
| `us_econ` | 1 / 0 | **1** = about US economic conditions or policy that affects Americans' finances (prices, jobs, wages, rates, housing, tariffs, growth, the Fed). **0** = about a non-US economy, markets-only trivia, or not about economic conditions at all (pure ideology, jokes, personal stories with no economic claim). If 0, leave every later column blank. |
| `topic` | `prices_inflation`, `jobs_wages`, `tariffs_trade`, `rates_housing`, `growth_recession`, `other_econ` | The main subject. Pick one. |
| `tone` | -2, -1, 0, 1, 2 | How the item **portrays economic conditions for ordinary Americans**. **-2** very negative (crisis, collapse, hardship). **-1** negative or worrying. **0** neutral, mixed, or no judgement. **+1** positive. **+2** very positive. Judge the portrayal, not the topic: "inflation falls to 2%" is positive. A question headline ("Is a recession coming?") takes the implied stance; if there is none, code 0. |
| `frame` | `decline_crisis`, `cost_burden`, `resilience_recovery`, `risk_uncertainty`, `political_blame`, `neutral_report` | The dominant way the story is told. **decline_crisis** things are getting worse overall. **cost_burden** affordability and living costs hurting households. **resilience_recovery** the economy is holding up or improving. **risk_uncertainty** something bad might happen. **political_blame** the main point is who is responsible. **neutral_report** states data or events without a story. |
| `blame` | `none`, `trump_gop`, `biden_dems`, `fed`, `business`, `foreign`, `other` | Who the text holds responsible for the economic situation. Use `none` unless the text itself assigns responsibility (credit counts too: who is praised or blamed for the outcome). |
| `confidence` | 1, 2, 3 | 1 = guess, 2 = fairly sure, 3 = sure. |
| `note` | free text | Optional. Use it for sarcasm, mixed items or anything the scheme cannot capture. |

## Procedure
1. Label in blocks of 50 with a break between blocks. Write the start time of each block in `note` of its first row.
2. Do not change a label after moving on. If you doubt it, set `confidence` to 1 and add a note.
3. Do NOT look at any AI or dictionary label before you finish. Those are produced afterwards, without seeing yours.
4. Rows with `second_pass = 1` in the key file are labeled again later, from a fresh blank copy, without looking at the first pass.
