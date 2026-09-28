# Architecture Design Decisions

### 1. Deterministic Logic over AI for Priority Routing
* **Context:** AI models can hallucinate or misinterpret severity, potentially missing a critical production outage or causing alert fatigue.
* **Decision:** Priority rules (`urgent`, `high`, `normal`, `low`) are hardcoded using deterministic IF/Switch nodes based on labels and keyword matching.
* **Result:** 100% reliability for emergency `ntfy.sh` alerts, reserving the AI exclusively for non-critical issue categorization.

### 2. Webhook Race Condition Prevention
* **Context:** GitHub issues emit multiple simultaneous webhooks upon creation (e.g., `opened`, `labeled`).
* **Decision:** Implemented a strict "Front-Door Bouncer" checking the `body.action` payload. 
* **Result:** Only `opened` or `reopened` events are processed. All subsequent simultaneous payloads are safely dropped, preventing duplicate database entries and API rate limiting.

### 3. Google Sheets as a Prototyping Datastore
* **Context:** A fast, visible datastore was needed to observe the `ReviewQueue` and `RunLog` in real-time.
* **Decision:** Google Sheets was utilized instead of a traditional SQL database.
* **Trade-off:** Lacks native primary key constraints. This was mitigated by building an idempotency check (lookup node) directly into the n8n pipeline before appending rows.

### 4. Structured Output Parsing for LLMs
* **Context:** The workflow requires exact enum matches (`bug`, `feature`, etc.) to map to the database schema. Standard LLM text output is unpredictable.
* **Decision:** Utilized n8n's Structured Output Parser with a strict JSON schema.
* **Result:** Forces the LLM to adhere to the schema or gracefully fail into the `ReviewQueue`.
