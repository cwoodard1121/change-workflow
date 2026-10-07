# <Topic> — design

Issue: #<n> · Date: YYYY-MM-DD · Status: draft | approved

The shape every spec takes (the `change-workflow` skill, Step 3). Copy it to
`<specs>/YYYY-MM-DD-<topic>-design.md`, fill every section or write `None`, never `TBD`.
A small change gets short sections, not missing ones.

## Summary

Two or three sentences: what changes, for whom, and why now.

## Goals

-

## Non-goals

-

## Decisions

Every question asked, its options, and the choice. The alternatives matter as much as the pick.

| Question | Options | Chosen | Why |
| --- | --- | --- | --- |

## Approach

The approach chosen and the ones it beat, one line each on why.

## Data and contracts

Tables, columns and the migration; schemas and types shared between client and server; anything
about versions, idempotency or units that a caller has to get right.

## Interfaces

One row per new or changed route, command or public function.

| Name | Method / signature | Caller | Refuses |
| --- | --- | --- | --- |

## Screens

Per surface in the project profile: what the user sees and does, the exact copy, and the states —
loading, empty, refused, error.

## Behaviour and edge cases

What the server or core enforces; concurrency, offline, realtime, permissions; what is logged or
audited.

## Testing

The unit, integration and end-to-end tests that will prove it, and the owner's success check.

## Release

Exactly one of the profile's release kinds, and anything to do by hand when it ships.

## Deferred

What came up and was left for later; an issue for each.
