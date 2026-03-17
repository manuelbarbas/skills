# Reference: privacy-bite-protocol

## Why It Matters

BITE Protocol provides threshold encryption for transaction data on SKALE Network. It encrypts transaction data before submitting to the blockchain, keeping it private until decrypted by the consensus committee. This enables truly private transactions without revealing sensitive data to the public mempool or chain state.

## Architecture

```
User Transaction
       │
       ├───> BITE.encryptTransaction()
       │        │
       │        ├── 1. RLP encode (data, to)
       │        ├── 2. AES encrypt with random key
       │        ├── 3. BLS threshold encrypt AES key
       │        └── 4. Create payload: [EPOCH_ID, ENCRYPTED_DATA]
       │
       └───> Send to BITE magic address
              │
              └──> SKALE Consensus (2t+1 nodes)
                     │
                     └──> Threshold decryption
                            │
                            └──> Execute decrypted transaction
```

## Committee Model

| Parameter | Description |
|-----------|-------------|
| Committee size | `3t + 1` nodes |
| Decryption threshold | `2t + 1` nodes |
| Single committee mode | Normal operation |
| Dual committee mode | During rotation (3 min window) |

## Encryption Process

1. **RLP Encoding**: Original `data` and `to` fields are RLP encoded
2. **AES Encryption**: Encoded data encrypted with randomly generated AES key
3. **BLS Threshold Encryption**: AES key encrypted using committee's BLS public key
4. **Final Payload**: RLP format `[EPOCH_ID, ENCRYPTED_BITE_DATA]`

## Gas Limit Behavior

| Scenario | Gas Limit | Notes |
|----------|-----------|-------|
| Not set | 300,000 (default) | BITE auto-sets |
| Manual set | User-defined | Recommended for complex txs |
| estimateGas | Does not work | Never use for BITE txs |

## Committee Rotation

- **Rotation window**: 3 minutes before scheduled rotation
- **Detection**: `getCommitteesInfo()` returns 2 committees
- **Behavior**: Data encrypted with BOTH current and next committee keys
- **Monitoring**: Check array length to detect rotation periods

## Transaction Visibility

| Data Type | Without BITE | With BITE |
|-----------|--------------|-----------|
| Calldata | Public | Encrypted |
| Recipient | Public | Encrypted |
| Sender | Public | Public (signed by sender) |
| Value | Public | Public |
| Decrypted data | N/A | Available via RPC after consensus |

## Use Cases

- Private token transfers
- Confidential voting
- Private bid submissions
- Encrypted function calls
- Sensitive state updates

## Limitations

- Sender address remains public (transaction is signed)
- ETH/value transfers remain visible
- Decrypted data retrievable via JSON-RPC
- Only works on BITE-enabled SKALE chains

## Chain Availability

| Chain | Phase I | Phase II (CTX) |
|-------|---------|----------------|
| SKALE Base Sepolia | ✅ | ❌ |
| SKALE Base | ✅ | ❌ |
| BITE sandbox environment | Public docs do not list it in the Phase I quick-start table | Public CTX docs use it |

## References

- [BITE TypeScript Library](https://github.com/skalenetwork/bite-ts)
- [npm package](https://www.npmjs.com/package/@skalenetwork/bite)
- [SKALE Network Documentation](https://docs.skale.space)
