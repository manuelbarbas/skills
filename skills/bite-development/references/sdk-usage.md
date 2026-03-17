# SDK Usage

The `@skalenetwork/bite` TypeScript SDK for encrypted transactions and conditional transactions.

## Installation

```bash
npm install @skalenetwork/bite
# or
bun add @skalenetwork/bite
```

## Initialization

### SKALE Base Mainnet

```typescript
import { BITE } from '@skalenetwork/bite';

const bite = new BITE('https://skale-base.skalenodes.com/v1/base');
```

### SKALE Base Testnet

```typescript
import { BITE } from '@skalenetwork/bite';

const bite = new BITE('https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha');
```

## Encrypted Transactions

### ethers v6

```typescript
import { BITE } from '@skalenetwork/bite';
import { ethers } from 'ethers';

const bite = new BITE(rpcUrl);
const provider = new ethers.JsonRpcProvider(rpcUrl);
const wallet = new ethers.Wallet(privateKey, provider);

async function sendEncryptedTx(to: string, data: string) {
    const encryptedTx = await bite.encryptTransaction({ to, data });

    const tx = await wallet.sendTransaction({
        ...encryptedTx,
        gasLimit: 300_000
    });

    return await tx.wait();
}
```

### viem

```typescript
import { BITE } from '@skalenetwork/bite';
import { createWalletClient, http } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';

const bite = new BITE(rpcUrl);
const account = privateKeyToAccount('0x...' as `0x${string}`);
const client = createWalletClient({
    account,
    transport: http(rpcUrl)
});

async function sendEncryptedTx(to: `0x${string}`, data: `0x${string}`) {
    const encryptedTx = await bite.encryptTransaction({ to, data });

    const txHash = await client.sendTransaction({
        to: encryptedTx.to as `0x${string}`,
        data: encryptedTx.data as `0x${string}`,
        gas: 300_000n
    });

    return txHash;
}
```

## Resources

- **GitHub**: `github.com/skalenetwork/bite-ts`
- **npm**: `@skalenetwork/bite`
