---
layout: index
title: JQuery+CSS3实现雪花飘落效果
date: 2015-03-19 18:13:40
categories: [frontend, cordova]
---

JQuery+CSS3实现雪花飘落效果
---

> 看到淘宝APP上出现了雪花飘落的效果，脑袋一动觉得CSS3似乎也很好实现，就动手做了这么个demo。

* 预期效果：
  产生漫天雪花，从天空飘落到地面，停留稍瞬后消融。

* 实现思路：
  第一步是产生雪花，即产生div，并利用随机数，随机一定区间的数字作为div的宽高、透明度，并设置border-radius属性为宽高值，即可得到一片大小、透明度随机的雪花。之后将雪花位置设置在屏幕顶端，横向位置随机，即完成雪花的初始化。示例代码如下：

  	// 产生雪花dom
  	function snowflake(size, alpha, top, left) {
  		var s = document.createElement('div');
  		$(s).css({
  			'width': size,
  			'height': size,
  			'border-radius': size,
  			'background-color': 'rgba(255,255,255,' + alpha + ')',
  			'top': -50,
  			'left': left,
  		}).addClass('snowflake');
  		return s;
  	}
  	
  	// 获取一片随机大小、透明度的雪花示例
  	var size = Math.random() * flakeSize + 2;
  	var alpha = Math.random() * 0.7 + 0.1;
  	var left = Math.random() * $(window).width();
  	var s = snowflake(size, alpha, 0, left);

  产生雪花后，设置css3的transition属性，并将div移动到屏幕底部，即可开启动画效果。同时，雪花飘落后，停顿若干时长再将dom删除，即可达到雪花消融效果。示例代码如下：

  	.snowflake {
  		background-color: rgba(255, 255, 255, 0.5);
  		display: inline-block;
  		transition: top 2s;// 此处认为动画时间为2秒较为合适,实际情况中可自行调整
  		transition-timing-function: cubic-bezier(0.25,0.1,0.25,1);// 设置动画的贝塞尔值,使得动画不显得生硬,具体值同样也可按个人喜好调整
  	}
  	
  	// 利用setTimeout方法,延时消除雪花,melt的值即为雪花飘落后留存的时长
  	setTimeout(function() {
  		$s.remove();
  	}, 2000 + melt);
  	
  	完成一片雪花的动画效果后，只要快速不停地循环这个，即能产生雪花不停飘落的效果了。示例代码如下：
  	
  	var it = setInterval(function() {
  		// 初始化雪花
  		var id = 's_' + (++animateId);
  		var size = Math.random() * flakeSize + 2;
  		var alpha = Math.random() * 0.7 + 0.1;
  		var left = Math.random() * $(window).width();
  		var s = snowflake(size, alpha, 0, left);
  		var $s = $(s).attr('id', id);
  		$sky.get(0).appendChild(s);
  	
  		// 雪花飘落并消融,延迟执行以确保dom已经生成好
  		setTimeout(function() {
  			$s.css('top', maxTop + (flakeSize - $s.width()) / 2);
  	
  			setTimeout(function() {
  				$s.remove();
  			}, 2000 + melt);
  		}, 100);
  	}, rate);

* 结果展示：
  进入页面随机开始频率为60ms，融化时长2s的动画，页面向下拖动可设置参数，[点我查看页面](/example/snowflake/index.html)。
