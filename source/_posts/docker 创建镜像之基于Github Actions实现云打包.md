---
title: docker 创建镜像之基于 Github Actions 实现云打包
author: hongwei
top: false
cover: true
coverImg: /medias/featureimages/7.jpg
toc: true
mathjax: false
summary:  基于 Github Actions 实现 docker 镜像云打包，方便快捷。
categories:
  - docker容器化
tags:
  - Github Actions
  - 持续集成
  - docker
  - 云打包创建镜像
  - CI/CD
urlname: docker-build-image-github-actions
date: 2020-01-22 13:08:22
img:
password:
updated:
---

> GitHub Actions是GitHub开源的一个平台。这一平台可以让开发者实现定制化的程序逻辑，而不需要专门创建一个应用去完成需要的任务。开发者可以借助 Actions 平台建立工作流，使用他们代码仓库中定义好的 action、或者 GitHub 公开代码库中的 action，甚至是一个公开的 Docker 容器镜像。action 在这里指的是开发、测试、部署和发布代码中的各种流程。在开发程序代码之外，有时候需要面对各种编译、测试和部署过程中的繁琐流程。这些流程往往需要手动完成，且由于不同开发者的开发环境、版本和平台不同，需要专门针对特定的环境定制工作流，十分麻烦。并且Github Actions平台是定制化的，可以使用 GitHub 的 API 和任何开源的第三方 API，以便于和代码库进行交互。

对于`Github Actions`的使用在我博客中已经有很多的实例了，大家可以去看看。

对于`docker`的镜像打包也容易也复杂，容易的当然就是容器化本身就是简化操作，但是复杂一般来说就是生产环境和网络环境的配置，对于什么system其实大同小异，无论是centos、debian、macOS还有windows等等，环境搭建容易，但是一些可选包的安装需要一定的时间，网络环境就是速度慢，虽然可以通过“这样这样这样”访问墙外的地址，但是下载编译的时间也很慢，使用`Github Actions`可以说是一种推荐的选择了，因为其不需要物理机、不需要配置网络，毕竟其网络就是外外外网，所以下载速度还可以，原来还需要去更换源、使用加速，这回都不用了，而且我们从网页就可以进行操作。

其实2020年新升级的`全新Coding`也加入了持续集成功能，使用的是`Jenkinsfile`，我使用过jenkins但是对coding集成的还不熟悉，后面有时间在实践一下。

# 需求
- 根据Dockerfile打包成镜像
- 镜像推送到阿里云容器仓库（docker hub 大同小异）

# 流程
1. 注册阿里云容器镜像服务
2. 在阿里云创建命名空间、镜像仓库，注意地区的选择！！！
3. `github`上创建仓库，私有公有看自己需求
4. 本地`git`上传打包所需要的文件，如Dockerfile、requirements.txt、entrypoint.sh等等
5. 在创建的创库中配置所需要的变量，要用到的私密信息，如账号密码之类
6. 在创建的仓库中开启`Actions`
7. 配置`YAML格式`的Actions配置文件
8. 打包开始并自动`PUSH`到阿里云仓库

# 准备
- 阿里云上自己操作
- 本地与`Github`的`SSH KEY密钥`配置
- 简单实例只用到阿里云的账号密码，故Github中`项目 --> Setting --> Secrets`定为`Aliyun_Username`和`Aliyun_Password`，填入自己信息
- 在创建的仓库中开启Actions会自动创建默认配置文件，即项目根目录下`./.github/workflows/main.yml`文件，在线修改即可，文件名可自定义
  
# 配置参考
这里以一个例子说明，实现打包一个`python环境`的最小系统，里面包括我`django`博客中使用的python基础标准环境，安装`alpine`依赖：system update、system lib、mysqlclient、Pillow、bash

实现我的`django`镜像包，只需基于当前实例的最简python，安装所需要的python库，在配合`docker-compose`即可完成全部

## Dockerfile
```bash
# 从仓库拉取 带有 python 3.7 的 Alpine Linux 环境
FROM python:3.7-alpine

# 维护者信息
MAINTAINER  hongwei "https://www.zhwei.cn/"

# 添加alpine apk源，我在本地centos中添加索性就加上了，其实不加也行 
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/community/" >> /etc/apk/repositories
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community/" >> /etc/apk/repositories;
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories;
RUN echo "http://dl-3.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories;
RUN echo "http://dl-4.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories;
RUN echo "http://dl-5.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories

# 安装alpine依赖，system update\ system lib\ mysqlclient\ Pillow\ bash
RUN apk update \
    && apk add --no-cache linux-headers libffi-dev \
    && apk add --no-cache gcc python3-dev musl-dev mariadb-dev\
    && apk add --no-cache jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev \
    && apk add --no-cache bash bash-doc bash-completion
```
## main.yml
```YAML
name: CI Build python alpine 3.7

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      matrix:
        image-name: [python]
        image-version: [3.7-alpine-django]

    steps:
    - uses: actions/checkout@v1
    - name: docker build and push aliyun
      env:
        aliyun_username: ${{ secrets.Aliyun_Username }}
        aliyun_password: ${{ secrets.Aliyun_Password }}
      run: |
        sudo docker login --username=${aliyun_username} registry.cn-beijing.aliyuncs.com --password=${{ secrets.aliyun_password }}
        sudo docker build -t ${{ matrix.image-name }}:${{ matrix.image-version }} .
        sudo docker tag ${{ matrix.image-name }}:${{ matrix.image-version }} registry.cn-beijing.aliyuncs.com/命名空间/镜像仓库:${{ matrix.image-version }}
        sudo docker push registry.cn-beijing.aliyuncs.com/命名空间/镜像仓库:${{ matrix.image-version }}

```