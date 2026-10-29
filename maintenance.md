---
layout: default
title: Maintenance & Troubleshooting
nav_order: 4
---

# Maintenance & Troubleshooting

This page explains how the Personal Finance Tracker is maintained, what happens automatically, how to trigger refreshes manually, and what to check when something does not update as expected.

It is intended as an operational reference.

For normal report usage, use the **Getting Started** and **Report Pages** sections of the User Guide.

---

## System at a glance

The current process is:

**AceMoney → Google Drive → GitHub Actions → Google Drive outputs → Power BI**

Each part has a separate role:

- **AceMoney** — transaction entry and portfolio snapshot source.
- **Google Drive** — stores source files and processed outputs.
- **GitHub Actions** — runs the processing pipeline.
- **Power BI** — refreshes and displays the analytical report.

These are separate systems.

A problem in one part does not automatically corrupt the others.

---

# Routine workflow

## What you normally do

### 1. Enter transactions in AceMoney

Enter transactions normally as they occur.

This remains the manual source-entry step in the current system.

### 2. Export updated transaction data when needed

When you want recent transactions to appear in Power BI:

1. In AceMoney, go to:

   **Tools → Options**

2. Make sure **Show closed accounts** is enabled.

3. Go to:

   **Reports → General reports → Transactions by criteria**

4. Select the required date range.

5. Make sure:

   - **Show subtotals** is unchecked.
   - **Include transfers** is checked.

6. Export the report to the Transactions-Inbox folder.

7. Rename the file using the month and year format used by the existing process, for example:

   `Oct 2026.csv`

### 3. Export Portfolio.csv when investment holdings change

Export the latest `Portfolio.csv` from AceMoney to the Portfolio Snapshot folder when updated investment holdings or prices need to be reflected.

---

# Supporting maintenance files

Some source files are maintained manually and only need to be updated when the underlying information changes.

These include:

- `account_list.csv`
- `Budget.csv`
- `Category_master.csv`
- `Contribution_limits.csv`
- `Credit_Card_Limits.csv`
- `Targets.csv`
- `Opening_Balances.csv`
- `Salary_Rates.csv`
- `CarLoanSchedule.csv`

You do not need to edit all of these during a normal refresh.

Update only the file affected by the change.

---

# What happens automatically

The GitHub pipeline runs automatically twice each day.

The current authoritative schedule is:

- **Midday run:** 17:15 UTC
- **Closing run:** 22:15 UTC

The Closing run additionally updates portfolio price history.

The pipeline reads source files from Google Drive, processes them, and writes updated output files back to the Pipeline-Outputs folder.

Power BI refreshes separately from the pipeline.

The pipeline and the Power BI semantic-model refresh should therefore be treated as two distinct processes.

---

# Pipeline outputs

The main processed output files are:

## `prices.csv`

Contains current investment prices and available market metadata.

It can also contain fallback pricing for investments where an independent current price is unavailable.

## `transactions_master.csv`

Contains the consolidated transaction history used by the report.

## `account_list_enriched.csv`

Contains account classifications such as:

- account type;
- financial institution;
- owner;
- purpose.

## `price_history.csv`

Contains portfolio investment price history.

This is updated during the Closing pipeline run.

## `fx_rates.csv`

Contains daily USD/CAD exchange-rate history.

## `market_history.csv`

Contains separate market-index history used for market context.

Current indexes include:

- S&P/TSX Composite;
- S&P 500.

## `refresh_log.csv`

Records successful pipeline runs.

This file is used by the report to display pipeline-run information.

---

# Manual refresh

Manual refresh is useful when you do not want to wait for the next scheduled cycle.

## Refresh Pipeline

Use the **Refresh Pipeline** button on the Power BI System Status & Maintenance page to open the GitHub pipeline workflow.

When running the pipeline manually, choose the appropriate run type:

### Midday

Use for a normal current-data refresh.

### Closing

Use when you also want the portfolio price-history update included.

Wait until the GitHub Actions run completes successfully before expecting new pipeline outputs.

---

## Refresh Dataset

Use the **Refresh Dataset** button to open the Power BI Service area used to refresh the semantic model.

The Power BI refresh is separate from the GitHub pipeline.

A successful pipeline run does not by itself guarantee that the Power BI report has already loaded the new outputs.

---

# Pipeline Runs Today

The report can display how many successful pipeline runs have been recorded today.

This number reflects **pipeline runs**, not confirmed Power BI semantic-model refreshes.

It should therefore be interpreted as:

> **How many successful pipeline runs have been logged today?**

It should not be interpreted as an authoritative count of Power BI refresh capacity already consumed.

---

# Power BI Desktop changes

You only need Power BI Desktop when modifying the report itself.

Examples include:

- creating or changing report pages;
- changing visuals;
- changing measures;
- changing formatting;
- modifying model logic.

Normal data updates do not require Power BI Desktop.

## Publishing report changes

When report design changes are complete:

1. Open `Portfolio Analysis ver4.pbix`.
2. Make the required changes.
3. Publish the report to the existing Power BI workspace.
4. Replace the existing published model/report when prompted.
5. Open the report in Power BI Service.
6. Confirm that the changes appear correctly.

Publishing report-design changes is different from refreshing report data.

---

# Common limitations

## Manual AceMoney export

The transaction export remains a manual dependency.

If new transactions are entered in AceMoney but no updated export is placed in Google Drive, the pipeline can still run successfully using older transaction data.

A successful pipeline run therefore does not prove that all recent AceMoney activity was exported.

---

## Investment price availability

Not every investment has the same type of current price source.

Some mutual funds and other investments may use portfolio-snapshot pricing when an independent daily price cannot be retrieved reliably.

For those holdings:

- Day Change may show a dash;
- price-history charts can contain flat periods followed by jumps;
- available history can be shorter than a full year.

Do not interpret a dash as proof that the real investment price did not change.

---

## Contribution tracking

The Contribution Tracker is an internal monitoring tool.

Official Canada Revenue Agency records remain authoritative for registered-plan contribution room.

This is particularly important when:

- contribution room is nearly exhausted;
- withdrawals affect future room;
- first-60-days RRSP contributions need tax-year interpretation;
- account records differ from official CRA information.

---

## Refresh counts

The pipeline-run log and Power BI refresh history are not the same thing.

If you need to verify whether Power BI itself refreshed successfully, check the refresh history in Power BI Service.

---

# Troubleshooting

## Pipeline Runs Today shows 0

Possible reasons include:

- today's scheduled GitHub Actions run has not occurred yet;
- the pipeline failed;
- `refresh_log.csv` was not updated;
- Power BI has not refreshed since the new log entry was created.

### What to check

1. Open the GitHub repository.
2. Go to **Actions**.
3. Open the latest **Portfolio Pipeline** run.
4. Check whether it completed successfully.

---

## Pipeline succeeded but Power BI still shows old data

Possible causes include:

- the Power BI semantic model has not refreshed yet;
- the refresh failed;
- the report is displaying an older cached state;
- the source export itself was stale.

### What to check

1. Confirm the pipeline completed successfully.
2. Confirm the required source file was updated before the pipeline ran.
3. Check Power BI Service refresh history.
4. Refresh the report page after the semantic-model refresh completes.

---

## New transaction is missing from Power BI

Check the process in this order:

1. Was the transaction entered in AceMoney?
2. Was an updated transaction export created?
3. Was that export saved to the correct Google Drive folder?
4. Did the pipeline run after the export was uploaded?
5. Did Power BI refresh after the pipeline completed?

The first missing step usually identifies the cause.

---

## New account appears as Not Defined

The pipeline can discover a new account that is not yet fully classified.

If this occurs:

1. Update `account_list.csv`.
2. Add the correct account details, such as:
   - financial institution;
   - owner;
   - purpose;
   - account type.
3. Run the pipeline again.

The updated classification should be picked up on the next successful run.

---

## GitHub Actions failure

If GitHub sends a failure notification:

1. Open the repository.
2. Go to **Actions**.
3. Open the failed run.
4. Expand the failed step.
5. Read the error message.

Possible causes can include:

- temporary external price-source issues;
- authentication problems;
- malformed source data;
- missing source files.

Do not assume a failed pipeline means historical data was lost.

A failure generally means the new outputs were not refreshed successfully.

---

## Power BI shows an unexpected balance

Before assuming the calculation is wrong, check:

- selected date;
- owner filter;
- account filter;
- price source;
- whether the latest transaction export was included;
- whether the latest Power BI refresh completed.

For investment balances, also consider whether the investment uses a current market price or a fallback snapshot price.

---

## Day Change is blank or shows a dash

This is common for holdings that do not have a reliable independent daily market price.

A dash means no meaningful calculated day change is available from the recorded price data.

It does not necessarily mean the investment's actual market price was unchanged.

---

# System references

Keep the main system links available in this section.

## GitHub

Portfolio pipeline repository:

`portfolio-pipeline`

Documentation repository:

`personal-finance-tracker-docs`

## Google Drive

The Google Drive structure contains:

- source transaction exports;
- portfolio snapshots;
- maintenance files;
- pipeline outputs.

## Power BI

The published Power BI report is the main analytical interface.

The report and its semantic model can refresh separately from the GitHub pipeline.

---

# What is not currently automated

The current system deliberately still requires some manual steps.

These include:

- entering transactions in AceMoney;
- exporting transaction data from AceMoney;
- exporting `Portfolio.csv` when required;
- maintaining selected supporting CSV files;
- publishing report-design changes from Power BI Desktop;
- confirming official tax contribution room from CRA records.

The system is designed to automate processing and reporting after the source data has been supplied.

---

# When to use the Power BI Maintenance page

Use the **System Status & Maintenance** page in Power BI when you need to:

- check whether the pipeline has run;
- see the latest available status information;
- trigger a manual pipeline run;
- open the Power BI refresh controls;
- open this Maintenance & Troubleshooting guide.

For detailed instructions, troubleshooting, and reference information, use this page instead of keeping long technical instructions inside the Power BI report.
