# Final report skeleton

Write the delivered report in this shape. Section order matters — a reader (often a separate remediation team, not the person who commissioned the audit) should be able to stop reading after the executive summary and risk table and already know whether this is urgent.

```markdown
# [App name] — Security Audit Report [ "— Round N" if a re-audit ]

**Type:** Read-only security audit. No source, configuration, dependency,
database, secret or deployment was changed, and nothing was committed.
**Date:** [date]
**Target:** [repo path / branch / commit or version]
**Method:** [N] focused subagent passes across [list the domains], plus
direct verification of the highest-impact claims by [you]. [Tools used for
verification, e.g. npm audit, npm ls]. No .env.local or secret-bearing file
was opened; no secret values appear in this report.
**Independently verified:** [the specific claims you personally re-checked
by reading code, not just trusting an agent]
**Limits of this audit:** static review only — anything runtime-dependent
is marked PLAUSIBLE, not CONFIRMED. [Note anything you genuinely couldn't
see, e.g. a hosting dashboard, live environment variables, a third party's
actual API behavior.]

---

## 1. Executive Summary
Lead with the real risk, in plain language, in 3-6 sentences. Name the
worst realistic thing that could happen and roughly how hard it would be.
Don't bury this under a list of every informational finding.

## 2. Status of prior findings  [only for a re-audit]
A table: prior finding | status now (Fixed / Partially fixed / Not fixed) |
evidence. Also correct anything the prior report got wrong.

## 3. Architecture Overview
The actors, the core workflow, what changed since a prior audit (if any),
and the actual trust boundaries in this system. Explicitly name what's N/A
for this stack (e.g. "no RLS — this is not a Supabase app; authorization is
entirely in application code") so a reader doesn't wonder if you forgot it.

## 4. Risk Summary
A table: Severity | Count. (Critical/High/Medium/Low/Informational)

## 5. Critical Attack Paths
2-5 short narratives chaining findings the way a real attacker would use
them together, not just restating individual findings. This is often the
most useful section — it's what makes "17 medium findings" feel either
alarming or manageable.

## 6. Detailed Findings
Grouped by severity (Critical first), each using the exact per-finding
format from finding-format.md. Number or key them so section 9 can
reference them.

## 7. Positive Security Controls
What's already working, pulled from every agent's positive-controls list
plus anything you confirmed yourself. Specific, not generic ("all N admin
routes call requireAdmin() as the first operation, verified in every file"
beats "authorization seems fine").

## 8. Security Gaps (systemic themes)
Cross-cutting patterns that produced multiple individual findings — e.g.
"rate limiting fails open whenever its backing store is unconfigured, which
is the root cause behind findings #4, #9, and #15" is more useful to a team
than three separate write-ups that don't connect the dots.

## 9. Remediation Priority
An ordered list/roadmap, grouping related findings so a team can tackle
them together rather than working strictly by severity number (e.g. "fix
the payout name-check and the account-change flow together, they're the
same underlying gap").

## 10. Final Security Gate
A short set of direct yes/no questions **tailored to this app's actual
business model** — don't ask about admin bypass if there's no admin role,
do ask about double-spend if there's a wallet. Answer each in 1-3 sentences
with a pointer to the relevant finding number. End with an explicit
production-readiness verdict: ready, ready-with-conditions (name them), or
not ready (name the blockers).

---
*This report is a discovery and documentation deliverable for a separate
remediation effort. No fixes, migrations, configuration changes,
deployments or commits were made during this engagement.*
```

## Delivery

Save this as a markdown file and send it with the environment's file-delivery
tool rather than pasting it into the chat. In the chat itself, give a short,
human summary: overall posture in one line, the severity counts, and the
handful of findings that actually matter — not a table of contents for the
file. If a finding is genuinely urgent (an unauthenticated critical, a fix
that takes five minutes), say so plainly rather than letting it wait for the
reader to reach section 6.
