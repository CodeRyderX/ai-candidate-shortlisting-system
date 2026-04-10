# 🤖 AI Candidate Shortlisting System

> An end-to-end n8n automation that sources, scores, and personalises outreach for recruitment candidates — saving **35+ hours per week** and over **£15,000/month** in manual recruitment costs.

---

## 📌 Overview

This workflow automates the full top-of-funnel recruitment pipeline for a recruitment agency. When a new role is added to Airtable, the system automatically:

1. Scrapes LinkedIn for matching candidates via Apify
2. Deduplicates them against an existing Airtable candidate database
3. Uses an AI agent (GPT-4o) to score and evaluate each candidate against the job requirements
4. Filters only the highest-ranking candidates
5. Generates a hyper-personalised LinkedIn connection message for each one
6. Saves everything back to Airtable and loops until the required number of qualified candidates is met

No manual sourcing. No copy-pasted outreach. No duplicate data entry.

---

## 🧩 Workflow Architecture

```
Airtable Trigger (New Role)
        │
        ▼
Status Check → Only "Ready" roles proceed
        │
        ▼
Data Transformation → Formats role data for LinkedIn scraper
        │
        ▼
Apify LinkedIn Scraper → Finds matching candidates
        │
        ▼
Deduplication → Removes candidates already in database
        │
        ▼
AI Verdict Agent (GPT-4o) → Scores & classifies each candidate
  • over-qualified / qualified / under-qualified
  • Years of experience, strengths, weaknesses, fit score
        │
        ▼
Filter → Only high-scoring qualified candidates pass
        │
        ▼
Icebreaker Generator (GPT-4o) → Personalised LinkedIn message (≤250 chars)
        │
        ▼
Airtable → Saves candidate + score + message
        │
        ▼
Loop Check → If target candidate count not met, re-runs automatically
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **Airtable** | Candidate & job role database / trigger |
| **Apify** | LinkedIn profile scraping |
| **OpenAI GPT-4o** | Candidate evaluation & message generation |
| **n8n LangChain nodes** | AI agent orchestration + structured output parsing |

---

## ✨ Key Features

- **AI Candidate Verdict Agent** — Evaluates candidates against role requirements including years of experience, target companies, target schools, and industry. Returns a structured JSON verdict with scores and justification.
- **Icebreaker Message Generator** — Crafts personalised LinkedIn connection requests referencing each candidate's unique career path, skills, or background. Capped at 250 characters.
- **Automatic Deduplication** — Checks Airtable before adding any candidate to prevent duplicate records.
- **Self-Looping Until Target Met** — If the required number of qualified candidates hasn't been reached, the workflow re-runs automatically.
- **Status Gate** — Only roles marked as "Ready" in Airtable trigger the workflow, giving recruiters full control.
- **Structured Output Parsing** — AI responses are parsed into typed JSON schemas for reliable downstream use.

---

## 📊 Impact

| Metric | Value |
|--------|-------|
| Manual hours saved per week | **35+ hours** |
| Monthly cost saving | **£15,000+** |
| Workflow nodes | **30** |
| Automation type | Fully autonomous, end-to-end |

---

## 🚀 Setup

### Prerequisites

- n8n instance (self-hosted or cloud)
- Airtable account with a base configured for roles and candidates
- OpenAI API key (GPT-4o access)
- Apify account with LinkedIn scraper access

### Airtable Schema

Your **Roles** table should include fields for:
- `Role Name`
- `Years of Experience`
- `School` (target universities)
- `Past Companies` (target employers)
- `Industry`
- `Job Description`
- `Status` (set to `Ready` to trigger)
- `Required Candidate Count`

Your **Candidates** table will be populated automatically by the workflow.

### Installation

1. Import `Candidate_Shortlisting_System.json` into your n8n instance via **Workflows → Import from file**
2. Set up credentials in n8n for:
   - Airtable (API key or OAuth)
   - OpenAI (API key)
   - Apify (API token)
3. Update the Airtable base and table IDs in the trigger and Airtable nodes to match your setup
4. Set a role's `Status` field to `Ready` in Airtable to trigger the workflow

---

## ⚠️ Notes

- The workflow uses **polling** on the Airtable trigger, not webhooks — adjust the poll interval to suit your needs
- LinkedIn scraping is handled via Apify's actor; ensure your Apify plan supports the volume required
- AI scoring prompts are embedded in the agent nodes and can be customised to match your agency's specific evaluation criteria

---

## 📄 License

MIT — free to use, adapt, and build on.

---

## 🙋 Author

Built by Emmanuel Uttams (CodeRyderX). For questions, custom builds, or freelance enquiries, reach out via [Upwork](#).