# Reference: contracts-cross-chain

## Why It Matters

SKALE supports cross-chain messaging between SKALE chains, Ethereum, and Base. Proper patterns ensure messages are delivered, processed, and secured. Incorrect implementation leads to lost messages or failed transfers.

## Architecture

```
┌─────────────────┐         Message Proxy         ┌─────────────────┐
│  SKALE Chain A  │ ──────────────────────────────▶│  SKALE Chain B  │
│   (Sender)      │                                │   (Receiver)    │
└─────────────────┘                                └─────────────────┘
        │                                                   │
        │                                                   │
        └───────────────────┬───────────────────────────────┘
                            │
                    ┌───────▼────────┐
                    │ Message Proxy  │
                    │   (Predeployed) │
                    └────────────────┘
```

## Key Components

| Component | Address | Purpose |
|-----------|---------|---------|
| MessageProxy | Predeployed | Routes messages between chains |
| AddressMapper | Predeployed | Maps contract addresses across chains |
| KeyStorage | Predeployed | Manages cross-chain signing keys |

## Gas Limit Guidelines

| Operation | Recommended Gas Limit |
|-----------|----------------------|
| Token transfer | 100,000 |
| Contract call | 200,000 |
| Complex state change | 300,000-500,000 |
| Batch operations | Calculate: base × count + buffer |

## Safety Checklist

- [ ] Validate sender in `postMessage`
- [ ] Use `msg.sender` check for Message Proxy only
- [ ] Implement replay protection (message hash tracking)
- [ ] Mark messages processed BEFORE handling
- [ ] Use ReentrancyGuard for all external handlers
- [ ] Set appropriate gas limits for target operations
- [ ] Verify chain configuration before sending
- [ ] Handle failed message scenarios

## Testing Cross-Chain

```solidity
// Test with forked SKALE chains
contract CrossChainTest is Test {
    SKALEChainA chainA;
    SKALEChainB chainB;

    function setUp() public {
        // Fork both chains
        vm.createSelectFork(SKALE_A_RPC, blockA);
        chainA = new SKALEChainA();

        vm.createSelectFork(SKALE_B_RPC, blockB);
        chainB = new SKALEChainB();
    }

    function testCrossChainMessage() public {
        // Send from Chain A
        vm.createSelectFork(SKALE_A_RPC, blockA + 1);
        chainA.sendMessage(TEST_DATA);

        // Process on Chain B
        vm.createSelectFork(SKALE_B_RPC, blockB + 1);
        // Simulate Message Proxy call
        vm.prank(MESSAGE_PROXY);
        chainB.postMessage(TEST_DATA, MESSAGE_ID, CHAIN_A_ID, address(chainA));
    }
}
```

## References

- [SKALE Message Proxy Documentation](https://docs.skale.space/developers/skale-bridge/messaging-layer/message-proxy)
- [SKALE Cross-Chain Architecture](https://docs.skale.space/developers/skale-bridge/overview)
- [MessageProxyForMainnet.sol](https://github.com/skalenetwork/skale-solidity)
