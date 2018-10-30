# Documentation for the hackathon

## 1 Elastos BlockChain

### 1.1 code

Main Chain : <https://github.com/elastos/Elastos.ELA>

Side Chain : <https://github.com/elastos/Elastos.ELA.SideChain>

### 1.2 API

* REST API

  <https://github.com/elastos/Elastos.ELA/blob/release_v0.2.1/docs/Restful_API.md>

* JSON RPC API

  <https://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md>

### 1.3 Build a development environment

The hackathon marathon is recommended to use 1.3.1.

1.3.2 and 1.3.3 are advanced levels.

#### 1.3.1 Direct, AWS, docker

Elastos has prepared the relevant environment for the hacker to use, please hacker to view the following documents：
<https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/ela-aws-ami.md>

#### 1.3.2 Build a node, link the test environment

* Main Chain
  * Build the Main Chain:
    <https://github.com/elastos/Elastos.ELA/blob/master/README.md>
  * Connect to testnet:
    <https://github.com/elastos/Elastos/wiki/Connect-to-testnet>

* ID Chain
  * Build the ID Chain:
    <https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/ELA_SideChain_deploy.md>
  * Connect to the DID of testnet:
    <https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/Connect_to_SideChain_of_testnet.md>

#### 1.3.3 Developers build their own test chains

Building test chains can be divided into three types:

* Build a MainChain
* Build a IDChain
* Build a SideChain

<https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/Build_test_Chain.md>

### 1.4 Create a transaction

#### 1.4.1 Create a transaction on the Main Chain

Trading on the Main Chain can be divided into two ways:

* Create a transaction using Utilities.Java

  <https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/Java_offline_signature.md>

* Create a transaction using ela-cli

  <https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/ela-cli_Client_usage_MainChain.md>

#### 1.4.2 Create a transaction on the Side Chain

<https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/ela-cli_Client_usage_SideChain.md>

### 1.5 How to get the test coin

In order to facilitate the development and testing of the Elastos test chain by community developers, the Elastos Foundation also provides a faucet tool for issuing test coins. After filling in some information, you can get 10 ELA test coins on the test network.

Test network wallet address:
<https://wallet-beta.elastos.org/>

Test network browser address:
<https://blockchain-beta.elastos.org/>

Test coin tap address:
<https://faucet.elastos.org/>

Application method:
Go to <https://faucet.elastos.org/> and submit the application form.
Note: The ELA address should be the address of the registered wallet at <https://wallet-beta.elastos.org/>.

## 2 Wallet service related information

### 2.1 DID

#### 2.1.1 Code

<https://github.com/elastos/Elastos.ORG.DID.Service.git>

#### 2.1.2 API & doc

<https://didservice.readthedocs.io/en/latest/>

### 2.2 ela related

#### 2.2.1 Code

<https://github.com/elastos/Elastos.ORG.Wallet.Service>

#### 2.2.2 API & doc

<https://walletservice.readthedocs.io/en/latest/>

### 2.3 Signature transaction related

#### 2.3.1 Code

<https://github.com/elastos/Elastos.ORG.Wallet.Lib.C.git>

#### 2.3.2 API & doc

<https://elastoswalletlibc.readthedocs.io/en/latest/>

## 3 Carrier

### 3.1 Carrier Overview

Elastos Carrier solves the problem that the app nodes on the Internet have no public network ip and cannot be directly connected. Usually solving this problem requires deploying a central server for data transfer. Also, Elastos Carrier implements a centerless direct connection communication scheme based on P2P communication technology.
Elastos Carrier provides cross-network access capabilities. For example, any two app nodes can be in different subnets, one is at home wifi environment, another is at corporate wifi environment. App can communicate directly by using an "address" string and confirming the authorization by adding "friends" to each other.

#### 3.1.1 code

<https://github.com/elastos/Elastos.NET.Carrier.Native.SDK.git>

#### 3.1.2 API

Refer to the last part of the documentation below to export the latest API documentation:
<https://github.com/elastos/Elastos.NET.Carrier.Native.SDK/blob/master/README.md>

### 3.2 Create the first carrier application

#### 3.2.1 Demo

<https://github.com/elastos/Elastos.Developer.Doc/tree/master/Demo/android>

#### 3.2.2 Doc

<https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/Carrier_For_Android.md>

### 3.3 FAQ

<https://github.com/elastos/Elastos.Developer.Doc/blob/master/Doc/How_to_use_Carrier_API_FAQ.md>

## 4 Relevant information

* Social media
  * twitter/facebook/instagram/youtube/…

* ETH related information
  * <https://etherscan.io/>
  * <https://www.myetherwallet.com/>
  * <https://www.ethereum.org/>
  * <https://truffleframework.com/ganache>
  * <https://remix.ethereum.org>
  * <https://www.parity.io/>
  * <https://infura.io/>

* Other related information
  * <https://coinmarketcap.com/coins/>
