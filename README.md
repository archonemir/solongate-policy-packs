# SolonGate Policy Packs

**Ready-to-use security policies for AI agents**

A curated collection of [SolonGate](https://solongate.com) policies for different user profiles. Download any policy and load it directly into your SolonGate dashboard or CLI — no configuration needed.

---

## What is this?

When you run an AI agent, it can read files, execute commands, and make network requests on your behalf. Without guardrails, that agent can accidentally delete data, expose credentials, or be manipulated into attacking your own infrastructure.

SolonGate sits between your AI agent and the outside world, enforcing a policy that defines exactly what the agent can and cannot do. This repo gives you a head start with policies written for the most common use cases.

---

## Quick Start

**1. Install SolonGate**
```bash
npx @solongate/proxy@latest init
```

**2. Pick a policy pack** from the table below and download its `policy.json`

**3. Import the policy**

**Via Dashboard:**
- Go to [app.solongate.com](https://app.solongate.com) → **Policies** → **Import JSON**
- Paste the contents of `policy.json` and save

**Via CLI:**
```bash
npx @solongate/proxy@latest push --policy-id <your-policy-id>
```

---

## Available Policy Packs

| Pack | Target Audience | Restriction Level | Path |
|---|---|---|---|
| **vibe-coder** | Non-technical users using AI to write code | Maximum | [`packs/vibe-coder/`](./packs/vibe-coder/) |
| **freelancer** | Freelance developers on client projects | Moderate | [`packs/freelancer/`](./packs/freelancer/) |
| **startup** | Early-stage startup teams | Low | [`packs/startup/`](./packs/startup/) |

### Choosing a pack

```
Are you non-technical and just want AI to "vibe" and build for you?
  → vibe-coder

Are you a developer working on a client's project?
  → freelancer

Are you a startup team that values speed and has technical developers?
  → startup
```

---

## What each pack blocks

| Protection | vibe-coder | freelancer | startup |
|---|:---:|:---:|:---:|
| Credential files (`.env`, `*.key`, `*.pem`) | Yes | Yes | Yes |
| Destructive commands (`rm -rf`, `mkfs`, `dd`) | Yes | Yes | Yes (critical only) |
| SSRF attacks (HTTP, localhost, internal IPs) | Yes | Yes | Yes |
| Package manager installs | Yes | No | No |
| Production environment paths | No | Yes | No |
| Client data directories | No | Yes | No |
| Database direct access (mysql, psql, etc.) | No | Yes | No |
| System directory writes (`/etc`, `/usr`, `/bin`) | Yes | Yes | Yes |

---

## Policy Format

All policies use the standard SolonGate JSON format. Each file contains a list of `rules`, where each rule has:

- `id` — unique identifier (e.g., `"block-env-files"`)
- `effect` — `"ALLOW"` or `"DENY"`
- `minimumTrustLevel` — `"UNTRUSTED"`, `"VERIFIED"`, or `"TRUSTED"`
- `priority` — lower number = evaluated first (0 is highest priority)
- `permission` — `"READ"`, `"WRITE"`, `"EXECUTE"`, `"MOVE"`, `"DELETE"` or an array
- One optional constraint: `commandConstraints`, `filenameConstraints`, `pathConstraints`, or `urlConstraints`

For the full format reference, see the [SolonGate documentation](https://solongate.com/docs).

---

## Contributing

Have a policy pack for a specific use case (data scientists, DevOps engineers, security teams)? Submit a PR!

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

---

## License

MIT — use these policies freely in your own projects.

---

Built with [SolonGate](https://solongate.com) — AI agent security, done right.
