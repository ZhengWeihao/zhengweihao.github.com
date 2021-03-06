---
layout:     post
title:      "JQuery+HTML实现年会摇奖小工具"
subtitle:   ""
date:       2015-02-16 12:56:42
author:     "ZhengWeihao"
header-img: "imgs/post-bg-default.jpg"
tags:
    - JavaScript
---

JQuery+HTML实现年会摇奖小工具
---

* 背景：

    　　因公司年会抽奖需要而编写网页版的此工具。使用流程为：由工作人员为员工们发配个人编号（连续数字），然后在工具中设置抽奖编号区间，之后点击“摇奖”按钮进行摇号，点击“停止”按钮后，确认一个区间内数字，即为中奖号，而该号对应编号的同事即为中奖人了。

* 实现思路：

    　　首先定义两个数组：一为```号码数组```，将摇号的所有数字打乱次序后，放入其中；另外一个则是```中奖数组```，将中奖号码从号码数组中删去，存入到中奖数组中。

    　　定义好数组变量后，还需要一个```标志变量```，作为摇奖进行中与停止的标志。编写一个主函数，定时地进行执行，根据旗帜变量进行相应的操作：在摇奖过程中时，将号码数组的所有数字依依快速地显示到显示框中；在停止后，选定当时显示的数字作为中奖数字，并停止定时执行。实现如下:    　　

        //主函数
        var range = [];//号码数组
        var prize = [];//中奖数组
        var inGame = false;//旗帜变量
        var exe_i = 0;//当前执行到的下标
        function main() {
            if(inGame) {//继续执行
                //操作数组下标
                exe_i++;
                if(exe_i >= range.length) exe_i = 0;
        
                //显示数组内容
                $stage.html(range[exe_i]);
        
                //数组长度不够时自动结束
                if(range.length <= 1) inGame = false;
            } else {//结束游戏
                var cur = range[exe_i];//当前号码
        
                //更新数组数据
                prize.push(cur);
                range.splice(exe_i, 1);
        
                //显示结果
                console.log('下标:' + exe_i + ';数字:' + cur);
                alert('摇中号码：' + cur);
                msg();
        
                //停止定时任务
                window.clearInterval(mainInterval);
            }
        }

    　　有了主函数，工具已经完成了一大半，现在还需要一个触发的函数作为开始与停止操作的触发者。点击“开始”按钮时修改状态位为开始状态并开始定时执行主函数，而点击“停止”按钮时修改旗帜变量状态位为停止状态，主函数会根据状态为进入到停止操作。实现如下：

        //触发函数
        var mainInterval;//定时执行任务标识，通过它可以取消定时执行
        function play() {
            if(inGame) {//执行时触发 -> 停止
                inGame = false;
            } else {//非执行状态触发 -> 开始
                //检查执行环境
                if(range.length <= 0) {
                    alert('没号了，加几个进来呗');
                    return;
                }
                
                //开始执行
                inGame = true;
                mainInterval = self.setInterval('main()', 50);
            }
        }

    　　至此核心部分已经基本完成，当然还缺少一些细节的控制，比如：需要提供输入框进行交互；需要根据输入区间产生乱序的号码数组等，这里就不再赘述了，当然，最后还需要对界面美化一下~

* 结果展示：

    这里再提供一下最终完成的作品（背景图是人事MM找来的，原来丑得惨不忍睹。。。），[点我查看](/example/random-nums/index.html)，详细代码就请点```审查元素```咯 ^_^