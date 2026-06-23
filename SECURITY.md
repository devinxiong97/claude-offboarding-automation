# Security & Data-Handling Notes

This repository is a **sanitized case study**. It documents the *design* of a real automation without exposing anything sensitive. Knowing what **not** to publish is part of the engineering.

## What is deliberately NOT in this repo

**Credentials & secrets** — never committed, and blocked by [`.gitignore`](.gitignore):
- Google service-account private keys (`oauth2service.json`)
- GAM OAuth tokens (`oauth2.txt`), `client_secrets.json`
- Slack tokens, `credentials.json`
- Any `.env`, `.pem`, or `.key` material

**Personally identifiable information (PII):**
- Real employee names, personal emails, home/return addresses, phone numbers
- Real termination or hire details

**Internal identifiers:**
- Google customer IDs, GCP project IDs, service-account emails
- Slack channel IDs, user IDs, workspace name
- Cloud routine / trigger IDs

## How secrets are handled in the real system

- Service-account keys live only on the operator's local machine at `~/.gam/`, `chmod 600`.
- Privileged Google Workspace actions (suspend, transfer) run **locally** via GAM, never from the cloud agent.
- Domain-wide delegation is scoped to the minimum APIs needed and authorized explicitly in the Admin console with **"Overwrite existing client ID"** to replace any over-broad legacy scopes.
- The cloud triage agent holds **no** admin credentials — it only reads Slack and drafts messages.

## Example data

Every name, email, domain (`example.com`), org, and ID in this repo is **fabricated** for illustration. Any resemblance to real people or systems is coincidental.
