# Troubleshooting & Runbook

### Symptom: Duplicate issues appearing in MasterIssues
* **Root Cause:** The n8n webhook response is timing out, causing GitHub to redeliver the payload, OR the `action` filter is missing, allowing `labeled` events through.
* **Resolution:** Ensure the n8n Webhook node is set to **"Respond: Immediately"**. Verify the first IF node strictly filters for `action == 'opened'`.

### Symptom: API Enrichment failing silently
* **Root Cause:** GitHub Personal Access Token expired or lacks repository permissions.
* **Resolution:** 
  1. Generate a new Fine-Grained PAT in GitHub with `Issues: Read-only`.
  2. Update the credentials in n8n. 
  3. Replay the failed delivery IDs from the GitHub Webhook settings.

### Symptom: ntfy.sh alerts not firing for 'Urgent' issues
* **Root Cause:** The priority string evaluation is case-sensitive, or the webhook payload is missing the triggering keywords.
* **Resolution:** Check the `RunLog` to see what priority was actually assigned. Ensure the `Prepare Priority Text` node is converting all input strings to lowercase (`.toLowerCase()`) before the IF node evaluates them.

### Symptom: Webhooks not reaching local n8n instance
* **Root Cause:** The local tunnel (Ngrok/Cloudflared) restarted, changing the public URL.
* **Resolution:** Use a static domain (e.g., `ngrok http --domain=your-domain.ngrok-free.app 5678` ) and update the GitHub webhook settings to match.
