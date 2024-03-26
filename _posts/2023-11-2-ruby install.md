---
layout: post
title: ruby install
description: ruby install
categories:
  - ruby 
tags:
  - ruby 
---



### 安装Ruby的步骤

Ruby是一个开源的动态编程语言，它有优美的语法，可用于构建可伸缩的Web应用程序。ruby gems可以很好地增强Ruby开发者的开发效率。
要在Ubuntu系统上安装Ruby，有几种方法，每种方法都只需几步就能搞定。

#### Ubuntu

方法一：使用apt-get安装

可以直接使用两个命令完成Ruby的安装。

```bash
sudo apt-get update
sudo apt-get install ruby
```

或者

```bash
sudo apt-get install ruby2.0
```

方法二：使用brightbox ppa仓库安装

```bash
sudo apt-get install python-software-properties
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.1 ruby2.1-dev
```

方法三：使用RVM安装

RVM是Ruby的版本管理器工具。
1、安装RVM

```bash
sudo apt-get curl
\curl -sSL https://get.rvm.io | bash -s stable
source /usr/local/rvm/scripts/rvm
```

2、安装RVM的环境依赖

```bash
rvm requirements
```

3、安装Ruby

```bash
rvm install ruby
```

如果想在Ubuntu上安装多个Ruby版本，那么可以使用下面的命令来指定使用rvm作为默认的Ruby版本管理。

```bash
rvm use 3.0.0 --default
```

检查当前成功安装的Ruby版本

```bash
ruby -v
```

安装gems

```bash
gem list
gem install [gem-name]
```

比如gem-name可以写sass
如果要从本地安装gems，命令如下：

```bash
gem install --local [path of gem file]
```

可以使用命令更新已安装的gems，命令如下：

```bash
gem update --system
```

或者

```bash
gem update
```

#### CentOS

##### rbenv安装

安装gcc环境

```
yum install gcc-c++
```

下载ruby

```
wget https://cache.ruby-lang.org/pub/ruby/3.1/ruby-3.1.2.tar.gz 
```

解压

```
tar -xvf ruby-3.1.2.tar.gz 
```

编译和构建

```
mkdir -p /usr/local/ruby 
cd ruby-3.1.2
./configure --prefix=/usr/local/ruby 
make && make install 
```

##### 替换环境变量

```bash
vi /etc/profile
# 插入
export PATH=$PATH:/usr/local/ruby/bin:
# 保存退出
source /etc/profile
```

### 安装Jekyll和Bundler

```bash
yum install gem
gem sources --add https://mirrors.cloud.tencent.com/rubygems/ --remove https://rubygems.org/
gem sources --add https://rubygems.org/ --remove https://mirrors.cloud.tencent.com/rubygems/
gem install jekyll
gem install bundler
bundle install
```

### 安装 nginx 和配置 nginx

- 使用 apt 安装 nginx

  ```
  sudo apt-get install nginx -y
  ```

  移除 nginx 默认配置文件

  ```
  sudo rm -rf /etc/nginx/sites-enabled/default
  ```

- 编辑 nginx 配置文件

  修改`nginx.conf`配置文件,实现 nginx 的本地 jekyll 服务代理

  在终端输入以下命令：

```
sudo chmod a+w /etc/nginx/nginx.conf
vi /etc/nginx/nginx.conf
```

​	编辑/etc/nginx/nginx.conf

​	进入 nginx 配置文件在 http 代码块中添加以下配置

```
server{
    listen 80;
    location / {
        proxy_pass http://127.0.0.1:4000;
    }
}

# 同时映射多个网站
server {
    listen 80;
    server_name makaixin.com;

    location / {
        proxy_pass http://127.0.0.1:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 80;
    server_name ai.makaixin.com;

    location / {
        proxy_pass http://127.0.0.1:7860;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```
sudo nginx -s reload
```

### 创建 jekyll 博客

- 创建一个名为 myblog 的博客

  在 home 目录下使用`jekyll new`命令，**可能要等待一段较长的时间**

  ```
  jekyll new myblog
  ```

- 启动 jekyll 服务

```
cd myblog
jekyll server
```

​	如果要让 jekyll 服务运行于后台，使用以下命令

```
jekyll server --detach
```
