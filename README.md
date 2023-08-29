# A HTLC Demo

> @author: wlh
>
> This is a hashed time lock demo with simple web client and solidity contract for EVM based blockchain

## Getting started

#### 准备环境

* Ganache

首先你需要安装 [Ganache](https://trufflesuite.com/ganache/), by

```shell
npm install -g ganache-cli
```

* Truffle

接着你需要安装 [Truffle](https://trufflesuite.com/docs/truffle/), by

```shell
npm install -g truffle
```

#### 安装依赖

打开工程目录运行以下命令

```shell
npm install
```

#### 运行区块链

打开一个终端窗口在工程根目录下运行以下命令

```shell
npm run ganache-chainA
```

打开另一个终端窗口在工程根目录下运行以下命令

```shell
npm run ganache-chainB
```

#### 单元测试

在本地启动区块链后，在工程根目录运行以下命令

```shell
npm run test
```

## 教程

#### Run The Dapp

web客户端是用vue实现的，它实现了在两个基于EVM的区块链之间进行代币转换的简单的功能，采用哈希时间锁的方式实现。在本地启动了两个区块链网络（A和B）之后,还需要在两个网络上部署HTLC智能合约， 在工程根目录下打开文件 `truffle-config.js` , 你可以像下面那样修改文件内容

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

然后运行以下命令在chainA上部署合约

```shell
truffle deploy
```

然后记录下合约地址contract address(A)

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

然后再次更新文件 `truffle-config.js`, 内容如下

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

再次运行命令 `truffle deploy` , 记录合约地址 contract address(B)

在完成以上的步骤后，你还需要更新文件 `.env.development`

```
NODE_ENV='development'

VUE_APP_WEB3A_URL='http://127.0.0.1:8545' # this is chainA's web3 url
VUE_APP_WEB3B_URL='http://127.0.0.1:1723' # chainB
VUE_APP_HTLC_ADDRA='0x6EE0a6bdE20567B80F07e02113583a34082CA463' # this is chainA's HTLC contract address
VUE_APP_HTLC_ADDRB='0x31920CE32269aE5a2f836455EB8C2cd1e2F2D493' # chainB

```

在工程跟目录下运行以下命令

```shell
npm run dev
```

如果一切正常，你将看到输出以下信息:

> DONE  Compiled successfully in 4067ms
>                                                                                              
>  App running at:
>
>  - Local:   http://localhost:80/ 
>  - Network: http://192.168.244.1:80/

然后你可以在你的浏览器打开链接, 4个功能分别对应以下四个路由接口:

> * /newTx
> * /getTx
> * /withdraw
> * /refund
