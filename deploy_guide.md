# 跨链步骤

docs: 
https://docs.chain.link/ccip/tutorials/evm/cross-chain-tokens/register-from-eoa-burn-mint-foundry

## 准备环境变量 

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
```bash
forge script script/DeployToken.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

https://sepolia.etherscan.io/address/0xaec6775f47a9b0120c340c3bebd0c0f86c6d9b18

 

```bash
forge script script/DeployToken.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

https://sepolia.basescan.org/address/0x2de3e0200e022057decfa6692d30b073eeadfd70


## 部署 Token Pool
```bash
forge script script/DeployBurnMintTokenPool.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

https://sepolia.etherscan.io/address/0xbe04ec9d394083461d00384521cb14c1caecbf06

```bash
forge script script/DeployBurnMintTokenPool.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

https://sepolia.basescan.org/address/0x84bf546bd2e33e539cb58068f6d0f0137e2333b8

## Claim Admin Role 
```bash
forge script script/ClaimAdmin.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/ClaimAdmin.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```


## Accept Admin Role 
```bash
forge script script/AcceptAdminRole.s.sol  --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/AcceptAdminRole.s.sol  --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```


## 设置 Token Pool
```bash
forge script script/SetPool.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/SetPool.s.sol  --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

##  Apply Chain Updates
```bash
forge script script/ApplyChainUpdates.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

```bash
forge script script/ApplyChainUpdates.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

## Mint Tokens on Source Chain
```bash
forge script script/MintTokens.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

https://sepolia.etherscan.io/token/0xaec6775f47a9b0120c340c3bebd0c0f86c6d9b18

## 跨链转账
```bash
forge script script/TransferTokens.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast 
```

https://ccip.chain.link/msg/9306a633fa5ede826b1f01206296ffa021be91b659463c7778628610ec78374b