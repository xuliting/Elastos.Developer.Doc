# cli客户端转账-主链交易

## 1. 主链-主链转账

### 1.1 编译cli客户端程序

1. 下载代码: <https://github.com/elastos/Elastos.ELA.Client>
2. make 编译cli节点程序

### 1.2 cli-config.json准备

1. Host:连接节点RPC,用来获取utxo和发送交易
2. DepositAddress:根据地址主链给侧链充值还是侧链给主链提币
3. 充值: DepositAddress 配置侧链创世区块Hash生成的X开头地址，用来主链给某个侧链充值交易
4. 提币: DepositAddress 配置"0000000000000000000000000000000000"给主链提币交易

```json
{
    "Host": "127.0.0.1:11336",
    "DepositAddress":"XEmfgnrDLQmFPBJiWvsyYGV2jzLQY58J6G"
}
```

### 1.3 创建钱包

1. 生成keystore.dat文件和wallet.db文件
2. keystore.dat:加密私钥后的文件
3. wallet.db:存储地址的UTXO

```shell
$ ./ela-cli wallet -c
```

```shell
ADDRESS                            PUBLIC KEY
---------------------------------- ------------------------------------------------------------------
ENMnJUCiwLnZoV51buMGuiLbwSTBRndh17 02fab9d047f59ecbc49309de83e4b3ef58bc0b53fa8b9eab5ff1ab09fbb28b4b65
---------------------------------- ------------------------------------------------------------------
```

### 1.4 显示账户余额

```shell
$ ./ela-cli wallet -l
```

```shell
INDEX                            ADDRESS BALANCE                           (LOCKED)   TYPE
----- ---------------------------------- ------------------------------------------ ------
    1 ENMnJUCiwLnZoV51buMGuiLbwSTBRndh17 2.92998500                    (2.92998500) MASTER
----- ---------------------------------- ------------------------------------------ ------
```

### 1.5 单签转账

#### 1.5.1 创建交易生成

1. from:花费地址
2. to:到账地址
3. amount:转账金额
4. fee:交易费

```shell
$ ./ela-cli wallet -t create --from ENMnJUCiwLnZoV51buMGuiLbwSTBRndh17 --to EXYPqZpQQk4muDrdXoRNJhCpoQtFBQetYg --amount 100 --fee 0.00001

生成:to_be_signed.txn 文件
```

#### 1.5.2 签名交易

```shell
$ ./ela-cli wallet -t sign --file  to_be_signed.txn

生成:ready_to_send.txn 文件
```

#### 1.5.3 发送交易

```shell
$ ./ela-cli wallet -t send --file  ready_to_send.txn

生成txid:94d4b62bc91532dc357ee411394540f301efb723d5356e4864abcc7e42a717ec
```

### 1.6 多签转账

#### 1.6.1 创建多签地址

1. 创建多签地址需要三个或三个以上地址对应的公钥，默认签名数量为：M=N/2+1,M为签名数，N为公钥数
2. 创建的多签地址是8开头，多签地址转账需要M个私钥进行签名

```shell
$ ./ela-cli wallet --addaccount 03AFFC115E36CEFBC32081D57E1373781266989A40B7B70714DC118C7A5E6FD713,02F3A141103E46164263C7F4695F273F00E253388F5C56A3D61CAC995F7D556542,024E232C5201824526246336DF9CA6BC33ADF9726464FF4BF6164EC2F90116EEF6,02EE4469ED7948630F709E90F6B2519CA57FA0C2141EE8C9BA1F811171EEE20F52
```

```shell
INDEX                            ADDRESS BALANCE                           (LOCKED)   TYPE
----- ---------------------------------- ------------------------------------------ ------
    1 ENMnJUCiwLnZoV51buMGuiLbwSTBRndh17 0                             (5.85997000) MASTER
----- ---------------------------------- ------------------------------------------ ------
    2 8YaHDqipzMEqGyqCgPSBQxVdWwLyhWJrSp 0                                      (0)  MULTI
----- ---------------------------------- ------------------------------------------ ------
```

#### 1.6.2 创建交易生成

```shell
$ ./ela-cli wallet -t create --from 8YaHDqipzMEqGyqCgPSBQxVdWwLyhWJrSp --to EXYPqZpQQk4muDrdXoRNJhCpoQtFBQetYg --amount 100 --fee 0.00001

生成:to_be_signed.txn 文件
```

#### 1.6.3 签名交易

```shell
签名M次

$ ./ela-cli wallet -t sign --file  to_be_signed.txn

生成: to_be_signed_1_of_3.txn 文件


$ ./ela-cli wallet -t sign --file  to_be_signed_1_of_3.txn

生成: to_be_signed_2_of_3.txn 文件


$ ./ela-cli wallet -t sign --file  to_be_signed_2_of_3.txn

生成: ready_to_send.txn  文件
```

### 1.6.4 发送交易

```shell
$ ./ela-cli wallet -t send --file  ready_to_send.txn

生成txid: 94d4b62bc91532dc357ee411394540f301efb723d5356e4864abcc7e42a717ec
```

## 2. 更多说明

```shell
$ ./ela-cli wallet
NAME:
   ela-cli wallet - wallet operations

USAGE:
   ela-cli wallet [command options] [args]

DESCRIPTION:
   With ela-cli wallet, you can create an account, check account balance or build, sign and send transactions.

OPTIONS:
   --password value, -p value     arguments to pass the password value
   --name value, -n value         to specify the created keystore file name or the keystore file path to open (default: "keystore.dat")
   --import value                 create your wallet using an existed private key
   --export                       export your private key from this wallet
   --create, -c                   create wallet, this will generate a keystore file within you account information
   --account, -a                  show account address, public key and program hash
   --changepassword               change the password to access this wallet, must do not forget it
   --reset                        clear the UTXOs stored in the local database
   --addaccount value             add a standard account with a public key, or add a multi-sign account with multiple public keys
                                  use -m to specify how many signatures are needed to create a valid transaction
                                  by default M is public keys / 2 + 1, witch means greater than half
   -m value                       the M value to specify how many signatures are needed to create a valid transaction (default: 0)
   --delaccount value             delete an account from database using it's address
   --list, -l                     list accounts information, including address, public key, balance and account type.
   --transaction value, -t value  use [create, sign, send], to create, sign or send a transaction
                                  create:
                                    use --to --amount --fee [--lock], or --file --fee [--lock]
                                    to create a standard transaction, or multi output transaction
                                  sign, send:
                                    use --file or --hex to specify the transaction file path or content
   --from value                   the spend address of the transaction
   --to value                     the receive address of the transaction
   --amount value                 the transfer amount of the transaction
   --fee value                    the transfer fee of the transaction
   --lock value                   the lock time to specify when the received asset can be spent
   --hex value                    the transaction content in hex string format to be sign or send
   --file value, -f value         the file path to specify a CSV file path with [address,amount] format as multi output content,
                                  or the transaction file path with the hex string content to be sign or send
```

