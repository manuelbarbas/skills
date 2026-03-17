# Conditional Transactions (CTX)

A BITE protocol primitive for executing transactions based on encrypted conditions. Has its own compiler requirements and callback pattern. The public SKALE docs use a dedicated sandbox for examples and note that access is coordinated with the SKALE team.

## Available Chains

| Chain | CTX Support | Notes |
|-------|-------------|-------|
| BITE V2 Sandbox 2 | ✅ | Public docs use this sandbox and note that access is coordinated with the SKALE team |
| SKALE Base | ❌ | Public docs do not list CTX support here |
| SKALE Base Sepolia | ❌ | Public docs do not list CTX support here |

## Compiler Requirements

| Requirement | Value |
|-------------|-------|
| Solidity | `>=0.8.27` |
| EVM Version | `istanbul` |

```toml
[profile.default]
solc_version = "0.8.27"
evm_version = "istanbul"
```

## Example: Time-Locked Message

Execute and reveal a message only after a specified timestamp.

```solidity
pragma solidity >=0.8.27;

import { BITE } from "@skalenetwork/bite-solidity/BITE.sol";
import { IBiteSupplicant } from "@skalenetwork/bite-solidity/interfaces/IBiteSupplicant.sol";

contract TimeLockedMessage is IBiteSupplicant {
    string public revealedMessage;
    uint256 public revealTime;

    event MessageRevealed(string message, uint256 timestamp);

    // Set the time after which message can be revealed
    function setRevealTime(uint256 _revealTime) external {
        revealTime = _revealTime;
    }

    // Submit encrypted message to be revealed when time >= revealTime
    function revealAtTime(bytes calldata encryptedMessage) external payable {
        bytes[] memory encryptedArgs = new bytes[](1);
        encryptedArgs[0] = encryptedMessage;

        bytes[] memory plaintextArgs = new bytes[](1);
        plaintextArgs[0] = abi.encode(revealTime);

        BITE.submitCTX(
            BITE.SUBMIT_CTX_ADDRESS,
            msg.value / tx.gasprice,
            encryptedArgs,
            plaintextArgs
        );
    }

    // Called by BITE after decryption if condition is met
    function onDecrypt(
        bytes[] calldata decryptedArgs,
        bytes[] calldata plaintextArgs
    ) external override {
        // Verify time condition
        uint256 requiredTime = abi.decode(plaintextArgs[0], (uint256));
        require(block.timestamp >= requiredTime, "Too early");

        // Decode and store the revealed message
        revealedMessage = abi.decode(decryptedArgs[0], (string));
        emit MessageRevealed(revealedMessage, block.timestamp);
    }
}
```

## How It Works

1. Deployer sets `revealTime` to a future timestamp
2. User calls `revealAtTime()` with an encrypted message
3. BITE committee decrypts and checks condition: `block.timestamp >= revealTime`
4. If true, `onDecrypt()` executes and emits `MessageRevealed`
5. If false, transaction reverts with "Too early"

## Checklist

- Use Solidity `>=0.8.27`.
- Set `evm_version = "istanbul"`.
- Use `BITE.submitCTX(...)` from `@skalenetwork/bite-solidity`.
- Implement `onDecrypt(bytes[] calldata, bytes[] calldata)`.
- Test on the documented BITE sandbox environment.

## Resources

- [Conditional Transactions](https://docs.skale.space/developers/bite-protocol/conditional-transactions)
- [BITE API](https://docs.skale.space/developers/bite-protocol/bite-api-and-faqs)
