# Contributing to SolonGate Policy Packs

Thank you for wanting to contribute. This repo is built for the community — the more high-quality packs it contains, the more useful it becomes for everyone using AI agents.

---

## What makes a good policy pack?

A good pack targets a **specific user profile** with a **clear security philosophy**. It should be:

- **Purposeful** — written for a real, identifiable audience (e.g., "data scientists running Jupyter notebooks", "DevOps engineers managing infrastructure")
- **Correct** — zero format errors, all rules valid per the SolonGate policy spec
- **Documented** — includes a `README.md` explaining who it's for, what it blocks, and what it allows
- **Testable** — includes a test scenario table showing expected ALLOW/DENY outcomes

---

## Policy format rules (critical — read before writing)

Before submitting a policy, verify against this checklist:

- [ ] Only **one constraint type** per rule (`commandConstraints`, `filenameConstraints`, `pathConstraints`, or `urlConstraints` — never two in the same rule)
- [ ] `**` wildcard is always its own path segment: `**/secret.txt` ✓ — `**secret.txt` ✗
- [ ] All `effect` values are uppercase: `"ALLOW"` or `"DENY"` (not `"allow"` or `"deny"`)
- [ ] Every rule has a `minimumTrustLevel` field
- [ ] Every rule has a unique `id` with no spaces or special characters (hyphens only)
- [ ] `permission` arrays are formatted correctly: `["READ", "WRITE"]` not `"READ, WRITE"`
- [ ] DENY rules have **lower** priority numbers than ALLOW rules they override
- [ ] The policy ends with an `allow-everything-else` rule at priority 1000

---

## Directory structure for a new pack

```
packs/
└── your-pack-name/
    ├── policy.json   ← The policy (required)
    └── README.md     ← Documentation (required)
```

Name your directory with lowercase letters and hyphens only (e.g., `data-scientist`, `devops-engineer`).

---

## README.md requirements for your pack

Your pack's `README.md` must include:

1. **Pack name and one-line description**
2. **Target audience** — who specifically is this for?
3. **Restriction level** — Maximum / High / Moderate / Low
4. **What it protects** — a table of protected categories and details
5. **What the agent can still do** — what's explicitly allowed
6. **Test scenarios** — a table of test cases with expected ALLOW/DENY results
7. **How to use** — Dashboard and CLI import instructions
8. **Customization tips** — at least 2-3 tips for adapting the policy

---

## Submitting a pull request

1. Fork this repository
2. Create a branch: `git checkout -b add-pack-your-pack-name`
3. Add your pack directory under `packs/`
4. Validate your `policy.json` is valid JSON (`cat policy.json | python3 -m json.tool`)
5. Open a PR using the PR template — fill out every section
6. One of the maintainers will review your policy for correctness and completeness

---

## What we will not merge

- Packs that are too similar to existing ones without a clearly differentiated use case
- Policies with format errors (wrong constraint usage, missing fields, bad priorities)
- Policies that are dangerously permissive without clear justification
- Packs without a `README.md`
- Packs with `README.md` that lack test scenarios

---

## Questions?

Open an issue with the label `question` and we'll help you out.
