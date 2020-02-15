---
title: Github Actions 通过 SSH 自动备份代码到托管网站
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/12.jpg
toc: true
mathjax: false
summary:  通过SSH自动将Github备份到Gitee和Coding。Github Actions：持续集成，自动执行软件开发工作流程
categories:
  - 持续集成和交付
tags:
  - Github Actions
  - 持续集成
  - SSH
  - Hexo
  - CI/CD
urlname: github-actions-backup-code
date: 2020-01-09 10:22:11
img:
password:
updated:
---

## 需求
每次像把源码放在不同平台都要进行如下操作：
- 准备一台物理机，创建密钥并配置对应网站的公钥
- 多平台需要多次clone、pull、push
- 换物理机还要重新配置密钥，是所有网站的密钥
- 不用物理机用docker也要安装docker环境
  
通过Github Actions持续集成可实现：
- 一次clone、pull、push，多平台同布
- 配置对应网站的公钥只需配置一次
- 换设备只需配置Github和本机的密钥

## 说明
前一阵玩`docker`的时候用`docker`搭建了`Hexo`环境，感觉像`Hexo`的环境搭建使用`docker`好笨重

本次使用的是Github Actions，就是因为其简单、无需VPS、公有仓库免费、私有仓库每个月2000分钟、还能体验这个新功能。本文主要来使得github仓库的内容备份到其他代码托管平台，可以是github page、gitlab、coding、gitee、vps等等，这里以coding、gitee举例。


## 环境准备
- Github仓库：blog
> 这里是存放源码的
- Coding仓库：blog
> 这里是用来存放备份的源码，要求`空`仓库，没有初始化及任何操作的
- Gitee仓库： blog
> 这里是用来存放备份的源码，要求`空`仓库，没有初始化及任何操作的

## 密钥准备
为了方便运行GitHub Actions时登录GitHub账号，我们使用SSH方式登录。就是要把设备的私钥交给GitHub Actions，公钥交给GitHub，需要去Settings里去配置。

使用ssh-keygen生成一组公私秘钥对
```bash
ssh-keygen -t rsa -C "Github 的邮箱地址"

如 ssh-keygen -t rsa -C "123123123@gmail.com"
```
**生成的密钥一定要保存好了，最重要的是私钥**

- 配置公钥，应该已经配好，不然如何上到的项目资源，配置路径：github网站-->Settings-->SSH and GPG keys
- 配置仓库私钥，blog私有仓库的 Settings --> Secrets 里添加私钥`Secrets`，以下面名称命名，输入对应的值，这个值就是`私钥`！
- token_Private_Keys

## 配置GitHub Actions
GitHub Actions 有一些自己的术语。
- workflow （工作流程）：持续集成一次运行的过程，就是一个 workflow。
- job （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。
- step（步骤）：每个 job 由多个 step 构成，一步步完成。
- action （动作）：每个 step 可以依次执行一个或多个命令（action）。


在blog仓库的Actions选项卡下点击新建workflow，名称默认或者自定义修改，编写如下配置。

**这里分为两次，区别只有分支操作的区别，这也是为什么前面说要创建空仓库！！**
- `git checkout master`：切换分支
- `git checkout -b master`：创建并切换分支

### 第一次运行
我的`Github`和`Coding`的`git`信息一样，如果你不一样，请分开写

```yaml
# workflow name
name: CI Backup First

# master branch on push, auto run
on: 
  push:
    branches:
      - master
      
jobs:
  Backup:
    name: Backup To Private Project 
    runs-on: ubuntu-latest 
    
    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master 
      
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '用户名' 
        git config --global user.email '邮箱'  
        
    - name: Setup SSH Private Key
      env:
        token_Private_Keys: ${{ secrets.token_Private_Keys }}
      run: |
        mkdir -p ~/.ssh/
        echo "$token_Private_Keys" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan gitee.com >> ~/.ssh/known_hosts
        ssh-keyscan e.coding.net >> ~/.ssh/known_hosts
    
    - name: Get Latest Commit Message 
      run: |
        git log --pretty=format:"%s from Github Actions at `date +"%Y-%m-%d %H:%M:%S"`" --date=short -n 1  > commit-message.log
    
    - name: Backup To Gitee Private Project
      env:
        Gitee_Blog: gitee.com:用户名/blog.git
      run: |
        git clone git@${Gitee_Blog} .gitee_blog
        cd .gitee_blog
        git checkout -b master
        cd ../
        \cp -rf ./* ./.gitee_blog/
        cd .gitee_blog
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "git@${Gitee_Blog}" master:master
        
    - name: Backup To Coding Private Project
      env:
        Coding_Blog: e.coding.net:用户名/blog.git
      run: |
        git clone git@${Coding_Blog} .coding_blog
        cd .coding_blog
        git checkout -b master
        cd ../
        \cp -rf ./* ./.coding_blog/
        cd .coding_blog
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "git@${Coding_Blog}" master:master
```
老版本的coding与新版本的区别：
- 老：`ssh-keyscan git.coding.net >> ~/.ssh/known_hosts` 
- 新：`ssh-keyscan e.coding.net >> ~/.ssh/known_hosts` 

### 第二次及以后运行
```yaml
# workflow name
name: CI Backup LTS

# master branch on push, auto run
on: 
  push:
    branches:
      - master
      
jobs:
  Backup:
    name: Backup To Private Project 
    runs-on: ubuntu-latest 
    
    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master 
      
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '用户名' 
        git config --global user.email '邮箱'  
        
    - name: Setup SSH Private Key
      env:
        token_Private_Keys: ${{ secrets.token_Private_Keys }}
      run: |
        mkdir -p ~/.ssh/
        echo "$token_Private_Keys" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan gitee.com >> ~/.ssh/known_hosts
        ssh-keyscan e.coding.net >> ~/.ssh/known_hosts
    
    - name: Get Latest Commit Message 
      run: |
        git log --pretty=format:"%s from Github Actions at `date +"%Y-%m-%d %H:%M:%S"`" --date=short -n 1  > commit-message.log
    
    - name: Backup To Gitee Private Project
      env:
        Gitee_Blog: gitee.com:用户名/blog.git
      run: |
        git clone git@${Gitee_Blog} .gitee_blog
        cd .gitee_blog
        git checkout master
        cd ../
        \cp -rf ./* ./.gitee_blog/
        cd .gitee_blog
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "git@${Gitee_Blog}" master:master
        
    - name: Backup To Coding Private Project
      env:
        Coding_Blog: e.coding.net:用户名/blog.git
      run: |
        git clone git@${Coding_Blog} .coding_blog
        cd .coding_blog
        git checkout master
        cd ../
        \cp -rf ./* ./.coding_blog/
        cd .coding_blog
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "git@${Coding_Blog}" master:master
```
