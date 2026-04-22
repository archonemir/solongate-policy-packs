# Startup Policy Pack

**Target audience:** Early-stage startup teams with multiple developers
**Restriction level:** Low — speed-first with essential guardrails

---

## Who is this for?

Your team moves fast. You use AI agents to accelerate development, and your developers are technical enough to know what they're doing. This policy applies only the essential safety rails: it prevents catastrophic data loss, protects credential files, and blocks SSRF attacks — and gets out of the way for everything else.

---

## What this policy protects

| Category | Details |
|---|---|
| Credential files | `.env`, `.env.production`, `*.key`, `*.pem`, `id_rsa`, `credentials.json`, `service-account.json` |
| Catastrophic commands | `rm -rf /`, `rm -rf /*`, `rm -rf ~`, `mkfs`, `dd if=` on devices, fork bombs |
| System directory writes | Write/delete access to `/etc`, `/usr`, `/bin`, `/sbin`, `/boot`, `/root` |
| SSRF attacks | HTTP (unencrypted), localhost, and all private IP ranges |

---

## What the agent can still do

- Run any package manager: `npm`, `pip`, `brew`, `apt`, `yarn`, `pnpm`, `cargo`, `go`
- Run `git`, `node`, `python`, `ruby`, `go`, and virtually any dev tool
- Read system directories (for debugging and introspection)
- Access wide parts of the filesystem
- Fetch from HTTPS endpoints

---

## Test scenarios

| Scenario | Expected result |
|---|---|
| Execute `rm -rf /` | DENY |
| Execute `rm -rf /*` | DENY |
| Execute `mkfs.ext4 /dev/sda` | DENY |
| Read `.env.production` | DENY |
| Write to `/etc/hosts` | DENY |
| Fetch `http://localhost:8080` | DENY |
| Fetch `http://10.0.0.1` | DENY |
| Execute `rm -rf ./node_modules` | ALLOW |
| Execute `sudo apt install build-essential` | ALLOW |
| Execute `brew install postgresql` | ALLOW |
| Execute `pip install pandas` | ALLOW |
| Read `src/index.ts` | ALLOW |
| Fetch `https://api.github.com` | ALLOW |

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

- As your team grows, consider migrating to the `freelancer` or `vibe-coder` pack for more granular control.
- To protect specific paths (e.g., your production config directory), add a DENY rule with `pathConstraints` at a priority lower than 1000.
- To restrict `sudo` (currently allowed), add a new rule at priority 50 with `commandConstraints.denied: ["sudo*"]`.
- Never delete the `allow-everything-else` rule at priority 1000 — without it, the default-deny system will block everything.
