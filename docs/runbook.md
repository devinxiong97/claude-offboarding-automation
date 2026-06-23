# On/Offboarding Runbook (Example)

> Fabricated example data throughout. Real runbook substitutes live org names and tooling.

## Roles

- **HR** submits structured requests via Slack workflow forms.
- **IT/ops** (the operator) reviews agent action-cards and executes privileged steps.
- **AI agent agent** detects, parses, triages, and drafts — never executes destructive steps.

---

## Onboarding

**Trigger:** a new-hire form posted to `#newhires-<org>`.

Example form:
```
New Hire Name: Jane Doe
Title: Senior Backend Engineer
Hire Type: Contractor
Location: Remote — Portugal
Start Date: 2026-07-01
Manager: Alex Rivera
Company: OrgA
Personal Email: jane.personal@example.com
Create Google Account: Yes
Invite To Slack: Yes
Send Welcome Email: Yes
```

**Agent produces an action-card:**
1. Parsed summary + ⚠️ flags for any missing required fields (e.g. department, manager email).
2. A drafted welcome email personalized with name + start date.
3. ⚠️ flag if start date is today/past.

**Operator executes:**
1. Create Google account (`gam create user …`).
2. Invite to Slack (admin UI or SCIM where available).
3. Send the welcome email.

---

## Offboarding

**Trigger:** an offboarding form posted to `#offboarding`.

Example form:
```
Employee Name: John Smith
Company: OrgA
Termination Date: 2026-06-30
Device: MacBook Pro 14"
Remove Access to Google: Yes
Remove Access to Slack: Yes
Google Account Deletion (requires written approval): Hold
Drive transfer to: Alex Rivera
```

**Agent produces an action-card:**
- Termination date (⚠️ if today/past)
- Access-removal checklist
- Device-recovery + return-address line
- Drive-transfer recipient (or ⚠️ "not specified")
- ⚠️ **APPROVAL REQUIRED** banner if deletion = Yes

**Operator executes — in this order (never delete first):**
1. **Suspend** Google account (reversible access-cut):
   ```
   gam update user john.smith@example.com suspended on
   ```
   > Confirm the exact `primaryEmail` first — watch for look-alike accounts.
2. **Archive Drive** into the Shared Drive (before any deletion):
   ```
   gam user john.smith@example.com transfer drive <recipient> ...
   ```
3. **Slack:** deactivate the seat (manual on standard Business plan; rides on Google SSO if enabled).
4. **Deletion** (only with written approval) — always manual, never automated.

## Guardrails

- Destructive actions require explicit target confirmation.
- Look-alike account detection before suspend/delete (e.g. `j.doe@` vs `j.doer@`).
- Reversible-first: suspend over delete.
- Capability honesty: steps that aren't automatable on the current plan are documented as manual, not faked.
