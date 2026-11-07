# Unit Economics Calculator: Specification (Step 1)

This document says **what** the calculator must do, in plain English, before any code is written.
Every formula here becomes code in Step 3, and the worked example becomes our test.

## Assumptions (please confirm or change)

1. **Business model: subscription.** Each customer pays a fixed price every month until they cancel.
2. **One currency** is shown throughout (default: £).
3. **Churn is constant.** The same percentage of customers cancels every month.
4. **New customers pay in their first month.** Cancellations come from the previous month's customers.
5. **Marketing spend = new customers × CAC.** All acquisition cost is paid in the month the customer joins.
6. **No tax, inflation, discounting or price changes.** These are deliberately left out to keep it simple.

## Inputs (what the user types in)

| Input | Meaning | Example |
|---|---|---|
| Price per month | What one customer pays each month | £50 |
| Variable cost per customer per month | Cost of serving one customer (hosting, materials, payment fees) | £15 |
| CAC | Customer Acquisition Cost: marketing and sales spend to win one customer | £200 |
| Monthly churn | Percentage of customers who cancel each month | 5% |
| Fixed costs per month | Rent, salaries and so on, which don't change with customer numbers | £10,000 |
| New customers per month | How many customers you win each month | 50 |

## Outputs (what the calculator shows)

| Output | Formula | Worked example |
|---|---|---|
| Monthly margin per customer | Price − Variable cost | 50 − 15 = **£35** |
| Gross margin % | Margin ÷ Price | 35 ÷ 50 = **70%** |
| Average customer lifetime | 1 ÷ Churn | 1 ÷ 0.05 = **20 months** |
| LTV (lifetime value) | Margin × Lifetime | 35 × 20 = **£700** |
| LTV:CAC ratio | LTV ÷ CAC | 700 ÷ 200 = **3.5 : 1** |
| CAC payback | CAC ÷ Margin | 200 ÷ 35 = **5.7 months** |
| Break-even customers | Fixed costs ÷ Margin, rounded up | 10,000 ÷ 35 = 285.7 → **286 customers** |

**Health signal for LTV:CAC.** A common rule of thumb is that **3:1 or higher** is healthy and **below 1:1** means you lose money on every customer.
This is an industry convention, not a law. The calculator will show it as guidance only.

## 24-month projection (the chart)

For each month, starting from zero customers:

- **Active customers** = last month's active × (1 − churn) + new customers
- **Monthly profit** = active × margin − new customers × CAC − fixed costs
- **Cumulative profit** = running total of monthly profit

Worked example (same inputs as above):

| Month | Active customers | Monthly profit | Cumulative profit |
|---|---|---|---|
| 1 | 50 | −£18,250 | −£18,250 |
| 6 | 264.9 | −£10,728 | −£86,164 |
| 12 | 459.6 | −£3,913 | −£125,661 |
| 16 | 559.9 | −£404 | **−£132,316** (lowest point) |
| 17 | 581.9 | +£366 | −£131,950 |
| 18 | 602.8 | +£1,097 | −£130,852 |
| 24 | 708.0 | +£4,780 | −£110,827 |

**What this shows:** monthly profit turns positive in **month 17**.
Cumulative profit hits its lowest point in **month 16**, at **−£132,316**. That low point is the **cash you'd need to fund the business** before it pays for itself.

> **Correction (Step 3):** an earlier version of this document said the lowest point was about £131k around month 18.
> That was wrong. It was read from sample months only. Testing the finished calculator found the true low point at month 16.

After that the hole starts to shrink, but by month 24 it is still about £111k.

## Known limitations

- The model is deliberately simple and is not a full financial forecast. There is no seasonality, no growth in new customers, no pricing tiers and no expansion revenue.
- "Customers" can be fractional in the projection (for example, 264.9). That's normal for this kind of averaged model.
- If churn is 0%, lifetime and LTV are infinite. The calculator will show "∞" and not crash.
- If margin is zero or negative, payback and break-even can't be reached. The calculator will say so.
