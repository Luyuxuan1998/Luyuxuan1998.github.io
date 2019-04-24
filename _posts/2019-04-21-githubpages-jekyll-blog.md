---
layout: post
title:  "基于Github Pages与jekyll搭建个人博客"
comments: true
categories: 教程
---


## 小知识

---
### 1.Git简介

Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。

GitHub可以托管各种git库的站点。

GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。


---

### 2.Github Pages是什么？
Github Pages设计的初衷是为托管在GitHub上的项目提供介绍页面,开发者们可以通过GitHub Pages为他们的每一个项目创建一个用于介绍该项目的静态网站,不过由于他的空间免费而且稳定,因此用它搭建一个个人博客网站是再好不过了。

（1）搭建简单而且免费；

（2）支持静态脚本；

（3）可以绑定你的域名；

（4）DIY自由发挥，动手实践一些有意思的东西git,markdown,bootstrap,jekyll；

（4）理想写博环境，git+github+markdown+jekyll；

---

### 3.Jekyll是什么？

Jekyll是一种简单的、适用于博客的、静态网站生成引擎。它使用一个模板目录作为网站布局的基础框架，支持Markdown、Textile等标记语言的解析，提供了模板、变量、插件等功能，最终生成一个完整的静态Web站点。说白了就是，只要安装Jekyll的规范和结构，不用写html，就可以生成网站。

---

### 4.Markdown是什么？

Markdown是一种纯文本格式的标记语言。通过简单的标记语法，它可以使普通文本内容具有一定的格式。

---
## 搭建教程

---

#### 1.注册Github账户并建立仓库

仓库名称为：<用户名.github.io>,如下图所示

<img src="https://luyuxuan1998.github.io/pictures/personalsite.png" alt="personalsite" width="540">

---

#### 2.下载GitHub Desktop并安装

下载网址：[https://desktop.github.com/](https://desktop.github.com/)

---

#### 3.下载并安装Ruby

下载网址：[https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.3.tar.gz](https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.3.tar.gz)

---

#### 4.使用命令提示行安装jekyll

{% highlight ruby %}
gem install bundler jekyll
{% endhighlight %}	

---

#### 5.使用GitHub Desktop拉取<用户名.github.io>仓库至本地

---

#### 6.至jekyll官网下载jekyll主题并粘贴至本地文件夹中

网址：[http://jekyllthemes.org/](http://jekyllthemes.org/)

---

#### 7.使用命令提示行输入

{% highlight ruby %}
cd 用户名.github.io
bundle exec jekyll serve
{% endhighlight %}	

---

#### 8.使用[http://localhost:4000](http://localhost:4000)访问你的本地网站

如出现与jekyll主题一致的网站，说明本地已安装成功

---

#### 9.使用GitHub Desktop推送'用户名.github.io'仓库至GitHub上

如使用[https://用户名.github.io](https://用户名.github.io)可以访问网站，则说明基于jekyll个人Github Pages博客已经搭建完成

---


















