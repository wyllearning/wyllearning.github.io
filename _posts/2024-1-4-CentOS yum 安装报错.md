---
layout: post
title: CentOS yum安装问题
description: yum安装问题失败
categories:
  - ruby 
tags:
  - ruby 
---



## 问题描述

使用yum安装时出现如下错误：

Errors during downloading metadata for repository 'AppStream':
  - Status code: 404 for http://mirrors.cloud.aliyuncs.com/centos/8/AppStream/x86_64/os/repodata/repomd.xml (IP: 100.100.2.148)
Error: Failed to download metadata for repo 'AppStream': Cannot download repomd.xml: Cannot download repodata/repomd.xml: All mirrors were tried

## 问题分析

阿里云解释：centos8官方源已下线，建议切换centos-vault源

## 解决方法

### 1.删除AppStream源

```
rm -f /etc/yum.repos.d/CentOS-AppStream.repo
```



### 2.取消并备份旧yum源（可直接删除）

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```



### 3.下载vault源

```
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
```



### 4.清除yum缓存

```
yum clean all
```



### 5.生成新缓存

```
 yum makecache
```



## 附加

如果出现 Couldn't resolve host 'mirrors.cloud.aliyuncs.com' ，非阿里云服务器无法访问其内网域名，需要作替换，eg：

sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
