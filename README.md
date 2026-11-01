# Gayanara | Product & Inventory Analysis

**Portfolio case study · Spreadsheet analysis · Buying & inventory decision support**

## Project overview

Gayanara's buying team needs to allocate a limited restock budget. This case study analyzes order-item, order-status, product, and inventory data to identify products with strong demand, brands contributing the most completed-order revenue, and inventory that may need replenishment or further review.

The goal is to turn transaction and stock data into practical buying signals—not to make an automatic purchase or discount decision.

## Business questions

1. Which products generated the highest unit demand?
2. Which brands contributed the most gross product revenue from delivered orders?
3. Which currently out-of-stock products recorded demand in the latest 90-day period?
4. Which products have stock on hand but no recorded demand in the latest 90 days, or unusually high stock relative to recent sales?

## Dataset and scope

The analysis uses the supplied CSV datasets:

| Dataset | Purpose |
|---|---|
| `orders.csv` | Order date, order status, and order-level attributes |
| `order_items.csv` | Product quantities, item prices, and item subtotals |
| `products.csv` | Product attributes, brand, category, and current stock |
| `customers.csv` | Supporting customer attributes; not used for the core buying metrics |
| `reviews.csv` | Supporting product-review data; not used for the core buying metrics |

**Dataset period:** 1 January 2022 – 28 February 2025  
**Recent-period window:** 1 December 2024 – 28 February 2025 (inclusive), anchored to the latest order date in the supplied data.

## Method

### 1. Prepare and validate the data

- Enrich order-item records with order status/date and product attributes using the order and product identifiers.
- Check uniqueness of order, order-item, and product IDs.
- Check that every order item maps to an existing order and product.
- Exclude `cancelled` and `returned` orders from demand and realized-sales metrics.

### 2. Separate demand from completed sales

Two measures are kept separate to avoid treating all open orders as completed sales:

- **Order demand:** quantities from `processing`, `shipped`, and `delivered` orders.
- **Realized sales:** quantities from `delivered` orders only.

### 3. Analyze products and brands

- Aggregate eligible item quantities by product and rank products by units.
- Aggregate delivered item subtotals by brand and calculate each brand's share of delivered gross product revenue.
- Retain demand-based revenue as context, but use delivered revenue for the brand ranking.

### 4. Screen inventory

- **Stockout candidate:** current stock is zero and demand in the latest 90 days is greater than zero.
- **No-sales review candidate:** current stock is positive and demand in the latest 90 days is zero.
- **High stock-cover proxy:** current stock divided by units sold in the latest 90 days is at least 90.

These are screening rules for further review. They are not, by themselves, proof of lost sales, dead stock, or an optimal reorder quantity.

## Key findings

| Metric | Result | Interpretation |
|---|---:|---|
| Order-demand units | 5,442 | Units in processing, shipped, and delivered orders |
| Delivered units | 3,783 | Units in delivered orders only |
| Demand gross product revenue | Rp1,296,768,000 | Item subtotals before order-level discount allocation |
| Delivered gross product revenue | Rp899,937,000 | Item subtotals from delivered orders |
| Stockout candidates with recent demand | 10 products | Review for possible replenishment |
| Positive-stock products with no recent demand | 47 products | Review before considering markdowns |
| Products with high stock-cover proxy | 65 products | Review stock exposure and recent sales velocity |

### Product performance

The top demand product in the full dataset is **T-Shirt Graphic BajuKita (PRD299)** with **33 units**.

The workbook also includes a separate ranking based on delivered orders only. This provides a more conservative view of completed sales.

### Brand performance

**Riang Apparel** ranks first by delivered gross product revenue at **Rp118,443,000**, representing approximately **13.2%** of delivered gross product revenue.

Revenue is a buying signal, not a profitability measure. Margin, return rate, supplier terms, and availability should also be considered before increasing a brand's allocation.

### Inventory review

Ten products have zero current stock and positive demand during the latest 90-day window. These are replenishment review candidates.

Forty-seven products have positive current stock but no recorded demand in the same 90-day window. They should be investigated for seasonality, listing status, product age, and stock-data accuracy before any discount decision.

## Business recommendations

1. **Review stockout candidates first.** Prioritize products using recent demand and recent gross revenue, then confirm supplier availability, inbound purchase orders, and lead times.
2. **Use delivered revenue to compare brands.** Riang Apparel is the current leader in delivered gross product revenue, but compare gross margin and return rates before committing more budget.
3. **Investigate no-sales products.** Check whether items are seasonal, newly listed, inactive, or incorrectly recorded before recommending markdowns.
4. **Review high stock exposure.** Use the high stock-cover proxy as a screening list, then validate against stock age, purchase history, and expected demand.
5. **Improve future inventory analysis.** Request stock movement history, opening stock, receipts, adjustments, supplier lead times, product launch dates, and gross margin data to calculate more decision-ready reorder metrics.

## Tools and spreadsheet techniques

- **Google Sheets / Microsoft Excel-compatible workbook** for tabular analysis and reporting.
- Lookup and join logic to combine order items with order and product attributes.
- Filtering by order status and date window.
- Aggregation and ranking by product and brand.
- Inventory screening using stock, recent unit demand, and calculated ratios.

**Workbook implementation note:** The provided `.xlsx` is a prepared analysis snapshot. Its output tables are stored as calculated values rather than live formulas connected to the raw sheets. Editing the raw data will not automatically refresh the analysis; the calculations need to be rerun or rebuilt with formulas/pivot tables in Google Sheets or Excel.

## Workbook guide

| Sheet | Contents |
|---|---|
| `Start Here` | Project purpose, key definitions, and navigation |
| `Dashboard` | Executive metrics and decision highlights |
| `Product Performance` | Top 10 products by order demand |
| `Delivered Products` | Top 10 products by delivered units |
| `Brand Performance` | Brand ranking by delivered gross product revenue |
| `Inventory Analysis` | Product-level stock and recent-demand metrics |
| `Stockout Review` | Products with zero stock and recent demand |
| `No Sales 90d` | Positive-stock products with no demand in the recent window |
| `High Stock Cover` | Products with a high stock-to-recent-sales ratio |
| `Methodology` | Definitions, caveats, and buying recommendations |
| `Data Quality` | Validation checks and source-data counts |
| `Prepared Order Items` | Enriched order-item-level analysis table |
| `Raw_*` sheets | Source data retained for traceability |

## Limitations

- Current stock is a snapshot; historical stock balances and stock movements are unavailable.
- The latest 90-day period is anchored to the dataset's maximum order date, not the current date.
- A no-sales result over 90 days is a review flag, not definitive dead stock.
- The high stock-cover metric is a **ratio of current stock to units sold in the last 90 days**. It is not days of inventory on hand and should not be interpreted as a demand forecast.
- Gross product revenue is calculated from item subtotals. Order-level discounts, shipping, refunds, and other adjustments are not allocated to individual products, so this is not net revenue or profit.
- No supplier lead times, purchase orders, gross margins, product launch dates, or stock aging data are available.

## Data privacy and publication note

Before publishing this project, confirm that the supplied datasets are approved for public portfolio use. Do not publish customer-identifiable information or review content without authorization. The portfolio workbook omits direct customer contact fields, but customer IDs and review text may still require permission or removal depending on the dataset's terms.

## Files

- `Gayanara_Product_Inventory_Analysis_Portfolio.xlsx` — prepared workbook containing the dashboard, analysis tables, methodology, quality checks, and source tabs.

---

*This is an analytical case study based on the supplied datasets. It is not a claim of employment, consulting work, or an operational buying decision made on behalf of Gayanara.*
