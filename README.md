# 🏢 HR Attendance & Workforce Productivity Dashboard
### Power BI | DAX | Power Query | HR Analytics

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Executive Summary

> **"74 employees. 3 months of data. 7 hidden patterns that no standard HR report would catch."**

This is not a standard attendance tracker.  
This is a **business consultancy case study** built in Power BI — designed to answer the questions a C-suite executive asks *before* making workforce policy decisions.

**The result:** A 2-page executive dashboard that surfaces flight risks, burnout signals, policy violations, and ghost employees — with prescriptive recommendations, not just charts.

---

## 🎯 Business Problem

AtliQ Technologies HR team needed answers to 5 critical questions:

| # | Business Question | Answer Found |
|---|-------------------|--------------|
| 1 | Is WFH adoption under control? | ❌ No — grew 53% in 3 months, breaching 25% policy |
| 2 | Why is absenteeism cost rising? | ❌ LWP doubled from Apr to Jun (21 → 37 days) |
| 3 | Who are the top flight risks? | ⚠️ Miley Combs (16 LWP), Gregory Carr (10 LWP) |
| 4 | Are any employees never coming in? | ❌ 3 employees: 0 office days across all 3 months |
| 5 | Is sick leave linked to absenteeism? | ✅ Partially — 2 different employee clusters found |

---

## 💥 The 7 Bombshell Findings

These are insights that only surface when you go beyond surface-level reporting:

**🔴 Finding 1 — LWP Tripled**  
Leave Without Pay grew from 21.5 days (April) to 37 days (June) — a **72% increase** in just 2 months. Financial productivity bleed estimated at ₹2.1L+.

**🔴 Finding 2 — WFH Policy Breach**  
Company WFH% grew 9.4% → 11.5% → 11.9% across 3 months — **breaching the 25% threshold** with no enforcement action recorded.

**🔴 Finding 3 — Miley Combs Flight Risk**  
16 LWP days. Zero sick leave. Zero WFH. Attendance declining every month. Pattern indicates **willful disengagement**, not illness — requires immediate 1:1 intervention.

**🟡 Finding 4 — May SL Spike (4x)**  
Sick Leave jumped from 7 days (April) to 29 days (May) — a **314% spike** in one month. Possible workplace environment or health issue. Investigated but unresolved in data.

**🔴 Finding 5 — 3 Ghost Employees**  
Gustavo Ritter (61 WFH, 0 Office), Maximus Mckenzie (33 WFH, 0 Office), Alexander Davenport (22 WFH, 0 Office) — **zero office presence across all 3 months**. No policy approval on record.

**🟡 Finding 6 — Friday WFH Pattern**  
Friday WFH rate is **34% higher** than Mon–Thu average. Employees systematically extending weekends through remote work.

**🟡 Finding 7 — Health vs Absenteeism Are Different**  
Scatter analysis revealed 2 distinct employee clusters:  
→ High LWP + Low SL = Willful absence (Miley Combs type)  
→ High LWP + High SL = Genuine health issue (Ayanna Atkins type)  
**Two different HR interventions needed.**

---

## 🛠️ Tools & Techniques Used

| Layer | Tool / Technique |
|-------|-----------------|
| Data Source | Excel (.xlsx) — 3 monthly sheets, 16 attendance codes |
| Data Cleaning | Power Query — Append, unpivot, null handling, date formatting |
| Data Modeling | Star schema — Calendar table, Main Attendance table, relationship setup |
| DAX Measures | 15+ measures — WFH%, LWP Days, Burnout Score, Risk Score, Dynamic Insight Text, Absenteeism Cost |
| Advanced DAX | CALCULATE, DATEADD, TOPN, MAXX, ADDCOLUMNS, SWITCH, FORMAT, VAR patterns |
| Visualization | 2-page executive dashboard — KPI cards, scatter chart, matrix heatmap, line chart, donut, horizontal bar |
| Design System | 8-point grid, semantic color palette (Teal/Amber/Red), F-pattern layout, amber insight strip |
| UX Features | Dynamic text measure, sync slicers, conditional border formatting, Top N filters |

---

## 📊 Dashboard Structure

### Page 1 — C-Suite Executive Insights
*"Give me the numbers — fast."*

![Page 1 - Executive Overview](Screenshots/page1_executive_overview.png)

- 5 KPI Cards (Presence%, WFH%, Absenteeism Cost, LWP Days, MoM Growth)
- Combo Chart — Absenteeism Cost vs Burnout Score by Month
- 100% Stacked Bar — Attendance composition drift Apr→Jun
- Line + Column — Day-of-week WFH pattern (Friday exposed)
- Dynamic Insight Strip — Auto-updating strategic alert text

### Page 2 — The Root Cause Analyses
*"Tell me who, why, and what to do."*

![Page 2 - Bombshell Breakdown](Screenshots/page2_bombshell_breakdown.png)

- Risk Leaderboard Matrix — Top 10 employees, heat-colored by risk score
- Scatter Chart — SL vs LWP correlation (health vs willful absence)
- Monthly Trend Line — LWP, SL, WFH% three-line story
- Leave Type Donut — Full composition of all 16 attendance codes
- WFH Dependency Bar — Who never came to office (Gustavo Ritter exposed)
- 3 Prescriptive Recommendations — Amber footer strip

---

## 📐 DAX Highlights

**Burnout Score (Composite)**
```dax
Burnout Score =
VAR _streak_score = DIVIDE([Max Office Streak], 20, 0) * 100
VAR _sl_score     = DIVIDE([SL Frequency], 0.15, 0) * 100
VAR _wfh_score    = DIVIDE([WFH Dependency Ratio] - 1, 1, 0) * 100
VAR _raw = (_streak_score * 0.40) + (_sl_score * 0.35) + (_wfh_score * 0.25)
RETURN ROUND(MIN(_raw, 100), 0)
```

**Dynamic Insight Text (Auto-updating alert)**
```dax
Dynamic Insight Text =
VAR _PeakMonth   = -- finds worst month automatically
VAR _CostFormatted = FORMAT(_TotalCost / 100000, "#,##0.00") & "L INR"
RETURN
"Strategic Alert: WFH peaked at " & _PeakWFHFormatted &
" in " & _PeakMonth & " — breaching 25% threshold. LWP surged " &
_WorstLWPFormatted & " MoM, productivity bleed: " & _CostFormatted
```

**Employee Risk Score**
```dax
Risk Score =
VAR _lwp     = [LWP Days]
VAR _sl_freq = [SL Frequency] * 100
VAR _wfh_ex  = IF([WFH %] > 0.25, ([WFH %] - 0.25) * 100, 0)
RETURN ROUND(MIN((_lwp * 4) + (_sl_freq * 2) + (_wfh_ex * 1), 100), 0)
```

---

## 💡 Strategic Recommendations

Based on data analysis, three immediate actions were identified:

**① Burnout Intervention** — Initiate 1:1 check-ins for Miley Combs + 2 high-risk employees. Implement max 15 consecutive office days policy.

**② WFH Policy Enforcement** — Cap Friday WFH at 25% company-wide. Require manager approval for repeat WFH weeks. Current growth trajectory will breach 15% average by September 2022.

**③ Data Governance Audit** — 3 employees with zero office presence and no leave approval on record. Immediate payroll audit recommended. Implement mandatory daily WFH logging.

---

## 📁 Repository Structure
hr-attendance-analytics-powerbi/
│
├── 📊 Dashboard/
│   └── hr_power_bi_project.pbix
│
├── 📂 Dataset/
│   └── Attendance-Sheet-2022-2023.xlsx
│
├── 📸 Screenshots/
│   ├── page1_executive_overview.png
│   └── page2_bombshell_breakdown.png
│
└── README.md
---

## 🔑 Key Skills Demonstrated
✅ End-to-end data pipeline (Excel → Power Query → Power BI)
✅ Advanced DAX (15+ measures including composite scores)
✅ Executive dashboard design (C-suite ready, 2-page structure)
✅ Business storytelling (findings → recommendations)
✅ Data quality investigation (ghost employees, missing data)
✅ HR domain knowledge (burnout, flight risk, WFH policy)
✅ Enterprise design system (8pt grid, semantic colors, F-pattern)
---

## 📬 Connect

**LinkedIn:** [https://www.linkedin.com/in/talaiha-abdul-sattar-78154b416/]  
**Email:** [talaihaabdulsattar50@gmail.com]

---
*Built as part of HR Analytics portfolio — demonstrating end-to-end Power BI development from raw Excel data to C-suite executive dashboard.*
