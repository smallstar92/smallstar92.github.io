---
layout:     post
title:      "css3动画属性探究（transition、transform、@keyframes）及伪元素的使用"
subtitle:   "百度前端技术学院2017糯米任务鼠标悬浮效果"
date:       2017-02-26 12:00:00
author:     "Small Star"
header-img: "img/baidutask2017/post-bg-css3.jpg"
tags:
    - 百度前端技术学院2017
    - css3

---

>百度前端技术学院2017于2月24日开始进行，一共有6个学院可供学习，分别是小薇学院（html/css基础）、斌斌学院（js基础）、耀耀学院（小游戏/交互）、
商业平台学院（web/ios/android）、ECharts & WebVR学院（VR相关）和糯米学院（css3/vue等），从百度前端技术学院今年推出的各学院任务看来，
目前互联网行业对从事前端行业的程序员要求越发高了。转行前端或是才开始学习前端的同学任重而道远，大家一起加油，共勉。

　　《有趣的鼠标悬浮模糊效果》作为糯米学院中的进阶任务，目的主要是让学院通过完成任务，掌握transition、transform、@keyframes等属性的使用，
从任务的标签“CSS3、动画、过渡、效果”也能看出其主旨。经过3天的努力，终于把这个任务完成了，通过连续不断的发现bug、解bug,
不仅对很多属性进行使用，也越来越有一套独自解决问题的办法。以下是总结。

　　任务分解：该任务要达到的效果是，当鼠标悬浮在按钮上方时，按钮上的文字由透明到上浮淡出，文字上有的颜色有流光渐变效果，按钮的边框从上、右、
下、左四边的中间出现并向两端扩展至衔接在一起，按钮的背景由清晰变为模糊。则按以上要求，可以将这个大的任务分解为四个小任务：

1.文字的上浮淡出效果；<br/>
2.文字颜色的流光渐变效果；<br/>
3.按钮边框的中间向两端扩展效果；<br/>
4.背景的模糊效果<br/>

　　打完上面这四个小怪兽，这个任务也就基本被解决了。总结下来，我用了这些技能打怪兽：

1.4个效果都使用到伪类:hover；<br/>
2.“边框扩展”、“背景模糊”使用了伪元素::before、::after；<br/>
3.“文字上浮淡出、“边框扩展”、“背景模糊”都用到了transition属性；<br/>
4.“文字流光渐变”使用了@keyframes系列属性、以及chrome浏览器支持的-webkit-background-clip、-webkit-text-fill-color、-webkit-linear-gradient；<br/>
5.“背景模糊”使用了filter属性及其值blur();<br/>
6.“边框扩展”使用了transform属性及其值scale3d()、rotate()，使用了transform-origin属性。<br/>

　　以下是单个按钮html结构代码：
    
	<article>
		<figure>
			<div id="left-right1">
				<div id="top-bottom1">
					<a href="http://smallstarz.com/">“小星的博客|Small Star's Blog”</a>
				</div>
			</div>
		</figure>
	</article>

　　其中，figure元素用于放背景，也用于实现背景模糊效果；两个div层主要用于产生伪元素，模仿边框；a元素用于实现文字的上浮淡出。
figure元素的宽高要大于子元素，两个div层和a元素的宽高相同。该结构是可以优化的：<br/>
1.可以使用a元素产生伪元素，但使用a元素产生deep伪元素模仿边框扩展的时候，必须给伪元素另加过渡效果来抵消a元素实现文字上浮淡出时产生的移动；<br/>
2.实现边框扩展时用了两种方法，其中一种要使用4个伪元素，另一种使用到2个伪元素，使用2个伪元素的方法只需一个div层即可。<br/>
以下是构建按钮结构的css代码：<br/>

	article{
		float:left;
		width:100%;
		height:500px;
	}
	figure{
		position:relative;
		width:400px;
		height:180px;
		margin:0 0 0 50%;
		top:160px;
		left:-200px;
		box-sizing:border-box;
		background:url(../img/button-bg.jpg);
		border-radius:5px;
	}
	figure>div{
		position:relative;
		width:320px;
		height:120px;
		top:30px;
		left:40px;
		box-sizing:border-box;
	}
	div>div{
		position:relative;
		width:320px;
		height:120px;
	}
	div>a{
		display:block;
		position: relative;
		text-align:center;
		line-height:120px;
	}

　　按钮样式如下：

![](/img/baidutask2017/post-nuomitask1-anniu.jpg)

　　4个效果的实现及属性的使用如下：

　　1.文字的上浮淡出：<br/>
　　实现原理：在鼠标未悬浮在按钮上时，设置a元素下移一段距离（top:14px），文字透明（opacity:0）；在鼠标悬浮时，
a元素回到正常位置（top:0），文字不再透明（opacity:1）。属性值的变化使用transition过渡，即可达到平滑变化效果。
效果请见文末demo链接，代码如下:

	div>a{
		top:14px;
		opacity:0;
		-webkit-transition:opacity 0.3s linear,top 0.3s linear; 
		transition:opacity 0.3s linear,top 0.3s linear;
	}
	figure:hover a{
		top:0;
		opacity:1; 
	}

　　2.文字颜色流光渐变效果：<br/>
　　实现原理：目前没有特定的css属性达到这样的效果，只能使用其他属性组合来达到这样的效果。简单来说，是将文字镂空，使其显示其背景，
然后对背景使用从左至右的颜色渐变，再在鼠标悬浮时，对背景进行移动来模仿流光渐变。具体来说，以“-webkit-background-clip:text;
-webkit-text-fill-color:transparent;”来进行文字镂空（前者裁剪掉文字内容之外的背景，后者将文字填充色设为透明），
以“background-image:-webkit-linear-gradient()”来实现背景颜色渐变，最后使用@keyframes来进行背景平移的无限循环以模仿流动。
在此，需要注意的是，要做到平移结束与平移开始之间的衔接平滑：一是必须设置平移速度为匀速（linear）；还有就是在结束和开始那一瞬间，
背景颜色渐变的样式相同，对此，可以通过设置颜色渐变为“0% x色”-“25% y色”-“50% x色”-“75% y色”-“100% x色”，在将背景拉长至200%来实现。
效果请见文末demo链接，代码如下：

	div>a{
		-webkit-background-clip:text;
		-webkit-text-fill-color:transparent;
		background-image:-webkit-linear-gradient(left,yellow,green 25%,yellow 50%,green 75%,yellow 100%);
		background-size:200% 100%;	
	}
	@keyframes gradient{
		from{background-position:0% 0%;}
		to{background-position:-100% 0%;}
	}
	figure:hover a{ 
		animation:gradient 3s linear infinite; 
	}

　　3.背景模糊效果：<br/>
　　即毛玻璃效果，实现原理：在背景所在的元素上使用伪元素，使伪元素宽高、位置完全与背景所在元素相同，再将元素背景设置为伪元素的背景，
对伪元素使用“filter:blur()”即可。同上，使用transition来达到平滑过渡。效果请见文末demo链接，代码如下：

	figure::before{
		content:'';
		position:absolute;
		top:0;
		right:0;
		bottom:0;
		left:0;
		background:url(../img/button-bg.jpg);
		border-radius:5px;
		-webkit-transition:filter 0.3s linear;
		transition:filter 0.3s linear;
	}
	figure:hover::before{
		filter:blur(5px);
	}

　　4.边框扩展：<br/>
　　（1）方法一：使用4个伪元素来模仿4个边框，通过在鼠标悬浮与否时使用transform的3D缩放（scale3d()）来达到效果。
需要注意的是，在使用3D缩放时，是应该设置缩放原点的（transform-origin），由于默认的缩放原点为元素几何中点，
与边框扩展所要求的位置刚好相同，所以在本处可以不设定该属性。效果请见文末demo链接，代码及注释如下：

	/*2-5-1.使用scale3d实现边框扩展*/
	/*2-5-1-1.构建伪元素*/
	#left-right1::before,
	#left-right1::after,
	#top-bottom1::before,
	#top-bottom1::after{
		content:'';
		position:absolute;
		background:#ffffff;
		-webkit-transition:-webkit-transform 0.3s;
		transition:transform 0.3s;
		-webkit-transition-timing-function:cubic-bezier(0.44, 0.05, 0.55, 0.95);
		transition-timing-function:cubic-bezier(0.44, 0.05, 0.55, 0.95);/*贝塞尔曲线*/
	}
	/*2-5-1-2.左右边框构建*/
	#left-right1::before,
	#left-right1::after{
		width:2px;
		height:100%;
		top:0;
		-webkit-transform: scale3d(0.1, 0, 1);
		transform: scale3d(0.1, 0, 1);
	}
	#left-right1::before{
		left:0;
	}
	#left-right1::after{
		right:0;
	}
	/*2-5-1-3.上下边框构建*/
	#top-bottom1::before,
	#top-bottom1::after{
		width:100%;
		height:2px;
		left:0;
		-webkit-transform: scale3d(0, 0.1, 1);
		transform: scale3d(0, 0.1, 1);
	}
	#top-bottom1::before{
		top:0;
	}
	#top-bottom1::after{
		bottom:0;
	}
	/*2-5-1-4.使用伪类*/
	figure:hover #left-right1::before,
	figure:hover #left-right1::after,
	figure:hover #top-bottom1::before,
	figure:hover #top-bottom1::after{
		-webkit-transform: scale3d(1, 1, 1);
		transform: scale3d(1, 1, 1);
	}

　　对方法一的实现效果，如果仔细观察是可以发现与方法二（见后文）的细微区别的：第一，扩展速度使用了贝塞尔曲线，不是匀速，
并且速度曲线的选择有多种，第二种方法却很难做到匀速意外的其他变速类型；第二，扩展实际是往4个方向进行的，
如果将模仿的边框宽度设置的更大，这个效果更明显，这主要是因为在上面代码中设置的scale3d(x,y,z)的3个值当中，x和y都是改变的。

　　（2）方法二：使用2个伪元素来模仿左右、上下两对边框的扩展。以左右边框为例：假设使用::before伪元素来实现，
此时将伪元素width设置为100%，height设置为0，top设置为50%，并将其左右边框设定为需要宽度，上下边框设置为0；
在鼠标悬浮时，改变height为100%,top为0即可。效果请见文末demo链接，代码如下：

	#top-bottom2::before,
	#top-bottom2::after{
		content:'';
		position:absolute;
		box-sizing:border-box;
	}
	#top-bottom2::before{
		width:100%;
		height:0;
		top:50%;
		border-left:2px solid #fff;
		border-right:2px solid #fff;
		-webkit-transition:height 0.3s linear,top 0.3s linear;
		transition:height 0.3s linear,top 0.3s linear;
	}
	article figure:hover #top-bottom2::before{
		height:100%;
		top:0;
	}
	#top-bottom2::after{
		width:0;
		height:100%;
		top:0;
		left:50%;
		border-top:2px solid #fff;
		border-bottom:2px solid #fff;
		-webkit-transition:width 0.3s linear,left 0.3s linear;
		transition:width 0.3s linear,left 0.3s linear;
	}
	article figure:hover #top-bottom2::after{
		width:100%;
		left:0;
	}

　　以上，需要特别注意box-sizing属性的设定，如果按照w3c的标准Box Model，即content-box,此时两个伪元素内容区的宽高相同，
但一个伪元素有左右边框，一个伪元素有上下边框，会使得最后形成的边框存在缝隙，如下：

![](/img/baidutask2017/post-nuomitask1-bug.jpg)

　　因此，在方法二中，对于伪元素，必须设定box-sizing为border-box。
　　
　　（3）边框的其他效果一：<br/>
　　主要利用transition-delay属性，在指定的时间开始过渡效果，从而达到边框的连续动画效果。
效果请见文末demo链接，具体代码请见文末代码链接（过长）：

　　（4）边框的其他效果二：<br/>
　　同时使用transform的scale3d()、rotate()，配合transform-origin属性，达到效果：从按钮四个角所在点出发，
垂直于边框方向射出射线，同时射线顺时针旋转回到边框所在位置并最终耦合成边框。
效果图如下，效果请见文末demo链接，具体代码请见文末代码链接：

![](/img/baidutask2017/post-nuomitask1-border.jpg)

　　以上，为整个任务的完成过程，及完成各个小任务的原理及使用属性。此外，在完成demo期间，也注意到以下方面：

1.对transition的使用：平滑过渡是双面的，在鼠标未悬浮到悬浮时需做到平滑，在悬浮到未悬浮时也要做到平滑。
对此，如果两个过程的动作相同（注意开始时间也要相同），即两次过渡的transition属性设定相同，在原本的元素使用transition即可，
伪类选择时的元素中不用再次设定，否则两边都要设定；<br/>
2.善用伪元素，伪元素只在渲染层表现，不影响html的本来结构，通过使用伪元素，可以制作很多炫丽的效果；<br/>
3.对代码进行优化，在完成demo之后，对css代码进行了优化，存在选择器不精炼、不同元素的相同样式未合并、注释不成体系等缺点。

　　最后，附上解决各个任务的参考资料：

1.“文字的上浮淡出”：这个小怪兽是在打完另外几个小怪兽之后解决的，没找帮手就解决了；<br/>
2.“文字颜色流光渐变效果”：主要参考了w3cplus大漠老师的《css3 background-clip》:http://www.w3cplus.com/content/css3-background-clip;
以及已完成这个任务的同学的笔记。w3cplus大漠老师的文章对学习css3非常有帮助，对transition、transform的使用，也参考了大漠老师的文章；<br/>
3.“背景模糊效果”：《css揭秘》第4章第18节毛玻璃效果；<br/>
4.“边框扩展”：在任务的网页下方有在线参考资料，其中有Codrops的链接，Codrops网站的第二项“Inspiration for Line Menu Styles”就是
各种按钮效果的展示，其中就有边框扩展。作为github上的开源项目，可直接fork下载项目源码，进行研究学习。
边框扩展是其中一个叫ariel的高手做的。需要注意的是，这个项目中各类按钮的状态变化是鼠标悬浮、鼠标点击按钮和鼠标点击其他按钮三种，
其中点击事件使用了js代码，效果的展现全是使用的css3属性。<br/>

![](/img/baidutask2017/post-nuomitask1-ariel.jpg)

　　我的demo:http://smallstarz.com/baidutask-2017/nuomi-task1-mouseSuspending/mouseSuspending.html;<br/>
　　demo代码:https://github.com/smallstar92/baidutask-2017/tree/gh-pages/nuomi-task1-mouseSuspending;

