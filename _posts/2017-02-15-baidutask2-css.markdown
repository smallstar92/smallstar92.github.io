---
layout:     post
title:      "html5标签语义化及表单居中对齐"
subtitle:   "百度前端技术学院2016task1-2"
date:       2017-02-15 12:00:00
author:     "Small Star"
header-img: "img/baidutask2016/post-bg-html5.jpg"
tags:
    - 百度前端技术学院2016
    - html5
    - css3

---

>在百度前端技术学院2016的春季任务中，第一阶段系列任务作为基础练习，主要是让学员熟练html及css。
其中，task1-9提供了从零开始到整个静态页面的整个搭设过程思路。task10/11涉及移动端，task12涉及css3新特性。

　　task1主要是使用html搭设网页框架。之前通过模仿课程、书籍例子，已经搭建过网页，但当时未使用html5语义化标签。
因此，在做这个任务的时候，有两个目标：再次熟悉html语言；使用语义化标签。以下为完成html之后，无css情况下的页面图：

![](/img/baidutask2016/post-task1-html1.jpg)

　　通过4个列表对ul、ol、dd标签进行练习；通过一个表格对table标签进行练习，通过数个图片及链接对img、a标签进行练习；
通过1个表单对form标签进行练习。此外，就是使用语义化标签，本次任务中使用了article、header、aside及footer标签。
对各语义化标签的使用，下图应该可以一目了然：

![](/img/baidutask2016/post-task1-html2.jpg)

　　具体来说，各标签的使用情况如下：

1.header:代表“网页”或“section”的页眉。通常包含h1-h6元素或hgroup，作为整个页面或者一个内容块的标题。
也可以包裹一节的目录部分，一个搜索框，一个nav，或者任何相关logo。
2.footer:代表“网页”或“section”的页脚，通常含有该节的一些基本信息，譬如：作者，相关文档链接，版权资料。
如果footer元素包含了整个节，那么它们就代表附录，索引，提拔，许可协议，标签，类别等一些其他类似信息。
3.hgroup:代表“网页”或“section”的标题，当元素有多个层级时，该元素可以将h1到h6元素放在其内，譬如文章的主标题和副标题的组合。
4.nav:代表页面的导航链接区域。用于定义页面的主要导航部分。
5.aside:被包含在article元素中作为主要内容的附属信息部分，其中的内容可以是与当前文章有关的相关资料、标签、名次解释等。
在article元素之外使用作为页面或站点全局的附属信息部分。最典型的是侧边栏，其中的内容可以是日志串连，其他组的导航，甚至广告，这些内容相关的页面。
6.section:代表文档中的“节”或“段”，“段”可以是指一篇文章里按照主题的分段；“节”可以是指一个页面里的分组。
section通常还带标题，虽然html5中section会自动给标题h1-h6降级，但是最好手动给他们降级。
7.article:最容易跟section和div容易混淆，其实article代表一个在文档，页面或者网站中自成一体的内容，其目的是为了让开发者独立开发或重用。
譬如论坛的帖子，博客上的文章，一篇用户的评论，一个互动的widget小工具。

　　task2利用css编写样式，使用在task1所写页面。通过练习，对以下内容有深刻体会：
1.设置单行文本line-height等于父元素以上第一个高度定值的父级元素的height，可以使文本垂直居中于容器；
2.使用浮动（float）属性使列表横排；
3.缩进：text-indent；
4.表单的居中对齐（下文详细叙述）；
5.border-radius:可设置元素四角圆滑。

　　表单的居中对齐（或者定于某个值对齐）：为表单每个项加层（div），
再设置所有层浮动属性、宽度（主要控制对齐的轴线位置）属性、及文本对齐属性即可。

　　html代码：

	<form>
	<div>
		<label for="email" class="blign">请输入邮箱地址：</label>
		<input type="text" name="emailss" id="email"><br>
	</div>
		<p>邮箱地址请按要求格式输入</p>
	<div>
		<label for="pw1" class="blign">请输入密码：</label>
		<input type="password" name="pwd1" id="pw1">
	</div>
	<div>
		<label for="pw2" class="blign">请重复输入密码：</label>
		<input type="password" name="pwd2" id="pw2"><br>
	</div>
		<p>密码请为6-16英文数字</p>
	<div>
		<label class="blign">性别：</label>
		    <input type="radio" checked="checked" name="Sex" value="male">男
		    <input type="radio" name="Sex" value="female">女
	</div>
	<div>
        <label class="blign">城市：</label>
            <select name="">
            	<option value="北京">北京</option>
            	<option value="广州">广州</option>
            	<option value="上海">上海</option>
            </select>
    </div>
    <div>
        <label class="blign">爱好：</label>
            <input type="checkbox" name="checkbox1" value="checkbox">运动
            <input type="checkbox" name="checkbox2" value="checkbox">艺术
            <input type="checkbox" name="checkbox1" value="checkbox">科学
    </div>
    <div>
        <label class="blign">个人描述：</label>
            <textarea>这是一个多行输入框，输入你的个人描述</textarea>
    <div>
        <input type="submit" value="确认提交" id="smit">    
	</form>

　　css代码：
    
	.blign{
	    float: left;
	    width: 36%;
	    text-align: right;
		}

　　页面样式：

![](/img/baidutask2016/post-task1-css.jpg)
