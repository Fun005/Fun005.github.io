---
title: CSS Modules
date: 2020-03-23 18:19:37
tags: [css]
categories: css
description: CSS Modules，顾名思义，就是CSS的模块化。我们都知道js有模块化的概念，那CSS模块化是什么意思呢？
---
## CSS Modules

### 1. 什么是CSS Modules

##### CSS模块化管理方案；它既不是官方标准，也不是浏览器的特性，而是在构建步骤（例如使用Webpack）中对CSS类名选择器限定作用域的一种方式（通过hash实现类似于命名空间的方法）。例如我们在buttons.js里引入buttons.css文件，并使用.btn的样式，在其他组件里是不会被.btn影响的，除非它也引入了buttons.css.

## 2. CSS Modules的好处

为什么我们需要CSS模块化。

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。现在我们说组件式开发，随着工程的页面数量和复杂度的提升，大家都遇到过样式冲突问题，一般我们会采用一下几种方法：

- class名写长点、写独特点，减少冲突可能性
- 添加父元素选择器，限制CSS选择范围
- 重新命名class，这样比较保险

上面这些办法应用后，带来更多的问题：

- 全局样式冲突、污染
- css选择器嵌套过深
- 代码冗余；

为了解决这些问题，我们由js模块化想到，能不能把CSS也进行模块化管理。CSS模块化的方案，主要有以下几种：

​	1.  **命名约定**。比如BEM（Block Element Modifier），使用BEM规范来命名CSS，组织HTML中选择器的结构，   利于CSS代码的维护，使得代码结构更清晰，但是BEM也有不足，就是写出来的类名太长，看起来一大堆的不够优雅；没有打通js和css之间的选择器与变量(和CSS Modules相比)；我在项目中写的样式有很多BEM的思想

 	2. **CSS  in JS**。不用CSS了，直接用JS写样式。代表是(styled-componentd)[https://github.com/styled-components/styled-components]。这种方式有很多好处，比如让CSS真正意义地写到JS里面、让标签更具有语意化、让样式也具备组件化思想等等，当然也有不够好的地方：样式代码大量重复、不能很好利用CSS预处理器以及，个人觉得CSS IN JS写起来很别扭，感觉很难受，我觉得css和js分离写起来才是最舒服的方式。
 	3. **用JS管理CSS**。利用JS编译CSS文件，让CSS也拥有模块化的便利，代表就是今天要说的CSS  Modules。CM和第二点不一样，还是两者分离的形式，并且CM只需修改构建代码和使用模块依赖引入className的方式即可，还支持less和sass的语法，使用CSS Modules可以让组件className控制权转交给JS，我们不会去关心命名冲突污染等问题，同时可以灵活控制生成的命名。

## 3. 通过creact-react-app演示

1. 基本使用：creact-react-app默认开启。（webpack.config.js）。**css文件要以component.module.css命名**，比如Button.module.css。

2. **类名的使用**：组件中的类名不会影响别的组件中的相同类名，比如在组件中（`Button`）可以使用像`.button`的名称来声明类名，而不必担心类名的冲突。渲染后的类名：Button_button__2LXQM，是一个随机hash值，用来确保具有相同名称的多个CSS Modules的唯一性。在Webpack的配置文件`webpack.config.dev.js`中，可以配置CSS Modules编译器如何重写类名，并且`hash`值是可选的。`styles`变量是一个JavaScript对象。另外styles下的类名必须上驼峰形式，比如className={styles.MyApp}，而className={styles.my-app}就会报错。有一点值得说的是，值得注意的是，如果你在CSS文件中编写了一些CSS，而在模板中没有使用它们（或错误的使用它们），Webpack将跳过生成输出任何未使用的CSS，从而能减少代码大小。

3. **多类名的使用**：通过模板字符串来写就行。也可以通过**组合特性compose**做到：  composes: button; CSS Modules最大的特色之一就是“**每个CSS代码都是应该用于特定的组件中，并且与项目的其他部分是隔离的**”。那现在有个问题令，假如我要在多个地方使用相同的样式，怎么办，要写多次吗，怎么抽离和利用公共样式。方案就是刚刚的composes。比如在`App.module.css`中要引用`Button.module.css`中的`.button`中的样式代码、或者在组件中引用公共代码。可以把各个组件共用的样式放置在一个独立的文件中，比如`common.css`。然后在需要的地方调用。这么来看，`composes`是非常强大的，可以让你跳出CSS Modules局部作用域名的限制，在组件中调用共用样式，也就是在局部作用域调用全局作用域样式。

4. **使用classnames**： 虽然可以通过composes达到类名组合的效果，但是我就想写className='primary white'，怎么办。也有办法，通过classnames 模块做到。npm install classnames *--save*。(classnames 官方文档)[https://github.com/JedWatson/classnames]。

5. ##### 组合选择器的使用

6. ##### 选择器的作用域： :local (局部作用域名，相当于组件中的本地选择器)、:global(全局作用域名)

   采用`:global`声明的全局选择器，和我们平时声明类名无差异，而且也不需要遵循CSS Modules的类名命名规则，并且在模板中调用的时候，也不需要使用`styles.xxx`这样的方式，可以直接在`className`中引用类名。比如示例中的`className="app-logo"`。
   还可以跨组件的使用，比如我们在`Button`组件的`Button.module.css`中使用`:global`声明了一个全局的`.button`样式

7. ##### 标签元素和属性选择器的使用:

   在CSS Modles中，类名是被限定在定义它的组件中使用（除非使用了`:global`标识的类），但对于HTML的标签元素和属性选择器是不受影响的。Button组件

8. **CSS Modules中使用`id`声明的样式不会被运用到元素上**.

## 4. 结语

CM还有很多功能，比如依赖管理、共享变量、代码压缩方面都有很好的表现，但是由于我也没有在具体项目中全面应用CM，所以大家有兴趣可以自己去研究。