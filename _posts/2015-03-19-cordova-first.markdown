---
layout: index
title: 使用Cordova开发项目的一些心得体会
date: 2015-03-19 18:13:40
categories: [front, other, cordova]
---

##使用Cordova开发项目的一些心得体会
---
> 　　最近接触了一个需要将网页打包成为手机App，以达到快速成型目的的项目，故而需要使用Cordova来进行开发，也因此进行了一些对于使用Cordova开发手机App的实践，在这里记录Cordova的使用，以及这段时间以来的心得体会。

### Cordova基本开发环境

* 安装

　　Cordova的安装可以选择 *下载安装* 及 *使用node安装工具* 两种方式，而我采用了node安装的方式，所以需要先安装[nodejs](https://nodejs.org "nodejs")，之后使用npm命令安装：

		// MACOS
		$ sudo npm install -g cordova
		// windows
		C:\>npm install -g cordova


　　命令安装完之后，在node目录下就会产生cordova的文件夹了，再将它配置到环境变量中之后就支持cordova命令了。

* hello world

　　基本操作命令比较简单就直接记录命令了：

		// 创建项目
		cordova create {project name} [{project package} {app name}]
		// 添加环境
		cordova platform add [android|ios|...]
		// 添加插件
		cordova plugin add {plugin package}
		// 运行环境
		cordova run [android|ios|...]
		// 打包项目（需要先将证书文件拷到ant-build目录）
		cordova build -release

>
　　今天就先到这里，有空再从 *开发过程* 及 *心得体会* 两方面，对本文进行补充。