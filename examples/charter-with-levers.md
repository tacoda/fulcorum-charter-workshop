# Charter With Levers — One Task (reference shape)

> Checkpoint 3 target. The same task, run through all three levers, each placed in
> the right layer. This is what you point the harness at in Workshop 2.

## The task

Add a health-check endpoint.

---

## Lever 1 — Narrow the input (feedforward: set before the agent acts)

**Scope**
- IN: add `GET /healthz` in `app/routes/health.py`.
- OUT: do not modify existing routes. Do not refactor adjacent code. Do not add
  dependencies.

**Falsifiable acceptance criteria**
- `GET /healthz` returns `200` with body `{"status": "ok"}`.
- A test in `tests/test_health.py` asserts status and body.
- `git diff --stat` shows only `app/routes/health.py` and `tests/test_health.py`
  (plus a one-line router registration).

## Lever 2 — Narrow the output (feedback: reject out-of-band work)

| Filter | Sensor or gate | Layer |
|---|---|---|
| `pytest tests/test_health.py` passes | Sensor | Repo |
| Diff touches only the two expected files | Gate (fail the run if not) | Task |
| `ruff` + `mypy` clean | Sensor now, gate in CI | Repo/Org |
| No secret introduced | Gate (pre-commit) | Org |

## Lever 3 — Narrow the recovery (feedback: known next move, not improvised)

- If the test fails → fix the endpoint, not the test.
- If `:8000` is taken when running the app → stop and report; do not pick a new
  port.
- If a file outside the two expected changes → revert that file, then continue.
- If acceptance can't be met in the scope → stop and ask; do not widen scope.

---

### Why this bites

The Segment-1 run went wide because the input was silent, so the agent read
silence as permission. Here the scope names what's out, the acceptance is
checkable by someone who isn't you, the output gate refuses the balloon diff, and
the recovery rules remove every "the helpful thing is to improvise" branch. The
model is still non-deterministic. The band it can land in is now narrow.
