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

Optional high-risk files:

```text
  eval-contract.md
  contracts/
  handoffs/
  final-audit.md
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
## Eval contract
## Integration policy
## Verification plan
## Completion criteria
```

For explicit Ultracode on a non-trivial workflow run without native agents, include the concrete no-delegation reason.

## orchestration.md

Required sections:

```text
# Orchestration

## Parent critical path
## Packets
## Delegation
## Agents
## Delegation limits
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
  "baseline_ref": "git HEAD sha or no-git",
  "risk_level": "low|medium|high|unknown",
  "eval_contract": {
    "level": "none|inline|full",
    "path": "eval-contract.md or null",
    "status": "pending|ready|checked"
  },
  "approval": {
    "required": false,
    "granted": null,
    "notes": ""
  },
  "delegation": {
    "native_agent_available": false,
    "native_agent_planned": false,
    "native_agent_used": false,
    "agent_count": 0,
    "wave_count": 0,
    "no_delegation_reason": "",
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
## Handoff
## Files changed
## Decisions
## Risks
## Verification run
## Open questions
```

Handoff block for shared behavior:

```text
Handoff:
- Summary:
- Changed surfaces:
- Contracts satisfied:
- Assumptions:
- Local checks:
- Integration evidence:
- Risks:
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
## Final audit
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
- Keep native agent fan-out under 5 total without explicit approval.
- For explicit Ultracode on non-trivial work, `native_agent_used: false` needs a concrete `no_delegation_reason`.
