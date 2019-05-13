---
title: python使用protobuff
date: 2018-09-30 23:30:28
tags: protocol buffer
categories: python工具包使用
---

### 1、下载protoc

protoc为编译器，根据自己的系统下载相应的protoc，[下载地址](https://github.com/protocolbuffers/protobuf/releases)

### 2、配置protoc环境变量

将protoc压缩包中/bin目录下的protoc.exe（可以拷贝至别的路径下）所在路径放入环境变量（path）中

### 3、安装 ProtoBuf 相关的 python 依赖库

<!-- more -->

3.1、方法一

> $ pip install protobuf

3.2、方法二

从源码手动编译安装，[下载地址](https://github.com/protocolbuffers/protobuf/releases)

### 4、创建项目demo，编写temp.proto文件

```protobuf
syntax = "proto2";
package example;

message person {   
    int32 id = 1;
    string name = 2;
}

message all_person {    
    repeated person Per = 1;
}
```

### 5、使用protoc编译temp.proto

> $ protoc --python_out=. temp.proto
>
> 会生成temp_pb2.py文件

### 6、引用temp_pb2

注意需要从项目根目录进行引用（否则会报错）

