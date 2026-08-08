# Workshop 1 — Charter (2 hours)

> `agent = model + harness`. The model predicts text. The harness is everything
> wrapped around it to turn prediction into work. The **charter** is what the
> harness consults while it does. Today you author the charter — and you promote
> its rules from prose the agent *should* follow to gates it *cannot* skip.
>
> The line is: **charter → harness → model → result.** The charter is the input
> you control. Everything downstream in this series applies it.

You won't write application code today. You will write the standard the code in
Workshops 2–4 enforces — and you'll install a real pre-commit gate and watch it
block a bad commit. A charter isn't only prose; its strongest rules have teeth.

---

## Before you arrive

Setup is not workshop time. Arrive with this done:

- Pick a repo you know well and can edit. Real conventions beat a clean slate.
- Have Claude Code working against it (`claude` in the repo root).
- `git` and a bash shell. That's the only tooling. **No Python. No API key.**
- Skim `examples/` so the target shapes are familiar.

---

## The clock

| Time | Segment |
|---|---|
| 0:00–0:10 | 0. Framing: the seam you're authoring |
| 0:10–0:35 | 1. Read an unreliable run; name the levers |
| 0:35–1:03 | 2. Layers + globs: where a rule lives, what it targets |
| 1:03–1:08 | Break |
| 1:08–1:40 | 3. The promotion ladder: guide → sensor → gate |
| 1:40–2:00 | 4. Exercise the levers: alter, re-run, diff |

Two hours, four segments, one five-minute break. Every segment ends at an
artifact you can diff against `examples/`.

---

## Segment 0 — Framing (0:00–0:10)

One idea holds the whole workshop: **the charter never runs.** It is the
specification an agent is held to. The harness — Claude Code, Cursor, a LangGraph
app — is the engine that reads it and acts.

The charter comes in four families, and today you move a rule *up* through them:

- **Guides** — judgment, written down. Prose in `CLAUDE.md`, `rules/`.
- **Tool definitions** — what the agent can reach for. `mcp.json`, skills.
- **Sensors** — tell the agent whether the work is acceptable. Tests, lint. They
  **report**; they do not refuse.
- **Gates** — **refuse**. Pre-commit hooks, branch protection.

The same rule can live at any rung. "No secrets in code" as a guide is a sentence
the agent may ignore. As a sensor it's a scan that warns. As a gate it's a
pre-commit hook that blocks the commit. Same rule, rising stakes. Choosing the
rung *is* the narrow-output lever.

> **Checkpoint 0:** on paper, list what your repo has in each family today. Most
> people find a few guides, some sensors nobody wired to refuse, and zero
> deliberate gates. That inventory is the gap you spend the next two hours closing.

---

## Segment 1 — Read an unreliable run; name the levers (0:10–0:35)

You can't author a constraint you haven't watched get violated. Break something
first.

### Exercise 1.1 — Provoke the wide run (10 min)

Give the agent a deliberately vague task, no boundary:

```text
Add a health-check endpoint.
```

No acceptance criteria. No "don't touch the existing routes." Let it run. Watch
the diff balloon: the agent reads silence as permission, finds adjacent code it
considers ugly, and cleans it up while it's there. The PR claims completeness
because the literal ask was met.

### Exercise 1.2 — Name the missing constraint (8 min)

In writing, answer three questions about that run:

1. Where did it go wider than you wanted? *(output)*
2. What did it assume because you didn't say? *(input)*
3. When something failed, what did it improvise? *(recovery)*

Those map to the **three reliability levers**, applied in order:

- **Narrow the input** *(feedforward)*: scope plus falsifiable acceptance
  criteria. A narrow input is one the agent cannot reasonably misinterpret.
- **Narrow the output** *(feedback)*: gates that reject anything out of band.
- **Narrow the recovery** *(feedback)*: the next move after a failure is known,
  not improvised.

Reliability is what survives these filters. A run that skips a lever ships work
outside the band.

### Discussion (7 min)

Around the room: one violation each, and which lever catches it. The pattern is
almost always a missing input constraint that a downstream gate would have caught.
You fix it upstream in the charter and let the harness enforce it later.

> **Checkpoint 1:** three lines for your run — the input gap, the output gap, the
> recovery gap. This is your backlog for the rest of the workshop.

---

## Segment 2 — Layers + globs: where a rule lives, what it targets (0:35–1:03)

Every rule answers two placement questions. Layer is the first; glob is the second.

### 2.1 — Layers: concentric, not stacked (11 min)

Draw the layers **concentric**, not side by side:

```text
   ORG POLICY  ⊃  TEAM  ⊃  REPO  ⊃  PERSONAL/TASK
```

Inner is contained by outer. The task can't override org policy any more than a
function rewrites the language spec. No bus, no broker — they nest.

**The litmus test** names the layer with one question asked twice:

1. Survives a **tech-stack** change? Yes → org/team. No → repo.
2. Survives a **process** change (Jira→Linear, PR→trunk)? Yes → inner. No → outer.

| Stack? | Process? | Layer |
|:---:|:---:|---|
| Yes | Yes | Org policy |
| Yes | No | Team |
| No | Yes | Repo |
| No | No | Personal / task |

Sort your existing rules into rings. Diff the shape against
`examples/charter-skeleton.md`.

### 2.2 — Globs: what the rule targets (11 min)

Layer says *where the rule lives*. A **glob** says *which files it applies to*. A
rule that fires everywhere gets tuned out; scope it to its paths and it earns
attention.

Write two or three path-scoped rule files for your repo:

```text
# rules/routes.md
---
globs: ["app/routes/**/*.py"]
---
- New routes register in app/routes/__init__.py. No routers elsewhere.
- Every route has a test asserting its status and shape.
```

Claude Code reads nested `CLAUDE.md` and globbed rules; Cursor uses
`.cursor/rules/*.mdc` with a `globs:` key. Same idea — attach the rule to the
path. Diff against `examples/rule-globs.md`.

### 2.3 — Fill the gaps (6 min)

Take your Checkpoint 1 backlog. Each gap becomes a rule; place it by layer *and*
glob. "Don't touch the status route" is repo-layer, globbed to `app/routes/**`.

> **Checkpoint 2:** a concentric skeleton plus two or three glob-scoped rule
> files, with every Segment-1 gap placed on both axes. Diff against
> `charter-skeleton.md` and `rule-globs.md` — the shape should match; the content
> is yours.

---

## Break (1:03–1:08)

---

## Segment 3 — The promotion ladder: guide → sensor → gate (1:08–1:40)

This is the heart of the rework. You take one rule and walk it up the families
until it has teeth — the narrow-output lever, made mechanical.

### 3.1 — Rung 1: the guide (5 min)

Write the rule as prose in `CLAUDE.md`:

```text
- No secrets in source. Read credentials from the environment.
```

Ask the agent to add a config that needs an API key. Watch what it does. A guide
is advice; a capable model *usually* follows it — usually is the problem.

### 3.2 — Rung 2: the sensor (7 min)

Promote it to something that *reports*. A grep, a test, a lint rule:

```shell
# a sensor: reports, does not refuse
git grep -nE '(sk-ant-|AKIA[0-9A-Z]{16})' -- '*.py' && echo "SENSOR: possible secret"
```

A sensor turns "please don't" into "here's proof you did." But it still ships —
nothing stopped the commit.

### 3.3 — Rung 3: the gate (20 min)

Promote it to something that **refuses**. Install the reference pre-commit gate:

```shell
cp examples/pre-commit .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

Read the script (`examples/pre-commit`). Three gates, each a promoted rule, each
globbed to the paths it's about:

- secrets scan on `*.py|*.yaml|*.env*` (org policy),
- routes-register-centrally on `app/routes/*.py` (repo rule),
- tests-pass (a sensor promoted to a gate).

Now try to commit a fake secret:

```shell
echo 'API_KEY = "sk-ant-fake123"' >> app/config.py
git add app/config.py && git commit -m "add config"
# → BLOCKED (charter gate): possible secret in app/config.py
```

The commit does not land. That's the difference between a sensor and a gate, and
you just felt it. **The rule that matters most is the rule you promote highest.**

> **Checkpoint 3:** one rule walked from guide → sensor → gate, and a real commit
> blocked by your pre-commit hook. Diff your one-task charter against
> `charter-with-levers.md`. Then decide which of *your* repo's rules deserve the
> top rung — most don't; the few that do are your highest-leverage gates.

---

## Segment 4 — Exercise the levers: alter, re-run, diff (1:40–2:00)

Now iterate. A charter is never finished; you sharpen it every time a run
surprises you. Change one lever at a time and watch the behavior move.

### 4.1 — Baseline with the charter (5 min)

Point the harness at your charter (`CLAUDE.md` + `rules/`, hook installed). Re-run
the Segment-1 task, now carrying scope, acceptance, and the gate. Diff against the
wide run and save it as your baseline.

### 4.2 — Alter one lever, observe (12 min)

Three quick experiments, one lever each. Change, re-run, note the delta:

1. **Loosen the input.** Delete the acceptance criteria and re-run. Watch scope
   widen. This proves the input lever was doing the work — put it back.
2. **Remove the gate.** `rm .git/hooks/pre-commit`. Ask for a change that trips
   it. Watch the bad change sail through. Reinstall. This proves output ≠ input:
   a guide the model ignored, a gate stops cold.
3. **Sharpen a glob.** Narrow a rule's glob to a single path, or widen it. Watch
   the agent's attention follow — the rule bites where the glob points.

Short on time? Run experiment 2 — the gate removal is the one you can't
un-see. The lesson lives in the diffs: each lever narrows a different band, and
you can *feel* which one you touched.

### 4.3 — The seam to Workshop 2 (3 min)

Everything you wrote is inert until a harness reads it. Today that harness was
Claude Code — a vendor's engine. In Workshop 2 you build your own and see where
each lever plugs in:

- **Narrow input** → the harness's context assembly (what it loads each turn).
- **Narrow output** → the gates and sensors it fires — your pre-commit hook, in
  code.
- **Narrow recovery** → the loop's bounds and kill switch.

Keep your charter and your hook. Workshop 2 points a real harness at them.

> **Final checkpoint:** a layered, glob-scoped charter; one rule promoted to a
> working gate; and at least one lever experiment diffed against baseline. The
> lever that still leaks is your take-home.

---

## Take-home

`take-home.md` — the full concentric org/team/repo document, the canon-to-charter
map, the layer × lever grid, and upgrading your raw git hook to the `pre-commit`
framework for a real project.
