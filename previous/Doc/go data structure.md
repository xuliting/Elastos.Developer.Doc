# data structure

#### 1.data structure

###### payload Information

```go
type PayloadInfo interface{}

type RechargeToSideChainInfo struct {
    MainChainTransactionHash string `json:"mainchaintxhash"`
}

type CrossChainAssetInfo struct {
    CrossChainAddress string `json:"crosschainaddress"`
	OutputIndex       uint64 `json:"outputindex"`
    CrossChainAmount  string `json:"crosschainamount"` 
}

type TransferCrossChainAssetInfo struct {
    CrossChainAssets []CrossChainAssetInfo `json:"crosschainassets"`
}
```

###### transaction information

```go
type InputInfo struct {
   TxID     string `json:"txid"`
   VOut     uint16 `json:"vout"`
   Sequence uint32 `json:"sequence"`
}

type OutputInfo struct {
   Value      string `json:"value"`
   Index      uint32 `json:"n"`
   Address    string `json:"address"`
   OutputLock uint32 `json:"outputlock"`
}

type AttributeInfo struct {
   Usage AttributeUsage `json:"usage"`
   Data  string         `json:"data"`
}

type ProgramInfo struct {
	Code      string `json:"code"`
	Parameter string `json:"parameter"`
}

type TransactionInfo struct {
   TxId           string          `json:"txid"`
   Payload        PayloadInfo     `json:"payload"`
}
```

###### transaction type

```go
type TransactionType byte

const (
	CoinBase                TransactionType = 0x00
	RegisterAsset           TransactionType = 0x01
	TransferAsset           TransactionType = 0x02
	Record                  TransactionType = 0x03
	Deploy                  TransactionType = 0x04
	SideChainPow            TransactionType = 0x05
	RechargeToSideChain     TransactionType = 0x06
	WithdrawFromSideChain   TransactionType = 0x07
	TransferCrossChainAsset TransactionType = 0x08
)
```

error code and message

```json
const (
	Error                   ErrCode = -1
	Success                 ErrCode = 0
	ErrInvalidInput         ErrCode = 45003
	ErrInvalidOutput        ErrCode = 45004
	ErrAssetPrecision       ErrCode = 45005
	ErrTransactionBalance   ErrCode = 45006
	ErrAttributeProgram     ErrCode = 45007
	ErrTransactionSignature ErrCode = 45008
	ErrTransactionPayload   ErrCode = 45009
	ErrDoubleSpend          ErrCode = 45010
	ErrTxHashDuplicate      ErrCode = 45011
	ErrSidechainTxDuplicate ErrCode = 45012
	ErrMainchainTxDuplicate ErrCode = 45013
	ErrXmitFail             ErrCode = 45014
	ErrTransactionSize      ErrCode = 45015
	ErrUnknownReferedTxn    ErrCode = 45016
	ErrInvalidReferedTxn    ErrCode = 45017
	ErrIneffectiveCoinbase  ErrCode = 45018
	ErrUTXOLocked           ErrCode = 45019
	ErrRechargeToSideChain  ErrCode = 45020

	SessionExpired          ErrCode = 41001
	IllegalDataFormat       ErrCode = 41003
	PowServiceNotStarted    ErrCode = 41004
	InvalidMethod           ErrCode = 42001
	InvalidParams           ErrCode = 42002
	InvalidToken            ErrCode = 42003
	InvalidTransaction      ErrCode = 43001
	InvalidAsset            ErrCode = 43002
	UnknownTransaction      ErrCode = 44001
	UnknownAsset            ErrCode = 44002
	UnknownBlock            ErrCode = 44003
	InternalError           ErrCode = 45002
)

var ErrMap = map[ErrCode]string{
	Error:                   "Unclassified error",
	Success:                 "Success",
	SessionExpired:          "Session expired",
	IllegalDataFormat:       "Illegal Dataformat",
	PowServiceNotStarted:    "pow service not started",
	InvalidMethod:           "Invalid method",
	InvalidParams:           "Invalid Params",
	InvalidToken:            "Verify token error",
	InvalidTransaction:      "Invalid transaction",
	InvalidAsset:            "Invalid asset",
	UnknownTransaction:      "Unknown Transaction",
	UnknownAsset:            "Unknown asset",
	UnknownBlock:            "Unknown Block",
	InternalError:           "Internal error",
	ErrUTXOLocked:           "Error utxo locked",
	ErrInvalidInput:         "INTERNAL ERROR, ErrInvalidInput",
	ErrInvalidOutput:        "INTERNAL ERROR, ErrInvalidOutput",
	ErrAssetPrecision:       "INTERNAL ERROR, ErrAssetPrecision",
	ErrTransactionBalance:   "INTERNAL ERROR, ErrTransactionBalance",
	ErrAttributeProgram:     "INTERNAL ERROR, ErrAttributeProgram",
	ErrTransactionSignature: "INTERNAL ERROR, ErrTransactionSignature",
	ErrTransactionPayload:   "INTERNAL ERROR, ErrTransactionPayload",
	ErrDoubleSpend:          "INTERNAL ERROR, ErrDoubleSpend",
	ErrTxHashDuplicate:      "INTERNAL ERROR, ErrTxHashDuplicate",
	ErrXmitFail:             "INTERNAL ERROR, ErrXmitFail",
	ErrTransactionSize:      "INTERNAL ERROR, ErrTransactionSize",
	ErrUnknownReferedTxn:    "INTERNAL ERROR, ErrUnknownReferedTxn",
	ErrInvalidReferedTxn:    "INTERNAL ERROR, ErrInvalidReferedTxn",
	ErrIneffectiveCoinbase:  "INTERNAL ERROR, ErrIneffectiveCoinbase",
}
```



#### 2.sample recharge to side chain transaction

```json
{
    "txid":"764342ae4a9a60e02a28339b9302871973fd60c25ca79380064dd09dcf14aeb0",
    "hash":"764342ae4a9a60e02a28339b9302871973fd60c25ca79380064dd09dcf14aeb0",
    "size":555,
    "vsize":555,
    "version":0,
    "locktime":0,
    "vin":[],
    "vout":[{
        "value":"0.00100000",
        "n":0,
        "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
        "outputlock":0
    }],
    "blockhash":"9a89993bed88466e91b81cf86ad9ca31613837bfdf823e0f13199a06eddcf8a5",
    "confirmations":4427,
    "time":1536918424,
    "blocktime":1536918424,
    "type":6,
    "payloadversion":0,
    "payload":{
   "mainchaintxhash":"6685c5608de13be95badfd4605bab372276a7cad09202a810169185be31f247d"
    },
    "attributes":[{
        "usage":0,
        "data":"313338333534343230383237303133343633"
    }],
 	"programs":[]
}
```



#### 3.sample withdraw transaction on side chain

```json
{
   "txid":"e30e315681fc104820737baa41e65eb0668d979f81c4169708f72801a0046683",
   "hash":"e30e315681fc104820737baa41e65eb0668d979f81c4169708f72801a0046683",
                "size":282,
                "vsize":282,
                "version":0,
                "locktime":0,
                "vin":[
                    {
       "txid":"5228401cca844cc51b255e675cd56bf8b8bce585b19398958829301b617836b6",
                        "vout":0,
                        "sequence":0
                    }
                ],
                "vout":[
                    {
                        "value":"0.96000000",
                        "n":0,
                        "address":"0000000000000000000000000000000000",
                        "outputlock":0
                    }
                ],
                "blockhash":"d77a9e0db69dd815f2b5c004852ad7b0b8782ba74aa475f6e1cec203b9ab2bb8",
                "confirmations":8,
                "time":1537503765,
                "blocktime":1537503765,
                "type":8,
                "payloadversion":0,
                "payload":{
                    "crosschainassets":[{
           			   "crosschainaddress":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                       "outputindex":0,
                       "crosschainamount":95000000
                    }]
                },
                "attributes":[{
                    "usage":0,
                    "data":"2d36393130383137303436333135373037373130"
                }],
                "programs":[{
 "code":"21037f3caede72447b6082c1e8f7705ffd1ed6e24f348130d34cbc7c0a35c9e993f5ac",
 "parameter":"406c9fa867dfb54051ea75932782914fb2212c8c5f3c46b6a27fb50dadf1ca4ee5e699dcf8b7f098745457dc75d3f41f2263dd46a6a06aebdccac8d668d4429ebd"}]
}
```



# rpc interface

#### 1.getblockcount

description: get block count

argument sample:
```json
{   
  "method":"getblockcount"
}
```
result sample:
```json
{
    "result": 171454,
    "id": null,
    "error": null,
    "jsonrpc": "2.0",
}
```



#### 2.sendtransactioninfo

description: send recharge to side chain transaction to transaction pool

argument sample:

```json
{
  "method": "sendtransactioninfo",
  "params":{
  "info":{
    "txid":"764342ae4a9a60e02a28339b9302871973fd60c25ca79380064dd09dcf14aeb0",
    "hash":"764342ae4a9a60e02a28339b9302871973fd60c25ca79380064dd09dcf14aeb0",
    "size":555,
    "vsize":555,
    "version":0,
    "locktime":0,
    "vin":[],
    "vout":[{
        "value":"0.00100000",
        "n":0,
        "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
        "outputlock":0
    }],
    "blockhash":"9a89993bed88466e91b81cf86ad9ca31613837bfdf823e0f13199a06eddcf8a5",
    "confirmations":4427,
    "time":1536918424,
    "blocktime":1536918424,
    "type":6,
    "payloadversion":0,
    "payload":{
   "mainchaintxhash":"6685c5608de13be95badfd4605bab372276a7cad09202a810169185be31f247d"
    },
    "attributes":[{
        "usage":0,
        "data":"313338333534343230383237303133343633"
    }],
 	"programs":[]
	}
  }
}
```

result sample:

```json
{
    "error":null,
    "id":null,
    "jsonrpc":"2.0",
    "result":["764342ae4a9a60e02a28339b9302871973fd60c25ca79380064dd09dcf14aeb0"]
}
```

if the recharge to side chain transaction double spend, need to return ErrDoubleSpend error

if the recharge to side chain transaction  duplicated, need to return ErrMainchainTxDuplicate error



#### 3.getwithdrawtransaction

description: get transaction information by hash for check tx3

parameters:

| name | type   | description             |
| ---- | ------ | ----------------------- |
| txid | string | the hash of transaction |

arguments sample:

```json
{
  "method": "getwithdrawtransaction",
  "params": {
   		"txid": "e30e315681fc104820737baa41e65eb0668d979f81c4169708f72801a0046683"
  }
}
```

result sample:

```json
{
    "error":null,
    "id":null,
    "jsonrpc":"2.0",
    "result": {
          "txid":"e30e315681fc104820737baa41e65eb0668d979f81c4169708f72801a0046683",
          "crosschainassets":[
             {
                "crosschainaddress":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                "crosschainamount":95000000
             },
             {
                "crosschainaddress":"EYN7QVArMS23YtCxTEbTiy3EfZjD1Vk86M",
                "crosschainamount":5000000
             }
          ]
    }
}
```



#### 4.getwithdrawtransactionsbyheight

description: get withdraw transactions by block height

parameters:

| name   | type    | description               |
| ------ | ------- | ------------------------- |
| height | integer | the height of block chain |

arguments sample:

```json
{
  "method": "getwithdrawtransactionsbyheight",
  "params":{
      "height":"1"
  }
}
```

result sample:

```json
{
    "error":null,
    "id":null,
    "jsonrpc":"2.0",
    "result":[
       {
          "txid":"e30e315681fc104820737baa41e65eb0668d979f81c4169708f72801a0046683",
          "crosschainassets":[
             {
                "crosschainaddress":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                "crosschainamount":95000000
             },
             {
                "crosschainaddress":"EYN7QVArMS23YtCxTEbTiy3EfZjD1Vk86M",
                "crosschainamount":5000000
             }
          ]
       },
       {
          "txid":"6685c5608de13be95badfd4605bab372276a7cad09202a810169185be31f247d",
          "crosschainassets":[
             {
                "crosschainaddress":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                "crosschainamount":1000000
             },
             {
                "crosschainaddress":"EYN7QVArMS23YtCxTEbTiy3EfZjD1Vk86M",
                "crosschainamount":2000000
             }
          ]
       }
    ]
}
```



#### 5.getexistdeposittransactions

description: check deposit transactions and return existed transactions

parameters:

| name | type     | description                    |
| ---- | -------- | ------------------------------ |
| txs  | []string | hashes of deposit transactions |



arguments sample:

```json
{
  "method": "getexistdeposittransactions",
  "params": {
      "txs": [
          "164691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c01",
          "264691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c02",
          "364691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c03"
      ]
  }
}
```

result sample:

```json
{
    "error":null,
    "id":null,
    "jsonrpc":"2.0",
    "result":{
    "txs": [
    	"264691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c02",
        "364691821f937fd566bcf533611a5e5b193008ea1ba1396f67b7b0da22717c03"
     ]
    }
}
```



#### rpc error:

```javascript
{ 
	"jsonrpc": "2.0", 
	"error": {
    	"code": error code, 
    	"message": error message,
        "id": error id,
     }
}
```

