# Eval contracts

Use eval contracts to prevent integration drift. Do not use them as paperwork.

## Levels

Use `none` when:

- the task is tiny
- one file or one obvious behavior is touched
- no packet produces anything another packet consumes

Use `inline` when:

- the task is medium risk
- agents or packets have clearly separate scopes
- packets do not share fragile surfaces
- 5-12 lines in `plan.md` can prove enough

Use `full` when:

- write-capable agents share integration surfaces
- one packet produces a surface another packet consumes
- the task touches public APIs, schemas, CLIs, UI flows, migrations, auth, data contracts, or shared modules
- the user asks for high-risk audit or independent verification

## Inline template

```text
Eval contract:
- Outcome:
- Shared surfaces:
- Required checks:
- Blocking conditions:
- Handoff evidence:
```

## Full template

```text
# Eval contract

## Goal

## Success criteria

## Integration surfaces

## Downstream consumers

## Required checks

## Deliverables

## Blocking conditions
```

## Downstream contract

Use `contracts/` only when one packet produces something another packet consumes.

```text
# Downstream contract: <surface>

Producer:
Consumers:
Surface:
Location:

## Guarantees

## Compatibility checks

## Allowed changes

## Forbidden changes
```

## Handoff evidence

Every packet result that touches shared behavior needs:

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

## Anti-bureaucracy rule

If the contract cannot name a consumer, surface, check, deliverable, or blocker, skip it or keep it inline.
