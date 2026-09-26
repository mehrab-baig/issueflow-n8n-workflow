# Troubleshooting
* **Duplicate records appearing?** Ensure GitHub webhooks aren't timing out (n8n must reply instantly) and verify the "New Issue?" front-door bouncer is catching non-opened events.
* **401/404 API Errors?** Verify the GitHub token hasn't expired and has 'Read' access to Issues.
* **Missing ntfy alerts?** Verify the issue priority evaluates to exactly `urgent`.
