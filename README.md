## 拍卖方案

1.用户设置拍卖起始价格，tokenid，公示期持续区块数,拍卖持续区块数，结算持续区块数，最低竞价增幅，竞价倒计时持续区块数，竞价保证金比例

2.公示阶段

    2.1 取消拍卖

3.竞价阶段
    3.1 校验当前是否处于竞价阶段，校验竞价是否达到最低增幅要求
    3.2 修改当前竞价信息，包括价格、竞价人、倒计时截止区块编号
    3.3 退还上次竞价保证金，扣减本次竞价保证金
4.结算阶段
    4.1 校验当前是否处于结算阶段，校验是否有竞价
    4.2 修改拍品状态为已结算
    4.3 从合约账户将保证金转账给拍卖方，剩余待支付余额从竞价胜利者账户转账给拍卖方
    4.4 将拍卖金额转账给拍卖方
5.违约处理
    5.1 竞价胜出者在结算阶段未支付保证金，合约将保证金转账给拍卖方，并将拍品退还给拍卖方
6.拍卖方取消拍卖
    6.1 拍卖方可以在公示期撤回拍卖品
    6.2 拍卖方可以在拍卖结束后撤回拍卖品

7.竞价倒计时方案

    7.1 创建拍卖时设置倒计时时长以及首次竞价信息的截止时间

    7.2 每次竞价后，在竞价信息内标记下次竞价的截止时间



## 还原包

```bash
forge install
```



## 编译

```bash
forge build
```



## 单元测试

```bash
forge test --match-path test/NftAuctionWithCredit.t.sol
```

## 部署

```bash
forge script script/AuctionSystemDeployer.s.sol:AuctionSystemDeployer \
    --rpc-url $SEPOLIA_RPC_URL \
    --env-file .env \
    --broadcast \
    --verify \
    -vvvv
```

## Slither

### 1. 配置文件.slither.config.json

```json
{
    "filter_paths": "node_modules,lib",
    "detectors_to_exclude": ["low-level-calls","different-pragma"]
}
```



### 2. 扫描

```bash
# erc20合约
slither src/Weth.sol --exclude-dependencies --config-file .slither.config.json

#erc721合约
slither src/NftERC20.sol  --exclude-dependencies --config-file .slither.config.json

# 拍卖合约
slither src/NftAuctionWithCredit.sol  --exclude-dependencies --config-file .slither.config.json
```



## myth

分析代码

```bash
myth analyze src/NftAuctionWithCredit.sol
```



## 
