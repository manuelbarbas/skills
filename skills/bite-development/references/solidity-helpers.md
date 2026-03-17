# Solidity Helpers

The `@skalenetwork/bite-solidity` package provides precompile wrappers for BITE primitives. Different files support different Solidity versions.

## Installation

### Foundry

```bash
forge install skalenetwork/bite-solidity
echo "@skalenetwork/bite-solidity/=lib/bite-solidity/" >> remappings.txt
```

### Hardhat

```bash
npm install @skalenetwork/bite-solidity
# or
bun add @skalenetwork/bite-solidity
```

## Version-Specific Imports

Select the BITE wrapper based on your contract's Solidity version:

| File | Solidity Version | Notes |
|------|------------------|-------|
| `BITE.sol` | `>=0.8.27` | Latest, uses custom errors |
| `VeryLegacyBITE.sol` | `>=0.8.4` | Uses custom errors |
| `VeryVeryLegacyBITE.sol` | `>=0.8.0` | Uses `require` strings |
| `VeryVeryVeryLegacyBITE.sol` | `>=0.6.0` | Uses `require`, requires `ABIEncoderV2` |

```solidity
// For Solidity >=0.8.27
import { BITE } from "@skalenetwork/bite-solidity/BITE.sol";

// For Solidity >=0.8.4
import { BITE } from "@skalenetwork/bite-solidity/VeryLegacyBITE.sol";

// For Solidity >=0.8.0
import { BITE } from "@skalenetwork/bite-solidity/VeryVeryLegacyBITE.sol";

// For Solidity >=0.6.0
import { BITE } from "@skalenetwork/bite-solidity/VeryVeryVeryLegacyBITE.sol";
pragma experimental ABIEncoderV2;
```

## Interface Import

The `IBiteSupplicant` interface works with `>=0.8.0`:

```solidity
import { IBiteSupplicant } from "@skalenetwork/bite-solidity/interfaces/IBiteSupplicant.sol";
```

## Pattern Overview

See `references/conditional-transactions.md` for a complete Conditional Transactions (CTX) example.

```solidity
pragma solidity >=0.8.27;

import { BITE } from "@skalenetwork/bite-solidity/BITE.sol";
import { IBiteSupplicant } from "@skalenetwork/bite-solidity/interfaces/IBiteSupplicant.sol";

contract MyConditionalTx is IBiteSupplicant {
    function submit(bytes calldata encrypted, bytes[] calldata conditions) external payable {
        bytes[] memory encryptedArgs = new bytes[](1);
        encryptedArgs[0] = encrypted;

        BITE.submitCTX(
            BITE.SUBMIT_CTX_ADDRESS,
            msg.value / tx.gasprice,
            encryptedArgs,
            conditions
        );
    }

    function onDecrypt(
        bytes[] calldata decryptedArgs,
        bytes[] calldata plaintextArgs
    ) external override {
        // Implement condition check and execution logic
    }
}
```

## Resources

- `github.com/skalenetwork/bite-solidity`
- `references/conditional-transactions.md`
