Create or update a client subscription tracker and generate a branded PDF invoice from it: $ARGUMENTS

The toolkit lives at `/Users/harit/AI-Projects/slash-commands/invoicing/invoice.py`.
It has two subcommands. The **workbook is the single source of truth** — `invoice`
only ever READS it, so regenerating can never clobber figures the operator typed.

## Decide which mode

- `$ARGUMENTS` names a **new client** (or no tracker exists yet) → run `init`, then
  tell them what to fill in.
- `$ARGUMENTS` names an **existing client**, or asks to bill/regenerate/re-issue →
  run `invoice` against their existing workbook.
- `$ARGUMENTS` is empty → ask which client, and whether they want a new tracker or
  an invoice from an existing one.

Never run `init` against an existing workbook without `--force`; it refuses by
default precisely because it would wipe entered amounts.

## New client — FIRST decide the billing model

Ask (or infer) **who pays the vendors**, because it picks the tracker shape and
they are not interchangeable:

- **`--mode subscriptions`** (default) — YOU front the vendor costs and pass them
  through. A month cell holds the **amount** billed. (Clarity i2: Hari pays Apollo,
  Instantly, Azure and rebills.)
- **`--mode services`** — the client owns and pays their own vendors, so you bill
  **work**. The sheet gains a **Rate** column; a month cell holds a **quantity**
  (hours/units) and amount = quantity x rate. Leave Rate blank on a row to bill a
  flat amount, so retainers and hourly can share one sheet. (Cyber9 owns and pays
  for its own GovCloud subscription.)

```
python3 /Users/harit/AI-Projects/slash-commands/invoicing/invoice.py init \
  --client "<who you're billing>" \
  --company "<your company>" \
  --mode <subscriptions|services> \
  --out "<directory for the workbook>" \
  --logo "<path to your logo PNG>" \
  [--months 12] [--rows 12] [--start YYYY-MM]
```

Convention: one folder per client under `/Users/harit/OdysseyTech/Clients/<Name>/`.
Company assets (logos, W-9) stay at the OdysseyTech root and are referenced by path.

Then tell them exactly what to fill: the vendor rows in **Subscriptions**, and the
amber cells in **Invoice** (payment due, bill-to address). Amber = still needs a
real figure.

## Generate the invoice

```
python3 /Users/harit/AI-Projects/slash-commands/invoicing/invoice.py invoice \
  --book "<path to the workbook>" [--month Sep-2026] [--out <file.pdf>]
```

Defaults to the month named in the workbook's **Billing month** cell. Pass
`--month` to bill a different period without editing the sheet.

## Always report the "NOT BILLED" line

The script prints `NOT BILLED (blank in the sheet): …` for every vendor whose
amount cell is empty. **Surface that to the user every time** — a blank cell is
the one way to silently under-invoice, so the warning existing is the whole point.
Offer to fill those figures before sending.

## Before telling them it's ready to send

Check and mention any of these that apply:

- **Placeholder bill-to AND from fields.** Empty lines are simply omitted from the
  PDF, so an incomplete address renders as a *plausible-looking* invoice. That cuts
  both ways: a missing `From — street` / `city/state/zip` / `registration` means the
  invoice has no remit-to at all and still looks finished. Check both blocks.
- **`Payment details`.** If it's blank the Payment line doesn't render, so the client
  has no way to pay you. Worth checking every time on a services invoice.
- **Estimated vs verified amounts.** The *Verified* column is there for this. Call
  out anything still marked "Needs check", especially the largest lines.
- **Metered vendors mid-month.** Anything usage-billed (cloud, AI APIs) isn't final
  until the month closes. Recommend issuing after close, or label it clearly.

## Notes

- **Notes / Payment details** (Invoice sheet) render as a bordered block under the
  totals — contractual context like "consumption is billed directly by Microsoft on
  the client's own subscription", partner-pricing caveats, separate-SOW clauses. Both
  are optional; the block is omitted entirely when both are empty.
- **From — contact / email / street / city-state-zip / registration** render under the
  logo. All optional, empty lines dropped.
- Requires `openpyxl` and Chrome/Chromium (headless) for rendering. The PDF is real
  vector text, not a screenshot.
- Layout mirrors the Odyssey Tech invoice: logo left, `INVOICE` right, bill-to and
  meta blocks, dark-header items table, footer. If no logo path resolves, the
  company name renders as a wordmark instead.
- A vendor row whose Status starts with "cancelled" and whose amount is 0 is
  dropped from the invoice but kept in the workbook for history.
- Set `Include zero-value lines` to `no` in the Invoice sheet to hide $0.00 rows.
  Default `yes` — showing them documents the full stack the client is getting.
