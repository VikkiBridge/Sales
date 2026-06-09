# 📊 Commercial Sales Performance Suite (Multi-Device Reporting)
## 🏢 Business Problem & Objective
### The Business Problem
Executive leadership and regional management required a two-tiered approach to sales monitoring that standard, static end-of-month reporting could not provide:
1. **Regional Strategic Alignment:** Regional managers lacked a unified view to instantly compare current Month-to-Date (MTD) performance against historical baselines (such as a full view of the previous month) and mid-month progress checks. Furthermore, without automated regional ranking against explicit targets, it was difficult to quickly identify which territories were driving growth and which were falling behind.
2. **On-the-Go Executive Mobility:** The Managing Director (MD) required instant, friction-free access to critical operational metrics while traveling between branches or attending external meetings. Standard desktop-oriented dashboards are notoriously difficult to navigate on mobile devices, and delayed data refreshes meant the MD lacked visibility into today's moving targets.
### The Objective
This project delivers a robust, dual-purpose reporting system designed to serve both deep desktop analysis and agile mobile consumption:
* **The Regional Desktop Dashboard:** Serves as the primary tool for territory evaluation, providing a comprehensive layout that breaks down sales velocity across various time horizons and ranks regional performance directly against corporate targets.
* **The Executive Mobile Dashboard:** A highly optimized, near-real-time mobile view tailored specifically for the Managing Director. It isolates high-impact "Today" and "MTD" metrics, ensuring business viability is visible at a glance from any location.
---
## 🛠️ Tech Stack & Architecture
* **BI Platform:** Microsoft Power BI Desktop / Power BI Service Cloud
* **Data Refresh Architecture:** Near-real-time scheduled refresh intervals for high-frequency operational tables.
* **Layout Design:** Dual-canvas development (Standard 16:9 Desktop Layout + Custom Mobile-Responsive Layout).
* **Data Model:** Clean Star Schema optimization to ensure rapid DAX execution over live sales transactions.
---
## 📋 Report Canvas Breakdowns
### 1. Regional Sales Performance & Target Rankings (Desktop View)
This view focuses on time-intelligence comparison and gamified performance ranking. It tracks sales across highly specific operational horizons to give context to current performance:
* **Today's Sales:** Immediate visibility into the current day's invoicing.
* **Current MTD Sales:** Cumulative sales for the active calendar month.
* **Mid-to-Yesterday Sales:** A rolling baseline tracking performance from the middle of the month up to the previous day, allowing managers to spot mid-month acceleration or stagnation.
* **Last Month’s Sales:** A fixed historical benchmark for MoM comparison.
* **Dynamic Regional Rankings:** A custom ranking matrix that automatically evaluates and orders regions based on their percentage-to-target achievement, making underperforming territories instantly visible.
### 2. Branch Sales & Margin Tracker (Mobile-Optimized View)
Designed with a "Mobile-First" philosophy specifically for the MD on the move.
* **Thumb-Friendly Navigation:** Features clean, vertical card layouts and enlarged touch-targets to avoid visual clutter on phone screens.
* **Almost Live Updates:** Connected to an aggressive refresh pipeline, providing near-real-time updates for **Today's Sales** and **Current Month-to-Date (including today) Sales**.
* **Margin Integrity Protection:** Displays both top-line revenue and gross margin percentages by branch, ensuring that a high volume of low-margin sales doesn't mask a drop in profitability.
---
## 📐 DAX & Data Transformation Highlights
ByRankAll Today = 
RANK(
    SKIP,
    ALL('MPDSMD - Today'),
    ORDERBY('Measures Table'[Profit for rank], DESC, 'MPDSMD - Today'[Branch], ASC)
)
---
ByRankX = IF(
    HASONEVALUE('MPDSMD - Today'[Branch]),
        RANKX(
            ALLSELECTED('MPDSMD - Today'), 
            CALCULATE(
SUM('MPDSMD - Today'[Profit])),
            ,
            DESC, Dense))
