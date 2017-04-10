---
layout:     post
title:      "百度前端技术学院2017小薇学院阶段性总结"
subtitle:   "task4-10小结"
date:       2017-04-10 12:00:00
author:     "Small Star"
header-img: "img/baidutask2017/post-bg-css3.jpg"
tags:
    - 百度前端技术学院2017
    - html/css

---

>百度前端技术学院2017于2月24日开始进行，一共有6个学院可供学习，分别是小薇学院（html/css基础）、斌斌学院（js基础）、耀耀学院（小游戏/交互）、
商业平台学院（web/ios/android）、ECharts & WebVR学院（VR相关）和糯米学院（css3/vue等），从百度前端技术学院今年推出的各学院任务看来，
目前互联网行业对从事前端行业的程序员要求越发高了。转行前端或是才开始学习前端的同学任重而道远，大家一起加油，共勉。

　　在将小薇学院任务完成之后，对完成各任务期间所掌握的一些细节，以及部分任务要求掌握的系统知识进行了小结。
以下这些内容主要涉及的是自己在制作过程中所遇到的问题，触及的是一些点，如果需要系统的掌握结构样式这方面的内容，还是需要多练。

　　*task4：《定位与居中》。<br>各类情况下的居中，在以下的情况中，对涉及flex的描述可能不大恰当，因为对于flex而言，内部元素的排列是按主侧轴定义的：*

　　A:水平居中。<br>
1.行内元素：在父元素中设置text-align；（包括flexbox）<br>
2.单个块级元素：a.令margin-left/margin-right:auto；b.定宽时，absolute定位，left:50%,margin-left:-1/2height<br>
3.多个块级元素：a.设置为inline-block，在父元素中使用text-align；b.设置父元素为伸缩容器，并设置justify-content:center。

　　B:竖直居中。<br>
1.单行行内元素：当父元素不定高时，设置其padding-top/padding-bottom相等；此外，对于块级元素中的单行文字，设置块级元素line-height=height；<br>
2.多行行内元素：a.采用padding-top/padding-bottom相等；b.使其处于表格中，自动居中；
c.使其父元素display为table，其自身display为table-cell，再设置父元素vertical-align:middle；d.如果父元素为伸缩容器（定高），子元素自会竖直居中；
e.在父元素中使用伪元素撑高（height:100%），vertical-align:middle，此时子元素设为inline-block，vertical-align:middle。<br>
3.块级元素：a.定高时，absolute定位，top:50%,margin-top:-1/2height；b.不定高时，同a，此时将margin-top:-1/2height改为transform:translateY(-50%)；
c.设置父元素为伸缩容器，并设置align-content:center。

　　C:水平竖直居中（针对单个块级元素）。<br>
1.定高定宽：绝对定位，left/top:50%,margin-left/margin-top:-1/2height；<br>
2.不定宽高时：同上理，此时使用translateX/translateY替代margin-left/margin-top即可；<br>
3.设置父元素为伸缩容器，同时设置justify-content/align-content为center。

　　[demo](http://smallstarz.com/baidutask-2017/xiaoweixueyuan/task4/task_1_4_1.html);<br>
　　[代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/xiaoweixueyuan/task4);<br>
　　
　　*task6：《通过HTML及CSS模拟报纸排版》。本任务主要是各种字体大小颜色不同等的设置，以及涉及区块定位问题：*

1.对于首字下沉效果：需要注意此时切记设置首字行高等于font-size，如果不设置，在某些浏览器中会实现首字下沉效果（如火狐），另一些则不会；<br>
2.如果文本中含有url地址等内容，需对其设置word-wrap:break-word，强制自动换行，否则某些浏览器中会出现该文本溢出容器的情况；<br>

　　[demo](http://smallstarz.com/baidutask-2017/xiaoweixueyuan/task6/task_1_6_1.html);<br>
　　[代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/xiaoweixueyuan/task6);<br>
　　效果图如下：

![](/img/baidutask2017/post-xiaowei-1.png)

　　*task7：《实现常见的技术产品官网的页面架构及样式布局》。本任务涉及了使用ps切图等一些ps基础技能，在完成任务的过程中，
应该考虑到官网实际应该是怎样的，然后合理使用标签，增加一些psd图中不能看到的css效果：*

1.对按钮能元素合理使用transition属性，使其效果平滑；<br>
2.不能将块级元素放进行内元素，即使是将行内元素设置为block也不行，因为此时对浏览器而言，其在解释过程中，并未将该块级元素放进行内元素，
则在这种情况下，某些选择器会失效，对行内元素使用伪类来控制该块级元素也会失效；<br>
3.对行内元素，定位后，宜设为inline-block，并定高；<br>
4.目前的css3标准下，伪类只能控制其本身和子元素。

　　[demo](http://smallstarz.com/baidutask-2017/xiaoweixueyuan/task7/task_1_7_1.html);<br>
　　[代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/xiaoweixueyuan/task7);<br>
　　效果图如下：

![](/img/baidutask2017/post-xiaowei-2.png)

　　*task8：《响应式网格（栅格化）布局》。了解其原理及bootstrap栅格化布局原理：*

1.响应式主要使用到了媒体查询，@media；<br>
2.栅格化原理：将父容器宽度平均化为n等份，其下子元素按照多少等分来分配宽度（不能超过n），在对子元素浮动之后，子元素即会进行合理排列；<br>
3.bootstrap的栅格原理：其把父元素分成了12等份，最基本的情况下有三层结构，假设最外层为contain，中间层为row-1,最里层为col-1-n，
此时设置contain的padding值/row-1的margin绝对值/col-1-n的padding值三者相等，其中row-1的margin值为负值，这样就能进行成功嵌套，
如果还需在col-1-n层内部继续栅格化，此时就可将col-1-n直接看作该栅格的contain层，在其中加入row-2和col-2-n即可（padding和margin等值分别与最外的栅格布局相关层的值相等）。

　　[demo](http://smallstarz.com/baidutask-2017/xiaoweixueyuan/task8/task_1_8_1.html);<br>
　　[代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/xiaoweixueyuan/task8);<png>

　　*task9：《使用HTML/CSS实现一个复杂页面》。对效果图实现像素级还原，通过该任务综合练习html/css的相关知识，个人认为涉及到这样一些东西：
table的复杂样式设置；栅格化布局；多个ul嵌套的下拉菜单（需要考虑代码复用）；form的复杂样式设定；选择器的合理使用；类名的科学设定；代码的复用性。
下面几点是实际遇到的问题：*

1.使用tabel的时候，不管html结构中写没写tbody，浏览器都认为table与tr之间包裹了一层tbody，因此，使用选择器的时候，需要注意加入tbody；<br>
2.假设有祖父子三个层，分别命名为abc，如果b绝对定位并使用了top/right/bottom/left等属性定位，c也绝对定位并使用了top/right/bottom/left等属性定位。
此时如果b的box-sizing为border-box，则当使用伪类hover使其边框宽度发生了变化后，c在前后位置会相对b发生变化；当b的box-sizing为content-box，
则当使用伪类hover使其边框宽度发生了变化后，b在前后位置会相对a发生变化。

　　[demo](http://smallstarz.com/baidutask-2017/xiaoweixueyuan/task8/task_1_9_1.html);<br>
　　[代码](https://github.com/smallstar92/baidutask-2017/tree/gh-pages/xiaoweixueyuan/task9);<br>
　　效果图如下：

![](/img/baidutask2017/post-xiaowei-3.png)

　　task10是对flexbox进行学习，flexbox的原理较为系统，其各类属性的作用也比较复杂，在小结中不能详细叙述，需要做系统性总结。

