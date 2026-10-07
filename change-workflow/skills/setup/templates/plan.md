# <Topic> — plan

Issue: #<n> · Spec: `<specs>/YYYY-MM-DD-<topic>-design.md` · Branch: `<type>/<topic>` · Date: YYYY-MM-DD

The shape every plan takes (the `change-workflow` skill, Step 4). Copy it to
`<plans>/YYYY-MM-DD-<topic>.md`. Every task carries its files, its failing test, its code, its
commands with their expected output and its commit; nothing is "similar to Task N" or "add validation".

**Goal:** one sentence.
**Approach:** two or three sentences, from the spec.
**Release:** one of the profile's release kinds

## File structure

Every file this plan creates or modifies, one line each on what it is for. Boundaries are decided
here, not during a task.

- Create: `path` —
- Modify: `path` —

## Interface checklist

One block per new or changed route, command or contract, in the order the project profile gives.
Delete the section when nothing of the kind changes.

- [ ] <step 1 from the profile>
- [ ] <step 2 from the profile>

## Tasks

### Task 1: <name>

Files: Create `…` · Modify `…` · Test `…`

- [ ] Write the failing test

  ```
  ```

- [ ] Run it red: `<exact command>` → FAIL: `<message>`
- [ ] Implement

  ```
  ```

- [ ] Run it green: `<exact command>` → PASS
- [ ] Commit: `type(scope): what and why (#n)`, push

## Verification

- [ ] <each gate from the profile that applies> → <expected result>
- [ ] the owner's success check: <from the spec>

## Deviations

Written during execution, before continuing: what the plan said, what was done instead, why.
