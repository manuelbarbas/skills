---
name: skale
description: SKALE Network development guidelines and documentation access. Use for building dApps, smart contracts, cross-chain solutions, chain directory lookups, and SKALE docs research. Triggers include SKALE, skale-dev, skale-docs, BITE Protocol context, Europa, Calypso, and skale-bridge.
---

# SKALE Network Development

Comprehensive development guide for SKALE Network, covering smart contracts, web/mobile integration, cross-chain bridges, privacy primitives, and chain metadata retrieval.

## File Organization

Use this skill as a nested reference system:

- `rules/`: prescriptive requirements that must be followed.
- `references/`: conceptual and lookup material.
- `examples/`: concrete implementation patterns.

## Workflow

1. Identify request type (chain lookup, docs, architecture, implementation pattern).
2. Check `rules/` first for mandatory constraints.
3. Use `references/` for decision-making context and source-of-truth lookups.
4. Use `examples/` for concrete code and command patterns.

## Quick Index

### Rules (Must Follow)

- `rules/chain-naming.md`
- `rules/chain-gas-terminology.md`
- `rules/chain-target-correct.md`
- `rules/contracts-compiler-settings.md`

### References (Concepts + Lookup)

- `references/chain-hierarchy.md`
- `references/skale-on-base.md`
- `references/skale-on-ethereum.md`
- `references/privacy-bite-protocol.md`
- `references/docs-search-patterns.md`
- `references/contracts-solidity-patterns.md`
- `references/contracts-cross-chain.md`
- `references/chains-guide.md`
- `references/chains.json`
- `references/agents-on-skale.md`

### Examples (Concrete Patterns)

- `examples/contracts-deployment.md`
- `examples/bridge-skale-bridge.md`
- `examples/x402-on-skale.md`
- `examples/random-number-generation.md`
- `examples/bite-encrypted-transactions.md`
- `examples/bite-conditional-transactions.md`
- `examples/bite-sdk-usage.md`
- `examples/bite-solidity-helpers.md`

## Chain Directory Rules

- Treat `references/chains.json` as source of truth.
- Match requested chain by `name`, `short_alias`, or `chain_id`.
- Keep endpoints and URLs exact.
- If field is missing, return `N/A`.

## Documentation Endpoints

- `https://docs.skale.space/llms.txt`
- `https://docs.skale.space/llms-full.txt`
- `https://docs.skale.space/llms-small.txt`

Pages support `.md` suffix for raw markdown.

## Related Skills Installation

Install capability-specific companion skills from this repository when needed:

```bash
npx -y skills add skalenetwork/skale-skills --skill bite-development
npx -y skills add skalenetwork/skale-skills --skill skale-cli
```

Use these companion skills for:

- `bite-development`: detailed BITE protocol implementation flows.
- `skale-cli`: exact CLI command playbooks and troubleshooting.
