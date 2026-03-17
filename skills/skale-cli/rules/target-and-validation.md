# Target and Validation Rules

## Rules

- Require `--chain` for SKALE chain-targeted commands.
- Require `--network` for Ethereum network-targeted commands.
- Validate contract support against `references/command-matrix.md` before recommending `read` invocations.
- Validate address format for commands requiring addresses.
- Do not suggest command flags that are not in the matrix.
