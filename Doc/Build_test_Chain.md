# 开发者自行搭建测试链

该部分包含ela、ID、sidechain等节点。

## 1 搭建主链

### 1.1 准备ela节点程序

#### 1.1.1 编译ela节点程序

1. 下载代码https://github.com/elastos/Elastos.ELA
2. 准备编译环境
3. 编译ela节点程序

#### 1.1.2 下载release版本

### 1.2 修改config.json

1. config.json为模板

	```json
   {
     "Configuration": {
       "Magic": 201801,
	    "Version": 23,
	    "SeedList": [
	      "127.0.0.1:10338",
	      "127.0.0.1:11338"
	    ],
	    "HttpInfoPort": 10333,
	    "HttpInfoStart": true,
	    "HttpRestPort": 10334,
	    "HttpWsPort": 10335,
	    "WsHeartbeatInterval": 60,
	    "HttpJsonPort": 10336,
	    "NodePort": 10338,
	    "NodeOpenPort": 10866,
	    "OpenService": false,
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
	    "MinCrossChainTxFee": 10000,
	    "FoundationAddress":"8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3",
	    "PowConfiguration": {
	      "PayToAddr": "EeEkSiRMZqg5rd9a2yPaWnvdPcikFtsrjE",
	      "AutoMining": true,
	      "MinerInfo": "ELA",
	      "MinTxFee": 100,
	      "ActiveNet": "MainNet"
	    },
	    "Arbiters": [
	      "03e333657c788a20577c0288559bd489ee65514748d18cb1dc7560ae4ce3d45613",
	      "02dd22722c3b3a284929e4859b07e6a706595066ddd2a0b38e5837403718fb047c",
	      "03e4473b918b499e4112d281d805fc8d8ae7ac0a71ff938cba78006bf12dd90a85",
	      "03dd66833d28bac530ca80af0efbfc2ec43b4b87504a41ab4946702254e7f48961",
	      "02c8a87c076112a1b344633184673cfb0bb6bce1aca28c78986a7b1047d257a448"
	    ]
	  }
	}
	```

2. 根据运行环境修改Magic、SeedList、端口及FoundationAddress等
3. 如需运行侧链，在为Arbiter节点创建keystore.dat文件后，并将config.json中Arbiters修改为对应Arbiter节点的公钥，并重启节点。

### 1.3 运行ela节点

1. 后台启动命令

	```bash
	nohup ./ela 2>output 1>/dev/null &
	```

2. 前端启动命令

	```bash
	./ela
	```

### 1.4 正常运行检查点

1. 节点高度自动增长，可通过定期查看节点高度判断

	```bash
	curl http://localhost:10334/api/v1/api/v1/block/height
	```

2. 如运行2个以上ela节点，可查看节点连接数

	```bash
	curl http://localhost:10334/api/v1/node/connectioncount
	```

3. 查看节点状态

	```bash
	curl http://localhost:10334/api/v1/node/state
	```

### 1.5 ela节点接口文档

1. RESTFUL接口
https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/Elastos_Wallet_Node_API_CN.md
2. RPC接口
https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/jsonrpc_apis.md

## 2 搭建ID链

### 2.1 准备IDChain节点程序

编译后程序名为side，建议修改为did

### 2.2 准备config.json

1. sidechain配置文件模板
	```json
	{
	  "Configuration": {
	    "Magic": 201802,
	    "SpvMagic": 201801,
	    "Version": 23,
	    "SeedList": [
	      "127.0.0.1:10608",
	      "127.0.0.1:11608"
	    ],
	    "SpvSeedList": [
	      "127.0.0.1:10866",
	      "127.0.0.1:11866",
	    ],
	    "MainChainFoundationAddress": "8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3",
	    "FoundationAddress": "EHCGDgxxRTj4rTSmZESmVqDHfYPZZWPpFQ",
	    "SpvMinOutbound": 3,
	    "SpvMaxConnections": 10,
	    "SpvPrintLevel": 1,
	    "ExchangeRate": 1.0,
	    "MinCrossChainTxFee": 10000,
	    "HttpInfoPort": 10603,
	    "HttpInfoStart": true,
	    "HttpRestPort": 10604,
	    "HttpWsPort": 10605,
	    "WsHeartbeatInterval": 60,
	    "HttpJsonPort": 10606,
	    "NoticeServerUrl": "",
	    "OauthServerUrl": "",
	    "NodePort": 10608,
	    "NodeOpenPort": 10607,
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
	    "ConsensusType": "pow",
	    "PowConfiguration": {
	      "PayToAddr": "EHka9ktoHWBjjCp1uqwrJ8vWgdRPcTt4im",
	      "AutoMining": false,
	      "MinerInfo": "did",
	      "MinTxFee": 100,
	      "ActiveNet": "MainNet"
	    }
	  }
	}
	```

2. 根据环境修改Magic、SpvMagic、端口、SeedList、MainChainFoundationAddress、FoundationAddress等

### 2.3 节点运行

1. 后台启动命令

	```bash
	nohup ./did 2>output 1>/dev/null &
	```

2. 前端启动命令

	```bash
	./did
	```

### 2.4 查看节点运行状态

1. 查看节点高度
	```bash
	curl http://localhost:10604/api/v1/api/v1/block/height
	```

2. 查看创世区块Hash
	```bash
	curl http://127.0.0.1:10604/api/v1/block/hash/0
	```

3. 查看邻居节点数量
	```bash
	curl http://localhost:10604/api/v1/node/connectioncount
	```

## 3 搭建其他侧链

### 3.1 准备侧链节点程序

#### 3.1.1 根据侧链需求准备节点程序

#### 3.2 准备侧链config.json

参照 2.2

### 3.3 启动侧链节点

1. 获取GenesisBlock Hash

### 3.4 修改arbiter配置

1. 为每个arbiter添加一个keystore文件，密码是与该arbiter其他keystore文件一致
2. 为arbiter的config.json添加第二条侧链信息，内容参照SideNodeList的第一条信息，下面为配置2条侧链的配置信息，可供参考
	```json
	{
	  "Configuration": {
	    "Magic": 201806272,
	    "Version": 0,
	    "SeedList": [
	      "did-regtest-001.elastos.org:22538",
	      "did-regtest-002.elastos.org:22538",
	      "did-regtest-003.elastos.org:22538",
	      "did-regtest-004.elastos.org:22538",
	      "did-regtest-005.elastos.org:22538"
	    ],
	    "NodePort": 22538,
	    "PrintLevel": 1,
	    "SpvPrintLevel": 1,
	    "HttpJsonPort": 22536,
	    "MainNode": {
	      "Rpc": {
	        "IpAddress": "127.0.0.1",
	        "HttpJsonPort": 22336
	      },
	      "SpvSeedList": [
	        "127.0.0.1:22866",
	        "node-regtest-002.elastos.org:22866",
	        "node-regtest-003.elastos.org:22866",
	        "node-regtest-004.elastos.org:22866"
	      ],
	      "Magic": 20180627,
	      "MinOutbound": 4,
	      "MaxConnections": 100,
	      "FoundationAddress": "8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3"
	    },
	    "SideNodeList": [
	      {
	        "Rpc": {
	          "IpAddress": "127.0.0.1",
	          "HttpJsonPort": 22606
	        },
	        "ExchangeRate": 1.0,
	        "GenesisBlock": "56be936978c261b2e649d58dbfaf3f23d4a868274f5522cd2adb4308a955c4a3",
	        "KeystoreFile": "keystore1.dat",
	        "PayToAddr": "ESHtMtd4v4247fBn3KcDG4pfoCtz51Q6nZ"
	      },
	      {
	        "Rpc": {
	          "IpAddress": "127.0.0.1",
	          "HttpJsonPort": 22616
	        },
	        "ExchangeRate": 19.0,
	        "GenesisBlock": "38f8cc21e25161350de4f3e44260b386a8be5239c83c9f8a908d8b46b5f328100",
	        "KeystoreFile": "keystore2.dat",
	        "PayToAddr": "2"
	      }
	    ],
	    "MinThreshold": 1000000,
	    "DepositAmount": 1000000,
	    "SyncInterval": 1000,
	    "SideChainMonitorScanInterval": 1000,
	    "ClearTransactionInterval": 60000,
	    "MinReceivedUsedUtxoMsgNumber": 1,
	    "MinOutbound": 3,
	    "MaxConnections": 8,
	    "SideAuxPowFee": 50000
	  }
	}
	```

3. 重启arbiter