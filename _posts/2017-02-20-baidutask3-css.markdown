---
layout:     post
title:      "三栏式布局之双飞翼与圣杯"
subtitle:   "百度前端技术学院2016task1-3-1"
date:       2017-02-20 12:00:00
author:     "Small Star"
header-img: "img/baidutask2016/post-bg-css3.jpg"
tags:
    - 百度前端技术学院2016
    - css3

---

>在百度前端技术学院2016的春季任务中，第一阶段系列任务作为基础练习，主要是让学员熟练html及css。
其中，task1-9提供了从零开始到整个静态页面的整个搭设过程思路。task10/11涉及移动端，task12涉及css3新特性。

　　task3主要是利用css完成三栏式布局。关于三栏式布局，常规情况下，可以使用float实现，也可以使用relative来实现。
而task3中要求左右两层定宽，中间层的宽度随浏览器窗口宽度变化而变化，这种情况下，适合使用圣杯和双飞翼布局。
下图为task3中要求的效果图：

![](/img/baidutask2016/post-task3-xiaoguo1.jpg)

　　通过对圣杯及双飞翼布局学习之后，我认为两种布局的主旨是在html结构中，使中间的主体层在左右两层的前面。

1.两种布局的基本构思为：首先让中间层100%宽度占满同一高度的空间，在左右两层被挤出中间层所在区域时，
使用margin-left的负值将左右两个层拉回与中间层同一高度的空间，接下来调整左右两层到指定位置，
最后使用中间层的margin或padding属性使中间层的内容躲出左右两层占住的显示区。
2.两种布局的主要区别在于：圣杯布局采用一个父层包含中间、左右三个子层，设置父层的padding值腾出左右两层的显示区，
并对左右两层使用relative和left、right值调整位置；双飞翼采用中间、左右三层并列，再在中间层里设置一个子层，
设置中间层子层的margin值腾出左右两层的显示区，对左右两层使用margin值即可调整位置；

　　圣杯html结构：

	<div id="container">
		<div id="main"></div>
		<div id="left"></div>
		<div id="right"></div>
	</div>

　　圣杯css基本代码：

	#container{
		padding: 0px 120px 0 140px;/*腾出宽度*/
		}
	
	#main{
		width: 100%;
		position: relative;
		}
	
	#left{
		width: 140px;
		margin-left: -100%;/*将left层拉回main层所在高度区域*/
		left: -140px;/*调整位置*/
		position: relative;
		}
	
	#right{
		width: 120px;
		margin-left: -120px;/*将right层拉回main层所在高度区域*/
		right: -120px;/*调整位置*/
		position: relative;
		}

　　双飞翼html结构：

	<div id="main">
		<div id="main-inner"></div>
	</div>
	<div id="left"></div>
	<div id="right"></div>

　　双飞翼css基本代码：

	#main{
		width: 100%;
		position: relative;
		}
	
	#main-inner{
		margin：0 120px 0 140px；
		｝
	
	#left{
		width: 140px;
		margin-left: -100%;/*将left层拉回main层所在高度区域*/
		}
	
	#right{
		width: 120px;
		margin-left: -120px;/*将right层拉回main层所在高度区域*/
		}

　　以上，从理论上来讲，双飞翼布局可以比圣杯布局少设置4个属性：left、right层的position属性和"left"、"right"属性。
但在实际使用时并非如此，比如task3中，需要将网页设计成上面效果图的样式，可以看到left、main、right层之间都有间隙，
在使用双飞翼布局的时候，可以使用margin-left来调整层的位置，但此时对于left层来说，其margin-left值为-100%，
而间隙为定宽，不能使用百分比定位，此时，仍然要使用position（relative）和"left"属性来调整定位。
right层的margin-left值使用的就是像素值，此时可以直接用margin-right调整位置。

　　在设置样式过程中遇到的问题：

1.按照要求，设置了层的边框属性为1px solid #999999，在实现到网页中之后，发现边框的宽度为0.667px;
2.css3中可以对块级元素和内联块元素设置box-sizing属性，属性值分别为content-box（默认值，让元素维持W3C的标准Box Model，
元素的width/height值为元素内容区宽高）和border-box（让元素维持传统的Box Model，元素的width/height值为内容区宽高、
内边距宽高、边框宽高之和），在圣杯和双飞翼布局中，如果某层的宽度设置为100%，还需要设置其边框、内边距、外边距属性时，
为了避免该层撑破浏览器窗口宽度，需要将box-sizing属性设置为border-box;
3.margin属性的百分比值的参照是父元素的宽度，此处父元素的宽度是遵循content-box值的，并且，我发现，当我把父层的box-sizing
设置为border-box时，margin属性的百分比值参照的父元素宽度，仍然是按照content-box计算的;
4.双飞翼布局中，如果设置了main层的边框属性，在调整left层的位置时，必须考虑main层边框宽度，才能将left层调整到正确位置：
例如，设置了main层边框为1px soild #DEDEDE，此时，如果要将left层调整至距离main层边框20px的位置，在left层的margin-left
和position属性已经设置好了的情况下，此时需要将left层的left设置为20.667px，即20px加上main层边框宽度（宽度为何为0.667px见第1条）。

　　使用圣杯和双飞翼完成task3后的效果图如下：

![](/img/baidutask2016/post-task3-xiaoguo2.jpg)

　　使用圣杯和双飞翼完成布局的代码地址：https://github.com/smallstar92/baidutask/tree/gh-pages/task3;

　　demo效果网页：

1.圣杯：http://smallstarz.com/baidutask/task3/shengbei/task_1_3_1.html；
2.双飞翼：http://smallstarz.com/baidutask/task3/shuangfeiyi/task_1_3_1.html；

　　参考文章，第1篇为w3c中文章，第2篇为w3cplus文章，相对来说对box-sizing的解释要详尽许多：

1.《CSS3 box-sizing 属性》:http://www.w3school.com.cn/cssref/pr_box-sizing.asp;
2.《CSS3 Box-sizing》:http://www.w3cplus.com/content/css3-box-sizing;

