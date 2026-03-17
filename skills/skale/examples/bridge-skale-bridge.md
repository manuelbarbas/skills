# Example: bridge-skale-bridge

## Incorrect

```typescript
// Direct token transfer is not a bridge operation
await token.transfer(skaleAddress, amount);
```

```typescript
// Do not assume a bridge SDK exists unless it is already present in the project
```

## Correct

```typescript
import { Contract, JsonRpcProvider, Wallet } from "ethers";

const ERC20_ABI = [
    "function approve(address spender, uint256 amount) external returns (bool)"
];

// Replace this with the exact ABI from the official bridge docs for your source network.
const BRIDGE_ABI = [
    "function depositERC20(address token, uint256 amount, bytes32 targetChain, address receiver) external"
];

async function bridgeToSKALE(
    rpcUrl: string,
    privateKey: string,
    tokenAddress: string,
    bridgeAddress: string,
    amount: bigint,
    targetChain: string,
    receiver: string
) {
    const provider = new JsonRpcProvider(rpcUrl);
    const signer = new Wallet(privateKey, provider);

    const token = new Contract(tokenAddress, ERC20_ABI, signer);
    const bridge = new Contract(bridgeAddress, BRIDGE_ABI, signer);

    await (await token.approve(bridgeAddress, amount)).wait();
    return await (await bridge.depositERC20(tokenAddress, amount, targetChain, receiver)).wait();
}
```

## Bridge Operations

| Direction | Mechanism |
|-----------|-----------|
| Ethereum -> SKALE | Lock on source, mint or represent on destination |
| SKALE -> Ethereum | Burn or exit on source, unlock on destination |
| SKALE <-> SKALE | Use documented cross-chain messaging and bridge routes |

## Integration Checklist

- Use the official bridge docs for the exact route and ABI.
- Approve ERC-20 spending before the bridge call.
- Use the correct source-network bridge contract address.
- Test the full route on testnet before production.

## References

- [SKALE Bridge Overview](https://docs.skale.space/developers/skale-bridge/overview)
- [Message Proxy](https://docs.skale.space/developers/skale-bridge/messaging-layer/message-proxy)
