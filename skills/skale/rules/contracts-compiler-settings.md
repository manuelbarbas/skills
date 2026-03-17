# Rule: contracts-compiler-settings

## Why It Matters

SKALE-specific compiler guidance depends on the feature you are using. The public docs clearly define CTX requirements, while general deployment guidance emphasizes legacy transaction format rather than a universal Solidity cap for all contracts.

## Incorrect

```solidity
// Wrong for CTX according to the public docs
pragma solidity ^0.8.24;
```

```toml
[profile.default]
solc_version = "0.8.24"
evm_version = "shanghai"
```

## Correct

```solidity
// Standard SKALE contract
pragma solidity ^0.8.24;
```

```solidity
// CTX contract
pragma solidity >=0.8.27;
```

```toml
[profile.default]
solc_version = "0.8.27"
evm_version = "istanbul"
```

## Context

### Publicly Documented Rules

| Scenario | Requirement |
|----------|-------------|
| Standard SKALE deployment | Use legacy transaction format when deploying |
| CTX contract | Solidity `>=0.8.27` |
| CTX contract | `evm_version = "istanbul"` or lower |

### Foundry Deployment

```bash
forge script script/Deploy.s.sol \
  --rpc-url $SKALE_RPC_URL \
  --private-key $PRIVATE_KEY \
  --legacy \
  --broadcast
```

Use `--slow` when your script sends multiple transactions.

## Best Practices

1. Keep ordinary contracts aligned with your project dependencies and tooling.
2. Treat CTX as a separate compiler profile.
3. Use `--legacy` for SKALE deployments with Foundry.

## References

- [Setup Foundry](https://docs.skale.space/cookbook/deployment/setup-foundry)
- [Conditional Transactions](https://docs.skale.space/developers/bite-protocol/conditional-transactions)
