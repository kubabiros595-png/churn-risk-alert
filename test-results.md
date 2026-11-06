# Test results

All data is fictional. I set the expected score and alert decision for every case before running the test.

## Evaluation workflow

`Churn Risk Alert Eval` reads `eval_cases.csv`, builds test accounts with dates relative to the day of the run and sends them through the same `Score accounts` and `Apply dedup rule` nodes as the daily workflow. Test accounts carry their own past alerts, so the test never reads or writes the real alerts table and never calls the AI.

| Run | Cases | Correct alert decision | Correct score | False alerts | Missed alerts |
| --- | --- | --- | --- | --- | --- |
| 2026-11-05 21:25 | 37 | 37 | 37 | 0 | 0 |
| 2026-11-05 21:31 | 37 | 37 | 37 | 0 | 0 |
| 2026-11-06 20:07 | 38 | 38 | 38 | 0 | 0 |

The third run added case D14 for the failed-draft rule.

## What the cases cover

| Group | Cases | Examples |
| --- | --- | --- |
| Thresholds | C01 to C24 | usage -14% and -15%, -29% and -30%; 30% active users; 2 and 3 tickets; contact after 30, 31, 45 and 46 days; renewal in 61, 60, 31, 30 and 0 days |
| Data quality | C21, C23 | renewal date already passed; missing active users value |
| Deduplication | D01 to D14 | same-day second run; exactly 7 and 8 days ago; score up by 1 and by 2; score fell; `rejected` and `edit` do not block; a failed draft does not block |

## Two runs in a row

| Run | New alerts | AI calls | Time |
| --- | --- | --- | --- |
| First | 4 | 4 | about 20 seconds |
| Second, right after | 0 | 0 | about 2 seconds |

On the second run all four accounts were marked `Skipped: open alert with score …`.

## Review form

A test submission set one alert to `approved`, one to `edit`, one to `rejected` and left one pending. All four statuses and notes were saved. A following run created new alerts only for the `edit` and `rejected` accounts, as the rule requires.

## Token use

| Version | Input tokens | Output tokens |
| --- | --- | --- |
| Before the changes, 4 drafts | 2603 | 3987 |
| After the changes, 4 drafts | 2686 | 3241 |

Most output tokens are the model's internal reasoning, which the n8n Anthropic node cannot turn off. The largest saving is the deduplication itself: a day with no new alerts makes no AI calls.

## Limits

- 38 cases test the rules as written. They do not show whether the thresholds predict real churn.
- Email quality was reviewed by hand on 4 drafts per run, not scored automatically.
