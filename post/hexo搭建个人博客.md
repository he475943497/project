---
title: hexo搭建个人博客
date: 2017-08-31 10:46:56
tags: [hexo,博客]
categories: 搭建博客
---
## 引言
想拥有一个个人博客，利用hexo可以满足基本需求，博客应有的基本功能都可以实现

## 环境 
ubantu mac windows 均可

## 必备 
git  nodejs  npm  nvm  具体怎么安装百度一下即可方法很多

## 注意 
所关联github上个人仓库必须和用户名相同 如用户名为heweiwei 则仓库名应该为heweiwei.github.io

## 中文乱码 
解决 vimrc添加一下内容
set fileencodings=utf-8,gb2312,gbk,gb18030
set termencoding=utf-8
set encoding=prc

## 安装hexo
前面各个必备软件安装正常
执行 npm install -g hexo-cli 

### 推送到远程
hexo init hexoweb
cd hexoweb/
npm install
hexo clean
hexo new your post name
hexo generate
hexo deploy
当然这样做之后访问的页面很简单具体修改参见:[hexo官方文档](https://hexo.io/zh-cn/docs/)

### 访问
浏览器输入yourname.github.io即可访问
也可以去域名服务商买一个域名
价格不等我买的.top顶级域第一年8块大洋

## 主题
我用的是next主题简单大体参照[next官方使用文档](http://theme-next.iissnan.com/theme-settings.html)就够了
更换背景：
找一个背景图片放到 hexo（hexo工程文件）themes/next/source/images 下
修改themes/next/source/css/_custom/custom.styl文件
在文件加上一代码
 body { background:url(/images/yourbackGround.jpg);} 

## 文章
外观设计基本搞定之后剩下的就是苦力活了
参照[hexo官方文档](https://hexo.io/zh-cn/docs/index.html)基本都能搞定

