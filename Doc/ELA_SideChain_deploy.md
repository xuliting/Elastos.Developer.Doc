1. 准备 `SideChain` 和 `Arbiter` 二进制文件以及配置文件

`https://github.com/elastos/Elastos.ELA.SideChain`
`https://github.com/elastos/Elastos.ELA.Arbiter`

都使用 `release_v0.0.1` 分支的代码编译出可执行文件，拷贝配置文件模版和 `ela-cli` 可执行文件

> 测试环境最少可以使用两个节点，每个节点上都部署 `Arbiter`和 `Side`应用程序， 在生产应用环境建议最少使用 5个 `Arbiter`节点和更多的 `Side`节点

配置文件示例：

**`MainChain Node Configuration`**

```json
{
  "Configuration": {
    "Magic": 20180920,
    "Version": 23,
    "SeedList": [
      "192.168.31.183:20338"
    ],
    "HttpInfoPort": 20333,
    "HttpInfoStart": true,
    "HttpRestPort": 20334,
    "HttpWsPort": 20335,
    "WsHeartbeatInterval": 60,
    "HttpJsonPort": 20336,
    "NoticeServerUrl": "",
    "OauthServerUrl": "",
    "NodePort": 20338,
    "NodeOpenPort": 20866,
    "OpenService": true,
    "PrintLevel": 0,
    "MaxLogsSize": 0,
    "MaxPerLogSize": 0,
    "IsTLS": false,
    "CertPath": "./sample-cert.pem",
    "KeyPath": "./sample-cert-key.pem",
    "CAPath": "./sample-ca.pem",
    "MultiCoreNum": 4,
    "MaxTransactionInBlock": 10000,
    "MaxBlockSize": 8000000,
    "PowConfiguration": {
      "PayToAddr": "Ef9Um6zQg4QiUagDZU7DVW1cJUuyfmQvhn",
      "AutoMining": true,
      "MinerInfo": "ELA",
      "MinTxFee": 100,
      "ActiveNet": "MainNet"
    },
    "Arbiters": [
      "0272baeeae8a3cdfcc78cff17a8fd9838fcb94a621035cf6bfb93b119d49a1c618"
    ]
  }
}
```

> 比无侧链环境的主链多了 `Arbiters` 项目的配置，这里面的内容是所有 `Arbiter` 节点中主账户的 `Public Key`

> 通常生产环境使用 `20xxx` 端口；测试环境使用 `21xxx` 端口；其他测试环境使用 `23xxx` 端口

**`SideChain Node Configuration`**

```json
{
  "Configuration": {
    "Magic": 20180921,
    "SpvMagic": 20180920,
    "Version": 23,
    "SeedList": [
      "192.168.31.186:20608",
      "192.168.31.185:20608"
    ],
    "SpvSeedList": [
      "192.168.31.183:20866",
      "192.168.31.182:20866"
    ],
    "MainChainFoundationAddress": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
    "FoundationAddress": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
    "SpvMinOutbound": 3,
    "SpvMaxConnections": 24,
    "SpvPrintLevel": 1,
    "ExchangeRate": 1.0,
    "MinCrossChainTxFee": 10000,
    "HttpInfoPort": 20603,
    "HttpInfoStart": true,
    "HttpRestPort": 20604,
    "HttpWsPort": 20605,
    "WsHeartbeatInterval": 60,
    "HttpJsonPort": 20606,
    "NoticeServerUrl": "",
    "OauthServerUrl": "",
    "NodePort": 20608,
    "NodeOpenPort": 20607,
    "OpenService": true,
    "PrintLevel": 1,
    "MaxLogsSize": 0,
    "MaxPerLogSize": 0,
    "IsTLS": false,
    "CertPath": "./sample-cert.pem",
    "KeyPath": "./sample-cert-key.pem",
    "CAPath": "./sample-ca.pem",
    "MultiCoreNum": 4,
    "MaxTransactionInBlock": 10000,
    "MaxBlockSize": 8000000,
    "ConsensusType": "pow",
    "PowConfiguration": {
      "PayToAddr": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
      "AutoMining": false,
      "MinerInfo": "ELA",
      "MinTxFee": 100,
      "ActiveNet": "MainNet"
    }
  }
}
```

> 这里 `MainChainFoundationAddress` `FoundationAddress` 地址都要设置为这个固定的基金会地址（或者更改源码）

> 侧链的所有节点应该设置为不自动挖矿

**`Arbiter Node Configuration`**

```json
{
  "Configuration": {
    "Magic": 20180922,
    "Version": 23,
    "SeedList": [
       "192.168.31.185:20538",
       "192.168.31.186:20538"
    ],
    "NodePort": 20538,
    "PrintLevel": 1,
    "SpvPrintLevel": 4,
    "HttpJsonPort": 20536,
    "MainNode": {
      "Rpc": {
        "IpAddress": "192.168.31.181",
        "HttpJsonPort": 20336
      },
      "SpvSeedList": [
        "192.168.31.182:20866",
        "192.168.31.183:20866"
      ],
      "Magic": 20180920,
      "MinOutbound": 3,
      "MaxConnections": 24,
      "FoundationAddress": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta"
    },
    "SideNodeList": [
      {
        "Rpc": {
          "IpAddress": "127.0.0.1",
          "HttpJsonPort": 20606
        },
        "ExchangeRate": 1.0,
        "GenesisBlock": "56be936978c261b2e649d58dbfaf3f23d4a868274f5522cd2adb4308a955c4a3",
        "KeystoreFile": "keystore-did.dat",
        "PayToAddr": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta"
      }
    ],
    "MinThreshold": 100000000,
    "DepositAmount": 100000000,
    "SyncInterval": 1000,
    "SideChainMonitorScanInterval": 1000,
    "ClearTransactionInterval": 60000,
    "MinReceivedUsedUtxoMsgNumber": 1,
    "MinOutbound": 3,
    "MaxConnections": 5,
    "SideAuxPowFee": 50000
  }
}
```

> 这里 `FoundationAddress` 地址都要设置为这个固定的基金会地址（或者更改源码）

> `GenesisBlock` 要配置 `SideChain` 创世块（区块高度为0）的 `Hash` ；获取方法后面有介绍

> `KeystoreFile` 要配置为 `Arbiter` 节点生成的副账户的 `keystore.dat` 文件；生成方法后面有介绍

2. 启动侧链获取侧链创世块Hash

```bash
./did > /dev/null &                                 //侧链默认文件名称是 side这里重命名为 did

curl http://localhost:20604/api/v1/block/hash/0

./ela-cli wallet -g <hash>
```

> 将上面获取到的创世块 `hash` 填入 `Arbiter` 配置文件相应位置

> 使用 `ela-cli wallet -g <Hash>`命令来生成一个以 `X` 开始的跨链充值地址，这个地址在进行跨链充值时候需要作为 `Output` 地址

3. 生成 `Aribter` 主副账户并启动

```bash
./ela-cli wallet -c -p elastos --name keystore.dat          //主账户，需要将地址填入主链配置文件
./ela-cli wallet -c -p elastos --name keystore-did.dat      //副账户，文件名要配置到 Arbiter 配置文件

./ela-cli wallet --reset
./ela-cli wallet -l         //充值之后需要将账户金额同步到本地数据库

./arbiter -p elastos > /dev/null &
```

> 生成主副账户之后要给账户充值 1 ELA以上，**另外** 一定要使用 `ela-cli` 将账户余额同步到本地数据库文件

4. 基于本主链环境部署另外一条侧链

* 目前需要编辑`Elastos.ELA.SideChain`项目源码来让新的侧链可执行文件启动时生成不同的创始块`hash`

* 更改`Elastos.ELA.SideChain/blockchain/blockchain.go` 文件中 `Timestamp:  uint32(time.Unix(time.Date(2018, time.June, 30, 12, 0, 0, 0, time.UTC).Unix(), 0).Unix()),` 这一行，更改任意时间参数即可

* 启动侧链获取创始块hash

* 在所有`arbiter`节点创建对应于本侧链的账户: `./ela-cli wallet -c --name keystore-another.dat`，并为所有账户充值1 ELA以上

* 在`arbiter`配置文件中`"SideNodeList"`配置项中添加如下配置：

```json
{
        "Rpc": {
          "IpAddress": "172.31.44.155",
          "HttpJsonPort": 23616
        },
        "ExchangeRate": 1.0,
        "GenesisBlock": "282d8f4dd626870e3e2a3042d9eb832ba2a769c9272f16f1de11d5b0bcea2a8a",
        "KeystoreFile": "keystore3.dat",
        "PayToAddr": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta"
       }
```

> `IpAddress`表示某一个侧链节点的地址；`GenesisBlock` 表示侧链创始块`hash`；`PayToAddr`表示收取侧链提币交易手续费的地址，目前必须填写这个固定地址

* 然后重启 `Arbiter`