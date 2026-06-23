# Google Workspace Admin (GAM) Command Patterns

The privileged execution layer uses [GAM7](https://github.com/GAM-team/GAM), run **locally** where credentials live. Examples use fabricated emails.

## Setup (one-time, per org)

GAM authenticates two ways:
- **Admin OAuth** (`oauth2.txt`) — for directory operations like suspend/restore.
- **Service account + domain-wide delegation** (`oauth2service.json`) — for user-data operations like Drive transfer (impersonation).

Verify delegation (read-only):
```bash
gam user admin@example.com check serviceaccount
```
If scopes show `FAIL`, GAM prints an Admin-console link. Authorize with **"Overwrite existing client ID"** to replace any over-broad legacy scopes with the correct minimal set.

## Offboarding operations

```bash
# Look up + disambiguate before acting (watch for look-alikes!)
gam print users query "familyName:Smith" fields primaryEmail,fullname,suspended,lastLoginTime

# Suspend (reversible access-cut)
gam update user john.smith@example.com suspended on

# Restore if needed
gam update user john.smith@example.com suspended off

# Verify
gam info user john.smith@example.com fields primaryEmail,suspended

# Archive Drive (requires working service account / DwD)
gam user john.smith@example.com transfer drive alex.rivera@example.com
```

## Safety conventions

- **Always** disambiguate `primaryEmail` before a destructive command — look-alike accounts (e.g. `j.doe@` vs `j.doer@`) are a real hazard.
- Prefer **suspend** (reversible) over **delete**.
- **Transfer Drive before any deletion** — deletion is irreversible and gated on written approval.
- Multi-org: each org has its own GAM profile / project; never cross-target.
