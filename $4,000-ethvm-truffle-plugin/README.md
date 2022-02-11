<img src="../assets/ethvm-logo.png" width="80px"/>

# BOUNTY: Create Truffle plugin
## Overview
Our team recently created [a project](https://github.com/EthVM/evm-source-verification) that holds verified contract information and the added ability to verify contracts on the fly using [EthVM's API](https://api.ethvm.com/). Create a [Truffle](https://trufflesuite.com/index.html) plugin to verify smart contracts so it is available for the whole world without any restrictions on Github.

## Reward: $4,000
The reward will be split between in-person and virtual contestants.

## Specs:
This Truffle plugin should generate `input.json` and `configs.json` files based on [Solidity standard JSON-input-output interface](https://docs.soliditylang.org/en/develop/using-the-compiler.html#compiler-input-and-output-json-description). Please note that `configs.json` is equivalent to Solidity's `output.json`. Using the `verifyContract()` mutation from our API will trigger contract verification.

`verifyContract()`:
Triggers contract verification.

|Param | Type | Desc
| ------ | ------ | ------ |
|contractData| string | stringified object with configs and input params |

```
contractData = JSON.stringify({
    configs: string
    input: string
  })
 ```

_Example of verifyContract() mutation:_

```
import { GraphQLClient, gql } from "graphql-request";
import fs from "fs";
const graphQLClient = new GraphQLClient("https://api.ethvm.com");
const mutation = gql`
  mutation verifyContract($contractData: String!) {
    verifyContract(contractData: $contractData) {
      error
      status
      url
    }
  }
`;
const contractData = {
  configs: JSON.parse(fs.readFileSync("./configs.json", { encoding: "utf8" })),
  input: JSON.parse(fs.readFileSync("./input.json", { encoding: "utf8" })),
};
const variables = {
  contractData: JSON.stringify(contractData),
};
graphQLClient.request(mutation, variables).then((data) => {
  console.log(JSON.stringify(data, undefined, 2));
});
```
For information to create a Truffle plugin, please read the official [Truffle documentations](https://trufflesuite.com/docs/truffle/getting-started/writing-external-scripts.html#creating-a-custom-command-plugin).

You can find examples for `input.json` and `configs.json `files [here](https://github.com/MyEtherWallet/eth-denver-2022-bounty/tree/main/plugin-helpers/contract-verify-sample).

You can locate [unverified contracts here](https://github.com/MyEtherWallet/eth-denver-2022-bounty/tree/main/plugin-helpers/unverified-contracts), that you can use for development and testing


## Judging Criteria
- Technical Difficulty and Accuracy (100%) — How clean and accurate the the implementation is.

## Submission
To submit your implementation, create a pr to the EthVM block explorer. If you are making a separate web app, you can directly put your app folder in the core next to the newclient folder, otherwise you can directly edit base code.

_Special Thanks to [Sourcify](https://sourcify.dev/) and [Etherscan](https://etherscan.io/) for helping us gather some contract addresses and information._
