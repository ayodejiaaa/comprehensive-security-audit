# Comprehensive Security Audit

A Claude Code skill that runs a deep, read-only, multi-agent security audit of a codebase.

Instead of starting from a fixed checklist ("check auth, check XSS, check SQLi"), it first maps the app's actual business model and trust boundaries — who the actors are, what the core workflow is, where money or trust actually changes hands. From that it derives a domain split (authentication, authorization/IDOR, admin/privilege, data exposure, config/secrets — plus whatever the app actually has: payments, escrow, KYC, marketplace bidding, multi-tenant isolation, Nigeria Data Protection Act exposure, and more), dispatches a parallel subagent per domain, independently re-verifies the highest-severity claims against the real source, and synthesizes everything into one structured report.

It never modifies, fixes, or commits anything. Discover, validate, document, report — that's the whole job.

## What it does

- **Recon first.** Reads the manifest, git state, architecture docs, and route/schema shape to build a mental model before deciding what to look for.
- **Domain split, not a template.** A small CRUD app might get 4 subagents; a multi-sided marketplace with funding, disputes, and payouts might get 9+. The domain catalog (`references/domain-catalog.md`) has a baseline every app needs plus a menu of conditional domains keyed to signals (money movement → financial-flow domain; KYC → identity/PII domain; Nigerian user base → NDPA compliance domain; etc.).
- **Parallel, evidence-based subagents.** Each domain agent gets the read-only rules, exact business context, exact file scope, the attack classes to check, and a strict per-finding format (file:line, code excerpt, attack scenario, impact, exploitability, remediation, verification method, and a CONFIRMED vs PLAUSIBLE label).
- **Verification pass.** High/Critical findings and anything surprising get re-checked directly against the source before they make it into the report — agents summarize what they *intended* to find, not always what's true.
- **Re-audit aware.** If this is a follow-up to a prior audit, every previous finding gets an explicit FIXED / PARTIALLY FIXED / NOT FIXED pass with fresh evidence.
- **One synthesized report**, delivered as a file: executive summary, architecture overview, severity table, chained attack-path narratives, findings, positive controls (what's already working), systemic gaps, remediation priority, and a final yes/no security gate tailored to that app's actual business model.

## Install

In a Claude Code session:

```
/plugin marketplace add ayodejiaaa/comprehensive-security-audit
/plugin install comprehensive-security-audit@comprehensive-security-audit
```

Or from a shell:

```bash
claude plugin marketplace add ayodejiaaa/comprehensive-security-audit
claude plugin install comprehensive-security-audit@comprehensive-security-audit
```

Once installed, check it's there with `/plugin list`, or just type `/` in a session and look for `/comprehensive-security-audit:comprehensive-security-audit`.

### Alternative: no plugin system, just the skill

If you'd rather not use the plugin/marketplace system, copy the skill folder directly:

```bash
git clone https://github.com/ayodejiaaa/comprehensive-security-audit
cp -r comprehensive-security-audit/skills/comprehensive-security-audit ~/.claude/skills/
```

This makes it available in every project on your machine, but you won't get `/plugin update`, namespacing, or a pinned version — you'll need to `git pull` and re-copy to update.

## Use it

Ask for a security audit, security review, penetration test, vulnerability assessment, "is this app secure", or a pre-launch security check — on the whole app or one narrow area (it still maps trust boundaries first even if you only asked about one). For a quick single-PR diff review or chasing one already-known bug, use a lighter code-review pass instead — this skill is for a full engagement.

## Extending the domain catalog

`skills/comprehensive-security-audit/references/domain-catalog.md` is a menu, not a fixed list. If you find yourself auditing the same kind of app-specific subsystem repeatedly (a particular regulatory regime, a bespoke matching engine, whatever), add a row: the signal that should trigger it, the domain name, and the core checks. Same shape as the existing rows.

## License

MIT — see [LICENSE](LICENSE).
