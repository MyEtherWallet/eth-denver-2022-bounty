
# BOUNTY: Add Balance Changes Graphs 
## Overview
This bounty relates to MEW's block explorer [EthVM.com](https://www.ethvm.com/). It's front-end code base can be found [here](https://github.com/EthVM/EthVM). One of the many features of EthVM is tracking the balance changes for each and every token that a given address has received or transferred, throughout the entire history of that address.  For this bounty you will be creating a UI element, which will create time series graphs displaying the balance history for a particular address/token pair for a given time frame.

## Reward: $1,000
The reward will be split between in-person and virtual contestants.

## Specs:
The main requirement is to use the `getTimeseriesData()` query provided by our [GraphQL API](https://api.ethvm.com/). You can choose to create a separate web app or you can use our block explorer and  implement it directly within the transaction's details page.

`getTimeseriesData()`:
Returns all balances changes from [unix timestamp](https://en.wikipedia.org/wiki/Unix_time) time A to time B. Note that it will only return actual balance changes; you will still have to interpolate the rest of the points on the front-end to get visually-accurate graphs. For example, if you are requesting data for the last month, but only get one point, it means the balance did not change. The graph should still be aesthetically pleasing, so additional points will have to be interpolated into the resulting array.

|Param | Type | Desc
| ------ | ------ | ------ |
| key | string | contains prefix, coin type, and address |
| scale| enum | time frames: seconds, minutes, hours, days |
| fromT | number | starting unix timestamp |
| toT| number | ending unix timestamp|

_How to construct key:_
``key = `${prefix}-${contract}-${owner}``

`${prefix}` can be one of the following:
- `'ACCOUNT_BALANCE_PREFIX_AVG'` - Average balance for the data points (useful when scale is day)
- `'ACCOUNT_BALANCE_PREFIX_MIN'` - Minimum balance 
- `'ACCOUNT_BALANCE_PREFIX_MAX'` - Max balance (useful to get accurate balances when scale is smaller than day)

`${contract}` represents which token you are fetching. To fetch the balance for ETH use `0xETH`, for all other tokens provide hex address strings.

`${owner}` represents which address you are getting the balance for.

_How to use scale:_
There are 4 time scale enum options: **seconds, minutes, hours, days**. For example, if you specify the prefix `ACCOUNT_BALANCE_PREFIX_MIN`, scale `day`, You will receive the minimum balance registered for that day.

_Example:_

```
query{
  getTimeseriesData(key: "ACCOUNT_BALANCE_PREFIX_MAX-0xETH-0xDECAF9CD2367cdbb726E904cD6397eDFcAe6068D", scale: minutes, toT: 1643875660, fromT: 1643875030) {
    items {
        value
        timestamp
    }
    nextKey
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
To submit your implementation, create a pr to the EthVM block explorer. If you are making a separate web app, you can directly put your app folder in the core next to the `newclient` folder, otherwise you can directly edit base code.



