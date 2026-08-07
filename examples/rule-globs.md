# Glob-Scoped Rules (reference shape)

A rule has two placement questions:

1. **Which layer?** (org / team / repo / personal) — the litmus test. *Where the
   rule lives.*
2. **Which files?** (a glob) — *what the rule applies to.*

A rule that fires everywhere is often a rule that fires nowhere — the agent tunes
out a wall of universal "always" statements. Scope rules to the paths they're
about, and each rule earns its attention.

## The pattern

Most harnesses read path-scoped rules. Claude Code reads nested `CLAUDE.md` files
and rules with globs; Cursor uses `.cursor/rules/*.mdc` with a `globs:` front
matter key. The idea is identical: attach the rule to the path.

```
# rules/routes.md
---
globs: ["app/routes/**/*.py"]
---
- New routes register in app/routes/__init__.py. Do not create routers elsewhere.
- Every route has a test in tests/routes/ asserting its status and shape.
```

```
# rules/migrations.md
---
globs: ["migrations/**"]
---
- Migrations are append-only. Never edit a shipped migration; add a new one.
- Every migration has a tested rollback.
```

```
# rules/secrets.md   (org policy — narrow glob, high stakes)
---
globs: ["**/*.py", "**/*.env*", "**/*.yaml"]
---
- No literal secrets. Read from the environment. This is a gate (see pre-commit).
```

## Why globs sharpen the levers

- **Narrow input:** the agent editing `migrations/003.sql` sees the migration
  rules and not the frontend rules. Less noise, tighter interpretation.
- **Narrow output:** the glob tells the *gate* what to check. The secret-scan
  pre-commit hook runs on exactly the globbed paths.

## Placement worksheet

| Rule | Layer (litmus) | Glob (what it targets) |
|---|---|---|
| Routes register centrally | Repo | `app/routes/**/*.py` |
| Migrations append-only | Repo | `migrations/**` |
| No secrets | Org | `**/*.py`, `**/*.env*` |
| This task's scope | Personal | the task's named files |
