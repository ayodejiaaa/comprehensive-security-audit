# Finding format

Require this from every domain subagent, and use it yourself when writing the final report. Consistency here is what lets you merge nine agents' output into one coherent document without rewriting everything from scratch.

## Per-finding fields

- **Title** — a one-line description of the specific defect, not the general category ("Payout account name check compares against a mutable, unverified profile field", not "Weak validation").
- **Severity** — Critical / High / Medium / Low / Informational. Calibrate on realistic impact and exploitability together, not on how the bug sounds. A theoretical issue that needs three cooperating conditions to matter is not Critical just because the affected system is money.
- **Affected component** — exact file path and line number(s) (or route/endpoint name if line numbers would go stale immediately, e.g. generated code). If several files matter, list the primary one first.
- **Description** — what is actually wrong, in plain terms.
- **Evidence** — a short code excerpt (a handful of lines, not the whole file) that shows the defect. No finding should exist without this. If an agent can't quote the exact code, it hasn't confirmed the finding yet.
- **Attack scenario** — a concrete, step-by-step account of how a realistic attacker (or an ordinary buggy client) would exploit this, given the actual auth/role model of the app. Not "an attacker could potentially..." — walk the steps.
- **Impact** — what the attacker or the business actually loses if this is exploited.
- **Exploitability** — prerequisites and difficulty (does it need a valid session? Two colluding accounts? A race window? Or is it a single unauthenticated request?).
- **Recommended remediation** — what the team should change, concrete enough to act on, but this is the audit's only opinionated part — the agent should not implement it.
- **Verification method** — how a developer can confirm, after a fix, that the issue is actually closed (a specific test, a specific request to try, a specific query to run).
- **CONFIRMED vs PLAUSIBLE** — CONFIRMED means the agent traced the exact code path and is certain. PLAUSIBLE means the reasoning is sound but something (runtime behavior, an external service's actual behavior, environment configuration the agent can't see) couldn't be verified statically. Never silently upgrade a PLAUSIBLE to a CONFIRMED claim when synthesizing the final report — carry the label through, and say what would resolve the uncertainty.

## Also require: positive controls

Every agent should list, briefly, the controls in its domain that are working correctly — atomic operations it traced and found race-safe, authorization checks it tried to break and couldn't, sanitization that held up. This isn't padding: it's what lets the final report be calibrated (a report of only bad news from an agent that clearly didn't try very hard to find good news is a report you should be suspicious of), and it's often useful information for the remediation team so they don't accidentally break something that already works.

## Avoiding false positives

Findings should not include:
- Generic best-practice suggestions with no demonstrated impact in this codebase ("you should add more logging" is not a finding).
- Anything the agent couldn't back with a file:line and an excerpt.
- Frontend-only concerns where the agent confirmed a proper backend check exists (note it as a positive control instead).
- Issues already mitigated elsewhere in the architecture that the agent didn't notice interacts with this one — cross-check before reporting duplicates from different agents as separate findings when synthesizing.

When genuinely uncertain whether something is exploitable, report it as PLAUSIBLE with the uncertainty stated, rather than omitting it or inflating its certainty.
