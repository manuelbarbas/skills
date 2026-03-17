# Encrypted Transactions

A BITE protocol primitive that encrypts transaction data (calldata + recipient) before submission. The encrypted data is only decrypted by the consensus committee during execution, keeping sensitive information private from the public mempool.

## Available Chains

| Chain | Chain ID | Support |
|-------|----------|---------|
| SKALE Base Sepolia | 324705682 | ✅ |
| SKALE Base | 1187947933 | ✅ |

## Key Points

- Always set `gasLimit` manually - `estimateGas` does not work for encrypted transactions
- Default gas limit: 300,000 (increase for complex transactions)
- Committee rotation: when 2 committees active, SDK handles dual encryption automatically

## Incorrect

```typescript
// Missing gas limit
const encryptedTx = await bite.encryptTransaction({ to, data });
await wallet.sendTransaction(encryptedTx);
// ERROR: estimateGas does not work
```

## Correct

```typescript
import { BITE } from "@skalenetwork/bite";

const bite = new BITE(rpcUrl);

const encryptedTx = await bite.encryptTransaction({
    to: contractAddress,
    data: calldata
});

const tx = await wallet.sendTransaction({
    ...encryptedTx,
    gasLimit: 300000
});
```

## Gas Limit Guidelines

| Scenario | Gas Limit |
|----------|-----------|
| Default | 300,000 |
| Complex tx | 500,000+ |
| estimateGas | ❌ Never use |

Note you can also run a local anvil node OR run the transaction on testnet to estimate gas limit once and then use that hardcoded in production.
SKALE Chains return unused gas to the sender so setting a higher gas limit just requires sufficient gas in the wallet. Any unused will be returned.

## References

- [BITE TypeScript Library](https://github.com/skalenetwork/bite-ts)
- [npm: @skalenetwork/bite](https://www.npmjs.com/package/@skalenetwork/bite)
