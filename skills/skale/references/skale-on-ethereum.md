# Reference: skale-on-ethereum

> Only use this path when the user explicitly needs an Ethereum-connected SKALE chain. Default chain selection still starts with SKALE Base.

## Why It Matters

Ethereum-based SKALE chains use IMA connectivity and have their own exact RPC, explorer, and portal values. This file keeps those public chain endpoints grounded in the SKALE docs instead of guessed hostnames.

## Public Chain Directory

| Chain | RPC URL | Explorer | Portal |
|-------|---------|----------|--------|
| Calypso Hub | `https://mainnet.skalenodes.com/v1/honorable-steel-rasalhague` | `https://honorable-steel-rasalhague.explorer.mainnet.skalenodes.com/` | `https://portal.skale.space/chains/calypso` |
| Europa Hub | `https://mainnet.skalenodes.com/v1/elated-tan-skat` | `https://elated-tan-skat.explorer.mainnet.skalenodes.com/` | `https://portal.skale.space/chains/europa` |
| Nebula Hub | `https://mainnet.skalenodes.com/v1/green-giddy-denebola` | `https://green-giddy-denebola.explorer.mainnet.skalenodes.com/` | `https://portal.skale.space/chains/nebula` |
| Calypso Testnet | `https://testnet.skalenodes.com/v1/giant-half-dual-testnet` | `https://giant-half-dual-testnet.explorer.testnet.skalenodes.com/` | `https://testnet.portal.skale.space/chains/calypso` |
| Europa Testnet | `https://testnet.skalenodes.com/v1/juicy-low-small-testnet` | `https://juicy-low-small-testnet.explorer.testnet.skalenodes.com/` | `https://testnet.portal.skale.space/chains/europa` |
| Nebula Testnet | `https://testnet.skalenodes.com/v1/lanky-ill-funny-testnet` | `https://lanky-ill-funny-testnet.explorer.testnet.skalenodes.com/` | `https://testnet.portal.skale.space/chains/nebula` |

## Bridge Guidance

- Use the official SKALE bridge documentation and contract interfaces for Ethereum <-> SKALE flows.
- Do not invent or assume a JavaScript bridge SDK package unless the user already has one in their codebase.
- For contract-level messaging, prefer the MessageProxy and SKALE bridge docs.

## Environment Variables

```bash
SKALE_CALYPSO_RPC=https://mainnet.skalenodes.com/v1/honorable-steel-rasalhague
SKALE_EUROPA_RPC=https://mainnet.skalenodes.com/v1/elated-tan-skat
SKALE_NEBULA_RPC=https://mainnet.skalenodes.com/v1/green-giddy-denebola
```

## References

- [SKALE Connect Docs](https://docs.skale.space/developers/integrate-skale/connect-to-skale)
- [SKALE on Ethereum](https://docs.skale.space/get-started/quick-start/skale-on-ethereum)
- [SKALE Bridge Overview](https://docs.skale.space/developers/skale-bridge/overview)
