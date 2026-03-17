---
name: bite-development
description: BITE Protocol implementation guidance for encrypted transactions (Phase I) and conditional transactions (Phase II/CTX) on SKALE. Use for protocol-specific architecture, SDK usage, Solidity integration, chain/phase selection, and CTX execution constraints.
---

# BITE Development

Use this skill for BITE protocol mechanics and implementation details.

## File Organization

- `rules/`: mandatory constraints (phase selection, chain, compiler, gas/payment guardrails).
- `references/`: detailed protocol docs and API usage.
- `examples/`: concrete implementation patterns and starter snippets.

## Workflow

1. Classify request as `phase-1`, `phase-2-ctx`, `sdk`, or `solidity`.
2. Read `rules/phase-and-chain-guardrails.md` first.
3. Load the smallest matching reference file:
   - `references/phase-1-encrypted-transactions.md`
   - `references/phase-2-ctx.md`
   - `references/sdk-usage.md`
   - `references/solidity-helpers.md`
4. Pull concrete snippets from `examples/implementation-patterns.md`.
5. For non-BITE SKALE architecture questions, route to `skale` skill.

## Rules (Must Follow)

- `rules/phase-and-chain-guardrails.md`
- `rules/ctx-compiler-and-gas.md`

## References (Concepts + Lookup)

- `references/phase-1-encrypted-transactions.md`
- `references/phase-2-ctx.md`
- `references/sdk-usage.md`
- `references/solidity-helpers.md`

## Examples (Concrete Patterns)

- `examples/implementation-patterns.md`
