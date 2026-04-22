## Pack name

<!-- The name of the policy pack you're adding or updating -->

## Target audience

<!-- Who is this pack for? Be specific. -->
<!-- Example: "Freelance backend developers working on Node.js APIs for multiple clients" -->

## What problem does this pack solve?

<!-- Why does this pack need to exist? What gap does it fill that existing packs don't cover? -->

## Restriction level

<!-- Choose one -->
- [ ] Maximum
- [ ] High
- [ ] Moderate
- [ ] Low

## Policy validation checklist

Go through each item before submitting. PRs missing checkboxes will not be reviewed.

- [ ] Only one constraint type per rule (`commandConstraints`, `filenameConstraints`, `pathConstraints`, or `urlConstraints`)
- [ ] `**` wildcard is always its own path segment (`**/foo/**` not `**foo/**`)
- [ ] All `effect` values are uppercase (`"ALLOW"`, `"DENY"`)
- [ ] Every rule has a `minimumTrustLevel` field
- [ ] Every rule `id` uses only lowercase letters, numbers, and hyphens (no spaces)
- [ ] `permission` arrays use correct format: `["READ", "WRITE"]` not `"READ, WRITE"`
- [ ] DENY rules have lower priority numbers than the ALLOW rules they should override
- [ ] Policy ends with an `allow-everything-else` rule at priority 1000
- [ ] `policy.json` is valid JSON (verified with `cat policy.json | python3 -m json.tool`)

## README checklist

- [ ] Includes target audience description
- [ ] Includes a "What this policy protects" table
- [ ] Includes a "What the agent can still do" section
- [ ] Includes a test scenario table with expected ALLOW/DENY results
- [ ] Includes "How to use" instructions (Dashboard and CLI)
- [ ] Includes at least 2 customization tips

## Test scenarios

<!-- List at least 5 test scenarios and their expected outcomes -->
<!-- Copy and fill in the table below -->

| Scenario | Expected result |
|---|---|
| Read `.env` file | DENY |
| Execute `rm -rf /` | DENY |
| Fetch `http://localhost` | DENY |
| Read `src/index.ts` | ALLOW |
| Execute `git status` | ALLOW |
| ... | ... |

## Additional notes

<!-- Anything else reviewers should know about the design decisions in this pack -->
