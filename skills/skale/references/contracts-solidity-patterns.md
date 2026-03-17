# Reference: contracts-solidity-patterns

## Why It Matters

Smart contracts on SKALE follow Solidity best practices but have unique considerations due to zero gas fees, cross-chain messaging, and SKALE-specific features. Following `solidity-dev-tips` ensures secure, efficient contracts.

## Key Patterns

### NatSpec

Document all functions/public vars. Include SKALE-specific notes.

### Custom Errors

Gas-efficient error handling. Zero gas makes this less critical but still best practice.

### Checks-Effects-Interactions

Prevent reentrancy. Essential for all chains.

### Immutable Variables

Gas-optimized constants. Use for chain IDs, config.

### Unchecked Blocks

Safe overflow operations. Useful for loops.

### ReentrancyGuard

Protect external calls. Use OpenZeppelin implementation.

## SKALE-Specific Imports

```solidity
// SKALE Network predeployed contracts
import "@skale/network/contracts/MessageProxy.sol";        // Cross-chain messaging
import "@skale/network/contracts/PredeployedAddressMapper.sol";
import "@skale/network/contracts/KeyStorage.sol";           // Key management

// When using Foundry
// remappings.txt
@skale/network/=lib/skale-solidity/contracts/
```

## Foundry Configuration

```toml
# foundry.toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
solc_version = "0.8.24"
optimizer = true
optimizer_runs = 200

# SKALE testnet configuration
[rpc_endpoints]
skale_testnet = "${SKALE_TESTNET_RPC}"
skale_mainnet = "${SKALE_MAINNET_RPC}"

[doc]
out = "docs"
```

## SKALE-Specific Considerations

| Pattern | SKALE Consideration |
|---------|---------------------|
| Zero gas fees | Can reduce minimum purchase amounts |
| Chain validation | Use `block.chainid` for cross-chain deployments |
| MessageProxy | Handle cross-chain messaging |
| Predeployed contracts | Use SKALE predeployed addresses |

## Testing Pattern

```solidity
// test/MyContract.t.sol
import "forge-std/Test.sol";
import "../src/MyContract.sol";

contract MyContractTest is Test {
    MyContract target;

    function setUp() public {
        // Fork SKALE testnet
        vm.createSelectFork(vm.envString("SKALE_TESTNET_RPC"));
        target = new MyContract();
    }

    function testDeployment() public {
        assertEq(target.CHAIN_ID(), block.chainid);
    }
}
```

## References

- [Solidity Dev Tips](https://github.com/transmissions11/solcurity)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts)
- [SKALE Solidity SDK](https://github.com/skalenetwork/skale-solidity)
- [Foundry Book](https://book.getfoundry.sh/)
