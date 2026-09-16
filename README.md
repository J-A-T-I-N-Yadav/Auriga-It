# Common Ground

Common Ground is a browser-based contribution tracker for shared gifts and equal-split pools. It shows what has been collected, what each person owes or gets back, and the shortest list of transfers that settles the room.

It also imports messy contribution history: inconsistent names, rupee formats, duplicate rows, repeated contributions, and invalid lines. The cleanup report distinguishes accepted rows, duplicate rows removed, normalized names merged, invalid rows rejected, and names not in the pool.

## Run it

This is a dependency-free HTML, CSS, and JavaScript app. Open [index.html](index.html) directly, or run:

```bash
npm start
```

The app opens at `http://localhost:8000`. If that port is busy, the start command automatically tries the next port. You can also choose one explicitly:

```bash
PORT=8001 npm start
```

## Use it

1. Enter the pool name, target amount, and comma-separated participants.
2. Click **Update pool** to calculate the equal share.
3. Enter payments in the table. Multi-digit values such as `1000` can be typed normally.
4. Click **Load messy sample**, paste rows, or choose a CSV/TXT file.
5. Click **Import & clean** and review the audit report.
6. Follow the settlement transfers at the bottom.

Expected import format:

```text
Name, Amount
Aarav, ₹1,500
Farah., 2,000
```

Names are trimmed, whitespace is collapsed, trailing periods are removed, and title case is applied. Amounts accept currency symbols, spaces, and comma grouping. Exact repeated name-and-amount rows are excluded from totals. Legitimate repeated payments for the same normalized person are added together. Invalid rows show a line number and reason.

## Calculation model

```text
fair share = target amount / participant count
balance = amount paid - fair share
remaining = max(0, target amount - total collected)
```

Negative balances are amounts owed. Positive balances are amounts to receive. Pool collection and participant fairness remain separate: a fully collected pool can still need settlements.

Settlement matches debtors and creditors in order, transferring the smaller remaining amount each time. This produces a clear, compact list without replaying the original payment history.

## Files

```text
index.html          Application markup
style.css           Responsive visual design
number-input.css    Removes browser number spinners
script.js           State, import cleanup, calculations, and settlements
package.json        Static start command
README.md           Setup and usage
Reasoning.md        Design and algorithm notes
AI_LOGS.md          Development log
```

The app keeps data in memory, so refreshing the page resets the current pool.
