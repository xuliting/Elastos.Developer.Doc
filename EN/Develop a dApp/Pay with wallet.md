# 1. Introduction to Elastos Wallet

* Elastos Wallet provides token transfer services.

## 2. How to configure Wallet for payment

### 2.1. Download and Install appmanager plugin

Plugin Directory：

<https://github.com/elastos/Elastos.ORG.Wallet.Mobile/tree/ds/appmanager>

Installation instructions:

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

![DApp_DID_1](../images/DApp_DID_1.png)

## 3. Explanation of Configuring the parameters

```
cordova.plugins.appmanager.StartApp("wallet/www/index.html" +
"? type=payment&message=pay message&amount=10000&address=EeDUy6TmGSFfVxXVzMpVkxLhqwCqujE1WL
&memo=memo&&information=sss&backurl=game/www/index.html ",
function (data) {},
function (error) {});
```

* “wallet/www/index.html”: Root path of target DApp, the current path of wallet is wallet/www/index.html
* type: logon type, payment is the type for using current wallet
* amount: the amount to pay, this value is the actual paymnet divided by 100000000
* address: receiver address
* memo and information: payment transaction remarks, memo will be written to Blockchain
* backurl: the returned root path of the DApp after payment transaction

## 4. Explanation of Returned Parameters

![DApp_Wallet_1](../images/DApp_Wallet_1.png)

* txId:  transaction ID,  32 bytes string

## 5. The Effects of Ruuning in Trinity

![DApp_1](../images/DApp_1.png)
![DApp_2](../images/DApp_2.png)
![DApp_Wallet_2](../images/DApp_Wallet_2.png)
