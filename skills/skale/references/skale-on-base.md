# Reference: skale-on-base

> **Preferred chain for new projects.** Default to SKALE Base chains unless BITE Phase II (CTX) is required or Ethereum integration is explicitly needed.

## Why It Matters

SKALE Base is a SKALE Chain deployed on Base blockchain. Understanding the correct chain configuration, RPC endpoints, and bridged tokens is essential for building applications that work correctly on SKALE Base.

## Chain Information

### SKALE Base Mainnet

| Property | Value |
|----------|-------|
| Name | SKALE Base |
| RPC URL | `https://skale-base.skalenodes.com/v1/base` |
| WSS URL | `wss://skale-base.skalenodes.com/v1/ws/base` |
| Explorer | `https://skale-base-explorer.skalenodes.com/` |
| Portal | `https://base.skalenodes.com/chains/base` |
| Chain ID | 1187947933 |
| Chain ID Hex | 0x46cea59d |
| Native Token | CREDIT |
| Decimals | 18 |

### SKALE Base Testnet (Base Sepolia)

| Property | Value |
|----------|-------|
| Name | SKALE Base Sepolia |
| RPC URL | `https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha` |
| Explorer | `https://base-sepolia-testnet-explorer.skalenodes.com/` |
| Portal | `https://base-sepolia.skalenodes.com` |
| Faucet | `https://base-sepolia-faucet.skale.space` |
| Chain ID | 324705682 |
| Chain ID Hex | 0x135A9D92 |
| Native Token | CREDIT |
| Decimals | 18 |

## Bridged Tokens

### Mainnet Tokens

| Token | Symbol | Base Address | SKALE Base Address |
|-------|--------|--------------|-------------------|
| USDC | USDC.e | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` | `0x85889c8c714505E0c94b30fcfcF64fE3Ac8FCb20` |
| USDT | USDT | `0xfde4C96c8593536E31F229EA8f37b2ADa2699bb2` | `0x2bF5bF154b515EaA82C31a65ec11554fF5aF7fCA` |
| WBTC | WBTC | `0x0555E30da8f98308EdB960aa94C0Db47230d2B9c` | `0x1aeeCFE5454c83B42d8A316246CAc9739E7f690e` |
| WETH | WETH | `0x4200000000000000000000000000000000000006` | `0x7bD39ABBd0Dd13103542cAe3276C7fA332bCA486` |
| ETH (ERC-20) | ETHC | Native ETH (Gas) | `0xD2Aaa00700000000000000000000000000000000` |

### Testnet Tokens

| Token | Symbol | Base Sepolia Address | SKALE Base Sepolia Address |
|-------|--------|---------------------|---------------------------|
| SKALE | SKL | `0xC20874EB2D51e4e61bBC07a4E7CA1358F449A2cF` | `0xaf2e0ff5b5f51553fdb34ce7f04a6c3201cee57b` |
| USDC | USDC.e | `0x036CbD53842c5426634e7929541eC2318f3dCF7e` | `0x2e08028E3C4c2356572E096d8EF835cD5C6030bD` |
| USDT | USDT | `0x0eda7df37785f570560dA74ceCFD435AB60D84a8` | `0x3ca0a49f511c2c89c4dcbbf1731120d8919050bf` |
| WBTC | WBTC | `0xC3893AEC98b41c198A11AcD9db17688D858588Bc` | `0x4512eacd4186b025186e1cf6cc0d89497c530e87` |
| WETH | WETH | `0x4200000000000000000000000000000000000006` | `0xf94056bd7f6965db3757e1b145f200b7346b4fc0` |
| ETH (ERC-20) | ETHC | Native ETH (Gas) | `0xD2Aaa00700000000000000000000000000000000` |

## Chain Notes

- **ETH Bridging**: When bridging ETH from Base to SKALE Base, ETH is locked as native ETH (msg.value) and minted as an ERC-20 called ETHC on SKALE Base. Display it as ETH in frontend.
- **WETH vs ETHC**: Both are ERC-20 on SKALE Base, but deposited differently. WETH is deposited as WETH on Base.
- **Testnet Access**: SKALE Base Sepolia testnet is 100% permissionless.
- **Canonical Addresses**: CREATE2Factory, SingletonFactory, Multicall3, and Permit2 are deployed to their canonical addresses.
- **Additional Tokens**: Join SKALE Telegram to request additional tokens on testnet.
- **No Factory Contracts**: Current SKALE Base does not have standard factory contracts deployed (CREATE2Factory exists but not Uniswap-style factory).

## Deployment

### CREDIT Purchase

SKALE Base uses a subscription-based pricing model with CREDIT tokens for chain operations.

### sFUEL Distribution

Your dApp needs to distribute sFUEL to users since:
- sFUEL has no market value (cannot be purchased by users)
- Users need sFUEL to transact on SKALE Base
- You distribute sFUEL, not users purchasing gas

## Environment Variables

```bash
# SKALE Base Mainnet
SKALE_BASE_MAINNET_RPC=https://skale-base.skalenodes.com/v1/base
SKALE_BASE_MAINNET_CHAIN_ID=1187947933
SKALE_BASE_MAINNET_EXPLORER=https://skale-base-explorer.skalenodes.com

# SKALE Base Testnet
SKALE_BASE_TESTNET_RPC=https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha
SKALE_BASE_TESTNET_CHAIN_ID=324705682
SKALE_BASE_TESTNET_EXPLORER=https://base-sepolia-testnet-explorer.skalenodes.com
SKALE_BASE_TESTNET_FAUCET=https://base-sepolia-faucet.skale.space
```

## References

- [SKALE Base Documentation](https://docs.skale.space/get-started/quick-start/skale-on-base)
- [SKALE Base Portal (Mainnet)](https://base.skalenodes.com/chains/base)
- [SKALE Base Portal (Testnet)](https://base-sepolia.skalenodes.com)
- [SKALE Discord](https://discord.gg/skale)
