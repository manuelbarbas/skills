# Rule: bite-solidity-helpers

## Why It Matters

The `@skalenetwork/bite-solidity` package is the documented helper library for CTX contracts. Use the public CTX pattern instead of inventing callback signatures or remappings.

## Installation

```bash
forge install skalenetwork/bite-solidity
echo "@skalenetwork/bite-solidity/=lib/bite-solidity/" >> remappings.txt
```

## Core Imports

```solidity
import { BITE } from "@skalenetwork/bite-solidity/BITE.sol";
import { IBiteSupplicant } from "@skalenetwork/bite-solidity/interfaces/IBiteSupplicant.sol";
```

## Compiler Requirements

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
    ) external override {}
}
```

## Resources

- `github.com/skalenetwork/bite-solidity`
- `references/phase-2-ctx.md`
