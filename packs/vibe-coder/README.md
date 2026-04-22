# Vibe Coder Policy Pack

**Target audience:** Non-technical users building with AI agents (vibe coders)
**Restriction level:** Maximum — built for safety over flexibility

---

## Who is this for?

You're using an AI agent to write code for you. You don't have a deep technical background, and you want the AI to do its job without being able to accidentally (or intentionally) break things. This policy acts as a safety net: maximum protection, minimum friction.

---

## What this policy protects

| Category | Details |
|---|---|
| Credential files | `.env`, `*.key`, `*.pem`, `id_rsa`, `id_ed25519`, `credentials.json`, and more |
| Destructive commands | `rm -rf`, `sudo`, `mkfs`, `dd`, `format`, `shred`, `wipefs` |
| Package installs | `npm install`, `pip install`, `brew install`, `apt-get install`, `yarn add`, etc. |
| SSRF attacks | HTTP (unencrypted), localhost, and all private IP ranges (10.x, 192.168.x, 172.16-31.x) |
| Sensitive system paths | `/etc`, `/usr`, `/bin`, `/sbin`, `/boot`, `/root`, `~/.ssh`, `~/.gnupg` |
| SolonGate config | `~/.solongate/**` — agent cannot tamper with its own policy |
| Private directories | `**/secrets/**`, `**/private/**` |

---

## What the agent can still do

- Read and write project source files
- Run `git` commands (`git status`, `git commit`, `git push`, etc.)
- Run `node`, `npx`, and basic shell navigation (`ls`, `cat`, `grep`, `find`)
- Fetch from HTTPS endpoints (public APIs, CDNs, etc.)

---

## Test scenarios

| Scenario | Expected result |
|---|---|
| Read `.env` file | DENY |
| Read `id_rsa` key | DENY |
| Execute `rm -rf ./dist` | DENY |
| Execute `sudo apt install curl` | DENY |
| Execute `npm install lodash` | DENY |
| Fetch `http://example.com` | DENY |
| Fetch `http://localhost:3000` | DENY |
| Fetch `http://192.168.1.1` | DENY |
| Read `src/index.ts` | ALLOW |
| Execute `git status` | ALLOW |
| Execute `ls -la` | ALLOW |
| Fetch `https://api.github.com/repos` | ALLOW |

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

- To allow a specific package manager (e.g., `npx`), remove it from the `block-package-managers` denied list.
- To allow writes to a specific system path, add an explicit ALLOW rule with a lower priority number than the deny rule.
- Never delete the `allow-everything-else` rule at priority 1000 — without it, the default-deny system will block everything.
