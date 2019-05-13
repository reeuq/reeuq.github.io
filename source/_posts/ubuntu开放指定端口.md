---
title: ubuntu开放指定端口
date: 2019-05-13 15:02:29
tags: [ubuntu,iptables]
categories: linux使用
---

参考https://www.jianshu.com/p/2ec5d16db02b 、 https://blog.csdn.net/hehaibo2008/article/details/70768955

##  开放端口

### 1、安装iptables

```shell
sudo apt-get install iptables
```

### 2、添加规则

```shell
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

<!-- more -->

### 3、保存规则

```shell
sudo iptables-save
```

## 持久化规则

### 1、安装iptables-persistent

```shell
sudo apt-get install iptables-persistent
```

### 2、使用iptables-persistent

#### 2.1、如果系统为Ubuntu14.04

```shell
sudo /etc/init.d/iptables-persistent save
sudo /etc/init.d/iptables-persistent reload
```

#### 2.1、如果系统为Ubuntu16.04

```shell
sudo netfilter-persistent save
sudo netfilter-persistent reloasd
```

### 3、规则保存位置

```shell
/etc/iptables/rules.v4
/etc/iptables/rules.v6
```

