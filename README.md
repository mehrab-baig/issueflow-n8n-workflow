# IssueFlow: GitHub Issue Triage with n8n

## Problem
Software teams are often overwhelmed by unformatted, unlabeled, and duplicate GitHub issues. Triaging these manually takes hours of engineering time away from actual development.

## Solution
IssueFlow is a fully automated, real-time triage system built in n8n. It captures GitHub webhooks, cleans the data, prevents duplicates, enriches it via the GitHub API, classifies the issue type using an AI LLM, writes to a Google Sheets datastore, and sends push notifications for urgent alerts.

## Architecture
The system consists of two primary workflows:
1. **Intake & Triage:** Catches live webhooks and routes them based on strict business logic and AI evaluation.
2. **Daily Digest:** A scheduled cron job that aggregates open issues into a summary report.
*(See `docs/architecture.mmd` for the full visual diagram).*

## Features
* **Webhook Intake & Validation:** Safely rejects malformed or irrelevant payloads (e.g., missing titles, non-'opened' events).
* **Idempotency:** Prevents duplicate processing of the same delivery ID or issue key.
* **API Enrichment:** Securely fetches live issue metadata directly from GitHub.
* **AI Classification (Optional):** Uses LLM Structured Output to classify issues, with strict fallback logic for low-confidence results.
* **Review Paths:** Routes failed API calls or AI hallucinations to a manual `ReviewQueue`.
* **Observability:** Maintains a detailed `RunLog` for every execution.

## Setup
To run these `.example.json` workflows yourself:
1. Import into n8n.
2. Connect your own Google Sheets credentials and create the 4 required sheets (`MasterIssues`, `ReviewQueue`, `RunLog`, `DailyDigest`).
3. Connect your GitHub API token and set up a webhook in your repository.
4. Replace the `ntfy.sh` URLs with your own private topic.

## Test Results
This system was built using Test-Driven Development. 
**[View the full Test Matrix and Results here](tests/test-cases.md)**. All 14 core validation, API failure, and routing tests are passing.

## Security
No private keys, OAuth credentials, or real IP addresses are stored in this repository. All workflow exports are sanitized.

## Lessons Learned
Building this system required mastering idempotency, JSON parsing, API status code handling (401/404/429), race condition prevention, and forcing strict JSON Schema outputs from LLMs.

## Limitations
Currently, Google Sheets is used as a learning datastore. In a true production environment, this would be swapped out for a robust database like PostgreSQL or directly synced to a ticketing system like Jira/Linear.

