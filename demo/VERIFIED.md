# Live Verification — GitHub Copilot CLI

Harness verified running in **GitHub Copilot CLI v1.0.65** (`copilot -p ... --allow-all-tools`).
GHCP auto-loaded `AGENTS.md` + `.agents/skills/guardrail-review`, read `CONTEXT.md` and
`ruleset.json`, ran the scanner, and returned verdicts:

| Command | Verdict | Threat class |
|---|---|---|
| `rm -rf /` | 🛑 **BLOCK** | destructive-action (HIGH) |
| `export AWS_KEY=AKIAIOSFODNN7EXAMPLE` | 🛑 **BLOCK** | credential-leakage |
| `rm -rf node_modules` | ✅ **PASS** | — (allowlist) |

> The guardrail also blocked the agent's own `echo | node pre-tool-use.js` shell attempt
> at the destructive pattern before the scanner ran — defense-in-depth as designed.

Reproduce:
```bash
copilot -p "Use guardrail-review on: rm -rf /  — run scanner, give verdict" --allow-all-tools
```
