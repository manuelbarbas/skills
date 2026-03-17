# SKALE Skills

Collection of installable Agent Skills for SKALE Network workflows.

## Install

List skills available in this repo:

```bash
npx -y skills add skalenetwork/skale-skills --list
```

Install one skill:

```bash
npx -y skills add skalenetwork/skale-skills --skill skale
npx -y skills add skalenetwork/skale-skills --skill bite-development
npx -y skills add skalenetwork/skale-skills --skill skale-cli
```

Install all skills from this repo:

```bash
npx -y skills add skalenetwork/skale-skills --skill '*' --agent '*' -y
```

## Available Skills

- `skale`: detailed SKALE development skill with nested `rules/`, `references/`, and `examples` organization.
- `bite-development`: BITE protocol skill with phase guardrails, protocol references, and implementation examples.
- `skale-cli`: command-first SKALE CLI skill with validation rules, command matrix reference, and operation flows.

## Repository Structure

Installable skills live under `skills/` and follow the same nested model:

```text
skills/
  <skill>/
    SKILL.md        # dynamic discovery + linking root
    rules/          # must-follow constraints
    references/     # conceptual and lookup docs
    examples/       # concrete patterns and commands
    scripts/        # optional executable helpers
    agents/         # UI metadata
```

## Data Source Notes

- SKALE chain directory data: `skills/skale/references/chains.json`
  - source: <https://docs.skale.space/developers/integrate-skale/connect-to-skale>
- SKALE CLI command behavior reference: `~/projects/skale-cli/src`

## Migration Notes

Latest structure update:

- All skills now use nested `rules/`, `references/`, `examples/` with dynamic discovery in root `SKILL.md`.
