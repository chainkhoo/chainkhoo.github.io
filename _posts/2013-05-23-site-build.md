---
layout: post
title: "最近完成：学习Git、搭建Jekyll博客和为主页启用SSL支持"
category: 技术笔记
tags: [git,jekyll,ssl]
---

前两天突然心血来潮学习github的使用，了解了git的意思（一种版本控制的方法），学习了一些基础的git命令，顺便搭建了一下久仰大名的jekyll静态博客,期间遇到问题无数，略作记录。

Git的基本命令我在Google上找到了一张非常明了的图![Git Command](http://geekpark-img.qiniudn.com/uploads/reading/seed/6574ad9fb8da5ec4bde82446d6f3390e.png)  
初学git的靠这个基本可以顺利操作。将你的本地代码推到github的基本命令只有三行：  

	git add .  
	git commit -m "Add new content"  
	git push origin master

最开始我是根据github pages里的帮助下载了原始的jekyll源代码，并且根据官方introducing配置了一下_config.yml。一开始没有理解intro的意思，企图在本地搭建jekyll，结果当然惨烈失败了，Windows下Ruby环境要多蛋疼有多蛋疼，各种不知名的错误层出不穷，于是我在与之奋斗了N久之后在网上找了找中文教程，发现了阮一峰写的[github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)，基本上就轻松搞定然后push到github上。
之后我又发现了[jekyllbootstrap](http://jekyllbootstrap.com/)和[搭建教程](http://ppxu.net/blog/2012/09/29/using-jekyll-bootstrap-host-blog-on-github/)，比原版的更加适合新手，大部分配置都已经弄好，自己简单地填几个空就搞定了。
搞定了jekyll之后，在master分支下新建了一个CNAME文件，只要将"blog.imqyc.com"填进去，再去设置一下域名解析就可以直接用blog.imqyc.com访问了，非常方便。

以前看到[@orvice](https://orx.me)的主页非常喜欢，简单明了，遂决定顺手copy过来，发现他的主页还启用了SSL支持（图片是狗爹的SSL支持，估计他的空间是在狗爹买的），甚是眼红，Google一番之后发现最有名的免费SSL是StartSSL，网上有很多[申请的教程](https://www.google.com/search?hl=zh-CN&q=startssl%E6%95%99%E7%A8%8B)，我轻松搞定申请之后，获得了两个证书，分别是`.key`文件和`.crt`文件，为了保证浏览器认识你的证书，后者还需要和startssl的根证书以及sub class1证书合并，然后可以将这俩证书丢到VPS的任意目录下面，安装完lnmp之后配置一下nginx，步骤如下：
  
1.修改nginx安装目录下(一般在`/usr/local/nginx`)的`nginx.conf`文件,我目前的配置如下：  

	listen       80;
	listen       443 ssl;
	server_name imqyc.com;

	ssl_certificate      /usr/local/nginx/conf/ssl/ssl_ca.crt;
	ssl_certificate_key  /usr/local/nginx/conf/ssl/ssl_ca.key;
	ssl_session_timeout  5m;

	index index.html index.htm index.php;
	root  /home/wwwroot;
		
	ssl_protocols  SSLv2 SSLv3 TLSv1;
	ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
	ssl_prefer_server_ciphers   on;
2.重启nginx :  

	/root/lnmp restart  

打开网页看了一下发现还是无法访问ssl加密主页，于是以各种姿势Google，折腾了各种方法均无效，于是祭出王牌，求助于大神（受）[@chenshaoju](https://twitter.com/chenshaoju)，发现是orca CDN不支持的问题，火速去改成直接解析到VPS上，反正主页也没东西，完全不需要CDN这种东西。  

3.成功打开了SSL页面！！！  
结果发现打不开http传输的页面了，提示 `400 Bad Request  The plain HTTP request was sent to HTTPS port` ，果断Google之，发现解决办法。

4.继续修改`nginx.conf`文件：  
在`http{... }`段内加入代码（但是不能在server里面）：
  
	map $scheme $fastcgi_https {
	default off;
	https on;
	}

在`server{... }`段内加入代码：  

	fastcgi_param  HTTPS $fastcgi_https;  
保存退出，然后运行	`/usr/local/nginx/sbin/nginx -t`检查配置文件是否出错，没错的话就可以	`/root/lnmp restart`重启nginx。我到这已经基本搞定，暂时没有其他错误出现了。
