# IssueFlow Test Suite



> **Note on GitHub Webhook Behavior:** When opening a new issue, GitHub often fires multiple `source_events` simultaneously (such as `opened`, `labeled`, or `edited`). To prevent race conditions and duplicate processing, this workflow uses a front-door bouncer node ("New issue?") to ensure only `opened` and `reopened` events are processed. All other events are safely rejected and documented in the `RunLog`. This minimizes irrelevant executions and protects the database.



---
<br>

## T01: Valid bug with reproduction steps

 * **Input fixture:** GitHub Issue: "Live Test: App crashes on login screen"

 * **Expected result:** MasterIssues row, high priority

 * **Actual result:** Row successfully appended with 'high' priority.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#386

 * **Screenshot path:** [`screenshots/T01.png`](screenshots/T01.png)

 * **What changed:** 1 row appended to MasterIssues, 2 rows added to RunLog (for `opened` and `labeled` events respectively).



## T02: Feature request

* **Input fixture:** GitHub Issue: "Feature Request: Add dark mode" (with feature label/keywords)

* **Expected result:** MasterIssues row, normal priority

* **Actual result:** Row successfully appended with 'normal' priority.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#404

* **Screenshot path:** [`screenshots/T02.png`](screenshots/T02.png)

* **What changed:** 1 row appended to MasterIssues, 2 rows added to RunLog (for `opened` and `labeled` events).



## T03: Security/production issue

* **Input fixture:** GitHub Issue: "CRITICAL: Major security vulnerability found" (with bugs, security label/keywords)

* **Expected result:** MasterIssues row, urgent priority and alert generated

* **Actual result:** Row appended successfully, ntfy alert triggered.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#407

* **Screenshot path:** [`screenshots/T03.png`](screenshots/T03.png), [`screenshots/T03_ntfy_alert.png`](screenshots/T03_ntfy_alert.png)

* **What changed:** 1 row added to MasterIssues, 2 row added to RunLog, ntfy alert webhook fired.



## T04: Missing title

* **Input fixture:** Pinned webhook data: "body.issue.title set to empty, `x-github-delivery` fake value set"

* **Expected result:** ReviewQueue row, RunLog row, no API call

* **Actual result:** Rejected by `CHECK: req fields` if-node, route to false branch, appended to `ReviewQueue`.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#413

* **Screenshot path:** [`screenshots/T04.png`](screenshots/T04.png)

* **What changed:** 1 row added to ReviewQueue, 1 row appended to RunLog.



## T05: Missing repository

* **Input fixture:** Pinned webhook data with `body.repository.full_name` set to empty string.

* **Expected result:** ReviewQueue row appended, no API call

* **Actual result:** Rejected by required fields validation, appended to ReviewQueue.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#414

* **Screenshot path:** [`screenshots/T05.png`](screenshots/T05.png)

* **What changed:** 1 row added to ReviewQueue, 1 row appended to RunLog.



## T06: Same delivery replayed

* **Input fixture:** Replayed execution of previously successful issue (T03)

* **Expected result:** No duplicate master row

* **Actual result:** Caught by the duplicate check node, routed to Duplicate RunLog.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#415

* **Screenshot path:** [`screenshots/T06.png`](screenshots/T06.png)

* **What changed:** 0 row added to MasterIssue, 1 row appended in RunLog (status duplicate_ignored)



## T07: Same issue with a new delivery

* **Input fixture:** Pinned webhook data with new `x-github-delivery` ID, but an existing `issue.number`

* **Expected result:** Update or duplicate-review path

* **Actual result:** Caught by duplicate check based on `issue_key`, routed to Duplicate RunLog.

* **Pass or fail:** ✅ PASS

* **n8n execution ID:** ID#416

* **Screenshot path:** [`screenshots/T07.png`](screenshots/T07.png)

* **What changed:** 0 rows added to MasterIssues, 1 row added to RunLog (duplicate_ignored).



## T08: API 401 (Unauthorized)

 * **Input fixture:** Pinned webhook data, with GitHub API token intentionally invalidated.

 * **Expected result:** ReviewQueue and RunLog error

 * **Actual result:** API returned `401`, error safely caught and routed to ReviewQueue.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#426

 * **Screenshot path:** [`screenshots/T08.png`](screenshots/T08.png), [`screenshots/T08_unauthorized_api.png`](screenshots/T08_unauthorized_api.png)

 * **What changed:** 1 row added to ReviewQueue, 1 row added to RunLog (api_failure).



 ## T09: API 404 (Not Found)

 * **Input fixture:** Pinned webhook data requesting a non-existent issue number (99999).

 * **Expected result:** ReviewQueue and RunLog error

 * **Actual result:** API returned `404` Not Found, error safely caught and routed to ReviewQueue.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#433

 * **Screenshot path:** [`screenshots/T09.png`](screenshots/T09.png)

 * **What changed:** 1 row added to ReviewQueue, 1 row added to RunLog (resource not found).



 ## T10: API 429 (Rate Limit)

 * **Input fixture:** Simulated `429` Rate Limit response from GitHub API.

 * **Expected result:** Limited retry, then review

 * **Actual result:** HTTP Request node's error output safely catches the `429` status and routes the payload to the ReviewQueue.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#434

 * **Screenshot path:** [`screenshots/T10.png`](screenshots/T10.png)

 * **What changed:** 1 row added to ReviewQueue, 1 row added to RunLog (api_failure).

&#x20;

 ## T11: Invalid AI label

 * **Input fixture:** Prompt injection attempting to force output "pizza".

 * **Expected result:** ReviewQueue or Schema Rejection

 * **Actual result:** Structured Output Parser strictly enforces the enum schema. AI self-corrected to "other" with low confidence.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#459

 * **Screenshot path:** [`screenshots/T12.png`](screenshots/T12.png), [`screenshots/T12_ntfy_alert.png`](screenshots/T12_ntfy_alert.png)

 * **What changed:** row appended: ReviewQueue, RunLog; ntfy-alert generated



 ## T12: AI confidence below 0.85

 * **Input fixture:** GitHub Issue with prompt injection confusing the AI to forget rest of instructions.

 * **Expected result:** ReviewQueue, RunLog, ntfy alert

 * **Actual result:** AI returned confidence of 0.6. Caught by AI safety check and routed to ReviewQueue.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#459

 * **Screenshot path:** [`screenshots/T12.png`](screenshots/T12.png), [`screenshots/T12_ntfy_alert.png`](screenshots/T12_ntfy_alert.png)

 * **What changed:** ReviewQueue/RunLog row appended with ID, ntfy alert with details



 ## T13: Empty body

 * **Input fixture:** GitHub Issue with a title but an empty body.

 * **Expected result:** Correctly accepted or reviewed according to documented rule

 * **Actual result:** Passed required fields check (body is not strictly required). Successfully appended to MasterIssues with high ai-confidence:0.9.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#466

 * **Screenshot path:** [`screenshots/T13.png`](screenshots/T13.png)

 * **What changed:** 1 row added to MasterIssues, 1 row added to RunLog.



 ## T14: Daily digest rerun

 * **Input fixture:** Manual trigger of Daily Digest workflow executed twice on the same day.

 * **Expected result:** No duplicate digest

 * **Actual result:** First run succeeded. Second run was caught by the date-key duplicate check and safely skipped.

 * **Pass or fail:** ✅ PASS

 * **n8n execution ID:** ID#471

 * **Screenshot path:** [`screenshots/T14_already_sent_daily_digest.png`](screenshots/T14_already_sent_daily_digest.png)

 * **What changed:** 0 rows added to DailyDigest sheet, 0 ntfy alerts sent on the second run.

