# 1. Introduction to Wallet

* Elastos Wallet provides token swap services.

## 2. How to use Wallet for payment

### 2.1.Download and Install appmanager Plugin

Plugin Directory：

https://github.com/elastos/Elastos.ORG.Wallet.Mobile/tree/ds/appmanager

Installation Instruction:

```
ionic cordova plugin add D:\project\Elastos.ORG.Wallet.Mobile\appmanager
```

### 2.2. Add the following code snippet to the page that make payment

```
declare let cordova: any;
cordova.plugins.appmanager.StartApp("wallet/www/index.html" +
"?type=payment&message=pay message&account=10000&address=EeDUy6TmGSFfVxXVzMpVkxLhqwCqujE1WL
&memo=memo&&information=sss&backurl=game/www/index.html",
function (data) {},
function (error) {});
```

![DApp_DID_1](images/DApp_DID_1.png)

## 3. Explanation of Parameters

```
cordova.plugins.appmanager.StartApp("wallet/www/index.html" +
"? type=payment&message=pay message&amount=10000&address=EeDUy6TmGSFfVxXVzMpVkxLhqwCqujE1WL
&memo=memo&&information=sss&backurl=game/www/index.html ",
function (data) {},
function (error) {});
```

* “wallet/www/index.html”: Root path of target DApp, the cuurent path of wallet is wallet/www/index.html
* type: logon type, payment is the pay value of current wallet
* amount: the amount to pay, the value is the actual paymnet divided by 00000000
* address: receivers address
* memo and information: transaction remarks， memo will be written to Blockchain
* backurl: the root path of the DApp after transaction

## 4.Explnation of Returned Parameters 

![DApp_Wallet_1](images/DApp_Wallet_1.png)

* txId:  transaction ID,  32 bytes string

## 5.The Effects of Ruuning in Trinity

![DApp_1](images/DApp_1.png)
![DApp_2](images/DApp_2.png)
![DApp_Wallet_2](images/DApp_Wallet_2.png)
