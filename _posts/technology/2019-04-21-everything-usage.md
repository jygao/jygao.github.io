---
layout: post
title: Everything使用技巧总结
date: 2019-04-21 10:39
author: jygao
comments: true
categories: [technology]
tags:
    - tools
---
<!-- wp:heading {"level":4} -->
<h4>Everything是什么？</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Everything是一个window文件搜索引擎，可以让你快速找到需要的文件，快速打开它或者拿到它的完整路径或者跳转到文件所在的目录。它小巧，干净，占用内存少，快速，实时更新，是提高效率的利器。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>背景</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>安装版本：V1.4.1.935 (x64)</li><li>官方帮助文档： <br><a href="http://www.voidtools.com/support/everything/">http://www.voidtools.com/support/everything/</a></li><li></li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4>搜索语法</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>操作符: <br>     space   与 (AND)<br>     |            或 (OR)<br>     !            非 (NOT)<br>     &lt; &gt;       分组<br>     " "         搜索引号内的词组.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>通配符: <br>     *           匹配 0 个或多个字符.<br>     ?           匹配 1 个字符.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>上述两类是最常用的是两种基本语法，除此之外，Everything还支持宏，修饰符，函数，详细语法介绍可以在Everything 菜单栏<strong>【帮助】-&gt;搜索语法 </strong>可以查到。熟悉并合理使用这些语法可以帮助我们快速找到我们需要的文件，看这些语法有点难理解，下面我以一些常用例子说明用法。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>例子1：如何查找出电脑上所有mp3格式周杰伦的歌曲？</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>周杰伦 *.mp3</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>解释：空格分隔表示“与"，*匹配任意字符。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe04d9edb.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>例子2：找出e盘中的，文件名是action20.swf到action29.swf的文件，还有action99.swf发给同事修改？</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>e: action2?.swf|action99.swf</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>解释：？表示匹配1个字符，“|”表示“或者”</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe0629e0b.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>例子3：找出电脑中文件大小超过1g的，2018年4月20至2019年4月20日创建的mp4文件？</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>size:&gt;1gb mp4 datecreated:20180420-20190420</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>解释：文件大小，用size:函数和datecreated:函数</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe04dbbed.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>筛选器</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> 筛选器允许你提前定制好筛选规则的，以后有需要可以快速重复调用的。可以在菜单栏【搜索】-&gt;管理筛选器 中新建或者删除筛选器。下面举个例子说明它的用法。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":6} -->
<h6> 经常需要在一个项目里面查找或者打开js或者json文件？可以考虑使用筛选器，操作如下：</h6>
<!-- /wp:heading -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe05e836f.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe04621c6.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe04432f9.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>正则表达式</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>正则表达式匹配提供了更强大的功能，值得说明的是，若是你勾选了菜单项<strong>【搜索】-&gt;使用正则表达式</strong> ，上述说的基本的搜索语法是不能用的了，跟正则表达式是互斥的。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>关于正则表达式的详细语法，在Everything 菜单项<strong>【帮助】-&gt;正则表达式语法&nbsp;</strong>可以找到它的用法。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe04a1253.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>关于更详细正则表达式说明，参考以下链接：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="http://tool.oschina.net/uploads/apidocs/jquery/regexp.html">http://tool.oschina.net/uploads/apidocs/jquery/regexp.html</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>若是你勾选了 菜单项<strong>【搜索】-&gt;使用正则表达式</strong> ，可以使用正则表达式的强大功能，下面继续以一样的例子说明。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>例子1 ：如何查找出电脑上所有mp3格式周杰伦的歌曲？ </h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>周杰伦.*.mp3</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>解释：<strong>.*</strong>表示匹配任何字符，中间没有空格哦</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfdff0b883.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4> 例子2：找出e盘中的，文件名是action20.swf到action29.swf的文件，还有action99.swf发给同事修改？ </h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>action99.swf|action2[0-9]{1}.swf</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>解释：<strong>|</strong>表示“或者”，[0-9]表示任意数字，{1}表示前面数字重复1次，即1位数字</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/04/21/5cbbfe062bdbf.jpg" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4> 例子3：找出电脑中文件大小超过1g的，2018年4月20至2019年4月20日创建的mp4文件？ </h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>正则表达式做不到这个筛选</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>技巧总结</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>熟悉Everything最基本的语法</li><li>根据需要合理使用筛选器</li><li>掌握使用正则表达式</li></ul>
<!-- /wp:list -->
