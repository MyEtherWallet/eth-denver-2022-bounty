<img src="../assets/ethvm-logo.png" width="80px"/>

# BOUNTY: Add Tx Balance State Changes
## Overview
This bounty pertains to MEW's block explorer [EthVM.com](https://www.ethvm.com/). Its front end code base can be found [here](https://github.com/EthVM/EthVM). For this bounty you will be creating a UI element, which will show balance state changes for all addresses involved in the transaction.

## Reward: $1,000
The reward will be split between in-person and virtual contestants.

## Specs:
The main requirement is to use the `getTransactionStateDiff()` query provided by our [GraphQL API](https://api.ethvm.com/). You can choose to create a separate web app or you can use our block explorer and  implement it directly within the transaction's details page.

`getTransactionStateDiff()`:
Returns ETH balance changes for all addresses involved in the transaction. It will return an object array, each with the properties: **to, from, owner**. 

- `to` - balance after tx execution
- `from` - balance before tx execution
- `owner` - address who's balance was changed

|Parameter | Type | Description
| ------ | ------ | ------ |
|hash| string | transaction hash |

_Example:_

```
query {
  getTransactionStateDiff(hash: "0xc6a11db8b97f870596556e8635bdc8cf78ebf7c0ef704236a0d27b8083cfe742"){
    to
    from
    owner
  }
}
```
If you are using the [block explorer front-end](https://github.com/EthVM/EthVM), you will need to add a `.env` file in the `/newclient` directory for it to work properly during development with the following content:
```
VUE_APP_PUBLIC_URL='www.ethvm.com/'
VUE_APP_HTTP_LINK=https://api.ethvm.com
VUE_APP_WS_CLIENT=wss://apiws.ethvm.com
VUE_APP_OPENSEA_API = https://nft.mewapi.io
VUE_APP_SENTRY_SECURITY_DSN=''
VUE_APP_ETH_NETWORK=''
```

## Judging Criteria
- Creativity (30%) — How original and novel the submission is.
- Technical Difficulty and Accuracy (30%) — How clean and accurate the the implementation is.
- User Experience (40%) — How intuitive and understandable the feature implementation is for potential users.

## Submission
To submit your implementation, create a pr to the EthVM block explorer. If you are making a separate web app, you can directly put your app folder in the core next to the newclient folder, otherwise you can directly edit base code.



