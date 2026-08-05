# Invoicing toolkit

Client subscription tracker + branded PDF invoice. Driven by the `/invoice`
command (`../commands/invoice.md`), or run directly.

    python3 invoice.py init    --client "Acme Co" --company "Odyssey Tech LLC" \
                               --out ~/Clients/Acme --logo ~/logo.png
    python3 invoice.py invoice --book ~/Clients/Acme/Acme_Co_Subscriptions.xlsx

**The workbook is the source of truth.** `invoice` only reads it, so regenerating
never clobbers entered amounts. A blank amount is never billed and is always
reported on stdout — that warning is the guard against silently under-invoicing.

Generalised from the PedalAI/Clarity i2 build (2026-08). Requires `openpyxl` and
Chrome/Chromium for headless PDF rendering.
