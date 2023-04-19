---
description: >-
  This is the first step in customizing your studio, this is where we call
  functions to add data to the blockchain.
---

# Customizing your smart contract

Surprisingly unless you are redeploying your studio on another network there is no need to edit your smart contract, the smart contract is set to take an array of strings which can be populated with data from your content fields. Just incase you are redeploying your smart contract to another evm network here are the steps to do that

## Open the contracts directory

You can do this by running

```
// open contracts
cd contracts
```

From your root directory.

## Installing node modules

You can do this by running

```
// Install node modules
npm i
```

## Set your environmental variables

Create a .env file and set the following

```
ALCHEMY_URL=alchemyProjectUrl
PRIVATE_KEY=yourPrivateKey
```

{% hint style="info" %}
You will need a few of the tokens on your blockchain of choice to pay gas fees of redeploying the contract
{% endhint %}

## Modify the hardhat.config file

By default the studio is deployed on the optimism network let's say you wanted your studio on the polygon mumbai network your hardhat.config file will look like

```
// Sample hardhat.config file

require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.9",
  networks: {
    hardhat: {
      chainId: 80001,
    },
    mumbai: {
      url: process.env.ALCHEMY_URL,
      accounts: [`0x${process.env.PRIVATE_KEY}`],
      gas: 2100000,
      gasPrice: 8000000000,
    },
  },
};
```

## Redeploy the smart contract

You can do this by running

```
// code to deploy your smart contract
npx hardhat run scripts/deploy.js --network nameofnetwork
```



## To make sure you are following along

We created sample code for all of the below steps, here they are

Your .env file

```properties
ALCHEMY_URL=https://polygon-mumbai.g.alchemy.com/v2/SowjTEYwWfS_zV1WdUgVqoom6zy5Sb7k
PRIVATE_KEY=8e720a36341ac8f2fc3237467a887501d6dc2a20384d10b043e53d3e32538960
```

{% hint style="info" %}
Those are sample values do not use them
{% endhint %}

Your hardhat.config to deploy on polygon mumbai

```
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.9",
  networks: {
    hardhat: {
      chainId: 80001,
    },
    mumbai: {
      url: process.env.ALCHEMY_URL,
      accounts: [`0x${process.env.PRIVATE_KEY}`],
      gas: 2100000,
      gasPrice: 8000000000,
    },
  },
};
```

Redeploy script

```
npx hardhat run scripts/deploy.js --network mumbai
```

## Hurray!!! You just customized your smart contract🎉🎉🎉
