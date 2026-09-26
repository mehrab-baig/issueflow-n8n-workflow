# Data Dictionary (MasterIssues)
* `issue_key` (String): Unique identifier (e.g., `owner/repo#1`)
* `delivery_id` (String): Webhook delivery ID for replay protection
* `title` / `body` / `url` (String): Core issue data
* `issue_type` (Enum): `bug`, `feature`, `security`, `question`, `other` (Set by AI)
* `priority` (Enum): `urgent`, `high`, `normal`, `low` (Set by logic rules)
* `status` (String): Current processing status
