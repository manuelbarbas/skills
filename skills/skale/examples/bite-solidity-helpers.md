# Example: bite-solidity-helpers

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

## Usage Pattern

```solidity
// SPDX-License-Identifier: MIT
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

## Security Check

Track and validate the callback sender using the pattern shown in the official CTX docs when you need stricter authorization around `onDecrypt`.

## Resources

- `github.com/skalenetwork/bite-solidity`
- `references/phase-2-ctx.md`
