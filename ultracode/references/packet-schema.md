# Packet schema

Use this reference when creating or validating workflow artifacts.

## Run layout

```text
<run-root>/<slug>/
  plan.md
  orchestration.md
  state.json
  packets/
    01-discovery.md
  results/
    01-discovery.md
  integration.md
  final-report.md
```

## plan.md

`<run-root>` should be `.workflow/ultracode/` unless project instructions name another scratch directory.

Required sections:

```text
# <task title>

## Goal
## Success criteria
## Current context
## Constraints
## Risk level
## Approval gates
## Mode
## Work packets
## Integration policy
## Verification plan
## Completion criteria
```

## orchestration.md

Required sections:

```text
# Orchestration

## Parent critical path
## Packets
## Delegation
## Agents
## Wait points
## Fallback
## Verification order
```

Keep this file short. It is the execution contract for this run, not a transcript.

## state.json

Required keys:

```json
{
  "title": "string",
  "slug": "string",
  "created_at": "ISO-8601 string",
  "updated_at": "ISO-8601 string",
  "status": "planning",
  "mode": "direct|workflow|delegated",
  "approval": {
    "required": false,
    "granted": null,
    "notes": ""
  },
  "delegation": {
    "native_agent_available": false,
    "native_agent_used": false,
    "notes": ""
  },
  "packets": [
    {
      "id": "01-discovery",
      "status": "pending",
      "owner": "parent|read-only-agent|write-capable-agent",
      "write_scope": [],
      "result_path": "results/01-discovery.md"
    }
  ],
  "verification": {
    "status": "pending",
    "checks": [
      {
        "name": "unit tests",
        "command": "npm test",
        "required": true,
        "status": "pending",
        "evidence": ""
      }
    ]
  }
}
```

Allowed run status values:

- `planning`
- `waiting_for_approval`
- `executing`
- `integrating`
- `verifying`
- `complete`
- `blocked`
- `cancelled`

Allowed packet status values:

- `pending`
- `in_progress`
- `complete`
- `blocked`
- `skipped`

## Packet files

```text
# Packet <id>: <name>

## Objective
## Context
## Sources
## Ownership
## Do
## Do not
## Expected output
## Verification
## Handoff format
```

For code-edit packets, also include:

```text
## Write scope

- path/to/file-a
- path/to/module/

## Coordination rule

You are not alone in the codebase. Do not revert edits made by others. Adapt to nearby changes.
```

## Result files

```text
# Result <id>: <name>

## Summary
## Evidence
## Files changed
## Decisions
## Risks
## Verification run
## Open questions
```

## integration.md

```text
# Integration

## Accepted
## Rejected
## Conflicts
## Decisions
## Final changes
## Verification still needed
## Remaining risks
```

## final-report.md

```text
# Final report

## Outcome
## What changed
## Verification
## Skipped checks
## Remaining risks
## Next useful step
```

## Naming rules

- Use two-digit packet prefixes: `01-discovery`, `02-tests`.
- Use lowercase hyphen-case slugs.
- Keep slugs under 64 characters.
- Match packet result names to packet IDs.
- Do not mark work complete without evidence in `verification.checks` or `final-report.md`.
