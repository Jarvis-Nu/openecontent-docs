---
description: >-
  Remember we want to pass our name, age and country? Well in this segment we
  are going to learn just that
---

# Modify studio ui

To keep things simple we are going to be building a button that when clicked adds a name, age and country to the smart contract you created, indexed and quarriable by your subgraph, if you did not change your smart-contract network you will need some optimism goerli tokens else you will need some of the tokens of the network you redeployed your smart contract to.

## Open the ui folder

You can do this by running

```
// Open ui folder from the root folder
cd ui
```

## Set your abi

Place the **FULL** abi **(remember in the customizing subgraph section we only placed a part of the full abi file, we can do the same here and make make extra changes but there is no need for that)** you got from redeploying your smart contract in the utils/abis folder

## Modify your connectContract.ts file

You can find your connectContract.ts file in the utils folder open it up and set the following

```
// custom connectContract.ts file

import contractAbi from "./abis/nameOfAbi.json"
import { ethers } from "ethers"

function connectContract() {
    const contractAddress = "yourSmartContractAddress"
    let contract
    try {
        const { ethereum }: any = window
        const { abi } = contractAbi
        if (ethereum) {
            const provider = new ethers.providers.Web3Provider(ethereum)
            const signer = provider.getSigner()
            contract = new ethers.Contract(contractAddress, abi, signer)
        }
        else {
            console.log("Ethereum object does not exist")
        }
    } catch (error) {
        console.log("Error: ", error)
    }
    return contract
}

export default connectContract
```

## Build your frontend

The button only works when connected to a wallet, so we need a connect wallet button (which we will get from rainbowkit) and the button that calls our smart contract, so we replace the code in our index.tsx folder with this

```
// Our custom frontend
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useConnectModal } from '@rainbow-me/rainbowkit';
export const Home() {
    //check if wallet has been connected
    const { openConnectModal } = useConnectModal();
    return(
        <div>
            {/** dynamically render connect button and add button */}
            {
                openConnectModal ? <ConnectButton /> : <button>Add data to blockchain</button>
            }
        </div>
    )
}
```

When the button is clicked we want it to pass "Supreme" as name, "20" as age and "Nigeria" as country, you can build this information to come from input fields in your own project but this is just a super example.

Let's call our smart contract and pass on the data

```
// Our custom frontend
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useConnectModal } from '@rainbow-me/rainbowkit';
import connectContract from '../utils/connectContract'
import { useState } from 'react'

export const Home() {
    //check if wallet has been connected
    const { openConnectModal } = useConnectModal();
    //store success state
    const [successful, setSuccessful] = useState(false)
    async function publish() {
        const contract = connectContract()
        try {
            const txn = await contract.createNewPost("Supreme", "20", "Nigeria")
            if (txn.wait()) {
                setSuccessful(true)
            }
        } catch(error) {
            console.log(error)
        }
    }
    return(
        <div>
            {/** dynamically render connect button and add button */}
            {
                openConnectModal ? <ConnectButton /> : <button>{
                    successful ? "Posted !" : "Add data to blockchain"
                }/button>
            }
        </div>
    )
}
```



## Hurray!!! You just modified your the studio ui🎉🎉🎉
