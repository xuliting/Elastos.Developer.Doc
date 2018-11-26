# Connect to testnet

It is recommended to use `1`, which is easier and more convenient. You can also use the `2` as an alternative.

## 1 Direct access node

Currently we have an open `testnet` node that you can use for development testing:

```bash
IP address: 54.64.220.165

Example of access:

$ curl http://54.64.220.165:21334/api/v1/block/height    //Get the height of the MainChain node
$ curl http://54.64.220.165:21604/api/v1/block/height    //Get the height of the SideChain node
```

Service deployment for wallet.Service and did.Service：

```
18.179.20.67:8080  // this is did.Service
18.179.207.38:8080   // this is wallet.Service
```


## 2 Start the node and connect to the test environment

In addition, you can use two ways to start a main chain and side chain nodes. The node has been configured to connect to the `testnet` environment synchronization data by default.

This document assumes that you are at least familiar with the `AWS EC2` administrative backend or `Docker` basic operations.

### 2.1 Start an instance of our `Elastos` main chain and sidechain nodes using our custom `AWS AMI`

* Select `Launch Instance` in the `AWS EC2 Dashbord` control panel

* In the first step, select `AWS AMI`, type `ami-0d9009b28b8aa3bc8` in the search box, and then use this `AMI` to start a mirror.

* Connect to this server using the `ssh` client

* Apply the launch command (use `aws ec2` default user `ubuntu` to log in to the server):

    ```bash
    $ cd ela_v0.2.0
    $ ./ela > /dev/null &
    ```

> Start the MainChain node. After startup, the `Chain` and `Logs` directories will be generated in the current directory to store the account data and log files respectively. If there are a data synchronization error and the node height does not increase. Please delete these files and restart the application.
> After starting the MainChain node, the server will open `TCP 21333 21334 21335 21336 21338 21866` ports, please open these application ports in `AWS` firewall.

```bash
$ cd ~/did_v0.0.1
$ ./did > /dev/null &
```

> Start the SideChain node. After startup, the `Chain Logs SPVLogs data_store.bin headers.bin queue.db` file will be generated in the current directory. If there are a data synchronization error and the node height does not increase. Please delete these files and restart the application.
> After starting the SideChain node, the server will open `TCP 21603 21604 21605 21606 21608` ports, please open these application ports in `AWS` firewall.

* After the MainChain and SideChain nodes are started, they wait for the node to synchronize data from the seed node. During the period, the data synchronization can be viewed using the `/api/v1/block/height` interface.

### 2.2 Start the service with the `docker` image we created

* Prepare the `docker` runtime environment on the target server first.

* The `docker` image can be downloaded from [here] (https://s3-ap-northeast-1.amazonaws.com/elastos-docker-img/ela_node_hackson.docker.img.zip), then imported into the local image library.

* Start a `container` with the following command:

    ```bash
    docker run -d -p 21334:21334 -p 21335:21335 -p 21336:21336 -p 21338:21338 -p 21604:21604 -p 21605:21605 -p 21606:21606 -p 21608:21608 ela-node-did
    ```

> The application in `container` will automatically start the synchronization data. You can use the `/api/v1/block/height` interface to check the synchronization during synchronization.
