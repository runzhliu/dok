# DOK(Deployer of Kubernetes)

![logo](logo.png)

## 概述

DOK 是一个 Kubernetes 集群的部署工具，可以查看帮助信息了解 DOK 的其他功能。

```shell
# 查看帮助信息
dok --help
# 创建一个3 contorlplane和3 worker节点的集群
dok createCluster -m 172.22.1.16,172.22.0.41,172.22.0.117 -w 172.22.1.89,172.22.0.136,172.22.0.19 -p /root/dok-release-without-app-image.gz --password Dokpwd2022
# 给已有集群添加节点
dok joinNode -m 172.22.0.140 -w 172.22.0.43 --password Dokpwd2022
# 从集群中删除节点
dok deleteNode -w 172.22.0.43
# 重置某个节点
dok resetNode -m 10.9.204.77 -w 10.9.24.177
```

## 功能汇总

1. 创建集群过程完全不需要连接公网和代理
2. 辅助性一键创建腾讯云CVM
3. 一键创建/销毁k8s集群，添加/清理节点
4. 日志持久化本地/tmp/dok.log
5. 配置文件持久化/tmp/dok-config.yaml
6. 离线安装git/zsh/k9s/nerdctl等工具
7. DOK非常容易扩展，可以引入各种webhook(比如用于注册集群主机信息到CMDB中)

## 名词解释

1. DOK: Deployer of Kubernetes的首字母缩写
2. master0: DOK部署创建的第一个controlplane节点
3. 执行机: 执行机可以仅仅是执行DOK创建集群操作的节点而不加入Kubernetes集群
4. 安装包: 通常是指DOK需要使用的文件，一般在1G以下，命名为dok-release-without-app-image.gz
5. master节点: 正常情况下controlplane和master节点是一个意思
6. worker节点: 正常情况下worker节点跟node是一个意思
7. pkg: 一般指安装包
8. path: 一般指到文件本身，dir一般指到文件夹

## 编译

````shell
# 其他target可以查看Makefile
make
````

## 机器配置

DOK 对机器有基本的前置检查，不会提前约定机器的详细参数。

## QuickStart

从这个[链接](https://share.weiyun.com/dEhCj0UZ)下载三个文件:

1. dok
2. dok-release-without-app-image.gz
3. dok-release-without-app-image.gz.md5sum

下载完三个文件之后将文件放至到 master0 或者执行机上 `/root/` 目录下，其中除了 dok 二进制文件外，其余两个文件**必须**放在 `/root/` 目录下，**不支持修改**。

```shell
[root@VM-1-169-centos ~]# ll -h /root/
-rwxr-xr-x 1 root root 8.2M dok
-rw-r----- 1 root root 820M dok-release-without-app-image.gz
-rw-r----- 1 root root   64 dok-release-without-app-image.gz.md5sum
# 如果master0作为执行机并且使用免密模式，必须保证master0可以免密到localhost
dok createCluster \
-m 172.22.1.108 \
-w 172.22.0.159 \
-p /root/dok-release-without-app-image.gz \
--password Dokpwd2022
```

dok 可以开启命令的自动补全功能，执行下面的命令之后重启 bash 即可，更多请查看 `dok completion -h`。

```shell
# bash
dok completion bash > /etc/bash_completion.d/dok
# zsh
echo "autoload -U compinit; compinit" >> ~/.zshrc
dok completion zsh > "${fpath[1]}/_dok"
```
