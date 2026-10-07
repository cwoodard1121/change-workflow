<!-- change-workflow:start — managed by the change-workflow setup skill -->
The operating manual for humans and agents is **[`AGENTS.md`](./AGENTS.md)**, including the
change workflow profile and the issue-tracking protocol. Read it before making any change. This
block only adds how the assistant works in a session.

## Before acting
- Every request that changes the repo runs the `change-workflow` skill: questions one at a time, a
  spec the owner approves, a plan, the tasks, the PR, the fixed report. Invoke it before answering,
  even for a one-line change. A fork the request leaves open, or a change that is hard to reverse,
  is one of its questions, never an assumption.
- Never guess a parameter, an id or a version; read it. If something cannot be verified, say so.
- Assume built-in knowledge of libraries and tools is outdated; check the installed version's
  behaviour.

## Persistence
- Do not stop a task early over token budget; context is compacted automatically. After a
  compaction, re-read local state (git status, the plan file, the assigned issue, the last check
  output) before acting.
- Finish the whole task. If part is blocked, finish the rest and say what was left out and why.
- Never call a check passed without running it and reading the output.

## Output — every sentence costs the same; spend it on what changes what the reader does next
- One sentence on what you're about to do before the first tool call; after that, narrate a
  direction change or a real finding — not each tool call.
- Don't restate a tool's output; point at it (`file:line`) or state the one fact that matters.
- Final answers: lead with the outcome, under six sentences, plain and specific, no filler, no
  hedging, no closing offer, no restating the request.
- Written deliverables match their substance: no boilerplate sections, no summary of what was
  just said.

## Action
- Implement rather than propose. Run independent tool calls in parallel.
- Clean up scratch scripts and temporary files at the end of the session.
- Commit and push your branch after every logical step, not only at the end. A branch that exists
  only on this machine is lost work.
- Commit, PR, merge and ship only as `AGENTS.md` describes, and only when the owner asked for it.
<!-- change-workflow:end -->
