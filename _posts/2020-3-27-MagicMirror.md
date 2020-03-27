---
layout: post
title: "中国民航大学院级大创-树莓派魔镜"
comments: true
categories: 项目

---
#### 硬件部分

* 树莓派3B+：ARM Cortex-A53 CPU，1GB内存  
* 8GB以上内存卡：Sandisk、Sony、Kingston等  
* 显示器及其驱动板：必须有，尺寸你自定  
* 原子镜：与显示器尺寸一致  
* HDMI线：连接显示器和树莓派，当然如果你显示器为VGA接口也没关系，用VGA转HDMI转接头就行  
* UPS不间断电源：通过一个电源输入，同时给树莓派和显示器供电，在停电情况下仍可持续运行一段时间  
* 其他设备：鼠标、键盘

---
#### 软件部分

* MagicMirror²
MagicMirror²是一个开源模块化智能镜像平台。随着越来越多的可安装模块列表，MagicMirror²允许您将走廊或浴室镜子转换为个人助理。

**官网地址：**
https://magicmirror.builders/


**官方文档地址：**
https://docs.magicmirror.builders/


**项目效果图：**

![树莓派魔镜效果图](https://i.loli.net/2020/03/27/HUx8R7F1WZwJrla.jpg)


**树莓派VMware虚拟机百度网盘下载链接：**


https://pan.baidu.com/s/19zhgD9B7eIPAwWhD81monw **提取码：**xe8c

```
虚拟机文件使用方法：
1、下载压缩包，解压在D盘中；
2、使用VMware，选择打开虚拟机，选择Raspberry x86.vmx；
3、选择编辑虚拟机设置，配置虚拟机配置为适合你计算机的设置；
4、开启此虚拟机；
5、开机进入系统后，会自动全屏打开MagicMirror。
```

```
MagicMirror使用方法：
1、项目路径：/home/pi/WebProject/MagicMirror
2、配置文件位置：/home/pi/WebProject/MagicMirror/config/config.js
3、打开MagicMirror方法：进入项目路径，运行“npm start”;
退出MagicMirror方法：在MagicMirror界面按Ctrl+Q;
打开MagicMirror任务栏方法：在MagicMirror界面按Ctrl，再按alt。
```

*  第三方模块

**第三方模块地址：**https://github.com/MichMich/MagicMirror/wiki/3rd-Party-Modules

---
