---
layout: post
title:  "基于Github Pages与jekyll搭建个人博客"
comments: true
categories: 教程
---


## 小知识

---

### 1.Github Pages是什么？
Github Pages设计的初衷是为托管在GitHub上的项目提供介绍页面,开发者们可以通过GitHub Pages为他们的每一个项目创建一个用于介绍该项目的静态网站,不过由于他的空间免费而且稳定,因此用它搭建一个个人博客网站是再好不过了。

---

### 2.Jekyll是什么？

jekyll是一个简单的免费的Blog生成工具,将纯文本转化为静态网站和博客;由于咱们的GitHub Pages生成的是静态页面,每次更新博客都需要手动更改HTML,这就使得每次写博客都变得很麻烦,而用了这个工具以后,它会根据预先设置好的格式来生成博客内容,你就无需关心html代码,只需要把重心放在博客的写作上。

---

### 3.Markdown是什么？

Markdown是一种纯文本格式的标记语言。通过简单的标记语法，它可以使普通文本内容具有一定的格式。

---
## 搭建教程

---

#### 1.注册Github账户并建立仓库

仓库名称为：<用户名.github.io>

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

















