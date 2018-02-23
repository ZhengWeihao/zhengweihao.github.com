---
layout:     post
title:      "Cordova开发笔记"
subtitle:   ""
date:       2015-03-19 18:13:40
author:     "ZhengWeihao"
header-img: "img/post-bg-2015.jpg"
tags:
    - Cordova
---

Cordova开发笔记
---
### Cordova基本开发环境

* 安装

　　Cordova的安装可以选择`下载安装`及`使用node安装工具`两种方式，而我采用了node安装的方式，所以需要先安装[nodejs](https://nodejs.org "nodejs")，之后使用npm命令安装：

		// MACOS
		$ sudo npm install -g cordova
		// windows
		C:\>npm install -g cordova


　　命令安装完之后，在node目录下就会产生cordova的文件夹了，再将它配置到环境变量中之后就支持cordova命令了。

　　但是还没结束，因为Cordova编译是通过ant来进行的，所以还需要安装ant，下载[ant](http://ant.apache.org "ant")并配置环境变量即可。

　　而如果还需要开发Android应用，则还需要下载安装[安卓开发工具](http://developer.android.com/tools/help/adt.html "Android Development Tools")，打开SDK Manager下载Cordova需要的SDK版本（当前使用21版本），并配置ANDROID_HOME环境变量即可。

* hello world



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
