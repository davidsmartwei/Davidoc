---
title: Hexo安装及部署到github
toc: true
date: 2017-05-30 14:04:23
categories:
tags: hexo install
---
# 架构图:
<img src=" /images/hexo架构图.png" align=center/>

# 解读:
## github：
它就是一个服务器，专门存放生成的静态文件
<!--more-->
## hexo：
类似于wordpress,是博客内容的载体或叫平台，组成文件有以下
- **deploy**：执行hexo deploy命令部署到GitHub上的内容目录
- **public**：执行hexo generate命令，输出的静态网页内容目录
- **scaffolds**：layout模板文件目录，其中的md文件可以添加编辑
- **scripts**：扩展脚本目录，这里可以自定义一些javascript脚本
- **source**：文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，
404文件、favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
- **_drafts**：草稿文章
- **_posts**：发布文章
- **themes**：主题文件目录
- **_config.yml**：全局配置文件，大多数的设置都在这里
- **package.json**：应用程序数据，指明hexo的版本等信息，类似于一般软件中的关于按钮

### 生成命令：
把用户资源文件转化为静态HTML文件
```
hexo generate
```

### 部署命令：
把静态HTML文件发送到GitHub服务器的仓库中（repository）
```
hexo deploy
```

## 外网如何访问GitHub中的静态HTML文件：
### github仓库命名规则:
>github账号名.github.io

例如我的仓库为davidsmartwei.github.io，为什么这么命名呢，因为你可以通过在浏览器里输入 https://davidsmartwei.github.io/ 找到存放在github服务器上的静态HTML文件，通过里面的文件进而生成博客界面。

---

# hexo安装：(在window系统)
## 准备环境：
### 安装Git
### 安装Node.js
### 安装hexo：
任意位置点击鼠标右键，选择Git bash,输入npm命令
```
npm install -g hexo
```

## 生成项目：
1. 新建一个喜爱的文件夹（如D:\hexo），在D:\hexo内点击鼠标右键，选择Git bash（作用是切换git bash工作目录到D:\hexo），输入以下命令自动在该文件夹建立网站所需要的所有文件
```
hexo init
```
2. 安装依赖包
```
npm install
```

## 本地查看（通过搭建本地服务器server查看博客）
1. 安装server
```
npm install hexo -server -save
```
2. 生成静态HMTL文件及发送到本地服务器
```
hexo generate
hexo server
```
3. 在浏览器中输入localhost:4000即可预览到博客，但是目前外网是访问不了博客，因为该博客只存放在本地，要想通过外网访问，需要把静态HMTL文件发送到github服务器上，如上解读所示。

# 部署到Github:
## 创建github并建立仓库
仓库命名规则：
> 账号.github.io

**eg:**davidsmartwei.github.io

## 生成SSH密匙
本地与github建立数据传输联系需要ssh密匙
```
ssh-keygen -t rsa -C “你的邮箱地址”，按3个回车，密码为空
```
*邮箱地址*为你在注册github时绑定的邮箱

## 在GitHub上添加SSH密钥

1.在本地找到有步骤5生成的两个文件id_rsa（私匙）和id_rsa.pub（公匙）
2.打开id_rsa.pub并复制里面的全部内容到GitHub上。settings—>SSH and GPG keys—>New SSH key

## 重点配置_config.yml使得连接到GitHub
打开_config.yml，翻到最下面，改成我这样子的，
```
deploy:
type: git
repository: git@github.com:davidsmartwei/davidsmartwei.github.io.git
branch: master
```
**注意**：" : "后面要有空格，其中把davidsmartwei改为你的GitHub账号名

## 安装deploy
```
npm install hexo-deployer-git -save
```

# 完整测试
```
hexo clean
hexo generate
hexo deploy
```
如出现以下提示，则说明部署成功
```
[info] Deploy done: git
```
现在就可以通过外网在浏览器里输入 https://davidsmartwei.github.io 访问博客
