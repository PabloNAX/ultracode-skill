# Forward testing

Use these prompts when testing or improving the skill. Run them in fresh sessions when possible. Do not pass expected answers.

## Test prompts

### Direct mode

```text
Use $ultracode to fix one typo in README and verify the diff.
```

Expected:

- direct mode
- no workflow artifacts unless requested
- focused verification

### Workflow audit

```text
Use $ultracode to audit this small repo for slow startup paths. Do not edit files.
```

Expected:

- workflow mode
- run directory follows the run root rule
- `plan.md`, `orchestration.md`, and `state.json`
- packet notes in `results/`
- no source edits

### Delegated mode

```text
Use $ultracode to audit this repository for correctness risks.
```

Expected:

- delegated mode only if tools exist and policy permits
- prefers 2-4 read-only sidecar agents when useful independent audit tracks exist
- parent keeps blocking work local
- workers have disjoint ownership if write-capable work is delegated
- final answer reports verification

### Approval gate

```text
Use $ultracode to migrate every config file to the new format.
```

Expected:

- plan before rewrite
- approval before broad codemod
- safe read-only progress if approval is absent

### Fallback

```text
Use $ultracode and run agents, but assume this environment cannot spawn subagents.
```

Expected:

- workflow fallback
- clear explanation
- same artifact discipline
- concrete no-delegation reason in the plan or orchestration

### Eval contract

```text
Use $ultracode to migrate this API route and UI consumer to a new response schema.
```

Expected:

- inline or full eval contract
- shared surfaces and required checks named before edits
- final audit checks the contract before completion

## Validation checklist

- The skill does not claim to be Claude Code `ultracode`.
- Direct mode stays lightweight.
- No Python helpers are required.
- Workflow mode creates useful artifacts.
- Run artifacts include `orchestration.md`.
- Delegated mode respects host delegation policy.
- Explicit Ultracode prefers bounded native agents for non-trivial independent packets.
- Workflow fallback records why native agents were not used.
- Eval contracts appear only when they reduce integration risk.
- Approval gates stop risky work.
- The installable skill folder contains no `scripts/` directory.
- The skill folder contains a valid `SKILL.md` frontmatter with matching folder name.
