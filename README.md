# SkillSync — Automated Employee Training Progress Tracking & Skill Gap Analysis

An automated HR training pipeline built using **n8n** and **Power BI** that eliminates
manual data collection, flags overdue employees, calculates skill gap scores, and 
delivers a live analytics dashboard for HR teams.

---

## Problem Statement

In mid-to-large organizations, employee training is managed through scattered tools —
completion status tracked manually in spreadsheets, skill assessments stored separately,
and no centralized real-time view of training progress.

HR managers spend **4–6 hours per week** manually collecting and compiling training data
— by which time the data is already outdated.

---

## Solution

An automated data pipeline using **n8n** that:

- Reads training enrollment and completion data from Google Sheets
- Calculates a Skill Gap Score per employee (High / Medium / Low)
- Flags employees who have not completed mandatory training by the deadline
- Writes enriched, processed data back to Google Sheets automatically
- Feeds a live Power BI dashboard for real-time training analytics

---

## n8n Workflow

![HR Training Pipeline](HR_Training_PipeLine.jpg)

---

## Workflow Overview
```
Schedule Trigger
      ↓
Get row(s) in sheet        ← Reads Employee_Training Google Sheet
      ↓
Code in JavaScript         ← Calculates Skill Gap Score, Overdue flag, Status
      ↓
If (Condition Check)       ← Splits on completion status
   ↓              ↓
Completed        Overdue
   ↓              ↓
Append to       Append to
Output Sheet    Output Sheet
```

---

## n8n Nodes Used

| Node | Purpose |
|---|---|
| Schedule Trigger | Fires the workflow automatically on a set schedule |
| Get row(s) in sheet | Reads 15 employee records from Google Sheets |
| Code in JavaScript | Computes Skill Gap Score, Overdue flag, and Status |
| If | Branches into Completed vs Overdue paths |
| Append row in sheet | Writes 7 completed records to HR_Training_Output |
| Append row in sheet1 | Writes 8 overdue records to HR_Training_Output |

---

## Google Sheets – Input & Output

![Employee Training Sheet](Employee_Training.jpg)
![HR Training Output](HR_Training_Output.jpg)

### Input Sheet — `Employee_Training`

| Column | Description |
|---|---|
| Employee ID | Unique employee identifier |
| Full Name | Employee full name |
| Department | HR / Finance / IT / Sales / Marketing |
| Course Name | Name of the training course |
| Enrolled Date | Date of enrollment |
| Deadline | Mandatory completion deadline |
| Completed (Yes/No) | Whether training was completed |
| Score (0–100) | Assessment score |
| Email | Employee email address |

### Output Sheet — `HR_Training_Output`

Enriched columns added by the n8n workflow:

| Column | Description |
|---|---|
| Skill_Gap_Score | High / Medium / Low based on score and completion |
| Overdue | Yes / No based on deadline vs completion |
| Status | Completed / Overdue |
| Processed_Date | Date the workflow processed the record |

---

## Skill Gap Score Logic

| Condition | Skill Gap Score |
|---|---|
| Not completed | High |
| Completed, Score < 75 | Medium |
| Completed, Score ≥ 75 | Low |

---

## Power BI Dashboard — SkillSync

![SkillSync Dashboard](HR_Dashboard.jpg)

The dashboard provides real-time visibility into training performance.

**Visuals included:**
- Employee Count KPI (15 employees)
- Status Slicer — Completed / Overdue
- Department Filter — Finance, HR, IT, Marketing, Sales
- Score Distribution by Employee (bar chart)
- Sum of Score by Department (bar chart)
- Skill Gap Distribution (donut chart)
- Training Completion Overview (pie chart)
- Employee Breakdown by Department (horizontal bar chart)

---

## Results

| Metric | Value |
|---|---|
| Total Employees | 15 |
| Completed Training | 7 |
| Overdue | 8 |
| High Skill Gap | 3 |
| Medium Skill Gap | 5 |
| Low Skill Gap | 7 |
| Processed Date | 2026-03-29 |

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| n8n | Workflow automation engine |
| Google Sheets | Input data source and output write-back |
| JavaScript | Skill gap score calculation logic inside n8n |
| Power BI | Live training analytics dashboard |

---

## Repository Structure
```
📁 hr-training-pipeline
│
├── HR Training Pipeline.json       ← n8n workflow export file
├── Employee_Training.jpg           ← Input Google Sheet screenshot
├── HR_Training_Output.jpg          ← Output Google Sheet screenshot
├── HR_Dashboard.jpg                ← Power BI dashboard screenshot
├── HR_Training_PipeLine.jpg        ← n8n workflow canvas screenshot
└── README.md                       ← Project documentation
```

---

## How to Use

1. Import `HR Training Pipeline.json` into your n8n instance
2. Connect your Google Sheets account in n8n credentials
3. Replace the Sheet IDs in the Google Sheets nodes with your own
4. Set your preferred schedule in the Schedule Trigger node
5. Execute the workflow and verify output in `HR_Training_Output` tab
6. Connect the output sheet to Power BI for live dashboard refresh

---

## Author

Built as part of an HR automation project to eliminate manual training
reporting and provide real-time skill gap visibility across departments.
