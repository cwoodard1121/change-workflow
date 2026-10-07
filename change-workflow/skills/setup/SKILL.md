---
name: setup
description: Set a repository up for the change-workflow skill - writes the project profile and the workflow and issue rules into AGENTS.md, the session rules into CLAUDE.md, the spec, plan and PR templates, and creates the GitHub labels. Use when the user asks to set up, install, add or update the change workflow in a project, or when change-workflow finds no profile.
---

# Set up the change workflow in this repo

Installing the plugin gives every session the `change-workflow` skill; it does not put anything in
the repo. This skill does, once per repo, and is safe to run again to update.

The templates are in `templates/` beside this file (the skill's base directory is given when it
loads). Announce each step in one line, `Setup N — <name>`.

## Setup 1 — Read the repo

Before asking anything, read: `AGENTS.md`, `CLAUDE.md`, `README*`, the manifest and its scripts
(`package.json`, `Makefile`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `justfile`…), `.github/`
(workflows, existing PR template), `docs/`, `gh repo view --json nameWithOwner,defaultBranchRef`,
`gh label list --limit 200`, and `git config user.name`.

From that, draft every profile field:

| Field | Draft it from |
| --- | --- |
| Read first | where routes, modules, the data layer and the screens live; existing route or API docs |
| Surfaces | the apps in the repo (web, phone, admin, CLI, public API, library) |
| Interface checklist | how a route or command is added today: inventory files, codegen scripts, docs tables; `None` when there is no such sequence |
| Gates | the check / lint / typecheck / test scripts, and when each applies (e.g. integration only when the database layer changed) |
| Release kinds | how the repo ships: deploy scripts, store builds, package publish, over-the-air; always ending with `None — docs, tests, tooling` |
| Specs / Plans | `docs/specs` / `docs/plans` unless the repo already keeps them elsewhere |
| After an issue change | a script that syncs issues somewhere (a board, a dashboard); else `None` |
| Agent label | `agent: <git user.name>-claude` |

## Setup 2 — Confirm the profile

Show the drafted profile as one block. Then ask, one `AskUserQuestion` at a time, only about the
fields you could not read with confidence — typically release kinds and gates. Recommended option
first. Never invent a command the repo does not have; a field you cannot fill is `None` with a note.

## Setup 3 — Write the files

Show the list of files you will create or change and what happens to each, then write:

1. **`AGENTS.md`** — insert `templates/agents-block.md` with the `{{…}}` placeholders filled. If
   the file already holds a `<!-- change-workflow:start -->` … `<!-- change-workflow:end -->`
   block, replace only that block and carry over the profile values the owner had edited. No
   `AGENTS.md`: create one with a two-line description of the project above the block.
2. **`CLAUDE.md`** — the same with `templates/claude-block.md`. Existing content outside the block
   is never touched; a rule there that contradicts the block is pointed out to the owner, not
   rewritten.
3. **`<specs>/_template.md`** from `templates/spec.md`, **`<plans>/_template.md`** from
   `templates/plan.md`, **`.github/pull_request_template.md`** from
   `templates/pull_request_template.md`. Replace `<specs>` and `<plans>` with the profile's paths.
   A file that already exists and differs: show the difference and ask before replacing it.

## Setup 4 — Labels

Create or update the labels (`--force` updates one that exists, so this is safe to repeat):

```
gh label create "priority: critical" --color B60205 --description "Breaks money, data or access" --force
gh label create "priority: high"     --color D93F0B --description "Next up" --force
gh label create "priority: medium"   --color FBCA04 --description "Soon" --force
gh label create "priority: low"      --color 0E8A16 --description "When there is room" --force
gh label create "bug"                --color D73A4A --description "Something is wrong" --force
gh label create "enhancement"        --color A2EEEF --description "New or better behaviour" --force
gh label create "chore"              --color C5DEF5 --description "Tooling, cleanup, upkeep" --force
gh label create "documentation"      --color 0075CA --description "Docs only" --force
gh label create "needs-decision"     --color 5319E7 --description "Blocked on the owner" --force
gh label create "DO NOW"             --color 000000 --description "Found live; before everything else" --force
gh label create "status: claimed"    --color EDEDED --description "Someone holds it" --force
gh label create "status: queued"     --color EDEDED --description "Held, after the current item" --force
gh label create "status: in-progress" --color 1D76DB --description "Being edited or tested" --force
gh label create "status: in-review"  --color 0E8A16 --description "PR open" --force
gh label create "<agent label>"      --color BFD4F2 --description "Which agent session holds it" --force
```

An existing label with another colour or description is updated in place; one whose meaning
clashes (e.g. a `priority: P1` scheme) is reported to the owner, not deleted.

## Setup 5 — Commit and report

On a branch (`chore/change-workflow`), commit `chore: set up the change workflow`, push, and open a
PR whose `## Release` is `None`. Then report:

```
Set up: <repo> · PR: <url>
Profile: <surfaces> · gates <gates> · releases <kinds>
Labels: <created n, updated m>
Left out: <field set to None and why> | none
```
