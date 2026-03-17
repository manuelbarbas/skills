# Example: random-number-generation

## Incorrect

```solidity
// Using Chainlink VRF - unnecessary on SKALE
import "@chainlink/contracts/src/v0.8/VRFConsumerBase.sol";

contract BadRandom is VRFConsumerBase {
    bytes32 internal keyHash;
    uint256 internal fee;

    function getRandom() external returns (uint256) {
        // Requires callback, extra gas, external dependency
        requestRandomness(keyHash, fee);
    }
}
```

```solidity
// Using blockhash - predictable and insecure
contract PredictableRandom {
    function getRandom() external view returns (uint256) {
        return uint256(keccak256(abi.encodePacked(
            block.timestamp,
            block.prevrandao,
            msg.sender
        )));
        // Miners can manipulate this
    }
}
```

## Correct

### Using Library

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@dirtroad/skale-rng/contracts/RNG.sol";

/// @title Contract using SKALE RNG library
/// @notice Generate random numbers natively on SKALE
contract MyRandomContract is RNG {

    /// @notice Generate a random number between 0 and 2^256-1
    function generateRandom() external view returns (uint256) {
        return getRandomNumber();
    }

    /// @notice Generate a random number between min and max (inclusive)
    function generateRandomInRange(uint256 min, uint256 max) external view returns (uint256) {
        return getNextRandomRange(min, max);
    }

    /// @notice Generate random array index
    function randomIndex(uint256 arrayLength) external view returns (uint256) {
        require(arrayLength > 0, "Empty array");
        return getNextRandomRange(0, arrayLength - 1);
    }
}
```

### Direct Precompile

```solidity
// Using direct precompile call (no library dependency)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract DirectRNG {

    /// @notice Get random number from SKALE precompile at 0x18
    function getRandomNumber() public view returns (uint256) {
        bytes32 randomValue;
        assembly {
            let freemem := mload(0x40)
            if iszero(staticcall(gas(), 0x18, 0, 0, freemem, 32)) {
                invalid()
            }
            randomValue := mload(freemem)
        }
        return uint256(randomValue);
    }

    /// @notice Get random number in range [min, max]
    function getRandomInRange(uint256 min, uint256 max) public view returns (uint256) {
        require(min <= max, "Invalid range");
        uint256 random = getRandomNumber();
        return min + (random % (max - min + 1));
    }
}
```

## Usage Examples

### Coin Flip

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@dirtroad/skale-rng/contracts/RNG.sol";

contract CoinFlip is RNG {
    enum CoinSide { Heads, Tails }

    event CoinFlipped(address indexed player, CoinSide result);

    /// @notice Flip a coin randomly
    function flip() external returns (CoinSide) {
        uint256 random = getRandomNumber();
        CoinSide result = (random % 2 == 0) ? CoinSide.Heads : CoinSide.Tails;

        emit CoinFlipped(msg.sender, result);
        return result;
    }

    /// @notice Flip multiple coins at once
    function flipMultiple(uint256 count) external returns (uint256 headsCount) {
        uint256 heads;
        for (uint256 i = 0; i < count; i++) {
            uint256 random = getRandomNumber();
            if (random % 2 == 0) {
                heads++;
            }
        }
        return heads;
    }
}
```

### Lottery

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@dirtroad/skale-rng/contracts/RNG.sol";

contract SKALELottery is RNG {
    struct Ticket {
        address owner;
        uint256 number;
    }

    uint256 public ticketPrice;
    uint256 public prizePool;
    uint256 public winningNumber;
    bool public lotteryActive;
    mapping(uint256 => Ticket) public tickets;
    uint256 public totalTickets;

    event TicketPurchased(uint256 indexed ticketId, address indexed buyer);
    event LotterySettled(uint256 winningNumber, address winner);

    constructor(uint256 _ticketPrice) {
        ticketPrice = _ticketPrice;
        lotteryActive = true;
    }

    /// @notice Buy a lottery ticket
    function buyTicket(uint256 luckyNumber) external payable {
        require(msg.value >= ticketPrice, "Insufficient payment");
        require(lotteryActive, "Lottery not active");

        tickets[totalTickets] = Ticket({
            owner: msg.sender,
            number: luckyNumber
        });

        prizePool += msg.value;
        emit TicketPurchased(totalTickets, msg.sender);
        totalTickets++;
    }

    /// @notice Draw winning number and pick winner
    function drawWinner() external {
        require(lotteryActive, "Already settled");
        require(totalTickets > 0, "No tickets");

        // Generate random winning number
        winningNumber = getRandomNumber() % totalTickets;

        // Find winner
        address winner = tickets[winningNumber].owner;

        // Transfer prize
        payable(winner).transfer(prizePool);

        lotteryActive = false;
        emit LotterySettled(winningNumber, winner);
    }
}
```

### NFT Random Attributes

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@dirtroad/skale-rng/contracts/RNG.sol";
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract RandomNFT is ERC721, RNG {
    struct NFTAttributes {
        uint256 strength;
        uint256 agility;
        uint256 intelligence;
        uint256 luck;
    }

    mapping(uint256 => NFTAttributes) public attributes;
    uint256 private nextTokenId;

    event NFTMinted(uint256 indexed tokenId, NFTAttributes attributes);

    constructor() ERC721("RandomNFT", "RNFT") {}

    /// @notice Mint NFT with random attributes
    function mint() external returns (uint256) {
        uint256 tokenId = nextTokenId++;

        // Generate random attributes (1-100)
        attributes[tokenId] = NFTAttributes({
            strength: getNextRandomRange(1, 100),
            agility: getNextRandomRange(1, 100),
            intelligence: getNextRandomRange(1, 100),
            luck: getNextRandomRange(1, 100)
        });

        _safeMint(msg.sender, tokenId);
        emit NFTMinted(tokenId, attributes[tokenId]);

        return tokenId;
    }
}
```

## How SKALE RNG Works

```
Block Validation Process:
┌─────────────────────────────────────────────────────────┐
│ SKALE Chain Block Signing                              │
│                                                         │
│  Multiple nodes sign the block                         │
│       ↓                                                 │
│  Signatures are combined to create block signature    │
│       ↓                                                 │
│  Block signature is randomized (depends on signers)   │
│       ↓                                                 │
│  Random value derived from block signature            │
└─────────────────────────────────────────────────────────┘
         ↓
    Precompile 0x18
         ↓
   Random bytes32
```

## Comparison with Alternatives

| Method | Gas Cost | Speed | Security | Dependencies |
|--------|----------|-------|----------|--------------|
| SKALE RNG (0x18) | ~2,000 | Instant | Cryptographic | None (native) |
| Chainlink VRF | ~200,000+ | Callback required | Cryptographic | External oracle |
| block.prevrandao | ~2,000 | Instant | Low (predictable) | None |

## Precompile Details

| Property | Value |
|----------|-------|
| Address | 0x18 |
| Gas Cost | Static call, minimal |
| Return | bytes32 (32 bytes of randomness) |
| Availability | All SKALE chains |

## Installation

```bash
# Install SKALE RNG library (recommended)
npm install @dirtroad/skale-rng
```

## Advanced: Rejection Sampling

```solidity
contract UniformRandom {
    uint256 private constant MAX_RANDOM = type(uint256).max;

    /// @notice Get uniform random in [0, max) without modulo bias
    function uniformRandom(uint256 max) public view returns (uint256) {
        require(max > 0, "Max must be positive");
        require(max <= MAX_RANDOM, "Max too large");

        uint256 factor = (MAX_RANDOM - MAX_RANDOM % max) / max;

        while (true) {
            uint256 random = getRandomNumberInternal();
            if (random / factor == max) {
                return random % max;
            }
            // Rejection: try again with new random value
        }
    }

    function getRandomNumberInternal() private view returns (uint256) {
        bytes32 randomValue;
        assembly {
            let freemem := mload(0x40)
            if iszero(staticcall(gas(), 0x18, 0, 0, freemem, 32)) {
                invalid()
            }
            randomValue := mload(freemem)
        }
        return uint256(randomValue);
    }
}
```

## Integration Checklist

- [ ] Import SKALE RNG library or use precompile directly
- [ ] Test random generation on testnet
- [ ] Validate range inputs to prevent overflow
- [ ] Consider modulo bias for critical applications
- [ ] Use view functions for RNG calls (no gas)
- [ ] Document randomness source in contract comments

## References

- [SKALE RNG Documentation](https://docs.skale.space/cookbook/native-features/rng-get-random-number)
- [SKALE RNG Library (@dirtroad/skale-rng)](https://www.npmjs.com/package/@dirtroad/skale-rng)
- [Solidity Assembly Documentation](https://docs.soliditylang.org/en/v0.8.24/assembly.html)
