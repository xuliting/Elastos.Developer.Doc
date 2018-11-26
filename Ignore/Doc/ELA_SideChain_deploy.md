1. Prepare `SideChain` and `Arbiter` binaries and configuration files

`https://github.com/elastos/Elastos.ELA.SideChain`
`https://github.com/elastos/Elastos.ELA.Arbiter`

Compile the executable with the code for the `release_v0.0.1` branch, copy the configuration file template and the `ela-cli` executable

> The test environment can use at least two nodes, each of which deploys the `Arbiter` and `Side` applications. It is recommended to use a minimum of 5 `Arbiter` nodes and more `Side` nodes in the production application environment.

Configuration file example:

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

> More than the main chain of the no-chain environment, the configuration of the `Arbiters` project, which is the `Public Key` of the main account in all `Arbiter` nodes.

> Usually the production environment uses the `20xxx` port; the test environment uses the `21xxx` port; other test environments use the `23xxx` port.

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

> Here the `MainChainFoundationAddress` `FoundationAddress` address must be set to this fixed base address (or change the source code)

> All nodes of the SideChain should be set to not automatically mine

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

> Here the `FoundationAddress` address must be set to this fixed base address (or change the source code)

> `GenesisBlock` To configure the `SideChain` creation block (block height is 0) `Hash`; the acquisition method is described later.

> `KeystoreFile` is to be configured as the `keystore.dat` file of the sub-account generated by the `Arbiter` node; the generation method is described later.

2. Start SideChain to obtain SideChain creation block Hash

```bash
./did > /dev/null &                                 //The SideChain default file name is side here renamed to did

curl http://localhost:20604/api/v1/block/hash/0

./ela-cli wallet -g <hash>
```

> Fill the creation block `hash` obtained above into the corresponding position of the `Arbiter` configuration file.

> Use the `ela-cli wallet -g <Hash>` command to generate a cross-chain recharge address starting with `X`, which needs to be used as the `Output` address for cross-chain recharge.

3. Generate `Aribter` primary and secondary accounts and launch

```bash
./ela-cli wallet -c -p elastos --name keystore.dat          // Master account, you need to fill in the address in the main chain configuration file
./ela-cli wallet -c -p elastos --name keystore-did.dat      // Sub-account, file name to be configured to the Arbiter configuration file

./ela-cli wallet --reset
./ela-cli wallet -l         //After recharging, you need to synchronize the account amount to the local database.

./arbiter -p elastos > /dev/null &
```

> To recharge the account after generating the primary and secondary accounts 1 ELA or more, **Additional** be sure to use `ela-cli` to synchronize the account balance to the local database file

4. Deploy another SideChain based on this main chain environment

* Currently you need to edit the `Elastos.ELA.SideChain` project source to generate a different starting block `hash` when the new SideChain executable starts.

* Change the line `Timestamp: uint32(time.Unix(time.Date(2018, time.June, 30, 12, 0, 0, 0, time.UTC). Unix(), 0).Unix()),` in file `Estos.ELA.SideChain/blockchain/blockchain.go`, you can change any time parameter.

* Start SideChain to get the starting block hash

* Create an account corresponding to this SideChain on all `arbiter` nodes: `./ela-cli wallet -c --name keystore-another.dat` and recharge all accounts above 1 ELA.

* Add the following configuration to the `"SideNodeList"` configuration item in the `arbiter` configuration file:

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

> `IpAddress` indicates the address of a SideChain node; `GenesisBlock` indicates the SideChain creation block `hash`; `PayToAddr` indicates the address of the SideChain transaction fee, which must be filled in at this time.

* Then restart `Arbiter`
