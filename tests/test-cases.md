\# IssueFlow Test Suite



\## T01: Valid bug with reproduction steps

\* \*\*Input fixture:\*\* GitHub Issue: "Live Test: App crashes on login screen"

\* \*\*Expected result:\*\* MasterIssues row, high priority

\* \*\*Actual result:\*\* Row successfully appended with 'high' priority.

\* \*\*Pass or fail:\*\* ✅ PASS

\* \*\*n8n execution ID:\*\* \[Paste your ID here]

\* \*\*Screenshot path:\*\* `screenshots/T01.png`

\* \*\*What changed:\*\* 1 row added to MasterIssues, 1 row added to RunLog.



\## T02: Feature request

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* MasterIssues row, normal priority

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T03: Security/production issue

\* \*\*Input fixture:\*\* GitHub Issue: "CRITICAL: Major security vulnerability found"

\* \*\*Expected result:\*\* MasterIssues row, urgent alert

\* \*\*Actual result:\*\* Row appended, ntfy alert triggered.

\* \*\*Pass or fail:\*\* ✅ PASS

\* \*\*n8n execution ID:\*\* \[Paste your ID here]

\* \*\*Screenshot path:\*\* `screenshots/T03.png`

\* \*\*What changed:\*\* 1 row added to MasterIssues, 1 row added to RunLog, ntfy webhook fired.



\## T04: Missing title

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue row, no API call

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T05: Missing repository

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue row, no API call

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T06: Same delivery replayed

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* No duplicate master row

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T07: Same issue with a new delivery

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* Update or duplicate-review path

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T08: API 401 (Unauthorized)

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue and RunLog error

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T09: API 404 (Not Found)

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue and RunLog error

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T10: API 429 (Rate Limit)

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* Limited retry, then review

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T11: Invalid AI label

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T12: AI confidence below 0.85

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* ReviewQueue, no notification

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T13: Empty body

\* \*\*Input fixture:\*\* 

\* \*\*Expected result:\*\* Correctly accepted or reviewed according to documented rule

\* \*\*Actual result:\*\* 

\* \*\*Pass or fail:\*\* 

\* \*\*n8n execution ID:\*\* 

\* \*\*Screenshot path:\*\* 

\* \*\*What changed:\*\* 



\## T14: Daily digest rerun

\* \*\*Input fixture:\*\* Manual trigger of Daily Digest workflow

\* \*\*Expected result:\*\* No duplicate digest

\* \*\*Actual result:\*\* Workflow stopped at 'Already Sent' node.

\* \*\*Pass or fail:\*\* ✅ PASS

\* \*\*n8n execution ID:\*\* \[Paste your ID here]

\* \*\*Screenshot path:\*\* `screenshots/T14.png`

\* \*\*What changed:\*\* No new rows added, no ntfy alert sent.

