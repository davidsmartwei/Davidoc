---
title: MyWiki构建过程
toc: true
date: 2018-02-01 16:05:02
tags:
	- wikitten
	- github
categories:
---
# 初衷：
**知识积累**：维基百科，google书签，印象笔记，知乎，简书，灵感等
**知识整理**：建立个人wiki知识管理系统，分类和整理零碎知识
**个人见解**：博客，表达自己的感悟及理解
# Wiki安装：
## 主题安装：wikitten
<!--more-->
### 选择wiki知识管理系统
**wikitten优点**：
- 双栏
- 界面简洁
- 有纵深，侧边可展开显条目标题
- 支持Markdown书写
- 全文搜素
- 可添加文章

### 安装hexo主题hexo-theme-Wikitten
1.进入hexo站点文件夹，克隆主题到themes/路径下
``` bash
cd your-hexo-directory
git clone https://github.com/zthxxx/hexo-theme-Wikitten.git themes/Wikitten
```
2.覆盖站点目录中一些默认页面模板
```
cp -rf themes/Wikitten/_source/* source/
cp -rf themes/Wikitten/_scaffolds/* scaffolds/
```
3.重命名主题中的 _config.yml.example 到 _config.yml，然后可以使用配置文件配置主题
```
cp -f themes/Wikitten/_config.yml.example themes/Wikitten/_config.yml
```

### 站点配置
修改站点 _config.yml 文件中的 theme 选项为 Wikitten

## 插件安装：
### 全文搜索(hexo-generator-json-content)
安装：
```
npm install hexo-generator-json-content --save
```
#### 配置：
在站点配置文件_config.yml中添加
```
jsonContent:
  ignore:
    - path/to/a/page
    - url/to/one/post
    - an-entire-category
    - specific.file
    - .ext # a file extension
```
### 爬虫抓取(hexo-generator-sitemap)
安装：
```
npm install hexo-generator-sitemap --save
```
#### 配置：
在站点配置文件_config.yml中添加
```
sitemap:
    path: sitemap.xml
```

### 显示层的文章统一放在wiki文件夹

#### 配置：

在站点配置文件_config.yml中添加
```
permalink: wiki/:title/
```

### 建立多层分类(根据文章目录)

安装：
```
npm install --save hexo-directory-category
```

### 显示访客统计及页面访问量

#### 配置

1. 安装脚本

   ~~~javascript
   <script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   ~~~

   打开**themes/Wikitten/layout/common/head.ejs**，添加上述的脚本即可

2. 安装标签

   ```html
   <br>
   <span id="busuanzi_container_page_pv"><i class="fa fa-eye"></i> <span id="busuanzi_value_page_pv"></span></span>
   &nbsp;|&nbsp;
   <span id="busuanzi_container_site_uv"><i class="fa fa-user"></i> <span id="busuanzi_value_site_uv"></span></span>
   ```

   打开**themes/Wikitten/layout/common/footer.ejs**，添加上述的脚本即可





**---**

# 部署到Github
## 建立自己的主页
### 个人主页
- 每个GitHub帐号下只能有1个个人主页repo
- 命名必须是<username>/<username>.github.io的形式，例如我的个人主页[davidsmartwei/davidsmartwei.github.io](https://davidsmartwei.github.io/)

### 项目主页
- 可以有不限数量的项目主页repo
- 项目主页的GitHub二级域名为<username>.github.io/<projectname>，命名没有限制，例如我的项目主页[davidsmartwei/mywiki](https://davidsmartwei.github.io/mywiki)

## 为了绑定域名与Github page
在阿里云里购买一个域名，域名解析如下图
<img src="/images/aliyun.png" width=50% height=50% align=center/>

## 为了Github绑定域名
在自己网站项目repo的根目录添加CNAME
```
www.less-is-more.top
```
**tip**: 如果是直接在GitHub网页上添加文件的话，会遇到一个问题就是在通过hexo g -d之后hexo会把根目录下的CNAME文件删除。所以要把CNAME文件添加到/source目录下，这样hexo g -d之后hexo会自动把CNAME复制到/puclic目录下然后将/public路径下的内容进行复制并push到远程master分支的根目录下

# sublime text可视化编辑
## Sublime安装LiveReload插件
1.打开[Package Control官网](https://packagecontrol.io/installation)
2.复制代码到sublime中运行
3.按住shift+ctrl+p -> Package Control:Install Package -> enter
### 配置：
**方法1**：过用户自定义配置来开启
Preferences > Package Settings > LiveReload > Settings User
```
{"enabled_plugins": ["SimpleReloadPlugin","SimpleRefresh"]}
```
**方法2**：通过控制台命令来开启
```
1. Ctrl+Shift+P

2. LiveReload: Enable/disableplugins

3. Enable - Simple Reload
```
## Chrome浏览器安装LiveReload插件
**tip**: 接着右键单击选择管理扩展程序，把允许访问网址文件这一选项勾选上

---

# 基本操作：
## 站内链接：
```
// /2015/01/16/hello-world/
{% post_path hello-world %}

// <a href="/2015/01/16/hello-world/">Hello World</a>
{% post_link hello-world %}

// <a href="/2015/01/16/hello-world/">Custom Title</a>
{% post_link hello-world Custom Title %}
```
*eg*:
{% post_link hexo安装 %}
{% post_link hexo安装 Hexo安装及部署 %}

## 为了页面内跳转(锚点)
*eg*:
[top to 插件安装](#插件安装：)

## hexo new跳转打开sublime
在 Hexo 目录下的 scripts 目录（若没有，则新建一个）中创建一个 JavaScript 脚本opensubl.js，监听 hexo new 这个动作。并在检测到 hexo new 之后，执行编辑器打开的命令
```
var exec = require('child_process').exec;

// Hexo 3.x
hexo.on('new', function(data){
    exec('start "" "C:/Program Files/Sublime Text 3/sublime_text.exe" ' + data.path);
});
```



# 参考资料:
> - [用 Hexo 做个人 Wiki 知识管理系统](https://blog.zthxxx.me/posts/Personal-Wiki-System-Theme-for-Hexo/)
> - [一个仿 Wikitten 样式的 Hexo 个人 wiki 系统主题](https://github.com/zthxxx/hexo-theme-Wikitten/blob/master/README_zh-CN.md)
> - [Hexo博客常用插件及用法](http://blog.cofess.com/2017/08/16/hexo-blogs-comonly-used-plugins-and-usage.html)
> - [单个GitHub帐号下添加多个GitHub Pages的相关问题](http://chitanda.me/2015/11/03/multiple-git-pages-in-one-github-account/)
> - [GitHub Pages绑定顶级域名的方法](http://pytlab.org/2016/01/23/GitHub-Pages%E7%BB%91%E5%AE%9A%E9%A1%B6%E7%BA%A7%E5%9F%9F%E5%90%8D/)
> - [Hexo框架下给博客插入本地图片](https://steflerjiang.github.io/2016/12/20/Hexo%E6%A1%86%E6%9E%B6%E4%B8%8B%E7%BB%99%E5%8D%9A%E5%AE%A2%E6%8F%92%E5%85%A5%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87/)
> - [Sublime Text 3 配置Live​Reload实时刷新网页](https://www.jianshu.com/p/f4ec41257206)
> - [hexo的站内链接问题](http://blog.gezhiqiang.com/2016/11/27/hexo-inner-link/)
> - [在 hexo new 之后立即打开新建的 Markdown 文稿](https://liam0205.me/2015/05/01/open-editor-after-hexo-new-immediately/)
> - [搞定你的网站计数](http://ibruce.info/2015/04/04/busuanzi/)

