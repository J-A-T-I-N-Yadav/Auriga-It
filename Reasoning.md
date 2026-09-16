# Reasoning Behind the Solution

## Problem interpretation

The organiser needs both a pool-level view and a person-level view. `Collected` answers whether the gift budget is covered; each participant's net balance answers whether that person has paid their fair share. Those are different states and must remain visible separately.

The second requirement adds a data-cleaning boundary before calculation. Raw contribution rows are not trusted directly. They are parsed, normalized, validated, de-duplicated, merged, and only then applied to known participants.

## Data model

The in-memory model is deliberately small:

```text
pool = {
  name,
  target,
  participants: [{ name, paid }]
}
```

Fair share is derived from `target / participants.length`; it is not stored, so changing the pool target or participant list cannot leave a stale share behind.

## Import pipeline

1. Split the pasted text or uploaded file into non-empty lines.
2. Skip a recognizable `Name, Amount` header.
3. Split each row on comma, semicolon, tab, or pipe.
4. Normalize the name by trimming, collapsing whitespace, removing a trailing period, and applying title case.
5. Normalize the amount by removing currency symbols, spaces, and grouping commas. Accept only non-negative numeric values with up to two decimals.
6. Reject malformed rows with a line number and reason.
7. De-duplicate exact pairs of normalized name and amount so a repeated copy of the same row is not double counted. The row remains visible as rejected with the reason `Exact duplicate row`.
8. Sum remaining rows by normalized name. This is the merge step for legitimate multiple payments from the same person.
9. Apply totals only to names already in the pool. Unknown valid names are reported rather than silently changing the participant list.

This makes the import explainable. The organiser can see what was accepted, which rows were de-duplicated, which names were merged, and why an invalid row did not affect the result.

## Balances and collection

For each participant:

```text
balance = paid - fair share
```

- `balance < 0`: the person owes `abs(balance)`.
- `balance = 0`: the person is settled.
- `balance > 0`: the person should receive the surplus.

The total collected is the sum of all accepted payments. The progress bar is capped visually at 100%, while the individual table still exposes overpayment and over-collection instead of hiding it.

## Settlement algorithm

Create two ordered lists:

- debtors with `fair share - paid > 0`
- creditors with `paid - fair share > 0`

Match the first debtor and first creditor. Transfer the smaller remaining amount, reduce both balances, and advance whichever reaches zero. Continue until one list is exhausted. Since the total of positive and negative balances is equal apart from rounding, this settles the net position using straightforward, understandable transfers.

The algorithm deliberately settles net balances rather than trying to reconstruct who originally covered whom. That keeps the result short and fair.

## Design decisions

- A static HTML/CSS/JavaScript implementation keeps the tool runnable without accounts, a server, or package installation.
- The summary is above the detailed table because the organiser's first question is whether the pool is covered.
- Payment inputs are inline so small corrections do not require a separate form.
- The import report stays next to the import control, where its decisions are easiest to audit.
- The visual language uses a warm paper background, green for fair/healthy states, coral for action, and monospace labels for amounts and operational metadata.
- `Intl.NumberFormat` formats INR for display; values are kept as numbers rounded to paise precision at the parser boundary.

## Edge cases

- An empty participant list is blocked before calculation.
- Duplicate participant names are removed when the pool is updated.
- A one-person pool receives the full target as its fair share.
- Zero contributions are valid and remain visible as owed.
- A fully collected but uneven pool still produces settlements.
- Unknown imported names are reported and do not mutate the pool.
- Invalid, negative, or over-precision amounts are rejected.
