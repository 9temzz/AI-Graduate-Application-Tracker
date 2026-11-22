# Architecture and data model

## Pipeline

The workflow begins at `Email Trigger (IMAP)`, then `Normalize Email` creates a stable message shape (`id`, `internalDate`, `From`, `Subject`, `To`, `snippet`). `Loop Over Items` processes messages, checking `Email_Log` to avoid reprocessing known IDs. An early `Skip Job Alerts` IF node filters common recommendation subjects and known senders.

Remaining emails pass through a Wait node before `Information Extractor`, powered by the `Google Gemini Chat Model`. The extractor returns `job_related`, `candidate_applied`, `company`, `job_title`, `classification`, `detected_status` and `ai_confidence` according to the configured schema. The workflow logs job-related emails, creates or links application confirmations and updates existing applications for later-stage recruitment messages. Unmatched or uncertain records may be routed to `Needs_Review`.

## Google Sheets tabs and fields

**Applications** (19 columns)

```text
application_id,company,job_title,sector,location,salary,date_applied,source,status,last_updated,response_date,deadline,job_url,email_sender,last_email_subject,ai_confidence,notes,match_key,source_email_id
```

**Email_Log** (11 columns)

```text
email_id,received_at,sender,subject,company,job_title,classification,detected_status,ai_confidence,application_id,processed
```

**Needs_Review** (11 columns)

```text
email_id,received_at,sender,subject,company,job_title,classification,detected_status,ai_confidence,reason,reviewed
```

The sample file shows illustrative row objects, not an importable Google spreadsheet template. Any new spreadsheet must be created and linked manually.

## Application matching

The expression builds a `match_key` by concatenating company + job title, lowercasing, removing common terms (`graduate`, `position`, `role`, `programme`, `program`), and removing punctuation and spaces. This is a heuristic, not a perfect entity-resolution algorithm.

## Configuration notes

- The public template contains **no credentials**, live Google Sheet ID, pinned email data, runtime execution records or n8n instance identifiers.
- Every Google Sheets node requires a spreadsheet/tab to be selected after importing.
- The error-alert workflow for the private deployment is separate and not included here.
- Existing workflow data and live sheets are not part of the repository.
- n8n version at export: **2.44.5**.
