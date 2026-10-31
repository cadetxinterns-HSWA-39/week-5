# Sprint Notes — Week 5

## Team

- Odetunde Olumide Temitope — Data Scientist
- Kukoyi Zainab Omotola — Data Analyst
- Nyong Asuabiat — Data Analyst

**Scrum Master this week:** _(to be filled in by the team)_

**Sprint scope:** Stock Health Assessment, Overstock & Understock Detection, Inventory Aging Analysis (with replenishment behaviour and stock value)

## What was completed

- Built `Week5_Stock_Health_Ageing.ipynb`, run end to end, producing six result tables and four charts.
- Classified all 180 product-branch positions against safety stock, reorder level and max stock (all 180 are Critical overstock).
- Measured excess units and value by position, product, category and branch; stock value ₹28,129 cr, of which ₹28,046 cr is above max stock.
- Screened for understock now and in the ledger history (none now; 5 low points in 3 positions in January 2019).
- Built a FIFO ageing of stock on hand: 65.4% older than 2 years, average age 2.9 years.
- Analysed replenishment: 99.75% of 127,611 receipts arrived when stock was already at or above max.
- Checked the recorded reorder levels against demand during lead time plus safety stock (161 of 180 are too low).

## Decisions made

- Status bands are set against each position's own safety stock, reorder level and max stock; "Critical overstock" means more than 10× max.
- Ageing uses FIFO as at the ledger end (28 Jan 2025). The FIFO result is tied back to the ledger balance (174 of 180 positions match exactly; 0.006% difference overall).
- Demand for cover calculations is the last 12 months of Delivered orders, consistent with Week 4.
- Results are reported as recorded and carry the Week 3 `scale_mismatch_flag` caveat.

## Findings

- Overstock is total and long-standing: every position passed its max stock within two months of 2019 starting and has stayed above it.
- The cause visible in the data is replenishment that ignores stock levels: 17–18 units received per unit sold, every year.
- Stock is ageing: 65% is more than 2 years old.
- Excess is spread evenly across branches and categories, and concentrated in the highest-revenue products.

## Open items

- Physical stock count and purchase-order records are needed to confirm the recorded stock quantities (scale flag).
- Reorder levels and max stock should be reviewed before replenishment is switched back on.

## Next steps

Week 6: Warehouse Space Utilisation, Throughput, Capacity, Product–Warehouse Alignment and Warehouse Performance Scoring.
