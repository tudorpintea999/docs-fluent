# Solidity Developer Guide

## Prerequisites

Before getting started, make sure to install the following:

* npm >= 19
* [Fluent build tool](./#install-fluent-scaffold-cli-tool)

***

## Install Fluent scaffold CLI tool

To install the Fluent scaffold CLI tool, simply run the following command in your terminal:

```bash
cargo install hellofluent
```

To create a project, run the following in your terminal:

```bash
hellofluent
```

This will prompt you to choose from the available setup options. You can opt for either **Hardhat JavaScript or TypeScript**; in this guide, we'll proceed with **JavaScript**.

## **Project Structure**

```
.
├── contracts
│   ├── hello.sol (contains our solidity hello world smart contract)
│   └── hello-v.vy
├── hardhat.config.js (contains Fluent devnet config and plugins)
├── package.json
└── scripts
    ├── deploy.js (deployment script for solidity smart contract)
    └── deployvyper.js
```

## Getting Started

Before we interact with our `helloworld` smart contract, run the below command to install all dependencies in the `package. json` file.

```bash
npm install
```

### Hardhat Configs

To first get a quick sense of Fluent network’s parameters, head over to the `hardhat.config.js` file in the root directory.&#x20;

You will find the configuration for connecting to the Fluent Devnet.

```solidity

require("@nomiclabs/hardhat-ethers");
require("@nomiclabs/hardhat-vyper");
    /**
     * @type import('hardhat/config').HardhatUserConfig
     */
    module.exports = {
      networks: {
        fluent_devnet1: {
          url: '<https://rpc.dev.thefluent.xyz/>', 
          chainId: 1337, 
          accounts : [
            `0x${"ac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"}` ], // Replace with the private key of the deploying account
        },
      },
      solidity: {
        version: '0.8.19', 
      },
      vyper: {
        version: "0.3.0",
      },
    };
  

```

Within the `networks` object, you can see the **`fluent_devnet1`** configuration. This specifies the URL to connect to the Fluent Devnet, along with the chain ID and the accounts available for transactions.

> As mentioned in the comments, you should replace the dummy private key provided with either your own private key or the deploying wallet address.\
> \
> Use [Fluent Faucet](https://faucet.dev.thefluent.xyz/) to request test tokens.

Next, let's explore how you can compile and deploy your first smart contract to the Fluent Devnet.

### Compiling the Smart Contract

If you take a look in the `contracts/` folder, you'll see `hello.sol` file:

```solidity
// SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    contract Hello {
        function main() public pure returns (string memory) {
            return "Hello, Solidity!";
        }
    }
```

To compile it, simply run:

```bash
 npm run compile
```

### Deploying the Solidity contract

In the `scirpts` folder is the deployment script `deploy.js`:

```javascript
const { ethers } = require("hardhat");

async function main() {
    const [deployer] = await ethers.getSigners();
  
    console.log("Deploying contracts with the account:", deployer.address);
  
    const Token = await ethers.getContractFactory("Hello");
    const token = await Token.deploy();

    // Access the address property directly
    console.log("Token address:", token.address);
}

main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exit(1);
    });

```

To deploy the compiled solidity smart contract, run:

```bash
npm run deploy

# Deploying contracts with the account: 
# Token address:
```

To view your deployed Fluent contract, navigate to the [Fluent Devnet Explorer](https://blockscout.dev.thefluent.xyz/). From there, you can input your token address to explore your deployed contract on the Fluent Network.
