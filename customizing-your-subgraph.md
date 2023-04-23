---
description: The subgraph helps you query data added to the blockchain.
---

# Customizing your subgraph

Because we have a super simple and flexible smart contract customizing the smart contract is also very easy to do.

You have to understand the smart contract has a function called&#x20;

```solidity
//the createNewPost function
createNewPost(string[] calldata data)
```

This is the function that collects an array of string data, so even if you have 10 fields in your frontend to pass to the blockchain you can pass them as a string array to the function above.

We are going to talk about how to index your smart contract, here we are assuming you redeployed your smart contract to polygon mumbai although by default the studio is on optimism-georli.

Let us assume you are passing an array like this

```
// sample string of how the array data looks like
const data = ["Supreme", "20", "Nigeria"]
```

Here the array goes name, age and country. This is important because you pass data to the smart contract in a specific pattern which you will define how to index in the subgraph

Here are how you customize your subgraph if you redeployed your smart contract

## Open the subgraph folder

You can do this by running

```
// open subgraph folder
cd subgraph
```

## Install node modules

You can do this by running

```
// Install node modules
npm install
```

## Edit your subgraph.yaml

By default the subgraph looks like

```
// Default subgraph.yaml
specVersion: 0.0.5
schema:
  file: ./schema.graphql
dataSources:
  - kind: ethereum
    name: OpenContent
    network: optimism-goerli
    source:
      address: "0x8632bF5830274db1A43cDc910aDCA981Db6ef0E8"
      abi: OpenContent
      startBlock: 8343465
    mapping:
      kind: ethereum/events
      apiVersion: 0.0.7
      language: wasm/assemblyscript
      entities:
        - Post
      abis:
        - name: OpenContent
          file: ./abis/OpenContent.json
      eventHandlers:
        - event: Post(string[],address)
          handler: handlePost
      file: ./src/open-content.ts
```

If your smart contract is redeployed you need to change

* network (If redeployed to another network)
* address (new smart contract address)
* startBlock (block number the smart contract was created)
* abi file field (change the abi file path from ./abis/OpenContent.json to the relative path of the abi gotten from the smart contract, the abi file should only contain the information on abi field of the abi file gotten from the smart contract)

Your abi should look like this

```
// Sample abi
[
  {
    "anonymous": false,
    "inputs": [
      {
        "indexed": false,
        "internalType": "string[]",
        "name": "data",
        "type": "string[]"
      },
      {
        "indexed": false,
        "internalType": "address",
        "name": "owner",
        "type": "address"
      }
    ],
    "name": "Post",
    "type": "event"
  },
  {
    "inputs": [
      { "internalType": "string[]", "name": "data", "type": "string[]" }
    ],
    "name": "createNewPost",
    "outputs": [],
    "stateMutability": "nonpayable",
    "type": "function"
  }
]
```

## Validate types and generate scripts

You can do this by running

```
// Code to validate types and generate scripts
graph codegen && graph build
```

## Modify mappings

This file can be found in src/open-content.ts, remember the format of the string array data we are using goes name, age, country in position 0, 1, 2 respectively, you replace this code

```
// open-content.ts file currently
import { Post as PostEvent } from "../generated/OpenContent/OpenContent"
import { Post } from "../generated/schema"

export function handlePost(event: PostEvent): void {
  let entity = new Post(
    event.transaction.hash.concatI32(event.logIndex.toI32())
  )
  entity.name = event.params.data[0]
  entity.description = event.params.data[1]
  entity.postThumbnailUrl = event.params.data[2]
  entity.authorName = event.params.data[3]
  entity.authorThumbnailUrl = event.params.data[4]
  entity.content = event.params.data[5]
  entity.date = event.params.data[6]
  entity.owner = event.params.owner

  entity.blockNumber = event.block.number
  entity.blockTimestamp = event.block.timestamp
  entity.transactionHash = event.transaction.hash

  entity.save()
}
```

to

```
// Modified code
import { Post as PostEvent } from "../generated/OpenContent/OpenContent"
import { Post } from "../generated/schema"

export function handlePost(event: PostEvent): void {
  let entity = new Post(
    event.transaction.hash.concatI32(event.logIndex.toI32())
  )
  
  entity.name = event.params.data[0]
  entity.age = event.params.data[1]
  entity.country = event.params.data[2]

  entity.blockNumber = event.block.number
  entity.blockTimestamp = event.block.timestamp
  entity.transactionHash = event.transaction.hash

  entity.save()
}
```

## Create subgraph project

Create subgraph project on the graph protocol's hosted service [here](https://thegraph.com/en/)

## Authenticate subgraph

You can do this by running

```
//authenticate project
graph auth --product hosted-service <ACCESS_TOKEN>
```

## Deploy subgraph

You can do this by running

```
//deploy subgraph
graph deploy --product hosted-service <GITHUB_USER>/<SUBGRAPH NAME>
```

## Hurray!!! You just customized your subgraph🎉🎉🎉
