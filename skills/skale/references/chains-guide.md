# Chains Guide

Use this guide with `chains.json` to answer SKALE chain directory questions.

## Source of Truth

- Primary data file: `references/chains.json`
- Do not infer or invent fields not present in the JSON.

## Supported Networks

- `mainnet`
- `testnet`
- `base-mainnet`
- `base-testnet`

## Lookup Strategy

1. If user requests all chains, group results by `network`.
2. If user requests a specific chain, match by one of:
- `name`
- `short_alias`
- `chain_id`
3. Return requested fields only; default fields are:
- `name`
- `short_alias`
- `chain_id`
- `rpc_endpoint`
- `portal_url`
- `explorer_url`
4. If a requested field is missing, return `N/A`.

## Alias Notes

- Some aliases differ between networks (for example, `europa` vs `europa-testnet`).
- Treat alias matching as exact unless user asks for fuzzy matching.

## Response Format

- Keep responses compact.
- Use absolute links for portal and explorer URLs.
- Preserve endpoint strings exactly from `chains.json`.
