---
layout: index
title: OSX常用操作笔记
date: 2015-10-28 14:39:51
categories: [osx, system] 
---

OSX常用操作笔记
---

* 环境变量设置：

    ```
    环境变量目录（按顺序加载）：
    /etc/profile
    /etc/bashrc
    ~/.bash_profile
    
    设置格式如：
    export PATH=path1:path2:${PATH}
    ```

* 设置Finder以显示路径：

    ```
    defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
    拷贝路径快捷键：
    Option+Command+C
    ```

* Host文件目录：

    * /etc/hosts

* JDK目录：

    * /Library/Java/JavaVirtualMachines/

* Finder 中显示隐藏文件：

    ```
    defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder  //显示
    defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder  //隐藏
    ```

* 隐藏美式键盘：

    ```
    备份 ~/Library/Preferences/com.apple.HIToolbox.plist
    使用 plist编辑器打开 ~/Library/Preferences/com.apple.HIToolbox.plist
    删除 AppleEnableInputSources 中不需要的节点。
    重启系统
    ```

    ​