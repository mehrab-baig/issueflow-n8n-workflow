# Data Dictionary

This document defines the schema for the `MasterIssues` datastore.

| Field Name | Data Type | Source | Description | Constraints |
|---|---|---|---|---|
| `issue_key` | String | Workflow | Unique identifier formatted as `owner/repo#number`. | **Primary Key** |
| `delivery_id` | String | GitHub Webhook | The `x-github-delivery` header value. | Used for replay protection |
| `title` | String | GitHub Webhook | The raw issue title. | Not Null |
| `body` | String | GitHub Webhook | The raw issue description. | Nullable |
| `url` | String | GitHub Webhook | The `html_url` pointing to the web interface. | Valid URL |
| `issue_type` | Enum | AI LLM | Categorization of the issue. | `bug`, `feature`, `question`, `other` |
| `confidence` | Float | AI LLM | The LLM's confidence score for the chosen type. | `0.0` to `1.0` (Must be > `0.85`) |
| `priority` | Enum | Deterministic Logic | Business severity of the issue. | `urgent`, `high`, `normal`, `low` |
| `status` | String | Workflow | Current processing status of the record. | Default: `processed` |
| `processed_at` | Timestamp | Workflow | ISO 8601 timestamp of workflow execution. | Generated via `$now` |
