# AGENTS.md - Agent Coding Guidelines

Repository: skale-skills - Collection of installable Agent Skills for SKALE Network workflows.

## Skills

| Skill | Description |
|-------|-------------|
| `about-skale` | What is SKALE, chain types, gas models |
| `deploy-to-skale` | Chain selection, deployment, RNG, bridge |
| `programmable-privacy` | Encrypted transactions, CTX, re-encryption, confidential tokens |
| `x402-on-skale` | AI agent payments |
| `skale-cli` | CLI commands |
| `ima-bridging-on-skale` | Programmatic IMA (Inter-chain Messaging Agent) bridging for SKALE chains |

## Installation

```bash
npx -y skills add skalenetwork/skale-skills --skill <skill-name>
```

## Naming Conventions

- Use "SKALE" (all caps), not "skale"
- File names: kebab-case
- Don't use protocol/library names as skill or rule identifiers (e.g. no "bite-" prefix on file names). Only use package names or specific code objects (e.g. `BITE.sol`, `@skalenetwork/bite`) in prose and code.

## Versioning

Each skill is versioned via metadata in its `SKILL.md` file following standard semantic versioning (`x.y.z`):
- **Major (x)**: Breaking changes or significant rewrites
- **Minor (y)**: New features, capabilities, or non-breaking additions
- **Patch (z)**: Bug fixes, documentation improvements, minor corrections

Version bumps should happen automatically as part of changes — increment the appropriate version component when modifying a skill.

## Docs

- `https://docs.skale.space/llms.txt.md`
