# Workshop 1 — Charter

**Author the standard that governs an agent.**

A charter is what you write. It is the specification an agent is held to: the
policy that never bends, the identity it works as, the task in front of it. The
model is non-deterministic. The charter is the fixed thing you push the model
toward. Get it right and every downstream workshop gets easier.

This workshop produces a real, layered charter for a repo you already know — not
a template you fill in later, a working document you leave with.

## Duration

3 hours, four segments. See `workshop.md`.

## Prerequisites

- You have used Claude Code (or a comparable agent) on a real repository.
- You can read a git diff and a short transcript of an agent run.
- You have a repo you know well and can edit. A side project is fine; something
  with real conventions is better.

## Dependencies

| Need | Why |
|---|---|
| Git + a text editor | The charter is text, versioned in the repo. |
| Claude Code CLI | You'll run the agent against your charter to see it bite. |
| A repo you know | The charter is *about* something. Abstract charters are useless. |

**No Python. No API key. No install.** This workshop is prose and judgment.

## What you'll leave with

- A charter for your repo, split into **concentric layers** (policy → identity → task).
- A litmus test you can apply to any new rule: which layer does this belong in?
- A mapping from the engineering canon you already trust to the charter that
  encodes it — so the charter isn't invented, it's *transcribed*.
- One run where you watched the agent obey the charter, and one where you watched
  it violate an unwritten rule — the gap that tells you what to write next.

## Learning objectives

By the end you can:

1. Read an unreliable agent run and name the missing constraint.
2. Place a rule in the right layer using the litmus test, not by feel.
3. Explain why layers are **concentric, not stacked** — and why stacking fails.
4. Distinguish what belongs in the charter from what belongs in the harness
   (the seam Workshop 2 builds against).

## Checkpoints

Each segment ends with a diffable artifact in `examples/`. If your file matches
the shape of the reference, you're on track. The *content* is yours — it's your
repo.

## Reading (optional, before)

*Harness Engineering* Ch. 4 (Charter and Harness: What Each One Is), Ch. 5 (the
boundary), Ch. 8 (the charter's layers). The workshop stands alone, but these
chapters are the source.
