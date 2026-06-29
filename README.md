# Flag Route Processor

A browser-based tool for processing flag subscription spreadsheets. Upload an Excel file and get an interactive dashboard showing subscriber status, payment info, holiday schedules, and optimized driving routes for each scout.

## How It Works

1. Open `flag_route_processor.html` in a browser
2. Upload your spreadsheet (.xlsm, .xlsx, or .csv)
3. Map your columns — the processor auto-detects common column names and remembers your mapping for next time
4. The processor displays a filterable dashboard with:
   - Active subscribers grouped by scout/route
   - Payment status (paid, pending, mailed, pickup)
   - Calculated first and last holiday based on payment date
   - Mismatch warnings when the spreadsheet's holidays don't match the calculated dates
   - Expired subscriber list for win-back outreach

## Column Mapping

Every troop keeps their spreadsheet differently. On first upload, the processor shows a mapping screen where you match your columns to the required fields (first name, last name, address, scout, payment info, etc.). It auto-matches common column names and saves your mapping to localStorage so you only do it once per spreadsheet format.

## Google Maps Route Planning

When you filter the dashboard to a single scout, a route planning toolbar appears:

- **Open Route in Google Maps** — builds an optimized driving route for all pending flag stops (up to 10 at a time per Google Maps limits)
- **Flag posting checkboxes** — tap to mark each address as posted; checked stops are skipped in the next route
- **Progress bar** — shows how many flags are posted vs. remaining for the current holiday
- **Batch workflow** — for routes with more than 10 stops, post the first batch, mark them done, then open Maps again for the next batch
- **Auto-reset** — checkboxes clear automatically after each holiday passes (with a 2-day buffer)

All flag posting state is stored in localStorage per scout, per holiday.

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

The processor works with any spreadsheet layout. On first upload, you map your columns to these fields:

**Required:** First Name, Last Name, Street Number, Street Name, Subdivision, Route, Scout, New/Renewal, Started Date, Renewed Date, Check #, Amount Paid

**Optional:** Status, Expiration Year, First Holiday, Last Holiday, # of Flags, Phone, Email, Notes

- **New subscribers (N):** payment date comes from the Started column, falls back to Renewed
- **Renewals (R):** payment date comes from the Renewed column, falls back to Started
- **Status:** `GOOD` or expiration year `2026`/`2027` = active; `EXPIRED` = shown in win-back list
- If no Status or Expiration Year columns are mapped, all rows with a name are treated as active

## Privacy

The spreadsheet file never leaves your device. All processing happens locally in the browser. Mapping profiles and flag posting state are stored in localStorage.

## Test Data

`test_data.xlsx` contains 12 dummy subscribers covering edge cases: standard renewals, holiday-boundary payments, missing dates, wrong-column dates, pending payments, and expired entries.
