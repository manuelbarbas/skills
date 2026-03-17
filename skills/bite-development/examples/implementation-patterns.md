# Implementation Patterns

Use these as routing examples to pick the right detailed references.

## Encrypted Transaction Flow

1. Build tx payload (`to`, `data`).
2. Encrypt with BITE SDK.
3. Set explicit `gasLimit`.
4. Send tx and capture hash.
5. Optionally query decrypted data post-execution.

Reference: `references/encrypted-transactions.md`

## CTX Flow

1. Ensure chain supports CTX.
2. Configure compiler/EVM per CTX requirements.
3. Implement required CTX interface/callback pattern.
4. Submit CTX with required gas/payment params.
5. Validate decrypt callback path.

Reference: `references/conditional-transactions.md`

## SDK-First Integration

- Prefer `references/sdk-usage.md` when user is in TypeScript/JavaScript.
- Prefer `references/solidity-helpers.md` when user is building contract-side CTX logic.
