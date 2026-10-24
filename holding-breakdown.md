---
layout: default
title: Holding Breakdown
parent: Report Pages
nav_order: 2
---

# Holding Breakdown

The **Holding Breakdown** page provides a focused view of an individual investment.

Unlike the Portfolio Holdings page, which compares many investments at once, Holding Breakdown is intended to answer:

> **What does this particular investment represent within my portfolio, and what should I know about it?**

The page is designed primarily as a **drill-through page**. This means it normally receives the selected investment automatically from another report visual.

---

# Opening the page

The most useful way to access Holding Breakdown is through **Drill through** from an investment elsewhere in the report.

For example:

1. Locate an investment on the **Portfolio Holdings** or **Portfolio Analysis** page.
2. Right-click the investment.
3. Select **Drill through**.
4. Choose **Holding Breakdown**.

Power BI carries the selected investment into the Holding Breakdown page.

This allows the page to focus specifically on that security without requiring the user to manually locate it again.

---

# Selected investment context

Once the page is opened through drill-through, the selected symbol becomes the context for the page.

The measures and visuals shown on the page are therefore calculated specifically for that investment.

For example, if **RY — Royal Bank of Canada** is selected before drilling through, the Holding Breakdown page shows information relating to RY rather than the portfolio as a whole.

> Always check the selected investment shown on the page before interpreting the results.

---

# Portfolio position

One of the key purposes of this page is to show how significant the selected investment is relative to the entire portfolio.

## Selected Investment

Represents the current market value of the investment being examined.

The value uses the currently selected pricing method.

Conceptually:

**Selected Investment Value = Quantity Held × Selected Price**

## Rest of Portfolio

Represents the combined market value of all other investments in the portfolio.

Conceptually:

**Rest of Portfolio = Total Portfolio Value − Selected Investment Value**

Together, these two values make up the full investment portfolio.

---

# Portfolio share visual

The portfolio comparison visual divides the portfolio into:

- **Selected Investment**
- **Rest of Portfolio**

This provides an immediate visual indication of how concentrated the portfolio is in the selected security.

### Example

Suppose:

- Selected investment market value = **$30,000**
- Total portfolio market value = **$300,000**

The selected investment represents:

**$30,000 ÷ $300,000 = 10%**

The visual would therefore show approximately:

- Selected Investment — **10%**
- Rest of Portfolio — **90%**

---

# Portfolio Weight

Portfolio Weight expresses the selected investment's current market value as a percentage of the total investment portfolio.

Conceptually:

**Portfolio Weight = Selected Investment Market Value ÷ Total Portfolio Market Value**

This is useful because the same investment performance can have very different consequences depending on position size.

### Example

Consider two investments that both decline by 20%.

**Investment A**

- Portfolio weight: 1%
- Approximate effect on total portfolio: relatively small

**Investment B**

- Portfolio weight: 20%
- Approximate effect on total portfolio: much more significant

Portfolio Weight therefore provides context for investment risk and concentration.

---

# Why portfolio concentration matters

A large investment is not automatically undesirable.

However, the larger an investment becomes as a percentage of the portfolio, the greater its influence on overall portfolio results.

A heavily concentrated holding can cause the portfolio to:

- benefit substantially when that investment performs well
- decline substantially when that investment performs poorly
- become more dependent on one company, fund, asset class, or investment strategy

The Holding Breakdown page therefore helps distinguish between:

> **How well is this investment performing?**

and

> **How important is this investment to the overall portfolio?**

Both questions matter.

---

# Price Source

Where available, the page uses the same **Price Source** logic as the rest of the portfolio report.

## Live

Uses the current available market price.

If a live market price is unavailable for an investment, the model can use the available portfolio snapshot price as a fallback.

## AceMoney

Uses the investment price recorded in the portfolio snapshot.

Because the selected investment value and the total portfolio value both depend on valuation, changing the Price Source can change:

- selected investment market value
- total portfolio value
- portfolio weight
- portfolio allocation percentages

---

# Understanding selected pricing

The portfolio comparison is based on the **selected pricing method** rather than mixing valuation sources.

This is important because portfolio weight should compare values calculated on a consistent basis.

For example, if Live pricing is selected:

1. the selected investment is valued using the available live price;
2. the portfolio is valued using the same selected-pricing logic; and
3. the investment's portfolio percentage is calculated from those values.

This keeps the selected investment and the rest of the portfolio comparable.

---

# Relationship with Portfolio Holdings

The **Portfolio Holdings** page answers:

> What investments do I own and how have they performed?

The **Holding Breakdown** page answers:

> What does this specific investment look like when examined individually?

Use Portfolio Holdings for broad comparison.

Use Holding Breakdown when one particular investment deserves closer examination.

---

# Relationship with Portfolio Analysis

The **Portfolio Analysis** page is designed for screening and comparison.

It helps identify holdings based on factors such as:

- portfolio weight
- annualized return
- holding age
- market value
- current price trend
- Review classification

Holding Breakdown is the logical next step when one of those investments deserves closer examination.

A typical workflow is therefore:

**Portfolio Analysis → identify investment → Holding Breakdown → investigate further**

---

# Example interpretation

Suppose an investment has:

- Market Value: **$50,000**
- Total Portfolio Value: **$300,000**
- Portfolio Weight: **16.7%**

This means approximately one-sixth of the investment portfolio is concentrated in this single holding.

That does not by itself indicate whether the investment is good or bad.

Instead, consider the weight together with information such as:

- ROI
- annualized return
- current trend
- holding age
- investment purpose
- diversification across the rest of the portfolio

A 16.7% position with strong long-term performance may have a very different interpretation from a 16.7% position experiencing persistent weak performance.

---

# What the page does not tell you

Portfolio Weight measures **concentration**, not investment quality.

A large portfolio weight does not automatically mean:

- the investment should be sold
- the portfolio is inappropriate
- the investment is too risky

Likewise, a small portfolio weight does not automatically mean an investment is safe or suitable.

The appropriate concentration depends on factors that may not be fully represented in the report, including:

- investment objectives
- time horizon
- risk tolerance
- diversification outside the tracked portfolio
- tax considerations
- personal financial circumstances

The page should therefore be used as an **analytical tool**, not as an automated investment recommendation.

---

# Practical uses

The Holding Breakdown page can be used to:

- investigate a specific investment selected elsewhere in the report
- understand its current dollar value
- measure its share of the total portfolio
- identify portfolio concentration
- compare one holding with the remainder of the portfolio
- understand how pricing assumptions affect its valuation
- provide additional context before making investment decisions

For comparison across all investments, return to **Portfolio Holdings** or **Portfolio Analysis**.
