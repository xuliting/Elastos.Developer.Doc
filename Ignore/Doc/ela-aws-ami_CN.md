# 环境使用说明

建议使用 1，最简单便捷，也可从 2 中选择一种来使用。

## 1 直接访问节点

当前我们提供了一个开放的 `testnet` 节点可以供您开发测试使用：

```bash
IP地址：54.64.220.165

访问示例：

$ curl http://54.64.220.165:21334/api/v1/block/height    //获取主链节点高度
$ curl http://54.64.220.165:21604/api/v1/block/height    //获取侧链节点高度
```

wallet.Service 和 did.Service 的服务部署：

```
18.179.20.67:8080  // 这是 did.Service
18.179.207.38:8080   // 这是 wallet.Service
```

## 2 启动节点，连接测试环境

另外您还可以使用如下两种方式来启动一个主链和侧链节点，节点已经默认配置连接`testnet`环境同步数据；本文档假设您至少熟悉 `AWS EC2` 管理后台或者 `Docker` 基本操作。

### 2.1 使用我们定制的 `AWS AMI` 启动一个实例来运行 `Elastos` 主链和侧链节点：

* 在 `AWS EC2 Dashbord` 控制面板中选择 `Launch Instance`

* 在第一步选择 `AWS AMI` 时候，在搜索框输入 `ami-0d9009b28b8aa3bc8`，然后使用这个 `AMI` 启动一个镜像

* 使用 `ssh` 客户端连接到这个服务器

* 应用启动命令(使用 `aws ec2` 默认用户 `ubuntu` 登录服务器)：

    ```bash
    $ cd ~/ela_v0.2.0
    $ ./ela > /dev/null &
    ```

> 启动主链节点，启动之后会在当前目录下生成 Chain 和 Logs目录分别存放账本数据和日志文件；如果出现数据同步错误，节点高度不增加问题，请删除这些文件之后重新启动应用
> 启动主链节点之后，服务器会打开 TCP 21333 21334 21335 21336 21338 21866端口，请在 AWS 防火墙打开这些应用端口

```bash
$ cd ~/did_v0.0.1
$ ./did > /dev/null &
```

> 启动侧链节点，启动之后会在当前目录下生成 Chain Logs SPVLogs data_store.bin headers.bin queue.db文件；如果出现数据同步错误，节点高度不增加问题，请删除这些文件之后重新启动应用
> 启动侧链节点之后，服务器会打开 TCP 21603 21604 21605 21606 21608端口，请在 AWS 防火墙打开这些应用端口

* 主链和侧链节点启动后等待节点从种子节点同步数据，期间可以使用 `/api/v1/block/height`接口查看数据同步情况

### 2.2 使用我们创建的 `docker` 镜像启动服务

* 先在目标服务器准备好 `docker` 运行环境

* `docker` 镜像可以从[这里](https://s3-ap-northeast-1.amazonaws.com/elastos-docker-img/ela_node_hackson.docker.img.zip)下载，然后导入本地镜像库

* 使用如下命令启动一个 `container`：

    ```bash
    docker run -d -p 21334:21334 -p 21335:21335 -p 21336:21336 -p 21338:21338 -p 21604:21604 -p 21605:21605 -p 21606:21606 -p 21608:21608 ela-node-did
    ```

> `container` 中的应用会自动启动同步数据，同步过程中可以使用 `/api/v1/block/height` 接口查看同步情况
