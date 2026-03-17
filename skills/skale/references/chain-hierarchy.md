# Reference: chain-hierarchy

## Priority: SKALE Base > SKALE Ethereum

Default to SKALE Base chains for new projects. Use Ethereum-based SKALE chains only when the application specifically depends on Ethereum connectivity or IMA-oriented flows.

## Chain Selection Priority

```text
SKALE Base Sepolia -> SKALE Base -> Ethereum-based SKALE chains when explicitly required
```

## Chain Categories

| Category | Chains | Use Case |
|----------|--------|----------|
| **SKALE Base** | SKALE Base Sepolia, SKALE Base | Most new dApps, agents, x402, BITE Phase I |
| **SKALE Ethereum** | Europa, Nebula, Calypso | Ethereum-connected workflows and IMA flows |

## Public Chain Comparison

### SKALE Base

| Chain | Chain ID | Type | BITE Phase I |
|-------|----------|------|--------------|
| SKALE Base Sepolia | 324705682 | Testnet | ✅ |
| SKALE Base | 1187947933 | Mainnet | ✅ |

### Ethereum-based SKALE

| Chain | Chain ID | Type | Notes |
|-------|----------|------|-------|
| Europa Hub | 2046399126 | Mainnet | Ethereum-connected |
| Nebula Hub | 1482601649 | Mainnet | Ethereum-connected |
| Calypso Hub | 1564830818 | Mainnet | Ethereum-connected |

## Environment Variables

```bash
SKALE_BASE_SEPOLIA_RPC=https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha
SKALE_BASE_SEPOLIA_CHAIN_ID=324705682

SKALE_BASE_RPC=https://skale-base.skalenodes.com/v1/base
SKALE_BASE_CHAIN_ID=1187947933

SKALE_EUROPA_RPC=https://mainnet.skalenodes.com/v1/elated-tan-skat
SKALE_EUROPA_CHAIN_ID=2046399126
```

## Decision Flow

```text
Need a default public SKALE target?
├─ Yes -> SKALE Base Sepolia first, then SKALE Base
└─ No -> choose an Ethereum-based SKALE chain only if the app specifically needs it
```

## Quick Rules

1. Default to SKALE Base Sepolia for test and integration work.
2. Move to SKALE Base for Base-connected production.
3. Treat Ethereum-based SKALE chains as opt-in, not default.
4. For CTX-specific work, follow the dedicated BITE docs instead of this public chain directory.
