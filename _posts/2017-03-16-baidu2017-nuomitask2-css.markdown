---
layout:     post
title:      "利用border-radius属性及动画属性制作圆饼loading动画"
subtitle:   "百度前端技术学院2017糯米任务CSS3圆饼loading效果"
date:       2017-03-16 12:00:00
author:     "Small Star"
header-img: "img/baidutask2017/post-bg-css3.jpg"
tags:
    - 百度前端技术学院2017
    - css3

---

>百度前端技术学院2017于2月24日开始进行，一共有6个学院可供学习，分别是小薇学院（html/css基础）、斌斌学院（js基础）、耀耀学院（小游戏/交互）、
商业平台学院（web/ios/android）、ECharts & WebVR学院（VR相关）和糯米学院（css3/vue等），从百度前端技术学院今年推出的各学院任务看来，
目前互联网行业对从事前端行业的程序员要求越发高了。转行前端或是才开始学习前端的同学任重而道远，大家一起加油，共勉。

　　《CSS3圆饼loading效果》作为糯米学院中的进阶任务，目的主要是让学员通过完成任务，掌握border-radius属性的使用以及旋转动画的设置。
在已经完成鼠标悬浮效果任务之后，相对来说本任务要简单很多。以下是总结。

　　任务分解：该任务要达到的![效果](http://ife.baidu.com/course/detail/id/36)。如网页中显示情况，可将任务分解为两个小任务：

1.制作四分之三圆环并旋转；
2.制作圆饼，圆饼内有两种颜色，分别交替旋转扩大其扇形区域（从0deg到到360deg），以达到圆饼loading效果。

　　通过完成任务，使用到如下一些重要属性：

1.制作圆环，使用伪元素，使用border(包括其transparent值)；
2.制作圆饼过程中，使用到animation-timing-function属性的steps()曲线；
3.圆饼及圆环的旋转，使用animation系列属性，并使用transform设置旋转角度，使用transform-origin属性设置旋转中心。

　　以下是完成本任务后的html结构：
    
	<header>
	  <h1><a href="http://smallstarz.com/">小星的博客|Small Star's Blog</a></h1>
	  <ul>
		<li><a href="#top-bottom1">点我</a></li>
		<li><a href="#top-bottom2">快点我</a></li>
		  <li><a href="#top-bottom3">点我吧</a></li>
		  <li><a href="#top-bottom4">点我啦</a></li>
	  </ul>
	</header>
	<article>
	  <p class="round1">
		<span class="bgcircle">
		  <span class="left"><span class="spining"></span></span>
		  <span class="right"><span class="spining"></span></span>
		</span>
	  </p>
	  <p class="round2">
		<span class="bgcircle">
		  <span class="inner">
			<span class="spiner"></span>
			<span class="filler"></span>
			<span class="masker"></span>
		  </span>
		  <span class="inner2">
			<span class="spiner"></span>
			<span class="filler"></span>
			<span class="masker"></span>
		  </span>
		</span>
	  </p>
	</article>
	<footer>
	  <p>版权所有 &copy 小星</p>
	</footer>
	</body>
	</html>

　　其中，round1为使用第一种方法制作圆饼时的loading整体结构，round2为使用第二种方法制作圆饼时的loading整体结构。
可以明显发现round2方法结构复杂，实际上其实现方法也较round1方法笨拙。结构样式代码请见文末demo代码地址。页面图如下：

![](/img/baidutask2017/post-nuomitask2-ym.jpg)

　　圆环及圆饼制作原理如下：

　　1.四分之三圆环的旋转：
　　实现原理：首先将上文html结构中的类名为bgcircle的span元素设置为圆形（将其border-radius设置为50%即可），其宽高大于两个子元素span，
以便圆环与圆饼分开。在.bgcircle上设置伪元素，同样设置为圆形，大小与.bgcircle相同，再设置伪元素边框，其中任一边框设置为transparent即可。
最后利用animotion和transform设置旋转动画。css代码如下：

	/*2-1-1.制作大圆背景*/
	.bgcircle{
	  position:relative;
	  display:inline-block;
	  height:80px;
	  width:80px;
	  margin:50% 0 0 50%;
	  top:-30px;
	  left:-30px;
	  border-radius:50%;
	  overflow:hidden;
	}
	/*2-2.loading圆环*/
	.bgcircle::before{
	  content:"";
	  position:absolute;
	  width:100%;
	  height:100%;
	  box-sizing:border-box;
	  border:4px solid #BD50A3;
	  border-top:4px solid transparent;
	  border-radius:50%;
	  -webkit-animation:circle-spining 4s infinite linear;
	  animation:circle-spining 4s infinite linear;
	  -webkit-transform-origin:50% 50% 0;
	  transform-origin:50% 50% 0;
	}
	@keyframes circle-spining{
	  0%{transform:rotate(0deg);}
	  50%{transform:rotate(-180deg);}
	  100%{transform:rotate(-360deg);}
	}
	@-webkit-keyframes circle-spining{
	  0%{-webkit-transform:rotate(0deg);}
	  50%{-webkit-transform:rotate(-180deg);}
	  100%{-webkit-transform:rotate(-360deg);}
	}

　　2.圆饼旋转：
　　（1）方法一：主要参考![codepen](http://codepen.io/)网站中![Geoffrey Crofte](http://codepen.io/CreativeJuiz/)的
![CSS Loader](http://codepen.io/CreativeJuiz/pen/vFBIh)demo，其中也写了一些其他loading效果。
总体来说，这些loading效果的主要原理都是：在.bgcircle之下，设置.left、.right为左右半圆，拼接为一个圆饼，
对.left，设置其子元素.spining往右移至.right区域，并形成半圆与.right区域重叠，此时只有.spining旋转至.left区域时才能显示出来。
同理，设置了.right的子元素.spining。最后，对两个.spining子元素设置不同的旋转动画已达到要求效果。
css代码过长，请见文末demo代码地址，其旋转过程如下步骤表述：

　　a.初始状态下，两个.spining都没有旋转（图中深紫色圆饼）：此时.left.spining围绕圆心开始逆时针旋转，至-180deg：

![](/img/baidutask2017/post-nuomitask2-1.jpg)

　　b.“.left.spining”围绕圆心开始逆时针旋转，至-180deg（图中浅紫色半圆）：

![](/img/baidutask2017/post-nuomitask2-2.jpg)

　　c.“.left.spining”停止旋转，“.right.spining”开始旋转至-180deg，此时两个.spining都显示出来了，形成浅紫色圆饼：

![](/img/baidutask2017/post-nuomitask2-3.jpg)

　　d.“.right.spining”停止旋转，“.left.spining”又开始旋转，从-180deg旋转至-360deg，此时只剩下“.right.spining”形成的浅紫色右半圆：

![](/img/baidutask2017/post-nuomitask2-4.jpg)

　　e.“.left.spining”一周的旋转动画结束，停止旋转；“.right.spining”开始最后半周的旋转，至-360deg停止。整个一周的动画结束。
对此动画设置无限循环次数，即可达到最终效果。

![](/img/baidutask2017/post-nuomitask2-5.jpg)

　　（2）方法二：参考![张鑫旭博客](http://www.zhangxinxu.com/)的文章![CSS3实现鸡蛋饼饼状图loading等待转转转](http://www.zhangxinxu.com/wordpress/2014/04/css3-pie-loading-waiting-animation/)，
在张鑫旭前辈的这篇文章里，利用比喻把他的方法原理表述的很清楚，简单来说：他将动画分成了两半，如果以本文前面页面图中右边的圆饼作为例子的话，
前半动画是深蓝色由12点钟位置开始逆时针旋转至-360deg，最后将橙色覆盖；后半动画刚好相反，橙色由12点钟位置开始逆时针旋转至-360deg，最后将深蓝色覆盖。
由于两半部分动画的过程是相同的，所以将前半部分动画制作完成后，将该动画过程复制一份，将颜色对调，重新套入相同结构的新元素中，再将新的一套重叠在
旧一套元素所占区域，设置前半段时间第一套元素显示、第二套元素透明，后半段时间第二套元素显示、第一套元素透明。这样就完成了圆饼loading效果。
下面以前半部分动画为例表述下动画过程：

　　首先需要了解html结构（以本文开始所列html结构为例），设置类名为inner的span作为父元素，该元素背景为深蓝色，其下同列三个span作为子元素，
类名分别为：spiner、filler、masker。以上四者中，按照原理，.filler、.masker的z-index应该设置得高于.spiner，
但在实际设置中，张鑫旭前辈没有设置z-index值，而是利用在html结构中将需要优先显示的元素并列在后面的方式
（如果在html结构中将.masker与.spiner对调，就会出现错误），规避了这点。.masker为左半圆，深蓝色；.filler为右半圆，橙色；
.spiner为旋转半圆，橙色，初始位置位于左半部分。

　　a.初始状态：.masker透明，此时由.spiner和.filler分别位于左右方拼接成橙色圆饼：

![](/img/baidutask2017/post-nuomitask2-6.jpg)

　　b.此时，.spiner开始围绕圆心逆时针旋转，则作为父元素的.inner开始从12点钟方向逆时针显露（深蓝色），到.spiner旋转-180deg时，
.masker由透明变为显示，.filler由显示变为透明：

![](/img/baidutask2017/post-nuomitask2-7.jpg)

　　c.之后，.spiner继续逆时针旋转，由于.filler已经透明，则.inner从6点钟方向逆时针显露，而左半圆区域由于.masker已经显示（z-index值比.spiner大），
也为深蓝色，这样，当.spiner旋转一周之后，就由橙色全部变为蓝色：

![](/img/baidutask2017/post-nuomitask2-8.jpg)

　　以上为动画原理，css代码过长，请见文末demo代码地址。在实现这个方法的过程中，还需要注意，要变化透明度的元素（.inner/.inner2/.masker/.filler），
其透明度变化并不是渐变，而是突变，所以需要设置animation-timing-function为steps()突变曲线，steps(n,start/end)主要有两个值，
简单来说，n设置突变次数（整数），start/end设置在动画开始还是结束突变。

　　思考：

1.利用steps()曲线，可以制作一些更炫酷的css3动画。并且，在同一个方向上连续渐变的动画，由于渐变位置不同，
通常需要两个以上元素实现，但利用steps()曲线突变位置，就能通过一个元素实现了；
2.完成以上任务之后，可以优化的地方：第一个方法中的.spining子元素，可以使用伪元素替代；同理，第二个方法中的子元素，也可用伪元素替代；
3.圆环除了定长旋转，也可根据圆饼loading旋转的方法，实现变长旋转loading效果。

　　具体参考已在文中引出，不再单列，以下为我的demo及demo代码：

　　![我的demo](http://smallstarz.com/baidutask-2017/nuomi-task2-loadingcircle/loadingcircle.html);
　　![demo代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/nuomi-task2-loadingcircle);

