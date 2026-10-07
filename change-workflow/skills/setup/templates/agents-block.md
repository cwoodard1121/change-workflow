<!-- change-workflow:start — managed by the change-workflow setup skill; edit the profile values freely, re-running setup keeps them -->
## Change workflow profile

Read by the `change-workflow` skill (any agent that is not Claude Code reads that skill's file and
follows it). Fill every field; `None` is a valid answer, a blank is not.

- **Read first:** {{READ_FIRST}}
- **Surfaces:** {{SURFACES}}
- **Interface checklist:** {{INTERFACE_CHECKLIST}}
- **Gates:** {{GATES}}
- **Release kinds:** {{RELEASE_KINDS}}
- **Specs / Plans:** `{{SPECS_DIR}}` / `{{PLANS_DIR}}`
- **After an issue change:** {{AFTER_ISSUE}}
- **Agent label:** `{{AGENT_LABEL}}`

## The change workflow: one path from "I want X changed" to the PR

Every change request, however small, runs it. The depth scales with the change; the steps do not.
The spec and the plan are committed on the branch and linked from the issue.

0. **Orient.** `DO NOW` issues first; the request's issue claimed; the read-first paths read; branch
   cut from the default branch.
1. **Questions, one at a time.** Each multiple choice, the recommended option first with one
   sentence on why: purpose, scope boundary, surface, data and interfaces, edge cases, the owner's
   success check. Skip what the request already answers; stop when no answer would change the design.
2. **Approaches.** Two or three with trade-offs and a recommendation; the owner picks one.
3. **Spec**, committed. No code until the owner approves it; a small change gets a short spec, not
   no spec. Owner away: `needs-decision` on the issue and move to another one.
4. **Plan**, committed and linked from the issue: the file structure, the interface checklist, and
   tasks that each carry their files, failing test, code, commands and commit — no placeholders.
5. **Execute** the tasks in order, test → code → commit → push each, without pausing for approval;
   stop only on a blocker and name it.
6. **Finish.** The gates with their real output, the PR from the template with `## Spec and plan`,
   `## Release`, `Closes #n`, `status: in-review`.
7. **Report** in the fixed shape: Done · PR and release kind · Spec and plan · Verified · Left out · Next.

## Issue tracking

Bugs and outstanding work live in GitHub Issues, not in a markdown file, a code comment or an
agent's memory. Every issue carries exactly one `priority: critical|high|medium|low` label and one
type label (`bug`, `enhancement`, `chore`, `documentation`); `needs-decision` when it is blocked on
the owner. `DO NOW` marks a problem found live: it sits on top of the priority label, never instead
of it, and goes before everything else. Exactly one `status:` label at a time.

Every agent does all of this, in order, for every issue it works. After each step, and after
opening, closing or relabeling any issue, run the profile's after-issue-change command.

0. **`DO NOW` first.** `gh issue list --label "DO NOW"` before picking anything up.
1. **Pick up.** Read it, check nobody else holds it, then
   `gh issue edit <n> --add-assignee @me --add-label "status: claimed" --add-label "<agent label>"`.
   Planned for after your current item instead: same command with `status: queued`.
2. **Start** — the moment you begin editing or testing:
   `gh issue edit <n> --remove-label "status: claimed" --remove-label "status: queued" --add-label "status: in-progress"`.
3. **Blocked on the owner:** `gh issue edit <n> --add-label needs-decision` and
   `gh issue comment <n> --body "Decision needed: <the exact question and the options>"`. Keep your
   `status:` label; remove `needs-decision` when the answer lands.
4. **PR opened:** `gh issue edit <n> --remove-label "status: in-progress" --add-label "status: in-review"`,
   with `Closes #<n>` in the PR body.
5. **Merged:** the `Closes #<n>` closes the issue. Never close one by hand unless it turned out
   done or obsolete, and then with a comment saying why.
6. **Handing back** (you stop without a PR): remove your assignee, your `status:` label and your
   agent label, and comment where you left it.

A bug or gap you find that you were not asked to fix gets an issue — labeled, assigned to you —
not a mention and not a bare code comment. Fixing it in the current work: `Closes #<n>` in the PR.
<!-- change-workflow:end -->
