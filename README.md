# change-workflow

One fixed path from "I want X changed" to a reviewed PR, for Claude Code and any agent that reads
`AGENTS.md`:

0. **Orient** — `DO NOW` issues first, claim the issue, read the code before asking, cut a branch.
1. **Questions, one at a time** — multiple choice, recommended option first: purpose, scope,
   surface, data and interfaces, edge cases, the owner's success check.
2. **Approaches** — two or three with trade-offs; the owner picks.
3. **Spec** — committed, approved before any code. A small change gets a short spec, not no spec.
4. **Plan** — file structure, interface checklist, tasks with the failing test, the code, the
   commands and the commit. No "similar to Task N".
5. **Execute** — test red → code → green → commit → push, task by task.
6. **Finish** — the gates with real output, the PR from the template, `Closes #n`.
7. **Report** — always the same six lines: Done · PR and release · Spec and plan · Verified ·
   Left out · Next.

It runs on a GitHub Issues protocol: one `priority:` and one type label on every issue, `DO NOW`
for problems found live, one `status:` label (`claimed → in-progress → in-review`) and an `agent:`
label saying which session holds it, so several agents can share a backlog without colliding.

## Install

```
/plugin marketplace add cwoodard1121/change-workflow
/plugin install change-workflow@change-workflow
```

or from a shell: `claude plugin marketplace add cwoodard1121/change-workflow` then
`claude plugin install change-workflow@change-workflow`. Add `--scope project` to install into
one repo only.

Then, in each repo, once:

```
/change-workflow:setup
```

Setup reads the repo, drafts its **profile** (where to read first, its surfaces, how an interface is
added, its gates, its release kinds, a command to run after issue edits), asks only what it could not
read, and writes:

| File | What goes in it |
| --- | --- |
| `AGENTS.md` | the profile, the workflow, the issue protocol (a managed block) |
| `CLAUDE.md` | the session rules: before acting, persistence, output, action (a managed block) |
| `docs/specs/_template.md`, `docs/plans/_template.md` | the spec and plan shapes |
| `.github/pull_request_template.md` | What changed · Spec and plan · How it was verified · Left out · Release |
| GitHub labels | priority, type, `needs-decision`, `DO NOW`, `status:`, your `agent:` label |

Run it again to update: only the content between `<!-- change-workflow:start -->` and
`<!-- change-workflow:end -->` is replaced, and the profile values you edited are kept.

From then on every change request goes through `change-workflow` on its own; you can also call it
with `/change-workflow:change-workflow`.

## Without Claude Code

The skills are plain markdown. Another agent (Codex, Cursor…) can follow
`change-workflow/skills/change-workflow/SKILL.md` directly; `AGENTS.md` points at it. To use the
skills without the plugin system, copy `change-workflow/skills/*` into a repo's `.claude/skills/`.

## Layout

```
.claude-plugin/marketplace.json          the marketplace (this repo)
change-workflow/.claude-plugin/plugin.json
change-workflow/skills/change-workflow/  the workflow itself
change-workflow/skills/setup/            the per-repo setup, with templates/
```

## License

MIT
