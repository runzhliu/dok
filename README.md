# DOK(Deployer of Kubernetes)

![logo](logo.png)

## Overview

DOK is a tool for creating Kubernetes clusters, please check the help information to find more usages.

```shell
# check the help information
dok --help
# create the Kubernetes cluster with 3 controlplane and 3 workers
dok createCluster -m 172.22.1.16,172.22.0.41,172.22.0.117 -w 172.22.1.89,172.22.0.136,172.22.0.19 -p /root/dok-release-without-app-image.gz --password Dokpwd2022
# join a Node to an existing cluster
dok joinNode -m 172.22.0.140 -w 172.22.0.43 --password Dokpwd2022
# delete a Node from an existing cluster
dok deleteNode -w 172.22.0.43
# reset a Node from an existing cluster
dok resetNode -m 10.9.204.77 -w 10.9.24.177
```

## Summary

1. Create the Kubernetes cluster without internet or any proxy
2. Help to create Tencent Cloud CVM easily
3. Create/Destroy Kubernetes cluster and Add/Delete Node
4. Logs persist to file /tmp/dok.log
5. Config Persist to file /tmp/dok-config.yaml
6. Install git/zsh/k9s/nerdctl offline
7. DOK is very easy to expand with shell or webhook(register to CMDB)

## Nouns

1. DOK: acronym of Deployer of Kubernetes
2. master0: the first controlplane node created by DOK deployment
3. exec machine: it can only be a node that executes DOK to create a cluster operation without joining the Kubernetes cluster
4. install pkg: refers to the file that DOK needs to use, generally less than 1G, named dok-release-without-app-image.gz
5. master node: controlplane and master have the same meaning
6. worker node: worker node and node have the same meaning
7. pkg: generally refers to the installation package
8. path: generally refers to the file itself, dir generally refers to the folder

## Compile

````shell
# check targets in Makefile
make
````

## QuickStart

Download these 3 files as follows from this [link](https://share.weiyun.com/dEhCj0UZ):

1. dok
2. dok-release-without-app-image.gz
3. dok-release-without-app-image.gz.md5sum

After download these 3 files, please put them to the path `/root` of master0 or execution machine, remember that, the path is not allowed to be changed for now.

```shell
[root@VM-1-169-centos ~]# ll -h /root/
-rwxr-xr-x 1 root root 8.2M dok
-rw-r----- 1 root root 820M dok-release-without-app-image.gz
-rw-r----- 1 root root   64 dok-release-without-app-image.gz.md5sum
# If master0 is used as the execution machine and uses password-free mode, it must be ensured that master0 can enter localhost without password
dok createCluster \
-m 172.22.1.108 \
-w 172.22.0.159 \
-p /root/dok-release-without-app-image.gz \
--password Dokpwd2022
```

DOK can enable the auto-completion function of the command. After executing the following command, restart bash. For more information, please check `dok completion -h`.

```shell
# bash
dok completion bash > /etc/bash_completion.d/dok
# zsh
echo "autoload -U compinit; compinit" >> ~/.zshrc
dok completion zsh > "${fpath[1]}/_dok"
```
