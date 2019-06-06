---
layout:     post
title:      "IDEA常用操作"
subtitle:   ""
date:       2018-01-22 21:08:17
author:     "ZhengWeihao"
header-img: "imgs/post-bg-default.jpg"
tags:
    - Java
    - IDEA
---

IDEA常用操作
---

* 初始设置：
  * Maven设置（UserSetting、Automatically Download）
  * JDK设置
  * FileEncoding
  * 自动导入包：Editor-AutoImport
  * 打开多个项目方式：Appearance-SystemSetting-Project Opering
* 常用实时模板：
  * psvm
  * sout
  * fori
  * ifn
* 调试：
  * F7：进入下一步，且进入方法体
  * F8：进入下一步，且不进入方法体
  * F9：恢复运行，且在下个断点暂停
  * alt+F8：输入代码并查看执行结果
  * Condition：右击断点，选择Condition
* 常用快捷键：
  * 光标操作：
    * 跳转指定行：ctrl+g（command+g）
    * 进入光标所在方法：ctrl+b（command+b）
    * 返回历史光标位置：alt+ctrl+←/→（alt+command+←/→）
  * 一般操作：
    * 代码提示：ctrl+space（control+space）
    * 导入包：alt+enter（option+enter）
    * 查找定位：ctrl+shift+n（command+shift+n） [CLASS_NAME:LINE_NUMBER]
    * 代码生成：alt+insert（control+n, control+enter）
    * 切换窗口：ctrl+tab（control+tab）
    * 最近打开的文件：ctrl+e（command+e）
    * 切换全屏模式：shift+ctrl+f12（shift+command+f12）
    * 显示LIveTemplate：ctrl+j（command+j）
    * 面板：
      * 导航面板：ctrl+1（command+1）
      * 控制台面板：ctrl+4（command+4）
    * 查看当前类的所有方法：ctrl+f12(command+f12)
    * 错误解决：alt+enter（option+enter）
    * 引入变量：ctrl+alt+v（command+option+v）
    * 运行代码：alt+shift+f10（option+shift+f10）
    * 重命名：shift+f6
  * 编辑区：
    * 删除一行：ctrl+y（command+y）
    * 复制一行：ctrl+d（command+d）
    * 注释：ctrl+/（command+/）
    * 按语法选择：[shift] ctrl+w（command+w）
  * 查找：
    * 类/资源查找：ctrl+n（command+n）
    * 全局查找：shift+shift
    * 内容查找：ctrl+shift+n（command+shift+n）
  * 格式化：
    * import：ctrl+alt+o（command+alt+o）
    * 代码：ctrl+alt+l（command+alt+l）