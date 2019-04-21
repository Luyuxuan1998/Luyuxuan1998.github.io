---
layout: post
title:  "Git的安装及操作代码"
comments: true
categories: git
---

## Git的安装及操作代码

一、Git的安装：

1.下载

Git官方下载地址：https://git-scm.com/downloads

2.安装

更改安装目录，无脑下一步

3.初始化设置

（1）Win+R打开命令提示框

（2）输入
{% highlight ruby %}
git config --global user.name "用户名"
git config --global user.email "邮箱"
git config --list
{% endhighlight %}
（3）若出现以下信息，说明初始化设置成功
{% highlight ruby %}
core.symlinks=false
core.autocrlf=true
core.fscache=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
help.format=html
rebase.autosquash=true
http.sslbackend=openssl
http.sslcainfo=D:/Git/mingw64/ssl/certs/ca-bundle.crt
credential.helper=manager
user.name=你填写的用户名
user.email=你填写的邮箱
{% endhighlight %}
二、建立Git仓库：

1.新建一个项目文件夹

如建立Initialized empty Git repository in E:/MyGitProject/.git/文件夹

2.在命令提示框里输入
{% highlight ruby %}
E：
cd MyGitProject
git init
{% endhighlight %}
  如出现Initialized empty Git repository in E:/MyGitProject/.git/则说明仓库已创建
  
3.在命令提示框里输入git add README.md，填写项目介绍，注意使用UTF-8无BOM编码格式

4.在命令提示框里输入
{% highlight ruby %}
git commit -m "add a readme file"
{% endhighlight %}
提交文件至暂存区域
如出现
{% highlight ruby %}
[master (root-commit) 552c5d6] add a readme file
1 file changed, 1 insertion(+)
create mode 100644 README.md
{% endhighlight %}	
   说明正常
   
Tips:
【将工作目录的文件放到Git仓库只需要两步：
	git add 文件名
	git commit -m "你做了什么"】

三、查看状态

1.在命令提示框中输入
{% highlight ruby %}
git status
{% endhighlight %}	
如出现
{% highlight ruby %}
On branch master
nothing to commit, working tree clean
{% endhighlight %}
   说明你在master的分支里，工作目录未更新

   如出现
{% highlight ruby %}
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

		LICENSE

nothing added to commit but untracked files present (use "git add" to track)
{% endhighlight %}	
   说明你的工作目录已更新，但未上传至暂存区，可通过git add <file>命令进行提交。

   如出现
{% highlight ruby %}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

		modified:   LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
{% endhighlight %}
   说明工作区与暂存区该文件不一致，可使用git add <file>进行提交，或者使用git checkout -- <file>进行云端覆盖本地

四、查看历史提交

1.在命令提示框中输入
{% highlight ruby %}
git log
{% endhighlight %}
   如出现
{% highlight ruby %}
On branch master
nothing to commit, working tree clean

E:\MyGitProject>git log
commit dabadbee2cde93a8c73ce611e7813aabcf920ee6 (HEAD -> master)
Author: Luyuxuan1998 <2575837178@qq.com>
Date:   Sat Apr 20 12:30:28 2019 +0800

	change the License file

commit f18f10ee646bb1909258434bed402f0d34506212
Author: Luyuxuan1998 <2575837178@qq.com>
Date:   Sat Apr 20 12:25:13 2019 +0800

	add a LICENSE file

commit 552c5d616a8f9006c93951c98e74c46b42ab75be
Author: Luyuxuan1998 <2575837178@qq.com>
Date:   Sat Apr 20 12:03:01 2019 +0800

	add a README file
{% endhighlight %}
   则说明进行了三次版本提交，并能够查看更改人的信息与更改的备注

五、返回过去
 
（1）回滚：
{% highlight ruby %}
git reset --mixed HEAD~
git reset --soft HEAD~
git reset --hard HEAD~
{% endhighlight %}	
（2）回滚个别文件
{% highlight ruby %}
git reset 版本快照 文件名/路径
{% endhighlight %}
（3）回滚至比当前版本新的版本
{% highlight ruby %}
git reset 版本快照的ID号
{% endhighlight %}


