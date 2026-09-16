# Reasoning Behind the Solution

## Problem interpretation

The organiser needs two views at once: whether the pool target is covered and whether each person has paid their fair share. Those are different states, so the dashboard shows collection progress while the participant table shows individual balances.

The messy import is treated as a cleaning pipeline before calculation. Raw rows are parsed, normalized, validated, de-duplicated, merged, and only then applied to known participants.

## Data model

```text
pool = {
  target,
  participants: [{ name, paid }]
}
```

The equal share is derived from `target / participant count`; it is not stored separately. That prevents stale shares when the target or participant list changes.

## Import pipeline

1. Split the text or uploaded file into non-empty lines.
2. Skip a recognizable name/amount header.
3. Split rows on comma, semicolon, tab, or pipe.
4. Normalize names by trimming, collapsing whitespace, removing trailing periods, and applying title case.
5. Normalize amounts by removing currency symbols, spaces, and comma grouping. Accept only non-negative numbers with up to two decimals.
6. Reject malformed rows with a line number and reason.
7. Remove exact duplicate pairs of normalized name and amount so copied rows are not counted twice.
8. Sum remaining rows by normalized name. This merges legitimate multiple payments from one person, including spelling variants such as `Farah` and `farah.`.
9. Apply totals only to participants already in the pool. Unknown valid names are reported instead of silently changing the participant list.

The report makes every decision visible: accepted rows, exact duplicates removed, merged names, invalid rows, cleaned total, and unknown names.

## Balances

```text
balance = paid - fair share
```

- Negative balance: the participant owes the absolute amount.
- Zero balance: the participant is settled.
- Positive balance: the participant should receive the surplus.

The total collected is the sum of accepted payments. The progress bar is capped visually at 100%, while the table still shows overpayment and over-collection.

## Settlement algorithm

Create debtors from participants below their fair share and creditors from participants above it. Match the first debtor and creditor, transfer the smaller remaining amount, reduce both, and advance the one that reaches zero. Continue until all net positions are covered.

This settles net balances instead of replaying who originally paid for whom, producing a shorter and easier-to-follow plan.

## Design decisions

- Plain HTML, CSS, and JavaScript keep the application easy to run in a browser without a backend or dependency installation.
- The summary is placed before the detailed table because collection status is the organiser's first question.
- Payment inputs update on `change`, not every keystroke, so values like `1000` can be entered without losing focus.
- A separate number-input stylesheet removes browser spinner controls while retaining numeric validation.
- The import report is displayed beside the import controls so cleanup decisions are auditable.
- `Intl.NumberFormat` provides INR display formatting, while parsed values are rounded to paise precision.
- Organizer workflows such as persistence, participant search, CSV/JSON export, backup restore, audit download, clipboard sharing, printing, drag-and-drop import, and keyboard shortcuts live in JavaScript because they are behavior, not presentation.

## Edge cases

- Empty participant lists and invalid targets are blocked.
- Duplicate participant names are removed when the pool is updated.
- A one-person pool receives the whole target as its share.
- Zero contributions are valid and remain visible as owed.
- A fully collected but uneven pool still produces settlements.
- Unknown imported names do not mutate the pool.
- Negative, malformed, and over-precision amounts are rejected.
