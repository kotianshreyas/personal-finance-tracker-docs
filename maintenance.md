---
layout: default
title: Maintenance & Troubleshooting
nav_order: 4
---

# Maintenance

This page explains how the Portfolio Analysis report is updated, when you may need to refresh it manually, and what to check if the information appears out of date.

---

## How the system works

The report is updated through a mostly automatic process:

**Source files**  
↓  
**Automated pipeline**  
↓  
**Processed files in Google Drive**  
↓  
**Automatic Power BI refresh**  
↓  
**Updated Portfolio Analysis report**

The pipeline prepares the information Power BI needs. Once the pipeline finishes successfully, it automatically asks Power BI to refresh the report.

This means the normal update process does not depend on Power BI refreshing at a predetermined time before the pipeline has finished.

---

## Normal daily operation

Under normal circumstances, no manual action is required.

The pipeline is scheduled to run twice each day.

### Intraday run — 8:15 AM

The first run is intended to update the report during market trading hours.

It refreshes items such as:

- transactions;
- current investment prices;
- exchange rates;
- account information; and
- market information.

### Closing run — 2:15 PM

The second run captures the later market position after market close.

It performs the regular updates and also updates investment price history.

> Scheduled times are target times. GitHub may occasionally start a scheduled run later than the displayed time.

The important point is that Power BI now waits for the pipeline to finish. Once the pipeline has successfully produced the new files, the Power BI refresh is triggered automatically.

---

## Power BI safety refreshes

Power BI also has two scheduled fallback refreshes:

- **1:30 PM**
- **11:30 PM**

These are safety nets.

They are not the main method used to keep the report current. Their purpose is to give Power BI another opportunity to reload the latest available files if an automatic API refresh was missed or interrupted.

---

## Pipeline Runs Today

The Process & Architecture page contains a **Pipeline Runs Today** indicator.

This shows the number of successful pipeline runs recorded for the current day.

It does **not** show the number of Power BI refreshes.

For example, Power BI may refresh automatically after a pipeline run and may also perform a scheduled safety refresh, while the Pipeline Runs Today number remains unchanged.

---

## Manual refresh options

There are two different manual actions because they perform different jobs.

### Refresh Pipeline

Use **Refresh Pipeline** when the underlying information itself may need to be updated.

Examples:

- you added or changed transactions;
- a source file was updated;
- you want fresh market prices;
- you want to rerun the normal automated process.

The button opens the GitHub Portfolio Pipeline.

Run the workflow and select the appropriate run type.

A successful pipeline run will automatically trigger the Power BI refresh afterward.

You normally do **not** need to run the pipeline and then separately click Refresh Dataset.

---

### Refresh Dataset

Use **Refresh Dataset** when the processed files are already current but Power BI needs to reload them.

Examples:

- the pipeline has already completed successfully;
- the Google Drive output files are current;
- the report still appears to show the previous data;
- you specifically want a Power BI-only refresh.

This button opens the semantic model in the **Personal Finance Analytics** workspace.

Use the Power BI refresh option from there.

Because this refresh does not run the pipeline first, it will simply reload whatever processed files are currently available.

---

## Which refresh should I use?

| Situation | Recommended action |
|---|---|
| Transactions or source files changed | **Refresh Pipeline** |
| You want newer market prices | **Refresh Pipeline** |
| You want to reproduce the normal daily process | **Refresh Pipeline** |
| Pipeline already completed but report still looks old | **Refresh Dataset** |
| Processed files are known to be current | **Refresh Dataset** |
| Everything looks current | No action required |

When in doubt, **Refresh Pipeline** is the more complete process because Power BI refreshes automatically afterward.

---

## Checking whether a refresh succeeded

### Check the pipeline

Open the Portfolio Pipeline in GitHub Actions.

A successful complete run should show a green success status.

The workflow now covers both:

1. preparing the data; and
2. refreshing Power BI.

Therefore, a successful workflow means the full automated process completed successfully.

If Power BI fails to refresh, the overall workflow should also show a failure rather than incorrectly reporting the process as complete.

### Check Power BI

In the **Personal Finance Analytics** workspace:

1. Find the **Portfolio Analysis** semantic model.
2. Open its refresh history.
3. Review the most recent refresh.

Automatic pipeline-triggered refreshes appear as **Via Api**.

Manual refreshes normally appear as **On demand**.

Scheduled safety refreshes appear as **Scheduled**.

---

## Publishing report changes

Changes made in Power BI Desktop are separate from normal data refreshes.

If you change:

- visuals;
- page layouts;
- measures;
- formatting;
- navigation; or
- report design,

save the PBIX file and publish it to the **Personal Finance Analytics** workspace.

Publishing replaces the report/model version in that workspace.

A normal data refresh does not publish Desktop design changes.

---

## Current workspace

The active report is published in:

**Personal Finance Analytics**

This is the workspace used by the automated pipeline refresh.

The older copy in **My workspace** is currently being retained as a temporary rollback copy.

Its scheduled refresh is disabled, so it should not be treated as the active report.

---

## Troubleshooting

### Pipeline Runs Today shows 0

Possible explanations:

- the scheduled pipeline has not run yet;
- the scheduled GitHub run was delayed;
- the pipeline failed; or
- the report has not yet loaded the latest refresh-log information.

Check GitHub Actions first.

---

### The report shows old information

First determine whether the pipeline has completed.

If the pipeline has **not** completed:

**Run Refresh Pipeline.**

If the pipeline completed successfully but Power BI still shows old information:

**Run Refresh Dataset.**

---

### A transaction is missing

Check that the transaction exists in the source export and that the relevant source file has been updated.

Then run **Refresh Pipeline**.

If the transaction still does not appear, investigate the pipeline output rather than repeatedly refreshing Power BI.

---

### An account appears as Not Defined

The account may not yet have a matching account classification.

Check the account source information and update the account mapping where required.

Then rerun the pipeline.

---

### GitHub shows a failed workflow

Open the failed workflow and review the failed step.

Possible causes include:

- source-file or Google Drive problems;
- market-data retrieval problems;
- Microsoft authentication problems; or
- a Power BI refresh failure.

Because Power BI refresh is now part of the workflow, a failure near the end of the workflow may relate to Power BI rather than the data-processing stage.

---

### Power BI shows a failed refresh

Open the semantic-model refresh history and review the error details.

If the pipeline itself completed successfully, the processed files may already be current.

In that situation, a Power BI-only **Refresh Dataset** may be sufficient after the underlying issue has cleared.

---

### Investment prices appear unchanged

Not every investment has a reliable live daily price source.

Some mutual funds may continue to use the latest available portfolio price instead.

A flat daily change or flat section of a trend line therefore does not necessarily mean the investment itself had no market movement.

---

## Routine maintenance

Most day-to-day use should require no maintenance.

The main items to keep current are:

- transaction/source exports;
- account information;
- supporting planning files when values change; and
- the PBIX report when report design changes are made.

The automated pipeline handles routine processing and Power BI refreshes.

---

## Important points to remember

- The pipeline normally runs twice each day.
- Power BI refreshes automatically after a successful pipeline run.
- Power BI also has two scheduled safety refreshes.
- **Pipeline Runs Today** counts pipeline runs, not Power BI refreshes.
- Use **Refresh Pipeline** when the underlying data needs updating.
- Use **Refresh Dataset** only when the processed files are already current.
- Report design changes still require publishing from Power BI Desktop.
- The active report is in the **Personal Finance Analytics** workspace.
