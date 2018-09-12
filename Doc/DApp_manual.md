#   Table of Contents

1. Introduction to Trinity
2. Definition of DApp
3. Setup Elastos Dapp Development Environment
4. How to create Dapp
5. Run the Dapp
6. How to develop Ionic App
7. Plugin for Trinity
<br>

##  1. Introduction to Trinity

* Trinity is a container that provides Elastos DApp running environment. Based on the trusted running environment of Elastos Runtime, Trinity provides a reliable running environment for applications to protect digital contents.
* Trinity makes developers develop their App easliy by using Ionic frameworks.  Developers can create Elastos DApp package directly in the Ionic export directory. The created DApp can run on Android, IOS, Windows, Linux devices.
* Ionic is a powerful HTML5 application development framework (HTML5 Hybrid Mobile App Framework). The developers can use HTML, CSS and Javascript web develoment skills to create an App with near native  experiences. Ionic framework focus on outlook, users' experiences and interaction. It is most suitable for devlopment of HTML5 hybrid mobile App.
* Ionic is a lightweight UI library. It is responsive with modern and beautiful interfaces.

### 1.1. Features of Trinity

![DApp_manual_1](../images/DApp_manual_1.png)

### 1.2. Advantages of Trinity

* The Dapp run on users' devices which permanently owned by them
* DApp does not rely on the centralized server
* Digital assets ownerships are recorded in the Blockchain using decentralized transactions
* The DApp can be freely packaged and innovated by users
* Protect the digital assets. The assets owensership will not be disclosed and destroyed

### 1.3. Trinity Architecture

![DApp_manual_2](../images/DApp_manual_2.png)
<br>

##  2. Definition of DApp

There are two components in DApp: The DApp owenership and DApp document.

The ownership of DApp is recorded in Blockchain like bitcoin in the personal wallet. The DApp cannot run if there is no ownership in the wallet. But of course the DApp can configure as free app for everyone.

You can resell the DApp similar to  bitcoin. Of course you must pay for this transaction similar to transfer of token. This transfer can be either centralized or decentralized.

There are various ways to distribute the DApp such as Yahoo Yellow page, different App Store, Cloud drives to peer to peer.
<br>

##  3. Setup Elastos DApp Development Environment
![DApp_manual_3](../images/DApp_manual_3.png)
<br>

##  4. How to create Dapp
![DApp_manual_4](../images/DApp_manual_4.png)
<br>

##  5. Run the DApp

![DApp_manual_5](../images/DApp_manual_5.png)

### 5.1. Run inside Trinity

1. Install Trinity
2. Use Ionic to compile a encrypted epk document
    https://github.com/elastos/Elastos.ORG.Wallet.Mobile/tree/ds/build

    ```
    ionic cordova build android --prod && node epk xxx
    ```

3. Copy or download the epk to the device
4. Import the epk document through Trinity, then run on the desktop

![DApp_manual_6](../images/DApp_manual_6.png) ![DApp_manual_7](../images/DApp_manual_7.png)
<br>

##  6. Develop Ionic App

### 6.1. UI Components

* Ionic apps are made of high-level building blocks called components. Components allow you to quickly construct an interface for your app. Ionic comes with a number of components, including modals, popups, and cards. Check out the examples below to see what each component looks like and to learn how to use each one. Once you’re familiar with the basics, head over to the API docs for ideas on how to customize each component.

### 6.2. Native APIs

![DApp_manual_8](../images/DApp_manual_8.png)
<br>

##  7. Plugin for Trinity

![DApp_manual_9](../images/DApp_manual_9.png)
