---
name: change-workflow
description: Mandatory for every request that changes the repo - a feature, a fix, a refactor, copy, config or docs, however small. The one fixed path from "I want X changed" to the PR - orient, questions one at a time, approaches, spec, plan, execute, PR, report - so every change goes the same way and ends with the same report. Invoke before answering such a request.
---

# The change workflow

One path for every change request. Depth scales with the change (a one-line fix gets a four-line
spec and a one-task plan); the steps and their order do not. Announce each step in one line,
`Step N — <name>`, and say nothing else about process.

**Hard gate:** no code, no scaffolding, no "quick fix first" until the spec is approved (Step 3).
Issue bookkeeping is not code.

## The project profile

Everything project-specific comes from the `## Change workflow profile` section of the repo's
`AGENTS.md`. Read it before Step 0. It names:

- **Read first**: the paths to read before asking anything (route tables, modules, data layers…).
- **Surfaces**: the places a user meets the change (web app, phone app, admin console, CLI, API…).
- **Interface checklist**: the ordered steps for adding or changing a route, command or contract,
  or `None`.
- **Gates**: the commands that must pass before a PR, and when each one applies.
- **Release kinds**: how a merged PR reaches users, one line each. `None` is always one of them.
- **Specs / Plans**: where specs and plans live (default `docs/specs/`, `docs/plans/`).
- **After an issue change**: a command to run after every issue edit (a board sync…), or `None`.
- **Agent label**: the `agent: <name>` label this agent puts on issues it holds.

No profile: say so in one line, offer to run the `setup` skill of this plugin, and until then use
the defaults above, `npm test` / `make test` / whatever the repo's README names as its check, and
the release kinds `Deploy` and `None`. Never invent a gate the repo does not have.

## Step 0 — Orient

- `gh issue list --label "DO NOW"` first: a free one goes before everything else. Then the
  request's issue, or open one (exactly one `priority:` label, one type label), and claim it:
  assignee, `status: claimed`, the profile's agent label. Run the profile's after-issue-change
  command.
- An issue assigned to someone other than the identity your commits carry is theirs: comment, do
  not start it.
- Read before asking: the profile's read-first paths that the request touches, the newest spec and
  plan on the same area, and `git log` on those files.
- `git fetch origin && git switch -c <type>/<topic> origin/<default branch>`.

## Step 1 — Questions, one at a time

One question per message, with `AskUserQuestion` where it exists (a numbered question otherwise):
2–4 options, the recommended one first and marked `(Recommended)`, one sentence on why. Open-ended
only when the options would be invented.

In this order, skipping any the request already answers:

1. Purpose — what is true for the user when this is done?
2. Scope boundary — what stays out?
3. Surface — which of the profile's surfaces, and which roles?
4. Data and interfaces — a new field, route or command, or existing ones; who may call it; what it
   refuses?
5. Edge cases the owner cares about — refusals, concurrency, offline, realtime, permissions.
6. Success check — what the owner will do to see it works.

Stop when no answer would change the design. Keep every question, its options and the choice; they
become the spec's Decisions table. A fork the request leaves open, or a change that is hard to
reverse (a delete, a schema or contract change, rewriting a convention other work depends on), is
one of these questions, never an assumption. If the request is several independent pieces, say so,
split it, and run this workflow on the first piece.

## Step 2 — Approaches

Two or three, each with the files it touches, whether it needs a migration or a contract change,
its release kind (from the profile) and its risk. Recommendation first, one sentence on why. One
`AskUserQuestion` to pick.

## Step 3 — Spec

Write `<specs>/YYYY-MM-DD-<topic>-design.md` from `<specs>/_template.md`, every section filled or
marked `None`; no TBD. Self-review: placeholders, contradictions between sections, scope beyond
Step 1, every decision present. Commit `docs(specs): <topic> design (#n)` — the first edit, so
`status: in-progress` and the after-issue-change command now — push, and ask the owner to review
it. Revise until approved. If the owner cannot answer now: `needs-decision` on the issue with the
approval question (`gh issue comment`), draft the plan if it helps, write no code, and pick up
another issue; the gate reopens when the answer lands.

## Step 4 — Plan

Write `<plans>/YYYY-MM-DD-<topic>.md` from `<plans>/_template.md`:

- **File structure** first: every file created or modified, one line each on what it is for.
- **Interface checklist**: one block per new or changed route, command or contract, in the
  profile's order. Delete the section when nothing of the kind changes or the profile says `None`.
- **Tasks**: each self-contained, with its files, the failing test, the code, the exact commands with
  their expected output, and its commit message. Code steps show the code. Never "similar to Task
  N", never "add validation", never a name no task defines.
- **Verification**: the profile's gates that apply to this change, and the owner's success check
  from Step 1.

Self-review against the spec: every spec section maps to a task; names and types agree across
tasks. Commit `docs(plans): <topic> (#n)`, push, and link both in the issue
(`gh issue comment <n> --body "Spec: <path> · Plan: <path>"`).

## Step 5 — Execute

Tasks in order. Per task: write the test, run it red, implement, run it green, tick the plan's
boxes, commit with the plan's message, push. Do not pause between tasks for approval. Stop only on
a blocker — a gate that fails for a reason you cannot explain, code that contradicts the plan, a
decision the spec does not cover — and say exactly what is blocked. A deviation from the plan is
written under its "Deviations" heading before you continue. Never weaken a gate to get green.

## Step 6 — Finish

Run the plan's verification list and keep the real output. Open the PR from
`.github/pull_request_template.md`, every section filled, `## Spec and plan` with both paths,
`## Release` exactly one of the profile's kinds, `Closes #n`. `status: in-review`, then the
after-issue-change command. Merge and ship only when the owner says.

## Step 7 — Report

Always this shape, nothing before it, nothing after it:

```
Done: <one sentence: what is true now>
PR: <url> · Release: <one of the profile's release kinds>
Spec: <specs>/<file> · Plan: <plans>/<file>
Verified: <gate> → <result>; <gate> → <result>
Left out: <what and why> | none
Next: <what the owner does now: review the PR, merge, ship, answer X>
```
