---
title: Github Actions 通过 API 自动部署 Hexo
author: hongwei
top: false
cover: true
coverImg: /medias/featureimages/6.jpg
toc: true
mathjax: false
summary:  通过Github和Coding的API进行静态网站Pages部署。Github Actions：持续集成，自动执行软件开发工作流程
categories:
  - 持续集成和交付
tags:
  - Github Actions
  - 持续集成
  - API
  - Hexo
  - CI/CD
urlname: github-actions-deploy-hexo-api
date: 2020-01-11 10:18:33
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

本次使用的是`Github Actions`，就是因为其简单、无需VPS、公有仓库免费、私有仓库每个月2000分钟、还能体验这个新功能，本文采用的是`API`推送的方式，免去需要物理机申请ssh key的步骤，如果习惯了采用`SSH`方式，你可以去看看我的另外一篇文章[Github Actions 通过 SSH 自动部署 Hexo](https://www.zhwei.cn/github-actions-deploy-hexo-ssh/)。

静态网站部署其实哪家的Pages都可以，可以是github page、gitlab、coding、gitee、vps等等，这里以github和coding举例。

## 环境准备
流程：博客源码通过本地`git`备份到`blog`库，`Github Actions`监控`github`的`PUSH`会自动进行部署，发布到`Github Pages`和`Coding Pages`

- 私有仓库： blog
> 这里是存放 Hexo 博客源码的
- 公有仓库： 用户名.github.io
> 这里是用来 public 静态页面的，要求`空`仓库，没有初始化及任何操作的
- 公有仓库： 用户名.coding.me
> 这里是用来 public 静态页面的，要求`空`仓库，没有初始化及任何操作的

所以如果你还没有创建 Hexo ，请参考 [官方快速开始文档](https://hexo.io/zh-cn/docs/)


## API 密钥申请
### github申请
- 路径：Settings --> Developer settings --> Personal access tokens --> Generate new token
- 勾选`repo`所有的权限，即repo:status、repo_deployment、public_repo和 repo:invite
- 可参考[GitHub API](https://developer.github.com/) 和 [OAuth scopes权限说明](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/)

> 这里的`token`只显示一次，请先保存下来在关闭网页，否则只能重新生成

### coding申请
最近coding和腾讯云开发者合并为全新coding，有好多的变化，我是2020年01月19日升级的，这里我以全新coding来说明，对于推送和拉取只有格式的区别.截至目前我只有coding的升级了腾讯云开发者的还没有升级。

- 路径：个人设置 --> 访问令牌 --> 新建令牌
- 勾选`project:depot`权限，即完整的仓库控制权限，可读可写
- 可参考[Coding API](https://help.coding.net/docs/member/tokens.html) 和 [OAuth scopes权限说明](https://open.coding.net/oauth/#%E5%8F%82%E6%95%B0)

> 这里的`token`只显示一次，请先保存下来在关闭网页，否则只能重新生成，这里还有一个`令牌用户名`需要记下来，最好用这个替换`coding用户名`操作

## API 密钥添加
- 配置仓库私钥，blog私有仓库的 Settings --> Secrets 里添加私钥`Secrets`，以下面名称命名，输入对应的值
- token_CodingAPI
- token_GithubAPI
- Username_Coding

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
name: CI Hexo Blog Deploy First

# master branch on push, auto run
on: 
  push:
    branches:
      - master

jobs:
  Deploy-Pages: 
    name: Deploy Hexo Public To Pages
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
    
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '用户名' 
        git config --global user.email '邮箱'  
 
    - name: Get Latest Commit Message 
      run: |
        git log --pretty=format:"%s from Github Actions at `date +"%Y-%m-%d %H:%M:%S"`" --date=short -n 1  > commit-message.log
        
    - name: Setup Hexo Dependencies
      run: |
        npm install hexo-cli -g
        npm install
        
    - name: Generate public files
      run: |
        hexo clean
        hexo generate 
        
    - name: Deploy To Github Pages 
      env:
        Github_Pages: github.com/用户名/用户名.github.io
        Github_Token: ${{ secrets.token_GithubAPI }}
      run: |  
        git clone https://${Github_Pages} .github_pages
        cd .github_pages
        git checkout -b master
        cd ../
        mv .github_pages/.git/ ./public/
        cd ./public/
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "https://${Github_Token}@${Github_Pages}" master:master

    - name: Deploy To Coding Pages 
      env:
        Coding_Pages: e.coding.net/用户名/用户名.coding.me.git
        Coding_Token: ${{ secrets.token_CodingAPI }}
        Coding_Username: ${{ secrets.Username_Coding }}
      run: |
        git clone https://${Coding_Username}:${Coding_Token}@${Coding_Pages} .coding_pages
        cd .coding_pages
        git checkout -b master
        cd ../
        rm -r ./public/.git/
        mv .coding_pages/.git/ ./public/
        cd ./public/
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "https://${Coding_Username}:${Coding_Token}@${Coding_Pages}" master:master
```
老版本的coding与新版本的区别：
- 老：`https://@${Coding_Pages}` 
- 新：`https://${Coding_Username}:${Coding_Token}@${Coding_Pages}` 

### 第二次及以后运行
```yaml
# workflow name
name: CI Hexo Blog Deploy LTS

# master branch on push, auto run
on: 
  push:
    branches:
      - master

jobs:
  Deploy-Pages: 
    name: Deploy Hexo Public To Pages
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
    
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '用户名' 
        git config --global user.email '邮箱'  
 
    - name: Get Latest Commit Message 
      run: |
        git log --pretty=format:"%s from Github Actions at `date +"%Y-%m-%d %H:%M:%S"`" --date=short -n 1  > commit-message.log
        
    - name: Setup Hexo Dependencies
      run: |
        npm install hexo-cli -g
        npm install
        
    - name: Generate public files
      run: |
        hexo clean
        hexo generate 
        
    - name: Deploy To Github Pages 
      env:
        Github_Pages: github.com/用户名/用户名.github.io
        Github_Token: ${{ secrets.token_GithubAPI }}
      run: |  
        git clone https://${Github_Pages} .github_pages
        cd .github_pages
        git checkout master
        cd ../
        mv .github_pages/.git/ ./public/
        cd ./public/
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "https://${Github_Token}@${Github_Pages}" master:master

    - name: Deploy To Coding Pages 
      env:
        Coding_Pages: e.coding.net/用户名/用户名.coding.me.git
        Coding_Token: ${{ secrets.token_CodingAPI }}
        Coding_Username: ${{ secrets.Username_Coding }}
      run: |
        git clone https://${Coding_Username}:${Coding_Token}@${Coding_Pages} .coding_pages
        cd .coding_pages
        git checkout master
        cd ../
        rm -r ./public/.git/
        mv .coding_pages/.git/ ./public/
        cd ./public/
        git add .
        git commit -F ../commit-message.log
        git push --force --quiet "https://${Coding_Username}:${Coding_Token}@${Coding_Pages}" master:master
```
