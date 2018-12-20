### java钱包文档

#### 一.获得jar包二种方式

##### 1.下载最新版本的jar包

```
网址：https://github.com/elastos/Elastos.ELA.Utilities.Java/releases

下载：Elastos.ELA.Utilities.Java_v0.1.*.jar
```
##### 2.下载源代码，编译jar包

```
网址：https://github.com/elastos/Elastos.ELA.Utilities.Java
```

- 编译jar包

```
File -> Project Structure -> Artifacts -> + -> JAR -> From modules with 
1、-> Main Class
2、-> extract to the target JAR
3、-> META-INF PATH （C:\DNA\src\ela_tool\src\main\resources）
4、ok ->  Include in project build -> Apply ->ok
```

- 删除签名jar包依赖，必须使用

```
命令： zip -d Elastos.ELA.Utilities.Java 'META-INF/*.SF' 'META-INF/*.RSA' 'META-INF/*SF'
```

#### 二.启动jar包

```
建议java版本：1.8

启动命令：java -cp Elastos.ELA.Utilities.v0.1.*.Java  org.elastos.elaweb.HttpServer

web服务默认端口：8989，可修改
```


#### 三.创建交易（自动获取utxo）

##### 自动获取utxo,也称非离线签名

##### 特点：
- 构造交易方便，不用计算utxo
- 不用计算找零地址金额，拿到从小到大排序的utxo，根据输出(outputs)金额，自动计算找零金额

##### 需要准备程序，并且后三个程序必须在同级目录下：
- ela —— ela节点
- jar包 —— java离线签名程序
- java-config.json —— 用来连接ela节点
- keystore.dat —— 账户钱包

##### 参数说明：
- java程序金额为最小单位0.00000001ela(即:amount="00.00000001"ELA)，String类型
- java-config.json 文件需要放在java程序同级目录，目的是连接节点获取utxo
- Host：节点程序所在的服务器ip和rpc端口
- Fee：双方规定的交易费，一笔交易的单个输出或多个输出交易费是一样的
- Confirmation:区块确认交易的次数，即区块数;建议16个确认数
- Account:password是该address对应账户密码
- Outputs：充值地址及金额
- ChangeAddress：转账后找零的地址，找零金额java程序自动处理
- 输入金额小于输出金额，提示金额不足

 - #### 接口名：genRawTransactionByAccount

##### java-config.json

```
{
  "Host": "127.0.0.1:11336",
  "Fee":"0.0005",
  "Confirmation":16
}

```

##### Request

```
{
    "method":"genRawTransactionByAccount",
    "id":0,
    "params":[
        {
            "Transactions":[
                {
                    "Account":[
                        {
                            "password":"12345",
                            "address":"ENKj2J5dGjSRgHHxBZ3yLjB6RyXvHikW5K"
                        }
                    ],
                    "Outputs":[
                        {
                            "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
                            "amount":"3"
                        }
                    ],
                    "ChangeAddress":"Edi5WWMFBsEL2qgggrFhnJe1HTjDnw447H"
                }
            ]
        }
    ]
}

```

##### Response

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

#### 四.账户接口

```
生成的keystore.dat文件在java离线签名程序同级目录下
```

 - #### createAccount 创建账户

##### Request
```
{
    "method":"createAccount",
    "id":0,
    "params":[
    	{
	       "Account":[
	            {
	                "password":"12345"
	            }
	        ]
    	}
    ]
}
```

##### Response
```
{
    "Action": "createAccount",
    "Desc": "SUCCESS",
    "Result": [
        {
            "address": "EbrydLF4BuJ7mPJYpxk7qzwx1CirFCDcPg",
            "encryptedPrivateKey": "8xHdZwJFeYLs0zRRf4uvxZThhGUA0jwwkae/WFxIXbf1aBy3Hm+iTbuVtkusFYiA",
            "salt": "ZUPbh6H6LcNv6PZ64e1HPw==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        }
    ]
}
```

 - #### importAccount 导入账户

##### Request
```
{
    "method":"importAccount",
    "id":0,
    "params":[
    	{
	       "Account":[
	            {
	                "password":"12345",
	                "privateKey":"5FA927E5664E563F019F50DCD4D7E2D9404F2D5D49E31F9482912E23D6D7B9EB"
	            }
	        ]
    	}
    ]
}
```

##### Response
```
{
    "Action": "importAccount",
    "Desc": "SUCCESS",
    "Result": [
        {
            "address": "EKNh1wS42ur6Pfai6DthfH61vDUmg8M93v",
            "encryptedPrivateKey": "sbMP2vWA/5rDMUT0aB22rLkS+QA7zHVXS3xEhJjhzEX0YPR4+HkPaQ8QSAg5LRJw",
            "salt": "sE7vPJSD2pnd1qDjIsHXnw==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        },
        {
            "address": "EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
            "encryptedPrivateKey": "+PVIa2LLfWWwsfRIWGE1BwGGqg1YtnG5jBHepI6x26TtEB12y2jf0m6FCp7Tc9BN",
            "salt": "Zu0sQcTq14ce7mCcP9emkA==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        }
    ]
}
```

 - #### removeAccount 移出账户

##### Request
```
{
    "method":"removeAccount",
    "id":0,
    "params":[
    	{
	       "Account":[
	            {
	                "password":"12345",
	                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB"
	            }
	        ]
    	}
    ]
}
```

##### Response
```
{
    "Action": "removeAccount",
    "Desc": "SUCCESS",
    "Result": [
        {
            "address": "EKNh1wS42ur6Pfai6DthfH61vDUmg8M93v",
            "encryptedPrivateKey": "sbMP2vWA/5rDMUT0aB22rLkS+QA7zHVXS3xEhJjhzEX0YPR4+HkPaQ8QSAg5LRJw",
            "salt": "sE7vPJSD2pnd1qDjIsHXnw==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        }
    ]
}
```

 - #### exportPrivateKey 导出私钥

##### Request
```
{
    "method":"exportPrivateKey",
    "id":0,
    "params":[
    	{
	       "Account":[
	            {
	                "password":"12345",
	                "address":"EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB"
	            },
	            {
	                "password":"12345",
	                "address":"EKNh1wS42ur6Pfai6DthfH61vDUmg8M93v"
	            }
	        ]
    	}
    ]
}
```

##### Response
```
{
    "Action": "exportPrivateKey",
    "Desc": "SUCCESS",
    "Result": [
        "5fa927e5664e563f019f50dcd4d7e2d9404f2d5d49e31f9482912e23d6d7b9eb",
        "06d243f6835ced1253c6cd939de12d4f482922b86c165d532384368ea2bbe72b"
    ]
}
```


 - #### getAccounts 获取所有账户

##### Request
```
{
    "method":"getAccounts",
    "id":0,
    "params":[
    	
    ]
}
```

##### Response
```
{
    "Action": "getAccounts",
    "Desc": "SUCCESS",
    "Result": [
        {
            "address": "EKNh1wS42ur6Pfai6DthfH61vDUmg8M93v",
            "encryptedPrivateKey": "sbMP2vWA/5rDMUT0aB22rLkS+QA7zHVXS3xEhJjhzEX0YPR4+HkPaQ8QSAg5LRJw",
            "salt": "sE7vPJSD2pnd1qDjIsHXnw==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        },
        {
            "address": "EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB",
            "encryptedPrivateKey": "+PVIa2LLfWWwsfRIWGE1BwGGqg1YtnG5jBHepI6x26TtEB12y2jf0m6FCp7Tc9BN",
            "salt": "Zu0sQcTq14ce7mCcP9emkA==",
            "scrypt": {
                "dkLen": 64,
                "n": 16384,
                "p": 8,
                "r": 8
            },
            "version": "1.0"
        }
    ]
}
```

 - #### getAccountAddresses 获取所有账户地址

##### Request
```
{
    "method":"getAccountAddresses",
    "id":0,
    "params":[
    ]
}
```

##### Response
```
{
    "Action": "getAccountAddresses",
    "Desc": "SUCCESS",
    "Result": "[EKNh1wS42ur6Pfai6DthfH61vDUmg8M93v, EQSpUzE4XYJhBSx5j7Tf2cteaKdFdixfVB]"
}
```