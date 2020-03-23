---
title: CSS去掉元素间隙
date: 2020-03-23 18:26:53
tags: [css]
categories: css
description: CSS去掉inline-block元素间隙的几种方法？
---
# CSS去掉inline-block元素间隙的几种方法？

间隙是怎么来的：间隙是由换行或者回车导致的；只要把标签写成一行或者标签没有空格，就不会出现间隙；
去除方法：

### 方法一：元素间的间隙出现的原因，是元素标签之间的空格，把空格去掉间隙就会消失。

```html
<div>
  <span>l111</span><span>l2222</span>
</div>
```

### 方法二：利用`HTML`注释标签

```html
<div class="demo">
    <span>l111</span><!-- 
    --><span>l222</span>
</div>
```

### 方法三： 取消标签闭合

```
<div class="demo">
    <span>saf
    <span>1321h
    <span>35435
    <span>32342
</div>
```

### 方法四： 在父容器上使用`font-size:0;`可以消除间隙

```
<div class="demo">
    <span>1111</span>
    <span>2222</span>
    <span>3333</span>
    <span>4444</span>
</div>
.demo {font-size: 0;}
```

