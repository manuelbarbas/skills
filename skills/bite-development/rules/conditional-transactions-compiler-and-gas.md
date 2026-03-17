# Conditional Transactions Compiler and Gas Rules

Use these rules whenever the request involves conditional transactions (CTX).

## Rules

- Apply compiler and EVM version requirements from `references/conditional-transactions.md` exactly.
- Include gas/payment requirements in implementation instructions.
- Do not omit required callbacks/interfaces.
- If compiler settings conflict with user project settings, call out the mismatch and provide exact remediation.
