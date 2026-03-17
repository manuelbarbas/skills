# Chain Guardrails

Determine which BITE primitive before proposing implementation.

## Rules

- Encrypted transactions can target SKALE Base Sepolia, SKALE Base, or SKALE BITE Sandbox (a SKALE Base Testnet Chain). These chains are all SKALE Base deployments. SKALE Ethereum does not support BITE currnetly.
- Conditional Transactions (CTX) are restricted to CTX-supported chains documented in `references/conditional-transactions.md`.
- Never suggest BITE on chains that do not support it.
- If user does not specify capability, ask for intended behavior.

## Output Requirements

- State selected capability explicitly.
- State selected chain explicitly.
- Include why the chain is valid for the capability.
