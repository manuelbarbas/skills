# Example: bite-conditional-transactions

## Available Chains

| Chain | CTX Support | Notes |
|-------|-------------|-------|
| BITE V2 Sandbox 2 | ✅ | Public docs use this sandbox and note that access is coordinated with the SKALE team |
| SKALE Base | ❌ | Public docs do not list CTX support here |
| SKALE Base Sepolia | ❌ | Public docs do not list CTX support here |

## Compiler Requirements

```toml
[profile.default]
solc_version = "0.8.27"
evm_version = "istanbul"
```

## Minimal Contract

```solidity
pragma solidity >=0.8.27;

import { Address } from "@openzeppelin/contracts/utils/Address.sol";
import { BITE } from "@skalenetwork/bite-solidity/BITE.sol";
import { IBiteSupplicant } from "@skalenetwork/bite-solidity/interfaces/IBiteSupplicant.sol";

contract SimpleSecret is IBiteSupplicant {
    using Address for address payable;

    bytes public decryptedMessage;

    function revealSecret(bytes calldata encrypted) external payable {
        bytes[] memory encryptedArgs = new bytes[](1);
        encryptedArgs[0] = encrypted;

        bytes[] memory plaintextArgs = new bytes[](0);

        address payable ctxSender = BITE.submitCTX(
            BITE.SUBMIT_CTX_ADDRESS,
            msg.value / tx.gasprice,
            encryptedArgs,
            plaintextArgs
        );

        ctxSender.sendValue(msg.value);
    }

    function onDecrypt(
        bytes[] calldata decryptedArgs,
        bytes[] calldata
    ) external override {
        decryptedMessage = decryptedArgs[0];
    }
}
```

## Flow

```text
1. Encrypt message off-chain
2. Pass encrypted bytes into a contract
3. Contract calls BITE.submitCTX(...)
4. Consensus decrypts on the next block
5. onDecrypt(...) receives decrypted arguments
```

## References

- [Conditional Transactions](https://docs.skale.space/developers/bite-protocol/conditional-transactions)
- [BITE API](https://docs.skale.space/developers/bite-protocol/bite-api-and-faqs)
