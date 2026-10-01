# Café Sales Analysis: Did Extending Opening Hours Pay Off?

An end-to-end analysis of nine months of point-of-sale (POS) data from an independent café ("Northside Café", an anonymized client). The owner had extended evening hours to 8pm and wanted to know whether it was worth it, along with five other questions about menu performance, peak hours and revenue trends.

The client's identity, staff, customers and raw transaction records are not included in this repository.

## Business problem

The café changed ownership in September 2025 and, from December 1, 2025, extended weekday and weekend hours into the evening (5-8pm). The owner asked six questions:

1. Which menu items drive revenue, versus just volume?
2. When is revenue actually highest, by hour and day of week?
3. Did the 5-8pm evening extension pay off?
4. What is the revenue trend since September 2025?
5. What is the average transaction value, overall and by time of day and weekday?
6. Is there anything else in the data worth acting on?

## Dataset (anonymized)

Source: two POS report types covering Sep 1, 2025 to May 31, 2026: order-level transactions (13,428 paid orders after dropping unprocessed payments) and an item-level sales report (363 items).

What is published in `data/`:

| File | Contents |
|---|---|
| `orders_hourly.csv` | Orders and revenue grouped by date, hour, order type and a few flags (catering, gift card, repeat catering client, rung up by an owner) |
| `items_summary.csv` | Per-item totals: gross and net sales, units sold, discounts, refunds, cost of goods where recorded |
| `item_modifiers.csv` | Per-item modifier selections and amounts (for example milk options) |

What was removed before publishing: order IDs, employee names and IDs, customer notes, payment and card details, and the business name. Employee names were reduced to a single true/false flag for whether an owner rang up the order. The raw exports are not in this repository.

The notebook expands each hourly row back into one row per order, giving each order in a group the group's average revenue. Hourly sums, counts and average order values match the raw-data results.

## Approach

- Cleaned and combined three order exports; dropped open (unprocessed) payments; defined revenue as payments minus refunds.
- Split the data at the extension start date (Dec 1, 2025) and compared pre and post periods by hour and weekday, using only the hours the café was actually open on each day.
- Flagged catering and gift card sales and recomputed average order value with and without them.
- Parsed the item report, flagged non-menu rows, and reconciled its totals against the report's own header totals and against order-level revenue (a gap of about 2.3%).
- Noted what the data cannot answer: the item and order files share no key, so item mix by time of day is not possible.
- Built 41 charts in a "Storytelling with Data" style (declarative titles, one accent color, direct labels). They are in `charts/`.

## Key findings

- Monthly revenue grew from $16,390 (Sep 2025) to $29,806 (May 2026), up 82%, and was already rising before the extension.
- The 5-8pm window is about 26% of open hours but only 9% of post-extension revenue and 7% of orders. The growth is not mainly explained by the extension.
- Revenue builds to a midday peak every day and falls off through the afternoon; Saturday and Sunday lead the week and Tuesday lags.
- Every day of the week grew after the extension, with weekends growing the most.
- Average transaction value is $14.18 ($13.99 excluding catering and gift cards).
- Basic espresso, sandwiches and bakery together drive more than 50% of revenue. Dine-in is about a quarter of orders but a third of revenue.
- Five year-round items sell very little (Breve, Macchiato, Egg Salad, Matcha Frappe, Mocha Frappe).
- Oat milk was charged on some modifier buttons and free on others, an estimated $280 of missed revenue over nine months.
- Cost data was entered for only a few items, so item-level profitability cannot be reliably ranked.
- One repeat catering client placed 6 orders growing from $353 to $437, about 1.2% of revenue.

## Recommendation delivered

Keep the core items (medium latte, Clubhouse sandwich, Apple Fritter, Classic Butter Croissant), review the five low sellers, and fix the oat milk pricing setup. On hours, the analysis found that staffing the 5-8pm window loses money on most days, so if closing times are ever changed, the suggested closing hours were 5pm Mon/Tue/Thu, 6pm Wed/Sun and 7pm Fri/Sat. Paid labor is concentrated in the closing hour mainly on Friday and Saturday, so that is where closing early saves real payroll.

The presentation given to the owner is in `presentation.pdf`.

## Tech stack

Python, pandas, NumPy, Matplotlib, Jupyter.

## Repository structure

```
README.md
analysis.ipynb      full analysis with outputs
data/               anonymized aggregates only
charts/             exported charts
presentation.pdf    anonymized client deck
LICENSE
.gitignore
```

## Run it

```
pip install -r requirements.txt
jupyter notebook analysis.ipynb
```

Run the notebook from the repository root so that `data/` and `charts/` resolve.
