# Workshop 1 — Take-Home: Harden the Charter

The workshop got you a charter for one task on one repo. This turns it into a
document you'd trust across a team. None of this is live-workshop material; do it
over the following week and bring the hard cases to Workshop 2.

## 1. Build the full concentric document

You sorted rules into rings. Now write each ring as its own artifact:

- **Org policy** → the outermost rules, versioned somewhere every repo inherits
  (a shared `org-charter.md`, an org-wide rules package).
- **Team** → the process- and stack-family rules.
- **Repo** → `CLAUDE.md` at the root.
- **Personal / task** → per-prompt or a scoped rules file.

Verify containment holds: no inner rule contradicts an outer one. If it does, the
inner rule is wrong or the outer rule is miscategorized. Re-run the litmus test.

## 2. The canon-to-charter map

Almost nothing in your charter is new. It's the engineering canon you already
trust — code review standards, testing discipline, security policy — transcribed
into a form the harness can apply. Make the mapping explicit:

| Canon you already follow | Where it lives in the charter |
|---|---|
| "Small, reviewable PRs" | Repo gate: diff-size ceiling |
| "Tests ship with the change" | Org policy + repo sensor + pre-commit gate |
| "No secrets in code" | Org policy gate: secret-scan pre-commit |
| … | … |

Filling this in does two things: it proves the charter isn't invented, and it
surfaces canon you follow by habit but never wrote down — exactly the unwritten
rules the agent violates.

## 3. The layer × lever grid

Two axes: rows are layers (org/team/repo/personal), columns are levers (narrow
input / narrow output / narrow recovery). Each cell is a filter scoped to that
layer. Fill it for your codebase:

|  | Narrow input | Narrow output | Narrow recovery |
|---|---|---|---|
| **Org** | acceptance-criteria template | secret-scan gate | incident runbook link |
| **Team** | ticket-to-scope rule | required CI checks | rollback procedure |
| **Repo** | rules file (paths, conventions) | pre-commit gate | "fix code not test" |
| **Personal** | per-task scope statement | local diff review | known local recoveries |

Most cells start blank. Blank cells are your reliability gaps — the places a run
can leak. Fill them as runs surprise you.

## 4. Sharpen falsifiability

Go through every acceptance criterion you wrote and ask: *could someone who isn't
me check this without asking me what I meant?* Rewrite every one that fails. This
is the single highest-leverage edit to a charter. Vague acceptance is the root of
most wide runs.

## 5. Sensor vs gate audit

List every sensor in your repo (tests, lint, types, coverage). For each, decide:
should this only report, or should it refuse? Wire the refusers into gates
(pre-commit, branch protection, required checks). A sensor nobody wired to refuse
is a rule the agent can ignore for free.

## Bring to Workshop 2

- Your falsifiable acceptance criteria for one task — the harness will assemble
  them into context.
- Your sensor/gate list — the harness will fire them.
- Your recovery rules — the harness loop will encode them as bounds and retries.

The charter is the input. Next you build the engine that acts on it.
