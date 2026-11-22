# Security and privacy

This repository contains a **sanitised workflow template** and **fictional sample data**.

## Removed from the public workflow

- Google Sheets spreadsheet ID and cached spreadsheet URLs
- n8n credential references (IDs and names)
- Original n8n instance ID, workflow ID, version ID and linked error-workflow ID
- Original webhook identifier and incidental node/condition identifiers
- Saved/pinned execution data (none existed in the original export)

## Keep private

- API keys and authentication headers
- Google service-account key JSON files
- Gmail/IMAP passwords, app passwords and OAuth credentials
- The `.n8n` database and the `n8n_data` Docker volume (may contain encrypted credentials and an encryption key)
- Logs, screenshots or exports containing real job applications and personal emails
- Environment files and anything with a real recruitment inbox address

The `.gitignore` helps prevent accidental commits, but **review all files manually** before publishing. If a secret is accidentally committed, remove it from Git history and rotate/revoke the credential; deleting the file in a later commit is not sufficient.

## Import warning

The public template is **inactive by default**. Reconfigure spreadsheet/tab mappings, attach your own credentials and review data flows before enabling. Do not point a demonstration copy at real candidate data.
