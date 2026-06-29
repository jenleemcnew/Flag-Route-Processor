# Flag Route Processor

A browser-based tool for processing flag subscription spreadsheets. Upload an Excel file and get an interactive dashboard showing subscriber status, payment info, and holiday schedules for each route.

## How It Works

1. Open `flag_route_processor.html` in a browser
2. Upload the `.xlsm` subscriptions spreadsheet
3. The processor parses the file and displays a filterable dashboard with:
   - Active subscribers grouped by scout/route
   - Payment status (paid, pending, mailed, pickup)
   - Calculated first and last holiday based on payment date
   - Mismatch warnings when the spreadsheet's holidays don't match the calculated dates
   - Expired subscriber list for win-back outreach

## Holiday Schedule Logic

Subscribers receive flags for 5 consecutive holidays starting from the first holiday **after** their payment date:

| Holiday | 2026 Date |
|---|---|
| Memorial Day | May 25 |
| Flag Day | June 14 |
| July 4th | July 4 |
| Labor Day | September 7 |
| Veterans Day | November 11 |

- Pay before May 25 → Memorial Day through Veterans Day
- Pay May 25–June 13 → Flag Day through Memorial Day '27
- Pay June 14–July 3 → July 4th through Flag Day '27
- Paying **on** a holiday starts the subscription on the **next** holiday

## Spreadsheet Format

The processor expects an Excel file with a sheet containing "sub" in the name and a header row with these columns:

`First Holiday` · `Last Holiday` · `Expiration Year` · `Status` · `Started` · `Check #` · `Paid` · `(R)en/(N)ew` · `Renewed` · `# of Flags` · `No.` · `Street` · `Sub` · `First` · `Last` · `Route` · `Scout` · `Phone` · `Email`

- **New subscribers (N):** payment date comes from the `Started` column, falls back to `Renewed`
- **Renewals (R):** payment date comes from the `Renewed` column, falls back to `Started`
- **Status:** `GOOD` or expiration year `2026`/`2027` = active; `EXPIRED` = shown in win-back list

## Test Data

`test_data.xlsx` contains 12 dummy subscribers covering edge cases: standard renewals, holiday-boundary payments, missing dates, wrong-column dates, pending payments, and expired entries.
