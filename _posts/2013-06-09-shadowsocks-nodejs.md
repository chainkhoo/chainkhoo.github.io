---
layout: post
title: "搭建Shadowsocks-nodejs详细过程与错误记录"
category: 技术笔记
tags: [shadowsocks,nodejs,VPS]
---

对于天朝用户来说，一些高端科学上网手段是必备的，否则你将无法访问许多不存在的网站，这无异于坐井观天，你将永远不知道外面的世界有多精彩。  
最近比较流行的技术是[Shadowsocks](http://www.shadowsocks.org/)，这是一款跨平台轻量级的高效代理工具，它由[@clowwindy](https://github.com/clowwindy/)最先开发，之后众多热心的技术达人一起完善，目前在服务器端已经支持[`Python`](https://github.com/clowwindy/shadowsocks)、[`nodejs`](https://github.com/clowwindy/shadowsocks-nodejs)、[`Go`](https://github.com/shadowsocks/shadowsocks-go)、[`libev`](https://github.com/madeye/shadowsocks-libev)、[`libuv`](https://github.com/dndx/shadowsocks-libuv)、[`erlang`](https://github.com/Yongke/shadowsocks-erlang)、[`dotcloud`](https://github.com/clowwindy/shadowsocks-dotcloud)等众多版本，而在客户端上，支持Windows、MacOSX、Linux、iOS、Android甚至是一些openWRT的路由器。以目前Shadowsocks的发展，个人感觉在安全性、可用性上绝对比SSH/Goagent等服务更加靠谱，目前已经有诸多以前贩售SSH的JS改为贩售Shadowsocks了。  
我选择的是[Shadowsocks-nodejs版本](https://github.com/clowwindy/shadowsocks-nodejs)，部署起来非常方便，首先为VPS安装上nodejs0.8.22（暂时不要安装最新的0.10.10）：    

	wget http://nodejs.org/dist/v0.8.22/node-v0.8.22.tar.gz
	tar xf node-v0.8.22.tar.gz
	cd node-v0.8.22/
	./configure
	make -j2 && sudo make install  

然后将github上的repo clone下来：  

	git clone git://github.com/clowwindy/shadowsocks-nodejs.git
	cd shadowsocks-nodejs  

（如果VPS上没有安装git的话也可以先下载好源代码，修改好配置后再上传到VPS。）  

修改`config.json`文件：  

	{
	"server": "0.0.0.0",  
	"server_port": 8388,        //服务器端口，最好修改成自己的，防止扫描
	"local_port": 1080,
	"password": "barfoo!",      // 换一个密码，保证加密性
	"timeout": 600,
	"method": null              // 支持"bf-cfb", "aes-256-cfb", "des-cfb", "rc4"等加密方式。默认是不加密          
	}  
然后就可以后台运行Shadowsocks了：  

	nohup node server.js > log &  
完成以上步骤，服务器端的设置就算OK了，然后下载一个客户端（推荐作者最新的[跨平台客户端](http://sourceforge.net/projects/shadowsocksgui/files/dist/)），因为Shadowsocks的协议是通用的，无论你部署的服务端用的是什么语言，任何客户端都是可以使用的。  
按照在服务器上改的`config,json`配置文件来对客户端进行设置： 
 
	server IP:   //输入服务器的IP地址  
	server Port:  //输入服务器开启的端口  
	password:  //保持与服务器端设置一致  
	Local Port:  //设置一个本地转发的端口，浏览器里设置代理就用这个端口  
	Encryption Method:  //对应你服务器上的加密方式，如果是默认则选table  
	Timeout in Second:  //保持与服务器端设置一致  

之后就可以开始访问各种不存在的网站了。速度飞快，就我个人的测试是比goagent快很多的。  

如果希望保持服务器上的Shadowsocks时刻开启，则最好设置一个crontab，定时重启Shadowsocks。你也可以使用高端大气的monit时刻监控服务，保证其时刻开启，参考[http://stackoverflow.com/questions/298760/how-to-make-sure-an-application-keeps-running-on-linux](http://stackoverflow.com/questions/298760/how-to-make-sure-an-application-keeps-running-on-linux)  
