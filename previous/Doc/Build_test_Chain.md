# Developers build their own test chains

This part contains nodes such as ela, ID, and SideChain.

## 1 Building the Main Chain

### 1.1 Prepare the ela node program

#### 1.1.1 Compile ela node program

1. Download code <https://github.com/elastos/Elastos.ELA>
2. Prepare the compilation environment and compile the ela node program

    Set the build environment and compile the node program according to [README](https://github.com/elastos/Elastos.ELA/blob/master/README.md).

#### 1.1.2 Download release version

You can get the ubuntu version of the ela program from [Release](https://github.com/elastos/Elastos.ELA/releases).

### 1.2 Modify config.json

1. config.json as a template

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

2. According to the operating environment, modify Magic, SeedList, Port, FoundationAddress, etc.
3. To run the sidechain, after creating the keystore.dat file for the Arbiter node, modify the Arbiters in config.json to the public key corresponding to the Arbiter node and restart the node.

### 1.3 Run ela node

1. Background start command

    ```bash
    nohup ./ela 2>output 1>/dev/null &
    ```

2. Front-end start command

    ```bash
    ./ela
    ```

### 1.4 Normal operation checkpoint

1. The node height grows automatically, which can be judged by periodically checking the node height.

    ```bash
    curl http://localhost:10334/api/v1/api/v1/block/height
    ```

2. If you run more than 2 ela nodes, you can view the number of node connections.

    ```bash
    curl http://localhost:10334/api/v1/node/connectioncount
    ```

3. View node status.

    ```bash
    curl http://localhost:10334/api/v1/node/state
    ```

### 1.5 Ela node interface document

1. RESTFUL API

    <https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/Elastos_Wallet_Node_API_CN.md>

2. RPC API

    <https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/jsonrpc_apis.md>

## 2 Build a ID Chain

### 2.1 Prepare the IDChain node program

#### 2.1.1 Compile the IDChain node program DID

1. Download code <https://github.com/elastos/Elastos.ELA.SideChain>
2. Prepare the compilation environment and compile the IDChain node program DID

    If the go build environment is not set locally, you can set the build environment by [README@ELA](https://github.com/elastos/Elastos.ELA/blob/master/README.md) and then compile the DID node program according to [README@DID](https://github.com/elastos/Elastos.ELA.SideChain/blob/master/README.md).

    Set the build environment and compile the node program according to [README](https://github.com/elastos/Elastos.ELA/blob/master/README.md).

### 2.2 Prepare config.json

1. SideChain profile template

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

2. According to the environment, modify Magic, SpvMagic, Port, SeedList, MainChainFoundationAddress, FoundationAddress, etc.

### 2.3 Node operation

1. Background start command

    ```bash
    nohup ./did 2>output 1>/dev/null &
    ```

2. Front-end start command

    ```bash
    ./did
    ```

### 2.4 View node running status

1. View node height

    ```bash
    curl http://localhost:10604/api/v1/api/v1/block/height
    ```

2. View Creation Zone Block Hash

    ```bash
    curl http://127.0.0.1:10604/api/v1/block/hash/0
    ```

3. View the number of neighbor nodes

    ```bash
    curl http://localhost:10604/api/v1/node/connectioncount
    ```

## 3 Build other side chains

### 3.1 Prepare the side chain node program

#### 3.1.1 Prepare the node program according to the side chain requirements

### 3.2 Prepare the side chain config.json

Refer to 2.2

### 3.3 Start side chain node

1. Get GenesisBlock Hash

### 3.4 Modify the arbiter configuration

1. Add a keystore file for each arbiter, the password is the same as the other arbiter keystore files.

2. Add a second sidechain information to arbiter's config.json. The content refers to the first information of the SideNodeList. The following is the configuration information of the two sidechains.

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

3. Restart arbiter
