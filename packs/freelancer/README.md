# Freelancer Policy Pack

**Target audience:** Freelance developers working on client projects
**Restriction level:** Moderate — productive but client-safe

---

## Who is this for?

You're a developer working on multiple client projects. You need full development tool access (git, npm, node) but must ensure your AI agent cannot touch production environments, client data directories, or database internals. This policy gives you the freedom to build fast while protecting the things clients care about most.

---

## What this policy protects

| Category | Details |
|---|---|
| Credential files | `.env`, `*.key`, `*.pem`, `id_rsa`, `credentials.json`, `service-account.json`, and more |
| Production paths | `**/prod/**`, `**/production/**`, `**/prod-*/**` |
| Client data | `**/client-data/**`, `**/customer-data/**` |
| Destructive commands | `rm -rf`, `sudo`, `mkfs`, `dd`, `format`, `shred` |
| Database direct access | `mysql`, `psql`, `mongodump`, `redis-cli` with connection flags |
| SSRF attacks | HTTP, localhost, and all private IP ranges |
| Sensitive system paths | `/etc`, `/usr`, `/bin`, `~/.ssh`, `~/.gnupg`, `**/secrets/**` |

---

## What the agent can still do

- Run `git`, `npm`, `node`, `npx` freely
- Read and write project source files
- Install packages (`npm install`, `yarn add`, etc.)
- Run build and test scripts
- Fetch from HTTPS endpoints
- Access staging and development environments

---

## Test scenarios

| Scenario | Expected result |
|---|---|
| Read `.env` file | DENY |
| Read files in `**/prod/**` | DENY |
| Read files in `**/client-data/**` | DENY |
| Execute `mysql -h prod.db.example.com -u root -p` | DENY |
| Execute `psql --host=prod-db --username=admin` | DENY |
| Execute `rm -rf ./` | DENY |
| Fetch `http://192.168.1.100` | DENY |
| Execute `npm install` | ALLOW |
| Execute `git push origin main` | ALLOW |
| Read `src/components/Button.tsx` | ALLOW |
| Fetch `https://api.stripe.com/v1/charges` | ALLOW |
| Execute `npx prisma migrate dev` | ALLOW |

---

## How to use

### Dashboard
1. Go to [app.solongate.com](https://app.solongate.com) → **Policies** → **Import JSON**
2. Paste the contents of `policy.json`
3. Save and assign to your agent

### CLI
```bash
npx @solongate/proxy@latest push --policy-id <your-policy-id>
```

---

## Customization tips

- To add additional client-specific protected paths, add them to the `block-production-paths` rule's `pathConstraints.denied` array.
- To allow read-only access to production (e.g., for debugging), create a new ALLOW rule at a lower priority number than `block-production-paths` (priority 25) with `permission: "READ"` and the specific path.
- To block additional database tools, add them to the `block-database-direct-access` rule's `commandConstraints.denied` array.
