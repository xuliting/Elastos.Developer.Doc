# Elastos.Developer.Document

> ## What Elastos want to say to the developer

Developers are welcome to work together to improve this document, including but not limited to the following ways to participate:

1. Submit issues for documents that you don't understand.
2. Improve the documentation. The default version is English version, you can submit multi-language version. If you submit a different language version, the file suffix will be expressed in a similar format as "_CN".
3. Supplement the document, the current document is more "simple", if you can say more clearly, more clearly, more detailed, welcome to pull request to modify it.

After the above workload is accepted, we will send you a number of ELA as a reward. At the same time, contributors will automatically become "Elastos Developer" and will enjoy priority trials, training and other benefits.

In order to avoid omissions, contributors are requested to send an email to Dev-support@elastos.org. Thank you very much for your participation and contribution.

## 1. CAR programming

### 1.1. How to write a CAR component

[CAR Language](https://github.com/elastos/Elastos.RT/blob/master/Docs/CAR_Language.md)

### 1.2. Compile the CAR component

* Ubuntu

  - [How to run test on ubuntu](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_to_run_test_on_ubuntu.md)

* Mac OS

  - [Build IOS](https://github.com/elastos/Elastos.RT/blob/master/Docs/getting_started.md#Build_IOS)

### 1.3. Call local CAR component

* Android

  - [How to use CAR in Android](https://github.com/elastos/Elastos.RT/blob/master/Sources/Sample/HelloCarDemo/Android/HelloElastosDemo/README.md)

* iOS

  - [How to use CAR in ios](https://github.com/elastos/Elastos.RT/blob/master/Sources/Sample/HelloCarDemo/iOS/README.md)

* Ubuntu

  - [How To Write A Car Component](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Write_A_Car_Component.md)

### 1.4. How to call a remote CAR component

* Android

  - [How To Call A Remote CAR Component](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Call_A_Remote_CAR_Component.md)

* Ubuntu

  - Being perfected

### 1.5. Implementing CAR components using Java

* Android

  - [how to implement car by java](https://github.com/elastos/Elastos.RT/blob/master/Docs/ImplementCarByJava/how_to_implement_car_by_java.md)

### 1.6. Reflection API

By default, the API document is provided, which also contains the API of the CAR SDK section.

Open the document, go to the html directory, find index.html and click it to open the API document in the browser.

[Elastos_SDK_API](SDK/Elastos_SDK_API.zip)

Refer to the last part of the documentation below to export the latest API documentation:

[How_to_Export_API](https://github.com/elastos/Elastos.RT/blob/master/Docs/DocTools/How_to_Export_API.md)

## 2. CAR SDK

You can refer to Reflection API.

[Elastos_SDK_API](SDK/Elastos_SDK_API.zip)

### 2.1. RPC

* IServiceManager
* IFriend
* ICarrier
* ICarrierListener

### 2.2. Wallet

* IMasterWalletManager
* IMasterWallet
* IIdChainSubWallet
* ISidechainSubWallet
* IMainchainSubWallet
* ISubWallet
* ISubWalletListener

### 2.3. DID

* IDID
* IDIDManagerCallback
* IDIDManager
* IDIDChecker
* IDIDInspector

## 3. Blockchain API

### 3.1. Node API

* REST API

  - No need in the future, no offer.

* JSON RPC API

  - [JSON_RPC_API](https://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md)

### 3.2. Explorer API

Document not yet available.

## 4. H5 DApp Programming

### 4.1. DApp Create, package, and install the entire process

* [DApp_manual_CN](DApp_manual_CN.md)

### 4.2. Call DID login

* [DApp_DID_CN](DApp_DID_CN.md)

### 4.3. Call Wallet to pay

* [DApp_Wallet_CN](DApp_Wallet_CN.md)

## 5. Elastos Carrier

The API is provided by default.

Open the document, go to the html directory, find index.html and click it to open the API document in the browser.

[Carrier_API](SDK/Elastos.NET.Carrier.Native.SDK_API.zip)

Refer to the last part of the documentation below to export the latest API documentation:

[How to build API documentation](https://github.com/elastos/Elastos.NET.Carrier.Native.SDK/blob/master/README.md#Build_API_documentation)
