# 1.Wallet 介绍

* Elastos  Wallet 提供token的交换能力。

## 2.调用 Wallet 支付说明

### 2.1.下载和安装 appmanager 插件

插件目录：

https://github.com/elastos/Elastos.ORG.Wallet.Mobile/tree/ds/appmanager

安装指令:

```
ionic cordova plugin add D:\project\Elastos.ORG.Wallet.Mobile\appmanager
```

### 2.2.在需要使用 Wallet 支付的 Page 里面加入如下的代码

```
declare let cordova: any;
cordova.plugins.appmanager.StartApp("wallet/www/index.html" +
"?type=payment&message=pay message&account=10000&address=EeDUy6TmGSFfVxXVzMpVkxLhqwCqujE1WL
&memo=memo&&information=sss&backurl=game/www/index.html",
function (data) {},
function (error) {});
```

![DApp_DID_1](images/DApp_DID_1.png)

## 3.调用参数说明

```
cordova.plugins.appmanager.StartApp("wallet/www/index.html" +
"? type=payment&message=pay message&amount=10000&address=EeDUy6TmGSFfVxXVzMpVkxLhqwCqujE1WL
&memo=memo&&information=sss&backurl=game/www/index.html ",
function (data) {},
function (error) {});
```

* “wallet/www/index.html”: 目标的DApp根路径，目前钱包的路径是wallet/www/index.html
* type: 登录类型，目前Wallet支付值为payment
* amount: 支付金额，实际支付金额的除以100000000的结果。
* address: 目标的账户地址
* memo和information: 支付的备注参数， memo参数是会上链的。
* backurl: 支付后返回的DApp的根路径

## 4.返回参数说明

![DApp_Wallet_1](images/DApp_Wallet_1.png)

* txId:  交易的编号，32位的一个字符串

## 5.运行在 Trinity 内的效果

![DApp_1](images/DApp_1.png)
![DApp_2](images/DApp_2.png)
![DApp_Wallet_2](images/DApp_Wallet_2.png)