#AlphaHunt Engine

An autonomous, real time property sourcing and multichannel outreach platform built in n8n designed to eliminate manual lead generation bottlenecks in real estate investment and brokerage operations.

 Runs on a fully automated hourly cycle: pulls fresh listings, scores and enriches them, generates personalized outreach, and delivers it with zero manual intervention.

## What it does

AlphaHunt Engine watches for new property listings, filters and scores them against investment criteria, enriches owner contact data, and sends AI-generated outreach emails to qualified leads logging everything to a tracking sheet along the way.

## How it works

The workflow runs on an hourly trigger and moves through three stages:

### 1. Ingest Listings
- **Read Raw Listings** — pulls newly added property listings from a connected Google Sheet

### 2. Score & Enrich
- **Filter & Score Listings** — applies scoring logic to rank listings against investment/lead criteria
- **Skip-Trace / Enrich Owner** — calls a skip-tracing API to look up property owner contact details

### 3. Generate & Deliver Outreach
- **Generate Outreach Email** — an OpenAI-powered node drafts a personalized outreach email for each qualified lead
- **Assemble Lead Record** — compiles the enriched listing, owner, and generated message into a single lead record
- **Stage Lead in Sheet** — appends/updates the lead in a tracking sheet
- **Has Owner Email?** — branches the flow based on whether a valid owner email was found
- **Send Outreach Email** — sends the generated email via Gmail if an owner email exists

## Architecture

```
Every Hour (trigger)
      │
      ▼
Read Raw Listings ──▶ Filter & Score Listings ──▶ Skip-Trace / Enrich Owner
                                                          │
                                                          ▼
                                              Generate Outreach Email ◀── OpenAI Chat Model
                                                          │
                                                          ▼
                                                 Assemble Lead Record
                                                          │
                                          ┌───────────────┴───────────────┐
                                          ▼                               ▼
                                Stage Lead in Sheet              Has Owner Email?
                                                                          │
                                                                     (if true)
                                                                          ▼
                                                                Send Outreach Email
```

## Tech stack

| Component | Tool |
|---|---|
| Automation platform | [n8n](https://n8n.io) |
| Data source / lead tracking | Google Sheets |
| Owner enrichment | Skip-trace API |
| Outreach generation | OpenAI (Chat Model) |
| Email delivery | Gmail |

## Setup

1. Import `AlphaHunt-Engine.json` into your n8n instance.
2. Connect credentials for:
   - Google Sheets (source listings + lead tracking sheet)
   - Skip-trace API (owner enrichment)
   - OpenAI (outreach generation)
   - Gmail (email delivery)
3. Update the placeholder values in the **Skip-Trace / Enrich Owner** node with your API endpoint and key.
4. Point the **Read Raw Listings** and **Stage Lead in Sheet** nodes at your own Google Sheet(s).
5. Activate the workflow — it will run automatically every hour.

## Status

Actively running in production, processing listings and generating outreach on an hourly cycle.
