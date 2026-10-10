# Data-Analyst-Python
# Wayfair Online Store: Sales Review 2024-2025

A cleaned and analysed view of two years of online orders (Jan 2024 to Dec 2025), built to support next year's e-commerce plan.

**Tools:** Python, pandas, matplotlib, Jupyter

## 1. Data cleaning decisions

| Problem found | Rows affected | What I did | Why |
|---|---|---|---|
| Repeated orders (same order_id exported twice) | 38 | Dropped duplicates, kept one copy (9,245 to 9,207 orders) | Exact repeats would count the same sale twice |
| Inconsistent category labels (e.g. "decor", " Kitchen", "KITCHEN") | 22 product rows | Trimmed spaces and standardised capitalisation (24 label variants to 8 categories) | Same category spelled differently would split its totals |
| Missing `unit_price` in order items | 199 lines | Filled with the median price paid for that product in other orders (list price as fallback) | Order prices differ from the current list price (often about 5% higher), so the median paid price is closer to what customers really paid |
| Blank `discount_code` | Many | Left blank, treated as "no promo" | Not an error |

Checks that passed: no orphan order items, products or customers, dates all inside 2024-2025, no missing values in customers.

## 2. Revenue definition

- **Counts as revenue:** orders with status `completed` only. Canceled orders never became sales, and returned orders were refunded.
- **Line revenue** = `quantity x unit_price - discount_amount`
- **Order revenue** = sum of its line revenues
- **Shipping fees are excluded.** They are a delivery pass-through, not product revenue.
- Total revenue (2024-2025): about $1.88M from 8,456 completed orders. Average order value: about $223.

## 3. Results

### Revenue trend

| | 2024 | 2025 | Growth |
|---|---|---|---|
| Revenue | $851,158 | $1,030,922 | **+21.1%** |

![Monthly revenue](charts/01_monthly_revenue.png)

- Every month grew in 2025 compared with 2024.
- **November is the peak in both years** (about $101K in 2024 and $130K in 2025), driven by the last week of the month (Thanksgiving, Black Friday, Cyber Monday). Promo-code use jumps from about 5% of orders in a normal month to about 38% in November.
- December grew only about 5% in 2025, the weakest month, likely because demand was pulled into November.

### Categories

![Category revenue](charts/02_category_revenue.png)

| Category | 2024 | 2025 | Growth | Share 2025 |
|---|---|---|---|---|
| Furniture | $144,465 | $178,452 | +23.5% | 17.3% |
| Kitchen | $123,747 | $156,105 | +26.1% | 15.1% |
| Decor | $127,211 | $150,815 | +18.6% | 14.6% |
| Bedding | $123,854 | $143,177 | +15.6% | 13.9% |
| Lighting | $106,223 | $131,653 | +23.9% | 12.8% |
| Outdoor | $117,207 | $130,426 | +11.3% | 12.7% |
| Storage | $58,039 | $81,676 | +40.7% | 7.9% |
| Bath | $50,413 | $58,619 | +16.3% | 5.7% |

- Top product by revenue: **Cotton Patio Chair** (about $67K). Top product by units: **Nordic Ornament Set** (355 units).
- **Category mix is seasonal:** Outdoor takes a big share in Q2 and Q3, while Decor jumps in Q4.

![Category mix by quarter](charts/03_category_mix.png)

### Returns, cancellations and order value

![Return rate by category](charts/04_return_rate.png)

| Category | Orders | Return rate | Cancel rate | AOV |
|---|---|---|---|---|
| Furniture | 1,007 | **11.7%** | 3.1% | $376 |
| Bath | 1,643 | 4.4% | 4.6% | $73 |
| Bedding | 1,800 | 5.1% | 3.3% | $162 |
| Outdoor | 1,353 | 4.4% | 3.7% | $199 |
| Kitchen | 2,844 | 4.1% | 3.8% | $107 |
| Decor | 2,729 | 4.2% | 3.5% | $110 |
| Storage | 1,733 | 4.2% | 3.3% | $87 |
| Lighting | 1,227 | 4.0% | 2.9% | $208 |

Rates are calculated per order and category. An order with items from two categories counts once in each, so the category order counts add up to more than the total number of orders.

 ### 4. Summary

### Three key findings

1. **Revenue grew 21.1%** (from $851K to $1.03M), with every month up. November peaks in both years because of late-November sales events and heavy promotion.
2. **Furniture leads revenue in both years, and Kitchen is the volume engine.** Kitchen has the most orders and the fastest growth among the big categories (+26.1%, moving from 4th to 2nd). Outdoor (Q2/Q3) and Decor (Q4) drive the seasonal swings.
3. **Furniture has a return problem.** Its return rate is 11.7%, more than twice the roughly 4% of other categories, on the highest order value ($376).

### Two recommendations

1. **Scale Kitchen.** It has the most orders, strong growth, steady sales all year and a normal return rate (4.1%). Invest in promotions and affiliate partnerships here, and lift its low order value ($107) with bundles or a free-shipping threshold. Affiliate is a small channel (330 orders) but has the lowest return and cancel rates, so it is a low-risk way to grow. Time seasonal pushes ahead of demand: Outdoor before Q2 and a Black Friday campaign before Q4.
2. **Reduce Furniture returns before pushing Furniture harder.** It is the biggest category by revenue, so each return on a $376 order costs more than on a $73 one. Find out why items come back (damage, size, product descriptions) and fix it.

## 5. Limitations

- Reasons for patterns (Black Friday timing, customers waiting for sales) are inferred from the data, not proven. The dataset has no survey, traffic or rating data.
- Revenue ignores product costs, so it shows sales, not profit.
- Missing prices were estimated (median price paid), so those 199 lines are approximate.
- The affiliate channel is small, so its low return rate may not hold at larger scale.
