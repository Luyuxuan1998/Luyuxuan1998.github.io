---
layout: post
title:  "制作树莓派魔镜"
comments: true
categories: 记录
---

#### 树莓派开机自启动chrome浏览器并进入某网址

##### 目的：设置树莓派开机自启动chrome浏览器

首先我们要了解几个命令： 

Linux下命令行打开chromium浏览器：

{% highlight ruby %}
chromium-browser
{% endhighlight %}

Linux下命令行打开chromium浏览器进入某个网址：

{% highlight ruby %}
chromium-browser "http://######/"
{% endhighlight %}

Linux下命令行进入某网址并设置浏览器全屏：

{% highlight ruby %}
chromium-browser "http://######" -kiosk
{% endhighlight %}

(-kiosk表示开启浏览器超级全屏模式)

##### 现在我们要做的是开机就执行这个命令。

我们进入一个文件夹内
{% highlight ruby %}
cd  /home/pi/.config/autostart/
{% endhighlight %}

（如果你一级一级进入并查看文件夹，你会发现你找不到.config文件夹，因为这是个隐藏目录，需要ls -a才会显示） 

autostart文件夹听名字就能知道这个文件夹储存的是自启动文件。 

进入autostart文件夹，你能发现只有一个.desktop文件（我不知道你们是不是这样），打开看一下。 

我的只有LXinput-setup.desktop这个文件。

{% highlight ruby %}
vi LXinput-setup.desktop
{% endhighlight %}

#### 解释如下： 

[Desktop Entry] 文件头 Desktop Entry文件是Linux 桌面系统中标准的程序启动配置描述方式。 

Name 应用名称 

Comment 注释 

NoDisplay 隐藏菜单栏 

Exec 执行的命令 

NotShowIn 我也不知道是啥（我只是个菜鸟） 

当然.desktop文件还有很多属性，有兴趣的可以Google一下。

##### 现在我们也要写一个.desktop文件了，就仿照上面，写一个简单点的。 

执行命令：
{% highlight ruby %}
vi my.desktop
{% endhighlight %}

(新建一个文件my.desktop)然后在里面编辑就行，如下所示。 


保存后。重启。 

{% highlight ruby %}
sudo reboot
{% endhighlight %}

然后重启时你就能自启动chromium并进入输入的网址。因为-kiosk设置了浏览器超级全屏模式，所以要退出程序需要按Ctrl +F4。

我的树莓派开机过一两秒才会自启动浏览器，需要耐心等待一会儿。

---
* 教程来源网址：[https://blog.csdn.net/szu_Vegetable_Bird/article/details/80231660](https://blog.csdn.net/szu_Vegetable_Bird/article/details/80231660)
