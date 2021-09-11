# How to run the project with Subgraph

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
