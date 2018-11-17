# Create a transaction

## Create a transaction on the MainChain

### Create a transaction using Utilities.Java

> # Java offline signature tool

## 1 There are two ways to get the jar package

### 1.1 Download the latest version of the jar package

```
URL: https://github.com/elastos/Elastos.ELA.Utilities.Java/releases

Download: Elastos.ELA.Utilities.Java_v0.1.*.jar
```

### 1.2 Download the source code, compile the jar package

```
URL: https://github.com/elastos/Elastos.ELA.Utilities.Java
```

* Compile the jar package
    ```
    File -> Project Structure -> Artifacts -> + -> JAR -> From modules with
    1、-> Main Class
    2、-> extract to the target JAR
    3、-> META-INF PATH （C:\DNA\src\ela_tool\src\main\resources）
    4、ok ->  Include in project build -> Apply ->ok
    ```

* Delete signature jar package dependencies, must be used
    ```
    Command: zip -d Elastos.ELA.Utilities.Java 'META-INF/*.SF' 'META-INF/*.RSA' 'META-INF/*SF'
    ```

### 1.3 Start jar package

```
Recommended java version: 1.8

Start command: java -cp Elastos.ELA.Utilities.v0.1.*.Java  org.elastos.elaweb.HttpServer

Web service default port: 8989, can be modified
```

## 2 MainChain - MainChain transfer

### 2.1 Single sign to create a transaction two ways (automatically get utxo and manually get utxo)

#### 2.1.1 Automatically get utxo, also known as non-offline signature

* Feature

  * Convenient trading, no need to calculate utxo
  * Instead of calculating the amount of the change address, get the utxo sorted from small to large, and automatically calculate the change amount based on the output (outputs) amount.

* Parameter Description

  * The java program amount is the smallest unit of 1 sela (1 ela = 100000000 sela (100 million sela)), can only be a positive integer.
  * The `java-config.json` file needs to be placed in the same directory of the java program, in order to connect to the node to get utxo.
  * Host: Server ip and rpc port where the node program is located.
  * Fee: The transaction fee stipulated by both parties is the same for a single output or multiple output transaction fees for a transaction.
  * Confirmation: The number of times the block confirms the transaction, that is, the number of blocks; 16 confirmed numbers are recommended.
  * PrivateKey: Input (transfer) requires the private key of the address, the java program internally obtains utxo.
  * Outputs: Recharge address and amount
  * ChangeAddress: Change the address after the transfer, the change amount is automatically processed by the java program
  * The input amount is less than the output amount, and the prompt amount is insufficient.

* Interface name: genRawTransactionByPrivateKey

  * java-config.json
    ```json
    {
        "Host": "127.0.0.1:11336",
        "Fee":5000,
        "Confirmation":16
    }
    ```

  * Request
    ```
    {
        "method":"genRawTransactionByPrivateKey",
        "id":0,
        "params":[
            {
                "Transactions":[
                    {
                        "PrivateKeys":[
                            {
                                "privateKey":"5FA927E5664E563F019F50DCD4D7E2D9404F2D5D49E31F9482912E23D6D7B9EB"
                            },
                            {
                                "privateKey":"4C573939323F11BCDB57B61CCE095D4B1E55E986F9944F88072141F3DFA883A3"
                            }
                        ],
                        "Outputs":[
                            {
                                "address":"Eazj14ifau5eH1SP5F8MJRuiSsPMiGbJV1",
                                "amount":28900000
                            },
                            {
                                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                                "amount":60000000
                            }
                        ],
                        "ChangeAddress":"Edi5WWMFBsEL2qgggrFhnJe1HTjDnw447H"
                    }
                ]
            }
        ]
    }
    ```

  * Response
    ```
    {
        "Action": "genRawTransaction",
        "Desc": "SUCCESS",
        "Result": {
            "rawTx": "0200010013323235E5F9463B37EDE39688**********8E7456FDE2554E77E1D9A1AB3360562F1D6FF4BAC",
            "txHash": "A32203B48C740552AF0CDB1E77ECCEBE147C5CDA51B2BD80BA9C59662CDCD322"
        }
    }
    ```

### 2.1.2 Manually get utxo, also known as offline signature

* Feature
  * Offline signature to secure your account

* Parameter Description
  * The java program amount is the smallest unit of 1 sela (1 ela = 100000000 sela (100 million sela)), can only be a positive integer.
  * Need to calculate the change address balance, change the balance = inputs-outputs-fee, write the change address and balance in the last line of outputs.
  * txid: The transaction where the available balance of the address is located, the information returned by the following interface txid is written here.
  * index: The serial number in the transaction where the balance is available, the information returned by the interface vout is `index`.
  * address: The address of outputs is the outgoing address.
  * privateKey: Private key corresponding to the address.
  * amount: Transferred amount, long type.

* listunspent (Get the utxo interface by address)
  * This interface is the ela node rpc interface
   ```
    Get txid、index:

    request

    {
        "method":"listunspent",
        "params":{"addresses": ["8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3", "EeEkSiRMZqg5rd9a2yPaWnvdPcikFtsrjE"]}
    }

    response
    {
        "error": null,
        "id": null,
        "jsonrpc": "2.0",
        "result": [
            {
                "assetid": "a3d0eaa466df74983b5d7c543de6904f4c9418ead5ffd6d25814234a96db37b0",
                "txid": "9132cf82a18d859d200c952aec548d7895e7b654fd1761d5d059b91edbad1768",
                "vout": 0,
                "address": "8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3",
                "amount": "33000000",
                "confirmations": 1102
            },
            {
                "assetid": "a3d0eaa466df74983b5d7c543de6904f4c9418ead5ffd6d25814234a96db37b0",
                "txid": "3edbcc839fd4f16c0b70869f2d477b56a006d31dc7a10d8cb49bd12628d6352e",
                "vout": 0,
                "address": "8ZNizBf4KhhPjeJRGpox6rPcHE5Np6tFx3",
                "amount": "0.01255707",
                "confirmations": 846
            }
        ]
    }
    ```

* Interface name: genRawTransaction

  * Request
    ```
    {
        "method":"genRawTransaction",
        "id":0,
        "params":[
            {
                "Transactions":[
                    {
                        "UTXOInputs":[
                            {
                                "txid":"61c22a83bb96d958f473148fa64f3b2be02653c66ede506e83b82e522200d446",
                                "index":0,
                                "privateKey":"5FA927E5664E563F019F50DCD4D7E2D9404F2D5D49E31F9482912E23D6D7B9EB"
                            },
                            {
                                "txid":"a91b63ba6ffdb13379451895c51abd25c54678bc89268db6e6c3dcbb7bb07062",
                                "index":0,
                                "privateKey":"A65E9FB6735C5FD33F839036B15D2DA373E15AED38054B69386E322C6BE52994",
                                "address":"EgSph8GNaNSMwpv6UseAihsAc5sqSrA7ga"
                            }
                        ],
                        "Outputs":[
                            {
                                "address":"ERz34iKa4nGaGYVtVpRWQZnbavJEe6PRDt",
                                "amount":200
                            },
                            {
                                "address":"EKjeZEmLSXyyJ42xxjJP4QsKJYWwEXabuC",
                                "amount":240
                            }
                        ]
                    }
                ]
            }
        ]
    }
    ```

  * Response
    ```
    {
        "Action":"genRawTransaction",
        "Desc":"SUCCESS",
        "Result":{
            "rawTx":"020001001234333238333AC482F4F********09131B13B648EEF428885A5F8AFB44EE38FAC",
            "txHash":"B14A65207B801E991292FED3A4CAB06E29D54A792115BC3D45B7F8235C1A0CF6"
        }
    }
    ```

## 3. MainChain - SideChain transfer

### 3.1 Single sign to create a transaction two ways (automatically get utxo and manually get utxo)

#### 3.1.1 Automatically get utxo, also known as non-offline signature

* Feature

  * Convenient trading, no need to calculate utxo
  * You do not need to calculate the amount of the change address, get the utxo sorted from small to large, and automatically calculate the change amount based on the output (outputs) amount.

* Parameter Description

  * java program amount is the smallest unit 1 Serra (1 ela = 100000000 sela (100 million sela)), can only be a positive integer
  * java-config.json file needs to be placed in the same directory of the java program, the purpose is to connect the node to get utxo
  * Host: server ip and rpc port where the node program is located
  * Fee: The transaction fee specified by both parties is the same for a single output or multiple output transaction fees for a transaction.
  * Confirmation: the number of times the block confirms the transaction, that is, the number of blocks; 16 confirmed numbers are recommended.
  * PrivateKey: input (inputs) transfer requires the private key of the address, java program internally obtain utxo
  * Outputs: address is the x address generated by the SideChain creation block hash, this transfer is recharge (MainChain - SideChain transfer)
    * If the address is "0000000000000000000000000000000000", this transfer is for the currency (SideChain - MainChain transfer)
    * Use this interface to raise coins, you should pay attention to the port of java-config host as SideChain rpc port
    * amount: the amount transferred out, long type
  * CrossChainAsset: address is the SideChain address, amount <= outputs amount - fee
  * ChangeAddress: the address to change to zero after the transfer, the zero amount of the java program is automatically processed
  * The input amount is less than the output amount, and the prompt amount is insufficient.

* Interface name: genCrossChainRawTransactionByPrivateKey

  * java-config.json
    ```
    {
    "Host": "127.0.0.1:11336",
    "Fee":5000,
    "Confirmation":16
    }
    ```

  * Request
    ```
    {
        "method":"genCrossChainRawTransactionByPrivateKey",
        "id":0,
        "params":[
            {
                "Transactions":[
                    {
                        "PrivateKeys":[
                            {
                                "privateKey":"5FA927E5664E563F019F50DCD4D7E2D9404F2D5D49E31F9482912E23D6D7B9EB"
                            }
                        ],
                        "Outputs":[
                            {
                                "address":"XLC69K4932zZf1SRwJCDbv5HGk7DbDYZ9H",
                                "amount":100000
                            }
                        ],
                        "CrossChainAsset":[
                            {
                                "address":"ESH5SrT7GZ4uxTH6aQF3ne7X8AUzWdREzz",
                                "amount":20000
                            }
                        ],

                        "ChangeAddress":"Edi5WWMFBsEL2qgggrFhnJe1HTjDnw447H"
                    }
                ]
            }
        ]
    }
    ```

  * Response
    ```
    {
        "Desc": "SUCCESS",
        "Action": "genCrossChainRawTransactionByPrivateKey",
        "Result": {
            "rawTx": "02000100132D39353032333632323639300001B037DB964A033990D77CBFD9E9BE08651456BB7C2A0854AE",
            "txHash": "0605EE84FA7C28B353806E00CC40477487586A9A03AAAD7154DBE0AD4197E15F"
        }
    }
    ```

### 3.1.2 Manually get utxo, also known as offline signature

* Feature
  * Offline signature to secure your account

* Parameter Description
  * java program amount is the smallest unit 1 Serra (1 ela = 100000000 sela (100 million sela)), can only be a positive integer
  * Need to calculate the change address balance, change the balance = inputs-outputs-fee, write the change address and balance in the last line of outputs
  * txid: the transaction where the available balance of the address is located, the information txid returned by the following interface is written here.
  * index: the serial number in the transaction where the available balance is located. The information returned by the interface vout is `index`.
  * Outputs: address is the x address generated by the SideChain creation block hash, this transfer is recharge (MainChain - SideChain transfer)
    * If the address is "0000000000000000000000000000000000", this transfer is for the currency (SideChain - MainChain transfer)
    * Use this interface to raise coins, you should pay attention to the port of java-config host as SideChain rpc port
    * amount: the amount transferred out, long type
  * privateKey: the private key corresponding to the input address, the same address can only write one private key.
  * CrossChainAsset: address is the SideChain address, amount <= outputs amount - fee

* Interface name: genRawTransaction

  * Request
    ```
    {
        "method":"genCrossChainRawTransaction",
        "id":0,
        "params":[
            {
                "Transactions":[
                    {
                        "UTXOInputs":[
                            {
                                "txid":"3a6b2653dc2dcc0f065e7d955bbe0e3bc71a2d7f44900fc1cb75402af89fd978",
                                "index":1,
                                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB"
                            }
                        ],
                        "Outputs":[
                            {
                                "address":"XKUh4GLhFJiqAMTF6HyWQrV9pK9HcGUdfJ",
                                "amount":70000
                            },
                            {
                                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                                "amount":999800000
                            }
                        ],
                        "PrivateKeySign":[
                            {
                                "privateKey":"4849048B13242F83107CAD9F8C0DF4A3698A0DFB37055F11B91A2E5F044557C2"
                            }
                        ],
                        "CrossChainAsset":[
                            {
                                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                                "amount":60000
                            }
                        ]
                    }
                ]
            }
        ]
    }
    ```

  * Response
    ```
    {
        "Desc": "SUCCESS",
        "Action": "genCrossChainRawTransaction",
        "Result": {
            "rawTx": "02000100132D39353032333632323639300001B037DB964A033990D77CBFD9E9BE08651456BB7C2A0854AE",
            "txHash": "0605EE84FA7C28B353806E00CC40477487586A9A03AAAD7154DBE0AD4197E15F"
        }
    }
    ```

## 4 Send transaction

* The sending transaction is the node `rpc` interface, java does not provide the sending transaction interface.

* sendrawtransaction

  * Request
    ```
    Post request: http://127.0.0.1:20336 (20336 is the node default port)

    {
        "method":"sendrawtransaction",
        "params": ["xxxxxx"]
    }
    ```

  * Response
    ```
    {
        "result":"764691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c02",
        "id": null,
        "jsonrpc": "2.0",
        "error": null
    }
    ```

## 5 web rpc interface

### 5.1 decodeRawTransaction（Anti-parse rawTransaction）

* Request
    ```
    {
        "method":"decodeRawTransaction",
        "id":0,
        "params":[
            {
                "RawTransaction":"02000100142D37323733373**********54E77E1D9A1AB3360562F1D6FF4BAC"
            }
        ]
    }
    ```

* Response
    ```
    {
        "Action":"decodeRawTransaction",
        "Desc":"SUCCESS",
        "Result":{
            "UTXOInputs":[
                {
                    "Txid":"22BADE15481F1AF8240993207E1DF61144A7776E6087994D240917A887F72052"
                }
            ],
            "Outputs":[
                {
                    "Address":"Eazj14ifau5eH1SP5F8MJRuiSsPMiGbJV1",
                    "Value":2999000000000000
                }
            ]
        }
    }
    ```

### 5.2 genPrivateKey（Generate private key）

* Request
    ```
    {
        "method":"genPrivateKey",
        "id":0,
        "params":[

        ]
    }
    ```

* Response
    ```
    {
        "Action":"genPrivateKey",
        "Desc":"SUCCESS",
        "Result":"94F2D1492963E991EA2878C55754293A627277108C2205C7F0EBC592896726D8"
    }
    ```

### 5.3 genPublicKey（Generate public key）

* Request
    ```
    {
        "method":"genPublicKey",
        "id":0,
        "params":[
            {
                "PrivateKey":"4EA80EDBFC783A19FAC1072D15893AC7A20B4EDE1402FD57DE76D02EA61E28E4"
            }
        ]
    }
    ```

* Response
    ```
    {
        "Action":"genPublicKey",
        "Desc":"SUCCESS",
        "Result":"03B462F4DB3F67A6A71E51BF3034A183022F092E8E6ED0C91F139E4871F5BA0B57"
    }
    ```

### 5.4 genAddress（Generate address）

* Request
    ```
    {
        "method":"genAddress",
        "id":0,
        "params":[
            {
                "PrivateKey":"4EA80EDBFC783A19FAC1072D15893AC7A20B4EDE1402FD57DE76D02EA61E28E4"
            }
        ]
    }
    ```

* Response
    ```
    {
        "Action":"genAddress",
        "Desc":"SUCCESS",
        "Result":"EPUhMEA8RVxqMEvxGDtC95Cwmm1gjtcsB3"
    }
    ```

### 5.5 gen_priv_pub_addr（Generate private key, public key, address）

* Request
    ```
    {
        "method":"gen_priv_pub_addr",
        "id":0,
        "params":[

        ]
    }
    ```

* Response
    ```
    {
        "Action":"genAddress",
        "Desc":"SUCCESS",
        "Result":{
            "PrivateKey":"579750E68061727B023FD0AB8A5ABFEE9FC00491220BA2C82402463E5AF3E84A",
            "PublicKey":"0278421F86F850D73A458680EEA36B49679CD09BE3F0D56E969AF8F0761E94BC46",
            "Address":"EZ4u7ewRX3LhUCJYZGENpRVPbeCWU2AdXQ"
        }
    }
    ```

### 5.6 checkAddress (Check address)

* Request
    ```
    Check address support map format and array format

    {
        "method":"checkAddress",
        "id":0,
        "params":[
            {
                "Addresses":[
                    {
                        "address":"EXgtxGg4ep6vM6uCqWuxkP9KG4AGFyufZz"
                    },
                    {
                        "address":"1C1mCxRukix1KfegAY5zQQJV7samAciZpv"
                    },
                    {
                        "address":"8Frmgg4KMudMEPc5Wow5tYXH8XBgctT8QT"
                    },
                    {
                        "address":"XQd1DCi6H62NQdWZQhJCRnrPn7sF9CTjaU"
                    }
                    ]
            }
        ]
    }

    or

    {
        "method":"checkAddress",
        "id":0,
        "params":[
            {
                "Addresses":["EXgtxGg4ep6vM6uCqWuxkP9KG4AGFyufZz","1C1mCxRukix1KfegAY5zQQJV7samAciZpv","8Frmgg4KMudMEPc5Wow5tYXH8XBgctT8QT","XQd1DCi6H62NQdWZQhJCRnrPn7sF9CTjaU"]
            }
        ]
    }
    ```

* Response
    ```
    {
        "Action": "checkAddress",
        "Desc": "SUCCESS",
        "Result": {
            "EXgtxGg4ep6vM6uCqWuxkP9KG4AGFyufZz": true,
            "1C1mCxRukix1KfegAY5zQQJV7samAciZpv": false,
            "8Frmgg4KMudMEPc5Wow5tYXH8XBgctT8QT": true,
            "XQd1DCi6H62NQdWZQhJCRnrPn7sF9CTjaU": false
        }
    }
    ```
>


### 5.7 genGenesisAddress (creation block hash generation x address)

* Request
    ```
    {
        "method":"genGenesisAddress",
        "id":0,
        "params":[
            {
                "BlockHash":"56be936978c261b2e649d58dbfaf3f23d4a868274f5522cd2adb4308a955c4a3"
            }
        ]
    }
    ```

* Response
    ```
    {
        "Desc": "SUCCESS",
        "Action": "genGenesisAddress",
        "Result": "XKUh4GLhFJiqAMTF6HyWQrV9pK9HcGUdfJ"
    }
    ```

* Create a transaction using ela-cli

  * [ela-cli_Client_usage_MainChain](Doc/ela-cli_Client_usage_MainChain.md)


#### 3.4.2. Create a transaction on the SideChain

* Create a transaction using ela-cli
  * [ela-cli_Client_usage_SideChain](Doc/ela-cli_Client_usage_SideChain.md)
