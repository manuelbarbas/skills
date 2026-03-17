# Example: contracts-deployment

## Incorrect

```bash
# Hardcoded production endpoint during development
npx hardhat run scripts/deploy.ts --network skaleMainnet
```

```typescript
const chainId = 123456789;
```

## Correct

```bash
npx hardhat run scripts/deploy.ts --network skaleBaseSepolia
```

## Hardhat Config

```typescript
import { HardhatUserConfig } from "hardhat/config";

const config: HardhatUserConfig = {
    solidity: {
        version: "0.8.24",
        settings: {
            optimizer: { enabled: true, runs: 200 }
        }
    },
    networks: {
        skaleBaseSepolia: {
            url: process.env.SKALE_TESTNET_RPC || "",
            chainId: 324705682,
            accounts: process.env.PRIVATE_KEY ? [process.env.PRIVATE_KEY] : []
        },
        skaleBase: {
            url: process.env.SKALE_MAINNET_RPC || "",
            chainId: 1187947933,
            accounts: process.env.PRIVATE_KEY ? [process.env.PRIVATE_KEY] : []
        }
    }
};

export default config;
```

## Deployment Script

```typescript
import { ethers } from "hardhat";

async function main() {
    const network = await ethers.provider.getNetwork();
    console.log(`Deploying to chain ${network.chainId}`);

    const ContractFactory = await ethers.getContractFactory("MyContract");
    const contract = await ContractFactory.deploy();
    await contract.waitForDeployment();

    console.log(`Deployed to ${await contract.getAddress()}`);
}

main().catch(console.error);
```

## Foundry

```bash
forge script script/Deploy.s.sol \
  --rpc-url $SKALE_TESTNET_RPC \
  --private-key $PRIVATE_KEY \
  --legacy \
  --broadcast
```

## Environment Variables

```bash
SKALE_TESTNET_RPC=https://base-sepolia-testnet.skalenodes.com/v1/jubilant-horrible-ancha
SKALE_MAINNET_RPC=https://skale-base.skalenodes.com/v1/base
PRIVATE_KEY=0x...
```

## References

- [Connect to SKALE](https://docs.skale.space/developers/integrate-skale/connect-to-skale)
- [Setup Foundry](https://docs.skale.space/cookbook/deployment/setup-foundry)
