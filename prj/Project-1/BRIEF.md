# Mini-project 1 — Service Ticket Data Pipeline

**Applied Programming for AI | Individual assignment | Contribution: 1.0/20 (5%)**

This is the Week 3 milestone in the [course schedule](../../rsc/Schedule.md). Submit through your own GitHub repository as described at the end of this brief. The submission deadline, collaboration and AI-assistance conditions will be announced by the lecturer.

### Getting started

Open `project_1_student.ipynb` from this folder or the repository root. In Colab, upload the supplied `service_tickets.csv` using the Files panel. Keep generated files in `outputs/` and submit those files together with your completed notebook and report. The October 2026 dates below are dataset validation rules, not deadlines.


## Problem

A fictional service desk wants a trustworthy descriptive table of support tickets before any future AI modelling. Produce an auditable cleaning process. You are not asked to train a model or use scikit-learn.

The supplied `service_tickets.csv` is synthetic and contains no actual people or service records. It was generated for this exercise (seed 71). One row is a ticket observation; repeated identifiers require investigation.

| Field | Meaning |
|---|---|
| ticket_id | Ticket identifier, format T followed by exactly three digits |
| opened_on | Opening date, required to be in October 2026 |
| channel | email, phone or web |
| priority | low, medium or high |
| handling_minutes | Nonnegative duration, in minutes |
| satisfaction | Optional integer rating from 1 to 5 |

## Required procedure

1. Keep the raw CSV unchanged. Report schema, raw row count, missingness and exact duplicates.
2. Remove only exact duplicate rows. Among the remaining rows, flag every occurrence of an identifier repeated with conflicting data. Do not silently keep the first conflicting row.
3. Reject rows with missing/malformed IDs, conflicting IDs, or missing, unparseable or out-of-October dates. Parse `opened_on` as dates and require 1 to 31 October 2026 inclusive; `pd.to_datetime(..., errors="coerce")` can help identify unparseable values. Keep rejected source rows and a `rejection_reason` field. If several reasons apply, record all of them. Count each rejected row once in row balance.
4. Normalize channel and priority by stripping spaces and lowercasing. Replace missing or unrecognized values with `unknown`; retain separate Boolean flags `channel_unknown` and `priority_unknown`.
5. Parse handling minutes numerically. Zero and fractional nonnegative durations are valid. Treat missing, unparseable, non-finite or negative values as missing, flag `handling_imputed`, and fill from the median of valid durations in the retained table. Fail with a clear error if no valid durations remain; do not invent a zero fallback.
6. Parse satisfaction numerically. Replace missing, unparseable, non-finite, fractional or out-of-range ratings with missing values (only integers from 1 to 5 are valid); **do not impute ratings**. Add `satisfaction_unavailable`. This prevents invented customer opinions.
7. Create `handling_zscore` using the retained/imputed durations, population standard deviation (`ddof=0`) and a safe denominator of 1 for a constant column. This is a descriptive output: for future modelling, learned statistics must instead be fitted on training data only.
8. Build a separate feature table with `ticket_id`, `handling_zscore` and the four indicator columns `channel_email`, `channel_phone`, `channel_web`, `channel_unknown`. Each row must have exactly one active indicator. Do not include the satisfaction rating as an input feature in this exercise.
9. Verify the outputs, save them and confirm they can be read back. Provide a concise explanation of the rules and two limitations of the resulting data.

### Implementation guidance

Distinguish values already missing in the source from present but invalid values. Apply field-specific rules: fractional durations can be valid, whereas fractional satisfaction ratings are invalid. Missing values alone do not always require rejecting the row; follow step 3 for rejection and steps 4-6 for repair.

Remove exact row copies before checking IDs with `duplicated(subset=['ticket_id'], keep=False)`. Combine rejection conditions into one Boolean mask so a row with several reasons is counted once. Preserve rejected source values and record every applicable reason. Estimate the imputation median only from valid durations in retained rows.

## Deliverables

- Completed `project_1_student.ipynb` or an equivalent documented Python script using NumPy/Pandas.
- `clean_tickets.csv`: retained, transformed data, original identifier and required flags.
- `rejected_tickets.csv`: rejected raw rows and reasons.
- `ticket_features.csv`: feature table defined above.
- `audit.json`: raw count, exact duplicates removed, rejected count, retained count, counts of unknown categories, imputed durations and unavailable ratings, and the duration fill value, mean and population standard deviation.
- `REPORT.md`: approximately 100–500 words on diagnosis, rules, checks, interpretation and limitations. Include run instructions and any assistance used. **English** or **Portuguese** is acceptable. The report is part of the assessment.

The clean table may contain missing satisfaction values by design. Do not use a blanket assertion that all columns must be complete.

## Acceptance checks

- Raw count = exact duplicates removed + rejected rows + retained rows.
- Retained IDs are nonempty, well formed and unique.
- Dates are valid October dates; durations are finite and nonnegative.
- Known categorical values or `unknown` only.
- Observed satisfaction values are integers between 1 and 5.
- Feature IDs align exactly with clean-table IDs; one channel indicator per row.
- Standardized duration has mean near zero, and standard deviation near one if duration varies. For a constant duration column, it is all zero.
- The source table is not mutated; all required files can be read back.

## Small worked acceptance case

Two valid tickets have durations 10 and missing, and channels email and web. After filling with the observed median, durations are `[10, 10]`; the second ticket alone is marked imputed; standardized durations are `[0, 0]`. Neither ticket is rejected. This example illustrates a contract, not the complete solution to the supplied file.

Also test an exact duplicate (remove only the extra copy), conflicting rows with the same ID (reject both), and missing, unparseable and out-of-October dates (reject those rows). A row with multiple rejection reasons must be counted once, with all reasons recorded. Verify that zero and fractional nonnegative durations are valid, while fractional satisfaction ratings become missing without imputation. A table with no valid retained duration must raise a clear error. Do not hard-code the supplied dataset's row counts or IDs into your function.

## Rubric

| Criterion | Weight |
|---|---:|
| Diagnosis, observation unit and distinction between invalid and missing | 20% |
| Correct and justified transformations and rejection policy | 40% |
| Checks, audit trail and small edge cases | 25% |
| Clear execution and explanation | 15% |

The rubric sums to 100%; its result is scaled to at most 1 course-grade point under the course schedule. A polished report cannot compensate for an incorrect or non-executable pipeline. The current approved course assessment rules take precedence.

## Individual oral defence

Each student must complete a **5-minute individual oral defence**, using the code submitted in their GitHub repository. **No slides are required.** Have the repository and execution environment ready before your turn.

| Time | Activity |
|---|---|
| 1 minute | Run the code and show the main outputs. |
| 2 minutes | Explain a data-preparation decision selected by the lecturer, referring to the relevant code. |
| 2 minutes | Answer a question about the implementation or make a small change requested by the lecturer. |

The defence confirms your understanding of the submitted work and clarifies questions arising from the repository review. It contributes evidence to the **existing rubric criteria**, including correctness, justification, checks, and clarity of explanation. It does not introduce a separate assessment weight: Mini-project 1 remains worth **1.0/20 (5%)**.

## Submission via GitHub

1. Create a repository under your own GitHub account for this individual assignment (for example, `programming-ai-mini-project-1`).
2. Commit and push your completed notebook or Python script, the unchanged source dataset, `REPORT.md`, and all required output files listed under **Deliverables**. Keep generated results in `outputs/`.
3. Include a `README.md` identifying you by name and student number and explaining the dependencies and how to run your code from a fresh clone.
4. Add the lecturer's GitHub account, **[tgvp](https://github.com/tgvp)**, as a **repository collaborator**, sending an invitation with access to the code. Sharing a repository link alone does not replace this invitation.
5. Submit the repository URL through the channel announced by the submission deadline. Before submitting, verify that your latest changes have been pushed and that the collaborator invitation has been sent.

**The repository must contain the actual executable code and required deliverables; screenshots or a link to a Colab session alone are not sufficient.**
