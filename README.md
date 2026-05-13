[README.md](https://github.com/user-attachments/files/27715232/README.md)
# Primerica Operations Hub — README

**Version:** 3.0 · **File:** `index.html` · **Type:** Self-contained single-page dashboard — no server, no install, open in any browser

---

## What Is This?

A fully offline, browser-based analytics dashboard. Open `index.html` in Chrome, Edge, or Firefox, upload your CSV exports, and everything processes locally — no data is sent anywhere. All five data sources feed into four main tabs that all respond to the same time period controls.

| Tab | What It Shows |
|---|---|
| 📞 **Operations** | Call volume, goal cards, hourly charts, heat map, staffing, queue breakdown, agent table |
| ⏱ **Adherence** | Schedule vs. actual, policy violations, clock in/out, absence tagging |
| 👑 **Team Leaders** | Dedicated cards for Shelley Branch, Melanie Vasallo, Cristina Cortes |
| 🎯 **QA Evaluators** | QA evaluation results + customer survey results, with two sub-tabs |

---

## Quick Start

1. Open `index.html` in Chrome, Edge, or Firefox
2. Click the upload buttons in the top-right header to load your data (all five support multiple files)
3. Use the **Day / Week / Month / Quarter / Annual** view controls to filter
4. All tabs, charts, tables, and the agent pop-up panel update instantly

---

## Upload Buttons

All five buttons are in the top-right header. Buttons turn green once data is loaded. Every button accepts multiple files — hold **Ctrl** (Windows) or **Cmd** (Mac) to select more than one at a time.

### 📞 CDR Report — supports multiple files

Your Call Detail Record export. Multiple files are merged and exact duplicate rows are automatically removed.

| Column | Used For |
|---|---|
| `Date / Time` | Date and hour of each call |
| `Queue` | Queue assignment and filtering |
| `User` | Agent name |
| `Completion Code` | Answered / Abandoned / IVR / Callback status |
| `Direction` | Inbound vs Outbound |
| `Answered In SLA` | Service Level calculation |
| `Talk Time`, `Handle Time`, `Queue Time`, `Wrap Time` | AHT, ASA, wrap metrics |
| `Callbacks (Requested)` | Callback volume |
| `Denied (Previously)` | Denied call tracking |
| `Forwarded` | Transfer tracking |
| `Channel Type` | Chat vs Voice separation |

> **True Abandons:** Only calls that waited **60 seconds or more** in queue count. Shorter hangs and IVR-stage drops are excluded from the Queue Abandon % metric.

---

### 📅 Schedule CSV — supports multiple files

Your staffing/schedule export. Select multiple weeks at once to cover a full month.

| Column | Used For |
|---|---|
| Agent name | Matched to State Detail and CDR data |
| Scheduled start / end | Defines expected shift window |
| Date | Which day the schedule applies to |

---

### 👤 User State Detail — supports multiple files

Agent state/activity export showing actual time in each state. Multiple files merge automatically.

| Column | Used For |
|---|---|
| `User Name` / `User` | Agent identifier |
| `State Name` | Break, Lunch, Ready, Answered, etc. |
| `Start Time`, `End Time` | Actual time in each state |

> If no State Detail file is uploaded, the Staffing by Hour chart auto-populates from the Schedule CSV as an approximation.

---

### 🎯 QA Evaluations — supports multiple files

Your QA monitoring results. Reads your exact export columns:

| Your Column | Used For |
|---|---|
| `Evaluator` | Who performed the evaluation |
| `User` | Agent being evaluated |
| `Date / Time` | Evaluation date |
| `Evaluation Score Percent` | Score 0–100% |
| `Evaluation Score` | Raw score (fallback) |
| `Evaluation Goal` | Goal threshold for attainment calculation |
| `Queue` | Which queue the eval was for |
| `Evaluation Form Name` | Form used |
| `Evaluation Status` | Completed / In Progress / etc. |

---

### ⭐ Survey Results — supports multiple files

Customer satisfaction scores. Reads your exact export columns:

| Your Column | Used For |
|---|---|
| `User` | Agent name |
| `Date / Time` | Survey date |
| `Survey Answer` | Score 1–5 |
| `Survey Name` | Survey type (Agent Support, etc.) |
| `Survey Category Name` | Category / question label |
| `Survey Channel` | Call, Chat, etc. |
| `Session ID` | Unique session identifier |

---

## Time Period Filters

Every tab — Operations, Adherence, Team Leaders, QA Evaluations, Survey Results, and the agent pop-up panel — all respond to the same controls.

| View | Picker | What It Shows |
|---|---|---|
| **Day** | Single date picker | One specific date |
| **Week** | Week range + arrows | Mon–Sun week, navigate with arrows |
| **Month** | Month dropdown + Year | Full calendar month |
| **Quarter** | Q1/Q2/Q3/Q4 + Year | Three-month quarter |
| **Annual** | Year dropdown | Full calendar year |

Switching views auto-selects the current month, quarter, or year. Change any dropdown and everything updates instantly.

---

## Operations Tab

### Goal Cards

Color-coded against targets. Green = on target, Yellow = within 25%, Red = off target.

| Metric | Target |
|---|---|
| Service Level | ≥ 80% |
| ASA (Avg Speed to Answer) | < 60 seconds |
| AHT (Avg Handle Time) | < 7:30 |
| Queue Abandon % | < 10% |
| Avg Callback Time | < 60 minutes |

### Call Volume Heat Map

Hourly grid color-coded in aqua/teal — dark = low volume, bright = high volume.

**Operating hours:**

| Day | Hours |
|---|---|
| Monday – Saturday | 8:00 AM – 10:00 PM |
| Sunday | 11:00 AM – 8:00 PM |

Hours outside operating windows are blank. Hover any cell for answered count, offered, and abandon %.

**Day / Week** — each row is a date, each column is an hour, shows actual counts.

**Month / Quarter / Annual** — each row is a day of the week (Sun–Sat), each column is an hour, shows the **average** answered calls across all matching days. The label next to each day name shows how many of that weekday are included (e.g. "Mon 13d").

The heat map sits side-by-side with Staffing by Hour.

### Staffing by Hour

Agent headcount per hour from State Detail data (Schedule data as fallback). Shows active agents, agents on break/lunch, and peak/low/average staffing levels.

### Hourly Charts

Calls Per Hour (Offered vs Answered), Transfers Per Hour, Denied Calls Per Hour, and AHT trend.

### IVR Activity

Calls that entered the system but never reached a queue — excluded from all agent metrics.

### Spanish Language Queues

Any queue with "Spanish" in the name gets its own goal cards and queue table.

### Chat Support

Chat channel interactions shown separately.

### Queue Breakdown Table

Per-queue: Offered, Answered, Transfer, Abandoned, Abandon%, Callbacks, Outbound, Denied, Denied%, Avg Talk, AHT, ASA, SL%.

### Agent Performance Table

Per-agent stats with sortable columns (click any header to sort). Live filter box under the Agent column.

**Hover** any agent name for a quick-stats popup: QA Evals Received, Avg QA Score, Survey Responses, Avg Survey Score.

**Click** any agent name to open their full detail panel — see Agent Pop-Up Panel section below.

---

## Adherence Tab

Compares scheduled shifts against actual state activity to calculate an adherence percentage.

### Formula

```
Adherence % = (Scheduled Minutes − Violation Minutes) ÷ Scheduled Minutes × 100
```

### Policy Limits

| State | Limit | What Counts as a Violation |
|---|---|---|
| Break | 10 min per break · 2 per shift | Minutes over 10 per break; entire 3rd+ break |
| Lunch | 30 min | Minutes over 30 |
| Additional Call Wrap | 2 min per instance | Minutes over 2 |
| System Setup | 5 min total per day | Minutes over 5 |
| Outbound Callback | 3 min per instance | Minutes over 3 |
| Not Ready | 2 min total per day | Minutes over 2 |
| Clock In | ±1 min tolerance | Every minute beyond ±1 |
| Clock Out | ±1 min tolerance | Every minute beyond ±1 |

### Adherence Views

| View | Layout |
|---|---|
| Day | Per-agent detail with clock badges, lunch time, all policy state cells |
| Week | Grid showing each day's adherence % for the week |
| Month / Quarter / Annual | Summary with violation counts, no-show days, and absence tags |

### Absence Tags

Click any agent row to open a detail modal. You can mark: PTO, FMLA, Excused, Unexcused, Late, Left Early, Bereavement, Jury Duty. Tags persist for the session.

### Filters

**Adherence filter:** ≥90% / 75–89% / 50–74% / <50% / No Activity

**Clock filter:** Late Clock-In / Early Clock-In / Early Out / Stayed Late / On Time

---

## Team Leaders Tab

Three cards — one each for **Shelley Branch**, **Melanie Vasallo**, and **Cristina Cortes**. Data is pulled automatically from whatever files you've already uploaded — no extra upload needed.

| Section | Data Source |
|---|---|
| Call Performance (Answered, Transfers, AHT) | CDR Report |
| QA as Evaluator (evals completed, avg score, goal attainment) | QA Evaluations |
| Personal QA Score (when they are the agent being evaluated) | QA Evaluations |
| Survey Score | Survey Results |
| Avg Adherence + QA/TL State Time | Schedule + State Detail |

All metrics respond to the current time period filter.

---

## QA Evaluators Tab

### 🎯 QA Evaluations Sub-Tab

**KPI bar:** Total Evaluations · Overall Avg Score % · Goal Attainment · Agents Evaluated

**Evaluator breakdown table:**
- Total evals given
- Avg score given (with progress bar)
- Number of agents reviewed
- Score bands: ≥90% / 75–89% / <75%
- Top queues covered
- Goal attainment (evals meeting goal / total evals with a goal set)

**Agent QA scores table:**
- Evals received, avg score, highest, lowest, goal met %, reviewed by whom

Both tables filter with the selected time period.

### ⭐ Survey Results Sub-Tab

**KPI bar:** Total Responses · Overall Avg Score · 5-Star Rate · Agents with Surveys

**Distribution bar:** Visual 5⭐ / 4⭐ / 3⭐ / 1–2⭐ breakdown across the period with percentages.

**Agent survey table:** Response count, avg score, per-star-rating counts, survey type.

Fully filters with the selected time period — Day, Week, Month, Quarter, or Annual.

---

## Agent Pop-Up Panel

Click any agent name in the Operations tab. Three sub-tabs appear:

### 📞 Performance

Full call metrics for the selected period plus an hourly breakdown chart: calls answered, SL%, AHT, ASA, wrap time, transfers, outbound, callbacks, denied, abandoned.

### ⏱ Attendance

- Summary: avg adherence %, days worked vs scheduled, no-shows, late clock-ins, early clock-outs, policy violation days
- Full daily clock in/out log — click any date row to open the policy detail modal and add an absence tag

### 📊 State Detail

Summary cards for every state: Productive, Break, Lunch, Additional Call Wrap, Outbound Callback, System Setup, Not Ready, Training, Spanish Callback, Chat Support, Hold, QA / Team Lead.

Each card shows total minutes, percentage of shift, and how far over or under the policy limit.

Below the cards is a per-day table showing every worked day in the selected period with minutes per state and the day's adherence %.

The State Detail view is fully period-aware — Day shows one day, Month shows the whole month, Quarter shows the quarter, Annual shows the full year.

---

## Sharing & Publishing

### 🔗 Share Link

Compresses all loaded data into a URL. Anyone who opens the link sees the same data with no upload step. Best for smaller datasets (a few days of CDR).

### 📤 Publish Data

Downloads `dashboard-data.json`. Place this file in the same folder as `index.html` (or in the root of your GitHub repo). Anyone who opens the HTML will load that data automatically — no upload required. Ideal for keeping a shared team dashboard current.

### ⬇ Export CSV

Exports the current filtered view: agent performance table (Operations tab) or agent adherence detail (Adherence tab).

---

## GitHub Pages Setup

1. Push `index.html` to the root of a **public** GitHub repository
2. Go to **Settings → Pages → Branch: main · Folder: / (root) → Save**
3. Your dashboard is live at `https://yourusername.github.io/your-repo-name/`

**To keep data current:**
1. Open the dashboard locally and upload fresh CSVs
2. Click **📤 Publish Data** — downloads `dashboard-data.json`
3. Upload/replace that file in the repo root
4. GitHub redeploys in ~30 seconds — the team gets fresh data on next page open

---

## Troubleshooting

**State Detail tab in the agent panel is blank**
No State Detail data found for this agent in the selected period. Check that the User State Detail file was uploaded and that the agent's dates fall within the current filter.

**Survey Results not updating when I change the period**
Make sure you're using the upload button in the top header (⭐ Survey Results). The tab filters from the raw uploaded rows — if no file is loaded it can't filter.

**Queue Abandon % is 0%**
Calls must wait ≥ 60 seconds in queue before hanging up to count as a true abandon. Shorter drops don't count. If your abandons genuinely happen in under a minute, the metric will show 0% correctly.

**Agent names don't link between CDR and Adherence**
The dashboard uses fuzzy matching (handles "Last, First" vs "First Last" and double spaces). If an agent still doesn't link, verify their first and last name are spelled consistently across all files.

**Team Leader cards show dashes (—)**
Either no data exists for that TL in the selected period, or the name in the file doesn't match exactly. The three configured names are: Shelley Branch · Melanie Vasallo · Cristina Cortes.

**Staffing by Hour is empty**
Upload the User State Detail CSV. The chart uses Schedule data as a rough fallback but needs State Detail for actual agent headcount.

**Share link is too large**
Use 📤 Publish Data instead. Download the JSON file and commit it to your GitHub repo.

---

*Primerica Operations Hub · All data is processed locally in your browser · Nothing is sent to any external server*
