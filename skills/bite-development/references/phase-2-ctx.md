# Rule: bite-conditional-transactions (Phase II - CTX)

## Why It Matters

CTX is a separate BITE capability with its own compiler requirements and callback pattern. The public SKALE docs use a dedicated BITE sandbox for examples and note that access is coordinated with the SKALE team.

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

## Correct Pattern

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

## Checklist

- Use Solidity `>=0.8.27`.
- Set `evm_version = "istanbul"`.
- Use `BITE.submitCTX(...)` from `@skalenetwork/bite-solidity`.
- Implement `onDecrypt(bytes[] calldata, bytes[] calldata)`.
- Test on the documented BITE sandbox environment.

## Resources

- [Conditional Transactions](https://docs.skale.space/developers/bite-protocol/conditional-transactions)
- [BITE API](https://docs.skale.space/developers/bite-protocol/bite-api-and-faqs)
