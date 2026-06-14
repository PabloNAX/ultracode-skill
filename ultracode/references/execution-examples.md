# Execution examples

Use these examples when mode choice is unclear.

## Small typo

User:

```text
Use $ultracode to fix this typo in README.
```

Mode: direct.

Reason: the task is tiny and workflow overhead would exceed the work.

Expected behavior:

- Edit the typo.
- Inspect the diff.
- Say a full workflow was not needed.

## Broad audit without Ultracode

User:

```text
Audit this repo for slow startup paths and give me a fix plan.
```

Mode: workflow.

Reason: broad investigation benefits from packets, but this prompt does not invoke Ultracode or delegated agents.

Expected packets:

- `01-entry-points`
- `02-startup-costs`
- `03-fix-plan`

Expected behavior:

- Create a run directory using the run root rule.
- Write `plan.md`, `orchestration.md`, and `state.json`.
- Save packet notes in `results/`.
- Integrate findings before final answer.

## Feature implementation with explicit ultracode

User:

```text
Use $ultracode. Split this across agents and implement the settings export feature.
```

Mode: delegated, if host-native agent tools are available and policy permits.

Expected parent work:

- Create artifacts.
- Write `orchestration.md` before spawning agents.
- In Codex, call native `spawn_agent` for the independent sidecar packets.
- When using Codex `explorer` or `worker` agent types, pass a self-contained prompt instead of combining agent type with a full-history fork.
- Inspect the blocking architecture path.
- Own integration and verification.

Possible sidecar packets:

- explorer: find settings storage and existing export patterns
- worker: implement backend export route in named files
- worker: add UI button and loading state in named files
- explorer: find tests and fixtures

Each worker must have a disjoint write scope.

## Feature implementation with ultracode only

User:

```text
Ultracode: implement the settings export feature end to end.
```

Mode: direct, workflow, or delegated depending on task size, independent packet value, native tools, and host policy.

Reason: `ultracode` requests the structured workflow and authorizes the agent to choose the workflow depth. For non-trivial work, if native agents are useful and host policy permits it, delegated mode is the default.

Expected behavior:

- Create workflow artifacts if the work is non-trivial.
- Split discovery, implementation, and verification into packets when useful.
- Use native delegated agents when the work has independent packets and the host permits delegation.
- Fall back to workflow mode only when native delegation is unavailable, blocked by host policy, not useful, or restricted by the user.
- Record the concrete no-delegation reason in `plan.md` and `orchestration.md`.

## Whole-repo audit with explicit ultracode

User:

```text
Use $ultracode to audit this repository.
```

Mode: delegated, if host-native agent tools are available and policy permits.

Expected behavior:

- Create workflow artifacts using the run root rule.
- Prefer 2-4 read-only explorer agents for independent audit tracks unless the repo is tiny.
- Keep integration, prioritization, and final claims in the parent session.
- Do not exceed 5 total agents without explicit approval.
- If no agents are used, give a concrete reason such as unavailable tools, tiny scope, or no independent packet value.

## Risky migration

User:

```text
Use $ultracode to migrate all API clients to the new SDK.
```

Mode: workflow or delegated depending on independent packet value, available native agent tools, and host policy.

Approval gate:

- broad codemod
- dependency updates
- possible behavior change across modules

Expected behavior:

- Plan and inspect first.
- Ask before broad rewrites.
- Continue with read-only mapping if approval is not granted.
- Use an inline or full eval contract when shared APIs, schemas, auth, or data contracts are touched.

## No subagent runner

User:

```text
Use $ultracode and run parallel agents for this audit.
```

Mode: workflow fallback when native agent tools are unavailable.

Expected behavior:

- Say native agent tools are unavailable in this environment.
- Create packet files.
- Execute isolated passes in the parent session.
- Keep evidence separate by result file.
- Record the no-delegation reason.
