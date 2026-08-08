# Charter Skeleton — Concentric Layers (reference shape)

> Checkpoint 2 target. Copy the shape, fill with YOUR repo's rules. The point is
> the rings and the litmus-test placement, not these example rules.

## Ring 1 — Org policy (survives stack AND process change)

- Every change ships with a test that asserts the new behavior.
- No secrets in source. Ever.
- Security-sensitive changes require a second reviewer.

## Ring 2 — Team / process policy (survives stack, not process)

- Work traces to a ticket. The ticket defines scope; the PR does not expand it.
- Review is PR-based; no direct pushes to `main`.

## Ring 3 — Repo (survives process, not stack — dies if we change frameworks)

- This is a FastAPI service. Routes live in `app/routes/`. Do not add a router
  outside that package.
- Prefer composition over inheritance. Justify any base class in the PR.
- Tests use `pytest`; fixtures live in `conftest.py`.

## Ring 4 — Personal / task (the volatile inner ring — one job at a time)

Rewritten every run. Here it holds the health-check task from
`charter-with-levers.md`:

- TASK: add `GET /healthz` in `app/routes/health.py`.
- SCOPE: in — the new route and its test. out — existing routes, adjacent
  refactors, new dependencies.
- RECOVERY: test fails → fix the endpoint, not the test. Out-of-scope file
  touched → revert it, then continue.

---

### Placement worksheet (how each rule got here)

| Rule | Survives stack? | Survives process? | → Ring |
|---|:---:|:---:|---|
| Ships with a test | Yes | Yes | Org |
| Traces to a ticket | Yes | No | Team |
| Routes in `app/routes/` | No | Yes | Repo |
| This task's scope | No | No | Personal |
