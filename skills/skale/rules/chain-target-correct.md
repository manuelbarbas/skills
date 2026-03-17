# Rule: chain-target-correct

## Why It Matters

Deploying to the wrong SKALE chain breaks testing assumptions, bridge assumptions, and explorer verification. Use exact chain metadata from the public SKALE docs rather than placeholders or guessed endpoints.

## Incorrect

```typescript
// Hardcoded production endpoint during development
const rpcUrl = "https://mainnet.skalenodes.com/v1/elated-tan-skat";
```

```typescript
// Placeholder chain IDs are not safe source-of-truth
const chainId = 0x727cd5b7a84dc;
```

## Correct

```typescript
const CHAINS = {
    baseTestnet: {
        id: 324705682,
        name: "SKALE Base Sepolia",
        rpc: "https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha",
        explorer: "https://base-sepolia-testnet-explorer.skalenodes.com/",
        faucet: "https://base-sepolia-faucet.skale.space"
    },
    baseMainnet: {
        id: 1187947933,
        name: "SKALE Base",
        rpc: "https://skale-base.skalenodes.com/v1/base",
        explorer: "https://skale-base-explorer.skalenodes.com/"
    },
    europa: {
        id: 2046399126,
        name: "Europa Hub",
        rpc: "https://mainnet.skalenodes.com/v1/elated-tan-skat",
        explorer: "https://elated-tan-skat.explorer.mainnet.skalenodes.com/"
    }
} as const;

const currentChain = CHAINS[process.env.SKALE_CHAIN ?? "baseTestnet"];
```

```solidity
contract MyContract {
    uint256 public immutable deployedChainId;

    constructor() {
        deployedChainId = block.chainid;
    }
}
```

## Context

### Chain Selection Priority

Default path for new work:

```text
SKALE Base Sepolia -> SKALE Base -> Ethereum-based SKALE only when explicitly required
```

### Public Chain IDs

| Chain | Chain ID | Type | Recommended Use |
|-------|----------|------|-----------------|
| SKALE Base Sepolia | 324705682 | Testnet | Default for testing |
| SKALE Base | 1187947933 | Mainnet | Default for Base-connected production apps |
| Europa Hub | 2046399126 | Mainnet | Ethereum-connected workflows |
| Nebula Hub | 1482601649 | Mainnet | Ethereum-connected workflows |
| Calypso Hub | 1564830818 | Mainnet | Ethereum-connected workflows |

### Deployment Checklist

- Load chain selection from config or environment.
- Validate `block.chainid` before broadcasting.
- Keep RPC and explorer values exact.
- Default to SKALE Base Sepolia for development unless requirements say otherwise.

## References

- [SKALE Connect Docs](https://docs.skale.space/developers/integrate-skale/connect-to-skale)
- [SKALE on Base](https://docs.skale.space/get-started/quick-start/skale-on-base)
