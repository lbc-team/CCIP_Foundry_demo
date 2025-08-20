# 跨链步骤

参考官方文档: 
https://docs.chain.link/ccip/tutorials/evm/cross-chain-tokens/register-from-eoa-burn-mint-foundry

## 准备环境变量 


```
forge install foundry-rs/forge-std
npm install
forge build
```

Example `.env` file to interact with the Sepolia testnet and Base Sepolia testnet:

```bash
PRIVATE_KEY=<your_private_key>
SEPOLIA_RPC_URL=<your_rpc_url_>
BASE_SEPOLIA_RPC_URL=<your_rpc_url_base_sepolia>
ETHERSCAN_API_KEY=<your_etherscan_api_key>
```

给 `PRIVATE_KEY` 对应的账号充值。

## 修改配置 config.json

The `config.json` file within the `script` directory defines the key parameters used by all scripts.

修改 `ccipAdminAddress` 为自己的地址
 



## 部署 Token 

在源链部署 Token 
```bash
forge script script/DeployToken.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

例如部署 Token 为：https://sepolia.etherscan.io/address/0xaec6775f47a9b0120c340c3bebd0c0f86c6d9b18


在目标链部署 Token ：

```bash
forge script script/DeployToken.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

例如部署 Token 为：
https://sepolia.basescan.org/address/0x2de3e0200e022057decfa6692d30b073eeadfd70


## 部署 Token Pool


在源链部署 TokenPool ： 
```bash
forge script script/DeployBurnMintTokenPool.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

Token 跨出时，Token 将在此pool 中燃烧掉，如果不支持 Burn ， 则使用 `DeployLockReleaseTokenPool` 。

示例 TokenPool 地址：https://sepolia.etherscan.io/address/0xbe04ec9d394083461d00384521cb14c1caecbf06

在目标链部署 TokenPool：
```bash
forge script script/DeployBurnMintTokenPool.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

示例 TokenPool 地址：https://sepolia.basescan.org/address/0x84bf546bd2e33e539cb58068f6d0f0137e2333b8

## 在 CCIP 中确认管理员

只有指定的管理员地址能够控制代币与 Pool 关联，设置目标链对应的 Token 等操作，为安全起见，分成了两步`Claim Admin Role` 和 `Accept Admin Role` 。


**Claim Admin Role** 

```bash
forge script script/ClaimAdmin.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/ClaimAdmin.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```


**Accept Admin Role**
```bash
forge script script/AcceptAdminRole.s.sol  --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/AcceptAdminRole.s.sol  --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```


## 设置 Token Pool

在 TokenAdminRegistry合约上 SetPool()， 建立代币与池之间的映射关系。

源链上执行：

```bash
forge script script/SetPool.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

目标链上执行：

```bash
forge script script/SetPool.s.sol  --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

##  配置跨链传输参数

设置本地池与远程链池之间的连接关系， 双向跨链通道的配置。配置跨链传输的速率限制器参数


```bash
forge script script/ApplyChainUpdates.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/ApplyChainUpdates.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

## 在源链上发行一些 Token   

```bash
forge script script/MintTokens.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

https://sepolia.etherscan.io/token/0xaec6775f47a9b0120c340c3bebd0c0f86c6d9b18

## 进行跨链转账

Token 授权给 Router, 构建跨链消息：包括、接收者、tokenAddress、tokenAmount ， 目标链执行需要的 gas 等
通过 router.ccipSend(destchain, message) 进行跨链，返回的 msgid 可以在 https://ccip.chain.link/msg/ 查询状态。




```bash
forge script script/TransferTokens.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast 
```

转账示例链接：
https://ccip.chain.link/msg/9306a633fa5ede826b1f01206296ffa021be91b659463c7778628610ec78374b