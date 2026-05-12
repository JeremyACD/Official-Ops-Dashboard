[README.md](https://github.com/user-attachments/files/27655887/README.md)
# Primerica Operations Hub — README

**Version:** 2.0 | **File:** `index.html` | **Type:** Self-contained single-page dashboard (no server required)

---

## What Is This?

The Primerica Operations Hub is a fully offline, browser-based analytics dashboard. You open the HTML file in any modern browser, upload your CSV exports, and the dashboard processes everything locally — nothing is sent to any server. All data stays on your computer.

It gives you four main views:

| Tab | What It Shows |
|---|---|
| 📞 Operations | Call volume, goal cards, hourly charts, queue breakdown, agent performance table |
| ⏱ Adherence | Schedule vs. actual worked time, policy violations, clock in/out tracking |
| 👑 Team Leaders | Dedicated view for Shelley Branch, Melanie Vasallo, and Cristina Cortes |
| 🎯 QA Evaluators | QA evaluation totals & scores + customer survey results by agent |

---

## Quick Start

1. Open `index.html` in Chrome, Edge, or Firefox
2. Click the upload buttons in the top-right header to load your data
3. Use the **Day / Week / Month / Quarter / Annual** view controls to filter
4. All charts and tables update instantly as you change filters

---

## Data Uploads

All uploads are in the top-right header. Buttons turn **green** once data is loaded. You can upload multiple files at once for any source that supports it — the dashboard merges them automatically.

### 📞 CDR Report *(required for Operations tab)*

Your Call Detail Record export from the phone system.

**Key columns the dashboard reads:**

| Column | Used For |
|---|---|
| `Date / Time` | Date and hour of each call |
| `Queue` | Queue assignment and filtering |
| `User` | Agent name — must match Schedule/State names |
| `Completion Code` | Determines Answered / Abandoned / IVR / Callback status |
| `Direction` | Inbound vs Outbound |
| `Answered In SLA` | Service Level calculation |
| `Talk Time`, `Handle Time`, `Queue Time`, `Wrap Time` | AHT, ASA, Wrap metrics |
| `Callbacks (Requested)` | Callback volume |
| `Denied (Previously)` | Denied call tracking |
| `Forwarded`, `Transfer Time` | Transfer tracking |
| `Channel Type` | Chat vs Voice separation |

> **Abandons:** Only calls that waited **≥ 60 seconds** in queue before hanging up count as true abandons. Shorter drops and IVR-stage hangs are excluded from the Queue Abandon % metric.

---

### 📅 Schedule CSV *(required for Adherence tab)* — supports multiple files

Your staffing/schedule export. Select multiple weeks at once using Ctrl+Click (Windows) or Cmd+Click (Mac) — they all merge into one dataset.

**Key columns:**

| Column | Used For |
|---|---|
| Agent name | Matched to State Detail and CDR data |
| Scheduled start/end times | Defines the expected shift window |
| Date | Which day the schedule applies to |

---

### 👤 User State Detail *(required for Adherence + Staffing by Hour)* — supports multiple files

The agent state/activity export showing actual time spent in each state. Supports merging multiple files (e.g. one per week).

**Key columns:**

| Column | Used For |
|---|---|
| `User Name` / `User` | Agent identifier |
| `State Name` | Break, Lunch, Ready, Answered, etc. |
| `Start Time`, `End Time` | Actual time in each state |

> **Note:** Even if you don't upload a State Detail file, the Staffing by Hour chart will auto-populate using your Schedule CSV data as an approximation.

---

### 🎯 QA Evaluations — supports multiple files

Your QA monitoring results. Column names are flexible — the dashboard recognizes common variations automatically.

**Recognized column names:**

| Data Point | Accepted Column Names |
|---|---|
| Evaluator | `Evaluator`, `Evaluator Name` |
| Agent | `Agent`, `Agent Name`, `Employee`, `Rep` |
| Date | `Date`, `Eval Date` |
| Score | `Score`, `Final Score`, `Total Score`, `Points` |
| Category | `Category`, `Type`, `Form`, `Queue` |
| Comment | `Comment`, `Comments`, `Notes`, `Feedback` |

**Minimum required:** Evaluator + Agent + Score (anything > 0)

**Example CSV:**
```
Evaluator,Agent,Date,Score,Category,Comment
Jane Smith,John Doe,2026-03-01,92,Compliance,Good call
Jane Smith,April Brown,2026-03-02,87,Quality,
Bob Jones,John Doe,2026-03-03,78,Soft Skills,Needs improvement
```

---

### ⭐ Survey Results — supports multiple files

Customer satisfaction scores tied to agents. Common CSAT, NPS, or star-rating exports all work.

**Recognized column names:**

| Data Point | Accepted Column Names |
|---|---|
| Agent | `Agent`, `Agent Name`, `Employee`, `Rep` |
| Date | `Date`, `Survey Date` |
| Score | `Score`, `Rating`, `Survey Score`, `CSAT`, `NPS` |
| Question | `Question`, `Category` |
| Comment | `Comment`, `Comments`, `Feedback`, `Notes` |

**Minimum required:** Agent + Score (anything > 0)

**Example CSV:**
```
Agent,Date,Score,Comment
John Doe,2026-03-01,4.5,Very helpful
April Brown,2026-03-02,5,Excellent service
John Doe,2026-03-03,3,Waited too long
```

---

## Tabs In Detail

### 📞 Operations Tab

#### Goal Cards (top row)
Color-coded against targets. Green = meeting goal, Yellow = within 25% of target, Red = off-target.

| Metric | Target | Direction |
|---|---|---|
| Service Level | ≥ 80% | Higher is better |
| ASA (Avg Speed to Answer) | < 60 seconds | Lower is better |
| AHT (Avg Handle Time) | < 7:30 (450 sec) | Lower is better |
| Queue Abandon % | < 10% | Lower is better |
| Avg Callback Time | < 60 minutes | Lower is better |

#### Volume Cards
Total counts for: Queue Offered, Answered, Outbound, Transferred, Abandoned, Callbacks, Denied, and Avg Talk Time.

#### Call Volume Heat Map
Color-coded tiles for every day in the selected period — darker = lower volume, amber/gold = higher volume. Each tile shows the answered call count and abandon rate. Useful for spotting day-of-week patterns.

#### Hourly Charts
- **Calls Per Hour** — Offered vs Answered
- **Transfers Per Hour**
- **Denied Calls Per Hour**
- **AHT Per Hour** — line chart in minutes

#### IVR Activity
Calls that entered the phone system but never reached a queue. Separated from all other metrics.

#### Spanish Language Queues
Any queue with "Spanish" or "Non-Monitored Spanish" in the name is broken out here with its own goal cards and queue table.

#### Chat Support
Chat channel interactions shown separately.

#### Staffing by Hour
Agent headcount per hour based on State Detail data (or Schedule data if State Detail isn't uploaded). Shows staffed agents (active), agents on break/lunch, and peak/low/average staffing levels.

#### Queue Breakdown Table
Per-queue stats: Offered, Answered, Transfer, Abandoned, Abandon%, Callbacks, Outbound, Denied, Denied%, Avg Talk, AHT, ASA, SL%.

#### Agent Performance Table
Per-agent stats with sortable columns. Click any column header to sort. The **Agent** column has a filter box for quick name search.

**Hover over any agent name** to see a popup with:
- QA Evaluations received + average score
- Survey responses + average score

Click an agent name to open their **detail panel** with three sub-tabs: Performance, Attendance, and State Detail.

---

### ⏱ Adherence Tab

Compares scheduled shifts against actual state activity and calculates a violation-based adherence percentage.

#### How Adherence % Is Calculated

```
Adherence % = (Scheduled Minutes − Violation Minutes) ÷ Scheduled Minutes × 100
```

Violations are minutes spent **over** the policy limit for each state:

| State | Policy Limit | Notes |
|---|---|---|
| Break | 10 min per break, max 2 breaks | 3rd+ breaks count entirely as violation |
| Lunch | 30 min | Time over 30 min = violation |
| Additional Call Wrap | 2 min | Per occurrence |
| System Setup | 5 min total | Per day |
| Outbound Callback | 3 min | Per occurrence |
| Not Ready | 2 min total | Per day |
| Clock-In late/early | ± 1 min tolerance | Every minute beyond ±1 min = violation |
| Clock-Out late/early | ± 1 min tolerance | Every minute beyond ±1 min = violation |

#### Views
- **Day** — single-day detail per agent with clock badges, lunch time, all policy cells
- **Week** — grid view showing each day's adherence % for the week
- **Month / Quarter / Annual** — summary view with violation counts, no-show days, absence tags

#### Absence Tags
Click any agent row in Day or Week view to open a detail modal where you can mark absence status (PTO, FMLA, Excused, Unexcused, Late, Left Early, Bereavement, Jury Duty). Tags are saved in your browser session.

#### Filters
- **Adherence filter** — show only agents ≥90%, 75-89%, 50-74%, <50%, or No Activity
- **Clock filter** — show only Late Clock-In, Early Clock-In, Early Out, Stayed Late, or On Time

---

### 👑 Team Leaders Tab

Shows dedicated metrics for: **Shelley Branch**, **Melanie Vasallo**, and **Cristina Cortes**

Each team leader card shows:
- Calls Answered (of offered)
- Transfers (count + % of answered)
- QA / Team Lead state time in minutes (from State Detail data)
- AHT
- Days worked and productive time summary (if State Detail is uploaded)

Data is pulled from the CDR and State Detail files — no separate upload needed for this tab.

---

### 🎯 QA Evaluators Tab

Two sub-tabs accessible via the toggle at the top of the tab:

#### 🎯 QA Evaluations Sub-Tab

**KPI bar:**
- Total Evaluations
- Overall Average Score
- Number of QA Evaluators
- Number of Agents Evaluated

**Evaluator table** (per evaluator):
- Total evals given
- Average score given (with mini progress bar)
- How many unique agents they reviewed
- Score band breakdown (≥90 / 75-89 / <75)
- Top categories evaluated

**Agent table** (per agent):
- Total evals received
- Average score
- Highest and lowest individual score
- Which evaluators reviewed them

Both tables respect the current time period filter (Day / Week / Month / Quarter / Annual).

#### ⭐ Survey Results Sub-Tab

**KPI bar:**
- Total survey responses
- Overall average score
- Satisfaction rate (% of responses ≥ 4.5 out of 5)
- Number of agents with survey data

**Agent table:**
- Response count
- Average score with progress bar
- Star-rating distribution (5⭐ / 4⭐ / ≤3⭐ buckets)
- Two most recent customer comments

---

## View Controls

| Control | Description |
|---|---|
| **Day** | Shows data for one date (use the date picker) |
| **Week** | Shows Mon–Sun week (use ◀ ▶ to navigate weeks) |
| **Month** | Shows the full calendar month of the selected date |
| **Quarter** | Q1–Q4 with year selector |
| **Annual** | Full calendar year with year selector |

The **Queue** dropdown filters all Operations metrics to a single queue. The **Search agent** box filters the agent performance table by name.

---

## Sharing & Publishing

### 🔗 Share Link
Creates a compressed URL containing all loaded data. Paste the link and anyone can open it and see the exact same data. Works best for smaller datasets (a few days of CDR). Very large datasets may exceed URL limits — use Publish Data instead.

### 📤 Publish Data
Downloads a `dashboard-data.json` file. Place this file in the same folder as `index.html` and anyone who opens the HTML file will automatically load that data — no upload step needed. Ideal for shared drives or GitHub Pages deployments.

### ⬇ Export
Exports the current filtered view as a CSV. In Operations mode it exports the agent performance table. In Adherence mode it exports the agent adherence detail.

---

## Agent Detail Panel

Click any agent name in the Operations tab to open a slide-out panel with three tabs:

**📞 Performance**
- Calls answered, service level, AHT, ASA, wrap time, transfers, outbound, callbacks, denied, abandoned
- Hourly breakdown chart for the selected period

**⏱ Attendance**
- Summary: avg adherence %, days worked/scheduled, no-shows, late clock-ins, early clock-outs, policy violation days
- Full daily clock in/out log — click any row to open the policy detail modal and mark absence status

**📊 State Detail**
- Summary cards for each state (productive, break, lunch, ACW, callback, setup, not ready, training, Spanish callback, chat, hold, QA/Team Lead)
- Per-day breakdown table showing minutes in each state vs. the policy limit

---

## Troubleshooting

**Queue Abandon % shows 0% or seems wrong**
Abandons only count if the caller waited ≥ 60 seconds in queue. Short hangs before that threshold are excluded by design. If your queue abandon rate is genuinely 0%, the card will show 0% in a neutral color — that's correct behavior.

**Staffing by Hour is empty**
Upload the User State Detail CSV. If you've already uploaded it and the chart is still empty, check that the state file's dates match the date you have selected in the date picker.

**Agent names don't match between CDR and Schedule/State files**
The dashboard uses a fuzzy name matcher — it handles "Last, First" vs "First Last" formats and partial matches. If an agent's CDR data isn't linking to their adherence data, check that the first and last name are spelled consistently across both files.

**Adherence shows 100% for everyone**
Adherence of 100% is genuinely achievable (it means the agent had violations no greater than the policy limits). If you expected lower scores, verify that the State Detail file loaded correctly and that dates overlap with the schedule.

**QA/Survey hover tooltip shows dashes (—)**
Upload your QA Evaluations and/or Survey Results CSVs using the header buttons. Once loaded, hover tooltips populate automatically.

**Share link says "Data too large"**
Use **📤 Publish Data** instead — download the JSON file and host it alongside `index.html`.

---

## CSV Column Name Flexibility

The dashboard tries several common column name variants for each field. If your export uses a different name than listed above, you can either rename the column in your CSV or check if your variant is already recognized. Column names are case-insensitive.

---

## Policy Reference (Footer)

| Policy | Limit |
|---|---|
| Break | 10 minutes max per break · 2 breaks per shift max |
| Lunch | 30 minutes max · 1 per shift |
| Additional Call Wrap | 2 minutes max per instance |
| System Setup | 5 minutes total max per day |
| NOT READY | 2 minutes total max per day |
| Clock In/Out | ± 1 minute tolerance |

---

*Primerica Operations Hub · Built for internal use · All data processed locally in your browser*
