## 1. Basic setup

- create new file called `.env` in the root directory (for hardhat config)
  - in `.env` put:
    PRIVATE_KEY=[account private key from running `npx hardhat node`]
    RINKEBY_INFURA_API_KEY=[from your Infura Project ID]
- follow the steps below (#2)
  - we recommend putting each command in its own terminal window for error troubleshooting

# How to run the project with the Categories Subgraph

## Running it locally

### Install Docker

Install **docker** and **docker-compose** for your system

### Install the Graph tool

Install the **graph-cli** tool from TheGraph:

```
$ npm install -g graph-cli
```

### Install dependencies

```
$ yarn install
```

or

```
npm i
```

### Edit the graph local config

Edit the _docker-compose.yml_ file and change the IP address in

```
ethereum: 'mainnet:http://192.168.1.137:8545'
ethereum: 'mainnet:http://174.62.110.140'
```

to your machine IP address

### Run the scripts in order

- Run the local hardhat node:

```
$ yarn 01-hardhat-node
```

- In another shell run the docker container for the local graph node:

```
$ yarn 02-graph-start-local
```

- In another shell migrate the contracts to the local node:

```
$ yarn 03-migrate
```

- Generate the code for the subgraph: (creates `/generated`)

```
$ yarn 04-graph-codegen
```

- Create the subgraph in the local node:

```
$ yarn 05-graph-create-local
```

- Deploy the subgraph to the local node

```
$ yarn 06-graph-deploy-local
```

- Run the Next application

```
$ yarn 07-start
```

If you run into errors on step 02 with your docker containers, delete the `/data` folder. It is a cache from previous running tests

## Deployment

#### Gitpod (optional, would recommend using Vercel)

To deploy this project to Gitpod, follow these steps:

1. Click this link to deploy

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#github.com/dabit3/polygon-ethereum-nextjs-marketplace)

2. In **pages/index.js**, pass in the RPC address given to you by GitPod to the call to `JsonRpcProvider` function:

```javascript
/* update this: */
const provider = new ethers.providers.JsonRpcProvider();

/* to this: */
const provider = new ethers.providers.JsonRpcProvider(
  "https://8545-youendpoint.gitpod.io/"
);
```

3. Import the RPC address given to you by GitPod into your MetaMask wallet

![MetaMask RPC Import](wallet.png)

### Hardhat Configuration

To deploy to Polygon test or main networks, update the configurations located in **hardhat.config.js** to use a private key and, optionally, deploy to a private RPC like Infura.

```javascript
require("@nomiclabs/hardhat-waffle");
const fs = require("fs");
const privateKey =
  fs.readFileSync(".secret").toString().trim() || "01234567890123456789";

// infuraId is optional if you are using Infura RPC
const infuraId = fs.readFileSync(".infuraid").toString().trim() || "";

module.exports = {
  defaultNetwork: "hardhat",
  networks: {
    hardhat: {
      chainId: 1337,
    },
    mumbai: {
      // Infura
      // url: `https://polygon-mumbai.infura.io/v3/${infuraId}`
      url: "https://rpc-mumbai.matic.today",
      accounts: [privateKey],
    },
    matic: {
      // Infura
      // url: `https://polygon-mainnet.infura.io/v3/${infuraId}`,
      url: "https://rpc-mainnet.maticvigil.com",
      accounts: [privateKey],
    },
  },
  solidity: {
    version: "0.8.4",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
};
```

If using Infura, update **.infuraid** with your [Infura](https://infura.io/) project ID.

# NFT-Marketplace
