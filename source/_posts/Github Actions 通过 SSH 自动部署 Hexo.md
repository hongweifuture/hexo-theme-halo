---
title: Github Actions 通过 SSH 自动部署 Hexo
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/5.jpg
toc: true
mathjax: false
summary:  通过SSH模拟物理机进行静态网站部署。Github Actions：持续集成，自动执行软件开发工作流程
categories:
  - 持续集成和交付
tags:
  - Github Actions
  - 持续集成
  - SSH
  - Hexo
  - CI/CD
urlname: github-actions-deploy-hexo-ssh
date: 2020-01-11 11:08:22
img:
password:
updated:
---

## Hexo的自动部署
目前的主流方式：
- Travis CI：travis-ci.org 专门针对开源项目，Github 上所有的公开仓库都能够免费使用；travis-ci.com 针对私有及商业项目，新用户前 100 次构建是免费的，后面就要收费了。现在github私有库已经免费了！！！
- Githooks： 这个如果 vps 本地部署，配合 nginx ，还是很推荐的
- Github Actions： 持续集成，自动执行软件开发工作流程

## 说明
前一阵玩`docker`的时候用`docker`搭建了`Hexo`环境，感觉像`Hexo`的环境搭建使用`docker`好笨重

本次使用的是Github Actions，就是因为其简单、无需VPS、公有仓库免费、私有仓库每个月2000分钟、还能体验这个新功能，我采用的是deploy的方式，所以公有仓库就看你的需求了，可以是github page、gitlab、coding、gitee、vps等等，这里以github举例。


因为是deploy方式，所以要安装插件
```bash
npm install hexo-deployer-git --save
```
同时在Hexo项目根目录配置文件`_config.yaml`中配置
```yaml
deploy:
  type: git
  repo: git@github.com:Github用户名/Github用户名.github.io.git
  branch: master
```

## 环境准备
- 私有仓库： blog
> 这里是存放 Hexo 博客源码的
- 公有仓库： 用户名.github.io
> 这里是用来 public 静态页面的，要求`空`仓库，没有初始化及任何操作的

所以如果你还没有创建 Hexo ，请参考 [官方快速开始文档](https://hexo.io/zh-cn/docs/)


## 密钥准备
为了方便运行GitHub Actions时登录GitHub账号，我们使用SSH方式登录。就是要把设备的私钥交给GitHub Actions，公钥交给GitHub，需要去Settings里去配置。

使用ssh-keygen生成一组公私秘钥对
```bash
ssh-keygen -t rsa -C "Github 的邮箱地址"

如 ssh-keygen -t rsa -C "123123123@gmail.com"
```
- 配置公钥，应该已经配好，不然如何上到的项目资源，配置路径：github网站-->Settings-->SSH and GPG keys
- 配置私钥，blog私有仓库的Settings->Secrets里添加私钥，名称为`HEXO_DEPLOY_PRIVATE_KEY`


## 配置GitHub Actions
GitHub Actions 有一些自己的术语。
- workflow （工作流程）：持续集成一次运行的过程，就是一个 workflow。
- job （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。
- step（步骤）：每个 job 由多个 step 构成，一步步完成。
- action （动作）：每个 step 可以依次执行一个或多个命令（action）。


在blog仓库的Actions选项卡下点击新建workflow，名称默认或者自定义修改，编写如下配置。

```yaml
# workflow name
name: Hexo Blog CI

# master branch on push, auto run
on: 
  push:
    branches:
      - master
      
jobs:
  build: 
    runs-on: ubuntu-latest 
        
    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master 
      
    # from: https://github.com/actions/setup-node  
    - name: Setup Node.js 10.x 
      uses: actions/setup-node@master
      with:
        node-version: "10.x"
    
    - name: Setup Hexo Dependencies
      run: |
        npm install hexo-cli -g
        npm install
    
    - name: Setup Deploy Private Key
      env:
        HEXO_DEPLOY_PRIVATE_KEY: ${{ secrets.HEXO_DEPLOY_PRIVATE_KEY }}
      run: |
        mkdir -p ~/.ssh/
        echo "$HEXO_DEPLOY_PRIVATE_KEY" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        
    - name: Setup Git Infomation
      run: | 
        git config --global user.name 'Github用户名' 
        git config --global user.email 'Github注册邮箱'
    - name: Deploy Hexo 
      run: |
        hexo clean
        hexo generate 
        hexo deploy
```
这些命令不需注释都可以看懂吧，如果想发布到VPS，只需执行run命令即可