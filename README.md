# CadetX — Heavy Supplier, Inventory & Warehouse Analytics

## Week 5 submission: Stock Health, Overstock & Understock Detection, Inventory Ageing

Phase 2 (Core Product, Inventory & Warehouse Analytics), sprint 2 of 3.

## Team

- Odetunde Olumide Temitope — Data Scientist
- Kukoyi Zainab Omotola — Data Analyst
- Nyong Asuabiat — Data Analyst

## What's in this folder

```
week-05/
├── Week5_Stock_Health_Ageing.ipynb    (full analysis, run end to end, outputs included)
├── README.md
├── SPRINT_NOTES_WEEK5.md
└── outputs/
    ├── position_stock_health.csv      (180 product-branch positions: status, cover, excess units and value)
    ├── product_stock_health.csv       (30 products: stock vs max, years of supply, average age)
    ├── ageing_by_bucket.csv
    ├── ageing_by_branch_pct.csv
    ├── replenishment_by_year.csv
    ├── historical_low_stock_events.csv
    └── charts/ (4 PNG charts)
```

To re-run the notebook, point `DATA_DIR` at a folder holding `sales_transactions.csv`, `inventory_position.csv` (both in the Week 2 folder) and `stock_ledger.csv` (Week 3 folder).

## Ground rules

- Demand = Delivered sales orders, last 12 months to 2024-12-31.
- Stock = `inventory_position.csv` `current_stock`, as recorded. Value = `current_stock × unit_cost`.
- Stock history from `stock_ledger.csv` (2019-01-01 to 2025-01-28) using `implied_signed_quantity`, the Week 3 fix for the ADJUSTMENT sign problem.
- Positions are product × branch (30 products × 6 branches = 180).

## Results

### 1. Stock health
Every one of the 180 positions is in the **Critical overstock** band (more than 10× its max stock). Stock is a median **349× max stock** (range 177× to 854×; 333× across the network). No position is below its reorder level or safety stock. Median cover is about 98 years of demand, while max stock itself equals a median 102 days of demand.

The recorded **reorder levels are also low**: 161 of 180 sit below demand during lead time plus safety stock (average 65 recorded vs 96 needed). This does not cause problems while stock is this high, but the parameters would need fixing before replenishment is switched back on.

### 2. Overstock and understock
- Stock value is **₹28,129 cr**, of which **₹28,046 cr (99.7%)** is above max stock: 19.25 million of 19.30 million units.
- At the last-12-month sales rate, the excess takes **97 years to sell**.
- By category, the excess is Engine ₹9,381 cr, Hydraulic ₹7,673 cr, Transmission ₹4,519 cr and Undercarriage ₹3,693 cr. The five largest products hold 60% of all excess value.
- By branch, excess value is similar everywhere (₹4,453 cr to ₹4,765 cr). Pune is most overstocked relative to max (381×) and Kolkata least (288×).
- **Understock: none now.** In history, the ledger balance fell below reorder level at only 5 points, in 3 positions, all in January 2019 (the last on 17 Jan 2019). No balance ever reached zero (the lowest was 14 units).
- All 180 positions went above max stock within the first two months of 2019 (median 21 Jan 2019) and have stayed there since.

### 3. Inventory ageing (FIFO, as at 28 Jan 2025)
| Age | Share of units | Share of value |
|---|---|---|
| 0–90 days | 3.9% | 4.0% |
| 91–180 days | 4.2% | 4.1% |
| 181–365 days | 8.6% | 8.7% |
| 1–2 years | 17.8% | 18.0% |
| 2–3 years | 17.5% | 17.2% |
| 3+ years | 48.0% | 48.1% |

**65.4% of stock is more than 2 years old**, and the weighted average age is 2.9 years. The pattern is the same in every branch (46–50% is 3+ years old) and every product (64–67% older than 2 years).

### 4. Replenishment behaviour
- The ledger records **127,611 purchase receipts totalling 20.4 million units** (average 160 units).
- **99.75% of receipts (127,289) arrived when stock was already at or above max stock**, and only 3 receipts arrived below the reorder level.
- Each year 2019–2024 the business received about **17–18 units for every unit sold** (about 3.3–3.4 million received against about 0.19 million sold).
- Replenishment therefore does not respond to stock levels. Buying carried on at the same pace after every position was far above its limit.

## Caveats

- **Scale flag.** All 180 positions carry the Week 3 `scale_mismatch_flag`: recorded stock is hundreds of times its max stock. The ledger shows how that stock built up (receipts far above sales), but the figures need confirming with a physical count and the purchase-order records before action is taken.
- **FIFO is an assumption** about how stock is picked. The FIFO stock equals the last ledger balance in 174 of 180 positions (total 19,305,994 vs 19,304,792 units, a 0.006% difference) and equals `current_stock` in 166 positions, matching the 166-of-180 finding from Week 3.
- 2025 covers only 1–28 January, so its receipt-to-sales ratio is not comparable with full years.
