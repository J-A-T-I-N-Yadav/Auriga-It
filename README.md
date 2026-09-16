# Common Ground

Common Ground is a browser-based contribution tracker for shared gifts and other equal-split pools. It answers three practical questions in one view:

- How much has been collected?
- What does each person still owe, or get back?
- What is the shortest list of transfers that settles everyone fairly?

The app also imports untidy contribution history. It accepts CSV or plain text rows, normalizes names such as `farah.` and `Farah`, parses values such as `₹1,500`, `1500`, and `1,500`, merges repeated contributions, and reports accepted, merged, unknown, and rejected data.

## Run it

There are no dependencies or build steps. Open [index.html](index.html) directly, or serve the folder:

```bash
npm start
```

This serves the app at `http://localhost:8000`. If that port is already in use, the start command automatically tries the next available port. You can also choose a starting port explicitly:

```bash
PORT=8001 npm start
```

The equivalent direct command is:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Use it

1. Set the pool name, target amount, and comma-separated participant list.
2. Click **Update pool**. The equal share is calculated immediately.
3. Enter payments in the table, or click **Load messy sample** to see the import workflow.
4. Paste rows or choose a `.csv`/`.txt` file, then click **Import & clean**.
5. Review the import report and the resulting balances.
6. Follow the settlement cards at the bottom.

Expected import shape:

```text
Name, Amount
Aarav, ₹1,500
Farah., 2,000
```

The first row is treated as a header when it contains `name` and `amount`, `paid`, or `contribution`. Rows need a name and amount separated by a comma, semicolon, tab, or pipe. Invalid amounts, missing names, malformed rows, and exact duplicate rows are rejected with a reason.

## Calculation model

All displayed calculations are derived from the current pool:

```text
fair share = target amount / participant count
balance = amount paid - fair share
remaining = max(0, target amount - total collected)
```

A negative balance is an amount owed. A positive balance is an amount to receive. Collection status and participant fairness are intentionally separate: a pool can be fully collected while still needing settlement between participants.

For settlement, debtors and creditors are matched in order. Each transfer is the smaller of the remaining debtor amount and remaining creditor amount. This produces a clear, small set of payments without replaying the original payment history.

## Import rules

- Names are trimmed, whitespace is collapsed, trailing periods are removed, and title casing is applied.
- Currency symbols, spaces, and comma grouping are accepted in amounts.
- Amounts are limited to non-negative values with at most two decimal places.
- Repeated normalized names are merged by summing their accepted rows.
- Exact repeated name-and-amount rows are reported as duplicate rows and are not counted twice.
- A valid imported name not in the current pool is reported as **Not in pool** and does not silently create a participant.
- The report shows accepted rows, exact duplicate rows removed, normalized names merged, invalid rows rejected, cleaned total, and unknown names. Duplicate rows are excluded from the calculation but remain traceable by their rejection reason.

## Files

```text
index.html   Application markup
style.css    Responsive visual design
script.js    State, validation, import cleaning, calculations, and settlement
README.md    Setup and usage
Reasoning.md Design decisions and algorithm notes
AI_LOGS.md   Development conversation log
```

## Scope and limitations

The app is intentionally client-side and currently keeps data in memory. Refreshing the page clears the pool. Future extensions could add local storage, exports, edit history, unequal shares, or shared access without changing the balance model.
