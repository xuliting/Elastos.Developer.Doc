1 Select `Launch Instance` in the `AWS EC2 Dashbord` control panel

2 In the first step, select `AWS AMI`, type `ami-0d9009b28b8aa3bc8` in the search box, and then use this `AMI` to start a mirror.

3 Connect to this server using the `ssh` client

4 Application instructions:

```bash
$ cd ela_v0.2.0
$ ./ela > /dev/null &
```

> Start the MainChain node. After startup, the `Chain` and `Logs` directories will be generated in the current directory to store the account data and log files respectively. If there are a data synchronization error and the node height does not increase. Please delete these files and restart the application.
> After starting the MainChain node, the server will open `TCP 21333 21334 21335 21336 21338 21866` ports, please open these application ports in `AWS` firewall.

```bash
$ cd ~
$ cd did_v0.0.1
$ ./did > /dev/null &
```

> Start the SideChain node. After startup, the `Chain Logs SPVLogs data_store.bin headers.bin queue.db` file will be generated in the current directory. If there are a data synchronization error and the node height does not increase. Please delete these files and restart the application.
> After starting the SideChain node, the server will open `TCP 21603 21604 21605 21606 21608` ports, please open these application ports in `AWS` firewall.