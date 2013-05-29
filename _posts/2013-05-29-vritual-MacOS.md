---
layout: post
title: "为虚拟机搭建MacOSX系统"
category: 技术笔记
tags: [macosx,virtual]
---

今年新买了一个笔记本电脑，本打算购入心仪已久的Macbook Pro 15‘’ With Retina Display，但是学校校园网需要使用客户端登录（锐捷在Windows下独有的客户端）·，为了不出现MBP装Windows的糗样，不得已只好买了Sony S系列的Windows电脑（坑爹的是价格还比RMBP贵=。-），结果最近学校改用Web页面登录校园网了，真是一口老血喷在屏幕上，非常眼红同学在用的MBP，为了过过干瘾，以及我翻了800遍还在第一页的Objective-C教程（-.-），遂决定在虚拟机上安装一把Mac OS X。  
参考教程：  
[在virtualbox上安装Mac OS X Lion 之 配置过程](http://www.crifan.com/install_mac_os_x_lion_on_virtualbox_config_process/)  
[在 Win 7 下使用 VirtualBOX 虚拟机安装 OS X 10.8 Mountain Lion 及 XCode 4.4.1 (iOS SDK5.1) 作开发](http://bbs.weiphone.com/read-htm-tid-5329046.html)
开始前准备如下：  
1.[Oracle VM VirtualBox4.2.12.exe](http://download.virtualbox.org/virtualbox/4.2.12/VirtualBox-4.2.12-84980-Win.exe)  
2.[VirtualBox 4.2.12 Oracle VM VirtualBox Extension Pack ](http://download.virtualbox.org/virtualbox/4.2.12/Oracle_VM_VirtualBox_Extension_Pack-4.2.12-84980.vbox-extpack)  
3.[Mac OS X Mountain Lion 10.8.3.iso](http://kuai.xunlei.com/d/.nniAAIKgADS-KVR3d5)  
4.[HackBoot 1.iso/HackBoot 2.iso](http://kuai.xunlei.com/d/.nniAPXMJv6lUQQA166)  
5.[MultiBeast.zip](http://kuai.xunlei.com/d/.nniAAJPgAC--aVRf75)  
首先安装`Oracle VM VirtualBox4.2.12.exe`和`VirtualBox 4.2.12 Oracle VM VirtualBox Extension Pack`。  
然后开始虚拟机的安装：  
1.在VirtualBox中新建虚拟机
![新建虚拟机](http://storage.live.com/items/9A8B8BF501A38A36!1918?filename=%e7%82%b9%e5%87%bb%e6%96%b0%e5%bb%ba[1].png)  
设置虚拟机名称和系统类型  
![虚拟机设置](http://images.weiphone.com/attachments/Day_121007/39_261697_a483919e83e9c9f.png)  
内存设置最好在2048M以上（安装10.8.3需要分配起码4096M内存以上）  
![内存分配](http://storage.live.com/items/9A8B8BF501A38A36!1921?filename=%e5%86%85%e5%ad%98%20%e8%ae%be%e7%bd%ae[1].png)  
下一步选虚拟硬盘  
![虚拟硬盘](http://storage.live.com/items/9A8B8BF501A38A36!1922?filename=%e8%99%9a%e6%8b%9f%e7%a1%ac%e7%9b%98%20%e4%b8%bb%e7%a1%ac%e7%9b%98.png)  
默认选择 VDI（Virtualbox磁盘映像）即可。  
![虚拟硬盘向导](http://storage.live.com/items/9A8B8BF501A38A36!1923?filename=VDI%20Virtualbox%e7%a3%81%e7%9b%98%e6%98%a0%e5%83%8f.png)  
之后是虚拟硬盘细节（我在这里选择的是动态分配）  
![虚拟硬盘细节](http://storage.live.com/items/9A8B8BF501A38A36!1924?filename=%e5%9b%ba%e5%ae%9a%e5%a4%a7%e5%b0%8f.png)  
这两个分配方法的不同之处在于：  
固定大小：优点是不需要以后动态根据使用情况而分配，可提高性能。缺点是，一次性直接占用整个你所分配的，比如30G的硬盘空间。

动态分配：可根据虚拟机实际使用硬盘的大小而只分配用到的那一部分。比如你虚拟机创建完毕了，只用了10G硬盘，那么此时虽然你给虚拟机设置了30G硬盘，但是此时虚拟机大小也只是10G。缺点是，需要虚拟机动态的根据使用情况而去分配对应的空间，效率相对低，性能相对没有固定大小分配的效率高。  
但是因为我把虚拟机都放在C盘了，所以选择了动态分配，目前感觉如果和我一样只是偶尔使用的话，动态分配就够用了。  
然后选择虚拟硬盘的位置和大小  
![虚拟硬盘位置和大小](http://storage.live.com/items/9A8B8BF501A38A36!1925?filename=%e8%99%9a%e6%8b%9f%e7%a3%81%e7%9b%98%e6%96%87%e4%bb%b6%e4%bd%8d%e7%bd%ae%e5%92%8c%e5%a4%a7%e5%b0%8f.png)  
推荐至少设置20GB空间，如果需要安装Xcode和iOS SDK等开发工具的话就设置成40GB，当然 如果是像我一样选择动态分配的话，再设置大一些也是可以的，因为动态分配这种方式在没有实际占用的时候是不会占用空间的。  
点击创建以后需要等待VirtualBox开始创建虚拟磁盘就OK了（这一步需要一些时间，请耐心等待）。  
2.虚拟机设置：  
选中新建的虚拟机，在右侧可以看到一些虚拟机的明细  
![前往设置](http://storage.live.com/items/9A8B8BF501A38A36!1931?filename=%e9%80%89%e4%b8%ad%e8%99%9a%e6%8b%9f%e6%9c%ba%20%e8%ae%be%e7%bd%ae[1].png)  
进入“系统”项。取消软驱和网络，然后将光驱设置在硬盘之前启动，取消EFI和UTC时间的勾选，其他按照图片上的设置就行了  
![主板设置](http://storage.live.com/items/9A8B8BF501A38A36!1933?filename=%e5%8f%96%e6%b6%88%e8%bd%af%e9%a9%b1%20%e8%ae%be%e7%bd%ae%e5%85%89%e9%a9%b1%e5%92%8c%e7%a1%ac%e7%9b%98%20%e5%8f%96%e6%b6%88EFI.png)  
接着是“处理器”选项，这里因为我的电脑是i7的，所以我分了一半给虚拟机，但是这样的话Windows会稍微有一些卡顿，所以如果是喜欢在打开虚拟机的同时开其他一堆东西的人，可以设置为2核  
![处理器设置](http://storage.live.com/items/9A8B8BF501A38A36!1934?filename=%e5%a4%84%e7%90%86%e5%99%a8%e8%ae%be%e7%bd%ae.png)  
再进入“显示”项，将显存大小调到最大的128M，并启用3D加速  
![显卡设置](http://storage.live.com/items/9A8B8BF501A38A36!1936?filename=%e6%98%be%e7%a4%ba%e8%ae%be%e7%bd%ae.png)  
“存储”项，新添加一个模拟光盘  
![存储设置](http://storage.live.com/items/9A8B8BF501A38A36!1938?filename=%e9%80%89%e6%8b%a9%e4%b8%80%e4%b8%aa%e8%99%9a%e6%8b%9f%e5%85%89%e7%9b%98[1].png)  
这里选择之前准备工作中下载的`HackBoot 1.iso`作为引导  

3.安装虚拟机系统：  
接下来就可以启动虚拟机了  
![启动虚拟机](http://storage.live.com/items/9A8B8BF501A38A36!1941?filename=%e9%80%89%e4%b8%ad%20%e7%82%b9%e5%87%bb%e5%90%af%e5%8a%a8[1].png)  
在`HackBoot 1.iso`的启动引导下，可以看到如下页面  
![HackBoot 1](http://images.weiphone.com/attachments/Day_121007/39_261697_8c3d5f68b6b4ddf.png)  
这个时候在虚拟机右下角光盘图标上点击，选择下载好的系统镜像`Mac OS X Mountain Lion 10.8.3.iso`  然后按F5刷新后，回车选择系统镜像就可以开始安装了
![安装系统](http://images.weiphone.com/attachments/Day_121008/39_261697_158e510283b1bca.png)  
接下去按照正常步骤安装系统（这一步如果出现鼠标动不了可能得等一会，如果长时间不动，则强制关闭虚拟机，重新设置`HackBoot 1.iso`引导启动，重复一下上述步骤再试试一般就OK了）  
详细过程如下：  
![1](http://storage.live.com/items/9A8B8BF501A38A36!1966?filename=%e4%bb%a5%e7%ae%80%e4%bd%93%e4%b8%ad%e6%96%87%e4%bd%9c%e4%b8%ba%e4%b8%bb%e8%a6%81%e8%af%ad%e8%a8%801%5B1%5D.png)  
![2](http://storage.live.com/items/9A8B8BF501A38A36!1968?filename=%e5%ae%9e%e7%94%a8%e5%b7%a5%e5%85%b7%20%e7%a3%81%e7%9b%98%e5%b7%a5%e5%85%b71.png)  
这一步需要我们将虚拟硬盘格式化分区  
![3](http://storage.live.com/items/9A8B8BF501A38A36!1971?filename=%e5%88%86%e5%8c%ba%20%e8%af%a6%e7%bb%86%e8%ae%be%e7%bd%ae1%5B1%5D.png)  
然后一路确认下去  
![4](http://storage.live.com/items/9A8B8BF501A38A36!1975?filename=%e8%be%93%e5%85%a5%e5%90%8d%e7%a7%b0%20%e7%82%b9%e5%87%bb%20%e6%8a%b9%e6%8e%891.png)  
关闭磁盘工具  
![5](http://storage.live.com/items/9A8B8BF501A38A36!1963?filename=%e7%82%b9%e5%87%bb%e5%b7%a6%e4%b8%8a%e8%a7%92%e7%9a%84x%20%e5%85%b3%e9%97%ad%e5%bd%93%e5%89%8d%e7%aa%97%e5%8f%a3.png)  
选中磁盘安装就好了  
![6](http://storage.live.com/items/9A8B8BF501A38A36!1977?filename=%e5%8f%af%e4%bb%a5%e7%9c%8b%e5%88%b0%e6%96%b0%e5%bb%ba%e7%9a%84%e7%a3%81%e7%9b%98%e4%ba%86%5B1%5D.png)  
安装过程  
![7](http://storage.live.com/items/9A8B8BF501A38A36!1990?filename=%e6%ad%a3%e5%9c%a8%e5%ae%89%e8%a3%85%20%e5%a4%a7%e7%ba%a617%e5%88%86%e9%92%9f.png)    
安装成功  
![8](http://storage.live.com/items/9A8B8BF501A38A36!1991?filename=%e5%ae%89%e8%a3%85%e6%88%90%e5%8a%9f.png)  


安装完成后关闭虚拟机，更改引导镜像为`HackBoot 2.iso`，再次启动虚拟机，出现如下画面，右键选择Macintosh HD回车启动系统  
![引导启动](http://images.weiphone.com/attachments/Day_121007/39_261697_3d3346e74d39aff.png)  
启动后，选择“系统偏好设置”，“安全性与私隐”将允许“任何来源”打开  
![任何来源](http://images.weiphone.com/attachments/Day_120802/19_261697_84330cb6053d8e5.png)  
然后将前面准备的`MultiBeast.zip`解压，安装其中的`MultiBeast 4.6.1.pkg`勾选其中4项  

UserDSDT Install
-
 System Utilities -> Repair Permissions  
-
 AppleHDA Rollback
-
 NullCPUPowerManagement  
-
  
![](http://images.weiphone.com/attachments/Day_121007/39_261697_6fba090a195c759.png)  
![](http://images.weiphone.com/attachments/Day_121007/39_261697_9f3163ff9a13f06.png)  
![](http://images.weiphone.com/attachments/Day_121007/39_261697_91ac5544649ddbd.png)   
在Finder 菜单，前往 -> 前往文件夹中输入`/System/Library/Extensions/` 删除其中的`AppleGraphicsControl.kext`文件  
在Finder 菜单，前往 -> 前往文件夹中输入`/Extra/` 修改其中的`org.Chameleon.boot.plist`文件，增加分辨率内容如下
   
	<key>Graphics Mode</key>  
	<string>1440x900x32</string>

也可以选择其他分辨率如：  
>
1152x720x32  
1366x768x32  
1440x768x32  

然后关机重启就万事大吉啦！