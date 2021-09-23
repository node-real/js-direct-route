<p align="center">
  <img src="assets/logo/web3js.jpg" width="200" alt="web3.js" />
</p>
# NodeJs SDK of DirectRoute

## What is Direct Route 

NodeReal MEV is a permissionless, transparent service for efficient MEV extraction on EVM blockchains. It achieves following goals:

1. **Transaction privacy**. Transactions submitted through MEV can never be detected by others before they have been included in a block.
2. **First-price sealed-bid auction**. It allows users to privately communicate their bid and granular transaction order preference.
3. **No paying for failed transactions**. Losing bids are never included in a block, thus never exposed to the public and no need to pay any transaction fees.
4. **Bundle transactions**. Multiple transactions are submitted as a bundle, the bundle transactions are all successfully validated on chain in the same block or never included on chain at all.
5. **Efficiency**. MEV extraction is performed without causing unnecessary network or chain congestion.

## Installation

### Node

```bash
npm install @node-real/web3
```

### Yarn

```bash
yarn add @node-real/web3
```

```bash
npm run build
```

## Usage

Init Client

```js
// Init client
const Web3 = require('@node-real/web3');

var directRouteEndPoint = "https://api.nodereal.io/direct-route";
let web3 = new Web3(directRouteEndPoint);
```

1, Query suggested bundle price
`var price = await directClient.eth.getBundlePrice();`

2, Send bundle 

```js
const tx1 = {
        'from': account1,
        'to': account2,
        'gas': 23000,
        'gasPrice': price,
        'data': rpcClient.utils.toHex(data1),
        'chainId': 56,
    };
const tx2 = {
    'from': account2,
    'to': account1,
    'gas': 23000,
    'gasPrice': price,
    'data': rpcClient.utils.toHex(data2),
    'chainId': 56,
};

const privateKey1 = '';
const privateKey2 = '';
const signedTx1 = await rpcClient.eth.accounts.signTransaction(tx1, privateKey1);
const signedTx2 = await rpcClient.eth.accounts.signTransaction(tx2, privateKey2);

var myDate = new Date();
const maxTime = Math.floor(myDate.getTime() / 1000) + 80;
const minTime = Math.floor(myDate.getTime() / 1000) + 20;

console.log(maxTime, minTime);

const bundleArgs = {
    'txs': [signedTx1.rawTransaction, signedTx2.rawTransaction],
    'minTimestamp': minTime,
    'maxTimestamp': maxTime,
    'revertingTxHashes': [signedTx2.transactionHash],
};
const bundleHash = await directClient.eth.sendBundle(bundleArgs);
```

After the bundle is successfully submitted, you may need wait at lest 3-60 seconds before the transaction been verified on chain

3, Query bundle

`const queryBundle = await directClient.eth.getBundleByHash(bundleHash);`

## SDK example

1. `getBundlePriceDemo`. The bundle price is volatile according to the network congestion, the demo shows you how to get proper bundle price.
2. `sendBUSDByBundleDemo`. In this case, we use two accounts to send BUSD to each other, the second transaction is allowed to be failed, and the bundle should be verified on chain during [now+20 second, now+80 second]. This case shows you how to interact with smart contract through direct-route, and how to control the timing to be verified.

If you want to try with above examples, what you need to do is just to replace the private keys of `account1` and `account2` in `bundle_example.js`

## Building

### Requirements

-   [Node.js](https://nodejs.org)
-   [npm](https://www.npmjs.com/)

```bash
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
```



