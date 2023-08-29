# A HTLC Demo

> @author: wlh
>
> This is a hashed time lock demo with simple web client and solidity contract for EVM based blockchain

## Getting started

#### Prepare Environment

* Ganache

First of all you need to install [Ganache](https://trufflesuite.com/ganache/), by

```shell
npm install -g ganache-cli
```

* Truffle

Then you have to install [Truffle](https://trufflesuite.com/docs/truffle/), by

```shell
npm install -g truffle
```

#### Install

Open the root directory of the project and run commad

```shell
npm install
```

#### Run Blockchain

You can open a terminal window and go to the root directory of the project and run commad

```shell
npm run ganache-chainA
```

Then open another terminal window and go to the root directory of the project and run commad

```shell
npm run ganache-chainB
```

#### Unit Test

After running a test blockchain, you can open the root directory of the project and run commad

```shell
npm run test
```

## Tutorial

#### Run The Dapp

The web client is writen by VUE,  It provides an easy way to swap tokens between two different EVM based blockchain by using the HTLC, after running two test blockchain (A and B), then you have to deploy HTLC on those two blockchain, you can go to the root directory of the project and update the file `truffle-config.js` , you have to post your two blockchains' info on this file like this

```js
module.exports = {
    networks: {
        /// chainA
        development: {
            host: "localhost",
           port: 8545,
            network_id: "1337"
        }
        /// chainB
         //development: {
         //    host: "localhost",
         //    port: 1723,
         //    network_id: "74"
         //}
    }
}
```

then you can run command to deploy HTLC on chainA

```shell
truffle deploy
```

and record the HTLC contract address(A)

```
...

2_deploy_contracts.js
=====================

   Replacing 'HashedTimeLock'
   --------------------------
   > transaction hash:    0x858bd1ee3d79a987b82f69ce8db3f48b8207554ec9e767dd4adccbd923666f9c
   > Blocks: 0            Seconds: 0
   > contract address:    0x6EE0a6bdE20567B80F07e02113583a34082CA463
   > block number:        19
   > block timestamp:     1693279738
   > account:             0x9e5E62E84Dbd592e9302a661ac89aDB846B2E82e
   > balance:             99.90417506
   > gas used:            1057171 (0x102193)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.02114342 ETH

   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:          0.02114342 ETH

...

```

then you have to update the `truffle-config.js` file again, like this

```js
module.exports = {
    networks: {
        /// chainA
        //development: {
        //    host: "localhost",
        //   port: 8545,
        //    network_id: "1337"
        //}
        /// chainB
         development: {
             host: "localhost",
             port: 1723,
             network_id: "74"
         }
    }
}
```

and run `truffle deploy` again, record the HTLC contract address(B)

after all those steps above, you have to update the file `.env.development`

```
NODE_ENV='development'

VUE_APP_WEB3A_URL='http://127.0.0.1:8545' # this is chainA's web3 url
VUE_APP_WEB3B_URL='http://127.0.0.1:1723' # chainB
VUE_APP_HTLC_ADDRA='0x6EE0a6bdE20567B80F07e02113583a34082CA463' # this is chainA's HTLC contract address
VUE_APP_HTLC_ADDRB='0x31920CE32269aE5a2f836455EB8C2cd1e2F2D493' # chainB

```

then run command at the root directory of the project

```shell
npm run dev
```

If every thing works ok, you may see a notice like this:

> DONE  Compiled successfully in 4067ms
>                                                                                              
>  App running at:
>
>  - Local:   http://localhost:80/ 
>  - Network: http://192.168.244.1:80/

Then you can open the link in your browser, there are 4 routes that correspond to different functions:

> * /newTx
> * /getTx
> * /withdraw
> * /refund
