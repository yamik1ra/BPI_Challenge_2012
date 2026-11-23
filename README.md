# Loan Application Process Analysis (BPI Challenge 2012)

Process mining on a public event log of a loan application process: where cases end, where time is lost, and which manual tasks are candidates for automation.

Author: [Your name] · [Month Year]

## Overview

The BPI Challenge 2012 event log records loan applications at a financial institution: 13,087 cases and 262,200 events. This project reconstructs the process (AS-IS), measures waiting time and manual workload, and proposes an improved process (TO-BE).

## Key findings

| Finding | Value |
|---|---|
| Cases declined immediately, with no manual work logged | 3,429 (26.2%) |
| Final outcomes | 58.3% declined, 21.4% cancelled, 17.2% approved |
| Median time to approval | 14.5 days |
| Logged hands-on work on an approved case | about 91 minutes (about 0.4% of elapsed time) |
| Working hours spent on cases that were not approved | about 54% |
| Largest shares of working hours | completion 30.4%, validation 29.7%, follow-up calls 30.7% |
| Cases reaching the completion step that repeat it | 60% |
| Consecutive follow-up calls | 13,521, median 1.9 days apart |

## Proposed improvements (TO-BE)

1. Scheduled reminders (email, SMS or portal) after the offer is sent; staff call only overdue or at-risk cases.
2. Completeness check at submission (form rules, mandatory-document list); staff handle exceptions only.
3. Automatic routing to validation once conditions are met, with a checklist for the human decision.
4. Optional: an early-warning model for cancellation risk.

The effect of these proposals is a design estimate, not a measured result.

## Repository contents

| File | Description |
|---|---|
| `[notebook name].ipynb` | Data loading, cleaning and analysis |
| `BPI2012_process_AS-IS_TO-BE.drawio` | AS-IS and TO-BE swimlane diagrams (Draw.io, two pages) |
| `sop.md` or PDF | Standard operating procedure reconstructed from the log |
| `slides.pdf` | Presentation of the analysis |

The event log is not included. Download it from 4TU.ResearchData ([link]) and check its licence and citation terms before reuse.

## How to reproduce

```bash
pip install pm4py pandas
```

```python
import pm4py

log = pm4py.read_xes("BPI_Challenge_2012.xes.gz")
df = pm4py.convert_to_dataframe(log)
print(df.shape)  # (262200, 7)
```

Place `BPI_Challenge_2012.xes.gz` next to the notebook, then run the cells in order.

## Method notes

- Activities are grouped by prefix: `A_` (application status), `O_` (offer status), `W_` (manual work items). Activity names are in Dutch; the English labels used here are my translation.
- "Working hours" are the time between the START and COMPLETE events of a work item. They measure how long an item stayed open, not pure effort. Records with non-positive duration (under 0.01%) were removed and the top 1% of durations were capped per activity.
- Waiting times are medians between consecutive COMPLETE events within a case.
- Resource `112` is assumed to be a system account. Confirm this against the dataset description.

## Limitations

- The log cannot explain why customers reply slowly.
- Work done outside the logged system is invisible.
- Results come from a single organisation's historical data.

## Tools

Python, pandas, pm4py, Draw.io.

## Data source

BPI Challenge 2012 event log, 4TU.ResearchData. Cite the original dataset as its page requires.
