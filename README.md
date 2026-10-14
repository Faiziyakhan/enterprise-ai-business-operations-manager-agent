# Enterprise AI Business Operations — Manager Agent

An enterprise-grade multi-agent business orchestration system built with **n8n**.

This system automates incoming business requests from intake through AI-driven departmental routing, executive synthesis, automated CRM tracking, and a human-in-the-loop decision gate.

---

## Overview

The Enterprise AI Business Operations Platform coordinates enterprise departments using specialized AI agents.

Incoming requests are first analyzed by an operations Manager agent, which classifies intent, assigns priority, and routes tasks to specialist departmental agents. Findings are then consolidated into an executive business report, reviewed by a CEO agent for strategic decision-making, logged into a central Google Sheets CRM, and routed through an interactive human approval gate before client communications are triggered.

---

## Problem

Modern enterprise operations suffer from disjointed inter-departmental communication:

- Manual triage and routing of client requests to sales, HR, finance, or customer support.
- Siloed department assessments that lack centralized business context.
- Slow executive decision turnaround due to scattered documentation.
- Lack of audit trails linking initial inquiry, analysis, approval, and final client notification.

---

## Solution

This project implements a **49-node n8n workflow** coordinating **7 specialized AI agents** powered by Google Gemini with strict structured output parsing:

1. **Intake & Triage:** Normalizes payload and extracts business parameters.
2. **Manager Assessment:** Classifies request category and triggers appropriate department agents via dynamic routing.
3. **Specialist Evaluation:** Parallel execution across Sales, Finance, HR, and Customer Support.
4. **Strategic Reporting & Executive Review:** Aggregates findings into an executive report for final CEO evaluation.
5. **CRM Audit & Human Approval Gate:** Records multi-department insights to Google Sheets, sends an interactive manager approval email, and pauses execution until approved or rejected.
6. **Automated Notification:** Dispatches tailored confirmation or status update emails based on the human decision.

---

## Multi-Agent Architecture

The system features **7 dedicated AI Agent nodes**, each backed by Google Gemini and a LangChain Structured Output Parser:

| Node | Agent | Role |
|------|-------|------|
| 05 | **Manager AI Agent** | Chief Operations Manager evaluating primary intent, priority, confidence score, and departmental assignment. |
| 12 | **Sales AI Agent** | Senior Sales Consultant evaluating opportunity size, buying stage, recommended automation package, and follow-up priority. |
| 27 | **Finance AI Agent** | Senior Financial Consultant evaluating project investment size, implementation costs, expected ROI, and pricing models. |
| 17 | **HR AI Agent** | Senior HR Automation Consultant analyzing workforce automation, resume screening requirements, and onboarding complexity. |
| 22 | **Customer Support AI Agent** | Support Automation Consultant identifying ticket deflection, knowledge base scope, and SLA automation needs. |
| 34 | **Reporting AI Agent** | Business Strategy Consultant synthesizing multi-department outputs into an executive business report. |
| 38 | **CEO AI Agent** | Executive decision-maker providing final transformation verdict (Approved / Approved with Conditions / Rejected), investment priority, and rollout strategy. |

---

## Workflow Flow

```text
Client Request (Webhook)
      ↓
Normalize & Build Enterprise Context
      ↓
Manager AI Agent (Gemini + Structured Parser)
      ↓
Agent Router (Conditional Routing)
  ├─ Sales AI Agent
  ├─ Finance AI Agent
  ├─ HR AI Agent
  └─ Customer Support AI Agent
      ↓
Merge & Aggregate Department Results
      ↓
Enterprise Reporting AI Agent (Executive Synthesis)
      ↓
CEO AI Agent (Final Strategic Decision)
      ↓
CRM Logger (Google Sheets: Initial Append)
      ↓
Manager Approval Email (Interactive Approve / Reject Actions)
      ↓
Wait for Approval (Webhook Resume Hook)
      ↓
IF Approval Decision
  ├─ [Approved] → Update Google Sheets Row → Dispatch Approval Email
  └─ [Rejected] → Update Google Sheets Row → Dispatch Status Update Email
      ↓
Merge Final Execution Output
```

---

## Intake Parameters & Schema

The intake webhook accepts structured business parameters:

| Parameter | Description |
|-----------|-------------|
| `customer_name` | Primary stakeholder name |
| `customer_email` | Stakeholder email address |
| `customer_company` | Client company or organization |
| `request_type` | Nature of service requested (e.g., Enterprise AI Business Transformation) |
| `request_message` | Detailed business requirements |
| `priority` | Priority classification (Default: `Normal`) |

The system assigns an operational tracking execution ID (`request_id`) and timestamps each phase for end-to-end traceability.

---

## Technology Stack

- **Orchestration:** n8n (49 nodes, advanced branching, merge, wait, and condition logic)
- **AI Core:** Google Gemini (via `@n8n/n8n-nodes-langchain.lmChatGoogleGemini`)
- **Schema Validation:** LangChain Structured Output Parsers (`outputParserStructured`)
- **Data Store:** Google Sheets (centralized CRM with multi-department audit columns)
- **Communication:** Gmail (automated responsive HTML email templates)
- **Human-in-the-Loop:** n8n Wait Node with cryptographic resume URLs for one-click approval/rejection

---

## Evidence & Verified Visual Proof

This repository contains verified captures from the active self-hosted n8n environment, CRM synchronization, and live communication flows:

### 1. Multi-Agent Workflow Canvas & Architecture
| Full Workflow Canvas (49 Nodes) | End-to-End Orchestration Flow |
| :---: | :---: |
| ![Full Workflow Canvas](Screenshot%202026-08-01%20221801.png) | ![End-to-End Orchestration Flow](Screenshot%202026-10-11%20222053.png) |

### 2. Multi-Agent Router & Parallel Execution Detail
| Departmental AI Agent Stack Detail |
| :---: |
| ![Departmental AI Agent Stack](Screenshot%202026-10-11%20222001.png) |

### 3. Centralized Google Sheets CRM Database
| Centralized CRM Ledger & Human Decision Audit Log |
| :---: |
| ![CRM Ledger](Screenshot%202026-10-09%20234728.png) |

### 4. Human-in-the-Loop Governance & Client Notification Loop
| Manager Interactive Review Email (Approve / Reject Action) | Dispatched Client Confirmation (Project Approved) |
| :---: | :---: |
| ![Manager Review Email](Screenshot%202026-09-16%20005002.png) | ![Client Confirmation Email](Screenshot%202026-09-16%20003939.png) |
---

## Data & Demo Notice

All records, company names, and email instances displayed in screenshots and demonstration data are **fictional test samples** used exclusively for validation and demonstration purposes. No real corporate or client data is contained herein.

---

## Security

API keys, OAuth credentials, service account tokens, and webhook IDs are sanitized in the exported workflow template.

---

## Project Status

**Completed — Portfolio Project**

---

## Author

**Faiziya Khan**
AI Automation Engineer | n8n | AI Agents | RAG | Workflow Automation

📧 Email: khanfaiziya2@gmail.com
