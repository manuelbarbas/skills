# Error Handling Rules

Always map failures to the known error taxonomy from `references/command-matrix.md`.

## Required Behavior

- Preserve error code semantics: `MISSING_TARGET`, `INVALID_ADDRESS`, `INVALID_CONTRACT`, `INVALID_METHOD`, `CALL_FAILED`.
- Provide one concrete next command per failure when possible.
- Do not invent new error codes.
