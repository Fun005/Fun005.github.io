---
title: CSS实现毛玻璃效果
date: 2019-07-14 21:18:32
tags: [css]
categories: css
description: 通过css实现"毛玻璃"效果
---
在family一次版本迭代中，产品小哥给出了这样一个需求，更换头像背景的模糊效果，也就是所谓的毛玻璃效果。
{% asset_img Image.png avatar %}
以前没做过这种，怎么办？当然是搜索引擎，不懂就去搜。

发现有个filter属性，给它设置blur(x)即可。

主要思路是给相关元素设置伪元素，然后设置伪元素的背景+filter：blur(x)。

html如下：

    <div class="main">
    		<button type="">点我</button>
    		<div class="content">
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    			<p>用CSS制作毛玻璃效果（高斯模糊效果）</p>
    		</div>
    	</div>

CSS如下：

    <style>
    		.main {
    			position: relative;
    			background: hsla(0, 0%, 100%, .3);
    			overflow: hidden;
    			width: 800px;
    			height: 500px;
    		}
    		.main::before {
    			content: '';
    			position: absolute;
    			top: 0;
    			right: 0;
    			bottom: 0;
    			left: 0;
    			filter: blur(20px);
    			background: url('./mmexport1546413607115.jpg') no-repeat;
    			background-size: 100% 100%; 
    			margin: -30px;
    			z-index: -1;
    		}
    		p {
    			color: #fff;
    		}
    	</style>

效果如下：
{% asset_img 1563110911059.png avatar %}

但具体到需求，头像不是固定的图，没办法通过css设置伪元素背景（或者说我不知道怎么样动态给伪元素设置背景），这就头大了。无意间发现张鑫旭大神有文章讨论过这个，他文章里采用的方法是用两个img标签，然后设置模糊效果。原文链接：https://www.zhangxinxu.com/wordpress/2013/11/css-svg-image-blur/

这样也能做到同样的效果，这里就不给出代码了，文章里有。
