# Elastos.Developer.Document

> ## What Elastos want to say to the developer

Developers are welcome to work together to improve this document, including but not limited to the following ways to participate:

* Submit issues for documents that you don't understand.
* Improve the documentation. The default version is English version, you can submit multi-language version. If you submit a different language version, the file suffix will be expressed in a similar format as "_CN".
* Supplement the document, the current document is more "simple", if you can say more clearly, more clearly, more detailed, welcome to pull request to modify it.

After the above workload is accepted, we will send you a number of ELA as a reward. At the same time, contributors will automatically become "Elastos Developer" and will enjoy priority trials, training and other benefits.

In order to avoid omissions, contributors are requested to send an email to Dev-support@elastos.org. Thank you very much for your participation and contribution.

## 1. CAR programming

### 1.1. How to write a CAR component

* CAR Language

  * [EN](https://github.com/elastos/Elastos.RT/blob/master/Docs/CAR_Language.md)
  * [CN](https://github.com/elastos/Elastos.RT/blob/master/Docs/CAR_Language_CN.md)

### 1.2. Compile the CAR component

* Ubuntu

  * [How to run test on ubuntu](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_to_run_test_on_ubuntu.md)

* Mac OS

  * [Build IOS](https://github.com/elastos/Elastos.RT/blob/master/Docs/getting_started.md#Build_IOS)

### 1.3. Call local CAR component

* Android

  * [How to use CAR in Android](https://github.com/elastos/Elastos.RT/blob/master/Sources/Sample/HelloCarDemo/Android/HelloElastosDemo/README.md)

* iOS

  * [How to use CAR in ios](https://github.com/elastos/Elastos.RT/blob/master/Sources/Sample/HelloCarDemo/iOS/README.md)

* Ubuntu
  * How To Write A Car Component
    * [EN](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Write_A_Car_Component.md)
    * [CN](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Write_A_Car_Component_CN.md)

### 1.4. How to call a remote CAR component

* Android

  * [How To Call A Remote CAR Component](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Call_A_Remote_CAR_Component.md)

* Ubuntu

  * Being perfected

### 1.5. Implementing CAR components using Java

* Android

  * [how to implement car by java](https://github.com/elastos/Elastos.RT/blob/master/Docs/ImplementCarByJava/how_to_implement_car_by_java.md)

### 1.6. Reflection API

By default, the API document is provided, which also contains the API of the CAR SDK section.

Open the document, go to the html directory, find index.html and click it to open the API document in the browser.

[Elastos_SDK_API](SDK/Elastos_SDK_API.zip)

Refer to the last part of the documentation below to export the latest API documentation:

[How_to_Export_API](https://github.com/elastos/Elastos.RT/blob/master/Docs/DocTools/How_to_Export_API.md)

## 2. CAR SDK

The CAR SDK (CAR Software Development Kit) is a development kit for the native developers of the Elastos.RT (<https://github.com/elastos/Elastos.RT>) project. The main infrastructure services of Elastos are integrated through the Elastos.RT project and packaged into a Car interface for developers to use. Currently CAR SDK can support RPC and reflection, its basic services include: Carrier, Wallet and Did.

Traditionally, APP can expand its capabilities by including the SDK of Elastos, and obtain the typical capabilities of blockchains such as identity authentication and trusted records. For more existing apps, the lightest way is to embed the SDK, which is used to access some of the blockchain functions to achieve partial decentralization. For example, you can bind your own users through the ID provided by the SDK for identity authentication. You can also save the hash of important content in the blockchain through the API provided by the SDK, thus playing the role of notarization/certification.

You can refer to Reflection API.

[Elastos_SDK_API](SDK/Elastos_SDK_API.zip)

Refer to the last part of the documentation below to export the latest API documentation:

[How_to_Export_API](https://github.com/elastos/Elastos.RT/blob/master/Docs/DocTools/How_to_Export_API.md)

### 2.1. RPC

The main interface in RPC are as follows. For the specific interface methods, refer to the API in the CAR SDK above.

* IServiceManager
* IFriend
* ICarrier
* ICarrierListener

The examples that currently use them are the following two, which are also described earlier.

* [How To Call A Remote CAR Component](https://github.com/elastos/Elastos.RT/blob/master/Docs/How_To_Call_A_Remote_CAR_Component.md)

* [how to implement car by java](https://github.com/elastos/Elastos.RT/blob/master/Docs/ImplementCarByJava/how_to_implement_car_by_java.md)

### 2.2. Wallet

The main interface in Wallet are as follows. For the specific interface methods, refer to the API in the CAR SDK above.

* IMasterWalletManager
* IMasterWallet
* IIdChainSubWallet
* ISidechainSubWallet
* IMainchainSubWallet
* ISubWallet
* ISubWalletListener

### 2.3. DID

The main interface in DID are as follows. For the specific interface methods, refer to the API in the CAR SDK above.

* IDID
* IDIDManagerCallback
* IDIDManager
* IDIDChecker
* IDIDInspector

### 2.4. Wallet Service

* Built-in use case for offline wallet Java version

  * [Elastos.ORG.Wallet.Service Code](https://github.com/elastos/Elastos.ORG.Wallet.Service.git)

  * [Elastos.ORG.Wallet.Service documentation](https://walletservice.readthedocs.io)

## 3. ELA Blockchain

### 3.1. Node API

* REST API

  * [REST API](https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/Elastos_Wallet_Node_API.md)
  * [REST API_CN](https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/Elastos_Wallet_Node_API_CN.md)

* JSON RPC API

  * [JSON_RPC_API](https://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md)

### 3.2. Explorer API

Document not yet available.

### 3.3. Deploy ELA nodes

#### 3.3.1 Build a node and connect to the test environment

* Build a node of MainChain and connect to the test environment

  * [MainChain_deploy](https://github.com/elastos/Elastos.ELA/blob/master/README.md)

  * [Connect to testnet](https://github.com/elastos/Elastos/wiki/Connect-to-testnet)

* Build a node of IDChain and connection test environment

  * [SideChain_deploy](Doc/ELA_SideChain_deploy.md)

  * [Connect to the SideChain of testnet](Doc/Connect_to_SideChain_of_testnet.md)

#### 3.3.2 Building a test chain

Building test chains can be divided into three types:

* Build a MainChain

* Build a IDChain

* Build a SideChain

Specific steps can refer to the following document content:

* [Build_test_Chain](Doc/Build_test_Chain.md)

### 3.4. Create a transaction

#### 3.4.1. Create a transaction on the MainChain

* Create a transaction using Utilities.Java
  * [Java_offline_signature_CN](Doc/Java_offline_signature_CN.md)

* Create a transaction using ela-cli

  * [ela-cli_Client_usage_MainChain](Doc/ela-cli_Client_usage_MainChain.md)

#### 3.4.2. Create a transaction on the SideChain

* Create a transaction using ela-cli
  * [ela-cli_Client_usage_SideChain](Doc/ela-cli_Client_usage_SideChain.md)

### 3.5. Integrate exchange

* Java offline signature

  * [Link_CN](Doc/Java_offline_signature_CN.md)

  * [PDF_CN](Doc/Java_offline_signature_CN.pdf)

* Java Wallet document

  * [PDF](Doc/Java_wallet_doc.pdf)

## 4. H5 DApp Programming

### 4.1. DApp Create, package, and install the entire process

* DApp_manual

  * [EN](Doc/DApp_manual.md)

  * [CN](Doc/DApp_manual_CN.md)

### 4.2. Call DID login

* DApp_DID

  * [EN](Doc/DApp_DID.md)

  * [CN](Doc/DApp_DID_CN.md)

* DID Spec

  This is a draft，which is a document that introduces the id side-chain interface.

  * [CN](Doc/Elastos_DID_Spec(draft)_CN.docx)

* DID programming guide

  This is a draft，which is a description of how to use did, recommendations and specifications.

  * [CN](Doc/Elastos_DID_Programming_Guide(draft)_CN.docx)

* DID service

  This is a draft，which is the interface that introduces the did service we are doing. It is based on the "DID programming guide".

  * [CN](Doc/Elastos_DID_Service(draft)_CN.docx)

### 4.3. Call Wallet to pay

* DApp_Wallet

  * [EN](Doc/DApp_Wallet.md)

  * [CN](Doc/DApp_Wallet_CN.md)

## 5. Elastos Carrier

Elastos Carrier solves the problem that the app nodes on the Internet have no public network ip and cannot be directly connected. Usually solving this problem requires deploying a central server for data transfer. Also, Elastos Carrier implements a centerless direct connection communication scheme based on P2P communication technology.

Elastos Carrier provides cross-network access capabilities. For example, any two app nodes can be in different subnets, one is at home wifi environment, another is at corporate wifi environment. App can communicate directly by using an "address" string and confirming the authorization by adding "friends" to each other.

### 5.1. API

#### C++

The API is provided by default.

Open the document, go to the html directory, find index.html and click it to open the API document in the browser.

[Carrier_API](SDK/Elastos.NET.Carrier.Native.SDK_API.zip)

Refer to the last part of the documentation below to export the latest API documentation:

[How to build API documentation](https://github.com/elastos/Elastos.NET.Carrier.Native.SDK/blob/master/README.md#Build_API_documentation)

#### Android

[Code](https://github.com/elastos/Elastos.NET.Carrier.Android.SDK)

[API](https://github.com/elastos/Elastos.NET.Carrier.Android.SDK#build-docs)

#### JS

[Code](https://github.com/elastos/Elastos.NET.Carrier.Nodejs.SDK)

### 5.2. Sample

demo： <https://github.com/elastos/Elastos.Developer.Doc/tree/master/Demo/android>

doc：[Carrier_For_Android_CN](Doc/Carrier_For_Android_CN.md)

### 5.3. FAQ

* [FAQ](https://github.com/elastos/Elastos.NET.Carrier.Native.SDK/wiki/How-to-use-Carrier-API)
