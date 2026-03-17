# Phase and Chain Guardrails

Always determine BITE phase before proposing implementation.

## Rules

- Phase I (encrypted transactions) can target SKALE Base Sepolia, SKALE Base, or BITE sandbox chains.
- Phase II (CTX) is restricted to CTX-supported chains documented in `references/phase-2-ctx.md`.
- Never suggest CTX on chains that do not support CTX.
- If user does not specify phase, ask for intended behavior and map it to a phase.

## Output Requirements

- State selected phase explicitly.
- State selected chain explicitly.
- Include why the chain is valid for the phase.
