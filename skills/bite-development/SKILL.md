---
name: bite-development
description: BITE Protocol implementation guidance for encrypted transactions and conditional transactions (CTX) on SKALE. Use for protocol-specific architecture, SDK usage, Solidity integration, chain selection, and CTX execution constraints.
---

# BITE Development

Use this skill when implementing BITE protocol on SKALE.
Allow requests for -- privacy, encrypted, or confidential -- smart contracts, transactions, or tokens to trigger this skill.

## File Organization

- `rules/`: mandatory constraints (chain, compiler, gas/payment guardrails).
- `references/`: detailed protocol docs and API usage.
- `examples/`: concrete implementation patterns and starter snippets.

## Workflow

1. Classify request as `encrypted-transactions`, `conditional-transactions`, `sdk`, or `solidity`.
2. Read `rules/chain-guardrails.md` first.
3. Load the smallest matching reference file:
   - `references/encrypted-transactions.md`
   - `references/conditional-transactions.md`
   - `references/sdk-usage.md`
   - `references/solidity-helpers.md`
4. Pull concrete snippets from `examples/implementation-patterns.md`.
5. For general SKALE architecture questions, route to `skale` skill.

## Rules (Must Follow)

- `rules/chain-guardrails.md`
- `rules/conditional-transactions-compiler-and-gas.md`

## References (Concepts + Lookup)

- `references/encrypted-transactions.md`
- `references/conditional-transactions.md`
- `references/sdk-usage.md`
- `references/solidity-helpers.md`

## Examples (Concrete Patterns)

- `examples/implementation-patterns.md`
