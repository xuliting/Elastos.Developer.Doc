1. Download or compile ELastos.ELA.SideChain
2. Save the configuration information as a config.json file and place it in the same directory as the side file

    ```json
    {
      "Configuration": {
        "Magic": 20180011,
        "SpvMagic": 2018001,
        "Version": 23,
        "SeedList": [
          "did-testnet-001.elastos.org:21608",
          "did-testnet-002.elastos.org:21608",
          "did-testnet-003.elastos.org:21608",
          "did-testnet-004.elastos.org:21608",
          "did-testnet-005.elastos.org:21608"
        ],
        "SpvSeedList": [
          "127.0.0.1:21338",
          "node-testnet-002.elastos.org:21866",
          "node-testnet-003.elastos.org:21866",
          "node-testnet-004.elastos.org:21866"
        ],
        "MainChainFoundationAddress": "8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3",
        "FoundationAddress": "8NRxtbMKScEWzW8gmPDGUZ8LSzm688nkZZ",
        "SpvMinOutbound": 3,
        "SpvMaxConnections": 10,
        "SpvPrintLevel": 1,
        "ExchangeRate": 1.0,
        "MinCrossChainTxFee": 10000,
        "HttpInfoPort": 21603,
        "HttpInfoStart": true,
        "HttpRestPort": 21604,
        "HttpWsPort": 21605,
        "WsHeartbeatInterval": 60,
        "HttpJsonPort": 21606,
        "NoticeServerUrl": "",
        "OauthServerUrl": "",
        "NodePort": 21608,
        "NodeOpenPort": 21607,
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
          "PayToAddr": "ESHtMtd4v4247fBn3KcDG4pfoCtz51Q6nZ",
          "AutoMining": false,
          "MinerInfo": "DID-testnet",
          "MinTxFee": 100,
          "ActiveNet": "MainNet"
        }
      }
    }
    ```

3. Start the node program ./side or nohup

   ``` shell
   ./side 2>output 1>/dev/null &
   ```