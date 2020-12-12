---
layout: post
title:  "CentOS7安装Samba记录"
comments: true
categories: 记录
---
## 使用Samba服务的原因 
使用Samba服务，可以在Windows环境下提交Git代码，非常方便，不需要再在Linux下配置Git环境。

---
## 参考资料
基本操作参考：[Centos7 安装samba简单教程](https://www.jianshu.com/p/c01e1a674435)

登录shared用户无法退出参考：[记window10下samba切换用户](https://www.jianshu.com/p/39bbc363138f)

设置开机启动命令参考：[虚拟机centos7，开启samba，设置开机启动](https://blog.csdn.net/xiaoxinna1/article/details/48708665) 

---
## 解决主账户登录不进桌面、root账户可以问题
完成Samba配置后，主账户登录不进桌面，root账户可以，检查后发现是权限问题。

第一个教程上更改权限使用的命令为chown -R 777 /home/wesleylu/
这会导致权限出错，无法读取该文件夹下的数据

**解决方法：**
进入root账户桌面，在Terminal里运行chmod -R 777 /home/wesleylu/
问题解决！

---
